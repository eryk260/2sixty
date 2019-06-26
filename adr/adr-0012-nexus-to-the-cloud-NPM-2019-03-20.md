<!-- File format adr/adr-0000-project-keyword-YYYY-MM-DD.md -->

# ADR 0012: Nexus as our chosen NPM registry 

## Status

- [x] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context

The left-pad issue a few years ago, where a developer unpublished his node package modules (NPM) from the NPM registry following a dispute with KIK... A lot of modules in the Node.js ecosystem depended on left-pad. After removing left-pad from NPM, thousands of app deployments across the world were broken because the apps could not be built due to this missing dependency. 2Sixty needs a mechanism to protect itself against any app dependency failures in the Node.js ecosystem which will prevent the building and deployment of Frontend apps. This can be achieved by having a private NPM registry using something like Artifactory, Nexus or and NPM Enterprise

More about the [left-pad issue](https://www.businessinsider.com/npm-left-pad-controversy-explained-2016-3?r=US&IR=T).

## Decision

The decision is to use Nexus for managing our NPM

Main reasons for choosing Nexus:

- we already have Nexus
- easy to setup to tackle this issue
- trivial config update for Devs to point to Nexus for NPM

## Consequences

- Nexus needs to be moved to the Cloud and this has associated costs:
- GCP FileStore (NFS) - Standard 1TB Storage w/ 120Mb Read 1 instance
- 2 Instances for Nexus. 2X8GB (1 Failover)
- 1 X Loadbalancer

Approx Cost $450 PCM

