# ADR 0000: Standardised authentication and authorization

## Status

- [x] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context

It is expensive and painful to maintain authorization decisions that are hardcoded into a service. Authorization decisions should be
decoupled from authorization enforcement.

It is possible to provide authentication and authorization at a platform level, meaning that each service in the
mesh would get this provided for it by default with no work needed by developers.

The technologies that would enable this are [Istio](https://istio.io) and [Open Policy Agent](https://www.openpolicyagent.org/).
Istio provides a sidecar proxy which can be configured with an external authn/z provider, this external provider would be
Open Policy Agent which would be able to make decisions on the authentication and authorization status of a request.

Policies would then be written and deployed to describe how access is granted to each service. These policies are then
easy to iterate and change depending on product needs, without using developer resource.

### Architecture

```
     Incoming request

            +
            | 2.
            |                      Deployment
+-------------------------------------------+
|           |                               |
|           v                               |
|  +--------+--------+       +-----------+  |
|  |                 |  4.   |           |  |
|  |  Istio sidecar  +------>+  Service  |  |
|  |                 |       |           |  |
|  +--------+--------+       +-----------+  |
|           |                               |
|           | 3.                            |
|           v                               |
|       +---+---+                           |
|       |       |                           |
|       |  OPA  |                           |
|       |       |                           |
|       +---+---+                           |
|           |                               |
+-------------------------------------------+
            |
            | 1.
            v
    +-------+-------+
    |               |
    | Policy Server |
    |               |
    +---------------+
```

1. On deployment of a service to the cluster the Istio sidecar and OPA agent are automatically injected alongside the service. The OPA agent on startup will fetch the bundle of policies from the policy server, and refresh this bundle at a regular cadence.
2. A request is made to the Service containg a JWT token, this is proxied through the Istio sidecar.
3. The Istio sidecar makes an authn/z check to the OPA agent which will make the deicision as to allow the request based on the policies it has fetched from the policy server.
4. If the check is allowed the request continues through to the service itself, if it is denied the sidecar responds with an appropriate error code.

## Decision

- Move authentication and authorization decisions to OPA
- Create a policy server to maintain policies for the platform

## Consequences

- Learning Rego to write policies
- Adjusting existing services to use OPA
- Less developer time spent writing / adjusting authn/z
