---
layout: default
permalink: /blog/some-notes-on-exchange-2019-and-preparing-ad
---
[Home](https://lonestarcoder.github.io) | [Blog](https://lonestarcoder.github.io/blog/home) | [Articles](https://lonestarcoder.github.io/articles)
# Some Notes on Exchange 2019 and Preparing AD
*Author: Brody Kilpatrick* | *Created: October 15, 2021*

Here are some notes from my recent experience preparing AD for Exchange 2019 and the commands I used.

First of all, I did not find any blogs that had fully correct step-by-step instructions on schema extensions and preparing AD. They were all missing something, and I had to refer back to Microsoft's documentation, which was not clear and is (in my opinion) incomplete.

Secondly, these are notes - not instructions or a how-to. This is to help you write your own instructions.

**What does my environment look like?**

1. Single Domain
2. Hybrid Deployment of Exchange 2013 SP1 with the latest URs AND all latest security patches running on Server 2012.
3. AD and forest functional level 2012 R2 with all latest updates.
4. Going to Exchange 2019 with the latest URs AND all latest security patches running on server 2019.

**Here are references to articles, blogs, and documentation I used when extending AD for Exchange 2019.**

1. <https://docs.microsoft.com/en-us/exchange/plan-and-deploy/plan-and-deploy?view=exchserver-2019>
2. <https://social.technet.microsoft.com/Forums/en-US/bb02e0dd-c3ef-4abc-8d08-9e58d6b8f3ed/prepare-ad-tenantorganizationconfig>
3. <https://www.nucleustechnologies.com/blog/step-by-step-guide-to-install-exchange-server-2019-part-1/>

**My Notes**

1.

Prerequisites - Regardless of all the Exchange prerequisites needed, you MUST have at least the following items to prepare AD using the command line.

1. .NET Framework 4.8
2. Visual C++ Redistributable for Visual Studio 2012
3. Remote Tools Administration Pack

You can see the above information at this link: <https://docs.microsoft.com/en-us/exchange/plan-and-deploy/prerequisites?view=exchserver-2019#exchange-2019-prerequisites-for-preparing-active-directory>

To install Exchange there are many more prerequisites.

2.

ALL domain controllers MUST BE UP, and you have to give time for replication to take place.

3.

Since I am already running a hybrid environment, I had to download the hybrid config xml from O365 and add the xml file to the final command. For reference and more information, check this blog: <https://blog.rmilne.ca/2021/05/10/tenantorganizationconfig-required-when-preparing-active-directory/>

4.

Here was my final command:

`Setup.exe /IAcceptExchangeServerLicenseTerms_DiagnosticDataOFF /PrepareAD /OrganizationName:"orgname" /TenantOrganizationConfig C:\MyTenantOrganizationConfig.xml`
