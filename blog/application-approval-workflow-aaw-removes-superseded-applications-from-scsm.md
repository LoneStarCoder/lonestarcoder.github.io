---
layout: default
permalink: /blog/application-approval-workflow-aaw-removes-superseded-applications-from-scsm
---
# Application Approval Workflow (AAW) Removes Superseded Applications from SCSM
*Author: Brody Kilpatrick* | *Created: November 9, 2016*

I was recently troubleshooting the following issue: A user requested an application from the SCCM Application Catalog that required approval. However, AAW was not creating a request in SCSM. The application did not exist in SCSM. Everything was working properly in Orchestrator's AAW runbooks.  
  
As it turns out, if an application is superseded, it is removed from SCSM. If you view the runbooks, you cannot see this. It must be hard-coded in the library files that Microsoft refuses to release the code for.  
  
Anyway, this cost me quite a bit of time, hopefully it will save someone else some time.
