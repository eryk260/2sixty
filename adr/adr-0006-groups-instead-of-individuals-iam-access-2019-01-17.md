<!-- File format adr/adr-0000-project-keyword-YYYY-MM-DD.md -->

# ADR 0006: GSuite Groups for managing permissions in GCP

## Status

- [x] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context

The way currently we are working is to give individuals the necessary access in GCP, so are able to do their work. This sometimes a cumbersome procedure, because the work for those individuals changes, which causes ad-hoc changes to GCP project's iam policy bindings.<br>
Other problem, that even if there are no changes regarding the required permissions, if a new person, who does the same job starts, sometimes IT or SREs receive a vague request, that the new starter requires the same permissions as "John". This causes extra burden on the people who try to complete the ticket, because they also need to first investigate what "John" has and then assign similar permissions to the new starter.<br>
Another problem, when "John" moves on and get a different position in the company. Then IT or SREs receive a ticket (IF they receive the ticket), that John moved from position A to position B, and now he needs the following permissions...

This ADR is meant to solve the mentioned issues.

## Decision

We will use GSuite groups to group and organise people, who share the same responsibilities.

1) identify Essence and 2Sixty products: [2Sixty Service/Product Catalogue](https://docs.google.com/spreadsheets/d/13dRolht7ZV7CV8gD1zPZpf8KmfSygFrhmPexurE0nhE/edit#gid=0)
2) define the groups and its members: [GSuite Groups for GCP IAM](https://docs.google.com/spreadsheets/d/14DP7YvBhoZFu-hdbYppEOwHPN_XGCKxwBMbp86BfosA/edit#gid=492359136)
3) define roles for responsibilities: TBD
4) assign groups with roles to GCP folders/projects: [Folder structure](https://docs.google.com/spreadsheets/d/14DP7YvBhoZFu-hdbYppEOwHPN_XGCKxwBMbp86BfosA/edit#gid=0)

## Consequences

With this system, we eliminate the need of on-demand, ad-hoc permission elevations. When we have new starters, we can instantly provide them the necessary permissions based on their job title. If somebody changes their role, all we (IT or SREs) have to do is to move that person from one group to another (or from/to multiple groups).
