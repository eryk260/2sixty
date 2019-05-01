<!-- File format adr/adr-0000-project-keyword-YYYY-MM-DD.md -->

# ADR 0014: Architecture decision record template

## Status

- [x] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context

To have a consistent view of security and level of access for every user on GCloud per project.


## Decision 
Tao Song our security consultant proposes following Folder structure for all the projects:

260 folder

	Shared folder
		- Security project
		- Admin project
		- Logging project
		- Monitoring project
		- Host ( this is the host for shared vpc if you need VPN and interconnect

	Production folder
		- Security project 2sixty-prod-proj-security
		- Admin project

	Dev folder
		 - Security project 2sixty-dev-proj-security (time to live needs to be determined) 
		 - Admin project

	SIT/Test folder ( system integration test env)
		- Security project
		- Admin project

Proposed Security Folder structure : https://docs.google.com/presentation/d/1wFKmTN6W0OVZvvKm2cqrb2jwgrJlYXLdZ9PT0h7s91Y/edit?usp=sharing


## Consequences
   The benefit of following the above folder structure :
   
		- Generic role/policies for Dev, Production & Test environment rather than defining rules at the project level. 
		- Roles and policies will be inherited based on the environment.
		- More clarity on the state of project i.e. project in dev mode or production.
		- Access to the production instance of all project can be managed by roles/privilege at the Production folder level.
		-  Setting up Audit log at folder level will enable capturing all the projects under the folder. e.g Setting up audit log at the Production folder will ensure all the projects in production have uniform audit logs.


