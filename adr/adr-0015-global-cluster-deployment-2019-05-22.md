# ADR 0015: Deployment to Global GKE Clusters

## Status

- [x] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context

ADR-0007 describes how we are using a 'global cluster' model whereby all the
kubernetes based applications run on two or more clusters.

There will be circumstances when the clusters require maintenance. In these
circumstances the developers need to know how and when to redeploy to the
clusters, and what support they should expect to receive from the SRE team to
help coordinate their efforts.

This ADR assumes the availability of a load-balancing solution that can
handle routing of requests to clusters based on health-checks.

## Decision

### Normal Operation

SRE will provision at least two clusters. The clusters will be maintained to
the same versions of platform, and as far as the developers are concerned are
identical resources.

Developers will use GitLab CI to deploy their applications to the clusters.
There is no requirement to inform SRE of an application deployment. Developers
will ensure that the correct deployments are made to the available clusters.

SRE will provide Global Cluster Utilities to help developers write pipelines
that can deploy to the currently active clusters. The developers should use
these utilities unless there is good reason not to.

The deployment pipelines must be cluster agnostic - no cluster specific
information will be presented to the deployments, only some abstract
identifiers. The applications should not depend on which cluster(s) they are
being deployed to or running on.

### Cluster Events

SRE maintain a list of the currently active clusters (called the Global Cluster
List File), which is used by the Global Cluster Utilities.

SRE will inform developmemt teams when an update to this list has occurred;
usually in response to a cluster event. The only requirement on development
teams is for them to trigger a re-deployment process. If the pipelines are
using the Global Cluster Utilities then the deployments to the currently active
clusters will 'just work' (tm).

Behind the scenes, SRE will also be manipulating the load-balancing set up
necessary to direct the traffic to the clusters.

## Consequences

- developers have the responsibility for deployment to clusters
- SRE has the responsibility for notifications about cluster events
- SRE provides tooling to help deployments to the clusters and help manage
  failovers automatically.
- SRE has the responsibility to ensure that global clusters are completely
  interchangeable resources with no intrinsic dependencies for the developers.

## Implementation Details

This may not be the best place to document but it may be useful.

When a cluster is taken down (or goes offline) SRE will evaluate the situation,
but load-balancing should already be directing traffic to the remaining
cluster(s).

If the cluster can be restored to service then it will be. Traffic will start
to flow back to the cluster if everything restarts OK.

However if SRE determine a more serious incident response is required then:

- the cluster will be deleted from GCP
- SRE *may* update the Global Cluster List at this point
- the cluster will be rebuilt by IaC
- SRE will update the Global Cluster List

### Global Cluster List

This list contains the details of the currently provisioned global clusters.

Developers are notified any time that the Global Cluster List is changed.

The list is a simple text file stored in GCS. The format is ```rank clusterID
projectID region```.  `rank` is an incrementing number. Any new or rebuilt
cluster gets assigned the next 'rank' and added to the end of the list.

The rank is passed from the Global Cluster Utilities to the deployment. Its
purpose is to provide an abstract unique identifier for the cluster at
deployment time without revealing the actual cluster ID. This is to avoid the
temptation of building deployment scripts that rely on cluster IDs.

One place rank is useful in a deployment is the Cluster Rank Service (see
below).

Example list file:
```
1 us-c1-gke sixty-empire us-central1
2 us-e1-gke sixty-empire us-east1
```

Example events:

(developers are notified after each list update)

us-c1-gke goes down. SRE update the list to remove it. It's the weekend so they
decided to wait until Monday for a rebuild:

```
2 us-e1-gke sixty-empire us-east1
```

SRE rebuild us-c1-gke and then add it back to the end of the list:

```
2 us-e1-gke sixty-empire us-east1
3 us-c1-gke sixty-empire us-central1
```

SRE win the lottery and give us a new cluster:

```
2 us-e1-gke sixty-empire us-east1
3 us-c1-gke sixty-empire us-central1
4 us-w1-gke sixty-empire us-west1
```

us-c1-gke has another outage cycle, but it's dealt with quickly.

```
2 us-e1-gke sixty-empire us-east1
4 us-w1-gke sixty-empire us-west1
5 us-c1-gke sixty-empire us-central1
```

### Primary Cluster / Single Location Applications

The primary cluster is the first in the list. In the case where an application
should only ever run on one cluster, it will need to choose the primary
cluster. This scenario is appropriate for systems which can handle an outage
measured in minutes.

SRE provides a cluster-rank-service that can be used to control these kinds of
applications. The CRS will respond with an HTTP code 200 if the cluster is the
primary cluster. Once it is the primary cluster it will remain that way until
it is deleted. i.e. there is no flip-flop back to the previous. The next
cluster in the rank will take over once the cluster list is updated to remove
the primary.

A simple shell script wrapper waits for the CRS to report that the cluster
is primary:

e.g.
```
#!/bin/sh
while ! curl --silent --fail http://rank.service.default.cluster;do
    sleep 60
done
# start application...
```

