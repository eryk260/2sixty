# ADR 0007: Global GKE Clusters

## Status

- [x] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context

- As our company name says, we are a `global` company. To be really global, we need to host our applications around the world (or at least provide some sort of CDN).
- Beside this, if we give every team a GKE cluster for their own, we are not managing the resources in an optimal way
- It also causes the SRE/Devops team to maintain as many clusters as many team is using Kubernetes (including: ingress controllers, patching, upgrading, securing etc..)

## Decision

- To decrease the overhead of the GKE administration and to be really Global, the decision is to create 2 (at start) GKE clusters.
- One in US Central1 region and one in US East1. Beside that, we can enable GCP's CDN to cache static content.

```
                   +--------+
                   |  GLB   |
                   +--+--+--+
                      |  |
+--------------+      |  |       +-------------+
|              |      |  |       |             |
| US Central1  | <----+  +-----> |  US East1   |
|              |                 |             |
+--------------+                 +-------------+
```

- The stateless components of each application will be deployed to both GKE clusters (US central1 and US east1). For global load balancing we'll use Cloudflare's Load Balancing and Intelligent Failover ([tutorial](https://support.cloudflare.com/hc/en-us/articles/115000081911-Tutorial-How-to-Set-Up-Load-Balancing-Intelligent-Failover-on-Cloudflare)) or Akamai Global Traffic Management.
- Based on http://gcp.latenci.es, the best region to store the stateful components of an application is us-central1. Meaning, all of the database backends (CloudSQL, etc...) will be hosted in that region.
- Later, if a customer requires to host their application/data in other regions, we can create 2/4 extra clusters in EMEA and/or in APAC and deploy the developped application to two regions within a continent.

The result will be:

Option 1) An application will be hosted eventually in 3 continents and 6 regions, which will decrease the HTTP requests round-trip time.

Option 2) An application will be hosted in the US continent and with CDN enabled for caching, the HTTP requests round-trip time will be decreased no matter where the client is connecting to the app.

### GCP specification

The above mentioned compute layer will be hosted in the `sixty-empire` project on the US (and later potentially on EMEA and APAC) GKE clusters.

To accommodate any application specific GCP resource, when the application development starts, 2 new projects will be created for the newly developped app. One for the `non-production`, one for the `production` GCP resources.

The GCP resources will be created with an IaC (infrastructure-as-code) solution provided by the devops engineers.

Reaching those resources will be with service accounts created in the project.

### GKE specifications

The US clusters (central1 and east1) will host also our development environments. Meaning, anybody who wishes to reach production with their application, we'll need to deploy to both development clusters first, and make sure their application works "continentally".

This means, that **no stateful component** can be hosted on any of the GKE clusters.

#### Node pools in GKE clusters

The US clusters will have 1-1 `non-production` node pool, which will be shared among all development teams. We (devops) will provide a dashboard, where the developers and POs can monitor, how much resource they are using and how much it costs the company.

Within the GKE clusters, there will be dedicated namespaces with resource quotas for each application. When an application development is started, the PO or the lead developer of the application will need to provide the resource requirements of the application (cpu, memory and disk) to the devops engineers, so they can create the namespace. Once the namespace is created, any pod started in that namespace will be scheduled on the `non-production` node pool (enforced by a `Mutating Admission Controller Webhook`).

Beside the `non-production` node pool, there will be a `production` node pool attached to each cluster. Once the application is ready for production, a new `prod-appname` namespace will be created, where all the pods will be scheduled on the `production` node pool.

This provides separation from `production` and `non-production`.

Running `privileged` containers on any of the GKE clusters will be forbidden. This provides further separation between applications running on the same node pool.

## Consequences

- We will pay double of the resources, that we actually need with High-Availability.
- We will also receive continental (at the beginning US) High-Availability.
- Because there won't be dedicated node pools / production application, the developers will need to inject service account keys in their application to reach the resources in their projects (or use Vault).
- No **stateful components** can be run on the GKE clusters
- No `privileged` containers can be run on the GKE clusters
