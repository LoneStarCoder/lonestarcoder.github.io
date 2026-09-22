---
layout: default
permalink: /blog/scsm-process-cubes-manually
---
# SCSM - Process Cubes Manually
*Author: Brody Kilpatrick* | *Created: November 2, 2016*

If you need to process cubes manually directly from the SSAS server, you can use powershell. I pulled this script from [here](http://www.scsm.se/?p=881).  
  
[System.Reflection.Assembly]::LoadWithPartialName("Microsoft.AnalysisServices") > $NULL  
$Server = New-Object Microsoft.AnalysisServices.Server  
$Server.Connect("SSASServerName")  
$Databases = $Server.Databases  
$DWASDB = $Databases["DWASDatabase"]  
$Dimensions = New-Object Microsoft.AnalysisServices.Dimension  
$Dimensions = $DWASDB.Dimensions  
foreach ($Dimension in $Dimensions){$Dimension.Process("ProcessFull")}
