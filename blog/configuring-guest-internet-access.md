---
layout: default
permalink: /blog/configuring-guest-internet-access
---
[Home](https://lonestarcoder.github.io) | [Blog](https://lonestarcoder.github.io/blog/home) | [Articles](https://lonestarcoder.github.io/articles)
# Configuring Guest Internet Access
*Author: Brody Kilpatrick* | *Created: January 2, 2011*

To configure internet access:  
Create a new external network in Hyper v and bind it with a physical NIC that has  
internet access.  
On the Host, go to network connections. Then bridge the new network with the physical adapter  
After bridging, check to see if the guest can access the internet.  
Also, check to make sure the guest and host can still talk. YOu may have to add the names to the hosts file,  
otherwise talk by IP  
If you have a firewall on, on your guest, make sure you allow ping.  
In advanced firewall settings, there is already a rule called file and printer sharing (Echo Request - ICMPv4-In)  
Enable it
