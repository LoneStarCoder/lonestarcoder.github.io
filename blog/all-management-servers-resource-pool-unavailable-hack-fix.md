---
layout: default
permalink: /blog/all-management-servers-resource-pool-unavailable-hack-fix
---
# All Management Servers Resource Pool Unavailable Hack/Fix
*Author: Brody Kilpatrick* | *Created: December 31, 2012*

I have seen a lot of posts regarding the "All Management Servers Resource Pool Unavailable" error since SCOM 2012 RC, and I am still seeing numerous posts. I have also not seen anything in SP1 regarding fixing this error. The "All Management Servers Resource Pool Unavailable" error seems to cause trouble where no trouble really exists.  
  
This is what I have found under normal circumstances:  
  

1. Everything is fine
2. Then, you get the error
3. You might get a couple of other errors, saying stuff doesn't work
4. The agents on the affected management server all serve up a false heartbeat failure
5. The agents fail to a different management server
6. The heartbeat failures resolve
7. The affected management server says, "oh, I'm fine now, turns out nothing was really wrong with me"
8. The agents change back to their primary management server
9. Everything is now fine again...except you now have inaccurate availability, a bunch of auto-resolved false alerts, unnecessary state stages, unnecessary e-mails, and probably an upset server team who received those e-mails.
10. Then, it all happens over again...

A Microsoft article existed at one point, which provided a registry change to workaround/fix the issue, but then the article disappeared. If you make this registry change on all of your management servers, it might just fix the issue. It fixed it for me, as well as other people. As far as I know, this change is unsupported, but it is also easily reversible.

Here is the fix/workaround:

Some Notes First:

1. As always, before making any changes, back up your registry and SCOM databases.
2. Make this change on ONE Management Server at a time, and give the management server some time to recover after the restart - about 15 minutes should do.
3. As far as I know, Microsoft does not support this, so use at your own risk.

The Actual Steps:

1. Open you Registry editor (Start > Run > regedit)
2. HKLM\SYSTEM\CurrentControlSet\services\HealthService\Parameters
3. Select the PoolManager Folder/Key. (If it does not exist, create it under Parameters)
4. Create 2 new D-Words

1. **PoolLeaseRequestPeriodSeconds**
2. Give it a Value of 600 (Decimal)
3. **PoolNetworkLatencySeconds**
4. Give it a Value of 120 (Decimal)

5. Restart the Management Server
6. Give it Time to Recover
7. Repeat on all Management Servers
8. Let things calm down (probably an hour or more)

Good luck!
