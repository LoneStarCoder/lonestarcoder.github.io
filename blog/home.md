---
layout: default
permalink: /blog/home
---
# Blog Home
## [How to install a simple postfix relay on Red Hat Linux](https://lonestarcoder.github.io/blog/install_simple_postfix_relay_on_redhat)
***October 24, 2025***
> A primer on setting up the simplest possible Postfix relay on Red Hat Linux, including adding a custom mail header to every message that traverses the relay.

## [Create a Simple RAG System with AnythingLLM and LMStudio](https://lonestarcoder.github.io/blog/Create_a_Simple_RAG_System_with_AnythingLLM_and_LMStudio)
***October 9, 2025***
> This beginner-friendly tutorial guides you through building a local Retrieval-Augmented Generation (RAG) system without writing code. You'll learn to set up your own AI assistant that can answer questions based on your documents, running entirely on your own hardware for complete privacy and control.

## [Query Windows Firewall Rules ONLY from GPO](https://lonestarcoder.github.io/blog/query-windows-firewall-rules-only-from-gpo)
***August 18, 2022***
> If a computer has a GPO or a local security policy that defines windows firewall rules, AND there are also local firewall rules, it is difficult to discern between them unless you open each rule. The below PowerShell script will only retrieve rules that were...

## [Make the Windows Defender Firewall Log Useful with PowerShell](https://lonestarcoder.github.io/blog/make-the-windows-defender-firewall-log-useful)
***August 16, 2022***
> A small PowerShell script to parse the windows firewall log into an array.

## [Some Notes on Exchange 2019 and Preparing AD](https://lonestarcoder.github.io/blog/some-notes-on-exchange-2019-and-preparing-ad)
***October 15, 2021***
> Here are some notes from my recent experience preparing AD for Exchange 2019 and the commands I used.

## [WSUS - Update IIS Timeout](https://lonestarcoder.github.io/blog/wsus-update-iis-timeout)
***May 11, 2021***
> **If WSUS keeps timing out, you may need to update the timeout on the IIS Site to something longer.**

## [WSUS - Set SQL Express Max Memory using Powershell](https://lonestarcoder.github.io/blog/wsus-set-sql-express-max-memory-using-powershell)
***March 17, 2021***
> If you are using SQL Express with WSUS, you may want to limit the amount of memory that it uses. Most likely you don't have any SQL tools installed, and there is no reason to install any tools because we can do it with PowerShell. This is just a simple script...

## [Resolve SCOM Alerts from a List](https://lonestarcoder.github.io/blog/resolve-scom-alerts-from-a-list)
***March 8, 2021***
> I created a Powershell command that will resolve SCOM Alerts in a "New" state (State 0) based on a list of alert names that you feed the script from a text file. I will put on Github soon, until there, here it is.

## [ComplianceOutlookLogonToArchiveRpcCtpProbe has failed against servername01 proxying to Unknown for healthmailbox](https://lonestarcoder.github.io/blog/complianceoutlooklogontoarchiverpcctpprobe-has-failed-against-servername01-proxying-to-unknown-for-healthmailbox)
***August 16, 2019***
> A code/script snippet with no accompanying prose - see the post for the full script.

## [PowerShell - Search for Certificate on server in an OU](https://lonestarcoder.github.io/blog/powershell-search-for-certificate-on-server-in-an-ou)
***August 1, 2019***
> My colleague Robert sent me this nice one-liner. It searches for a specific certificate across all servers in a specific OU.

## [DP With Failed Stuff and PowerShell to Redistribute (From B)](https://lonestarcoder.github.io/blog/dp-with-failed-stuff-and-powershell-to-redistribute-from-b)
***July 29, 2019***
> This is from B.

## [PowerShell - Uninstall SCOM Agent Using WMI](https://lonestarcoder.github.io/blog/powershell-uninstall-scom-agent-using-wmi)
***July 12, 2019***
> Sometimes you need to uninstall one or more SCOM agents using PowerShell. This script does NOT require SCOM cmdlets or anything special and it can easily be incorporated into a larger script. It uses WMI to perform a clean uninstall.

## [Move Active Window to Another Screen with Keyboard Hotkeys](https://lonestarcoder.github.io/blog/move-active-window-to-another-screen-with-keyboard-hotkeys)
***June 27, 2019***
> Tired of constantly dragging windows to different monitors with your mouse? You can move the current active window to the left or light with hot keys.

