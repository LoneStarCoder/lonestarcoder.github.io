---
layout: default
permalink: /blog/create-and-assign-service-manager-incidents-directly-from-scom-on-demand
---
# Create and Assign Service Manager Incidents Directly from SCOM on Demand
*Author: Brody Kilpatrick* | *Created: May 3, 2013*

**Update:**  
**As you read through below, you will notice that Microsoft has been *nice* enough to use some of the IDs I was using with the release of SP1. (This post was pre SP1) You will have to make small modifications to your script and Ids but the below solution still works.**  
**The Issue**  
If you use Operations Manager and Service Manager, you know by now that SCOM will automatically create Incidents in Service Manager. However, for most organizations, this just doesn’t make sense because they do not have a 1-to-1 Alert-to-Action ratio. You can set up basic criteria to limit the automatic creation, but this usually still results in too many unnecessary incidents. As a result, most organizations do not utilize this connector, which at one point was one of the most requested features of SCOM – to do really cool things with ticketing systems.  
**The Solution**  
So, instead, I have created a solution that will allow you to create incidents on demand directly from a SCOM Alert, while utilizing all the cool features of the Service Manager SCOM Alert connector. All you have to do is right click the alert(s) to create the on-demand tickets.  
What are some features of the solution in conjunction with the native Connector:  

- Right click one more multiple alerts and assign incidents directly to the specified group/user
- Closing the alert closes the ticket and vice-versa
- The Assigned User and the Ticket Id are maintained in the alert as sourced from SCSM
- The affected component in SCOM is automatically added as a related configuration item in SCSM
- Easily can be extended to do more fun stuff with only basic PowerShell Knowledge

**How Does it Work**  
The solution utilizes the following components:  

1. SCOM and SCSM obviously
2. A very small PowerShell Script
3. SCOM CMDLETS

Workflow:  

1. A user right clicks the alert and sets the resolution State.
2. A Command Subscription triggers based on the resolution state, sets a couple of custom fields, and changes the resolution state to “Generate Incident” a
3. The SCSM Alert connector triggers based on the new resolution state, generates an incident, and applies an incident template based on data in the custom fields.

## How to Implement the Solution

### These Steps need to be performed in SCOM

**Step One**  
Copy the following PowerShell script code and save on your SCOM management server as *UpdateCustomFieldPowershell.ps1*. *(I took this code from another blog online and modified it as my own. Unfortunately, I don’t know who wrote the original script.)*  
  
> Param($alertid)    
> $alertid = $alertid.toString()   
> write-eventlog -logname "Operations Manager" -source "Health Service Script" -eventID 1234 -entrytype "Information" -message "Running UpdateCustomFieldPowershell"   
> Import-Module OperationsManager; "C:\Program Files\System Center 2012\Operations Manager\Powershell\OperationsManager\Functions.ps1"; "C:\Program Files\System Center 2012\Operations Manager\Powershell\OperationsManager\Startup.ps1"  
> $alert = Get-SCOMAlert -Criteria "Id = '$alertid'"  
> write-host $alert  
> If ($alert.CustomField2 -ne "AlertProcessed")   
>     {   
> $AlertResState = (get-SCOMAlertResolutionState -ResolutionStateCode ($Alert.ResolutionState)).Name  
> $AlertResState  
>    # $alert.CustomField1 = $alert.NetBIOSComputerName   
>      $alert.CustomField1 = $AlertResState  
>      $alert.CustomField2 = "AlertProcessed"   
> $alert.ResolutionState  = 254  
>     $alert.Update("")   
>     }  
> exit

  
 **Step Two**  
 We need to create some new alert resolution states. The alert resolution states will trigger the script. You want to create a resolution state for each support group you would assign an alert. You can use whatever format you want. I used the format of “Assign to GROUPNAME”. Also keep in mind the Resolution State Ids and order you will use. I made my alphabetical. DO NOT use the resolution state 0,1,254, or 255.   
 To create new resolution states:  

- Go to the SCOM Console
- Go to the Administration Workspace
- Go to Settings
- Select Alerts
- Select the **new** button, create a resolution state and assign an Id. Resolution states will always be ordered by their Id
- Repeat for each resolution state

