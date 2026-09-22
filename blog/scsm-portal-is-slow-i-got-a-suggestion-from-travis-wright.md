---
layout: default
permalink: /blog/scsm-portal-is-slow-i-got-a-suggestion-from-travis-wright
---
# SCSM Portal is Slow - I got a suggestion from Travis Wright
*Author: Brody Kilpatrick* | *Created: February 6, 2011*

I got some more information from one of our devs today.  Try this:

- Open IIS
- Select the Application Pools view
- Select the SM\_AppPool application pool (unless your portal is using a different app pool for some reason)
- Click the Advanced Settings... link in the Actions pane on the right (or in the context menu)
- Scroll down to the bottom to the Recycling section
- Change the Regular Time Interval (minutes) option to 0 (never recycle)

That will make sure the portal doesnt just recycle the app pool and require a rebuild just because a certain amount of time has passed.
