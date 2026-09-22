---
layout: default
permalink: /blog/service-manager-role-based-security-scoping
---
# Service Manager role based security scoping
*Author: Brody Kilpatrick* | *Created: June 12, 2011*

The below post isn't real new, but it is highly informational. A lot of questions tend to arise around security scoping, and I found a great post, which I wanted to duplicate. The below text was taken from <http://scug.be/blogs/scsm/archive/2010/03/21/service-manager-role-based-security-scoping.aspx>.  
  
  
An important aspect in the overall configuration of the Service Manager environment is providing access to the SCSM environment to perform operations. This in a controlled way, so End Users, Operators, Resolvers, Change Owners… can easily access SCSM and perform the their tasks in a controlled environment.  
With Role based security scoping in SCSM there is the possibility to configure a controlled environment for different service roles. A SCSM role profile is a configuration set to define access to objects, views in the console, operations they can perform and members of the role (AD User/Group). SCSM components of a User role are:  

- The ***security scope***: Is the security boundary in SCSM. Boundaries can be set on Group/queue, Class, Property & relationships.
- ***UI filter*** scope: This filter is for defining what an operator can see in the SCSM console. Limiting the options visible in the console improves the usability. UI filters can be set on console tasks, templates and views.
- ***User role profile***: SCSM includes some predefined user profiles who include a set of allowed operations with a class/property/relationship scope over objects.
- ***User Assignment***: The members of the user role in SCSM. This can be set for users or groups. (Always recommended to use groups)

When configuring role based security scoping we have to think about the profiles that have to be defined in SCSM with the corresponding operations. The different profiles for an implementation is specific and is something that needs to be defined upfront.  
The following example “runs” through the creation of the Mail incident resolver role.  
Example info:  

- Only incidents from the “Email problem" category need to be visible for the role.
- The mgmt console Views access is limited.
- User roles can be controlled with AD security group.

## Preparing the Security Scope

As specified above, Security Scope for a user profile can be specified on different levels. This preparation step goes through the creation of the group and the incident queue for further use in the user profile creation.  

### Create a group in SCSM

Creating a group in the SCSM console is a straightforward task. In this example the  

