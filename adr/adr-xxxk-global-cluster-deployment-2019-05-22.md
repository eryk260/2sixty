# ADR XXXX: Deployment to Global GKE Clusters

## Status

- [ ] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context

ADR-0007 describes how we are using a 'global cluster' model whereby all the
kubernetes based applications run on a pair of clusters.

There will be circumstances when the global clusters require maintenance, or in
the event of a major outage, a complete rebuild. In these circumstances the
developers need to know how and when to deploy to these clusters, and what
support they should expect to receive from the SRE team to help coordinate
their efforts.

Route53 is mentioned within this ADR as it assumes this is how load
balancing/DNS will be managed.

## Decision

### Normal Operation

SRE will run two clusters during normal operations. Both clusters will be
maintained to the same versions of platform, and as far as the developers are
concerned should be identical resources.

Developers will use GitLab CI to deploy their applications to the clusters.
There is no requirement to inform SRE of an application deployment.  Developers
will ensure that the correct deployments are made to both clusters.

### Cluster Maintenance

SRE will inform developmemt teams when a cluster is going to be put into
maintenance. SRE will do this in two ways:

* manual communication (slack, email etc tbd) AND;
* cluster status service (automated, see below)

The development teams should ensure that their deployments on the other cluster
are up to date in advance of the maintenance.

Effort will be made to perform the maintenance with minimal disruption, but
this is not guaranteed. The developers have the option to scale-down their
deployments on the affected cluster if they choose to.

### Cluster Outage

In a sudden outage event the applications on the remaining cluster should take
over automatically. Health checks from R53 should detect the outage and route
traffic accordingly. The cluster status service (see below) also indicates the
status of the cluster will be the active one.

### Cluster Rebuilds

There are few circumstances requiring rebuilds:

* New features that can only be enabled at build-time
* Upgrade/maintenance went wrong
* Human error
* Large-scale outage within Google

In the event SRE can rebuild the cluster in the same region then it will be
re-created with the same cluster name. Following the rebuild SRE will notify
the developers that the cluster needs to be deployed to.

An alternative, more automated approach, involves GitLab triggers. These allow
simple HTTP requests to trigger pipelines in GitLab CI. The triggers even
support the use of variables passed in the request. Therefore, each development
team could simply provide SRE with the correct 'curl' command to trigger a
deployment, and the SRE team need only run the whole set of commands to deploy
everything to the new cluster.

### Cluster Status Service

While both clusters are normally running it may be useful to have a globally
consistent view of which cluster is considered active vs stand-by.

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


* Possible Leader Algorithm: *

each node:
- every 10s
- write nodeid (clusterId, or configMap entry?) and timestamp to fs

when queried from http client:
- read from FS, sort lowest id with valid timestamp
- report based on winning id compared to self.id etc.

## Consequences

- the responsibility for deployment to clusters remains with the developers
- 'you built it; you run it' applies
- SRE provides tooling to help deployments to the clusters and manage failovers
  automatically.
- software needs to be developed with the above environment in mind



