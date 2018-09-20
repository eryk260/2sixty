# ADR 0004: 2 Sixty file naming convention 

## Status

- [x] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context

The filenames under this repo for decision records (adr, pdr and ddr) are prefixed with a 4 digit sequential number. 

Date of the decision is an important part of the decision and needs to be part of the decision as an immutable piece of data.

## Decision

To use a date (`YYYY-MM-DD`) in the naming convention. This is even more important given that it is good practice to date decision records. The proposed file naming convention will be:

- `adr/adr-9999-project-keyword-YYYY-MM-DD.md`
- `pdr/pdr-9999-project-keyword-YYYY-MM-DD.md`
- `ddr/ddr-9999-project-keyword-YYYY-MM-DD.md`

The `9999` part as well as the `YYYY-MM-DD` are also part of the ADR whist in review. Once the ADR is accepted/rejected, the person approving and merging the decision record will need to number and date it.

## Consequences

- We need to rename the template files as above
- We need to retrospectively rename the existing adr, pdr and ddr files in the master branch (as of this ADR that is only 2 files including this file)

