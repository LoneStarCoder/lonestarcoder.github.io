---
layout: default
permalink: /blog/enabling-users-to-take-action-from-email-using-web-pages-web-services
---
# Enabling Users to Take Action from Email Using Web Pages/Web Services
*Author: Brody Kilpatrick* | *Created: January 31, 2011*

<http://blogs.technet.com/b/servicemanager/archive/2011/01/25/enabling-users-to-take-action-from-email-using-web-pages-web-services.aspx#3383712>

Travis Wright [MSFT]

25 Jan 2011 2:22 PM

• 4

This is a guest blog post from a Microsoft Consulting Services Senior Consultant – Andreas Rynes.  He has become a Service Manager expert lately and has come up with a really innovative solution I wanted to share with you all.  The scenario that Andreas writes about in this blog post is a way to enable an end user to receive an email notification when his incident is resolved and then to click a link to either reactivate or close the incident.  The link passes the work item ID on the query string and the web page the user is directed to by clicking on the link either updates the incident status to closed or active depending on the link the user clicked.  The exciting thing about this is not just this particular solution but the pattern which can be followed to enable many other types of scenarios for SCSM.  Some other scenarios I could see working with this pattern are: change request approval/reject, going to a web page to provide additional information about an incident, viewing the status of a change request, etc.

This solution is similar to some of these that we have already published if you are interested in looking at other examples of doing this kind of thing:

Incident Resolution Satisfaction Surveys on SharePoint

Automatically Sending Notifications to Reviewers

Inserting links to Review/Manual Activities in notifications

Thanks for writing this up Andreas and sharing with the community!

====================================================================================================================

During a SCSM session with my customer, while we gathering requirements for their implementation of SCSM, they asked me for the following requirement: whenever an incident is resolved by an analyst the affected end user should be notified with an email. This is nothing special and can be done easily with SCSM workflow features out of the box. So I thought: “yes, of course”, but then they continue with their requirements, that this email should include an option to close or reactivate an incident. The idea behind that requirement is that the end user is the one that can decide if the topic described within the incident was resolved or needs further attention by an analyst.

So the solution is not that complex, but it needs some very different parts to fulfill the requirement. First you need to use the SDK from Service Manager to define methods to close and activate an incident, the second part is the mail template, which can be defined within the Service Manager console and the third part is the workflow that triggers the notification, also managed within the console.

Part I – Visual Studio

1) First I’ve created a solution within Visual Studio. I’ve decided to create a Web Service to implement the functionality for the simple reason that a web service gives you the flexibility to use it both from a web and rich client application.

Start Visual Studio, click on File – New – Web Site … and choose ASP.NET Web Service and give it a meaningful name like SCSMIncidentService

VS creates a template for a asmx web service with three important files called service.cs, service.asmx and a web.config file. I won’t go into detail about SOA architecture or web services and how to implement that within Visual Studio. So I just mention all the details needed for this specific solution.

1) Now go on and remove the already generated first web method “HelloWorld” and implement the SCSM functionality.

2) After that you need to add references to the SDK, for that switch to the Solution Explorer, click on the right mouse button at the project and choose Add Referenceand go to the tab Browse and go to the Service Manager installation folder (normally on C:\Program Files\Microsoft System Center\Service Manager 2010\SDK Binaries and choose all three files there.

3) Then add these using statements in the service.cs file by adding the following lines at the very top:

using Microsoft.EnterpriseManagement;

using Microsoft.EnterpriseManagement.Configuration;

using Microsoft.EnterpriseManagement.Common;

You can also add another namespace, System.Configuration (as shown in the screenshot). I’ll describe later why we could use this one as well.

4) We first start implementing a constructor for the service that initiates all the required classes like the ManagementGroup with either an FQDN or localhost (I’m hosting that service on the same server as the Service Manager Data Access Service) of the Service Manager.

In the second line you’ll see the usage of ConfigurationSettings.AppSettings, which is why we add the System.Configuration namespace to the service. This uses the AppSettings section of the web.config file to gather the Version and PublicKeyToken for the Management Pack. The advantage of using the web.config is that you don’t have to hardcode those values in the code and with every change you would have to re-compile it everytime. If you put those values in the web.config you can just change the xml file.

Then we need the ManagementPack for the Incident class and the enumeration of the status of incidents. We put all these in the constructor of the service class.

5) Then we are going forward to implement the two web methods, I’ve called them CloseIncident and ActivateIncident. Those methods accept the ID from the incident (e.g. IR10) and a string for the current user, who called the method. That mechanism is needed to avoid closing or activating incidents by accident from other users (I’ll describe that feature in a moment). Both methods call another private method called UpdateIncident. That method does the real work and accepts three parameters - those two from the Webmethods and another one that defines if the incident needs to be closed or activated.

6) UpdateIncident has some more lines, but not that much. It initiates another MP that hosts a TypeProjection. This is required to get the Affected User (as this is not part of the Incident class, but is a relationship to a user instance) to compare it with the actual user (we’ll get the user from the security principial context of the webpage).

From that MP we initiate the TypeProjection and with the help of a criteria string that defines the search criteria for the TypeProjection (in this case the incident class is the seed of the TypeProjection) we can use GetObjectProjectionReader to receive the specific incident. If the incident with the specified ID was not found then the method return false. If there is a record found we use a ManagementPackRelationship instance to get the Affected User. Then we compare it with the activeUser string and make sure that the incident is not closed already (because from an ITIL process view a closed incident should not be re-activated again).

