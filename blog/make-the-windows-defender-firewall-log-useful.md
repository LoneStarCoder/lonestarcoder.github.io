---
layout: default
permalink: /blog/make-the-windows-defender-firewall-log-useful
---
# Make the Windows Defender Firewall Log Useful with PowerShell
*Author: Brody Kilpatrick* | *Created: August 16, 2022*

A small PowerShell script to parse the windows firewall log into an array.

```powershell
#Query Windows Firewall Log
$Path = "C:\windows\system32\LogFiles\Firewall\pfirewall.log"
$fwlogcontents = Get-Content -Path $Path
$fwlogcontentswithcommas = $fwlogcontents -replace " ",","
$fwlogcontentswithcommasClean = $fwlogcontentswithcommas.Split('`n') | Select -skip 11

$fw = @()
foreach ($line in $fwlogcontentswithcommasClean) {
 $splitline = $line.Split(",")
 $obj = new-object psobject
 $obj | add-member -name Date -type noteproperty -value $splitline[0]
 $obj | add-member -name Time -type noteproperty -value $splitline[1]
 $obj | add-member -name Action -type noteproperty -value $splitline[2]
 $obj | add-member -name Protocol -type noteproperty -value $splitline[3]
 $obj | add-member -name srcIP -type noteproperty -value $splitline[4]
 $obj | add-member -name dstIP -type noteproperty -value $splitline[5]
 $obj | add-member -name srcPort -type noteproperty -value $splitline[6]
 $obj | add-member -name dstPort -type noteproperty -value $splitline[7]
 $obj | add-member -name size -type noteproperty -value $splitline[8]
 $obj | add-member -name tcpflags -type noteproperty -value $splitline[9]
 $obj | add-member -name tcpsyn -type noteproperty -value $splitline[10]
 $obj | add-member -name tcpack -type noteproperty -value $splitline[11]
 $obj | add-member -name tcpwin -type noteproperty -value $splitline[12]
 $obj | add-member -name icmptype -type noteproperty -value $splitline[13]
 $obj | add-member -name icmpcode -type noteproperty -value $splitline[14]
 $obj | add-member -name info -type noteproperty -value $splitline[15]
 $obj | add-member -name path -type noteproperty -value $splitline[16]
 $obj | add-member -name pid -type noteproperty -value $splitline[17]
 $fw += $obj

}

$fw | ? {$_.Action -eq 'drop'} | ft *
```
