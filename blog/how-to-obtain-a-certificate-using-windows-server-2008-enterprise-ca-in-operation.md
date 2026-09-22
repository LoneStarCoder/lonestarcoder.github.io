---
layout: default
permalink: /blog/how-to-obtain-a-certificate-using-windows-server-2008-enterprise-ca-in-operation
---
# How to Obtain a Certificate Using Windows Server 2008 Enterprise CA in Operations Manager 2007
*Author: Brody Kilpatrick* | *Created: March 14, 2011*

<http://technet.microsoft.com/en-us/library/dd362553.aspx>  
  
  
  
  
  
How to Obtain a Certificate Using Windows Server 2008 Enterprise CA in Operations Manager 2007  
Updated: May 22, 2009  
  
Applies To: Operations Manager 2007 R2, Operations Manager 2007 SP1  
  
Use the procedures in this topic to obtain a certificate from Windows Server 2008 computer hosting Enterprise Root Active Directory Certificate Services (AD CS). You will use the CertReq command-line utility to request and accept a certificate, and you will use a Web interface to submit and retrieve your certificate.  
  
It is assumed that you have AD CS installed, an HTTPS binding has been created, and its associated certificate has been installed. Information about creating an HTTPS binding is available in the topic How to Configure an HTTPS Binding for a Windows Server 2008 CA.  
  
Important  
The content for this topic is based on the default settings for Windows Server 2008 AD CS; for example, setting the key length to 2048, selecting Microsoft Software Key Storage Provider as the CSP, and using Secure Hash Algorithm 1 (SHA1). Evaluate these selections against the requirements of your company’s security policy.  
The high-level process to obtain a certificate from an Enterprise certification authority (CA) is as follows:  
  
Download the Trusted Root (CA) certificate.  
  
Import the Trusted Root (CA) certificate.  
  
Create a certificate template.  
  
Add the template to the Certificate Templates folder.  
  
Create a setup information file for use with the CertReq command-line utility.  
  
Create a request file.  
  
Submit a request to the CA.  
  
Import the certificate into the certificate store.  
  
Import the certificate into Operations Manager using MOMCertImport.  
  
To download the Trusted Root (CA) certificate  
  
Log on to the computer where you installed a certificate; for example, the gateway server or management server.  
  
Start Internet Explorer, and connect to the computer hosting Certificate Services; for example, https://&lt;servername&gt;/certsrv.  
  
On the Welcome page, click Download a CA Certificate, certificate chain, or CRL.  
  
On the Download a CA Certificate, Certificate Chain, or CRL page, click Encoding method, click Base 64, and then click Download CA certificate chain.  
  
In the File Download dialog box, click Save and save the certificate; for example, Trustedca.p7b.  
  
When the download has finished, close Internet Explorer.  
  
To import the Trusted Root (CA) Certificate  
  
On the Windows desktop, click Start, and then click Run.  
  
In the Run dialog box, type mmc, and then click OK.  
  
In the Console1 window, click File, and then click Add/Remove Snap-in.  
  
In the Add/Remove Snap-in dialog box, click Add.  
  
In the Add Standalone Snap-in dialog box, click Certificates, and then click Add.  
  
In the Certificates snap-in dialog box, select Computer account, and then click Next.  
  
In the Select Computer dialog box, ensure that Local computer: (the computer this console is running on) is selected, and then click Finish.  
  
In the Add Standalone Snap-in dialog box, click Close.  
  
In the Add/Remove Snap-in dialog box, click OK.  
  
In the Console1 window, expand Certificates (Local Computer), expand Trusted Root Certification Authorities, and then click Certificates.  
  
Right-click Certificates, select All Tasks, and then click Import.  
  
In the Certificate Import Wizard, click Next.  
  
On the File to Import page, click Browse and select the location where you downloaded the CA certificate file, for example, TrustedCA.p7b, select the file, and then click Open.  
  
On the File to Import page, select Place all certificates in the following store and ensure that Trusted Root Certification Authorities appears in the Certificate store box, and then click Next.  
  
On the Completing the Certificate Import Wizard page, click Finish.  
  
To create a certificate template  
  
On the computer that is hosting your enterprise CA, on the Windows desktop, click Start, point to Programs, point to Administrative Tools, and then click Certification Authority.  
  
In the navigation pane, expand the CA name, right-click Certificate Templates, and then click Manage.  
  
In the Certificate Templates console, in the results pane, right-click IPSec (Offline request), and then click Duplicate Template.  
  
In the Duplicate Template dialog box, select Windows Server 2003 Enterprise Edition, and then click OK.  
  
