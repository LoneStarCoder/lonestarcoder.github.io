---
layout: default
permalink: /blog/complianceoutlooklogontoarchiverpcctpprobe-has-failed-against-servername01-proxying-to-unknown-for-healthmailbox
---
[Home](https://lonestarcoder.github.io) | [Blog](https://lonestarcoder.github.io/blog/home) | [Articles](https://lonestarcoder.github.io/articles)
# ComplianceOutlookLogonToArchiveRpcCtpProbe has failed against servername01 proxying to Unknown for healthmailbox
*Author: Brody Kilpatrick* | *Created: August 16, 2019*

```powershell
$ServerName = "servername01"
$Mailbox = "healthmailboxbb6621894bb246e4a8aba8aa8d3f4f55"


get-service MSExchangeHM -ComputerName $ServerName | stop-service
get-mailbox $Mailbox -Monitoring | Remove-Mailbox
get-service MSExchangeHM -ComputerName $ServerName | start-service
```
