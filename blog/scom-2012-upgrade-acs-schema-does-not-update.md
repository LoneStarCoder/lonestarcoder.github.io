---
layout: default
permalink: /blog/scom-2012-upgrade-acs-schema-does-not-update
---
[Home](https://lonestarcoder.github.io) | [Blog](https://lonestarcoder.github.io/blog/home) | [Articles](https://lonestarcoder.github.io/articles)
# SCOM 2012 Upgrade ACS Schema does not Update
*Author: Brody Kilpatrick* | *Created: July 19, 2013*

I didn't write the blog post, but I did experience the issue, and it is worth repeating. It seems this is going to affect anyone upgrading from SCOM 2012 to SP1 who uses ACS. The issue is that the schema doesn't get updated, causing crashes to ADTServer. If you want to full details, you can refer to this blog post <http://nocentdocent.wordpress.com/2013/02/12/sysctr-2012-sp1-acs-upgrade-error-scom-sysctr/>.   
  
Here are the steps you need to take, assuming you are having the issue. Again, if you are not sure you are having the issue, refer to the post above.   
  

1. Check the current Schema version of ACS

1. Open SQL Management Studio
2. Run the query Select \* FROM dtConfig
3. Check the row with a comment called "database schema version"
4. Make sure it is at version 7. If it is at version 7, continue. If not, do NOT do the rest of the steps.

2. Backup your ACS Database (Mine is Called OperationsManagerAC)
3. Find the SQL script called DbUpgV7toV8.sql
4. Making the assumption you already upgraded ACS, you can find the script in C:\Windows\System32\Security\AdtServer
5. Execute the script against your acs database
6. You might have to give it a day or so before another partition is created. If you don't receive any errors, you should be good.
