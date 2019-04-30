<!-- File format ddr/ddr-0000-project-keyword-YYYY-MM-DD.md -->

# DDR 0013: Centralised GCP Audit Logs

## Status

- [x] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context

This ADR is to Centralised GCP Audit Logs. 

No doubt GCP Stack Driver is use to for storing and monitoring audit logs both application and system logs. Stackdriver has retention period for each log. 

Requirement is to retain system audit logs beyond the standard retention  periods 
for Auditing purpose e.g. SOX compliance audit 

Develop Datalab reports for monitoring and alerting.
 
More information : https://docs.google.com/document/d/13kSTJEthSwiGpJrB37F2PFcucMIctAYzNT7F5SuKcIU/edit?usp=sharing
 
To properly audit activities and compliance , e.g GRC standards in a cloud environment multiple resources of information needs to be reviewed and correlated. 

There include a number of built in and third party services. Some are enabled by default ,and some need to be activated and configured . Ideally the various resource of actives logs and audit processes will be consolidated to facilitate change and incident management  

BigQuery script produced to monitor GCP audit Logs

At Essence , there is no Security Information and Event Management (SIEM) tool. We need to have a process which can monitor GCP audit logs see https://cloud.google.com/logging/docs/audit/ for more details. 

Cloud Audit Logging maintains three types of audit logs for each project, folder and organization - Admin Activity, System Event and Data Access. All GCP services writes audit log entries to these logs to help answer question of “who did what, where and When” within all GCP Projects 

Cloud Trails is what equivalent to Audit log  in AWS. Cross all cloud environment, Audit log is very critical for company to understand what is happening with your cloud environment.

Enable audit log from Org level. 
Cloud Platform resources are organized hierarchically, where the Organization node is the root node in the hierarchy, the projects are the children of the Organization, and the other resources are descendents of Projects. Once audit log is enabled from Org leve, it will automatically enable the audit log cross all the hierarchy. 
 Audit log retention
Individual audit log entries are kept for a specified length of time and are then deleted. 
https://cloud.google.com/logging/quotas#logs_retention_periods
Log type
Retention period
Admin Activity audit logs
400 days
Data Access audit logs
30 days
System Event audit logs
400 days
Access Transparency logs
400 days
Logs other than audit logs or Access Transparency logs
30 days

Aggregated Exports to central location
In order to save audit log for longer time, you need to use sink to save logs in to a persistent storage,e.g. GCP bucket or bigquery. 

You can create an aggregated export sink that can export log entries from all the projects, folders, and billing accounts of an organization. As an example, you might use this feature to export audit log entries from an organization's projects to a central location. You can have a shared project cross the entire organization dedicated for logging, audit log can be one of logs hosted within, you can easily manage it. You can have a Cloud Identity group, e.g security office  group for monitoring GCP security. 

Reporting

Once audit log is sinked into  BigQuery, you can use SQL to analyse GCP audit log to report any suspicious activity.

What you can query: 

project ownership assignments/changes
audit config changes - admin activity and data access logs produced by cloud audit logging enables security analysis, resource
custom role changes - monitoring roles creation, deletion and updating activities
VCPC network firewall rule changes
VPC network changes
SQL instance config changes
Enable auto backup and high availability - misconfig may adversely impact business continuity
Authorised network - misconfiguration may increase exposure to the untrusted networks
Cloud Storage IAM permission changes

Please see https://essencedigital.atlassian.net/browse/GS-25 for SQL query 

How should Audit logs be monitored

The script can be scheduled to run everyday , a security office can analysed the report and check if there is any suspicious activity
 
More information on HOWTO-Audit logs : https://docs.google.com/document/d/13kSTJEthSwiGpJrB37F2PFcucMIctAYzNT7F5SuKcIU/edit?usp=sharing
## Decision
   Centralised Audit logs in BigQuery

## Consequences
   - Datalab report for monitoring and alerting can be developed using audit logs in BigQuery
   - Data from BigQuery table could be used for SOX Compliance audit
   - Audit logs retainedment policy can be determined based on Requirement of 2sixty
