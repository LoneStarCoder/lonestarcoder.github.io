---
layout: default
permalink: /blog/service-manager-performance
---
# Service Manager Performance
*Author: Brody Kilpatrick* | *Created: December 29, 2010*

Excerpt from the MS website: <http://technet.microsoft.com/en-us/library/ff461124.aspx>  
Updated: December 1, 2010  
Applies To: System Center Service Manager 2010  

Performance for Service Manager server roles and features are affected by different factors. Generally, there are three areas where positive and negative performance is most noticeable in Service Manager:  

- Service Manager console responsiveness. This is the length of time it takes from the moment you take some sort of action in the console until it completes.
- Data insertion time for connectors. This is how long it takes for Service Manager to import data when a connector synchronizes.
- Workflow completion time. This is the length of time it takes for workflows to automatically apply some kind of action.

## Connector Performance

Connector initial synchronization can take a significant amount of time, for example 8-12 hours for a large initial synchronization with System Center Configuration Manager. As a connector synchronizes initially, you can expect performance to suffer for all Service Manager server roles and processes during this time. This occurs because of the way that data is inserted sequentially into the Service Manager database, which is a SQL Server database. Although you cannot hasten the connector’s initial synchronization process, you can plan for the initial synchronization and to ensure that the synchronization process completes well before Service Manager is put into production.  
Once the initial synchronization is complete, Service Manager continues synchronizing the differences, which does not have a measurable impact on performance.

## Workflow Performance

Workflows are automatic processes that occur and include sending e-mail notifications, the next step of a change request activating, and automatically applying a template.  

- Normally, workflows start and finish within 1 minute. When Service Manager server roles are under a heavy workload, workflows do not complete as quickly as normal.
- Additionally, when you create new workflows, such as a new notification subscription, additional load is placed on the system. As the number of new workflows that you create increases, the time it takes for each one to run also increases.

When the system is under a heavy load, if for example a large number of new incidents are being created and each incident generates many workflows, then performance might be negatively affected.  
If you plan to create a large number of workflows, one possible solution to help improve performance is to use the ManagmentHostKeepAlive management pack that is included in the Service Manager release media.  

- You need to manually copy the two files from the source directory into the Service Manager installation directory, and then import the management pack files.
- Importing these management pack files can greatly increase workflow processing responsiveness where almost all workflows process within 1 minute.
- However, importing this management pack gives higher priority to workflow processing and can lead to slower Service Manager console response in some cases so you should test its impact before deployment in a production environment.

## Groups, Queues, and User Roles Impact on performance

You should plan for groups and user roles early. Often, people create groups to make sure users have access to specified groups only. For example, in one scenario you might want to create a subset of incidents such as incidents that affect computers used by human resource personnel. In this scenario, you might want only specific personnel to be able to view or modify the group of sensitive servers. Then, to enable this type for access you would need to create a group for all users and a group for sensitive computers, and then ensure that a security role has access to both the All Users and the Sensitive Servers groups. Inevitably, creating a group containing all users results in performance impact because Service Manager frequently checks to determine if there are changes to the group. This check occurs once every 30 seconds, by default. For a very large group, checking for the changes creates a heavy load on the system and may slow down response time considerably.  
Solution 1: You can manually specify how often Service Manager checks for group changes by modifying a registry key. For example, if you change the group check frequency from 30 seconds to 10 minutes, you will significantly increase performance.  

| CautionCaution |
| --- |
| Incorrectly editing the registry may severely damage your system. Before making changes to the registry, you should back up any valued data on the computer. |

#### To manually specify the group change check interval

1. Run regedit and navigate to HKEY\_LOCAL\_MACHINE\SOFTWARE\Microsoft\System Center\2010\Common\.
2. Create a new DWORD value named **GroupCalcPollingIntervalMilliseconds**.
3. For its value, specify the interval in milliseconds. The result is multiplied by 6. For example, to set the interval to 10 minutes, type **1000000**.
4. Restart the System Center Management service.

