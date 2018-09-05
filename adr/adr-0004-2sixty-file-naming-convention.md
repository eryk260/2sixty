# ADR 0004: 2 Sixty file naming convention 

## Status

- [x] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context

The filenames under this repo for decision records (adr, pdr and ddr) are prefixed with a 4 digit sequential number. 

With the increase in people likely to make contributions to this, it becomes tedious to manually maintain a running sequence for files in the following folders: `adr`, `pdr`, `ddr`. This problem is exasperated when the filecounts increase and we have multiple PRs potentially proposing to use the same sequence number.

At the time of writing, this file and its PR were reluctantly sequenced with 0004 although there are no 0002 or 0003 files (they only exist as PRs). The problem grows when you need awareness of all other active PRs and their sequence. Not to mention that some decision records will likely need to be renamed depending on the order the PRs are merged. 

## Decision

To use a date (`YYYY-MM-DD`) instead of a 4 digit sequence. This is even more important given that it is good practice to date decision records. The proposed file naming convention will be:

- `adr/adr-YYYY-MM-DD-project-keyword.md`
- `pdr/pdr-YYYY-MM-DD-project-keyword.md`
- `ddr/ddr-YYYY-MM-DD-project-keyword.md`

## Consequences

- We need to rename the template files as above
- We need to retrospectively rename the existing adr, pdr and ddr files in the master branch (as of this ADR that is only 2 files including this file)

