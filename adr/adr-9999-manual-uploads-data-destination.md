# ADR 9999: Manual uploads data destination

## Status

- [x] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context

Currently, manual upload csv files are processed and validated before the output data gets put onto an MIS schema table on the datacenter postgres database. Datalake consumes this data by accessing the MIS schema database table. The business objective is to remove the reliance/dependency on the MIS database.

## Decision

In addition to the data from the csv upload being loaded to MIS Schema, Olive will also send CSVs to a GCP bucket. Datalake will change the source from which it reads manual uploads to be this GCP bucket.

## Consequences

* Datalake will need to change the source from which it reads Manual uploads
* Datalake will need to read CSV files rather than database table rows
* Olive will need to be refactored to change the destination of the manual uploads
* Olive has a feature where it shows the historical manual uploads. To maintain this feature, we will need to archive all old manual uploads in the new destination too.
* When we actually remove the MIS db we will need to do cleanup on Olive to no longer upload the CSVs in the MIS schema since it will not exist and will throw errors.
