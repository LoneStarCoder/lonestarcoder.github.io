---
layout: default
permalink: /blog/create-distribution-list
---
[Home](https://lonestarcoder.github.io) | [Blog](https://lonestarcoder.github.io/blog/home) | [Articles](https://lonestarcoder.github.io/articles)
# Create Distribution List
*Author: Brody Kilpatrick* | *Created: March 26, 2019*

```
#CreateDistributionList
#Connect to Exchange
$Session = New-PSSession -ConfigurationName Microsoft.Exchange -ConnectionUri http://serverFQDN/PowerShell
Import-PSSession $Session 
Get-PSSession $Session

$OU = "OU=Distribution Lists,DC=DOMAIN,DC=COM"
$DLAlias = "UDL.DLName"
$DLDisplayName = "DL Name"
$DLManagedBy = "owneruserid"
$DLMembersList = @()
$DLMembersList = 'owneruserid','user1','userid2','userid3','userid4'


New-DistributionGroup -Name $DLAlias -Alias $DLAlias -DisplayName $DLDisplayName -ManagedBy $DLManagedBy -Members $DLMembersList -OrganizationalUnit $OU

Get-DistributionGroup $DLAlias
(Get-DistributionGroupMember $DLAlias -ResultSize Unlimited).count

#Disconnect From Exchange
Get-PSSession | Remove-Pssession

```
