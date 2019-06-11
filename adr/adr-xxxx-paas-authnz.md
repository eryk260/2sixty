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

## Decision

The technologies that would enable this are [Istio](https://istio.io) and [Open Policy Agent](https://www.openpolicyagent.org/).
Istio provides a sidecar proxy which can be configured with an external authn/z provider, this external provider would be
Open Policy Agent which would be able to make decisions on the authentication and authorization status of a request.

Policies would then be written and deployed to describe how access is granted to each service. These policies are then
easy to iterate and change depending on product needs, without using developer resource.

### Architecture

#### Permissions and Roles

We will use Keycloak as our identity provider, and rely upon the JWT payloads that it provides. Each service within the mesh will
have a "Client" created for it in the 2sixty Keycloak realm. Within this client entity we will define the "Roles" that each service
provides, these could be created via an API by something like the Policy Server. The "Roles" at the client level should map fairly
closely to what are called permissions in google cloud, for example:

- cloudsql.instances.connect
- cloudsql.instances.get

Once these have been defined we can then create realm level "Composite Roles", for example taking the two permissions above, we would create
the _roles/cloudsql.client_ role. CloudSQL being our example service here.

This sort of setup will allow a high level of composability and granular control over who and what can access our services.

##### Nomenclature

From this point on we will refer to Keycloak "Roles" as Permissions, and Keycloak "Composite Roles" as Roles.

Permissions should be named in the following way:

> service.resource.action

Roles should be names in the following way:

> roles/service.role

The roles prefix allows us to identify the difference between permissions and roles in Keycloak.

#### Authorization enforcement

Given the above has been setup, we can enforce different policies at the request entry to the service. With OPA this is written using a
language called rego. As a simple example, we have a Hello World service running, we wish to restrict access to only a few users. First we
create a permission, lets call it _helloworld.greeter.get. Second we create a role \_roles/helloworld.user_ and assign the permission to the
role. Now we assign the users who need access to that role in Keycloak. Finally we write some policy to enforce this,
without having to make any code changes to the actual Hello World service.

```
package service.adwords

# This is the input that we are receiving
import input

# Parse the JWT token in the input
token = {"payload": payload} {
    io.jwt.decode(input.token, [_, payload, _])
}

# By default we are not going to allow requests to this service
default allow = false

# Within this block we AND the conditions to get the result
allow {
    input.method = "GET"
    input.path == "/"
    # Roles here are our Permissions
    token.payload.resource_access["hello-world"].roles[_] == "helloworld.greeter.get"
}
```

Now users who have the _helloworld.greeter.get_ permission can access the service, and users without it will be denied.

#### Example request flow

Assuming an JWT token has already been acquired:

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

#### Service to service communication

Each service with its corresponding "Client" has a service account attached to it. These can be assigned roles or permissions, and
will allow us to control which services access other services if needed. Access tokens outside of the context of a user can be acquired
using the Client Credentials grant type.

### Decisions

- Move authentication and authorization decisions to OPA
- Create a policy server to maintain policies for the platform

## Consequences

- Learning Rego to write policies
- Adjusting existing services to use OPA
- Less developer time spent writing / adjusting authn/z
