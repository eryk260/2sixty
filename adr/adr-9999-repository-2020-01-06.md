<!-- File format adr/adr-0000-project-keyword-YYYY-MM-DD.md -->

# ADR 9999: Picking repository provider for technology department

## Status

- [x] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context

Currently the business uses all three major providers (Gitlab, Github and Bitbucket). Going forward the business needs to select only one provider to have all code repositories situated within. For all new projects the provider of choice has been Gitlab as the SRE team has built its IaC system using Gitlab CI as well as the overall fuctionality compared to other major providers. Currently the historical TEL systems have been using Bitbucket and the historical Essence based projects have been using GitHub. 

## Decision

The decisions going forward will be to have all code stored using Gitlab. This will consolidate management of user access, reduce cost to running three different systems as well as allow the continued building of IaC on Gitlab CI. As well, other 2Sixty teams have invested significant time into created Gitlab CI pipelines and the cost of re-engineering these for a different provider would be high.

## Consequences

- Teams currently using GitHub will need to migrate their project source code to Gitlab
- Teams currently using Bitbucket will need to migrate their project source code to Gitlab
- Currently Github is managed using IaC and slack by support, the SRE team will need to develop a new system for managing Gitlab that Support will be able to use.
- There will be time required from the SRE and Support teams to assist in and access management and user creation
- Number of licenses needed for Gitlab will increase 
- There may be historical tie-ins to other providers that we are not aware and will need to be alerted by the development teams