## [Exchange - Quickly Remove Meetings From Room](https://lonestarcoder.github.io/blog/exchange-quickly-remove-meetings-from-room)
***June 25, 2019***
> Photo by rawpixel.com on [Pexels.com](https://www.pexels.com/photo/macbook-pro-turned-on-displaying-schedule-on-table-1893424/)

## [Inspiration](https://lonestarcoder.github.io/blog/inspiration)
***June 14, 2019***
> A code/script snippet with no accompanying prose - see the post for the full script.

## [Exchange PowerShell - Remove an Email from Multiple Mailboxes](https://lonestarcoder.github.io/blog/exchange-powershell-remove-an-email-from-multiple-mailboxes)
***June 11, 2019***
> Quickly remove a specific email from multiple mailboxes.

## [SCSM Script - Set MA to In Progress](https://lonestarcoder.github.io/blog/scsm-script-set-ma-to-in-progress)
***June 11, 2019***
> Sometimes, for whatever reason, an MA never goes into "In Progress" in SCSM and may need a little help. Rather than using SMLETS or logging onto a SCSM machine, if you have the SCSM Console installed on your workstation, you can run it directly from there.

## [Delete Files in Directory](https://lonestarcoder.github.io/blog/delete-files-in-directory)
***June 11, 2019***
> The normal command line is still great to use for deletions, and can be quicker than powershell

## [Skype - Cleanup Account in Database](https://lonestarcoder.github.io/blog/skype-cleanup-account-in-database)
***May 24, 2019***
> Recently, I spent some time troubleshooting a sign-in issue for one of our Skype for Business users. The account was a re-hire account, therefore it had previously existed and then had been deleted from Skype for Business. Once the person was rehired, the...

## [SCSM PowerShell - Reassign All Work Items to Another User](https://lonestarcoder.github.io/blog/scsm-powershell-reassign-all-work-items-to-another-user)
***April 26, 2019***
> This is another quickie. It find all work items assigned to one ad group/user, and then assigns to another.

## [SCSM PowerShell - Populate Blank Affected Users](https://lonestarcoder.github.io/blog/scsm-powershell-populate-blank-affected-users)
***April 18, 2019***
> This script will search for active SRs and IRs with no affected user. It will then take the assigned user and copy it to the affected user. This is just a snippet and not a full production script. You will need to add your error checking and other wrappings.

## [Exchange Powershell - Add Mailbox Permissions](https://lonestarcoder.github.io/blog/exchange-powershell-add-mailbox-permissions)
***April 17, 2019***
> A code/script snippet with no accompanying prose - see the post for the full script.

## [SQL Query - SCCM Breakout OU into Columns](https://lonestarcoder.github.io/blog/sql-query-sccm-breakout-ou-into-columns)
***April 17, 2019***
> Start with the below query and turn it into a view, or simply add it as the sub query. It breaks the OU into rows.

## [SQL Query - SCCM Breakout OU into Rows](https://lonestarcoder.github.io/blog/sql-query-sccm-breakout-ou-into-rows)
***April 17, 2019***
> A code/script snippet with no accompanying prose - see the post for the full script.

## [Exchange Scripts - Live Post](https://lonestarcoder.github.io/blog/exchange-scripts-live-post)
***April 16, 2019***
> This post is updated as new scripts are added, so don't let the original creation date fool you. Some scripts are snippets and others are full, completed scripts. As always, test and use at your own risk.

## [Create Distribution List](https://lonestarcoder.github.io/blog/create-distribution-list)
***March 26, 2019***
> A code/script snippet with no accompanying prose - see the post for the full script.

## [Script to Monitor Service Manager Workflows](https://lonestarcoder.github.io/blog/script-to-monitor-service-manager-workflows)
***December 27, 2018***
> I was asked to create a better way to check for workflow failures in Service Manager. If you use SCOM to monitor SCSM workflows, you know that there is one rule, and it throws an alert for every failure. I didn't like this. Instead, this script will provide a...

## [SCOM Exchange Microsoft.Exchange.15.MailboxStatsSubscription.Rule Failed to store data in the Data Warehouse](https://lonestarcoder.github.io/blog/scom-exchange-microsoft-exchange-15-mailboxstatssubscription-rule-failed-to-store-data-in-the-data-warehouse)
***December 27, 2018***
> I recently posted this to a MS forum:

## [Service Manager - Find subscriptions that are missing Templates](https://lonestarcoder.github.io/blog/service-manager-find-subscriptions-that-are-missing-templates)
***December 27, 2018***
> Recently I performed some Service Manager notification subscription cleanup. Shortly after, I started receiving notifications from the Operations Manager event log stating the following:

## [Get-MoveRequestStatistics staus completed or not completed](https://lonestarcoder.github.io/blog/get-moverequeststatistics-staus-completed-or-not-completed)
***February 23, 2018***
> Get-MoveRequestStatistics -MoveRequestQueue "mydbname" \| ? {$\_.Status.Value -ne 'Completed'}

## [Prepare Server to Install Lync - Quick Way](https://lonestarcoder.github.io/blog/prepare-server-to-install-lync-quick-way)
***January 12, 2017***
> Install-WindowsFeature AS-HTTP-Activation, Desktop-Experience, NET-Framework-45-Features, RPC-over-HTTP-proxy, RSAT-Clustering, Web-Mgmt-Console, WAS-Process-Model, Web-Asp-Net45, Web-Basic-Auth, Web-Client-Auth, Web-Digest-Auth, Web-Dir-Browsing,...

## [Application Approval Workflow (AAW) Removes Superseded Applications from SCSM](https://lonestarcoder.github.io/blog/application-approval-workflow-aaw-removes-superseded-applications-from-scsm)
***November 9, 2016***
> I was recently troubleshooting the following issue: A user requested an application from the SCCM Application Catalog that required approval. However, AAW was not creating a request in SCSM. The application did not exist in SCSM. Everything was working...

## [SCSM - Process Cubes Manually](https://lonestarcoder.github.io/blog/scsm-process-cubes-manually)
***November 2, 2016***
> If you need to process cubes manually directly from the SSAS server, you can use powershell. I pulled this script from [here](http://www.scsm.se/?p=881).

## [SCSM Cube Connection String Note for SQL Native Client](https://lonestarcoder.github.io/blog/scsm-cube-connection-string-note-for-sql-native-client)
***November 2, 2016***
> Recently, my DWASDataBase become corrupt and I have to rebuild the entire database and cubes from scratch. However, when Service Manager attempted to process the cubes, it was throwing 2 different errors, depending on which provider I used in the DWDataMart...

## [SCOM - Adding Alert Context to Emails](https://lonestarcoder.github.io/blog/scom-adding-alert-context-to-emails)
***February 19, 2015***
> I don't see a lot about this out here, so I am posting.

## [SCOM - Query Notification Subscription Data via SQL](https://lonestarcoder.github.io/blog/scom-query-notification-subscription-data-via-sql)
***February 5, 2015***
> I needed to provide data about SCOM notification subscriptions via SQL Reporting Services. I was hoping to find a simple answer, so I went to searching online. It seemed that no one had posted anything related to querying notifications from the...

## [Tip: Approve all In Progress Activities in Service Manager](https://lonestarcoder.github.io/blog/tip-approve-all-in-progress-activities-in-service-manager)
***September 3, 2014***
> Stop manually approving each test review activity. Service Manager implementations usually include immense amounts of testing. If you are testing Service Requests or Change Requests, you probably have tons of review activities to approve. It can be time...

## [Orchestrator - Misrepresented and Misunderstood](https://lonestarcoder.github.io/blog/orchestrator-misrepresented-and-misunderstood)
***August 1, 2014***
> 1. Talk about the misrepresentation of Orchestrator.

## [Query ALL Service Manager ENUMS and their Hierarchy](https://lonestarcoder.github.io/blog/query-all-service-manager-enums-and-their-hierarchy)
***April 22, 2014***
> I find myself listing out all of the enumerations for lists in Service Manager quite a bit. Rather than spending time doing this over and over, I wrote a query that retrieves all of the enumeration items from Service Manager. I tried to keep it simple so...

## [Get Parent Affected User for Notifications](https://lonestarcoder.github.io/blog/get-parent-affected-user-for-notifications)
***February 13, 2014***
> This is more of a note for myself. For an activity, this will get the affected user of the parent work item.

## [SCOM System Center Management Service is now Microsoft Monitoring Agent](https://lonestarcoder.github.io/blog/scom-system-center-management-service-is-now-microsoft-monitoring-agent)
***November 13, 2013***
> Microsoft Monitoring Agent is a new agent that replaces the Operations Manager Agent and combines .NET Application Performance Monitoring (APM) in System Center with the full functionality of IntelliTrace Collector in the Microsoft Visual Studio development...

## [SCSM 2012: Self Service Portal Service category color customization](https://lonestarcoder.github.io/blog/scsm-2012-self-service-portal-service-category-color-customization)
***September 3, 2013***
> The is a great solution and worthy of a repost.

## [Isolating Powershell Sessions in Workflows in Service Manager](https://lonestarcoder.github.io/blog/isolating-powershell-sessions-in-workflows-in-service-manager)
***July 23, 2013***
> **Problem:** One common issue I have ran into with writing workflows for Service Manager is the powershell sessions seem to be shared. When I call a set of cmdlets, such as the Active Directory cmdlets, they do not always load/unload properly, causing issues...

## [SCOM 2012 Upgrade ACS Schema does not Update](https://lonestarcoder.github.io/blog/scom-2012-upgrade-acs-schema-does-not-update)
***July 19, 2013***
> I didn't write the blog post, but I did experience the issue, and it is worth repeating. It seems this is going to affect anyone upgrading from SCOM 2012 to SP1 who uses ACS. The issue is that the schema doesn't get updated, causing crashes to ADTServer. If...

## [Create and Assign Service Manager Incidents Directly from SCOM on Demand](https://lonestarcoder.github.io/blog/create-and-assign-service-manager-incidents-directly-from-scom-on-demand)
***May 3, 2013***
> **Update:** **As you read through below, you will notice that Microsoft has been *nice* enough to use some of the IDs I was using with the release of SP1. (This post was pre SP1) You will have to make small modifications to your script and Ids but the below...

## [SCOM Availability Report Monitoring Unavailable SOLVED (Unsupported)](https://lonestarcoder.github.io/blog/scom-availability-report-monitoring-unavailable-solved-unsupported)
***February 27, 2013***
> I have seen numerous posts floating around regarding the SCOM Availability Reports showing "Monitoring Unavailable" even though the objects were healthy for the time period. For example, I can run the SCOM Availability Report, select "Exchange 2007 Service”,...

## [Enter a Group into Maintenance Mode using SCOM the Console (No Scripts Required)](https://lonestarcoder.github.io/blog/enter-a-group-into-maintenance-mode-using-scom-the-console-no-scripts-required)
***February 22, 2013***
> *I know this is a short post, but the idea is simplicity. Sometimes, we make things unnecessarily too complicated, and native functionality is sometimes overlooked. As a consultant, I strive to keep solutions for my customers as simple as possible while...

## [Create Quest Event-o-Pedia Online Event Search View in SCOM](https://lonestarcoder.github.io/blog/create-quest-event-o-pedia-online-event-search-view-in-scom)
***January 17, 2013***
> Quest software has a nice online website that allows easy searching or browsing of events. While it contains almost all windows events, it also contains events from VMWare and Juniper. I thought this would be a nice little utility to have in a SCOM view, so I...

## [Notes Regarding SCSM 2012 Upgrade](https://lonestarcoder.github.io/blog/notes-regarding-scsm-2012-upgrade)
***January 15, 2013***
> I just wanted to share some notes regarding the Service Manager 2012 SP1 Upgrade, that might not be obvious unless you thoroughly read the documentation. I hope these notes help prevent some problems.

## [SCOM Agent Supported in SCSM 2012 SP1](https://lonestarcoder.github.io/blog/scom-agent-supported-in-scsm-2012-sp1)
***January 15, 2013***
> System Center 2012 – Operations Manager

## [Removing a SCOM Management Group Remotely](https://lonestarcoder.github.io/blog/removing-a-scom-management-group-remotely)
***January 8, 2013***
> A code/script snippet with no accompanying prose - see the post for the full script.

## [All Management Servers Resource Pool Unavailable Hack/Fix](https://lonestarcoder.github.io/blog/all-management-servers-resource-pool-unavailable-hack-fix)
***December 31, 2012***
> I have seen a lot of posts regarding the "All Management Servers Resource Pool Unavailable" error since SCOM 2012 RC, and I am still seeing numerous posts. I have also not seen anything in SP1 regarding fixing this error. The "All Management Servers Resource...

## [SCOM 2012 Certificates - 16 Hours of Pain](https://lonestarcoder.github.io/blog/scom-2012-certificates-16-hours-of-pain)
***December 18, 2012***
> I have an answer coming soon :)

## [Monitoring Service Manager 2012 with Operations Manager 2012](https://lonestarcoder.github.io/blog/monitoring-service-manager-2012-with-operations-manager-2012)
***November 16, 2012***
> Note that if you are using System Center 2012: Service Manager, you will not be able to install Operations Manager 2012 agents on the machines. You must use agentless management; this is fine because the Service Manager 2012 Management Pack accounts for...

## [SCSM Cube Processing and Analysis Services is a Beast](https://lonestarcoder.github.io/blog/scsm-cube-processing-and-analysis-services-is-a-beast)
***July 11, 2012***
> If you are using the Service Manager DW and cubes, you may have ran into some issues with the cubes not processing, failing, data issue, or something else. I have provided a couple of resources to help with troubleshooting at the bottom of my post, but I want...

## [Exchange Connector 3.0 RC Released! - System Center: Service Manager Engineering Team Blog - Site Home - TechNet Blogs](https://lonestarcoder.github.io/blog/exchange-connector-3-0-rc-released-system-center-service-manager-engineering-tea)
***April 20, 2012***
> [Exchange Connector 3.0 RC Released! - System Center: Service Manager Engineering Team Blog - Site Home - TechNet Blogs](http://blogs.technet.com/b/servicemanager/archive/2012/04/15/exchange-connector-3-0-rc-released.aspx)

## [SCSM Service Request - Don't use a Single Review Activity](https://lonestarcoder.github.io/blog/scsm-service-request-don-t-use-a-single-review-activity)
***March 27, 2012***
> In Service Manager, we are allowed to have activities in service requests, just like Changes and Incidents. However, don't enter a single activity which is also a review activity. It will not move to "In Progress."

## [SCSM 2012 - Affected User Populates Affected Item Selection](https://lonestarcoder.github.io/blog/scsm-2012-affected-user-populates-affected-item-selection)
***February 14, 2012***
> I don't know if many people have noticed this, because I sure didn't. Maybe I am not the most observant person. On the incident form, you see "Affected user CIs:". I have always seen it, but nothing ever "clicked." When you enter an affected user, it gives...

## [SCSM Find the Relationship GUID between two Work Items Components](https://lonestarcoder.github.io/blog/scsm-find-the-relationship-guid-between-two-work-items-components)
***February 2, 2012***
> I was recently editing a notification mangement pack and needed to find the GUID for a relationship between two instance so I could send an email to the correct user. IN SCSM 2012 there is a cmdlet that can be used to find the name if a relationship called...

## [Notifications not working in SCOM 2012](https://lonestarcoder.github.io/blog/notifications-not-working-in-scom-2012)
***January 27, 2012***
> [Notifications not working in SCOM 2012](http://social.technet.microsoft.com/Forums/en-US/operationsmanagergeneral/thread/6d881470-f920-4fe2-b0ca-5a0ba7ddfcb2): "Notifications not working in SCOM 2012"

## [Operations Manager 2012 RC Console Memory Leak](https://lonestarcoder.github.io/blog/operations-manager-2012-rc-console-memory-leak)
***January 27, 2012***
> As you may know by now, the Operations Manager 2012 console has a memory leak. Microsoft has acknowledged the issue, and put it into the release notes for RC. It isn't that big of a deal. Just close the console and reopen it, if it begins taking too much...

## [My First Look at Network Monitoring in a Real Environment](https://lonestarcoder.github.io/blog/my-first-look-at-network-monitoring-in-a-real-environment)
***January 17, 2012***
> I recently deployed System Center 2012: Operations Manager in a development environment. I don't mean a small virtual lab. I mean a real environment with multiple devices that can be monitored via SNMP.

## [SCSM Workflow Date Time Token Error in the Authoring Console](https://lonestarcoder.github.io/blog/scsm-workflow-date-time-token-error-in-the-authoring-console)
***October 25, 2011***
> The Authoring Tool workflow has an option of "relative" when using date fields. However, when you check the checkbox and enter a relative token such as [now] the authoring console doesn't like it and will not save. I haven't seen anyone else complain about...

## [SCSM Tokens](https://lonestarcoder.github.io/blog/scsm-tokens)
***October 25, 2011***
> [me] [mygroups]

## [.NET 4 BEFORE IIS](https://lonestarcoder.github.io/blog/net-4-before-iis)
***October 24, 2011***
> When installing anything in Operations Manager 2012, .NET 4 must be installed before IIS. Otherwise, certain steps will have to be taken to resolve the issue.

## [Another Gotcha - No Windows XP Support for Consoles](https://lonestarcoder.github.io/blog/another-gotcha-no-windows-xp-support-for-consoles)
***October 24, 2011***
> Many organizations still use Windows XP. Windows XP is not supported for the console. It must be vista or higher.

## [Pay Close Attention to the Minimum Software Requirements!!!](https://lonestarcoder.github.io/blog/pay-close-attention-to-the-minimum-software-requirements)
***October 24, 2011***
> Minimum Software Requirements

## [Welcome to the new Operations Manager 2012 Blog](https://lonestarcoder.github.io/blog/welcome-to-the-new-operations-manager-2012-blog)
***October 24, 2011***
> Hi, my name is Brody Kilpatrick and I am the owner of this blog. It is dedicated to anything Operations Manager 2012. I created the blog to help others and remind myself how I did something. Some of my posts may be from other blogs. If so, I will link to...

## [Suppressing Duplicate SCSM Workflow Monitoring Alerts/Health State Changes in SCOM - SCSM Engineering Team Blog - Site Home - TechNet Blogs](https://lonestarcoder.github.io/blog/suppressing-duplicate-scsm-workflow-monitoring-alerts-health-state-changes-in-sc)
***October 10, 2011***
> [Suppressing Duplicate SCSM Workflow Monitoring Alerts/Health State Changes in SCOM - SCSM Engineering Team Blog - Site Home - TechNet...

## [Action Log, History, and Auditing in Service Manager - SCSM Engineering Team Blog - Site Home - TechNet Blogs](https://lonestarcoder.github.io/blog/action-log-history-and-auditing-in-service-manager-scsm-engineering-team-blog-si)
***October 10, 2011***
> [Action Log, History, and Auditing in Service Manager - SCSM Engineering Team Blog - Site Home - TechNet Blogs](http://blogs.technet.com/b/servicemanager/archive/2010/02/04/action-log-history-and-auditing-in-service-manager.aspx):

## [Microsoft System Center Suite: List current values for a list enumeration in Service Manager](https://lonestarcoder.github.io/blog/microsoft-system-center-suite-list-current-values-for-a-list-enumeration-in-serv)
***October 10, 2011***
> [Microsoft System Center Suite: List current values for a list enumeration in Service Manager](http://systemscentre.blogspot.com/2010/09/list-current-values-for-list.html):

## [Quickly find the SCSM Management Group Name](https://lonestarcoder.github.io/blog/quickly-find-the-scsm-management-group-name)
***September 12, 2011***
> I always assumed the SCSM management group name was in the title of the console until I actually needed one day, then I realized it wasn't there. It seems like you would see the Management Group name every where, but it is not. Use the registry on a SCSM...

## [Here are some useful SCSM DataMart Views](https://lonestarcoder.github.io/blog/here-are-some-useful-scsm-datamart-views)
***August 29, 2011***
> **Create Announcements view from The CMDB**

## [Manual enable of Registered Servers - The IT-Toolbox](https://lonestarcoder.github.io/blog/manual-enable-of-registered-servers-the-it-toolbox)
***August 16, 2011***
> [Manual enable of Registered Servers - The IT-Toolbox](http://itbloggen.se/cs/blogs/alexanderaxberg/archive/2011/01/16/manual-enable-of-registered-servers.aspx)

## [Notifying Analysts when Action Log was updated \| SCSMfaq.ch](https://lonestarcoder.github.io/blog/notifying-analysts-when-action-log-was-updated-scsmfaq-ch)
***August 6, 2011***
> [Notifying Analysts when Action Log was updated \| SCSMfaq.ch](http://blog.scsmfaq.ch/2011/08/06/notifying-analysts-when-action-log-was-updated/#more-1067)

## [Backing up Management packs when they're modified](https://lonestarcoder.github.io/blog/backing-up-management-packs-when-they-re-modified)
***August 6, 2011***
> *(Courtesy of my Co-worker and friend Thomas Bianco)*

## [How to delete items from the Incident Classification LIST?](https://lonestarcoder.github.io/blog/how-to-delete-items-from-the-incident-classification-list)
***July 25, 2011***
> [How to delete items from the Incident Classification LIST?](http://social.technet.microsoft.com/Forums/en/systemcenterservicemanager/thread/024d4824-8708-4413-ad06-0326eb2d60a2)

## [Extending Incident properties and forms - SCSM Engineering Team Blog - Site Home - TechNet Blogs](https://lonestarcoder.github.io/blog/extending-incident-properties-and-forms-scsm-engineering-team-blog-site-home-tec)
***July 25, 2011***
> [Extending Incident properties and forms - SCSM Engineering Team Blog - Site Home - TechNet Blogs](http://blogs.technet.com/b/servicemanager/archive/2011/04/26/extending-incident-properties-and-forms.aspx)

## [Four SCSM Self-Service Portal Solutions (certificate, redirect, alias)](https://lonestarcoder.github.io/blog/four-scsm-self-service-portal-solutions-certificate-redirect-alias)
***July 20, 2011***
> When accessing the Self-Service Portal, typing https://servername/enduser into the browser address isn't acceptable for most organizations. This blog posts addresses 4 concerns of the self-service portal and how to resolve them. The information may be common...

## [Showing the Correct Display Name for Related Objects in View Columns - SCSM Engineering Team Blog - Site Home - TechNet Blogs](https://lonestarcoder.github.io/blog/showing-the-correct-display-name-for-related-objects-in-view-columns-scsm-engine)
***July 18, 2011***
> [Showing the Correct Display Name for Related Objects in View Columns - SCSM Engineering Team Blog - Site Home - TechNet...

## [Changing SCSM Portal Address](https://lonestarcoder.github.io/blog/changing-scsm-portal-address)
***July 11, 2011***
> Great article on changing the SCSM portal Address <http://memoexp.wordpress.com/2011/03/30/changing-scsm-portal-address/>

## [Disabling Workflows with Overrides](https://lonestarcoder.github.io/blog/disabling-workflows-with-overrides)
***July 8, 2011***
> <http://blogs.technet.com/b/servicemanager/archive/2011/07/07/disabling-workflows-with-overrides.aspx>

## [How To Remove Default Impact and Urgency Options](https://lonestarcoder.github.io/blog/how-to-remove-default-impact-and-urgency-options)
***July 8, 2011***
> <http://social.technet.microsoft.com/Forums/en-US/systemcenterservicemanager/thread/74ba3991-2d97-4895-9994-af01e41c7d30>

## [Data warehouse failure concerns answered](https://lonestarcoder.github.io/blog/data-warehouse-failure-concerns-answered)
***June 28, 2011***
> I have been having some concerns for the lack of failover options and documentation for the Service Manager data warehouse. I created a forum post which can be seen at...

## [Attaching Email Screenshots with Service Manager](https://lonestarcoder.github.io/blog/attaching-email-screenshots-with-service-manager)
***June 27, 2011***
> I am not sure if many people realize this until SCSM is actually installed. But the SCSM out-of-the-box email solution will automatically attach pictures in the body of an email to an incident. I have had people ask me in the past if this is possible, and I...

## [SCSM SP1 CU2 Installation Instructions](https://lonestarcoder.github.io/blog/scsm-sp1-cu2-installation-instructions)
***June 14, 2011***
> <http://support.microsoft.com/kb/2542118>

## [Microsoft Authorization Manager Hotfix is not Need for Server 2008 R2 with SP1](https://lonestarcoder.github.io/blog/microsoft-authorization-manager-hotfix-is-not-need-for-server-2008-r2-with-sp1)
***June 13, 2011***
> I came across a problem attempting to install the SCSM prerequisite "Authorization Manager Hotfix" when installing SCSM on Server 2008 R2 with SP1. It said that it is not applicable, but it still shows a warning. IT IS NOT NEEDED. What is my source? Well,...

## [Service Manager role based security scoping](https://lonestarcoder.github.io/blog/service-manager-role-based-security-scoping)
***June 12, 2011***
> The below post isn't real new, but it is highly informational. A lot of questions tend to arise around security scoping, and I found a great post, which I wanted to duplicate. The below text was taken from...

## [This is so freaking awesome...Save the last set of parameters used in an SSRS report](https://lonestarcoder.github.io/blog/this-is-so-freaking-awesome-save-the-last-set-of-parameters-used-in-an-ssrs-repo)
***May 27, 2011***
> I just created the coolest thing at the request of a client. To make things a little easier for some end users, they wanted to be able to save the last set of parameters a user enters. This way, when they open the report, it always contains the most recent...

## [WebFront for Service Manager « Gridpro](https://lonestarcoder.github.io/blog/webfront-for-service-manager-gridpro)
***May 26, 2011***
> gridpro has created a web front end for Service Manager. It is set to be released May 2011. Check it out.

## [Copy Templates in Service Manager :: Litware](https://lonestarcoder.github.io/blog/copy-templates-in-service-manager-litware)
***May 26, 2011***
> [Copy Templates in Service Manager :: Litware](http://blogs.litware.se/?p=916)

## [BOOK: System Center Service Manager 2010: Unleashed](https://lonestarcoder.github.io/blog/book-system-center-service-manager-2010-unleashed)
***May 25, 2011***
> System Center Service Manager 2010 is available for pre-order on Amazon. Get it now!

## [SCSM 2010 SP1 Cumulative Update 2 (CU2) Now Available! - SCSM Engineering Team Blog - Site Home - TechNet Blogs](https://lonestarcoder.github.io/blog/scsm-2010-sp1-cumulative-update-2-cu2-now-available-scsm-engineering-team-blog-s)
***May 20, 2011***
> [SCSM 2010 SP1 Cumulative Update 2 (CU2) Now Available! - SCSM Engineering Team Blog - Site Home - TechNet Blogs](http://blogs.technet.com/b/servicemanager/archive/2011/05/20/scsm-2010-sp1-cumulative-update-2-cu2-now-available.aspx)

## [Service Manager Data Warehouse schema now available - SCSM Engineering Team Blog - Site Home - TechNet Blogs](https://lonestarcoder.github.io/blog/service-manager-data-warehouse-schema-now-available-scsm-engineering-team-blog-s)
***May 20, 2011***
> [Service Manager Data Warehouse schema now available - SCSM Engineering Team Blog - Site Home - TechNet Blogs](http://blogs.technet.com/b/servicemanager/archive/2011/03/14/service-manager-data-warehouse-schema-now-available.aspx)

## [Move the database in Service Manager 2010](https://lonestarcoder.github.io/blog/move-the-database-in-service-manager-2010)
***May 7, 2011***
> 2010-10-16 16:30:00Stefan Allansson

## [vbscript - call web service](https://lonestarcoder.github.io/blog/vbscript-call-web-service)
***April 11, 2011***
> <http://social.msdn.microsoft.com/forums/en-US/asmxandxml/thread/4f00c08e-0188-48ac-9bd8-78607c8fbfb9/>

## [Extracting stored procedure content via SQL](https://lonestarcoder.github.io/blog/extracting-stored-procedure-content-via-sql)
***March 30, 2011***
> Posted [Wednesday, October 18, 2006 9:46 PM](http://weblogs.asp.net/cumpsd/archive/2006/10/18/681812.aspx) by [CumpsD](http://weblogs.asp.net/members/CumpsD.aspx)

## [Service Manager Database Tour & Useful Queries](https://lonestarcoder.github.io/blog/service-manager-database-tour-useful-queries)
***March 18, 2011***
> <http://blogs.technet.com/b/servicemanager/archive/2009/12/31/service-manager-database-tour-useful-queries.aspx#3414058>

## [How to change the listening port for Remote Desktop](https://lonestarcoder.github.io/blog/how-to-change-the-listening-port-for-remote-desktop)
***March 18, 2011***
> <http://support.microsoft.com/kb/306759>

## [Great Article on How to Install Windows Deployment Services](https://lonestarcoder.github.io/blog/great-article-on-how-to-install-windows-deployment-services)
***March 17, 2011***
> <http://www.windows-noob.com/forums/index.php?/topic/446-how-can-i-setup-wds-in-windows-server-2008/>

## [Warming Up SSRS](https://lonestarcoder.github.io/blog/warming-up-ssrs)
***March 17, 2011***
> <http://www.sqlservercentral.com/Forums/Topic568045-149-1.aspx#bm568665>

## [How to Obtain a Certificate Using Windows Server 2008 Enterprise CA in Operations Manager 2007](https://lonestarcoder.github.io/blog/how-to-obtain-a-certificate-using-windows-server-2008-enterprise-ca-in-operation)
***March 14, 2011***
> <http://technet.microsoft.com/en-us/library/dd362553.aspx>

## [Enterprise CA: How to create a SCOM Certificate template.](https://lonestarcoder.github.io/blog/enterprise-ca-how-to-create-a-scom-certificate-template)
***March 12, 2011***
> FROM: <http://thoughtsonopsmgr.blogspot.com/2010/04/enterprise-ca-how-to-create-scom.html>

## [Inserting links to Review/Manual Activities in notifications.](https://lonestarcoder.github.io/blog/inserting-links-to-review-manual-activities-in-notifications)
***March 1, 2011***
> <http://blogs.technet.com/b/servicemanager/archive/2010/01/28/inserting-links-to-review-manual-activities-in-notifications.aspx#3391004>

## [SCCM Configuration Pack Search link on Microsoft Pinpoint](https://lonestarcoder.github.io/blog/sccm-configuration-pack-search-link-on-microsoft-pinpoint)
***February 15, 2011***
> <http://pinpoint.microsoft.com/en-US/applications/search?q=%22configuration%20pack%22>

## [Strange issues with the SCSM Mangement Server](https://lonestarcoder.github.io/blog/strange-issues-with-the-scsm-mangement-server)
***February 13, 2011***
> I sometimes find strange issues with the SCSM management server. For example, my connectors have not synchronized in several days. Performing a manual synchronization does nothing. When I look in the event log, there are no errors, but there is a sync...

## [Create Recurring Change Requests](https://lonestarcoder.github.io/blog/create-recurring-change-requests)
***February 11, 2011***
> <http://www.scsm.se/?p=239>

## [All work Items assigned to me](https://lonestarcoder.github.io/blog/all-work-items-assigned-to-me)
***February 10, 2011***
> <http://blogs.technet.com/b/servicemanager/archive/2010/09/28/all-work-items-assigned-to-me-view.aspx#3386613>

## [Export all unsealed MPs](https://lonestarcoder.github.io/blog/export-all-unsealed-mps)
***February 8, 2011***
> <http://www.scsm.se/?p=227>

## [SharePoint Surveys with SCSM](https://lonestarcoder.github.io/blog/sharepoint-surveys-with-scsm)
***February 6, 2011***
> http://blogs.technet.com/b/servicemanager/archive/2009/12/08/incident-resolution-satisfaction-surveys-on-sharepoint.aspx

## [SCSM Portal is Slow - I got a suggestion from Travis Wright](https://lonestarcoder.github.io/blog/scsm-portal-is-slow-i-got-a-suggestion-from-travis-wright)
***February 6, 2011***
> I got some more information from one of our devs today. Try this:

## [SLA Management](https://lonestarcoder.github.io/blog/sla-management)
***February 2, 2011***
> <http://blogs.litware.se/?p=749>

## [http://blogs.technet.com/b/servicemanager/archive/2009/12/08/incident-resolution-satisfaction-surveys-on-sharepoint.aspx](https://lonestarcoder.github.io/blog/http-blogs-technet-com-b-servicemanager-archive-2009-12-08-incident-resolution-s)
***February 2, 2011***
> <http://blogs.technet.com/b/servicemanager/archive/2009/12/08/incident-resolution-satisfaction-surveys-on-sharepoint.aspx>

## [A few SCSM Customizatin links](https://lonestarcoder.github.io/blog/a-few-scsm-customizatin-links)
***February 2, 2011***
> [Incident Resolution Satisfaction Surveys on SharePoint](http://blogs.technet.com/servicemanager/archive/2009/12/08/incident-resolution-satisfaction-surveys-on-sharepoint.aspx) [Automatically Sending Notifications to...

## [Enabling Users to Take Action from Email Using Web Pages/Web Services](https://lonestarcoder.github.io/blog/enabling-users-to-take-action-from-email-using-web-pages-web-services)
***January 31, 2011***
> <http://blogs.technet.com/b/servicemanager/archive/2011/01/25/enabling-users-to-take-action-from-email-using-web-pages-web-services.aspx#3383712>

## [Excel Hotkeys](https://lonestarcoder.github.io/blog/excel-hotkeys)
***January 20, 2011***
> <http://www.mvps.org/dmcritchie/excel/shortx2k.htm>

## [Approve SCOM Agents with Powershell](https://lonestarcoder.github.io/blog/approve-scom-agents-with-powershell)
***January 20, 2011***
> get-agentpendingaction \| where-object {$\_.AgentName -eq "server1.that.I.amtryingtomanage.com"} \| approve-agentpendingaction

## [Using the ACE Driver with 64 bit](https://lonestarcoder.github.io/blog/using-the-ace-driver-with-64-bit)
***January 14, 2011***
> <http://www.microsoft.com/downloads/en/details.aspx?FamilyID=c06b8369-60dd-4b64-a44b-84b371ede16d&displaylang=en> <http://social.msdn.microsoft.com/Forums/en/sqldataaccess/thread/4887d91f-6ac7-40c0-9fc8-5cdd0634e603>

## [How to install SCDPM](https://lonestarcoder.github.io/blog/how-to-install-scdpm)
***January 8, 2011***
> <http://capitalhead.com/articles/installing-system-center-data-protection-manager-(scdpm)-2007-on-windows-server-2008-step-by-step-guide.aspx>

## [Change WriterLoginName](https://lonestarcoder.github.io/blog/change-writerloginname)
***January 7, 2011***
> Have you changed Data Warehouse Action Account just before these events showed up? If so, you also need to change the 'WriterLoginName' field in the dbo.ManagementGroup table in the DW to match your new DW Action Account...

## [Bulk User Creation from Powershell](https://lonestarcoder.github.io/blog/bulk-user-creation-from-powershell)
***January 2, 2011***
> **Create a CSV with comma delimited information as below CN,SN,GivenName,Name,Title,Description,PostalCode,TelephoneNumber,Department,Company,StreetAddress,Countrycode** **Use this script and modify the obvious stuff**...

## [How to change SID on Windows 7 and Windows Server 2008 R2 using sysprep](https://lonestarcoder.github.io/blog/how-to-change-sid-on-windows-7-and-windows-server-2008-r2-using-sysprep)
***January 2, 2011***
> <http://www.brajkovic.info/windows-server-2008/windows-server-2008-r2/how-to-change-sid-on-windows-7-and-windows-server-2008-r2-using-sysprep/>

## [Configuring Guest Internet Access](https://lonestarcoder.github.io/blog/configuring-guest-internet-access)
***January 2, 2011***
> To configure internet access: Create a new external network in Hyper v and bind it with a physical NIC that has internet access. On the Host, go to network connections. Then bridge the new network with the physical adapter After bridging, check to see if the...

## [Configuring the internal network](https://lonestarcoder.github.io/blog/configuring-the-internal-network)
***January 2, 2011***
> - Add the internal network connection to the VM - This Network will allow communication between he host and VM - Go to the Hyper v HOST - Network Connections - Find the connection with the same name that you gave the internal network you just created -...

## [Starting the new Hyper-v Lab](https://lonestarcoder.github.io/blog/starting-the-new-hyper-v-lab)
***January 2, 2011***
> A code/script snippet with no accompanying prose - see the post for the full script.

## [Fill Incidents using only the Keyboard](https://lonestarcoder.github.io/blog/fill-incidents-using-only-the-keyboard)
***December 30, 2010***
> A code/script snippet with no accompanying prose - see the post for the full script.

## [Service Manager Performance](https://lonestarcoder.github.io/blog/service-manager-performance)
***December 29, 2010***
> Excerpt from the MS website: <http://technet.microsoft.com/en-us/library/ff461124.aspx> Updated: December 1, 2010 Applies To: System Center Service Manager 2010

## [Workflow Performance - Management Packs on the media to help Performance](https://lonestarcoder.github.io/blog/workflow-performance-management-packs-on-the-media-to-help-performance)
***December 29, 2010***
> Workflows are automatic processes that occur and include sending e-mail notifications, the next step of a change request activating, and automatically applying a template.

## [Additional Cmdlets from CodePlex - Very Useful](https://lonestarcoder.github.io/blog/additional-cmdlets-from-codeplex-very-useful)
***December 15, 2010***
> <http://smlets.codeplex.com/>

## [List of the Service Manager Cmdlets](https://lonestarcoder.github.io/blog/list-of-the-service-manager-cmdlets)
***December 15, 2010***
> <http://technet.microsoft.com/en-us/library/ff460963.aspx>

## [E-mail user from Service Manager Console - SCSM Engineering Team Blog - Site Home - TechNet Blogs](https://lonestarcoder.github.io/blog/e-mail-user-from-service-manager-console-scsm-engineering-team-blog-site-home-te)
***December 6, 2010***
> Very Useful Time saver for Analysts

## [When trying the "Request Software" link on the end user portal, you get the message "Unable to load portal ActiveX Control.](https://lonestarcoder.github.io/blog/when-trying-the-request-software-link-on-the-end-user-portal-you-get-the-message)
***December 6, 2010***
> **Problem:** When trying the "Request Software" link on the end user portal, you get the message "Unable to load portal ActiveX Control."

## [SCSM Command Shell](https://lonestarcoder.github.io/blog/scsm-command-shell)
***November 29, 2010***
> Unlike Operations Manager and Exchange 2010, the Service Manager command shell is not simply a shortcut. However, you can add the Service Manager command shell to PowerShell. The below is a clip taken from a [TechNet...

## [Incident Resolution Satisfaction Surveys on SharePoint - SCSM Engineering Team Blog - Site Home - TechNet Blogs](https://lonestarcoder.github.io/blog/incident-resolution-satisfaction-surveys-on-sharepoint-scsm-engineering-team-blo)
***November 29, 2010***
> Many people have requested incident surveys. Here is a potential solution.

## [Authoring Console and Management Pack References after CU3](https://lonestarcoder.github.io/blog/authoring-console-and-management-pack-references-after-cu3)
***November 24, 2010***
> **If** you have updated to Service Manager CU3, you will notice warnings in the event logs that show management pack mismatches between. \*.0 and \*.216. This is a bug, and it will be fixed in the next release. However, if you are working in the Authoring...

## [Troubleshooting Operations Manager Alert Connector](https://lonestarcoder.github.io/blog/troubleshooting-operations-manager-alert-connector)
***November 22, 2010***
> In Service Manager, there have been times where the OpsMgr Alert connector would stop forwarding alerts. I would troubleshoot for hours to no end. I tried just about everything short of uninstalling the software. Finally, I found what seems like a tried and...

## [New System Center Service Manager Blog](https://lonestarcoder.github.io/blog/new-system-center-service-manager-blog)
***November 22, 2010***
> I have recently been required to learn, demo and deploy Service Manager to various customers. I have created this blog to help you and me in our journey with System Center Service Manager. Feel free to comment on posts, as I will probably need help along the...
