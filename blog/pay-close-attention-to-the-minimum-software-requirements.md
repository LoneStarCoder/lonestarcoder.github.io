---
layout: default
permalink: /blog/pay-close-attention-to-the-minimum-software-requirements
---
# Pay Close Attention to the Minimum Software Requirements!!!
*Author: Brody Kilpatrick* | *Created: October 24, 2011*

## <http://technet.microsoft.com/en-us/library/hh205990.aspx>

Minimum Software Requirements

Operations Manager 2012 server features require a supported operating system. For a list of the supported operating systems for each server feature, see the[Requirements by Feature](http://technet.microsoft.com/en-us/library/hh205990.aspx#BKMK_ReqByFeature) section in this document.

Note the following database considerations for Operations Manager 2012:

- Operations Manager 2012 does not support a 32-bit Operations Manager Operations database, Reporting Server data warehouse, or Audit Collection database, or SQL Server Reporting Services on a 64-bit operating system.
- The use of SQL Server 2008 R2 is supported for both new installations of Operations Manager 2012 and SQL Server 2008 R2 database upgrades.
- Using a different version of SQL Server for different Operations Manager 2012 features is not supported. The same version should be used for all features.
- SQL collation for all databases must be SQL\_Latin1\_General\_CP1\_CI\_AS; no other collation configurations are supported.
- The SQL Server Agent service must be started, and the startup type set to automatic.
- If you plan to use the Network Monitoring features of Operations Manager 2012, you should move the tempdb database to a separate disk that has multiple spindles. For more information, see [tempdb Database](http://go.microsoft.com/fwlink/p/?LinkId=221414).
