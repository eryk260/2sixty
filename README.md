# 2Sixty Documentation

## How to use this repo

We are using a concept called an Architecture Decision Records (ADR) to document decisions made that affect projects. This
will allow us to see why decisions have been made in the past and have discussions around decisions that need to be made.

Read more about this concept here [ADRs](http://thinkrelevance.com/blog/2011/11/15/documenting-architecture-decisions), the whole
article is in fact in the format of an ADR.

A lot more detail is available [here](https://github.com/joelparkerhenderson/architecture_decision_record).

Provided in the [adrs](./adr) and [pdrs](./pdr) folders are templates to create a new one including how to name your records.

## How create a new ADR
1. Create a new branch from master
2. clone the `adr-0000-project-keyword-YYYY-MM-DD.md`, changing the record number to a `9999` placeholder and the project & keyword to better reflect the decision being made. 
3. Submit a pull request of the new branch with the new decision record, where it will be open for discussion.
4. Depending on the outcome (approved, rejected etc), another commit is made 
  - updating the status
  - updating the file name record number to the latest on master +1
  - updating the filename date to reflect the date the decision was made
5. The Pull request is merged into Master.


**IMPORTANT**: Any decision records in the master branch are considered Final and immutable. Any changes to the file should only be minor fixes (i.e. typos, format and grammatical fixes). The Decision itself MUST NOT be changed. If you wish to override the decision or change it, you will need to create a new ADR whose purpose is to supersede the previous one.