The real work is done within two lines of code, set the status to the string that is passed in the UpdateIncident method (close or active) and overwrite the instance.

7) The last thing we need to clarify for the service is the CreateIncidentCriteriaXml method that just returns the criteria that is needed to receive the correct incident. For that we just use a simple expression that uses the ID attribute of the WorkItem as the left part and the searchString as the right part and the equal operator.

So with that, we’ve finished the service.

8) You would be able to test that asmx service already, just press F5. You will get an interface in the browser to test your two webmethod.

Next, we need to create two aspx websites that calls the service and give the user some sort of feedback, if the call was successful or not.

After adding those aspx webpages to your solution go and add a method to each of both, one for activateIncident and one for closeIncident. There are two parameters that we need to get to call the service, the ID of the incident and the name of the current user. For the first parameter we just crawl the QueryString (so we have to set the ID in the URL in the email template later) and for the current user we call the WindowsIdentity.GetCurrent() method. Then initiate the service and call one method per each aspx webpage.

9) After that you’re absolutely free to design the webpage and customize it that it fits your needs and UI definitions and requirements. In my case I’ve just added some positive text if the method returns 0 or some negative text for the value 1.

Part II – eMail Template

Now it’s time to create the email template

Before you create the email template, the second part of our solution, it’s time to configure the notification channels within the SCSM console. Go to Administration – Notifications – Channels and choose the e-Mail Notification Channel and double click it or click on Properties in the task pane. There you need to configure your SMTP server, a return email address and enable the e-mail notification.

After that click on the templates within Administration – Notifications to create your new template.

i) General

Give the template a name and describe it. The most important part on that first page of the wizard is to choose the correct target class, which is Incident in this case and put it in your own management pack.

ii) Template Design

In the Template Design tab you can design your mail, if it’s old-style text email or a HTML formatted mail, the mail subject and the body. For me, I’ve decided to generate an HTML-formatted mail with some fancy stuff like images and different font-styles. The result could looks like this or even better if you like:

You can define your own CSS styles and arrange the text and images where you want them to be. The best way it works for me was using an editor outside of the console and create the entire look and feel there. Then the HTML source can just be copy/pasted into Service Manager’s notification template wizard textbox.

You can see more information about how to create notification templates in this blog post:

http://blogs.technet.com/servicemanager/archive/2009/09/28/creating-notification-templates-in-system-center-service-manager.aspx

The most importing thing is the URL of those two hyperlinks, as we defined two webpages we need to use those and add the incident ID with ?ID=IR10 (we need to replace that in the console with dynamic data from the incident)

Then just paste the complete HTML code to the console template wizard and replace the dynamic text parts with those items from the incident instance with theInsert… button (don’t try to just type the value in – that won’t work. Use the Insert… button).

From there you can choose the attributes from the incident and related entities, which are then placed within the template (both body and subject field). For example you have to replace the URL with &lt;a href="http://atssm/ServiceManagerSample/CloseIncident.aspx?ID=$Context/Property[Type='CustomSystem\_WorkItem\_Library!System.WorkItem']/Id$"&gt;Incident wieder eröffnen&lt;/a&gt; (Please note that you have to customize the URL to your own webserver and webapplication!)

The result looks like the screenshot below:

After that you’re done with the template, just click on Next for the Summary tab and finish the wizard.

Part III – Workflow

Now we have the web services/site pages and the email template, now we need the workflow that triggers the entire solution.

Open the console and go to the workflow configuration view that can be found under Administration – Workflow – Configuration. There are some already defined workflow configurations that can be used and we go with the Incident Event Workflow Configuration. After selecting that entry click on Properties in the task pane.

Now you can setup a new workflow that runs whenever an Incident is updated or created.

So click on Add and fill out the Workflow Information that are asked, take care to choose “When an incident is updated” in our case and choose also the correct Management Pack (it makes sense to choose the same as for the template, so that the solution is built in one single MP). And of course, enabling the workflow with the checkbox makes a lot of sense here J

Now we have to define the criteria when the workflow should be triggered. For now we just said whenever an incident is updates (which would be true, if someone just updates a description or any other field on the incident form). So we need to define that this should run only if the Status is changed to Resolved.

So keep the “Changed from” empty as this is not important to us, but add the Status in the “Changed To” tab and enter resolved to the textbox next to it.

So now it’s time to tell the workflow configuration what to do, when it is triggered. In our solution we’d like to Select People to Notify. So in the specific tab make sure the checkbox Enable notification is set and choose Affected User in the ComboBox and choose the new created mail template a couple of minutes before. And don’t forget to click the Add button (I did this from time to time J).

After that setup we are all set and done. Finish the wizard and see if the window for the Incident Event Workflows is updated with your new workflow definition.

Now it’s time to test the complete solution by resolving an incident in the console. Make sure that you have an incident that has an Affected User filled out and that this user has an email-address in the Active Directory and in the Service Manager database, otherwise there will no email be generated. If that’s a test lab environment like it was in my case, you can enter an email address in the CMBD on the user entity form, but make sure that you disable the AD Connector for a test, as it might overwrite the email-address with a blank value again J

In my template, I’m using also the Resolution Category and Comments from the analyst, so I put some more or less useful text in those fields and resolve the incident.

A couple of seconds later the email is generated and showing the correct values from the incident and the option to close or re-activate the incident with a simple click on the specific link.

The user gets a response that his/her incident was closed now (or re-activated) with the HTML page I designed before.

The source files for this solution are attached.

EMailIncidentSample.zip
