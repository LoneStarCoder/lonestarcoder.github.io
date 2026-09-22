---
layout: default
permalink: /blog/approve-scom-agents-with-powershell
---
[Home](https://lonestarcoder.github.io) | [Blog](https://lonestarcoder.github.io/blog/home) | [Articles](https://lonestarcoder.github.io/articles)
# Approve SCOM Agents with Powershell
*Author: Brody Kilpatrick* | *Created: January 20, 2011*

get-agentpendingaction | where-object {$\_.AgentName -eq "server1.that.I.amtryingtomanage.com"} | approve-agentpendingaction
