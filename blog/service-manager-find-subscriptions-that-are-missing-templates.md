---
layout: default
permalink: /blog/service-manager-find-subscriptions-that-are-missing-templates
---
# Service Manager - Find subscriptions that are missing Templates
*Author: Brody Kilpatrick* | *Created: December 27, 2018*

---

Recently I performed some Service Manager notification subscription cleanup. Shortly after, I started receiving notifications from the Operations Manager event log stating the following:

> The System Center Data Access service client failed to send a notification because no template was provided.

We must have deleted a couple of email templates that were still in use by a notification subscription. We have too many subscriptions, and I didn't want to go through each one and find the missing template. So I decided to write a SQL query to find it. While the SQL query gave me the information that I needed, I wasn't real happy with the nasty query, so I decided to also write it in PowerShell.

If you ever receive the above error, find the subscription with the missing template in:

### SQL

```
Select rv.id RuleId, rv.name RuleName, rv.DisplayName RuleDisplayName, ot.ObjectTemplateId, ot.ObjectTemplateName, otds.DisplayName ObjectTemplateDisplayName,
Cast(rmwa.RuleModuleConfiguration as xml) RuleModuleWriteAction

FROM  RuleView rv
Join RuleModule rmwa on rv.id = rmwa.RuleId and rmwa.RuleModuleName = 'wa'
left Join ObjectTemplate ot on rmwa.RuleModuleConfiguration like '%' + Cast(ot.ObjectTemplateId as varchar(36)) + '%'
left join DisplayStringView otds on ot.ObjectTemplateName = otds.ElementName and otds.LanguageCode = 'enu'

where rmwa.RuleModuleName = 'wa' and
rv.Category = 'System'        and
rmwa.RuleModuleConfiguration like '%templateids%'

order by ot.objectTemplateId, rv.id
```

### PowerShell

```
Import-module smlets; (Get-SCSMSubscription) | foreach {if ($_.TemplateIds) {$ot = $null; $ot = get-SCSMObjectTemplate -Id $_.TemplateIds; if ($ot -eq $null){write-host "No Template Found for" $_.DisplayName " / Is Enabled " $_.Enabled}; } }
```
