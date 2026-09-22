---
layout: default
permalink: /blog/powershell-uninstall-scom-agent-using-wmi
---
# PowerShell - Uninstall SCOM Agent Using WMI
*Author: Brody Kilpatrick* | *Created: July 12, 2019*

Sometimes you need to uninstall one or more SCOM agents using PowerShell. This script does NOT require SCOM cmdlets or anything special and it can easily be incorporated into a larger script. It uses WMI to perform a clean uninstall.

```powershell
$computers = 'computer1','computer2'
foreach($server in $computers)
{
     $server
     $app = Get-WmiObject -Class Win32_Product -computername $server | ? {$_.Name -eq 'Microsoft Monitoring Agent'}
     $var = ($app.Uninstall() ).returnvalue
if($var -eq 0)
    {
   write-host "Successfully Uninstalled or was not found."
    }
    else
    {
     Write-host "Uninstallation Failed"
    
    }
  
}
```

I received the original idea from Ramkumar Natarajan who posted a script on the Microsoft Code gallery. However, this script is simpler and more malleable.
