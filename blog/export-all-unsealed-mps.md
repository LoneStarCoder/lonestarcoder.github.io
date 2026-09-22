---
layout: default
permalink: /blog/export-all-unsealed-mps
---
# Export all unsealed MPs
*Author: Brody Kilpatrick* | *Created: February 8, 2011*

<http://www.scsm.se/?p=227>  
  
  

|  |
| --- |
| Here’s a little powershell script that I use for exporting all unsealed MPs. It’s really useful to run once in a while so you have a backup of all unsealed MPs. The script will create a new folder with the current date and store all MPs in that folder. Change the $OutPutDir variable to a folder of your choice. [powershell] Add-PSSnapin SMCMDLetSnapIn $OutPutDir = “C:\Unsealed MPs\” $UnsealedMPs = Get-SCSMManagementPack | ?{ ! $\_.Sealed } $CurrentDate = Get-Date $CurrentDate = $CurrentDate.ToShortDateString() $CompletePath = ($OutPutDir + $CurrentDate) if ( ! (test-path $CompletePath)) { $output = New-Item -Type Directory -Name $CurrentDate -Path $OutPutDir } $UnsealedMPs | %{ ” Exporting: {0}” -f $\_.Name $\_ | Export-SCSMManagementPack -directory “$CompletePath” } [/powershell] |
