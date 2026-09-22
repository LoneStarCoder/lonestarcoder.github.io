---
layout: default
permalink: /blog/scsm-command-shell
---
[Home](https://lonestarcoder.github.io) | [Blog](https://lonestarcoder.github.io/blog/home) | [Articles](https://lonestarcoder.github.io/articles)
# SCSM Command Shell
*Author: Brody Kilpatrick* | *Created: November 29, 2010*

Unlike Operations Manager and Exchange 2010, the Service Manager command shell is not simply a shortcut. However, you can add the Service Manager command shell to PowerShell. The below is a clip taken from a [TechNet article](http://technet.microsoft.com/en-us/library/ff461183.aspx).  
  
  

#### To add the Service Manager Windows PowerShell snap-in to a PowerShell session

1. On the computer that you run Windows PowerShell on, for example, the computer that hosts the Service Manager or data warehouse management server, on the taskbar, click **Start**, point to **Programs**, point to **Windows PowerShell 1.0**, right-click **Windows PowerShell**, and then click **Run as administrator**.
2. In the Windows PowerShell window, type the following commands:

- Set-ExecutionPolicy RemoteSigned
- Add-PSSnapIn SMCmdletSnapIn
