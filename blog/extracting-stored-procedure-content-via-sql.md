---
layout: default
permalink: /blog/extracting-stored-procedure-content-via-sql
---
[Home](https://lonestarcoder.github.io) | [Blog](https://lonestarcoder.github.io/blog/home) | [Articles](https://lonestarcoder.github.io/articles)
# Extracting stored procedure content via SQL
*Author: Brody Kilpatrick* | *Created: March 30, 2011*

*Originally posted [October 18, 2006](http://weblogs.asp.net/cumpsd/archive/2006/10/18/681812.aspx) by [CumpsD](http://weblogs.asp.net/members/CumpsD.aspx), reposted here.*

Some nifty SQL statements I made last week:

Firstly, listing all databases on a server.

-- Get Databases  
SELECT name FROM master.dbo.sysdatabases ORDER BY name

Secondly, a way to get all the user-created stored procedures from a database.

-- Get Stored Procedures  
--    Type = 'P' --> Stored Procedure.  
--    Category = 0 --> User Created.  
SELECT \* FROM sysobjects WHERE type = 'P' AND category = 0 ORDER BY name

Then we can retrieve the content of the stored procedure with the following query:

-- Get Stored Procedure Content  
--    Name = Stored Procedure Name.  
--    Colid = Multiple lines, their sequence.  
SELECT text  
FROM syscomments  
WHERE id = (SELECT id FROM sysobjects WHERE name = '{0}')  
ORDER BY colid

In C# you could concatenate the returned records to get the full stored
