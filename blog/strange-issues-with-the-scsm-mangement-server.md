---
layout: default
permalink: /blog/strange-issues-with-the-scsm-mangement-server
---
[Home](https://lonestarcoder.github.io) | [Blog](https://lonestarcoder.github.io/blog/home) | [Articles](https://lonestarcoder.github.io/articles)
# Strange issues with the SCSM Mangement Server
*Author: Brody Kilpatrick* | *Created: February 13, 2011*

I sometimes find strange issues with the SCSM management server. For example, my connectors have not synchronized in several days. Performing a manual synchronization does nothing. When I look in the event log, there are no errors, but there is a sync start/end immediately.  
  
I have found that a restart of all the SCSM services will clear this up. If services restarts do not work, a server restart usually does work.
