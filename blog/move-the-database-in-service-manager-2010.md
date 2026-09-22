---
layout: default
permalink: /blog/move-the-database-in-service-manager-2010
---
# Move the database in Service Manager 2010
*Author: Brody Kilpatrick* | *Created: May 7, 2011*

2010-10-16 16:30:00Stefan Allansson  

# Move the database in Service Manager 2010

If you want to move the SQL database for SCSM to another SQL server, you can do like this:

Start with taking backup of your SCSM database on the first SQL server.   
In SQL server Manager, right click the Service Manager database and choose Tasks\Backup.   
Default choices are ok.

![Backup DB SCSM2010](http://lumagate.se/upl/images/163895.png)

Copy the database backup file to your new SQL server. You can put the file where you want on the server. Open the SQL Manager on the new SQL server and right click “Databases” and choose Restore Database. Fill in the database name and choose “from device” and specify the location for the backup file that you have copied to the new SQL server.

![Restore DB SCSM2010](http://lumagate.se/upl/images/163896.png)

Next step is to do some changes in the registry on the Service Manager Management Server.   
Change the “DatabaseServerName” to your new SQL server under:**HKEY\_LOCAL\_MACHINE\SOFTWARE\Microsoft\System Center\2010\Common\Database**

If you have changed the name on your database when you restored it on the new server you have to change the “DatabaseName” in the registry. Restart the service “System Center Data Access Service” on the Service Manager Management Server and then open the SCSM console.   
Now your Console is connected to your new SQL server.
