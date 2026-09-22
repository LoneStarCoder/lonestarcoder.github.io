---
layout: default
permalink: /blog/my-first-look-at-network-monitoring-in-a-real-environment
---
# My First Look at Network Monitoring in a Real Environment
*Author: Brody Kilpatrick* | *Created: January 17, 2012*

I recently deployed System Center 2012: Operations Manager in a development environment. I don't mean a small virtual lab. I mean a real environment with multiple devices that can be monitored via SNMP.  
  
I will try to categorize a little here to make reading more structured.  
  
**Initial Discovery**  
Without reading any documentation, I went into the administration pane, looked around a little and noticed the Network Management category, which includes Discovery rules. First, I created a recursive discovery rule. I decided to use a router as a seed, to which a couple of switches were connected. I only filtered to the subnet I was discovering. I was disappointed when the initial discovery only discovered the router and its interfaces, but NONE of the attached devices. I went back into the discovery, and this time I used a switch as a seed. By using the switch as the seed, I was able to discover all connected devices, as expected.  
  
**SO, question 1: Why am I unable to use a router as a seed device to use for discovery?** I don't know the answer to this, and would love to find out.  
  
Besides the one question I have, device discovery was a breeze. It was very easy, the steps are intuitive, and the nodes/interfaces were discovered and related as expected.  
In terms of speed, the discovery was quite show for such a small number of devices on a Gigabit network. My Management Server was well-under utilized. I currently don't see this as an issue, but we will see where it takes us later.  
  
**Network Device Layouts and Dashboard**  
Microsoft did a pretty good job providing some nice out-of the box views. The views include a couple of dashboards, network vicinity, performance, availability, etc. You can see some screen shots [here.](http://www.techrepublic.com/blog/networking/using-the-network-dashboard-views-in-scom-2012/5226) <http://www.techrepublic.com/blog/networking/using-the-network-dashboard-views-in-scom-2012/5226>  
  
**Rules and Monitoring**  
Besides the normal up/down monitors, Microsoft provides a nice set of SNMP rules and monitors to monitoring performance and availability. Many of the rules are off by default. If you want to view a list of the rules from the console, you can scope you rule view the the Node and Interface classes.  
  
**Alerting**  
So it looks like Microsoft fell a little short on alerting. While alerting exists, there is not correlation. For example, ideally, if you have a router that goes down, and on that router you have a switch and ten devices connected to that switch, you would get ONE ALERT for the router itself, while all other alerts are suppressed. Unfortunately, this is not the case. You will get 12 ALERTS!!! One for the Router, One for the Switch, and ten for the Devices (assuming no backup link). Most of us can live with this, but I hope this is one of the first things that is changed.  
  
**Overall**  
In a sense, this is version 2 of SCOM Network monitoring. However, I am going to call it version 1, because they didn't really try to first time around in SCOM 2007. Overall, they did a fine job. If you are looking to replace your current network monitoring solution, you really need to bring SCOM 2012 up in your environment and take a look first. If you don't have a network monitoring solution, SCOM 2012 will provide a great foundation on which to build and customize.
