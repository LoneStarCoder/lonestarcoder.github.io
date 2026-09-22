---
layout: default
permalink: /blog/tip-approve-all-in-progress-activities-in-service-manager
---
[Home](https://lonestarcoder.github.io) | [Blog](https://lonestarcoder.github.io/blog/home) | [Articles](https://lonestarcoder.github.io/articles)
# Tip: Approve all In Progress Activities in Service Manager
*Author: Brody Kilpatrick* | *Created: September 3, 2014*

Stop manually approving each test review activity. Service Manager implementations usually include immense amounts of testing. If you are testing Service Requests or Change Requests, you probably have tons of review activities to approve. It can be time consuming to approve each activity, because, unlike manual activities, we can't just select them all and complete them. We have the option of deleting or canceling the work items, but this isn't really testing.  
  
SMLETS and powershell makes our lives much easier when it comes to administration. However, review activity relationships are slightly different than most other relationships. It can sometimes be difficult to figure out what to do when it comes to powershell and reviewers.  
  
The below powershell script retrieves all review activities, retrieves the reviewers for those activities, and then sets the decision to approved. This will allow the review activity to review the decisions, and then complete itself - exactly as it would happen inside the console.  
  
If you don't care how it works, then just grab the script and run it on your management server where you have SMLETS installed.  
If you want to learn a little and become a better Service Manager Administrator, I have broken down the script.  
If you don't have SMLETS, you can get it from [Codeplex](https://smlets.codeplex.com/).  
**The script with explanation comments:**  
#This is pretty self-explanatory, but we are importing SMLETS.  
Import-Module SMLETS  
#Before we can retrieve any objects, we need to get the object class. The Object class we are looking for is "ReviewActivity". Why the "$" (dollar sign) at the end? As part of this particular cmdlet,  the search is using regexp. The "$" marks the end of the line. We use this because there are cases where the cmdlet would retrieve more than one object class due to regexp matching.  
$RAC = get-SCSMClass System.WorkItem.Activity.ReviewActivity$  
#We need to filter out object results to only "In Progress" review activities. "Activity Status" is an enumeration.  
$ActStatusEnumInProgress = Get-SCSMEnumeration ActivityStatusEnum.Active$  
#We need the GUID of the enumeration so our "filter" switch will retrieve the correct results.  
$InProgressEnumId = $ActStatusEnumInProgress.id  
#Here, we are querying the review activity objects. The "class" switch specifies what class we want, in this case, "review activity".  
#The "Filter" switch is a server side filter that is far more efficient than "Where-object". We can filter on any column for that object.  
$RAS = get-SCSMObject -class $RAC -filter "Status -eq '$InProgressEnumId'"  
#So far, we should have all of the Review activities that are in progress. You can type $RAS to see a list of the review activities.  
#We probably have more than one activity in an array, but we need to perform actions on each individual object.   
foreach ($RA in $RAS){  
  #We are going to retrieve any relationships for each activity, where the activity is the source of the relationship.  
  $RElObj = Get-SCSMRelationshipObject -BySource $RA  
  #We do not want all relationships, only the reviewer relationship.  
  foreach ($Obj in $RELObj) {  
    if ($Obj.TargetObject.ClassName -eq "System.Reviewer") {  
      #Now we are getting the reviewer object itself, but rather than specifying class and filter, we have the GUID. We can use the "id" switch.  
      #Once we get the object, we are piping the object into the "Set-SCSMObject" command, which will update the object. The only thing we have to do is set the status to approved for each of the reviewers and internal SCSM workflow will take care of the rest.  
      #The command below will not actually make any changes. The "whatif" switch is a powershell switch that essentially tells you what the command is going to do, but does not commit the command. This is a great way to test non-destrutively.  
      #Many commands perform actions without output, so it is sometimes difficult to see what is going on. The "verbose" switch will output additional details at command execution.  
      #If you are ready to execute the command and approve your activities, simply remove "-whatif".  
      get-SCSMObject -id ($Obj.Targetobject.ID) | Set-SCSMObject -Property Decision -Value "Approved" -whatif -verbose  
    }  
  }  
}  
**The Script with no comments:**  
Import-Module SMLETS  
$RAC = get-SCSMClass System.WorkItem.Activity.ReviewActivity$  
$ActStatusEnumInProgress = Get-SCSMEnumeration ActivityStatusEnum.Active$  
$InProgressEnumId = $ActStatusEnumInProgress.id  
$RAS = get-SCSMObject -class $RAC -filter "Status -eq '$InProgressEnumId'"  
foreach ($RA in $RAS){  
  $RElObj = Get-SCSMRelationshipObject -BySource $RA  
  foreach ($Obj in $RELObj) {  
    if ($Obj.TargetObject.ClassName -eq "System.Reviewer") {  
      get-SCSMObject -id ($Obj.Targetobject.ID) | Set-SCSMObject -Property Decision -Value "Approved" -whatif -verbose  
    }  
  }  
}  

When you begin writing powershell scripts, it is a good idea to keep them in a repository, as you will most likely need them more than once, especially when it comes to System Center. If you want to talk more about powershell or System Center, come see me at Techfest Saturday, September 13th at the Sparkhound booth!  
Techfest Registration: [Techfest Registration](http://www.eventbrite.com/e/houston-techfest-2014-tickets-8015932871)  
Techfest Site: [Houston Techfest](http://houstontechfest-public.sharepoint.com/)