Note  
The option for Windows Server 2008 Enterprise Edition is not supported at this time.  
In the Properties of New Template dialog box, on the General tab, in the Template display name text box, type a new name for this template; for example, OperationsManagerCert.  
  
On the Request Handling tab, select Allow private key to be exported.  
  
Click the Extensions tab, and in Extensions included in this template, click Application Policies, and then click Edit.  
  
In the Edit Application Policies Extension dialog box, click IP security IKE intermediate, and then click Remove.  
  
Click Add, and in the Application policies list, hold down the CTRL key to multi-select items from the list, click Client Authentication and Server Authentication, and then click OK.  
  
In the Edit Application Policies Extension dialog box, click OK.  
  
Click the Security tab and ensure that the Authenticated Users group has Read and Enroll permissions, and then click OK.  
  
Close the Certificate Templates console.  
  
To add the template to the Certificate Templates folder  
  
On the computer that is hosting your Enterprise CA, in the Certification Authority snap-in, right-click the Certificate Templates folder, point to New, and then click Certification Template to Issue.  
  
In the Enable Certificate Templates box, select the certificate template that you created; for example, click OperationsManagerCert, and then click OK.  
  
To create a setup information (.inf) file  
  
On the computer hosting the Operations Manager component for which you are requesting a certificate, click Start, and then click Run.  
  
In the Run dialog box, type Notepad, and then click OK.  
  
Create a text file containing the following content:  
  
[NewRequest]  
  
Subject="CN=&lt;FQDN of computer you are creating the certificate, for example, the gateway server or management server.&gt;"  
  
Exportable=TRUE  
  
KeyLength=2048  
  
KeySpec=1  
  
KeyUsage=0xf0  
  
MachineKeySet=TRUE  
  
[EnhancedKeyUsageExtension]  
  
OID=1.3.6.1.5.5.7.3.1  
  
OID=1.3.6.1.5.5.7.3.2  
  
Save the file with an .inf file name extension; for example, RequestConfig.inf.  
  
Close Notepad.  
  
To create a request file to use with an enterprise CA  
  
On the computer hosting the Operations Manager component for which you are requesting a certificate, click Start, and then click Run.  
  
In the Run dialog box, type cmd, and then click OK.  
  
In the command window, type CertReq –New –f RequestConfig.inf CertRequest.req, and then press ENTER.  
  
Using Notepad, open the resulting file (for example, CertRequest.req), and copy the contents of this file into the clipboard.  
  
To submit a request to an enterprise CA  
  
On the computer hosting the Operations Manager component for which you are requesting a certificate, start Internet Explorer, and then connect to the computer hosting Certificate Services; for example, https://&lt;servername&gt;/certsrv.  
  
Note  
If an HTTPS binding has not been configured on the Certificate Services Web site, the browser will fail to connect. See the topic How to Configure an HTTPS Binding for a Windows Server 2008 CA in this guide.  
On the Microsoft Active Directory Certificate Services Welcome screen, click Request a certificate.  
  
On the Request a Certificate page, click advanced certificate request.  
  
On the Advanced Certificate Request page, click Submit a certificate request by using a base-64-encoded CMC or PKCS #10 file, or submit a renewal request by using a base-64-encoded PKCS #7 file.  
  
On the Submit a Certificate Request or Renewal Request page, in the Saved Request text box, paste the contents of the CertRequest.req file that you copied in step 4 in the previous procedure.  
  
In the Certificate Template select the certificate template that you created, for example, OperationsManagerCert, and then click Submit.  
  
On the Certificate Issued page, select Base 64 encoded, and then click Download certificate.  
  
In the File Download – Security Warning dialog box, click Save, and save the certificate; for example, save as NewCertificate.cer.  
  
Close Internet Explorer.  
  
To import the certificate into the certificate store  
  
On the computer hosting the Operations Manager component for which you are configuring the certificate, click Start, and then click Run.  
  
In the Run dialog box, type cmd, and then click OK.  
  
In the command window, type CertReq –Accept NewCertifiate.cer, and then press ENTER.  
  
To import the certificate into Operations Manager using MOMCertImport  
  
Log on to the computer where you installed the certificate with an account that is a member of the Administrators group.  
  
On the Windows desktop, click Start, and then click Run.  
  
In the Run dialog box, type cmd, and then click OK.  
  
At the command prompt, type &lt;drive\_letter&gt;: (where &lt;drive\_letter&gt; is the drive where the Operations Manager 2007 installation media is located), and then press ENTER.  
  
Type cd\SupportTools\i386, and then press ENTER.  
  
Note  
On 64-bit computers, type cd\SupportTools\amd64  
Type the following:  
  
MOMCertImport /SubjectName &lt;Certificate Subject Name&gt;  
  
Press ENTER.
