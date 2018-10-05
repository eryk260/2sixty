
Context
Decision.Consequences



# PDR 0001: Olive and Scrutineer Integration

## Status

- [x] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context

To run Scrutineer campaigns, Analytics needs to create a pacing tracker and compile a post campaign analysis. These tasks require Analytics to map DCM Placement IDs to Scrutineer Survey IDs to Olive Placement IDs by querying MIS/Datalake and BigQuery.  This is not only time-consuming and costly, but also makes it difficult to quickly identify issues like discrepancies.  We have begun putting Scrutineer survey IDs in the DCM Ad Names to make this a little easier to identify, but there are some considerations around data quality, character limits and query efficiency that could be improved.

## Decision

I propose that we make the mapping process simpler and more reliable by integrating Scrutineer with Olive. I think there a a few options we could implement that would automate this mapping
  Capture Scrutineer Survey ID in Olive
  Capture the DCM Ad ID in Scrutineer
  Capture the Olive OPID in Scrutineer

This would streamline the trafficking process, reduce a step in the mapping file and bring us closer to a more automated solution. Clients who don’t use Olive may want to use Scrutineer as a standalone product, so I think it would be better to capture information in Scrutineer but open to suggestions.

## Consequences

* Time saved (per quarter) building+QAIng the mapping file
    * Analytics - 80 hrs
    * Planning -  4 hrs
    * Activation - 65 hrs
* Can automate QA with more reliable mapping
* Can automated monitoring for in-flight recruitment if IDs are mapped
* Enables Analytics to further automate the survey procedure
* Automated mapping file can improve accuracy of realtime dashboard results
* Distinction between Scrutineer and Non-Scrutineer placements more easily identifiable
* Additional step for Activation/Analytics to enter ID
* Increased maintenance of an Olive/Scrutineer/DCM mapping table
