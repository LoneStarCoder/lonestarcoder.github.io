---
layout: default
permalink: /blog/workflow-performance-management-packs-on-the-media-to-help-performance
---
[Home](https://lonestarcoder.github.io) | [Blog](https://lonestarcoder.github.io/blog/home) | [Articles](https://lonestarcoder.github.io/articles)
# Workflow Performance - Management Packs on the media to help Performance
*Author: Brody Kilpatrick* | *Created: December 29, 2010*

## Workflow Performance

Workflows are automatic processes that occur and include sending e-mail notifications, the next step of a change request activating, and automatically applying a template.  

- Normally, workflows start and finish within 1 minute. When Service Manager server roles are under a heavy workload, workflows do not complete as quickly as normal.
- Additionally, when you create new workflows, such as a new notification subscription, additional load is placed on the system. As the number of new workflows that you create increases, the time it takes for each one to run also increases.

When the system is under a heavy load, if for example a large number of new incidents are being created and each incident generates many workflows, then performance might be negatively affected.  
If you plan to create a large number of workflows, one possible solution to help improve performance is to use the **ManagmentHostKeepAlive management pack that is included in the Service Manager release media.**  

- You need to manually copy the two files from the source directory into the Service Manager installation directory, and then import the management pack files.
- Importing these management pack files can greatly increase workflow processing responsiveness where almost all workflows process within 1 minute.
- However, importing this management pack gives higher priority to workflow processing and can lead to slower Service Manager console response in some cases so you should test its impact before deployment in a production environment.
