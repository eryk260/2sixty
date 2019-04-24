<!-- File format adr/adr-0000-project-keyword-YYYY-MM-DD.md -->

# ADR 0013: Github Admin Rights

## Status

- [x] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context

Before the [IaC framework](adr-0009-iac-framework-2019-01-30.md) and the Github module in it, it was necessary for Architects and managers to have (GitHub) Organisation wide administrator access. Since, then the SRE team provided a controlled and audited way for the Architects to create/manage repositories, teams and individuals within the organisation.

## Decision

The decision was made on the Arch + Infra chapter meeting on (17.04.2019.), that the SREs will revoke individual org-wide administrator access from everyone and provide a way via the slack bot interface to the Github IaC module for the Archs + SREs to elevate their privileges if necessary.

There was also a consensus regarding providing administrator access to repositories, because the Architects/Team leads need to manage their repository settings and because the Github IaC module is not providing those options, it was agreed, those permissions won't be changed.

## Consequences

No one will be able to bypass the IaC framework to create repositories, invite individuals to the organisation or manage teams.
Because of this there won't be need from the SREs to retrofit repositories, which were created manually

