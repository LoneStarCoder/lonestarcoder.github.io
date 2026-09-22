---
layout: default
permalink: /blog/sql-query-sccm-breakout-ou-into-columns
---
# SQL Query - SCCM Breakout OU into Columns
*Author: Brody Kilpatrick* | *Created: April 17, 2019*

Start with the below query and turn it into a view, or simply add it as the sub query. It breaks the OU into rows.

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

Then use this query the take the data from the above query and break into up to seven columns. There are other ways to do this, but this was very quick and met my needs at the time.

```
Select one.ResourceID, 
coalesce(seven.System_OU_Name0,six.System_OU_Name0,five.System_OU_Name0,four.System_OU_Name0,three.System_OU_Name0,two.System_OU_Name0,one.System_OU_Name0) as FullOU, 
one.OU as OUOne, two.OU as OUTwo, three.OU as OUThree, four.OU as OUFour, five.OU as OUFive, six.OU as OUSix, seven.OU as OUSeven
FROM      [dbo].[v_Custom_System_OU_Breakout] one
left join [dbo].[v_Shared_BASE_System_OU_Breakout] two on one.ResourceID = two.ResourceID and two.depth = 2
left join [dbo].[v_Shared_BASE_System_OU_Breakout] three on one.ResourceID = three.ResourceID and three.depth = 3
left join [dbo].[v_Shared_BASE_System_OU_Breakout] four on one.ResourceID = four.ResourceID and four.depth = 4
left join [dbo].[v_Shared_BASE_System_OU_Breakout] five on one.ResourceID = five.ResourceID and five.depth = 5
left join [dbo].[v_Shared_BASE_System_OU_Breakout] six on one.ResourceID = six.ResourceID and six.depth = 6
left join [dbo].[v_Shared_BASE_System_OU_Breakout] seven on one.ResourceID = seven.ResourceID and seven.depth = 7
where one.depth = 1
```
