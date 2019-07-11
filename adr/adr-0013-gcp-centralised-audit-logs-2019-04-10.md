<!-- File format ddr/ddr-0000-project-keyword-YYYY-MM-DD.md -->

# DDR 0013: Centralised GCP Audit Logs

## Status

- [x] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context

This ADR concerns the Centralisation and retention of GCP Audit Logs. 

*Scope :*
only GCP platform is being covered, other cloud providers will be covered as part of separate ADR.

GCP Stack Driver is used for storing and monitoring logs,
both application and system categories of logs.
Stackdriver has default retention period of 90 days for each log entry.

Not in Scope : Application logs 
In Scope : Everything else required for security audits e.g. System logs, Data Access logs and Storage access logs.

We need to retain these log entries beyond the standard retention periods 
for Auditing purpose e.g. SOX compliance audit 

  
*Goal:*  
1) To properly audit activities and compliance , 

   e.g GRC standards in a cloud environment 
       multiple resources of information could be reviewed and correlated. 

2) Once audit log is sinked into  BigQuery, you can use SQL to analyse GCP audit log to report any suspicious activity.
 
3) Build Capability to capture  audit log at Organisaiton level or folder level

4) Aggregated Exports to central location

5) Reports could be build to be used by Security officer to analyse and report if there is any suspicious activity

6) Data from BigQuery table can be used for SOX Compliance audit. 




More information on HOWTO-Audit logs : https://docs.google.com/document/d/13kSTJEthSwiGpJrB37F2PFcucMIctAYzNT7F5SuKcIU/edit?usp=sharing

## Decision
   Centralised Audit logs in BigQuery

## Consequences
   - Datalab report for monitoring and alerting can be developed using audit logs in BigQuery
   - Data from BigQuery table could be used for SOX Compliance audit
   - Audit logs retainedment policy can be determined based on Requirement of 2sixty
