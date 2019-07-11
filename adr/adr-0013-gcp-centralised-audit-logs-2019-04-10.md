<!-- File format ddr/ddr-0000-project-keyword-YYYY-MM-DD.md -->

# ADR 0013: Centralised GCP Audit Logs

## Status

- [x] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context

This ADR concerns the Centralisation and retention of GCP Audit Logs. 

*Scope :*

only GCP platform is being covered, other cloud providers will be covered as part of separate ADR.

GCP Stack Driver is used for storing and monitoring log entries,
both application and system categories of logs.

Not in Scope : Application logs 

In Scope : Everything else required for security audits e.g. System logs, Data Access logs and Storage access logs.

Stackdriver has default retention period of 90 days for each log entry.
We need to retain these log entries beyond the standard retention periods 
for Auditing purpose e.g. SOX compliance audit. 
We assume log retention period for audit entries is around 7 years. 

  
*Goal:*  
1 Facilitate audit activities and compliance within log retention period, 

   e.g GRC standards (https://en.wikipedia.org/wiki/Governance,_risk_management,_and_compliance) in a cloud environment 
       multiple resources of information could be reviewed and correlated. 

1 Ensure Capability to capture  audit log at all levels of Organisation hierarchy

1 To comply with all relevant retention policies.  

More information on HOWTO-Audit logs : https://docs.google.com/document/d/13kSTJEthSwiGpJrB37F2PFcucMIctAYzNT7F5SuKcIU/edit?usp=sharing

## Decision
   Centralised Audit logs in BigQuery

## Consequences
   - Facilitate running SQL queries against audit log entries stored in BigQuery Tables for suspicious activitites.
   - Faciliate builidng Reports for Security officer to analyse 
   - Data from BigQuery table could be used for SOX Compliance audit
 