Solution 2: You can use a Windows PowerShell script to add objects of a type, such as “Users”, to a user role. Essentially, an analyst logged on in this role can access all objects that have a type equal to “User”. If you use this method, you eliminate the need for a very large group (“All Users”) and the expensive check that Service Manager performs to determine this group membership. On the Service Manager management server, you can run the following Windows PowerShell script to set add “user” type to a role “RoleName”. You will need to modify this example script for your environment.  

#### To run a Windows PowerShell script to add objects to a user role

- Modify as needed and then run the following script.

  

[Copy Code](javascript:CopyCode('ctl00_MTCS_main_ctl02_code'); "Copy Code")

```
#
# Insert a "type" scope in a role
# Syntax:
#   AddTypeToRoleScope -server "put_server_name_here" -RoleName "put display name of the role here" -TypeToAdd "put display name of the type to add to scope here"
#
# Note:  This is a simple demonstration script without error checking. 
# 
 
# set script parameter defaults
param ([String]$Server = "localhost", [String]$RoleName="My Analyst Role", [String]$TypeToAdd="User")
 
 
$a = [reflection.assembly]::LoadWithPartialName("Microsoft.EnterpriseManagement.Core")
 
$m = new-object Microsoft.EnterpriseManagement.EnterpriseManagementGroup $Server 
 
# Get Type object
#   Note:  If you need to get a list of all available classes related to (for example) “User”,   use this command:
#               $m.EntityTypes.GetClasses() | ?{ $_.Name -like '*user*'} | %{ $_.Name}
#
$type = $m.EntityTypes.GetClasses() | ?{ $_.DisplayName -eq $TypeToAdd}
 
# Get role object, and insert the type GUID into scope
$role = $m.Security.GetUserRoles()  | ?{ $_.DisplayName -eq $RoleName}
$role.Scope.Objects.Add($type.Id)   
$role.Update()
 
# 
# Get the value from the database again and validate it is there
if ( $role.scope.objects.Contains($type.Id) ) {
    write-host *** Successfully set the scope for role `" $role.DisplayName`" and it now contains all instances of $type.DisplayName `( $type.Name `)
} else {
    write-host "There was an error trying to insert the scope into the role."
}
```

## View Performance

When creating views, plan on using “typical” classes in the system whenever possible. Most object classes, for example Incident Management, have two types: “typical” and “advanced”. The typical object type contains simple references to a small subset of data related to an item. The advanced type contains many complex references to data related to an item. Typical types are simple projections, advanced types are complex projections. Most advanced object types are used to populate different fields in forms that you would not normally want to see displayed in a view. Whenever you create a view based on an advanced object type and when you open the view, Service Manager queries the database and a large amount of data is read. However, very little of the retrieved data is actually displayed or used.  
If you have performance problems with the views you’ve defined and you’ve used advanced object types in views, you should switch to using typical types. Or alternatively, you can create your own projection types that contain only the data you need to base a view upon. Refer to the [Creating Views That Use Related Property Criteria (Type Projections) : Software Views Example blog post](http://go.microsoft.com/fwlink/?LinkId=184819) (http://go.microsoft.com/fwlink/?LinkId=184819) blog entry on the SCSM Engineering Team Blog.

## Service Manager Database Performance

Performance of the Service Manager database is directly affected by various factors including the number of concurrent Service Manager consoles reading or writing data, the group change check interval, and data inserted by connectors. More information is available in this document. Here are a few key points.  

- You should have a minimum of 8 GB of RAM for the management server that hosts the Service Manager database in order to have acceptable response time in typical scenarios.
- You should have at least 4 CPU cores on the computer hosting the Service Manager database.
- You can achieve better database performance by segregating log files and data files to separate physical disks, if possible. You can achieve further benefits by moving your tempdb on a different physical RAID drive than that of the Service Manager database. Use a RAID 1+0 disk system to host your Service Manager database, if possible.
- Performance can be negatively impacted if the Service Manager database is created with a smaller size and set to autogrow especially by small increments.

Refer to the Service Manager Sizing Helper tool included in the [Service Manager job aids](http://go.microsoft.com/fwlink/?LinkId=186291) documentation set (http://go.microsoft.com/fwlink/?LinkId=186291) to help assess the size of the database and create the database with a size closer to the final size, this will help performance by reducing the amount of times the database has to autogrow.  
Similarly all the other best practices that are applicable to a high performing database are applicable, as well. For example if you could take advantage of a superior disk sub system, you could benefit from splitting up the groups of tables on respective filegroups and moving them to a different physical drives.

## Service Manager Management Server Performance

Performance of the Service Manager management server is primarily affected by the number of active concurrent Service Manager consoles. Because all Service Manager roles interact with the management server, you should consider adding additional management servers if you plan to have a large number of concurrent consoles. You should have a minimum of 8 GB of RAM for the management server. You should have at least 8 CPU cores per management server, assuming you have 10-12 active consoles per CPU core, for a total of 80-100 consoles per management server.

## Service Manager Console Performance

Performance of the Service Manager console is primarily affected by the number of forms your analysts typically have open and the amount of data retrieved by views. You should have a minimum of 2 GB of RAM for the computer where the Service Manager console is installed. If you have views that retrieve a large amount of data, you will need additional RAM. You should have at least a dual-core CPU for the computer where the Service Manager console is installed. Because the Service Manager console is an end user application, we recommended that you restart it if you see excessive resource consumption – the Service Manager console aggressively caches information in memory, which can contribute to overall memory usage.

## Service Manager Data Warehouse Database Performance

Performance of the data warehouse is directly affected by various factors including the number of concurrent Service Manager management servers sending data, volume of data stored or the data retention period, rate of data change, and the ETL frequency. The amount of data stored in the data warehouse increases over time. Ensuring that you archive unnecessary data is important. Additionally, you can achieve better performance by segregating log files and data files to separate physical disks. Similarly you can achieve better throughput by putting the tempdb on a different physical disk than the other databases. Lastly, you can benefit by placing the three different databases on their respective physical disks, as well. Use a RAID 1+0 disk system to host your data warehouse, if possible. You should generally have a minimum of 8 GB of RAM for the computer where the data warehouse databases are installed, you will benefit from more memory on the SQL Server that hosts the data warehouse and even more so if the Datamart and Repository databases are on the same server. However, if you have 4,000 or fewer computers, then 4 GB is sufficient. You should have at least 8 CPU cores in the computer where the data warehouse database is installed. Additional cores will help both ETL and report performance.  
Performance can be negatively impacted if all the databases in the system are created with a smaller size and set to autogrow especially by small increments. Refer to the Service Manager Sizing Helper tool included in the [Service Manager job aids](http://go.microsoft.com/fwlink/?LinkId=186291) documentation set (http://go.microsoft.com/fwlink/?LinkId=186291) to assess the size of the database and create the database with a size closer to the final size, which will help performance by reducing the amount of times the database has to autogrow.  
Similarly all the other best practices that are applicable to a high performing database are applicable, as well. For example if you could take advantage of a superior disk sub system, you could benefit from splitting up the groups of tables on respective filegroups and moving them to a different physical drives.

## Service Manager Data Warehouse Server Performance

Performance of the data warehouse server is affected by the number of Service Manager management servers that are registered to the data warehouse and by the size of your deployment. You should generally have a minimum of 4 GB of RAM for the data warehouse server; however you’ll benefit by having additional memory up to 8 GB of RAM for advanced deployment scenarios where more than one Service Manager management server inserts data into data warehouse. If you must tradeoff performance, your highest priority should be for memory for the SQL Server. You should have at least 4 CPU cores to prevent performance problems. The data warehouse server is mostly stateless and it is unlikely to pose an I/O problem, so it should not present a performance problem.

## Self Service Portal Performance

The Self-Service Portal is designed for easy access to incident filing and software self-provisioning. It is not designed to handle thousands on users simultaneously using it. When complete, more thorough performance guidelines for the Self-Service Portal will be published.  
Performance testing for the Self-Service Portal was focused on typical “Monday morning” scenarios. Specifically, to ensure that on Monday morning hundreds of users can log in within the span of 5-10 minutes and open incidents with acceptable (less than 4-5 seconds) response times. This goal was achieved with the minimum hardware recommended in this document.
