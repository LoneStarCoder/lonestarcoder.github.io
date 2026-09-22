---
layout: default
permalink: /blog/wsus-set-sql-express-max-memory-using-powershell
---
[Home](https://lonestarcoder.github.io) | [Blog](https://lonestarcoder.github.io/blog/home) | [Articles](https://lonestarcoder.github.io/articles)
# WSUS - Set SQL Express Max Memory using Powershell
*Author: Brody Kilpatrick* | *Created: March 17, 2021*

## Summary

If you are using SQL Express with WSUS, you may want to limit the amount of memory that it uses. Most likely you don't have any SQL tools installed, and there is no reason to install any tools because we can do it with PowerShell. This is just a simple script where you can change the $maxMem and $minMem variables.

## Script

```powershell
[string]$SQLInstanceName = "\\.\pipe\MICROSOFT##WID\tsql\query"
[int]$maxMem = 2048
[int]$minMem = 0
[reflection.assembly]::LoadWithPartialName("Microsoft.SqlServer.Smo") | Out-Null
$srv = New-Object Microsoft.SQLServer.Management.Smo.Server($SQLInstanceName)
if ($srv.status -eq "Online") {
        Write-Host "[Running]: Maximum Memory is $($srv.Configuration.MaxServerMemory.RunValue)"
        Write-Host "[Running]: Minimum Memory is: $($srv.Configuration.MinServerMemory.RunValue)"
 
        Write-Host "[New] Setting Maximum Memory to: $maxmem"
        Write-Host "[New] Setting Minimum Memory to: $minmem"
        $srv.Configuration.MaxServerMemory.ConfigValue = $maxMem
        $srv.Configuration.MinServerMemory.ConfigValue = $minMem   
        $srv.Configuration.Alter()
    }
    else {
      write-host "Server Status is not online. Actual Server Status is:"
      write-host $srv.Status
    }
```

## References

I used the following sites/blogs to find the information below.

- [#PowerShell – Setting SQL Server Memory Allocation (Maximum and Minimum) | CheckYourLogs.Net](https://www.checkyourlogs.net/powershell-setting-sql-server-memory-allocation-maximum-and-minimum/)
- [How Do I Connect to the Windows Internal Database (WID)? | AJ Tek Corporation](https://www.ajtek.ca/wsus/how-do-i-connect-to-the-windows-internal-database-wid/)
- [WSUS - Limit SQL (Windows Internal Database) memory | Nick Sturgess (stugr.com)](https://www.stugr.com/2013/01/24/wsus-limit-sql-windows-internal-database-memory/)
