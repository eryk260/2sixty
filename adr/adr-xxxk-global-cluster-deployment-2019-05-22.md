# ADR XXXX: Deployment to Global GKE Clusters

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

Route53 is mentioned within this ADR as it assumes this is how load
balancing/DNS will be managed.

## Decision

### Normal Operation

SRE will provision at least two clusters. The clusters will be maintained to
the same versions of platform, and as far as the developers are concerned
should be identical resources.

Developers will use GitLab CI to deploy their applications to the clusters.
There is no requirement to inform SRE of an application deployment. Developers
will ensure that the correct deployments are made to the available clusters.

SRE will provide scripts and utilities to help developers write pipelines that
can deploy to the currently active clusters.

The deployment pipelines need to be cluster agnostic - no cluster-specific
information will be presented to the deployments. The applications should not
depend on which cluster(s) they are being deployed to or running on.

### Cluster Maintenance

SRE will inform developmemt teams when a cluster is going to be put into
maintenance. SRE will do this in two ways:

* manual communication (slack, email etc tbd) AND;
* cluster status service (automated, see below)

The development teams should ensure that their deployments on the other cluster
are up to date in advance of the maintenance.

SRE will ensure traffic is not routed to the affected cluster(s) before
maintenance starts. The cluster will then be taken offline.

Following a maintenance or outage event, the SRE team will notify the
development teams that one or more clusters needs to be re-deployed to.

### Cluster Outage

In a sudden outage event the applications on the remaining cluster should take
over automatically. Health checks from R53 should detect the outage and route
traffic accordingly within 15s.

SRE will provision a new cluster to replace the affected cluster, and notify
the developers that deployment is needed.

### Cluster Status Service

While the clusters are running it may be useful to have a globally consistent
view of which cluster is considered active vs stand-by; this can be used by
services that need to run only in a single location, but have the resilience
afforded by a fail-over to another cluster.

The proposal for this service is as follows.

SRE will provide a status service within the global clusters. This is an HTTP
endpoint (private within each cluster) that returns cluster status data when
queried:

    - name of the cluster
    - active/stand-by status flag
    - maintenance flag
    - next scheduled maintenance

The flags combine to the following meanings:

|Active | Maintenance |Comment          |
|-------|-------------|-----------------|
|  Y    |     N       |Active cluster   |
|  N    |     N       |Standby cluster  |
|  N    |     Y       |Under maintenance|
|  Y    |     Y       |We're in trouble |


#### Possible Leader Algorithm:

    each node:
    - every 10s
    - write nodeid (clusterId, or configMap entry?) and timestamp to fs

    when queried from http client:
    - read from FS, sort lowest id with valid timestamp
    - report based on winning id compared to self.id etc.

We have also discussed the possibility of an automated notification service
whereby pipelines can be triggered automatically in the event that a new
cluster has just been created.

## Consequences

- deveopers have the responsibility for deployment to clusters; 'you built it;
  you run it' applies
- software needs to be developed with the above environment in mind
- SRE has the responsibility for notifications about cluster events
- SRE provides tooling to help deployments to the clusters and manage failovers
  automatically.
