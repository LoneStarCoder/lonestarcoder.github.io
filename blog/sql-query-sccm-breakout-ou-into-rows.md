---
layout: default
permalink: /blog/sql-query-sccm-breakout-ou-into-rows
---
[Home](https://lonestarcoder.github.io) | [Blog](https://lonestarcoder.github.io/blog/home) | [Articles](https://lonestarcoder.github.io/articles)
# SQL Query - SCCM Breakout OU into Rows
*Author: Brody Kilpatrick* | *Created: April 17, 2019*

```
Select Distinct ResourceID, 
replace(
		REVERSE(
				LEFT(
						REVERSE(VOU.System_OU_Name0),
						CHARINDEX('/',REVERSE(VOU.System_OU_Name0))
					)

			  )
	   ,'/',''
	  ) as OU

,LEN(VOU.System_OU_Name0) - LEN(REPLACE(VOU.System_OU_Name0, '/', '')) as depth
,VOU.System_OU_Name0
FROM CM_CEN.dbo.v_RA_System_SystemOUName VOU
```
