---
layout: default
permalink: /blog/script-to-monitor-service-manager-workflows
---
[Home](https://lonestarcoder.github.io) | [Blog](https://lonestarcoder.github.io/blog/home) | [Articles](https://lonestarcoder.github.io/articles)
# Script to Monitor Service Manager Workflows
*Author: Brody Kilpatrick* | *Created: December 27, 2018*

|  |  |
| --- | --- |
| Product | Version |
| System Center 2012 R2: Service Manager | Update Rollup 9 |
| System Center 2012 R2: Operations Manager | Update Rollup 9 |

---

I was asked to create a better way to check for workflow failures in
Service Manager. If you use SCOM to monitor SCSM workflows, you know
that there is one rule, and it throws an alert for every failure. I
didn't like this. Instead, this script will provide a summary of
failures (if there are any failures), since the last time it checked.
The script I am posting was meant to be scheduled, ran ad-hoc, or it can
be modified to put into SCOM - property bags and such.The script could
had been smaller if I connected directly to the database, but people
don't seem to like this, so it uses the NATIVE SCSM powershell module.
No SMLETS needed. The script should be ran on the workflow server. You
can also throw a -verbose behind it so you can see what it is actually
doing. Modify until you heart is content. I am not putting this into the
Microsoft Gallery, because it would need some cleanup and such.

The first time you run it, it is going to look for a workflow status log file. It is also going to report all failures. However, it will save the most recent status ID in the log file, and then only give failures since that status ID.

```
param(  
 [Parameter(Mandatory=$False)][string] $LogFilePathandName = ".\MonitorWorkflowFailuresLog.txt"  
 )  
 write-verbose "Starting Script"  
 Write-EventLog -LogName "Operations Manager" -Source "Health Service Script" -EntryType Information -EventID 12345 -Message "Script Starting"  
 [int]$IntRef = $null  
 $ScriptRunTime = get-date  
 Write-verbose  @"  
 Paramters Used:  
 LogFilePathandName = $LogFilePathandName  
 "@  
 <#  
 We need to create a log file entry for each workflow and keep track of the latest workflow instance ID.  
 This way, we only check the latest IDs.  
 >  
 if ((get-childItem $LogFilepathandName -ErrorAction silentlycontinue) -eq $null)   
 {  
 Write-verbose "Could not find Log a File. A new log file will be created" New-Item $LogFilePathandName -type file -value "WorkflowName,WorkflowId,WorkflowInstanceId"  
 }  
 else   
 {  
 <#  
 If a log file has been found, it needs to be in the correct format.  
 The format should be:  
 WorkflowName,WorkflowId,WorkflowInstanceId  
 ThisisaWorflowName,1234,2134123  
 Thisisaworkflownametoo,123425,43251  
 #>  
 write-verbose @"  
 Found a Log File, make sure the Log file is in the following format:  
 WorkflowName,WorkflowId,WorkflowInstanceId  
 ThisisaWorflowName,1234,2134123  
 Thisisaworkflownametoo,123425,43251  
 "@  
   #check to make sure the first line is the header   $RetreivedLogFileFirstTwoLines = Get-Content $LogFilePathandName -totalcount 2   $RetrievedLogFileHeaderLine = $RetreivedLogFileFirstTwoLines[0]   if ($RetrievedLogFileHeaderLine -eq "WorkflowName,WorkflowId,WorkflowInstanceId")     {       write-verbose "The Log file header is in the expected State - $RetrievedLogFileHeaderLine"  
 $RetrievedLogFileHeaderLineInExpectedState = $true  
       <#The header is in the expected state, so let's make sure there is at least one line to compare       #We need to take the first data line and see if it is in a "string,string,int" format.       #We really just wants to make sure items 2 and 3 (in an array that is 1 and 2) con be converted to integers#>       $RetrievedLogFileFirstDataLine = $RetreivedLogFileFirstTwoLines[1]       write-verbose "Checking RetrievedLogFileFirstDataLine - $RetrievedLogFileFirstDataLine"  
 $RetrievedLogFileFirstDataLineSplit = $RetrievedLogFileFirstDataLine.Split(",")  
       Write-verbose "Checking to see if there are three array items in the file Line"       if ($RetrievedLogFileFirstDataLineSplit.Count -eq 3)         { write-verbose "Count is 3 for data line splt" #There are three comma Delimited Items. Can Items 2 and 3 (1 and 2) be converted to an integer? <##NONE OF THIS WORKS IN .NET 3.5 OR EARLIER [System.Guid]::Parse(($RetrievedLogFileFirstDataLineSplit[1])) | Out-Null Try {  $RetrievedLogFileFirstDataLineWorkFlowIdInExpectedState = [System.Guid]::Parse(($RetrievedLogFileFirstDataLineSplit[1])) | Out-Null # test if is possible to cast and put parsed value in reference variable  $RetrievedLogFileFirstDataLineWorkFlowIdInExpectedState = $true  }  catch {    $RetrievedLogFileFirstDataLineWorkFlowIdInExpectedState = $false   } #> <#So we are going to revert to a method that will work in all versions.#> write-verbose "Checking to see if WorkflowId is in a GUID State" $RetrievedLogFileFirstDataLineWorkFlowIdInExpectedState = $true write-verbose "Checking to see if WorkflowInstanceId is in an int State" $RetrievedLogFileFirstDataLineWorkflowInstanceIdInExpectedState = [int32]::TryParse(($RetrievedLogFileFirstDataLineSplit[2]) , [ref]$IntRef) # test if is possible to cast and put parsed value in reference variable  if ($RetrievedLogFileFirstDataLineWorkFlowIdInExpectedState -eq $true -and $RetrievedLogFileFirstDataLineWorkflowInstanceIdInExpectedState -eq $true)  {   #The First Data line integers are in the expected state.   $RetrievedLogFileFirstDataLineInExpectedState = $true   write-verbose "The First Data line integers are in the expected state."  }  else   {   #Either RetrievedLogFileFirstDataLineWorkFlowIdInExpectedState or RetrievedLogFileFirstDataLineWorkflowInstanceIdInExpectedState   #was not true, therefore the first data line was not in the correct format   $RetrievedLogFileFirstDataLineInExpectedState = $false   write-verbose "RetrievedLogFileFirstDataLineWorkflowInstanceIdInExpectedState is $RetrievedLogFileFirstDataLineWorkflowInstanceIdInExpectedState"   write-verbose "RetrievedLogFileFirstDataLineWorkFlowIdInExpectedState $RetrievedLogFileFirstDataLineWorkFlowIdInExpectedState"   write-verbose "Either RetrievedLogFileFirstDataLineWorkFlowIdInExpectedState or RetrievedLogFileFirstDataLineWorkflowInstanceIdInExpectedState was not true, therefore the first data line was not in the correct format"   } } else  {  #RetrievedLogFileFirstDataLineSplit did not have 3 array items, therefore was not in the correct format  $RetrievedLogFileFirstDataLineInExpectedState = $false  write-verbose "RetrievedLogFileFirstDataLineSplit did not have 3 array items, therefore was not in the correct format"  }  
 }  
 else  
 {  
 $RetrievedLogFileHeaderLineInExpectedState = $false write-verbose "RetrievedLogFileHeaderLine was not in the expected State"  
 }  
 }  
 if ($RetrievedLogFileHeaderLineInExpectedState -eq $false -or $RetrievedLogFileFirstDataLineInExpectedState -eq $false)  
 {  
 #Need to write the log file and then exit.  
 write-error "Something was false. RetrievedLogFileHeaderLineInExpectedState  is $RetrievedLogFileHeaderLineInExpectedState and RetrievedLogFileFirstDataLineInExpectedState  is $RetrievedLogFileFirstDataLineInExpectedState. There is formatting issue, the script is stopping."  
 exit  
 }  
 <#Now that the Log file is taken care of, we need to get all of the workflows from Service Manager#>  
 write-verbose "About to Import the System.Center.Service.Manager Module, checking to see if it is already imported"  
 if ((get-module -name "System.Center.Service.Manager") -eq $null)  
 {  
 write-verbose "System.Center.Service.Manager is not imported, importing now"  
 #Write-EventLog -LogName "Operations Manager" -Source "Health Service Script" -EntryType Information -EventID 12345 -Message $Log  
 import-module -force "C:\Program Files\Microsoft System Center 2012\Service Manager\Powershell\System.Center.Service.Manager.psd1"  
 }  
 if ((get-module -name "System.Center.Service.Manager") -eq $null)  
 {  
 write-error "System.Center.Service.Manager did not import, cannot continue, exiting."  
 exit  
 }  
 #declare the failure array  
 $FailureArray = @()  
 #load the entire log file  
 write-verbose "Load the log file $LogFilePathandName"  
 $EntireLogFile = Get-Content $LogFilePathandName  
 # Save all WF into $workflow  
 $workflow = Get-SCSMWorkflowStatus  
 # Loop $workflow  
 write-verbose "Looping through each workflow"  
 foreach ($wf in $workflow) {  
 write-verbose $wf.Name  
 write-verbose "Searching for the workflow in the log file"  
 #; if you find the workflow, get the most recent statusid; otherwise, make the status id 0  
 $WorkflowFromLogFile = $null  
 $wfid = $wf.id  
 [array]$WorkflowFromLogFile = $EntireLogFile | ? {$_ -like ",$wfId,"}  
 if ($WorkflowFromLogFile -eq $null)  
 {  
 write-verbose "The Workflow was not found in the log File, adding a line in the log file for the workflow."  
 [int]$LastStatusRowId = 0  
 $NewWorkflowLineforLogFile = [string]$wf.Name + "," + [string]$wf.id + "," + [string]0  
 Add-Content $LogFilePathandName "$NewWorkflowLineforLogFile"  
 $WorkflowFromLogFileSplit = $NewWorkflowLineforLogFile.split(",")  
 $LogLineWorkflowInstanceIdCovertableStatus = "GOOD"  
 $LogLineStatus = "GOOD"  
 }  
 else  
 {  
 if ($WorkflowFromLogFile.count -gt 1) {  #found more than one line, throw error for now  write-error "Found more than one line in the log file for the workflow. This means that the workflow will be checked two times, which does not cause errors. However, it would be a good idea to find and remove the duplicate."  $LogLineStatus = "GOOD" } if ($WorkflowFromLogFile.count -eq 1) {  write-verbose "Found one line for the workflow in the log file."  $LogLineStatus = "GOOD"  #We need to get the WorkflowStatusRowId from the log file. To do this, we can split the line from the log file into an array, and look at the third (2nd) item.  $WorkflowFromLogFileSplit = $WorkflowFromLogFile[0].split(",")   if ([int32]::TryParse(($WorkflowFromLogFileSplit[2]) , [ref]$IntRef) -eq $true)  {   [int]$LastStatusRowId = $WorkflowFromLogFileSplit[2]   $LogLineWorkflowInstanceIdCovertableStatus = "GOOD"  }  else    {   #The id did would not convert to an integer, there is a problem with the log file. Skip and report the error, but do not kill the script.   write-error  "The id would not convert to an integer, there is a problem with the log file. Skip and report the error, the script will continue."   $LogLineWorkflowInstanceIdCovertableStatus = "BAD"   } } } # this is the else, where it found a line  
 if ($LogLineWorkflowInstanceIdCovertableStatus -eq "GOOD" -and $LogLineStatus -eq "GOOD")  
 {  
 write-verbose "Get the workflow status"  
 $status = Get-SCSMWorkflowStatus -name $wf.Name  
 $status = $status.GetStatus()  
 $status = $status | ? {[int]$_.RowId -gt $LastStatusRowId}  
 #Get the last Row Id for the status  
 $MaximumStatusRowId = ($status | measure-object -Property RowId -maximum).maximum  
 write-verbose "Check if the workflow has ran since the last check"  
 if ($MaximumStatusRowId -eq $null)  
 {  write-verbose "No need to update anything on the log file here. The max ID is the same." } else  {  write-verbose "Replace the line in the text file with a new line of the same data"  $NewWorkflowFromLogFile = $WorkflowFromLogFile -replace ",$LastStatusRowId",",$MaximumStatusRowId"  write-verbose "THIS IS THE NEW FILE ENTRY:"  write-verbose [string]$NewWorkflowFromLogFile  write-verbose "Setting Content"  (Get-Content $LogFilePathandName) | Foreach-Object {$_ -replace $WorkflowFromLogFile, $NewWorkflowFromLogFile} | Set-Content $LogFilePathandName -verbose  foreach ($st in $status)   {  [string]$strowid = ($st.RowId)  write-verbose "checking each status in the workflow and adding the FailureArray if failed. Status Row Id: $strowid"  #If the status is failed, we need to report it. Rather than reporting a failure for each workflow, we are going to summarize it for all if them.  #So we are going to add them all to an array.   if ($st.status -eq "Failed")    {   #$Log = $wf.name   ##Write-EventLog -LogName "Operations Manager" -Source "Health Service Script" -EntryType Information -EventID 12345 -Message "Added to FailureLog $Log"   $object = New-Object -TypeName PSObject   $object | Add-Member -Name 'Name' -MemberType Noteproperty -Value $wf.name   $object | Add-Member -Name 'Status' -MemberType Noteproperty -Value $st.status   $object | Add-Member -Name 'TimeStarted' -MemberType Noteproperty -Value $st.TimeStarted   $object | Add-Member -Name 'TimeFinished' -MemberType Noteproperty -Value $st.TimeFinished   $object | Add-Member -Name 'RelatedObject' -MemberType Noteproperty -Value $st.RelatedObject   $FailureArray += $object   }  }      }  
 } #If everything is good statement  
 write-verbose "Next Workflow  
 "  
 } #foreach workflow statement  
 #$FailureArray | foreach {[string]$FailureArrayString = [string]$FailureArrayString + $_ + "`n"}  
 $FailureArray  
 $FailureArray | ConvertTo-HTML | Out-File .\Report.htm  
 Invoke-Expression .\Report.htm
```

```powershell
param(
 [Parameter(Mandatory=$False)][string] $LogFilePathandName = ".\MonitorWorkflowFailuresLog.txt"
 )
 write-verbose "Starting Script"
 Write-EventLog -LogName "Operations Manager" -Source "Health Service Script" -EntryType Information -EventID 12345 -Message "Script Starting"
 [int]$IntRef = $null
 $ScriptRunTime = get-date
 Write-verbose  @"
 Paramters Used:
 LogFilePathandName = $LogFilePathandName
 "@
 <#
 We need to create a log file entry for each workflow and keep track of the latest workflow instance ID.
 This way, we only check the latest IDs.
 >
 if ((get-childItem $LogFilepathandName -ErrorAction silentlycontinue) -eq $null) 
 {
 Write-verbose "Could not find Log a File. A new log file will be created" New-Item $LogFilePathandName -type file -value "WorkflowName,WorkflowId,WorkflowInstanceId"
 }
 else 
 {
 <#
 If a log file has been found, it needs to be in the correct format.
 The format should be:
 WorkflowName,WorkflowId,WorkflowInstanceId
 ThisisaWorflowName,1234,2134123
 Thisisaworkflownametoo,123425,43251
 #>
 write-verbose @"
 Found a Log File, make sure the Log file is in the following format:
 WorkflowName,WorkflowId,WorkflowInstanceId
 ThisisaWorflowName,1234,2134123
 Thisisaworkflownametoo,123425,43251
 "@
   #check to make sure the first line is the header   $RetreivedLogFileFirstTwoLines = Get-Content $LogFilePathandName -totalcount 2   $RetrievedLogFileHeaderLine = $RetreivedLogFileFirstTwoLines[0]   if ($RetrievedLogFileHeaderLine -eq "WorkflowName,WorkflowId,WorkflowInstanceId")     {       write-verbose "The Log file header is in the expected State - $RetrievedLogFileHeaderLine"
 $RetrievedLogFileHeaderLineInExpectedState = $true
       <#The header is in the expected state, so let's make sure there is at least one line to compare       #We need to take the first data line and see if it is in a "string,string,int" format.       #We really just wants to make sure items 2 and 3 (in an array that is 1 and 2) con be converted to integers#>       $RetrievedLogFileFirstDataLine = $RetreivedLogFileFirstTwoLines[1]       write-verbose "Checking RetrievedLogFileFirstDataLine - $RetrievedLogFileFirstDataLine"
 $RetrievedLogFileFirstDataLineSplit = $RetrievedLogFileFirstDataLine.Split(",")
       Write-verbose "Checking to see if there are three array items in the file Line"       if ($RetrievedLogFileFirstDataLineSplit.Count -eq 3)         { write-verbose "Count is 3 for data line splt" #There are three comma Delimited Items. Can Items 2 and 3 (1 and 2) be converted to an integer? <##NONE OF THIS WORKS IN .NET 3.5 OR EARLIER [System.Guid]::Parse(($RetrievedLogFileFirstDataLineSplit[1])) | Out-Null Try {  $RetrievedLogFileFirstDataLineWorkFlowIdInExpectedState = [System.Guid]::Parse(($RetrievedLogFileFirstDataLineSplit[1])) | Out-Null # test if is possible to cast and put parsed value in reference variable  $RetrievedLogFileFirstDataLineWorkFlowIdInExpectedState = $true  }  catch {    $RetrievedLogFileFirstDataLineWorkFlowIdInExpectedState = $false   } #> <#So we are going to revert to a method that will work in all versions.#> write-verbose "Checking to see if WorkflowId is in a GUID State" $RetrievedLogFileFirstDataLineWorkFlowIdInExpectedState = $true write-verbose "Checking to see if WorkflowInstanceId is in an int State" $RetrievedLogFileFirstDataLineWorkflowInstanceIdInExpectedState = [int32]::TryParse(($RetrievedLogFileFirstDataLineSplit[2]) , [ref]$IntRef) # test if is possible to cast and put parsed value in reference variable  if ($RetrievedLogFileFirstDataLineWorkFlowIdInExpectedState -eq $true -and $RetrievedLogFileFirstDataLineWorkflowInstanceIdInExpectedState -eq $true)  {   #The First Data line integers are in the expected state.   $RetrievedLogFileFirstDataLineInExpectedState = $true   write-verbose "The First Data line integers are in the expected state."  }  else   {   #Either RetrievedLogFileFirstDataLineWorkFlowIdInExpectedState or RetrievedLogFileFirstDataLineWorkflowInstanceIdInExpectedState   #was not true, therefore the first data line was not in the correct format   $RetrievedLogFileFirstDataLineInExpectedState = $false   write-verbose "RetrievedLogFileFirstDataLineWorkflowInstanceIdInExpectedState is $RetrievedLogFileFirstDataLineWorkflowInstanceIdInExpectedState"   write-verbose "RetrievedLogFileFirstDataLineWorkFlowIdInExpectedState $RetrievedLogFileFirstDataLineWorkFlowIdInExpectedState"   write-verbose "Either RetrievedLogFileFirstDataLineWorkFlowIdInExpectedState or RetrievedLogFileFirstDataLineWorkflowInstanceIdInExpectedState was not true, therefore the first data line was not in the correct format"   } } else  {  #RetrievedLogFileFirstDataLineSplit did not have 3 array items, therefore was not in the correct format  $RetrievedLogFileFirstDataLineInExpectedState = $false  write-verbose "RetrievedLogFileFirstDataLineSplit did not have 3 array items, therefore was not in the correct format"  }
 }
 else
 {
 $RetrievedLogFileHeaderLineInExpectedState = $false write-verbose "RetrievedLogFileHeaderLine was not in the expected State"
 }
 }
 if ($RetrievedLogFileHeaderLineInExpectedState -eq $false -or $RetrievedLogFileFirstDataLineInExpectedState -eq $false)
 {
 #Need to write the log file and then exit.
 write-error "Something was false. RetrievedLogFileHeaderLineInExpectedState  is $RetrievedLogFileHeaderLineInExpectedState and RetrievedLogFileFirstDataLineInExpectedState  is $RetrievedLogFileFirstDataLineInExpectedState. There is formatting issue, the script is stopping."
 exit
 }
 <#Now that the Log file is taken care of, we need to get all of the workflows from Service Manager#>
 write-verbose "About to Import the System.Center.Service.Manager Module, checking to see if it is already imported"
 if ((get-module -name "System.Center.Service.Manager") -eq $null)
 {
 write-verbose "System.Center.Service.Manager is not imported, importing now"
 #Write-EventLog -LogName "Operations Manager" -Source "Health Service Script" -EntryType Information -EventID 12345 -Message $Log
 import-module -force "C:\Program Files\Microsoft System Center 2012\Service Manager\Powershell\System.Center.Service.Manager.psd1"
 }
 if ((get-module -name "System.Center.Service.Manager") -eq $null)
 {
 write-error "System.Center.Service.Manager did not import, cannot continue, exiting."
 exit
 }
 #declare the failure array
 $FailureArray = @()
 #load the entire log file
 write-verbose "Load the log file $LogFilePathandName"
 $EntireLogFile = Get-Content $LogFilePathandName
 # Save all WF into $workflow
 $workflow = Get-SCSMWorkflowStatus
 # Loop $workflow
 write-verbose "Looping through each workflow"
 foreach ($wf in $workflow) {
 write-verbose $wf.Name
 write-verbose "Searching for the workflow in the log file"
 #; if you find the workflow, get the most recent statusid; otherwise, make the status id 0
 $WorkflowFromLogFile = $null
 $wfid = $wf.id
 [array]$WorkflowFromLogFile = $EntireLogFile | ? {$_ -like ",$wfId,"}
 if ($WorkflowFromLogFile -eq $null)
 {
 write-verbose "The Workflow was not found in the log File, adding a line in the log file for the workflow."
 [int]$LastStatusRowId = 0
 $NewWorkflowLineforLogFile = [string]$wf.Name + "," + [string]$wf.id + "," + [string]0
 Add-Content $LogFilePathandName "$NewWorkflowLineforLogFile"
 $WorkflowFromLogFileSplit = $NewWorkflowLineforLogFile.split(",")
 $LogLineWorkflowInstanceIdCovertableStatus = "GOOD"
 $LogLineStatus = "GOOD"
 }
 else
 {
 if ($WorkflowFromLogFile.count -gt 1) {  #found more than one line, throw error for now  write-error "Found more than one line in the log file for the workflow. This means that the workflow will be checked two times, which does not cause errors. However, it would be a good idea to find and remove the duplicate."  $LogLineStatus = "GOOD" } if ($WorkflowFromLogFile.count -eq 1) {  write-verbose "Found one line for the workflow in the log file."  $LogLineStatus = "GOOD"  #We need to get the WorkflowStatusRowId from the log file. To do this, we can split the line from the log file into an array, and look at the third (2nd) item.  $WorkflowFromLogFileSplit = $WorkflowFromLogFile[0].split(",")   if ([int32]::TryParse(($WorkflowFromLogFileSplit[2]) , [ref]$IntRef) -eq $true)  {   [int]$LastStatusRowId = $WorkflowFromLogFileSplit[2]   $LogLineWorkflowInstanceIdCovertableStatus = "GOOD"  }  else    {   #The id did would not convert to an integer, there is a problem with the log file. Skip and report the error, but do not kill the script.   write-error  "The id would not convert to an integer, there is a problem with the log file. Skip and report the error, the script will continue."   $LogLineWorkflowInstanceIdCovertableStatus = "BAD"   } } } # this is the else, where it found a line
 if ($LogLineWorkflowInstanceIdCovertableStatus -eq "GOOD" -and $LogLineStatus -eq "GOOD")
 {
 write-verbose "Get the workflow status"
 $status = Get-SCSMWorkflowStatus -name $wf.Name
 $status = $status.GetStatus()
 $status = $status | ? {[int]$_.RowId -gt $LastStatusRowId}
 #Get the last Row Id for the status
 $MaximumStatusRowId = ($status | measure-object -Property RowId -maximum).maximum
 write-verbose "Check if the workflow has ran since the last check"
 if ($MaximumStatusRowId -eq $null)
 {  write-verbose "No need to update anything on the log file here. The max ID is the same." } else  {  write-verbose "Replace the line in the text file with a new line of the same data"  $NewWorkflowFromLogFile = $WorkflowFromLogFile -replace ",$LastStatusRowId",",$MaximumStatusRowId"  write-verbose "THIS IS THE NEW FILE ENTRY:"  write-verbose [string]$NewWorkflowFromLogFile  write-verbose "Setting Content"  (Get-Content $LogFilePathandName) | Foreach-Object {$_ -replace $WorkflowFromLogFile, $NewWorkflowFromLogFile} | Set-Content $LogFilePathandName -verbose  foreach ($st in $status)   {  [string]$strowid = ($st.RowId)  write-verbose "checking each status in the workflow and adding the FailureArray if failed. Status Row Id: $strowid"  #If the status is failed, we need to report it. Rather than reporting a failure for each workflow, we are going to summarize it for all if them.  #So we are going to add them all to an array.   if ($st.status -eq "Failed")    {   #$Log = $wf.name   ##Write-EventLog -LogName "Operations Manager" -Source "Health Service Script" -EntryType Information -EventID 12345 -Message "Added to FailureLog $Log"   $object = New-Object -TypeName PSObject   $object | Add-Member -Name 'Name' -MemberType Noteproperty -Value $wf.name   $object | Add-Member -Name 'Status' -MemberType Noteproperty -Value $st.status   $object | Add-Member -Name 'TimeStarted' -MemberType Noteproperty -Value $st.TimeStarted   $object | Add-Member -Name 'TimeFinished' -MemberType Noteproperty -Value $st.TimeFinished   $object | Add-Member -Name 'RelatedObject' -MemberType Noteproperty -Value $st.RelatedObject   $FailureArray += $object   }  }      }
 } #If everything is good statement
 write-verbose "Next Workflow
 "
 } #foreach workflow statement
 #$FailureArray | foreach {[string]$FailureArrayString = [string]$FailureArrayString + $_ + "`n"}
 $FailureArray
 $FailureArray | ConvertTo-HTML | Out-File .\Report.htm
 Invoke-Expression .\Report.htm

```
