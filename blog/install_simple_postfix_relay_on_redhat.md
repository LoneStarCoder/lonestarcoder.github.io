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
* Red hat postfix server IP: 192.168.0.149
* Upstream SMTP server IP: 192.168.0.135

## Install Red Hat (No need to follow if you already have a red hat server up and running)
You should not need instructions to install Red Hat, just install it.
But, if you still want some instuctions, then do this:
* Boot from the RHEL ISO you downloaded from Red Hat
* Start the Installer
* Set your language to English (United States)
* Register your server with Red Hat
* Select Automatic Partitioning
* Create a User, name it lonestar, Make sure "Add admin Privs" and "Require Pass" is checked. Add a password.
* Select Sofware Selection, under Base Environment, select "Server". On Additonal software, select "Guest Agents", if you are running in a VM and "Headless Management" if you aren't using a GUI. You can optional select "Mail Server" which will auto install Postfix, but If you choose not to do that, I still have Postfix installation instructions below.
* Begin you Installation and boot up your server, and login with lonestar
  
### Set the Red Hat Host Name
Run the following commands and reboot
```
sudo nmcli general hostname rhpostfix1.local
reboot
```

### Set the Red Hat IP Address

### Open Firewall Ports
Firewall ports needed for SMTP Port 25
```
sudo firewall-cmd --permanent --zone=internal --add-port=25/tcp
sudo firewall-cmd --reload
```
## Install Postfix
Use yum to install Postfix
```
sudo yum install -y postfix
```
Start and Enable the postfix service
```
sudo systemctl start postfix
sudo  systemctl enable postfix
```
## Configure Postfix
### Configure mail.cf
Backup main.cf
```
sudo cp /etc/postfix/main.cf /etc/postfix/main.cf.bak
```
Open main.cf for editing with nano
```
sudo nano /etc/postfix/main.cf
```
Using the reference below, add, uncomment, or verify each of the lines in the file. Most of them will have a placeholder, or a section.
#### This is a summary of the specific settings that we set in /etc/postfix/mail.cf
* myhostname = rhpostfix1.ls.local
* mydomain = ls.local
* myorigin = $mydomain
* inet_interfaces = all (make sure all the other ones are commented out)
* mydestination = $myhostname, localhost.$mydomain, localhost
* mynetworks = 127.0.0.0/8, 192.168.0.0/24
* relayhost = 192.168.0.135
* header_checks = regexp:/etc/postfix/header_checks

