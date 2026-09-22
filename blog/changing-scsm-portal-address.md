---
layout: default
permalink: /blog/changing-scsm-portal-address
---
# Changing SCSM Portal Address
*Author: Brody Kilpatrick* | *Created: July 11, 2011*

Great article on changing the SCSM portal Address  
<http://memoexp.wordpress.com/2011/03/30/changing-scsm-portal-address/>  
  
  
  

Ever have a customer request that they want the Self Service Portal to be a specific address, different from the one you’ve initially setup? Well, its not possible to change the URL. However what you can do is set up a CNAME in your DNS to accomplish this goal. Here is how to do it.

1. Go to the DC. Open up DNS under Administrative Tools. Under the DC, expand **Forward Lookup Zones**, then right click the folder with the domain name, select **New Alias (CNAME)**.

[![image](http://memoexp.files.wordpress.com/2011/03/image_thumb93.png?w=309&h=326 "image")](http://memoexp.files.wordpress.com/2011/03/image93.png)

2. Under Alias Name, type in the portal URL that you want. Then under target host, drill down and browse to your portal host name, which in my case is SCSM. Click **Ok**.

[![image](http://memoexp.files.wordpress.com/2011/03/image_thumb94.png?w=409&h=453 "image")](http://memoexp.files.wordpress.com/2011/03/image94.png)

3. Go to the server which the Portal is installed. Launch **IIS Manager**, go to the **Portal Sites**, in my case, **SCSMPortal**, click on**Bindings** on the right panel.

[![image](http://memoexp.files.wordpress.com/2011/03/image_thumb95.png?w=622&h=275 "image")](http://memoexp.files.wordpress.com/2011/03/image95.png)

4. Click **Add**.

[![image](http://memoexp.files.wordpress.com/2011/03/image_thumb96.png?w=244&h=115 "image")](http://memoexp.files.wordpress.com/2011/03/image96.png)

5. Select **Type** as **https**, **IP address** as **All Unassigned**, **Port** as **443**. Then select the **SSL certificate**. Click **Ok**.

[![image](http://memoexp.files.wordpress.com/2011/03/image_thumb97.png?w=244&h=132 "image")](http://memoexp.files.wordpress.com/2011/03/image97.png)

6. While highlighting your portal on the left pane, in my case **SCSMPortal**, Double click **HTTP Redirect**.

**Note!** HTTP Redirect is not installed by default for IIS7. To install it, go to **Server Manager** > **Roles** > **Web Server (IIS)**, look for **Add Role Services**, you’ll find that you will have the option to install **HTTP Redirection**.

[![image](http://memoexp.files.wordpress.com/2011/03/image_thumb98.png?w=604&h=439 "image")](http://memoexp.files.wordpress.com/2011/03/image98.png)

7. **Check** Redirect requests to this destination. Key in the full address of the Portal Site, click **Apply** on the right pane. If you’re not sure, its **https://** + **Full Computer Name** + **/enduser**. So for my case it is https://scsm.systemcenter.local/enduser

(To get your Full Computer Name, Right click My Computer > Properties. You’ll see it there.)

[![image](http://memoexp.files.wordpress.com/2011/03/image_thumb99.png?w=604&h=179 "image")](http://memoexp.files.wordpress.com/2011/03/image99.png)

8. Browse to the new URL which in my case is **[https://servicemanager](https://servicemanager/)**, the portal should launch without problems.

P/s If you’re getting password prompts when using the new URL, then highlight the portal on the left pane, and double click on**Authentication**. Select **Basic Authentication** and click on **Enable** on the right pane. Try using the new URL again, you shouldn’t get any prompts now.

[![image](http://memoexp.files.wordpress.com/2011/03/image_thumb100.png?w=604&h=120 "image")](http://memoexp.files.wordpress.com/2011/03/image100.png)

[March 30, 2011](http://memoexp.wordpress.com/2011/03/30/) - Posted by [James](http://memoexp.wordpress.com/author/rain2418/ "Posts by James") | [System Center Service Manager (SCSM)](http://en.wordpress.com/tag/system-center-service-manager-scsm/ "View all posts in System Center Service Manager (SCSM)")
