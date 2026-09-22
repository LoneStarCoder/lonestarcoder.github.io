---
layout: default
permalink: /blog/authoring-console-and-management-pack-references-after-cu3
---
# Authoring Console and Management Pack References after CU3
*Author: Brody Kilpatrick* | *Created: November 24, 2010*

**If** you have updated to Service Manager CU3, you will notice warnings in the event logs that show management pack mismatches between. \*.0 and \*.216. This is a bug, and it will be fixed in the next release. However, if you are working in the Authoring console the referenced management packs will not necessarily be \*.216. To successfully import the management pack, you will have to manually change the references back to \*.0 first. After making the changes, you will be able to open the MP in the authoring console until you change the references back.  
  
  
**Changing the Authored Management Pack References for Import into Service Manager.**  

1. Make a copy of your authored management pack.
2. Open the MP using an editor.
3. Find the references section.
4. Perform a Find and Replace. Find .216. Replace with .0.
5. Import the MP into Service Manager
