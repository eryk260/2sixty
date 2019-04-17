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



## Consequences
   Benefit of following above folder structure is 
   
   	- we can define generic role/policies for Dev , Production & Test environment rather than defining rules at project level. 
   	- Roles and policies will be inherited based on environment.
   	- More clarity on state of project i.e. project in dev mode or production.
   	- Access to production instance of all project can be managed by roles/privilege at Production folder level.

