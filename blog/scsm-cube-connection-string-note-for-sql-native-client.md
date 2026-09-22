---
layout: default
permalink: /blog/scsm-cube-connection-string-note-for-sql-native-client
---
# SCSM Cube Connection String Note for SQL Native Client
*Author: Brody Kilpatrick* | *Created: November 2, 2016*

Recently, my DWASDataBase become corrupt and I have to rebuild the entire database and cubes from scratch. However, when Service Manager attempted to process the cubes, it was throwing 2 different errors, depending on which provider I used in the DWDataMart datasource.

**Error 1: (When attempting to use Native Client 10 or 10.1)**  

*Message : An Exception was encountered while trying during cube processing.  Message=  Processing error encountered - Location: , Source: Microsoft SQL Server 2012 Analysis Services Code: -1055850371, Description: Errors in the back-end database access module. The provider 'SQLNCLI10.1' is not registered..       Processing error encountered - Location: , Source: Microsoft SQL Server 2012 Analysis Services Code: -1056899072, Description: The following system error occurred:  Class not registered .       Processing error encountered - Location: , Source: Microsoft SQL Server 2012 Analysis Services Code: -1055784860, Description: Errors in the high-level relational engine. A connection could not be made to the data source with the DataSourceID of 'DWDataMart', Name of 'DWDataMart'..       Processing error encountered - Location: , Source: Microsoft SQL Server 2012 Analysis Services Code: -1054932980, Description: Errors in the OLAP storage engine: An error occurred while the dimension, with the ID of 'BillableTimeDim', Name of 'Billable Time Dim' was being processed..       Processing error encountered - Location: , Source: Microsoft SQL Server 2012 Analysis Services Code: -1054932979, Description: Errors in the OLAP storage engine: An error occurred while the 'TimeInMinutes' attribute of the 'BillableTimeDim' dimension from the 'DWASDataBase' database was being processed..       Processing error encountered - Location: , Source: Microsoft SQL Server 2012 Analysis Services Code: -1056964601, Description: Internal error: The operation terminated unsuccessfully..       Processing error encountered - Location: , Source: Microsoft SQL Server 2012 Analysis Services Code: -1055129594, Description: Server: The current operation was cancelled because another operation in the transaction failed..*

**Error 2: (When attempting to use Native Client 11, which was registered)**

*Message : An Exception was encountered while trying during cube processing.  Message=  Processing error encountered - Location: , Source: Microsoft SQL Server 2012 Analysis Services Code: -1056964601, Description: Internal error: The operation terminated unsuccessfully..       Processing error encountered - Location: , Source: Microsoft SQL Server 2012 Analysis Services Code: -1056571392, Description: OLE DB error: OLE DB or ODBC error: A network-related or instance-specific error has occurred while establishing a connection to SQL Server. Server is not found or not accessible. Check if instance name is correct and if SQL Server is configured to allow remote connections. For more information see SQL Server Books Online.; 08001; Client unable to establish connection; 08001; Encryption not supported on the client.; 08001..       Processing error encountered - Location: , Source: Microsoft SQL Server 2012 Analysis Services Code: -1055784860, Description: Errors in the high-level relational engine. A connection could not be made to the data source with the DataSourceID of 'DWDataMart', Name of 'DWDataMart'..       Processing error encountered - Location: , Source: Microsoft SQL Server 2012 Analysis Services Code: -1054932980, Description: Errors in the OLAP storage engine: An error occurred while the dimension, with the ID of 'BillableTimeDim', Name of 'Billable Time Dim' was being processed..       Processing error encountered - Location: , Source: Microsoft SQL Server 2012 Analysis Services Code: -1054932979, Description: Errors in the OLAP storage engine: An error occurred while the 'TimeInMinutes' attribute of the 'BillableTimeDim' dimension from the 'DWASDataBase' database was being processed..       Processing error encountered - Location: , Source: Microsoft SQL Server 2012 Analysis Services Code: -1055129594, Description: Server: The current operation was cancelled because another operation in the transaction failed..*

**The fix?** Just use the provider called **"Microsoft OLE DB Provider for SQL Server"**

**How to do it:**

1. Open SQL Management Studio
2. Connect to your SSAS server
3. Expand DWASDataBase, data sources, right click your data source, and go to properties.
4. Edit your Connection String and on Provider use **"Microsoft OLE DB Provider for SQL Server"**
5. That's it! Then attempt to process the cubes again.
