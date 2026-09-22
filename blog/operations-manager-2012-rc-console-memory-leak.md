---
layout: default
permalink: /blog/operations-manager-2012-rc-console-memory-leak
---
[Home](https://lonestarcoder.github.io) | [Blog](https://lonestarcoder.github.io/blog/home) | [Articles](https://lonestarcoder.github.io/articles)
# Operations Manager 2012 RC Console Memory Leak
*Author: Brody Kilpatrick* | *Created: January 27, 2012*

As you may know by now, the Operations Manager 2012 console has a memory leak. Microsoft has acknowledged the issue, and put it into the release notes for RC. It isn't that big of a deal. Just close the console and reopen it, if it begins taking too much memory. They will probably fix it in RTM.  
  
Having said that, this is the real warning I want to give. Many administrators will log into the management server and use the console for various reasons. Many times an administrator will disconnect his session, rather than logging out. If you do this, make sure you close the console...because I didn't. The console "ate" all the memory and brought the management server to its knees. I closed and reopened the console, it freed the memory, and all was good.
