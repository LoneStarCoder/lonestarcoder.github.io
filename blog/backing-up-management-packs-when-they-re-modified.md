---
layout: default
permalink: /blog/backing-up-management-packs-when-they-re-modified
---
[Home](https://lonestarcoder.github.io) | [Blog](https://lonestarcoder.github.io/blog/home) | [Articles](https://lonestarcoder.github.io/articles)
# Backing up Management packs when they're modified
*Author: Brody Kilpatrick* | *Created: August 6, 2011*

*(Courtesy of my Co-worker and friend Thomas Bianco)*   
  
  
So i'm warping up the SCSM project at my client, and one of the value add deliverables i wanted to include was an automatic way to backup their configuration in case they modifiy it later and break something. googling around i found This script to backup management packs, but that's not really what I wanted to do. Some quick modifications and I ended up with this script:  
  
  
Import-module SMLets  
  

```
#discover if anything is modified today  
$today = Get-Date("{0} 00:00:00" -f (get-date).ToShortDateString());   
if (Get-SCSMManagementPack | where-object {$_.LastModified -ge $today}) {  
      
    #Inscope Definitions  
    $OutPutDir = "C:\Management Packs\UnsealedBackups\";  
    $UnsealedMPs = Get-SCSMManagementPack | ?{ ! $_.Sealed };  
    [string]$CurrentDate = Get-Date -uformat "%Y\%m\%d-%A";  
    $CompletePath = ($OutPutDir + $CurrentDate);  
  
    if ( ! (test-path  $CompletePath)) {  
        $output = New-Item -Type Directory -Name $CurrentDate -Path $OutPutDir;  
    };  
      
    $UnsealedMPs | Foreach-Object {  
        "   Exporting: {0}" -f $_.Name;  
        $_ | Export-SCSMManagementPack -targetdirectory "$CompletePath";  
    };  
};  
Remove-module SMLets -force;
```

Then i scheduled a task to run it every night at 9:00 PM Local time.  
  
powershell.exe -command "& 'C:\Windows\System32\WindowsPowerShell\v1.0\Examples\Backup-SCSMUnsealedMPs.ps1' "  
The bonus on this is that the backup will only be created if any pack was changed today, and you get a nice sorted tree by year\month\date-day  
![2011\08\05-Friday](https://portal.sparkhound.com/Departments/CoreInfrastructure/CIBlogs/Lists/Photos/SCSMBackupUMPsTree.gif)
