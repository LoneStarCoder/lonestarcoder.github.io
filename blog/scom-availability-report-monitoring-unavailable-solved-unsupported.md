---
layout: default
permalink: /blog/scom-availability-report-monitoring-unavailable-solved-unsupported
---
# SCOM Availability Report Monitoring Unavailable SOLVED (Unsupported)
*Author: Brody Kilpatrick* | *Created: February 27, 2013*

I have seen numerous posts floating around regarding the SCOM Availability Reports showing "Monitoring Unavailable" even though the objects were healthy for the time period. For example, I can run the SCOM Availability Report, select "Exchange 2007 Service”, select the date range and expects lots of green, yellow, and red, but instead I primarily see dark gray.  
  
The issue could be caused by the following:  

- Bad Calculations during health rollups
- Bad Performance
- The Data Warehouse is behind
- and others

Several blog posts already exist regarding the above issues and can be found with Google/Bing. We will not be addressing those specific issues. We will be dealing with a very specific issue, which as far as I can tell is a bug, but I am not going to hold my breath.   

## Data Warehouse Availability Aggregation Process

SCOM has a process called the Data Warehouse Availability Aggregation Process. The Data Warehouse Availability Aggregation Process is somewhat complicated with about 10 steps that you probably don't care about. If you do, you can check [this diagram](http://michelkamp.files.wordpress.com/2012/04/image3.png), which gives a fairly decent picture of what is going on in the process.  
  
Remember the issues various people continue to have with **Management Server Resource Pools** becoming unavailable in SCOM 2012? That resource pool unavailability is calculated just like any other object. Inside the Data Warehouse, a table called "dbo.HealthServiceOutage" keeps the outage data when this occurs on all objects. However, sometimes, it *forgets* to enter an outage end time.  **And that is the key to our current issue.**  
  
So lets take a look at the Health Service Outage table.  

*Select top 10 \* FROM dbo.HealthServiceOutage with ( NOLOCK )*

[![](/assets/blog/HSOQR.png)](/assets/blog/HSOQR.png)

You can see how each managed entity has reason for the outage, start time, and end time. However, in some cases, the end time will be NULL.

[![](/assets/blog/HSOQRwnulls.PNG)](/assets/blog/HSOQRwnulls.PNG)

There are two reason for this.

1. The object that is "unavailable" is **truly** still unavailable. This SHOULD be NULL.
2. The object is now healthy, but an EndDateTime did not get written. Unless you have a big problem in your environment, you should have very few of these.

A bunch of Ids doesn't get us very much information, so lets enhance the query a bit. This query shows us ONLY the items with a NULL EndDateTime. It is also joined to the ManagedEntity table so we can see the actual names of the objects.

*Select h.HealthServiceOutageRowId, h.StartDateTime, h.EndDateTime, h.ReasonCode, h.DWLastModifiedDateTime, me.ManagedEntityRowId, me.DisplayName, ME.FullName*

*FROM dbo.HealthServiceOutage h with ( NOLOCK )*

*JoinManagedEntity me on h.ManagedEntityRowId = me.ManagedEntityRowId*

*WhereEndDateTime IS NULL*

[![](/assets/blog/HSOQamrpnull.PNG)](/assets/blog/HSOQamrpnull.PNG)

Notice the "All Management Servers Resource Pool" EndDateTime is NULL. Assuming it is actually available, this should have the actual end date.

The Health Service Outage data is considered in the availability calculations of the Standard Data Set in the Data Warehouse. If you look at any of the State.StateDaily\_[GUID] tables, you will see the "HealthServiceUnavailableMilliseconds" column is always maxed out, assuming the other columns are 0. We can make the Data Warehouse *recalculate* this data by modifying the "Health Service Outage" table.

If you want to make your objects available again, you can follow the steps below. Please note:

1. This is NOT supported by Microsoft
2. You SHOULD backup your Data Warehouse before making any changes
3. After we make the changes, you Data Warehouse might have to do A LOT of recalculate, causing a kick in performance for a short period of time, or it might cause you data warehouse to fall behind for a little while. If you are having performance issues with your Data Warehouse, you should address them first.

### How to Make my objects available again in SCOM Availability Reports

BACKUP YOUR OPERATIONSMANAGERDW DATABASE

**Open SQL Management Studio and connect to your OperationsManagerDW**

