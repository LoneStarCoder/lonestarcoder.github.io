---
layout: default
permalink: /blog/using-the-ace-driver-with-64-bit
---
[Home](https://lonestarcoder.github.io) | [Blog](https://lonestarcoder.github.io/blog/home) | [Articles](https://lonestarcoder.github.io/articles)
# Using the ACE Driver with 64 bit
*Author: Brody Kilpatrick* | *Created: January 14, 2011*

<http://www.microsoft.com/downloads/en/details.aspx?FamilyID=c06b8369-60dd-4b64-a44b-84b371ede16d&displaylang=en>    
<http://social.msdn.microsoft.com/Forums/en/sqldataaccess/thread/4887d91f-6ac7-40c0-9fc8-5cdd0634e603>   
  
  
Everyone so far is fairly correct.  This provider is not released for 64 bit and so you need a 32 bit process to host it.  Your scenario appears to be a linked server scenario as well.  To put the pieces together, you have the following scenario:  
  
1.) 64 bit Windows Operating System  
2.) 64 bit SQL Server installed  
3.) Linked server to &lt;insert office provider here&gt;  
  
Unfortunately, since there is no 64 bit provider, you cannot create linked servers directly to these data sources through (64 bit) SQL Server.  From any 64 bit process, these providers are not available.  
  
There are 3 workarounds, ranging in complexity:  
  
Option 1:  
Use 32 bit SQL Server on the 64 bit machine.  It will now be able to see all the 32 bit providers.  Until provider vendors begin to support 64 bit, this offers the highest level of interactivity with a minimum of labor / maintenance cost.  However, it also comes with a performance penalty as SQL Server will be running in WoW mode on the 64 bit server.  
  
Option 2:  
Build a bridge out of SQL Express.  Keep your main SQL Server instance 64 bit, but also install SQL Express 32 bit Side by Side.  Then when you need to use 32 bit providers, create the linked server on the SQL Express instance and link to that from the main instance.  Your linked server pathway would go SQL Server 64 bit -> SQL Server 32 bit (over 64 bit client) -> (32 bit client) for linked server X.  This has less performance cost, but more maintenance cost.  Still no development costs, though, outside of configuring the linked servers.  
  
Option 3:  
Reverse the flow of data from pull to push.  Instead of having SQL Server pull data from the source that only supports 32 bit clients, push data from that source to SQL Server (as it supports 32/64 bit clients).  For example: instead of having SQL Server pull data from an Access file, include macros in the Access file to connect to SQL Server and push the data up to the server.  Just don't do this one.  It is very expensive and makes it impossible to completely migrate, also often requires human intervention to perform the actual push.  
  
I have suggested these 3 options in one way or another on these forums quite a few times, and almost always see people go for Option 1, though a few have taken Option 2 if that provides some sense of where people in general are currently leaning when they see this problem.  
  
  
I hope that helps,  
  
John
