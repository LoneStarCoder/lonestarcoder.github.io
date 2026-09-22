---
layout: default
permalink: /blog/when-trying-the-request-software-link-on-the-end-user-portal-you-get-the-message
---
[Home](https://lonestarcoder.github.io) | [Blog](https://lonestarcoder.github.io/blog/home) | [Articles](https://lonestarcoder.github.io/articles)
# When trying the "Request Software" link on the end user portal, you get the message "Unable to load portal ActiveX Control.
*Author: Brody Kilpatrick* | *Created: December 6, 2010*

**Problem:** When trying the "Request Software" link on the end user portal, you get the message "Unable to load portal ActiveX Control."  
  
**Resolution**: Install the PortalClient.msi file located on the Installation media. When you install the media, you receive no notifications, but that is OK. It still worked. Try to request software again.  
  
*Most users will use the self-service portal to request software. Create a package in SCCM to deploy the package to all workstations so users never see this error.*
