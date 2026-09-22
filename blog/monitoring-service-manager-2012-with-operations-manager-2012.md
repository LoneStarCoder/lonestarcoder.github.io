---
layout: default
permalink: /blog/monitoring-service-manager-2012-with-operations-manager-2012
---
# Monitoring Service Manager 2012 with Operations Manager 2012
*Author: Brody Kilpatrick* | *Created: November 16, 2012*

Note that if you are using System Center 2012: Service Manager, you will not be able to install Operations Manager 2012 agents on the machines. You must use agentless management; this is fine because the Service Manager 2012 Management Pack accounts for agentless monitoring. However, you might want to think about where you place you Service Manager Self Service Portal. If it is placed on a shared SharePoint server, you will not be able to install the 2012 agent to monitor that SharePoint server - It will have to be agentless as well.  
  
The Service Manager MP is fairly thorough and now monitors workflows.  
You can download the Management Pack Documentation at the link below.  
<http://www.microsoft.com/en-us/download/details.aspx?id=29980>
