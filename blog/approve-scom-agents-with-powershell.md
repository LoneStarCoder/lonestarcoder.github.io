---
layout: default
permalink: /blog/approve-scom-agents-with-powershell
---
# Approve SCOM Agents with Powershell
*Author: Brody Kilpatrick* | *Created: January 20, 2011*

get-agentpendingaction | where-object {$\_.AgentName -eq "server1.that.I.amtryingtomanage.com"} | approve-agentpendingaction
