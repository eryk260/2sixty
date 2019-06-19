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

Details moved to GitLab, see the [empire-infra](https://gitlab.com/2sixty/sre/empire-infra) project for more information.
