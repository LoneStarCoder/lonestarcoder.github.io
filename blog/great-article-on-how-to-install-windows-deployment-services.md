---
layout: default
permalink: /blog/great-article-on-how-to-install-windows-deployment-services
---
# Great Article on How to Install Windows Deployment Services
*Author: Brody Kilpatrick* | *Created: March 17, 2011*

<http://www.windows-noob.com/forums/index.php?/topic/446-how-can-i-setup-wds-in-windows-server-2008/>  
  
  
Posted 05 May 2008 - 12:36 PM  
Note: If you already have an Active Directory setup and configured with DNS then skip to Step 2 (Setup DHCP). If you have already configured AD, DNS and DHCP then please skip to Step 3 (Add the WDS role).  
  
  
  
This guide assumes that you have first of all installed Windows 2008 server on your machine and partitioned it as c: (Operating system) and d: (data), note that you can change the partitions after the installation of Windows 2008 server by using disk management and right clicking on a disk and choosing to shrink or extend.  
  
Resized to 95% (was 1108 x 511) - Click image to enlarge  
  
  
  
Step 1. Setup and configure active directory  
  
Click on Start and choose Server Manager, scroll down to Roles Summary and choose Add Roles.   
  
When the wizard appears click next.  
  
  
  
  
  
  
As WDS needs the folllowing:-  
  
Active Directory Domain Services, DHCP, DNS, we will add the following role first:-  
  
Active Directory Domain Services  
  
We must install this role first before continuing to add the other roles, so lets select it and click on next.   
  
  
  
We will then be informed that installing AD requires us to run dcpromo.exe afterwards to make the server a fully functional domain controller. Click install to continue.  
  
  
  
once done we'll get the success screen  
  
  
  
click close to continue  
  
Once done the server manager will list our ADDS service with a red X beside it, click on it to run Dcpromo.exe.  
  
  
  
This will open up the Roles section of Server manager, and we'll see a summary which says This server is not yet running as a domain controller. Run the Active Directory Domain Services Installation Wizard (dcpromo.exe).  
  
Click on the blue text to continue.  
  
  
  
This will start the Active Directory Domain Services Installation Wizard. Click on next to continue.  
  
  
  
Next we will get a screen telling us that older versions of Windows (pre Vista Sp1 ....) may have problems with a bunch of things including Windows Deployment Services (for more info read KB942564)  
  
Quote  
Platforms impacted by this change include Windows NT 4.0, as well as non-Microsoft SMB "clients" and network-attached storage (NAS) devices that do not support stronger cryptography algorithms. Some operations on clients running versions of Windows earlier than Vista with Service Pack 1 are also impacted, including domain join operations performed by the Active Directory Migration Tool or Windows Deployment Services.  
  
  
  
  
click next to continue  
  
Now we get to choose what type of domain we will setup, and for the purpose of this lab, we will select the second option, Create a new domain in a new forest.  
  
  
  
next we are prompted for the fully qualified domain name of the forest root domain (eg: corp.contoso.com)  
  
  
  
after clicking next the wizard will check the validity of the fqdn,   
  
next we get to choose the forest functional level, click the drop down menu and select Windows Server 2008 from the three choices (Windows 2000, Windows Server 2003, Windows Server 2008).  
As this is only a test lab, we are not concerned that we can only add Windows 2008 or later servers to this forest.  
  
  
  
Clicking next will give some addtional options, leave them as they are (DNS Server selected) and click on Next again.  
  
  
  
if you get a warning about dynamically assigned ip addresses ignore it (choose yes),   
  
  
  
you will most likely get another DNS warning if like me you don't have a Windows DNS server in the forest. Click yes to continue.  
  
  
  
Next we are asked where to store the AD database, logfiles and sysvol, stay with the defaults and click next.  
  
  
  
  
and we are then prompted to set the Directory Services restore mode administrator account password  
  
  
  
finally we get a summary of our actions  
  
  
  
click next to get started  
  
  
  
after a while you should see the AD wizard complete. Click Finish  
  
  
  
you'll be prompted to restart, so please go ahead and click on Restart Now.  
  
  
Microsoft MVP 2010 - ConfigMgr  
My linkedin profile at > linkedin.com  
Follow me on Twitter > ncbrady  
Follow windowsnoob.com on Twitter > windowsnoob  
My blog on myITforum  
0  
#2 anyweb   
Administrator  
  
