---
layout: default
permalink: /blog/powershell-search-for-certificate-on-server-in-an-ou
---
[Home](https://lonestarcoder.github.io) | [Blog](https://lonestarcoder.github.io/blog/home) | [Articles](https://lonestarcoder.github.io/articles)
# PowerShell - Search for Certificate on server in an OU
*Author: Brody Kilpatrick* | *Created: August 1, 2019*

My colleague Robert sent me this nice one-liner. It searches for a specific certificate across all servers in a specific OU.

```
Invoke-command -scriptblock {get-childitem "Cert:\LocalMachine\My" | where-object {$_.thumbprint -eq 'abcdefghijklmnoprstuvwxyzabcdefghijklmn'}} -computername @(Get-ADComputer -SearchBase 'OU=Servers,DC=domain,DC=com' -filter *).name -ErrorAction SilentlyContinue
```
