---
layout: default
permalink: /blog/scom-query-notification-subscription-data-via-sql
---
# SCOM - Query Notification Subscription Data via SQL
*Author: Brody Kilpatrick* | *Created: February 5, 2015*

I needed to provide data about SCOM notification subscriptions via SQL Reporting Services. I was hoping to find a simple answer, so I went to searching online. It seemed that no one had posted anything related to querying notifications from the OperationsManager database. I am well aware of the Powershell cmdlets that can be used to get subscription information, but I love SQL, queries, and SSRS. I started digging around but had quite a bit of trouble finding any subscription information, so I began doing database searches using subscription GUIDs. Surprisingly, I only found the data in a few tables. This is not normally what most of us are used to querying from SCOM, so I had to do a little learning, and I want pass the queries on to you.

The queries and data are from the OperationsManager database. You can use them with ReadOnly credentials.

  
First of all, lets take a look at the tables. My GUID search led me to the tables and columns below; this was very disheartening, but I kept moving forward.  

- ManagementPack.MPRunTimeXML
- Module.ModuleConfiguration
- ManagementPack.MPXML
- ModuleType.MDTImplementationXML

Of the above 4 tables, we really only need 1 - Module.

However, Microsoft recommends that we stick to views if we can help it, and we do have a view that is associated with the module table, which is "RuleModule."

There are a few other table/views that we need just to get some key data, so our complete list of needed views/tables are:  

- RuleModule
- ModuleType
- RuleView

The rest of the details come from the xml configuration in RuleModule, which turns something that should be relatively simple, into something a little more complicated. This is not a tutorial on how to query and parse xml columns, so I will not go into those details.  
  
SCOM notification essentially have 4 components:  
  

1. Channel - SMTP, command, SMS, etc. In the xml, this is actually called "Protocol".
2. Subscriber - This is the recipient. A subscriber can have multiple addresses and protocols,
3. Address - Email Address, phone number, command location. In the xml this is called "Device". Addresses are found as part of a subscriber.
4. Subscription - The subscription is actually a SCOM rule that triggers based on the criteria specified. The rule module is where we find the components of the subscription we need.

**The Queries**

**Subscription Query**

Let's start with the subscription. Like I said earlier, a subscription is a rule. Rather than writing a query to only get one subscription, I decided to get *all* subscriptions, which could then be filtered.  
  

[![](/assets/blog/SubscriptionQuery.png.jpg)](/assets/blog/SubscriptionQuery.png.jpg)

  
> *Select r.RuleId, r.RuleModuleId, r.enabled, R.DisplayName,D.C.value('RecipientId[1]','varchar(4000)') SubscriberId  
> FROM (  
> Select RuleId, rm.RuleModuleId, enabled, r.DisplayName, cast(RuleModuleConfiguration as XML) xmlquery  
> FROM  
> RuleModule rm  
> join RuleView r on rm.RuleId = r.Id  
> where r.Category = 'Notification'  
> and RuleModuleName = 'CD1'  
> ) r  
> Cross Apply xmlquery.nodes('//DirectoryReference') D(C)*

You can filter this query to give you a single subscription Id by name, Id, enabled/disabled, etc.  
  
**Subscriber and Device Query**  
The next query gets the subscriber and the subscriber's devices. When you run the query, rows may look like they are repeating. The row will repeat for each device. Because a subscriber could have an unknown number of devices, I did want to give variable columns counts. Therefore, you can use your own grouping and SQL magic to manipulate the results.  
  

[![](/assets/blog/SubscriberQuery.jpg)](/assets/blog/SubscriberQuery.jpg)

> *Select r.SubscriberName, r.SubscriberId,  
> D.C.value('Name[1]','varchar(4000)') DeviceName,  
> D.C.value('Protocol[1]','varchar(4000)') DeviceProtocol,  
> D.C.value('Address[1]','varchar(4000)') DeviceAddress  
> FROM (Select  N.C.value('Name[1]','varchar(4000)') SubscriberName,  
>  N.C.value('RecipientId[1]','varchar(4000)') SubscriberId,  
>  n.c.query('.') as xmlquery  
> from (  
>  Select cast(MDTImplementationXML as xml) Recxml  
>  FROM [dbo].[ModuleType] mt  
>  Where MDTName = 'Microsoft.SystemCenter.Notification.Recipients'  
>  )a Cross Apply Recxml.nodes('//Recipient') N(C)) r  
> Cross Apply xmlquery.nodes('//Device') D(C)*

**Complete Query of Subscriptions with Subscribers and Devices**  
Both queries can be used alone to provide good information. However, what I really wanted was a list of subscriptions and who those subscription go to. For this I combined the two queries to give a nice result set that I could use in an SSRS report. Once I wrote the SSRS report, I used SSRS URL parameters to call the report directly from a SCOM notification.  

[![](/assets/blog/BothQueries.jpg)](/assets/blog/BothQueries.jpg)

  
> *-- This is the 2 queries joined together to provide the data that we want  
> Select Subscriptions.DisplayName, Subscriptions.RuleId,  
> Case subscriptions.enabled When 0 Then 'No' Else 'Yes' End as SubscriptionEnabled,  
> Subscribers.SubscriberName, Subscribers.DeviceName, Subscribers.DeviceProtocol,  
> Subscribers.DeviceAddress  
> FROM (  
>  --This is the whole subscriber query with Address   
>  Select r.SubscriberName, r.SubscriberId,  
>  D.C.value('Name[1]','varchar(4000)') DeviceName,  
>  D.C.value('Protocol[1]','varchar(4000)') DeviceProtocol,  
>  D.C.value('Address[1]','varchar(4000)') DeviceAddress  
>  FROM (Select  N.C.value('Name[1]','varchar(4000)') SubscriberName,  
>  N.C.value('RecipientId[1]','varchar(4000)') SubscriberId,  
>  n.c.query('.') as xmlquery  
>  from (Select cast(MDTImplementationXML as xml) Recxml FROM [dbo].[ModuleType] mt  
>  Where MDTName = 'Microsoft.SystemCenter.Notification.Recipients' )a  Cross Apply Recxml.nodes('//Recipient') N(C)) r  
>  Cross Apply xmlquery.nodes('//Device') D(C)  
>  ) Subscribers  
> Join  
>  (--These are the subscriptions  
>  Select r.RuleId, r.RuleModuleId, r.enabled, R.DisplayName,  
>  D.C.value('RecipientId[1]','varchar(4000)') SubscriberId  
>  FROM (Select RuleId, rm.RuleModuleId, enabled, r.DisplayName,  
>    cast(RuleModuleConfiguration as XML) xmlquery  
>  FROM  
>  RuleModule rm  
>  join RuleView r on rm.RuleId = r.Id  
>  where r.Category = 'Notification'  
>  and RuleModuleName = 'CD1'  
>  --and Enabled &lt;&gt; 0  
>  ) r  
>  Cross Apply xmlquery.nodes('//DirectoryReference') D(C)  
>  ) Subscriptions on Subscribers.SubscriberId = Subscriptions.SubscriberId  
> --Where ((Subscriptions.RuleId = Replace(Replace(@SubscriptionId,'{',''),'}','')  
>   --or @SubscriptionId IS NULL)  
> --and DeviceProtocol in ('SMS','SMTP'))  
> order by 1,2*

  
Please let me know if you have any questions or have any issues running the above queries.
