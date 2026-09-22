---
layout: default
permalink: /blog/scsm-script-set-ma-to-in-progress
---
# SCSM Script - Set MA to In Progress
*Author: Brody Kilpatrick* | *Created: June 11, 2019*

Sometimes, for whatever reason, an MA never goes into "In Progress" in SCSM and may need a little help. Rather than using SMLETS or logging onto a SCSM machine, if you have the SCSM Console installed on your workstation, you can run it directly from there.

Start, Run, Service Manager Shell, no admin necessary.

```
param ([string]$ID
)
#$ID = 'MA1055025'
$ServerName = "managementserver01"
write-host  -ForegroundColor Yellow -BackgroundColor Black "importing C:\Program Files\Microsoft System Center\Service Manager 2012\PowerShell\System.Center.Service.Manager.psm1"
Import-Module "C:\Program Files\Microsoft System Center\Service Manager 2012\PowerShell\System.Center.Service.Manager.psm1"
write-host  -ForegroundColor Yellow -BackgroundColor Black "Connecting to $ServerName"
$MGCon = New-SCSMManagementGroupConnection -ComputerName $ServerName -PassThru
$MAClass = Get-SCClass -Name "System.WorkItem.Activity.ManualActivity"
write-host -ForegroundColor Yellow -BackgroundColor Black "Attempting to get $ID"
$ClassInstance = Get-SCClassInstance -Class $MAClass -Filter "Id -eq $ID"
if ($ClassInstance -eq $null) 
 {
  write-error "Could not find $ID"
 }
else
 {
  write-host -ForegroundColor Green -BackgroundColor Black  "Found $ID"
  write-host -ForegroundColor Yellow -BackgroundColor Black  "Attempting to Set to In Progress"
  $ClassInstance | %{$_.Status="ActivityStatusEnum.Active";$_.Notes=$($_.Notes + " Forcing to in progress");$_} | Update-SCSMClassInstance
  write-host -ForegroundColor Yellow -BackgroundColor Black  "Final Result:"
  Get-SCClassInstance -Class $MAClass -Filter "Id -eq $ID" | ft Id, Status, '#LastModified', Title 
 }
Write-host -ForegroundColor Yellow -BackgroundColor Black  "Removing SC Connection"
$MGCon | Remove-SCSMManagementGroupConnection
```
