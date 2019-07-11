<!-- File format ddr/ddr-0000-project-keyword-YYYY-MM-DD.md -->

# DDR 0013: Centralised GCP Audit Logs

## Status

- [x] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context

This ADR is to Centralised GCP Audit Logs. 

Scope :
 GCP Stack Driver is use to for storing and monitoring audit logs both application and system logs. Stackdriver has retention period for each log. 

Requirement is to retain system audit logs beyond the standard retention  periods 
for Auditing purpose e.g. SOX compliance audit 

Develop Datalab reports for monitoring and alerting.
  
Goal:  
1) To properly audit activities and compliance , e.g GRC standards in a cloud environment multiple resources of information needs to be reviewed and correlated. 

There include a number of built in and third party services. Some are enabled by default ,and some need to be activated and configured . Ideally the various resource of actives logs and audit processes will be consolidated to facilitate change and incident management  

2) Once audit log is sinked into  BigQuery, you can use SQL to analyse GCP audit log to report any suspicious activity.

 
3) Build Capability to capture  audit log at Organisaiton level or folder level

4) Aggregated Exports to central location

5) Reports could be build to be used by Security officer to analyse and report if there is any suspicious activity

6) Data from BigQuery table could be used for SOX Compliance audit


More information on HOWTO-Audit logs : https://docs.google.com/document/d/13kSTJEthSwiGpJrB37F2PFcucMIctAYzNT7F5SuKcIU/edit?usp=sharing

## Decision
   Centralised Audit logs in BigQuery

## Consequences
   - Datalab report for monitoring and alerting can be developed using audit logs in BigQuery
   - Data from BigQuery table could be used for SOX Compliance audit
   - Audit logs retainedment policy can be determined based on Requirement of 2sixty
