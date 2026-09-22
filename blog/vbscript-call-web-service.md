---
layout: default
permalink: /blog/vbscript-call-web-service
---
[Home](https://lonestarcoder.github.io) | [Blog](https://lonestarcoder.github.io/blog/home) | [Articles](https://lonestarcoder.github.io/articles)
# vbscript - call web service
*Author: Brody Kilpatrick* | *Created: April 11, 2011*

<http://social.msdn.microsoft.com/forums/en-US/asmxandxml/thread/4f00c08e-0188-48ac-9bd8-78607c8fbfb9/>   
  
Dim oXMLDoc, oXMLHTTP  
Sub btnApproveLeave\_Click  
Set oXMLHTTP = CreateObject("MSXML2.XMLHTTP.3.0")  
Set oXMLDoc = CreateObject("MSXML2.DOMDocument")  
Msgbox("Calling WebService To Approve Leave")  
oXMLHTTP.onreadystatechange = getRef("HandleStateChange")  
call oXMLHTTP.open("POST","http://10.253.14.136/LeaveRequestWebService/CodeWebService.asmx/ApproveLeave",False)  
call oXMLHTTP.setRequestHeader("Content-Type","application/x-www-form-urlencoded")  
call oXMLHTTP.send()  
End Sub  
Sub HandleStateChange  
if(oXMLHTTP.readyState = 4) then  
dim szResponse: szResponse = oXMLHTTP.responseText  
call oXMLDoc.loadXML(szResponse)  
if(oXMLDoc.parseError.errorCode &lt;&gt; 0) then  
call msgbox(oXMLDoc.parseError.reason)  
else  
call msgbox(oXMLDoc.getElementsByTagName("string")(0).childNodes(0).text)  
end if  
end if  
End Sub
