---
layout: default
permalink: /blog/enter-a-group-into-maintenance-mode-using-scom-the-console-no-scripts-required
---
# Enter a Group into Maintenance Mode using SCOM the Console (No Scripts Required)
*Author: Brody Kilpatrick* | *Created: February 22, 2013*

*I know this is a short post, but the idea is simplicity. Sometimes, we make things unnecessarily too complicated, and native functionality is sometimes overlooked. As a consultant, I strive to keep solutions for my customers as simple as possible while providing the required functionality.*

Posts regarding group or class maintenance mode using powershell, scripts, new management packs, and otherwise complicated solutions are all over the internet. Is it really necessary to go to all this trouble? Not really. Yes, I use powershell to enter groups into maintenance mode for certain reasons, such as scheduled patching. ***But not everyone is a SCOM/scripting guru, and wants to use powershell.***

I never see posts about entering groups into maintenance mode using the SCOM Console. It's simple, quick (much quicker than powershell), and effective.  
There a couple of ways to enter a group into maintenance mode, but I will cover a single easy way - *use discovered inventory.*

### How to enter a Group into Maintenance Mode the Easy Way

1. Open the SCOM Console- Go the the Monitoring Pane- Select "Discovered Inventory"- Select "Change Target Type"- Type in your Group Name- Select it and click OK- On the Discovered Inventory View, select your Group- Click "Start Maintenance Mode"- Edit the Maintenance Mode Settings- Click OK- VIOLA!- SIMPLE!- DONE!

A simple post for a simple, yet effective solution using native features of SCOM.If you need some help or have any questions, leave a comment and I will be happy to help.
