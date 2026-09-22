---
layout: default
permalink: /blog/bulk-user-creation-from-powershell
---
[Home](https://lonestarcoder.github.io) | [Blog](https://lonestarcoder.github.io/blog/home) | [Articles](https://lonestarcoder.github.io/articles)
# Bulk User Creation from Powershell
*Author: Brody Kilpatrick* | *Created: January 2, 2011*

**Create a CSV with comma delimited information as below  
CN,SN,GivenName,Name,Title,Description,PostalCode,TelephoneNumber,Department,Company,StreetAddress,Countrycode**  
**Use this script and modify the obvious stuff**  
*$IndiaOU=[ADSI]“LDAP://localhost:389/ou=UserAccounts,dc=SH,dc=com“  
$UserDetails=Import-Csv “C:\Userdetails.csv”  
foreach($UD in $UserDetails) {*  
*$CN=$UD.CN*  
*$SN=$UD.SN*  
*$title=$UD.title*  
*$description=$UD.description*  
*$department=$UD.department*  
*$streetAddress=$UD.streetAddress*  
*$postalcode=$UD.postalcode*  
*$telephoneNumber=$UD.telephoneNumber*  
*$givenName=$UD.givenName*  
*$company=$UD.company*  
*$mail=$UD.mail*  
*$homePhone=$UD.homePhone*  
*$mobile=$UD.mobile*  
*$userPrincipalName=$UD.userPrincipalName*  
*$Samaccountname=$UD.Samaccountname*  
*$Indiauser=$IndiaOU.create(“user”,”cn=$cn”)*  
*$Indiauser.Put(“sAMAccountName”,$Samaccountname)*  
*$Indiauser.put(“SN”,$SN)*  
*$Indiauser.put(“Title”,$title)*  
*$Indiauser.put(“Description”,$description)*  
*$Indiauser.put(“department”,$department)*  
*$Indiauser.put(“streetAddress”,$streetAddress)*  
*$Indiauser.put(‘telephoneNumber’,$telephoneNumber)*  
*$Indiauser.put(‘givenName’,$givenName)*  
*$Indiauser.put(‘company’,$company)*  
*$Indiauser.put(‘mail’,$mail)*  
*$Indiauser.put(‘homePhone’,$homePhone)*  
*$Indiauser.put(‘mobile’,$mobile)*  
*$Indiauser.put(‘userPrincipalName’,$userPrincipalName)*  
*$Indiauser.setinfo()*  
*$Indiauser.psbase.Invoke(“SetPassword”,”P@ssW0Rd”)*  
*$Indiauser.psbase.InvokeSet(‘Accountdisabled’,$false)*  
*$Indiauser.psbase.CommitChanges()*  
*}*  
  
  
Found this on:  
<http://techstarts.wordpress.com/2007/02/21/bulk-user-creation-using-powershell-2/>
