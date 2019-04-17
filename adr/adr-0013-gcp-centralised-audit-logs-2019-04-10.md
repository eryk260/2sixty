<!-- File format ddr/ddr-0000-project-keyword-YYYY-MM-DD.md -->

# DDR 0013: Centralised GCP Audit Logs

## Status

- [x] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context

This ADR is to Centralised GCP Audit Logs in BigQuery. 

GCP Stack Driver is use to for storing and monitoring audit logs both application and system logs. Stackdriver has retention period for each log. 

Requirement is to retain system audit logs beyond the standard retention  periods 
defined in stackdriver so that logs could be use for monitoring , alerting and SOX Compliance audit

 
More information : https://docs.google.com/document/d/13kSTJEthSwiGpJrB37F2PFcucMIctAYzNT7F5SuKcIU/edit?usp=sharing
 

## Decision
   Centralised Audit logs in BigQuery

## Consequences
   - Datalab report for monitoring and alerting can be developed using audit logs in BigQuery
   - Data from BigQuery table could be used for SOX Compliance audit
   - Audit logs retainedment policy can be determined based on Requirement of 2sixty