After you create your alert resolution states, you will need to create one more that triggers the SCSM Connect. Name this Alert Resolution State “Generate Incident.” Also, make sure this is the exact name as the script requires. If you want to change the name, you will have to update the script. Also, set the Id to 254.  
 **Step Three**  
 We need to set up a command channel and subscription that will trigger and run the script.  

- Open the SCOM Console
- Go the the Administration Workspace
- Go to Channels
- Create a new Command Channel
- Enter the full path of the above script
- Enter the command line parameters as shown in the example below (Be sure the use the double and single quotes correctly)
  - "C:\OpsMgrProductionScripts\SCOMUpdateCustomField.ps1" '$Data/Context/DataItem/AlertId$'
- Enter the startup folder as C:\windows\system32\windowspowershell\v1.0\
- Save the new Channel

Next, we need to set up the subscriber for the command channel.  

- Open the SCOM Console
- Go the the Administration Workspace
- Open subscribers
- Create a new subscriber
- In the addresses tab, click Add
- In the subscriber address, set the channel type to command and then select the channel you set up in the previous steps.
- Save the address and the subscriber

Next, we need to set up the Command Subscription  

- Open the SCOM Console
- Go the the Administration Workspace
- Open Subscriptions
- Create a new Subscription
- On the subscription criteria, check the checkbox “**with a specific resolution state**”
- Select all the new resolution states except “Generate Incident” (Do not select anything other than the assignment states)
- On the subscribers, add the new subscriber you created in the previous steps
- On the Channels, add the new channel you created in the previous steps
- Save the subscription

**Step Four**  
 The last thing we have to do in SCOM is set up the Alert connector. The alert connector will be triggered based on the resolution status of “Generate Incident”.  

- Open the SCOM Console
- Go the the Administration Workspace
- Go to connectors and select Internal Connectors
- Open the SCSM Alert Connector
- Create a new subscription in the connector
- In the criteria of the subscription

### These Steps need to be performed in SCSM

**Step One**   
 The first thing you want to do is enable and connect your SCSM SCOM Alert Connector. If you do not know how to do that, you can refer to technet. [http://technet.microsoft.com/en-us/library/hh524325.aspx](http://technet.microsoft.com/en-us/library/hh524325.aspx "http://technet.microsoft.com/en-us/library/hh524325.aspx"). Verify it works before moving any further.  
 **Step Two**  

- Create a new Management Pack dedicated to storing the SCOM Incident Templates in SCSM
- Create a SCOM incident template for each group that you want to assign via SCOM. Typically, this is about 10-20 templates. For testing purposes, I would just start with one or two.
- Add the correct group as the assigned to in each template. It is not necessary to fill any other information.

**Step Three**  

- In SCSM open the SCOM Alert Connector
- Go to the alert routing rules and add a new rule
  - For each rule select one of the templates that you created
  - On the **select criteria type*,*** select the **Custom Field** radio button
  - For custom field one, enter the **exact name** of the resolution state you used in SCOM. For example, if you are going to assign to the server team, and the name of resolution state is called “Assign to ServerTeam”, this is the exact phrase you need to enter into Custom Field one.
- Select Custom Field two from the drop down
- For custom field two, enter “AlertProcessed”
- Click OK
- Repeat for each template

### Time for Testing!

Now you are ready to test. Find an alert in SCOM, right click the alert and set it to a resolution state for assignment. Give the subscription time to run and the SCSM connector time to run. Usually, if the connector is running every 2 minutes, it takes the total process about 5 minutes to complete. While the actual workflows are running in a second, it simply takes time for both of them to trigger.  
   

#### Troubleshooting

If there are any issues with the configuration, the event logs will usually tell you about failures. If it is not working, but you don’t see any failures, your criteria probably do not match.  

#### Conclusion

This is a great alternative solution to automatically creating tickets from SCOM. You can still automatically create tickets as well simply by adding subscriptions to the SCSM SCOM Alert connector. If you have any issues, question, leave a comment.
