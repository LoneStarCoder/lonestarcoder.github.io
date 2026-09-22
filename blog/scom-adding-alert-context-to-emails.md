---
layout: default
permalink: /blog/scom-adding-alert-context-to-emails
---
[Home](https://lonestarcoder.github.io) | [Blog](https://lonestarcoder.github.io/blog/home) | [Articles](https://lonestarcoder.github.io/articles)
# SCOM - Adding Alert Context to Emails
*Author: Brody Kilpatrick* | *Created: February 19, 2015*

I don't see a lot about this out here, so I am posting.  
  
Operations Manager notifications work pretty well. There are several community solutions out there to enhance it, but many of us still use the out of the box notifications. Over the years I have noticed people asking about adding alert context to email, but never receive any good answers. The alert context is powerful, and depending on the alert can provide a ton of data. The variables we use in email is simply xml.  
  
For example, Alert Name: $Data[Default='Not Present']/Context/DataItem/AlertName$  
  
Notice the slashes? Dataitem is the essentially where all of the alert context data is located. Want to see EVERYTHING in there?  
  
**Simply do this:**  
$Data[Default='Not Present']/Context$  
  
It will be different properties and data for each alert, and in some cases might be empty.  
However, I have found it VERY useful to append to the bottom of the emails.  
  
If you want to take a step further and actually see the different items in alert context, use powershell.  
  
import-module OperationsManager  
$a = Get-SCOMAlert | ?{$\_.name -like "\*SOMEALERT\*"} | select -first 1  
#All of the data  
([xml]($a.context)).dataItem | fl \*  
#just one property  
([xml]($a.context)).dataItem.Type  
  
Anyway, short and simple, hopefully it helps someone.