Group:Root Admin  
Posts:3,228  
Joined:28-September 06  
Gender:Male  
Location:Sweden  
Interests:Deploying Operating systems and more with System Center Configuration Manager  
Posted 06 May 2008 - 07:18 PM  
Step 2. Add the DHCP role  
  
After completing Step 1, you now should see that Active Directory Domain Services and DNS server are installed (you can verify this in Server Manager, Roles, Roles Summary).  
  
  
  
Now that we are in server manager lets click on Add roles again followed by next.   
  
  
Select DHCP Server and click next.  
  
  
  
  
  
Review the information about DHCP server...  
  
  
  
  
and if you have more than one network card in your Windows 2008 server, select the one which will be handing out ip's (in my case that is 192.168.3.1), I removed the tick from the other network card listed  
  
  
  
next you will need to clarify which DNS server ip address to listen to, I changed it from my second nic's ip address 192.168.3.1, clicking on Validate will verify that it's ok  
  
  
  
enter your WINS settings, I went with the default and clicked next,  
  
  
  
next we have to input our DHCP server scope options, to do this click on Add  
  
  
  
  
enter your scope settings (mine are listed below)  
  
  
  
click ok and your DHCP scope will now be listed  
  
  
  
click next and you'll get IPV6 options, I chose to disable this as I'm not using IPv6 yet  
  
Quote  
Windows Server® 2008 supports stateless and stateful DHCPv6 server functionality. DHCPv6 stateless mode clients use DHCPv6 to obtain network configuration parameters other than the IPv6 address, such as DNS server addresses. Clients configure an IPv6 address through a non-DHCPv6 based mechanism such as IPv6 address auto-configuration (based on the IPv6 prefixes included in router advertisements), or static IP address configuration.  
  
In DHCPv6 stateful mode, clients acquire both the IPv6 address as well as other network configuration parameters through DHCPv6.  
  
  
  
  
next we are asked about DHCP credentials for the AD DS, I stayed with the default,  
  
  
  
and then we will see a summary of our actions and choices for the DHCP server role  
  
  
  
clicking on Install will apply these settings and after some time you should hopefully see the following:-  
  
  
  
  
  
Step 3. Add the WDS role:-  
  
In Server Manager, Highlight and select Windows Deployment Services and click next.  
  
  
  
you will get an information screen which has some info including the following:-  
  
Quote  
Before you begin, you need to configure Windows Deployment Services by running either the Windows Deployment Services Configuration Wizard or WDSUtil.exe. You will also need to add at least one boot image and one install image in the image store.  
  
  
  
  
Click next and notice the two role services listed, Deployment Server and Transport Server, make sure they are both selected and click next to continue.  
  
  
  
  
view the summary and click install to install the WDS role...  
  
  
  
  
after some copying you should see this  
  
  
  
if you check server manager under roles you should now see the WDS role added.  
  
  
Microsoft MVP 2010 - ConfigMgr  
My linkedin profile at > linkedin.com  
Follow me on Twitter > ncbrady  
Follow windowsnoob.com on Twitter > windowsnoob  
My blog on myITforum  
0  
#3 anyweb   
Administrator  
  
Group:Root Admin  
Posts:3,228  
Joined:28-September 06  
Gender:Male  
Location:Sweden  
Interests:Deploying Operating systems and more with System Center Configuration Manager  
Posted 06 May 2008 - 07:51 PM  
Step 4. Configure the Windows Deployment Services gui (mmc snap in)  
  
Click on Start/All Programs/Administrative tools/Windows Deployment Services.  
  
at this point we can see that WDS is not configured yet, so let's do that now.  
  
  
  
right click on the server name in the left pane and choose Configure Server  
  
  
  
  
at the welcome page click next  
  
  
  
change the remoteinstall path from the default C:\RemoteInstall to D:\RemoteInstall  
  
  
  
I put a checkmark in each of the DHCP options then clicked next  
  
  
  
