---
layout: default
permalink: /blog/configuring-the-internal-network
---
[Home](https://lonestarcoder.github.io) | [Blog](https://lonestarcoder.github.io/blog/home) | [Articles](https://lonestarcoder.github.io/articles)
# Configuring the internal network
*Author: Brody Kilpatrick* | *Created: January 2, 2011*

- Add the internal network connection to the VM
- This Network will allow communication between he host and VM
- Go to the Hyper v HOST
- Network Connections
- Find the connection with the same name that you gave the internal network you just created
- Configure a manual IP Address
- The IP Address should be within the range of the hosts subnet
- Save and go to the VM
- Network Connections
- Open the internal network connection
- Create a new Manual IP Address and subnet mask
- Make the default gateway the actual IP Address of the hosts network card you configured above.
- This should be all
