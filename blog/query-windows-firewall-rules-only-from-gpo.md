---
layout: default
permalink: /blog/query-windows-firewall-rules-only-from-gpo
---
[Home](https://lonestarcoder.github.io) | [Blog](https://lonestarcoder.github.io/blog/home) | [Articles](https://lonestarcoder.github.io/articles)
# Query Windows Firewall Rules ONLY from GPO
*Author: Brody Kilpatrick* | *Created: August 18, 2022*

If a computer has a GPO or a local security policy that defines windows firewall rules, AND there are also local firewall rules, it is difficult to discern between them unless you open each rule. The below PowerShell script will only retrieve rules that were created via an Active Directory GPO or the local security policy (gpedit.msc).

## This first section simply grabs the rules and exports them to CSV.

```powershell
#Rules From GPO Only - Export to CSV
$FirewallRulesfromGPO = Get-NetFirewallRule -PolicyStore RSOP -PolicyStoreSourceType GroupPolicy
$FirewallRulesfromGPO | Export-Csv -NoTypeInformation -Path C:\temp\FirewallRulesfromGPO.csv
```

## This second section retrieves filtering details for each rule.

```powershell
#ALL RULE DETAILS - PROBABLY TOO MUCH INFORMATION
foreach ($rule in $FirewallRulesfromGPO){
write-host Name: $rule.Name
write-host DisplayName: $Rule.DisplayName
$FirewallRuleName = $rule.Name
write-host "Security Filter" -ForegroundColor Green
$FirewallRulesfromGPO | ? {$_.Name -eq $FirewallRuleName} | Get-NetFirewallSecurityFilter | ft
write-host "Port Filter" -ForegroundColor Green
$FirewallRulesfromGPO | ? {$_.Name -eq $FirewallRuleName} | Get-NetFirewallPortFilter | ft
write-host "Service Filter" -ForegroundColor Green
$FirewallRulesfromGPO | ? {$_.Name -eq $FirewallRuleName} | Get-NetFirewallServiceFilter | ft
write-host "Interface Type Filter" -ForegroundColor Green
$FirewallRulesfromGPO | ? {$_.Name -eq $FirewallRuleName} | Get-NetFirewallInterfaceTypeFilter | ft
write-host "Interface Filter" -ForegroundColor Green
$FirewallRulesfromGPO | ? {$_.Name -eq $FirewallRuleName} | Get-NetFirewallInterfaceFilter | ft
write-host "Application Filter" -ForegroundColor Green
$FirewallRulesfromGPO | ? {$_.Name -eq $FirewallRuleName} | Get-NetFirewallApplicationFilter | ft
write-host "Address Filter" -ForegroundColor Green
$FirewallRulesfromGPO | ? {$_.Name -eq $FirewallRuleName} | Get-NetFirewallAddressFilter | ft
}
```

### As a bonus, here is a simple filter that will find rules where the IP address is something besides "any".

```powershell
#RSOP RULES WHERE THE ADDRESS FILTER IS FILTERED DOWN TO SOMETHING BESIDES ANY
Get-NetFirewallAddressFilter -PolicyStore RSOP | ? {$_.LocalAddress -ne "Any" -or $_.RemoteAddress -ne "Any"} | select InstanceId, CreationClassName, LocalAddress, RemoteAddress
```
