---
layout: default
permalink: /blog/exchange-powershell-add-mailbox-permissions
---
# Exchange Powershell - Add Mailbox Permissions
*Author: Brody Kilpatrick* | *Created: April 17, 2019*

```
$id = "MailboxYouWantToGrantAccessTo@mail.com"
$us =  "UserThatNeedsAccess@mail.com"

Get-MailboxPermission -Identity $id -User $us | ft -AutoSize -Property Identity, User,AccessRights

Add-MailboxPermission -Identity $id -user $us -AccessRights FullAccess -AutoMapping $true
get-mailbox -Identity $id | Add-ADPermission -User $us -ExtendedRights "Send As"
Set-Mailbox -Identity $id -GrantSendOnBehalfTo $us
```
