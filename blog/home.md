---
layout: default
permalink: /blog/home
---
[Home](https://lonestarcoder.github.io) | [Blog](https://lonestarcoder.github.io/blog/home) | [Articles](https://lonestarcoder.github.io/articles)
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
