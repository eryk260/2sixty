# ADR 0000: Istio

## Status

- [x] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context

As we build out our platform we will need to be able to monitor our services effectively with as low effort as possible.

One of the primary needs for the platform will be **observability**. Observability will allow us to monitor our services
for their health and performance. Metrics gathered from each of our services will allow us to apply Service Level Objectives
(SLOs) to expose what is really happening in our platform. With SLOs we can then make informed decisions as to whether
focus is needed on technical debt, or we have some leeway in our "Error Budget" to introduce new features, with the
potential instability that comes with them.

In the future the platform may require advanced deployment strategies and advanced networking capabilities. All of the
aforementioned can be implemented using Istio. The sidecar containers that Istio injects will give us a great baseline
of observability for a very small amount of developer work.

## Decision

Istio is deployed as part of our Kubernetes clusters.

## Consequences

- Slightly more load upon the clusters as new containers are introduced
- Monitoring for all enabled Services
- Leads to standardised authentication and authorization along with OPA
