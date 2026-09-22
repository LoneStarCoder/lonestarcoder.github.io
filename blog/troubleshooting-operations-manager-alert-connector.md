---
layout: default
permalink: /blog/troubleshooting-operations-manager-alert-connector
---
[Home](https://lonestarcoder.github.io) | [Blog](https://lonestarcoder.github.io/blog/home) | [Articles](https://lonestarcoder.github.io/articles)
# Troubleshooting Operations Manager Alert Connector
*Author: Brody Kilpatrick* | *Created: November 22, 2010*

In Service Manager, there have been times where the OpsMgr Alert connector would stop forwarding alerts. I would troubleshoot for hours to no end. I tried just about everything short of uninstalling the software. Finally, I found what seems like a tried and true solution (for me).  
  
Operations Manager Alert Connector problems I have had:  
  

- Alerts stop forwarding
- I received a "Task Cannot be completed" error when attempting to delete the connector in SM, or even trying to view the properties.

Here are the steps I took to get alert forwarding working again.

\*\*\*Disclaimer: Microsoft may not support all of these steps, so please test and use at your own risk.

**Delete the SCSM Connector**

- Open SCSM
- Go to the Administration Tab, then go to connectors
- Find the OpsMgr Alert Connector and delete it.

**What if it won't delete?**

- I have been searching for a way to delete it from the database, but I have not been successful. I will update this post when I do.

**Delete the Connector from OpsMgr**

- Follow Kevin Holman's post to delete a connector from OpsMgr.

- <http://blogs.technet.com/b/kevinholman/archive/2009/09/10/removing-an-old-product-connector.aspx>

**Create a new Connector in Service Manager (Pulled Directly from the Admin Guide)**

**\*\*\*Follow steps exactly, and wait for the Connector to show up in Operations Manager**

**|  |
| --- |
| 1.   In the Service Manager console, click Administration.  2.   In the Administration pane, expand Administration, and then click Connectors.  3.   In the Tasks pane, under Connectors, click Create Connector, and then click Operations Manager Alert Connector.  4.   Follow these steps to complete the Operations Manager Alert Connector Wizard:  a.   On the Before You Begin page, click Next.  b.   On the General page, in the Name box, type a name for the new connector. Make sure that the Enable check box is selected, and then click Next. Make note of this name; you will need this name in step 7 of this procedure.  c.   On the Server Details page, in the Server name box, type the name of the server that is hosting the Operations Manager root management server. Under Credentials, click New.  d.   In the Run As Account dialog box, in the Display name box, type a name for this Run As account. In the Account list, select Windows Account.  e.   In the User Name, Password, and Domain fields, type the credentials for the Run As account, and then click OK. For more information about the permissions that are required for this Run As account, see [Accounts Required During Setup](http://go.microsoft.com/fwlink/?LinkId=182907) (http://go.microsoft.com/fwlink/?LinkId=182907) in the System Center Service Manager Planning Guide.  f.    On the Server Details page, click Test Connection. If you receive the following confirmation message, click OK, and then click Next:  The connection to the server was successful.  g.   On the Alert Routing Rules page, click Add.  h.   In the Add Alert Routing Rule dialog box, create a name for the rule, select the template that you want to use to process incidents created by an alert, and then select the alert criteria you want to use. Click OK, and then click Next.  i.    On the Schedule page, select Close alerts in Operations Manager when incidents are resolved or closed or Resolve incidents automatically when the alerts in Operations Manager are closed, click Next, and then click Create.  5.   Start the Operations Manager console, and connect to the Operations Manager root management server.  6.   Use the appropriate method based on the version of Operations Manager 2007 you are using:  ·      In Operations Manager 2007 SP1, in the Administration pane, click Product Connectors.  ·      In Operations Manager 2007 R2, in the Administration pane, click Product Connectors, and then click Internal Connectors.  7.   In the Connectors pane, click the name of the alert connector you specified in step 4b.  8.   In the Actions pane, click Properties.  9.   In the Alert Sync: &lt;name of connector&gt; dialog box, click Add.  10.  In the Product Connector Subscription Wizard dialog box, on the General page, in the Subscription Name box, type the name for this subscription. For example, type All Alerts, and then click Next.  11.  On the Approve groups page, click Next.  12.  On the Approve targets page, click Next.  13.  On the Criteria page, click Create.  14.  In the Alert Sync:&lt;name of connector&gt; dialog box, click OK. |

|  |
| --- |
| **To validate the creation of an Operations Manager 2007 alert connector** |

|  |
| --- |
| ·      Confirm that the connector you created is displayed in the Service Manager console in the Connectors pane.  ·      Confirm that incidents are created in Service Manager from alerts in Operations Manager. |**