- In the Service Manager console, click Library, expand Library, and then click Groups.
- In the Tasks pane, click Create Group.
  - On the Before You Begin page, click ***Next***.
  - On the General page, do the following:
    - Provide a name for the group, such as ***Email Servers***.
    - In the Description text box, type a description for the group.
    - Under Management pack, make sure that an unsealed management pack is selected. In our example we store the information in a dedicated custom mgmt pack.
    - Click ***Next***.[![image](http://scug.be/cfs-file.ashx/__key/CommunityServer.Blogs.Components.WeblogFiles/scsm/image_5F00_thumb_5F00_1926B0E5.png "image")](http://scug.be/cfs-file.ashx/__key/CommunityServer.Blogs.Components.WeblogFiles/scsm/image_5F00_2E64DC5A.png)  
  - On the Included Members page, click Add.
    - In the Select Objects dialog box, select a class such as “Windows Computer”. (Groups can includes members of the same class or from different classes.)
    - In our example select all the Exchange servers in the organization.
    - Click ***OK***, click ***Next***

> [![image](http://scug.be/cfs-file.ashx/__key/CommunityServer.Blogs.Components.WeblogFiles/scsm/image_5F00_thumb_5F00_26C94720.png "image")](http://scug.be/cfs-file.ashx/__key/CommunityServer.Blogs.Components.WeblogFiles/scsm/image_5F00_69F4C54D.png)

- On the Dynamic members page, click ***Next***.
- On the Subgroups page, click ***Next***.
- On the Excluded Members page, click ***Next***.
- On the Summary page, confirm the group settings that you made, and then click ***Create***.
- On the Completion page, make sure that you receive the following confirmation message, and then click ***Close***.

  

### Create the incident queue

Next step in the preparation of the User Role profile configuration is to create a Queue for incidents.  

- In the Library pane, expand Library, and then click Queues.
- In the Tasks pane, click Create Queue.
- On the Before You Begin page, click Next.
- On the General page,
  - type a name in the Queue name box. (In our example, Mail incidents Queue)
  - Work item type box, in the Select a Class dialog box, select a class. In our case “***Incident***”, and then click OK.
  - In the Management pack list, select the same “roles” mgmt pack that is used to create the group. (keeping the thing together)
  - Click ***Next***.

> [![image](http://scug.be/cfs-file.ashx/__key/CommunityServer.Blogs.Components.WeblogFiles/scsm/image_5F00_thumb_5F00_1E18C8C7.png "image")](http://scug.be/cfs-file.ashx/__key/CommunityServer.Blogs.Components.WeblogFiles/scsm/image_5F00_614446F4.png)

- On the Criteria page, build the criteria that you want to use to filter work items for the queue, and then click ***Next***.
  - In our example, select the ***Classification Category*** property in the “Available Properties” area, click ***Add***.
  - In the list, select ***Email Problems***, and then click ***Next***.
  - (more the one criteria can be specified on this page)

> [![image](http://scug.be/cfs-file.ashx/__key/CommunityServer.Blogs.Components.WeblogFiles/scsm/image_5F00_thumb_5F00_189A3256.png "image")](http://scug.be/cfs-file.ashx/__key/CommunityServer.Blogs.Components.WeblogFiles/scsm/image_5F00_2EB0C3B5.png)

- On the Summary page, click Create to create the queue.
- On the Completion page, click Close.

  

## Create a User role Profile in SCSM

Group and queue are created in the SCSM console, the User Role Profile creation can start. Groups and queues are two configuration items of a User Profile. Mgmt Pack access, Views, templates & tasks are other configuration items in the wizard. If there is a need to limit access to these items then this information needs to be available before the creation of the profile.  
Example step-by-step for the email incident resolver user profile:  

- In the Administration pane of the SCSM console, expand Security, and then select***User Roles***.
- In the Tasks pane under User Roles, select ***Create User Role***, and then select the user role profile.
  - In our example we select the ***Incident Resolver*** role.

> [![image](http://scug.be/cfs-file.ashx/__key/CommunityServer.Blogs.Components.WeblogFiles/scsm/image_5F00_thumb_5F00_7589C0E5.png "image")](http://scug.be/cfs-file.ashx/__key/CommunityServer.Blogs.Components.WeblogFiles/scsm/image_5F00_7AD8A796.png)

- On the Before You Begin page, click ***Next***.
- On the General page, enter a name and description for this user role, and then click***Next***.
  - **Important Info**: on the general page of each predefined role there is a clear description of the rights of the selected role profile.

> [![image](http://scug.be/cfs-file.ashx/__key/CommunityServer.Blogs.Components.WeblogFiles/scsm/image_5F00_thumb_5F00_1AE38E87.png "image")](http://scug.be/cfs-file.ashx/__key/CommunityServer.Blogs.Components.WeblogFiles/scsm/image_5F00_5834691B.png)

- On the Management Packs page, select the management packs that contain the data that you want to assigned access to. In our example “select all” and click *Next*.

> [![image](http://scug.be/cfs-file.ashx/__key/CommunityServer.Blogs.Components.WeblogFiles/scsm/image_5F00_thumb_5F00_1A6735C5.png "image")](http://scug.be/cfs-file.ashx/__key/CommunityServer.Blogs.Components.WeblogFiles/scsm/image_5F00_4DD31EEE.png)

- On the Queues page, select the Queues that this user role will have access to, and click ***Next***. Here we use the just created Queue for our Email Incident Resolvers role.

> [![image](http://scug.be/cfs-file.ashx/__key/CommunityServer.Blogs.Components.WeblogFiles/scsm/image_5F00_thumb_5F00_05658D85.png "image")](http://scug.be/cfs-file.ashx/__key/CommunityServer.Blogs.Components.WeblogFiles/scsm/image_5F00_184A36FC.png)

- On the Groups page, select the Groups that this user role will have access to, and click***Next***. Here we use the just created Group for our Email Incident Resolvers role.

> [![image](http://scug.be/cfs-file.ashx/__key/CommunityServer.Blogs.Components.WeblogFiles/scsm/image_5F00_thumb_5F00_3FB0DD99.png "image")](http://scug.be/cfs-file.ashx/__key/CommunityServer.Blogs.Components.WeblogFiles/scsm/image_5F00_51BD2126.png)

- On the Tasks page, select the Tasks that this user role will have access to, and click***Next***. In our example I don’t limit the available tasks.

> [![image](http://scug.be/cfs-file.ashx/__key/CommunityServer.Blogs.Components.WeblogFiles/scsm/image_5F00_thumb_5F00_054541EB.png "image")](http://scug.be/cfs-file.ashx/__key/CommunityServer.Blogs.Components.WeblogFiles/scsm/image_5F00_04E934C3.png)

- On the Views page, select the Views that this user role will have access to, and click***Next***. In our example I want to limit the view in the mgmt console and selected only items from Incident management and configuration management.

> [![image](http://scug.be/cfs-file.ashx/__key/CommunityServer.Blogs.Components.WeblogFiles/scsm/image_5F00_thumb_5F00_3171A302.png "image")](http://scug.be/cfs-file.ashx/__key/CommunityServer.Blogs.Components.WeblogFiles/scsm/image_5F00_311595DA.png)

- On the Form Templates page, select the Templates that this user role will have access to, and click ***Next***. In our example I don’t limit the available templates.

> [![image](http://scug.be/cfs-file.ashx/__key/CommunityServer.Blogs.Components.WeblogFiles/scsm/image_5F00_thumb_5F00_197A960F.png "image")](http://scug.be/cfs-file.ashx/__key/CommunityServer.Blogs.Components.WeblogFiles/scsm/image_5F00_4E974B0C.png)

- On the Users page, click Add, and use the Select Users or Groups dialog box to select users and user groups from Active Directory Domain Services for this user role, and click ***Next***.

> [![image](http://scug.be/cfs-file.ashx/__key/CommunityServer.Blogs.Components.WeblogFiles/scsm/image_5F00_thumb_5F00_0C709031.png "image")](http://scug.be/cfs-file.ashx/__key/CommunityServer.Blogs.Components.WeblogFiles/scsm/image_5F00_70234E10.png)

- On the Summary page, review settings and click Create.
- On the Completion page, click Close.

  

### To validate the creation of a user role

- In the Service Manager console, verify that the newly created user role appears in the middle pane.
- Log on to the Service Manager console as one of the users assigned to the user role.
  - Verify the access in the mgmt console
  - Verify the Views in the mgmt console

> [![image](http://scug.be/cfs-file.ashx/__key/CommunityServer.Blogs.Components.WeblogFiles/scsm/image_5F00_thumb_5F00_4C2A50E9.png "image")](http://scug.be/cfs-file.ashx/__key/CommunityServer.Blogs.Components.WeblogFiles/scsm/image_5F00_03A087E5.png)  
>
> - Only the “Work Items” and “Configuration Items” pane are visible for the user. “Work Items” pane is limited by the Views filter in the configuration of the profile.
> - Only Incidents from the Email queue are visible in the console
> - Read-only access to the configuration items in the console

This is just an example how you can setup a user profile. There are a lot of different roles with different configuration items that can be set in SCSM, all depends on the requirements of the environment. Keep in mind that each additional role profile that is created will have an additional load on the server.  
I hope this gives you an idea how to configure role based security scoping for your environment.  
  
Have fun!  
  
Kurt
