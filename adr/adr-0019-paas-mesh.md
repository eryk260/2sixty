# ADR 0019: A Service Mesh

## Status

- [ ] active
- [x] rejected
- [ ] deprecated
- [ ] superseded

## Context

As we build out our platform we will need to be able to monitor our services effectively with as low effort as possible.

One of the primary needs for the platform will be **observability**. Observability will allow us to monitor our services
for their health and performance. Metrics gathered from each of our services will allow us to compare
[Service Level Objectives](https://landing.google.com/sre/sre-book/chapters/service-level-objectives/) (SLOs) to expose
what is really happening in our platform (Indicators). With SLOs we can then make informed decisions as to whether
focus is needed on technical debt, or we have some leeway in our "Error Budget" to introduce new features, with the
potential instability that comes with them.

In the future the platform may require advanced deployment strategies and advanced networking capabilities. All of the
aforementioned can be implemented using Istio. The sidecar containers that Istio injects will give us a great baseline
of observability for a very small amount of developer work.

However:
* Service mesh technology is still imature.
* Istio has caused problems when added to the platform.
* Istio hass a lot of bells and whistles we don't need yet. 
* Threre are other services mesh technologies to consider: https://landscape.cncf.io/category=service-mesh&format=card-mode&grouping=category
* We have more pressing problems than observability, adding complexity to the system will hinder addressing them.

## Decision

Hold off adding Istio in the mix until there is an important problem we are confident it is the best solution for.

## Consequences

Simplefied deployment and debugging.
