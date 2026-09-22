---
layout: default
permalink: /blog/notes-regarding-scsm-2012-upgrade
---
[Home](https://lonestarcoder.github.io) | [Blog](https://lonestarcoder.github.io/blog/home) | [Articles](https://lonestarcoder.github.io/articles)
# Notes Regarding SCSM 2012 Upgrade
*Author: Brody Kilpatrick* | *Created: January 15, 2013*

I just wanted to share some notes regarding the Service Manager 2012 SP1 Upgrade, that might not be obvious unless you thoroughly read the documentation. I hope these notes help prevent some problems.  
  
Release Notes:  
<http://technet.microsoft.com/en-us/library/jj614520.aspx>   
  
**The SCSM Console New Requirement:**  
Upgrade Note2-core 2.0 GHz CPU  
4 GB of RAM  
10 GB of available disk spaces  
new requirement of Microsoft SQL Server 2012 Analysis Management Objects (AMO). Microsoft SQL Server 2012 AMO is supported on SQL Server 2008 and SQL Server 2012  
  
  
**Self-Service Portal: Web Content Server with SharePoint Web Parts**  
8-Core 2.66 GHz CPU  
8-core, 64-bit CPU for medium deployments  
16 GB of RAM for 20,000 users, 32 GB of RAM for 50,000 users (See the Hardware Performance section in this guide.)  
80 GB of available hard disk space  
  
  
**When you upgrade from System Center 2012 – Service Manager, you perform an in-place upgrade of the Self-Service Portal. - This is the only thing the documentation says. I am not sure what it means.**  
  
**Authoring Tool Workflows**  
When you use the Service Manager SP1 version of the Authoring tool to create a workflow, then custom scripts using Windows PowerShell cmdlets called by the workflow fail. This is due to a problem in the Service Manager MonitoringHost.exe.config file.  
  
To work around this problem, update the MonitoringHost.exe.config XML file using the following steps.  
  
  

1.     Navigate to %ProgramFiles%\Microsoft System Center 2012\Service Manager\ or the location where you installed Service Manager.

2.     Edit the MonitoringHost.exe.config file and add the section in italic type from the example below in the corresponding section of your file. You must insert the section before &lt;publisherPolicy apply="yes" /&gt;.

3.     Save your changes to the file.

4.     Restart the System Center Management service on the Service Manager management server.

```
<?xml version="1.0"?>  
<configuration>  
  <configSections>  
    <section name="uri" type="System.Configuration.UriSection, System, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />  
  </configSections>  
  <uri>  
    <iriParsing enabled="true" />  
  </uri>    
  <runtime>  
    <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">  
      <dependentAssembly>  
        <assemblyIdentity name="Microsoft.Mom.Modules.DataTypes" publicKeyToken="31bf3856ad364e35" />  
        <publisherPolicy apply="no" />  
        <bindingRedirect oldVersion="6.0.4900.0" newVersion="7.0.5000.0" />  
      </dependentAssembly>  
      <dependentAssembly>  
        <assemblyIdentity name="Microsoft.EnterpriseManagement.HealthService.Modules.WorkflowFoundation" publicKeyToken="31bf3856ad364e35" />  
        <publisherPolicy apply="no" />  
        <bindingRedirect oldVersion="6.0.4900.0" newVersion="7.0.5000.0" />  
      </dependentAssembly>  
  <dependentAssembly>   
         <assemblyIdentity name="Microsoft.EnterpriseManagement.Modules.PowerShell" publicKeyToken="31bf3856ad364e35" />  
        <bindingRedirect oldVersion="6.0.4900.0" newVersion="7.0.5000.0" />  
     </dependentAssembly>   
      <publisherPolicy apply="yes" />  
      <probing privatePath="" />  
    </assemblyBinding>  
    <gcConcurrent enabled="true" />  
  </runtime>  
</configuration>
```
