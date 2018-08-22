<!-- File format adr/adr-0000-project-keyword.md -->

# ADR 0003: Multiple client environments

## Status

- [x] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context

As we move towards providing our products to other agencies, and deploying separate environments per client, we need to
think carefully about how to achieve our goals. We must ask ourselves questions about our current practices and how
sustainable they are:

- How will we release new features?
- How do we share the burden of development across teams with similar needs?
- How do we meet the needs of our clients without increasing complexity?
- Can we simplify our infrastructure?
- Can we scale with our current practices?

A lot of these can be answered by talking about how we organise our code in terms of repositories, projects and products,
and how we can use this to enable sharing code and ideas between projects. Leveraging composibility of micro services
and making a service mesh.

### Services and a mesh

General concepts that we all know about micro-services

- Small, independent
- Automated deployment via CI/CD
- Communication through service discovery
- A contract in terms of input and output

But how can we define the services we create in terms of the business? What are the boundaries?

- As small as possible while providing a meaningful business function
- Must provide and encapsulate all of the data and business function it is responsible for
- Defining those boundaries we can use technologies to help us
  - OpenAPI
    - [+] Well supported lot of tooling
    - [-] Message contract not strict
    - [-] Verbose
    - [-] Request response only
  - GRPC
    - [+] Message contract is core
    - [+] Good tooling
    - [+] Backwards compatibility built in
    - [-] Support lower

### Code, Products, Projects, Development

The current status quo sees us creating silos per product, both in terms of code and infrastructure. We have many GCP
projects with many VMs and resources within each.

#### How do we increase observability of what code we are writing and what we can share between teams?

First we should move all of our code into one repository with each service sitting in a folder at the root of
project. This monorepo will be responsible for the testing and building of our code up to generating containers. However
deployment will be kept separate. By organising ourselves in this manner, every team will be able to observe what business
problems have already been solved by other teams. We will be able to collaborate between teams on a shared business
problem.

#### How do we reduce the maintenance burden for DevOps?

Secondly we should create repositories per 2sixty "client", the first being "Essence" which essentially contains the
deployment configuration for all current Essence services. These client repositories would be responsible for deploying
into a GKE cluster which would contain all the current products and services together.

#### How would the development flow work?

A release from the main code repo would trigger deployments from each client repo correctly configured for each client.

1. [monorepo] Feature branch created, development done.
2. [monorepo] Merge request created, tested and built.
3. [monorepo] Merge request reviewed and approved.
4. [monorepo] Merged into master, tested, built and artifacts pushed to gcr repo.
5. [monorepo] Success event sent either via webhook or pubsub.
6. [each clientrepo] Release event received.
7. [each clientrepo] Deployment started with new relase version (configuration is per repo).

### Wrap up

The above would allow for us to scale how we deliver separate customer configurations exactly to their specifications.
It will be a significant change in how as a business we go about delivering products and services. The benefits appear
to be significant, but the change is daunting.

## Decision

- Create standard guidelines for scoping and building micro services
- Stop thinking in narrow product terms, and more as generalised business problems / needs
- Move all development into a single artifact build and test repository.
- Create deployment repositories for each separate "client" environment. e.g. The first being the Essence environment

## Consequences

- Client specific configurations and service deployments
- Significant development process change
- An impact on DevOps procedures
- Billing...
