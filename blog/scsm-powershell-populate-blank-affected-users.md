---
layout: default
permalink: /blog/scsm-powershell-populate-blank-affected-users
---
[Home](https://lonestarcoder.github.io) | [Blog](https://lonestarcoder.github.io/blog/home) | [Articles](https://lonestarcoder.github.io/articles)
# SCSM PowerShell - Populate Blank Affected Users
*Author: Brody Kilpatrick* | *Created: April 18, 2019*

This script will search for active SRs and IRs with no affected user. It will then take the assigned user and copy it to the affected user. This is just a snippet and not a full production script. You will need to add your error checking and other wrappings.

```
import-module smlets

$DUC = Get-SCSMClass System.Domain.User$
$AffUC = Get-SCSMRelationshipClass System.WorkItemAffectedUser$
$AssignedUC = Get-SCSMRelationshipClass system.workitemAssignedTouser$
$SRC = Get-SCSMClass System.WorkItem.ServiceRequest$
$IRC = Get-SCSMClass System.WorkItem.Incident$
$SRStatusSubmitted = (Get-SCSMEnumeration ServiceRequestStatusEnum.Submitted$).id
$SRStatusInProgress = (Get-SCSMEnumeration ServiceRequestStatusEnum.InProgress$).id
$IRStatusActive = (Get-SCSMEnumeration IncidentStatusEnum.Active$).id

#SRs
$SRActiveArray = Get-SCSMObject -Class $SRC -Filter "Status -eq $SRStatusSubmitted -or Status -eq $SRStatusInProgress"
$SRsWithNoAffectedUser = @()
foreach ($SR in $SRActiveArray)
 {
   $AffectedUserObject = $NULL
   $AffectedUserObject = Get-SCSMRelatedObject -SMObject $SR -Relationship $AffUC
   if ($AffectedUserObject -eq $NULL)
    {
     $SRsWithNoAffectedUser += $SR
    }
 }

$SRsWithNoAffectedUser.count

#For all of the SRs with no affected user, get the assigned to relationship and make it the affected user.

foreach ($SRWithNoAffectedUser in $SRsWithNoAffectedUser)
 {
  $SRWithNoAffectedUser
  $AssignedUserObject = $NULL
  $AssignedUserObject = Get-SCSMRelatedObject -SMObject $SRWithNoAffectedUser -Relationship $AssignedUC
  if ($AssignedUserObject -ne $null)
   {
    New-SCSMRelationshipObject -Relationship $AffUC -Source $SRWithNoAffectedUser -Target $AssignedUserObject -bulk
   }
 }
 
 
 
 #IRs
 
 $IRActiveArray = Get-SCSMObject -Class $IRC -Filter "Status -eq $IRStatusActive"
$IRsWithNoAffectedUser = @()
foreach ($IR in $IRActiveArray)
 {
   $AffectedUserObject = $NULL
   $AffectedUserObject = Get-SCSMRelatedObject -SMObject $IR -Relationship $AffUC
   if ($AffectedUserObject -eq $NULL)
    {
     $IRsWithNoAffectedUser += $IR
    }
 }

$IRsWithNoAffectedUser.count

#For all of the IRs with no affected user, get the assigned to relationship and make it the affected user.

foreach ($IRWithNoAffectedUser in $IRsWithNoAffectedUser)
 {
  $IRWithNoAffectedUser
  $AssignedUserObject = $NULL
  $AssignedUserObject = Get-SCSMRelatedObject -SMObject $IRWithNoAffectedUser -Relationship $AssignedUC
  if ($AssignedUserObject -ne $null)
   {
    New-SCSMRelationshipObject -Relationship $AffUC -Source $IRWithNoAffectedUser -Target $AssignedUserObject -bulk
   }
 }
```
