---
layout: default
permalink: /blog/scsm-cube-processing-and-analysis-services-is-a-beast
---
# SCSM Cube Processing and Analysis Services is a Beast
*Author: Brody Kilpatrick* | *Created: July 11, 2012*

If you are using the Service Manager DW and cubes, you may have ran into some issues with the cubes not processing, failing, data issue, or something else. I have provided a couple of resources to help with troubleshooting at the bottom of my post, but I want give a little insight on my experience in dealing with what could possibly become a maintenance headache.  
  
Back story: I am currently working on a DEV and PRD SCSM 2012 RTM environment for a client. Each environment is up. DEV is being heavily used, but PRD is not. They have slightly different configurations, and each cube processing issue was resolved using two different methods.  
  
Because development is non-impacting, after troubleshooting for a few hours and not being about to resolve the issue, i figured it was best to reinstall the DW. After uninstall, reinstall, and re-sync everything works ALL data intact.  
  
  
**DEV Steps:**  
  

1. Go into the console > Administration
2. Unregister the DW
3. On the DW Server, Go to Add/Remove Programs > Uninstall System Center 2012 Service Manager

1. If you get an error about a log not being found, do this:

1. Shift Right Click "Add/Remove Programs" and select run in a different process

4. After the uninstall, restart the machine
5. After the machine restarts, go into the registry of the DW Management Server and remove the following keys and all sub keys:

1. System Center
2. Microsoft Operations Manager

6. Restart the Machine again
7. Go REMOVE/DELETE the DW databases
8. Go REMOVE/DELETE the DW Analysis services database
9. On the DW Management Server, perform a fresh install, following the prompts and creating new databases.
10. Once the install is complete, re-register SCSM to the DW
11. Leave it Alone for 24 hours
12. After 24 hours check to see if all the jobs and cubes have processed

The steps above (for dev) were quicker than troubleshooting. Hope this helps.

**PRD Steps - Actualy troubleshooting, not re-install**

Analysis services is install on the DW server. I noticed in the event log, I was receiving event 33573. One of the events stated "The operation has been cancelled due to memory pressure." This seemed pretty obvious, so I opened task manager, attempted to process the cube, and noticed that it maxed out my 8GB of memory in a couple of minutes, then the memory utilization dropped. I checked the event log again, and I received the same error. So, I increased the memory 16 GB, and processed again - No More Memory Errors. 4 of the 6 cubes processed. I still have two that are failing, but not because of memory. You might need to increase your memory above 16GB depending on the number of work and config items.

After fixing the memory issue, I noticed the following events:  

|  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- |
|  |  |  |  |  | Message : An Exception was encountered while trying to process a cube.  Cube Name: SystemCenterChangeAndActivityManagementCube Exception Message: An exception occurred while processing the cube.  Please see the event viewer log for more information.  Cube:  SystemCenterChangeAndActivityManagementCube Stack Trace:    at Microsoft.SystemCenter.Warehouse.Olap.OlapCube.Process(ManagementPackCube mpCube). |
| Warning | 7/11/2012 7:53 | Data Warehouse | 33573 | None | Message : An Exception was encountered while trying during cube processing.  Message=  Processing warning encountered - Location: , Source: Microsoft SQL Server 2008 R2 Analysis Services Code: 1092550657, Description: Errors in the OLAP storage engine: The attribute key cannot be found when processing: Table: 'ActivityAssignedToUser', Column: 'ActivityDimKey', Value: '2'. The attribute is 'ActivityDimKey'.. |
| Warning | 7/11/2012 7:53 | Data Warehouse | 33574 | None | Message : Cube Processing workitem has failed.  This is most likely caused by the DWDataMart (primary datamart) being out of sync from other marts.  This is an intermittent problem and will resolve on its own as the Load jobs complete their runs.  However, to work around this issue, administrators can manually start the Load.Common load job, wait for it to complete and then start the Cube processing job. |
| Error | 7/11/2012 7:53 | Data Warehouse | 33573 | None | Message : An Exception was encountered while trying during cube processing.  Message=  Processing error encountered - Location: , Source: Microsoft SQL Server 2008 R2 Analysis Services Code: -1054932986, Description: Errors in the OLAP storage engine: The process operation ended because the number of errors encountered during processing reached the defined limit of allowable errors for the operation..       Processing error encountered - Location: , Source: Microsoft SQL Server 2008 R2 Analysis Services Code: -1054932978, Description: Errors in the OLAP storage engine: An error occurred while processing the 'ActivityAssignedToUser' partition of the 'ActivityAssignedToUser' measure group for the 'SystemCenterChangeAndActivityManagementCube' cube from the DWASDataBase database..       Processing error encountered - Location: , Source: Microsoft SQL Server 2008 R2 Analysis Services Code: -1054932986, Description: Errors in the OLAP storage engine: The process operation ended because the number of errors encountered during processing reached the defined limit of allowable errors for the operation..       Processing error encountered - Location: , Source: Microsoft SQL Server 2008 R2 Analysis Services Code: -1056964601, Description: Internal error: The operation terminated unsuccessfully..       Processing error encountered - Location: , Source: Microsoft SQL Server 2008 R2 Analysis Services Code: -1055129598, Description: Server: The operation has been cancelled.. |
