---
layout: default
permalink: /blog/scom-exchange-microsoft-exchange-15-mailboxstatssubscription-rule-failed-to-store-data-in-the-data-warehouse
---
# SCOM Exchange Microsoft.Exchange.15.MailboxStatsSubscription.Rule Failed to store data in the Data Warehouse
*Author: Brody Kilpatrick* | *Created: December 27, 2018*

|  |  |
| --- | --- |
| Product | Version |
| System Center 2012 R2: Operations Manager | Update Rollup 9 |
| Exchange Server 2013 SP1 | Cumulative Update 12 |

---

I recently posted this to a MS forum:

> I am getting the below error, only for exchange in SCOM. Failed to store data in the Data Warehouse. The operation will be retried. Exception 'InvalidOperationException': The given value of type String from the data source cannot be converted to type nvarchar of the specified target column.
>
> One or more workflows were affected by this.
>
> Workflow name: Microsoft.Exchange.15.MailboxStatsSubscription.Rule

I thought that there were be a "real" fix for this by now. Unfortunately, MS has not provided a fix, so until then, here is a workaround.

So what's the problem?

> SCOM cannot insert data into a column in the SQL Server Table named "[Exchange2013].[MailboxStatsStaging]".

The column (in my case) SCOM is having an issue with is "[Mailbox\_EmailAddresses]". The column has an nvarchar size resriction of "1024" characters. Why would an email address in exchange be more than 1024 characters? Well, as it turns out, we had several mailboxes that had more than 1024 characters in this column. You can check all of you mailboxes by running:

```
get-mailbox | where-object { $_.EmailAddresses.ToCharArray().Length -ge 1024 } | foreach-object {write-host "$_" $_.EmailAddresses.ToCharArray().Length}
```

If you find some that are over 1024, it might be easiest to update the mailbox and attempt to remove some information to reduce the size. However, this is not always feasible, especially when you have many that meet this criteria. The option that I chose? Increase the column size. Is this supported? Probably not, I didn't ask. The maximum size you can specify for the nvarchar column is 4000. So first check to see if you have any mailboxes where the "emailaddresses" column is longer than 4000 characters. If you do, this will not work, and you will need to reduce it's size. Otherwise, you can go into SQL Management Studio, right click the table "Exchange2013].[MailboxStatsStaging]" and select Script table as, Drop and Create to, New query editor window. Go find the email addresses column and change the size the "4000". Execute the query. Give it a few minutes, and boom the table is populated and the errors have gone away.

#### Thanks To:

I could not have figured this out without the help of Stefan Wuchenauer, Ken Knicke and Dan Rawlings in the forum post [here](https://social.technet.microsoft.com/Forums/en-US/48a39b82-1163-43d8-bfdb-b98470f0098a/scom-exchange-microsoftexchange15mailboxstatssubscriptionrule-failed-to-store-data-in-the-data?forum=operationsmanagergeneral).
