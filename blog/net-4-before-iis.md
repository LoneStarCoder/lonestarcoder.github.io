---
layout: default
permalink: /blog/net-4-before-iis
---
# .NET 4 BEFORE IIS
*Author: Brody Kilpatrick* | *Created: October 24, 2011*

When installing anything in Operations Manager 2012, .NET 4 must be installed before IIS. Otherwise, certain steps will have to be taken to resolve the issue.  
  
From the Microsoft DOC:  
  
  

***Important**

You must install IIS before installing .NET Framework 4. If you installed IIS after installing .NET Framework 4, you must register ASP.NET 4.0 with IIS. Open a Command prompt window by using the Run As Administrator option and then run the following command:

%WINDIR%\Microsoft.NET\Framework64\v4.0.30319\aspnet\_regiis.exe -r*