I then chose to respond to all known and unknown computers (by default it's set to Do not respond to any)  
  
Please note, if you want to set this option to only respond to known computers, then you can do so, but you will have to prestage your computers in Active Directory to do so  
  
  
  
clicking on Finish applies these settings  
  
when done, you'll be told that the configuration is complete and that you can now add images to the WDS server, click on Finish (again).  
  
  
  
  
  
Step 5. Adding boot.wim and install.wim  
  
Note: You should use only the boot.wim file from the Windows Server 2008 DVD. If you use the boot.wim file from the Windows Vista DVD, you will not be able to use the full functionality of Windows Deployment Services for example, multicasting. If you have the Windows Vista SP1 dvd, you can safely use that for the boot.wim.  
  
  
  
The Windows Deployment Add image wizard will appear, insert your Windows 2008 Server DVD and click Browse, select the sources folder on the Windows 2008 DVD and then click next  
  
  
  
you'll be prompted to create a new image group, lets call it ImageGroup1 (the default name, you can change it later to Windows Server 2008 or Windows Vista Sp1 or whatever...).   
  
  
  
  
review your settings and click next when ready  
  
  
  
After a long while, the selected images will be added to the WDS server  
  
  
  
click finish and review the WDS server as it is now  
  
in the Boot Images pane, you should see Microsoft Windows Longhorn Setup (x86) and this is the original boot.wim file from the Windows 2008 Server DVD, please note that this is the default description name of the image, you could change it when addint the boot.wim image to something more descriptive as in this example  
  
  
  
In the Install Images pane, we can see the six available images from the Windows 2008 Server DVD, these are based upon the install.wim file on the DVD.  
  
  
  
you can now PXE boot your client computers to the Windows 2008 WDS server.  
  
if you want to see more free articles like this then please DIGG it to show your thanks  
  
troubleshooting note: if you add a new image to WDS and attempt to pxe boot and then install the image but get an error saying something like 'could not display the list of' then make sure you have used BOOT.WIM from a Windows Server 2008 DVD or Windows Vista sp1.   
Microsoft MVP 2010 - ConfigMgr  
My linkedin profile at > linkedin.com  
Follow me on Twitter > ncbrady  
Follow windowsnoob.com on Twitter > windowsnoob  
My blog on myITforum  
0  
#4 ramlan   
Newbie  
  
Group:Members  
Posts:6  
Joined:21-January 09  
Posted 27 January 2009 - 08:21 PM  
Hello,  
  
I am new to SMS or SCCM 2007. I have DSL router acting as DHCP and couple of client machines have dynamic ip address from the router. Do, I still need to install DHCP on DC to continue with the install of SCCM 2007. Look forward to your reply.  
  
My Lab setup - 4 laptops (P4 2.2ghz, 1GB Mem, 40GB HD) and 1 virtual machines on vmware workstation 6.0.3. One laptop will be DC with SCCM 2007 and the rest 3 laptops & 1 VM will act as client machines.  
  
Tks/Ram   
0  
#5 anyweb   
Administrator  
  
Group:Root Admin  
Posts:3,228  
Joined:28-September 06  
Gender:Male  
Location:Sweden  
Interests:Deploying Operating systems and more with System Center Configuration Manager  
Posted 27 January 2009 - 09:09 PM  
hello and welcome !  
  
Quote  
I am new to SMS or SCCM 2007. I have DSL router acting as DHCP and couple of client machines have dynamic ip address from the router. Do, I still need to install DHCP on DC to continue with the install of SCCM 2007. Look forward to your reply.  
  
  
you should enable dhcp on the DC, the dhcp on your router will not be sufficient.  
  
please post these questions as new topics in the SCCM section  
  
cheers  
anyweb   
Microsoft MVP 2010 - ConfigMgr  
My linkedin profile at > linkedin.com  
Follow me on Twitter > ncbrady  
Follow windowsnoob.com on Twitter > windowsnoob  
My blog on myITforum  
0  
#6 joelmcmillin   
Newbie  
  
Group:Members  
Posts:1  
Joined:21-July 09  
Posted 21 July 2009 - 12:11 PM  
Does WDS have to be installed on the DC?   
I have only W2K3 DCs and would like to install W2K8 and WDS on a separate server. Is this possible?   
0  
#7 anyweb   
Administrator  
  
Group:Root Admin  
Posts:3,228  
Joined:28-September 06  
Gender:Male  
Location:Sweden  
Interests:Deploying Operating systems and more with System Center Configuration Manager  
Posted 22 July 2009 - 08:00 AM  
yes of course it's possible, it was only installed on a DC in this example because this is a test lab   
Microsoft MVP 2010 - ConfigMgr  
My linkedin profile at > linkedin.com  
Follow me on Twitter > ncbrady  
Follow windowsnoob.com on Twitter > windowsnoob  
My blog on myITforum  
0  
#8 d0zer   
Member  
  
Group:Members  
Posts:13  
Joined:27-August 09  
Posted 17 September 2009 - 01:59 PM  
Hello  
  
  
our DHCP Server is a Cisco Server.  
I have to specify the Path/File name for the PXE-Boot File.  
  
What PXE Boot File have i to choose, so that i can boot x86 and x64 with PXE?  
  
i have found the "pxeboot.n12" but it is available in the x86 and also in the x64 Folder on D:\RemoteInstall\Boot\  
  
  
i hope, that you understand what i mean.  
  
  
thank you   
0  
#9 RGB09   
Advanced Member  
  
Group:Members  
Posts:94  
Joined:10-June 09  
Gender:Male  
Location:England  
Interests:Computers and games Level designing  
Posted 14 November 2009 - 10:08 AM  
Just to confirm, the network at work already has servers that run DHCP, DNS and AD do I still need to add those to the WDS 2008 server???  
I thought I could just add the WDS to the server2008 I am using and away it goes, I never had this problem with Server 2003?   
0  
#10 Malik4u   
Advanced Member  
  
Group:Members  
Posts:33  
Joined:19-November 09  
Posted 27 November 2009 - 06:32 PM  
Hi  
  
I am following this guide by you.... i have CISCO swich, it is not configured, let me know which services i am supposed to have on my switch ? What IP i should use for the ethernet ports ? If i have to use the static IP then it should be same as my NIC ip?  
  
  
  
anyweb, on 06 May 2008 - 07:51 PM, said:  
Step 4. Configure the Windows Deployment Services gui (mmc snap in)  
  
Click on Start/All Programs/Administrative tools/Windows Deployment Services.  
  
at this point we can see that WDS is not configured yet, so let's do that now.  
  
wds\_not\_configured.jpg  
  
right click on the server name in the left pane and choose Configure Server  
  
  
wds\_configure.jpg  
  
at the welcome page click next  
  
welcome\_page.jpg  
  
change the remoteinstall path from the default C:\RemoteInstall to D:\RemoteInstall  
  
remoteinstall\_path.jpg  
  
I put a checkmark in each of the DHCP options then clicked next  
  
dhcp\_options.jpg  
  
I then chose to respond to all known and unknown computers (by default it's set to Do not respond to any)  
  
Please note, if you want to set this option to only respond to known computers, then you can do so, but you will have to prestage your computers in Active Directory to do so  
  
pxe\_options.jpg  
  
clicking on Finish applies these settings  
  
when done, you'll be told that the configuration is complete and that you can now add images to the WDS server, click on Finish (again).  
  
config\_complete.jpg  
  
  
  
Step 5. Adding boot.wim and install.wim  
  
Note: You should use only the boot.wim file from the Windows Server 2008 DVD. If you use the boot.wim file from the Windows Vista DVD, you will not be able to use the full functionality of Windows Deployment Services for example, multicasting. If you have the Windows Vista SP1 dvd, you can safely use that for the boot.wim.  
  
  
  
The Windows Deployment Add image wizard will appear, insert your Windows 2008 Server DVD and click Browse, select the sources folder on the Windows 2008 DVD and then click next  
  
add\_boot\_wim.jpg  
  
you'll be prompted to create a new image group, lets call it ImageGroup1 (the default name, you can change it later to Windows Server 2008 or Windows Vista Sp1 or whatever...).   
  
  
image\_group.jpg  
  
review your settings and click next when ready  
  
review.jpg  
  
After a long while, the selected images will be added to the WDS server  
  
images\_added.jpg  
  
click finish and review the WDS server as it is now  
  
in the Boot Images pane, you should see Microsoft Windows Longhorn Setup (x86) and this is the original boot.wim file from the Windows 2008 Server DVD, please note that this is the default description name of the image, you could change it when addint the boot.wim image to something more descriptive as in this example  
  
boot\_images.jpg  
  
In the Install Images pane, we can see the six available images from the Windows 2008 Server DVD, these are based upon the install.wim file on the DVD.  
  
install\_images.jpg  
  
you can now PXE boot your client computers to the Windows 2008 WDS server.  
  
if you want to see more free articles like this then please DIGG it to show your thanks  
  
troubleshooting note: if you add a new image to WDS and attempt to pxe boot and then install the image but get an error saying something like 'could not display the list of' then make sure you have used BOOT.WIM from a Windows Server 2008 DVD or Windows Vista sp1.