The first thing we want to do, is make sure your Data Warehouse is not behind. Run the below query. If you get more than one of two rows, then follow [this article](http://blogs.technet.com/b/operationsmgr/archive/2011/09/06/standard-dataset-maintenance-troubleshooter-for-system-center-operations-manager-2007.aspx) and catch up your data warehouse.

*DECLARE @DatasetId uniqueidentifier*

*SELECT*

*@DatasetId = DatasetId*

*FROM Dataset d*

*WHERE (d.DatasetDefaultName ='State data set')*

*SelectAggregationDateTime, AggregationTypeId*

*FromStandardDatasetAggregationHistory*

*Where DatasetId = @DatasetId*

*And*

*DirtyInd = 1*

Assuming your data warehouse is not behind, let's continue. Paste and run the following query into SQL Management Studio.

*Select h.HealthServiceOutageRowId, h.StartDateTime, h.EndDateTime, h.ReasonCode, h.DWLastModifiedDateTime,*

*me.ManagedEntityRowId, me.DisplayName, ME.FullName*

*FROM dbo.HealthServiceOutage h with ( NOLOCK )*

*Join ManagedEntity me on h.ManagedEntityRowId = me.ManagedEntityRowId*

*Where  DisplayName = 'All Management Servers Resource Pool' and EndDateTime IS NULL*

  

If you have results, then you have an EndDateTime that is NULL. Before assuming that is should not be NULL, you should go into SCOM and verify the state of the object first to make sure it is available. If the state of the object is available, but your query returned one or more NULL EndDateTime entries, then lets continue.

Now we need to update the HealthServiceOutage table and enter an EndDateTime for the results above. The query below does the following:

1. Gets the rows from the query above
2. Updates the DWLastModifiedDateTime to the current UTC Date and Time
3. Updates the EndDateTime to match the StartDateTime

  

Why are we modifying the DWLastModifiedDateTime?

We want the data warehouse to recalculate the states, so the reports accurately reflect availability. We must update this column, otherwise recalculation will not happen.

If you a savvy SQL Query person, then you can update the below query and enter an EndDateTime of your choosing in UTC. I have decided that I want to make the EndDateTime the same of the StartDateTime, because I don't really know how long the resource pool was down, but it is usually a very short time.

NOTE: THIS QUERY WILL MAKE CHANGES TO THE HEALTHSERVICEOUTAGE TABLE

Paste and execute the following query into SQL Management Studio.

*Update dbo.HealthServiceOutage*

*SetDWLastModifiedDateTime = GETUTCDATE(),EndDateTime=StartDateTime*

*WhereHealthServiceOutageRowId*

*IN (*

*Select h.HealthServiceOutageRowId*

*FROM dbo.HealthServiceOutage h with ( NOLOCK )*

*JoinManagedEntity me on h.ManagedEntityRowId = me.ManagedEntityRowId*

*Where  DisplayName ='All Management Servers Resource Pool' and EndDateTime IS NULL)*

If you want to verify the changes, you can run the following Query.

*Select h.HealthServiceOutageRowId,h.StartDateTime,h.EndDateTime,h.ReasonCode, h.DWLastModifiedDateTime,me.ManagedEntityRowId, me.DisplayName, ME.FullName*

*FROM dbo.HealthServiceOutage h with ( NOLOCK )*

*JoinManagedEntity me on h.ManagedEntityRowId = me.ManagedEntityRowId*

*Where  DisplayName ='All Management Servers Resource Pool' and EndDateTime =StartDateTime*

*order by DisplayName*

After we make the change, the next time Standard Data Set Maintenance runs, it will recalculate Availability. It will make the DW look like it is behind, but just give it time to catch up and calculate the state. This could take several hours. You can run the query below periodically and verify that your row count is getting smaller. My row count increased to about 350, and over about 12 hours reduced down to the normal one or two rows.

*DECLARE @DatasetId uniqueidentifier*

*SELECT*

*@DatasetId = DatasetId*

*FROM Dataset d*

*WHERE (d.DatasetDefaultName ='State data set')*

*SelectAggregationDateTime, AggregationTypeId*

*FromStandardDatasetAggregationHistory*

*Where DatasetId = @DatasetId*

*And*

*DirtyInd = 1*

Once the Standard Data Set calculations are finished, run your reports and verify they are no longer gray.

On another note, the above steps will work for objects other than the Resource Pool. However, the resource pool is what caused ALL of my objects to be gray in reports. It is possible that other objects with NULL EndDateTime entries can cause that specific object to be gray in a report. I also had other objects that did not have an EndDateTime properly set. So, for each of the objects, I simply verified the were available in SCOM, then I set the EndDateTime and the DWLastModifiedDateTime. Remember, if an object in SCOM is unavailable then the EndDateTime SHOULD BE NULL.