### Confiure header_checks
```
sudo nano /etc/postfix/header_checks
```
#### This is a summary of the specific settings that we set in /etc/postfix/header_checks
This value is at the bottom of the file, and is the only value that is not a Comment (#).
* /^Received:/i PREPEND AUTH-X: mysecretkey

### Restart Postfix
After editing the files, we need to reload postfix
```
sudo systemctl restart postfix
```
### Verify the mail.cf file doesn't have any errors
```
sudo postfix check
```
Just check for errors.

### Send a Test Email using Telnet
```
telnet 192.168.0.149 25
```
Add the HELO command to tell the server which domain you are coming from:
```
HELO ls.local
```
Next is the sender. This ID can be added with the MAIL FROM command:
```
MAIL FROM: testsender@ls.local
```
This entry is followed by the recipient, and you can add more than one by using the RCPT TO command multiple times:
```
RCPT TO: testrecipient@ls.local
```
Add the content of the message. To reach the content mode, we add the prefix DATA on a line by itself, followed by the Subject line, and the body message. Listed below is an example:
```
DATA
Subject: This is a test message  
Hello,
This is a test message.
.
```
It should give you a queue number, then you can quit
```
QUIT
```
Here is an example:
```
user@ubuntu01:~$ telnet 192.168.0.149 25
#Trying 192.168.0.149...
#Connected to 192.168.0.149.
#Escape character is '^]'.
#220 rhpostfix1.ls.local ESMTP Postfix
HELO ls.local
#250 rhpostfix1.ls.local
MAIL FROM: <testserver@ls.local>
#250 2.1.0 Ok
RCPT TO: <testrecipient@ls.local>
#250 2.1.5 Ok
DATA
#354 End data with <CR><LF>.<CR><LF>
Subject: This is a test Message
Hello,
This is a test message.
.
#250 2.0.0 Ok: queued as 06E67305D927
QUIT
#221 2.0.0 Bye
```
### View the Postfix Logs
You don't have to use tail, use whatever you want, but the logs are usually here. You should see information about the message you sent, and attempting to deliver.
```
sudo tail -f /var/log/maillog
```
Here is an example where I don't have my upstream server on:
```
Oct 24 13:20:46 rhpostfix1 postfix/smtpd[3934]: connect from ubuntu01.com[192.168.0.10]
Oct 24 13:21:36 rhpostfix1 postfix/smtpd[3934]: AD551305D936: client=ubuntu01.com[192.168.0.10]
Oct 24 13:21:43 rhpostfix1 postfix/cleanup[3941]: AD551305D936: prepend: header Received: from ls.local (ubuntu01.com [192.168.0.10])??by rhpostfix1.ls.local (Postfix) with ESMTP id AD551305D936??for <testr@ls.local>; Fri, 24 Oct 2025 13:21:17 -0500 (CDT) from ubuntu01.com[192.168.0.10]; from=<testsender@ls.local> to=<testr@ls.local> proto=ESMTP helo=<ls.local>: AUTH-X: mysecretkey
Oct 24 13:21:43 rhpostfix1 postfix/cleanup[3941]: AD551305D936: message-id=<>
Oct 24 13:21:43 rhpostfix1 postfix/qmgr[3927]: AD551305D936: from=<testsender@ls.local>, size=206, nrcpt=1 (queue active)
Oct 24 13:21:44 rhpostfix1 postfix/smtpd[3934]: disconnect from ubuntu01.com[192.168.0.10] ehlo=1 mail=1 rcpt=1 data=1 quit=1 commands=5
Oct 24 13:21:46 rhpostfix1 postfix/smtp[3942]: connect to 192.168.0.135[192.168.0.135]:25: No route to host
Oct 24 13:21:46 rhpostfix1 postfix/smtp[3942]: AD551305D936: to=<testr@ls.local>, relay=none, delay=29, delays=26/0.02/3.1/0, dsn=4.4.1, status=deferred (connect to 192.168.0.135[192.168.0.135]:25: No route to host)

```
### View Queued Messages
If the messages are still queued, because maybe you don't have an upstream server, you can view the local queue.
```
mailq
```
Here are my current results in my mail queue.
```
lonestar@rhpostfix1:/var/spool/postfix$ mailq
-Queue ID-  --Size-- ----Arrival Time---- -Sender/Recipient-------
06E67305D927     252 Fri Oct 24 13:17:35  testserver@ls.local
                (connect to 192.168.0.135[192.168.0.135]:25: No route to host)
                                         testrecipient@ls.local

AD551305D936     206 Fri Oct 24 13:21:17  testsender@ls.local
                (connect to 192.168.0.135[192.168.0.135]:25: No route to host)
                                         testr@ls.local
```
### Look at the headers of a message in the mail queue
Replace the ID of the message with your own from your "mailq" query.
```
sudo postcat -q -h AD551305D936
```
Here are my results. Notice that it added the header I wanted.
```
lonestar@rhpostfix1:/var/spool/postfix$ sudo postcat -q -h AD551305D936
[sudo] password for lonestar: 
AUTH-X: mysecretkey
Received: from ls.local (ubuntu01 [192.168.0.10])
	by rhpostfix1.ls.local (Postfix) with ESMTP id AD551305D936
	for <testr@ls.local>; Fri, 24 Oct 2025 13:21:17 -0500 (CDT)
```
