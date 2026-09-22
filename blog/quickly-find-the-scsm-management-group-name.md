---
layout: default
permalink: /blog/quickly-find-the-scsm-management-group-name
---
[Home](https://lonestarcoder.github.io) | [Blog](https://lonestarcoder.github.io/blog/home) | [Articles](https://lonestarcoder.github.io/articles)
# Quickly find the SCSM Management Group Name
*Author: Brody Kilpatrick* | *Created: September 12, 2011*

I always assumed the SCSM management group name was in the title of the console until I actually needed one day, then I realized it wasn't there. It seems like you would see the Management Group name every where, but it is not. Use the registry on a SCSM server to find it.  
  
Open regedit:  
HKEY\_LOCAL\_Machine\SOFTWARE\Microsoft\Microsoft Operations Manager\3.0\Server Management Groups
