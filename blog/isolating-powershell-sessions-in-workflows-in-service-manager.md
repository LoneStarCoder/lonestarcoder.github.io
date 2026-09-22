---
layout: default
permalink: /blog/isolating-powershell-sessions-in-workflows-in-service-manager
---
# Isolating Powershell Sessions in Workflows in Service Manager
*Author: Brody Kilpatrick* | *Created: July 23, 2013*

**Problem:**  
One common issue I have ran into with writing workflows for Service Manager is the powershell sessions seem to be shared. When I call a set of cmdlets, such as the Active Directory cmdlets, they do not always load/unload properly, causing issues with subsequent scripts.  
  
**Solution:**  
We can isolate the powershell sessions inside the Service Manager Workflows. While powershell experts may know this, most of us don't, so here is my non-expert explanation.  
  

- When you run Service Manager Workflows, they run in the same process.
- If these workflows are running powershell, the powershell sessions are sometimes (or always) shared.
- By "running powershell inside a powershell" we can isolate our scripts, preventing issues between shared sessions.
- Once the script is complete, it cleans itself up, and completely closes the sessions.

The only issue I see with this method is that it might take the workflow a second or two longer to run because of having to open the new session - plan accordingly.

**How we do it:**

Take your completed script, and simply wrap the script in powershell.

For example, if my script is:

Import-Module Activedirectory

Get-User

I simply wrap it like this:

  
**Powershell {**  

Import-Module Activedirectory

Get-User

**}**

  
**Another Example with Parameters:**  
Powershell {  
param($Name, $pcc);  
get-process $Name -ComputerName $pcc;} -args "explorer", "localhost"  
  
  

Thanks goes out to Thomas Bianco for coming up with this simple workaround as a way to isolate Powershell Sessions.

Additional information was retrieved from <http://systemcenterblog.blogspot.com/2012/12/orchestrator-vs-powershell-i-win.html>
