---
layout: default
permalink: /blog/scsm-powershell-reassign-all-work-items-to-another-user
---
[Home](https://lonestarcoder.github.io) | [Blog](https://lonestarcoder.github.io/blog/home) | [Articles](https://lonestarcoder.github.io/articles)
# SCSM PowerShell - Reassign All Work Items to Another User
*Author: Brody Kilpatrick* | *Created: April 26, 2019*

This is another quickie. It find all work items assigned to one ad group/user, and then assigns to another.

```
$CurrentAssignedUser = "username1"
$NewAssignedUser = "username2"

import-module smlets
$DUClass = get-SCSMClass System.Domain.User$
$WIAssignedRelClass = Get-SCSMRelationshipClass System.WorkItemAssignedToUser$
$WIAssignedRelClassId = $WIAssignedRelClass.Id
$WIClass = Get-SCSMClass System.WorkItem$

#Get the currently Assigned User
$CurrentAssignedUserObject = Get-SCSMObject -Class $DUClass -Filter "Username -eq $CurrentAssignedUser"

#Get the User to Assign to
$NewAssignedUserObject = Get-SCSMObject -Class $DUClass -Filter "Username -eq $NewAssignedUser"

#Find all tickets assigned to the user
$WIRelObjectArray = @()
$WIRelObjectArray = Get-SCSMRelationshipObject -ByTarget $CurrentAssignedUserObject | ? {$_.RelationshipId -eq $WIAssignedRelClassId}
foreach  ($WIRelObject in $WIRelObjectArray)
 {
 $WIObject = ($WIRelObject.SourceObject)
 # Get-SCSMObject -class $WIClass -Filter "Id -eq $WIRelObjectid"
 New-SCSMRelationshipObject -Relationship $WIAssignedRelClass -Source $WIObject -Target $NewAssignedUserObject -Bulk -Verbose
 }

```
