<!-- File format adr/adr-0000-project-keyword-YYYY-MM-DD.md -->

# ADR 9999: GCP Networking - Hyperspace

## Status

- [x] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context

- the company is growing fast, which means, that we are creating more and more gcp projects for the new products
- the new projects require new VPC networks, because the default VPC created at project creation time is not satisfying the security (too open) and subnet criteria
- to allow VPC network peering, we need to make sure, that there are no overlapping subnets within each VPC
- this causes, that we need to track in a document, which subnets were created
- it also means, that if the product requires VPN connection back to the DC, we need to configure VPN multiple times (as many times as many times we create a new project for a new product, which wants to reach DC)

## Decision

GCP supports a technology called [Shared VPC](https://cloud.google.com/vpc/docs/shared-vpc). This allows us to create a single project with a single VPC and all the subnets and connect all of the product projects to this project, so they can see and use this one shared VPC.

For that, we created the following architecture:
```
                                                 +--------------------+
                                                 |Project A           |
                                                 |                    |
                                           +-----+  Hosts product A   |
                                           |     |                    |
              +-------------------------+  |     +--------------------+
              |Empire project           |  |
              |                         |  |     +--------------------+
              |    hosts all shared     |  |     |Project N           |
              |    compute resources    |  |     |                    |
              |                         |  |     |  Hosts product N   |
              |                         |  |     |                    |
              +-----------+-------------+  |     ++-------------------+
                          |                |      |
                          |Uses        Uses|      |Uses
                          |                |      |
+-------------------------v----------------v------v-------+
|Hyperspace project                                       |
|                                                         |
|     Hosts VPC networks with Subnets, Firewall rules,    |
|     route rules and VPN connections (all shared)        |
|                                                         |
+---------------------------------------------------------+
```

The Galactic Hyperspace project (sixty-hyperspace) project contains initially a single VPC network called Quadrant-A. That network will host all of the subnets, firewall rules (potentially route rules) and VPN connection (singular - name: Wormhole) for every product/project, which needs network stack.
 
Example:

- Empire project runs our shared GKE multitenant clusters, which are connected to each other via different subnets found in Quadrant-A (VPC hosted in sity-hyperspace project)
- If product A runs Compute VMs or CloudSQL instances, they are using their subnets from Quadrant-A (VPC hosted in sixty-hyperspace project)
- Same for product B and product N.


## Consequences

- Some services cannot use Shared VPC (yet), some examples: GCP composer environments, App Engine flex instances, GCP Filestore. For those, we need to maintain a VPC in the product's project.
- Easier management of VPC network(s), subnets and firewall rules.
- Easier management of VPN connection(s) back to DC or to other places.
- Single-pane of Glass for DevOps/SRE/IT, so they can see all firewall rules (most beneficial).

