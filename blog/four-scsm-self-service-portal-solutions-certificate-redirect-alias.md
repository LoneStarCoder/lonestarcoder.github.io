---
layout: default
permalink: /blog/four-scsm-self-service-portal-solutions-certificate-redirect-alias
---
# Four SCSM Self-Service Portal Solutions (certificate, redirect, alias)
*Author: Brody Kilpatrick* | *Created: July 20, 2011*

When accessing the Self-Service Portal, typing https://servername/enduser into the browser address isn't acceptable for most organizations. This blog posts addresses 4 concerns of the self-service portal and how to resolve them. The information may be common to many IT Professionals; however, it was not common to me, as I have rarely been required to set up IIS sites in the past. I must also mention and thank the source for most of this information, **Thomas Bianco,** who has challenged me with (I think uncommon) Service Manager questions and answers.  
  
1. Creating a DNS entry for your Self-Service portal  
2. Properly creating a certificate to access the Self-Service Portal  
3. Automatically redirecting to the end-user portal when typing in that alias  
4.  Preventing the need to type in ***https*** in front of the site name  
  
For each of these steps, we are going to assume the following:  
Self-Service Portal Server Name: SCSM01  
Website Name we want to use: ServiceManager.domain.local  
  
  

**1. Creating a DNS entry for your Self-Service portal**

1. Log into DNS for your Active Directory Domain
2. Go to Forward Lookup Zones
3. Select you domain
4. Right click on your domain
5. Select New Alias (CNAME)
6. In the Alias Name box type "ServiceManager" (Without the quotes)
7. In the Fully Qualified Domain Box Type "ServiceManager.domain.local" (Without the quotes)
8. Click Ok
9. To test, Browse to https://servicemanager/enduser
10. You will still receive a certificate error, but should be able to click on to the site

  

**2. Properly creating a certificate to access the Self-Service Portal**

1. Log onto you SCSM Self-Service Portal Server
2. Start > Run > MMC > File > Add/Remove Snap-in
3. In the list of snap-ins, select Certificates
4. Click Add
5. A window should pop up stating "This snap-in will always maange certificates for:"
6. Select Computer Account > Next
7. Select "Local Computer" > Next
8. Finish
9. Ok
10. Expand Certificate (Local Computer) > Personal > Certificates
11. Right Click > Request New Certificate
12. Select the Certificate Enrollment Policy
13. Next
14. On the enrollment policy click the link labeled *"More information is required to enroll for this certificate. Click here to configure settings."*
15. On the Subject tab, on the dropdown box under *Type:* Populate each field and add them as necessary
16. Under Alternative name, under *Type:*  Select DNS
17. Type in all the names that could be used to access the SSP. For example servicemanager, servicemanager.domain.local, SCSM01, SCSM01.domain.local
18. Click Add
19. Select the General Tab and populate the fields
20. Select the Private Key Tab > Key Options Check the box "Make private key exportable
21. Select the Certificate Authority Tab
22. Select the correct certificate authority for your organization
23. OK
24. Back on the Certificate Enrollment Window check the "Web Server" box
25. Click Enroll

**Update the SSP with the new certificate**

1. Go to IIS Manager
2. Select SCSMPortal
3. Select Bindings
4. On the HTTPS binding, click edit
5. In the SSL certificate box, select you new certificate
6. Click OK
7. Click Close

The Certificate Error on the Self-Service Portal should no longer exist.

  

**3. Automatically redirecting to the end-user portal when typing in that alias**

1. Open IIS Manager
2. Select SCSMPortal
3. Double Click HTTP Redirect (If HTTP Redirect in not installed, go to Roles, Add Features, Select HTTP Redirect)
4. Check the box "Redirect requests to this destination"
5. Type in "enduser\" (Without the quotes)
6. Under redirect behavior make sure "Redirect all requests to exact destination" is UNCHECKED
7. Under redirect behavior make sure "Only redirect requests to content in the directory" is CHECKED
8. In the Status Code Select "Found (302)"
9. Click Apply
10. Go to the Analyst and enduser vitual directories under SCSMPortal
11. Select HTTP Redirect
12. Make sure "redirect requests to this destination" is UNCHECKED
13. To Test type in https://servicemanager
14. You should automatically be redirected to https://servicemanager/enduser

  

**4.  Preventing the need to type in *https*in front of the site name**

**From Thomas:**

> *[A Client] had a requirement to silently redirect users from their HTTP support site to the new HTTPS. It took me a few hours, but I found out an easy way to do this without redirecting to an absolute path that would break outside access through the firewall. First, I’m using the same redirect we worked out at [Client], then adding in the URL rewrite add-in from <http://www.iis.net/download/URLRewrite>. The HTTP redirect moves users who hit the website root down to the enduser virtual directory, and the rewrite directives moves connections from HTTP to HTTPS. The only downside is that people going to the root with HTTP get redirected twice, which takes about 1-4 seconds...Feel free to steal this for your blog.*

  

Here's how:

1. Open IIS Manager
2. Select SCSMPortal
3. Select bindings
4. Click Add
5. Type: http
6. IP address: All unassigned
7. Port: 80
8. Host Name: Leave Blank
9. Click OK
10. If you get a warning about Port 80 being using on the default website, you may want to stop the default website or remove the binding from the default website
11. Click Close
12. Install the URL Rewrite utility from <http://www.iis.net/download/URLRewrite>
13. Browse to the directory of the SCSM Portal
14. Backup the web.config file
15. Open the Web.config file
16. Paste in the following and save the web.config file:

> *&lt;?xml version="1.0" encoding="UTF-8"?&gt;*
>
> *&lt;configuration&gt;*
>
>  *&lt;system.webServer&gt;*
>
>  *&lt;httpRedirect enabled="true" destination="enduser/" childOnly="true" /&gt;*
>
>  *&lt;rewrite&gt;*
>
>  *&lt;rules&gt;*
>
>  *&lt;rule name="HTTP to HTTPS redirect" stopProcessing="true"&gt;*
>
>  *&lt;match url="(.\*)" /&gt; &lt;!-- Require SSL must be OFF in the site settings --&gt;*
>
>  *&lt;conditions&gt;*
>
>  *&lt;add input="{HTTPS}" pattern="off" ignoreCase="true" /&gt;*
>
>  *&lt;/conditions&gt;*
>
>  *&lt;action type="Redirect" redirectType="Found" url="[https://{HTTP\_HOST}{REQUEST\_URI}](https://%7Bhttp_host%7D%7Brequest_uri%7D/)" /&gt;*
>
>  *&lt;/rule&gt;*
>
>  *&lt;/rules&gt;*
>
>  *&lt;/rewrite&gt;*
>
>  *&lt;/system.webServer&gt;*
>
> *&lt;/configuration&gt;*

  
To test, simply type servicemanager into your browser. You should be redirected.
