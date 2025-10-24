# How to install a simple postfix relay on Red Hat Linux
## Important Notes
* This is NOT a Secure Server. It is an open relay that will receive an email from anywhere and send it to anywhere.
* This guide assumes you already have an upstream smart host or server that the relay is going to send email to.
* If you do not, follow the instructions to create a simple IIS SMTP Windows Server, or just create your own, or just Google. Or don't create anything, you can still validate Postfix works without it.

## This server is designed to demonstrate:
* The Simplest Possible postfix relay configuration for someone who has never installed it.
* Adding a Mail Header to EVERY SINGLE EMAIL that traverses the relay.
* Proving that every email received by the relay gets the header.

### Some Prereqs and Names that you do not have to use, but I am going to use them, and if you want to follow EXACTLY, you will need to set the names and IPs to these as well. It's up to you.
* Red hat postfix server name: rhpostfix1.ls.local
* Red hat postfix server IP: 192.168.0.169
* Upstream SMTP server IP: 192.168.0.135

## Install Red Hat
You should not need instructions to install Red Hat, just install it.
### Set the Red Hat Host Name

### Set the Red Hat IP Address

### Open Firewall Ports
Firewall ports needed for SMTP Port 25

## Install Postfix

### This is a summary of the specific settings that we set in /etc/postfix/mail.cf
* myhostname = mailproxy.ls.local
* mydomain = ls.local
* myorigin = $mydomain
* inet_interfaces = all
* mydestination = $myhostname, localhost.$mydomain, localhost
* mynetworks = 127.0.0.0/8, 192.168.0.0/24
* relayhost = 192.168.0.135
* header_checks = regexp:/etc/postfix/header_checks

### This is a summary of the specific settings that we set in /etc/postfix/header_checks
This value is at the bottom of the file, and is the only value that is not a Comment (#).
* /^Received:/i PREPEND AUTH-X: mysecretkey
