---
layout: default
permalink: /blog/enterprise-ca-how-to-create-a-scom-certificate-template
---
# Enterprise CA: How to create a SCOM Certificate template.
*Author: Brody Kilpatrick* | *Created: March 12, 2011*

FROM: <http://thoughtsonopsmgr.blogspot.com/2010/04/enterprise-ca-how-to-create-scom.html>   
  
  
Enterprise CA: How to create a SCOM Certificate template.   
Bumped into this situation some time ago. A customer had a Enterprise CA in place, but no template for SCOM certificates was available:   
  
So it was time for some additional work: creating a SCOM certificate template and adding that very same template to the CA. This posting will be about how to go about it.  
Step 1: Creating the template on the CA server.  
1. Go to Start > type MMC &lt;enter&gt; > File > Add/Remove Snap-in > select Certificate Templates and Certification Authority (local computer) > OK.   
2. Select Certificate Templates, in the Console click with right mouse button on IPSec (Offline request) and select Duplicate Template.   
  
Select Windows Server 2003 Enterprise (this way you know the template is backwards compatible) > OK.   
3. Type a name which describes the template, like SCOM Certificate.   
  
4. Go to the tab Request Handling.     
Set the key size to 1024. This is sufficient and takes less cpu time to process.   
Also checkmark the option Allow private key to be exported.   
  
Click on the button CSPs…   
Select Microsoft Enhanced Cryptographic Provider 1.0, do not remove the option Microsoft RSA SChannel Cryptographic Provider. (Also for compatibility reasons.)   
  
Click OK.   
5. Go to the tab Extensions.   
Select the option Applications Policies and click Edit.   
Remove IP security IKE intermediate, click Add > select Client Authentication > OK > Add > select Server Authentication.   
  
Click OK.   
6. Go to the tab Security.   
Authenticated Users need to have Read access.   
  
Click Apply > OK.   
Beware!   
When running a CA based on Windows Server 2008 R2, additional security settings are needed. Go here and follow the items listed under Step 2: Changing the Security settings of the SCOM Certificate template.   
7. The template is now created:   
  
Step 2: Adding the template to the CA.  
Now the template created in Step 1 needs to be added to the CA. This is done from the same MMC.  
1. In the MMC, go to Certification Authority > collapse this node  > click with right mouse button on Certificate Templates > New > Certificate Template To Issue.   
     
Select the new template (SCOM Certificate) and click OK.   
  
2. Double click on the folder Certificate Templates. All the available templates will be shown, among them the SCOM Certificate template:   
  
3. Close the MMC. Save it when needed.   
Now the new certificate template will be shown and can be used for creating a SCOM certificate.
