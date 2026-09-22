---
layout: default
permalink: /blog/dp-with-failed-stuff-and-powershell-to-redistribute-from-b
---
[Home](https://lonestarcoder.github.io) | [Blog](https://lonestarcoder.github.io/blog/home) | [Articles](https://lonestarcoder.github.io/articles)
# DP With Failed Stuff and PowerShell to Redistribute (From B)
*Author: Brody Kilpatrick* | *Created: July 29, 2019*

This is from B.

This is what I've been using to fix some of the SP issues.

We should theoretically run this for all
failed deployments on a scheduled, but,.. it would crash if we would do that.

/////////////////////////////////////////////////////////////////////

declare @MyDP as varchar(25)

------------------------------------

----- partial DP name here ---------

------------------------------------

SET @MyDP = 'ehsin'

------------------------------------

------------------------------------

SELECT distinct A.DPName, A.PackageID, B.Name,

CASE a.MessageState

WHEN 1 THEN 'Success'

WHEN 2 THEN 'In Progress'

WHEN 4 THEN 'Failed'

ELSE 'Unknown'

END AS [Status]

,'$dp = Get-WmiObject -Namespace
"root\SMS\Site\_CEN" -Query "Select \* From SMS\_DistributionPoint
WHERE PackageID=''' + A.PackageID + ''' and serverNALPath like
''%' + A.DPname
+ '%''"' + CHAR(13)+CHAR(10) + '$dp.RefreshNow = $true' +
CHAR(13)+CHAR(10) + '$dp.Put()' as [PS]

FROM vSMS\_DPStatusDetails A

JOIN SMSPackages\_All B ON A.PackageID = B.PkgID

WHERE MessageState &lt;&gt; 1

and (a.dpname like '%' + @MyDP + '%')

--and B.Name like '%java%'

group by B.Name,A.DPName, A.PackageID, a.MessageState

order by B.Name, A.DPName, A.PackageID, [Status], [PS]

///////////////////////////////////////////////////////////////////////////////

Then run the ones in questions in powershell on the sccm
server

![](/assets/blog/dprefreshscreenshot.jpg)
