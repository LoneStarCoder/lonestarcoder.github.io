---
layout: default
permalink: /blog/data-warehouse-failure-concerns-answered
---
# Data warehouse failure concerns answered
*Author: Brody Kilpatrick* | *Created: June 28, 2011*

I have been having some concerns for the lack of failover options and documentation for the Service Manager data warehouse. I created a forum post which can be seen at <http://social.technet.microsoft.com/Forums/en-US/systemcenterservicemanager/thread/1479d2c4-25eb-4aaf-bd0d-9f95618d1a4a/>.  
  
  
> *If the DW goes down, Service Manager will continue grooming old work items but changes to existing or the creation of new work items won't be copied to the DW. The grooming job will delete any closed work item that has existed for more then your grooming policy setting. (Default settings is 90 days for IR and 365 days for CR and PR).**That means that if your DW goes down on Day 1, it could be down for another 89 days before any incidents created after day 1 will be groomed from your database. And that is if the incidents is closed. When you get your DW back up and running, any work item that was created or changed during the DW downtime will be copied to the DW.**Take a look at this blogpost:**http://blogs.technet.com/b/servicemanager/archive/2009/09/18/data-retention-policies-aka-grooming-in-the-servicemanager-database.aspx**Regards**//Anders**Anders Asp | Lumagate | www.lumagate.com | Sweden | My blog: www.scsm.se*
