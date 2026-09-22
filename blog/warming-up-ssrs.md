---
layout: default
permalink: /blog/warming-up-ssrs
---
# Warming Up SSRS
*Author: Brody Kilpatrick* | *Created: March 17, 2011*

<http://www.sqlservercentral.com/Forums/Topic568045-149-1.aspx#bm568665>  
  
  

|  |
| --- |
| Here is simplest thing to keep SSRS in warmed up state:    To keep SSRS, in warmed up state, save the following script in text file and save as, say SSRSWarmup.vbs (this is vbscript). And then set up a schedule in Windows to execute SSRSWarmup.vbs, every 1 hour or so, using command line "wscript C:\SSRSWarmup.vbs". You can set it up on either SSRS server or in any client PC, which has got access to SSRS report specified in URL (inside script). SSRS is always in "warmed up" state!!!    /\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*/  Dim strURL, strFile    Set webObj = CreateObject("MSXML2.ServerXMLHTTP")    ' Replace below URL with the URL of any report in the reporting server  strURL = "http://aubbhsqlrept01/Reports/Pages/Report.aspx?ItemPath=%2fRetail%2fCustomerDetailsReport"  webObj.Open "GET", strURL  webObj.send  /\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*/ |
