---
title: 'T-SQL: Azure SQL Database firewall rules | Microsoft Docs'
description: Learn how to configure server-level and database-level firewall rules for IP addresses that access Azure SQL databases using Transact-SQL.
services: sql-database
documentationcenter: ''
author: BYHAM
manager: jhubbard
editor: ''

ms.assetid: 71e692a1-5e2f-4a18-a6d6-527b849cf68e
ms.service: sql-database
ms.custom: authentication and authorization
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/07/2017
ms.author: rickbyh

---
# Configure Azure SQL Database server-level and database-level firewall rules using T-SQL

Microsoft Azure SQL Database uses firewall rules to allow connections to your servers and databases. You can define server-level and database-level firewall settings for the master or a user database in your Azure SQL Database server to selectively allow access to the database.

> [!IMPORTANT]
> To allow applications from Azure to connect to your database server, Azure connections must be enabled. For more information about firewall rules and enabling connections from Azure, see [Azure SQL Database Firewall](sql-database-firewall-configure.md). If you are making connections inside the Azure cloud boundary, you may have to open some additional TCP ports. For more information, see the **V12 of SQL Database: Outside vs inside** section of [Ports beyond 1433 for ADO.NET 4.5 and SQL Database V12](sql-database-develop-direct-route-ports-adonet-v12.md)
> 
> 

## Server-level firewall rules
Only the Azure SQL server admin login or the Azure Active Directory administrator can create a server-level firewall rule by using Transact-SQL.

1. Launch a query window and connect to the virtual master database by using SQL Server Management Studio.
2. Server-level firewall rules can be selected, created, updated, or deleted from within the query window.
3. To create or update server-level firewall rules, execute the `sp_set_firewall_rule` stored procedure. The following example enables a range of IP addresses on the server Contoso.<br/>Start by seeing what rules already exist.
   
        SELECT * FROM sys.firewall_rules ORDER BY name;
   
    Next, add a firewall rule.
   
        EXECUTE sp_set_firewall_rule @name = N'ContosoFirewallRule',
            @start_ip_address = '192.168.1.1', @end_ip_address = '192.168.1.10'
   
    To delete a server-level firewall rule, execute the sp_delete_firewall_rule stored procedure. The following example deletes the rule named ContosoFirewallRule.
   
        EXECUTE sp_delete_firewall_rule @name = N'ContosoFirewallRule'
   
   For more information on these stored procedures, see [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx) and [sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx).

## Database-level firewall rules
Only a database user with the **CONTROL** permission on the database (such as the database owner) can create a database-level firewall rule.

1. After creating a server-level firewall for your IP address, launch a query window through the Azure portal or through SQL Server Management Studio.
2. Connect to the database for which you want to create a database-level firewall rule.
   
    To create a new or update an existing database-level firewall rule, execute the `sp_set_database_firewall_rule` stored procedure. The following example creates a new firewall rule named ContosoFirewallRule.
   
        EXEC sp_set_database_firewall_rule @name = N'ContosoFirewallRule', 
            @start_ip_address = '192.168.1.11', @end_ip_address = '192.168.1.11'
   
    To delete an existing database-level firewall rule, execute the `sp_delete_database_firewall_rule` stored procedure. The following example deletes the rule named ContosoFirewallRule.
   `
   
        EXEC sp_delete_database_firewall_rule @name = N'ContosoFirewallRule'

For more information on these stored procedures, see [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) and [sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx).

> [!NOTE]
> For a tutorial that demonstrates the use of database-level firewalls, see [SQL authentication and authorization](sql-database-control-access-sql-authentication-get-started.md).
>


## Next steps
For articles on creating server-level firewall rules using other methods, see: 

* [Configure Azure SQL Database server-level firewall rules using the Azure Portal](sql-database-configure-firewall-settings.md)
* [Configure Azure SQL Database server-level firewall rules using PowerShell](sql-database-configure-firewall-settings-powershell.md)
* [Configure Azure SQL Database server-level firewall rules using the REST API](sql-database-configure-firewall-settings-rest.md)

For a tutorial on creating a database, see [Your first Azure SQL database](sql-database-get-started.md).
For help in connecting to an Azure SQL database from open source or third-party applications, see [Client quick-start code samples to SQL Database](https://msdn.microsoft.com/library/azure/ee336282.aspx).
To understand how to navigate to databases, see [Manage database access and login security](https://msdn.microsoft.com/library/azure/ee336235.aspx).

## Additional resources
* [Securing your database](sql-database-security-overview.md)
* [Security Center for SQL Server Database Engine and Azure SQL Database](https://msdn.microsoft.com/library/bb510589)

