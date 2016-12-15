---
title: Firewallregels voor Azure SQL Database op serverniveau en databaseniveau met T-SQL | Microsoft Docs
description: Informatie over het configureren van de firewall voor IP-adressen die toegang hebben tot Azure SQL-databases.
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
ms.assetid: 71e692a1-5e2f-4a18-a6d6-527b849cf68e
ms.service: sql-database
ms.custom: auth and access
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/30/2016
ms.author: rickbyh
translationtype: Human Translation
ms.sourcegitcommit: 867f06c1fae3715ab03ae4a3ff4ec381603e32f7
ms.openlocfilehash: 573d33bd3db103d741a1b1d47951ea33f61e2d06


---
# <a name="configure-azure-sql-database-server-level-and-database-level-firewall-rules-using-t-sql"></a>Firewallregels voor Azure SQL Database op serverniveau en databaseniveau configureren met T-SQL
> [!div class="op_single_selector"]
> * [Overzicht](sql-database-firewall-configure.md)
> * [Azure Portal](sql-database-configure-firewall-settings.md)
> * [TSQL](sql-database-configure-firewall-settings-tsql.md)
> * [PowerShell](sql-database-configure-firewall-settings-powershell.md)
> * [REST-API](sql-database-configure-firewall-settings-rest.md)
> 
> 

Microsoft Azure SQL Database maakt gebruik van firewallregels om verbindingen met uw servers en databases toe te staan. Als u toegang tot de database selectief wilt toestaan, kunt u op de Azure SQL Database-server firewallinstellingen definiëren op serverniveau of op databaseniveau, voor zowel de hoofddatabase als voor een gebruikersdatabase.

> [!IMPORTANT]
> Azure-verbindingen moeten zijn ingeschakeld, zodat toepassingen vanuit Azure verbinding kunnen maken met uw databaseserver. Zie [Firewall voor Azure SQL Database](sql-database-firewall-configure.md) voor meer informatie over firewallregels en het inschakelen van verbindingen vanuit Azure. Als u verbindingen maakt binnen de grenzen van de Azure-cloud, moet u mogelijk enkele aanvullende TCP-poorten openen. Voor meer informatie raadpleegt u de sectie **V12 of SQL Database: Outside vs inside** (V12 SQL Database: buiten vs binnen) van [Ports beyond 1433 for ADO.NET 4.5 and SQL Database V12](sql-database-develop-direct-route-ports-adonet-v12.md) (Andere poorten dan 1433 voor ADO.NET 4.5 en SQL Database V12)
> 
> 

## <a name="server-level-firewall-rules"></a>Firewallregels op serverniveau
Alleen de hoofdaanmelding op serverniveau of de Azure Active Directory-beheerder kan een firewallregel op serverniveau maken met behulp van Transact-SQL.

1. Open een queryvenster en maak met behulp van SQL Server Management Studio verbinding met de virtuele hoofddatabase.
2. In het queryvenster kunnen firewallregels op serverniveau worden geselecteerd, gemaakt, bijgewerkt of verwijderd.
3. Voer de opgeslagen procedure `sp_set_firewall_rule` uit als u een firewallregel op serverniveau wilt maken of bijwerken. In het volgende voorbeeld wordt een bereik van IP-adressen op de Contoso-server ingeschakeld.<br/>Start om te zien welke regels er al bestaan.
   
        SELECT * FROM sys.firewall_rules ORDER BY name;
   
    Voeg daarna een firewallregel toe.
   
        EXECUTE sp_set_firewall_rule @name = N'ContosoFirewallRule',
            @start_ip_address = '192.168.1.1', @end_ip_address = '192.168.1.10'
   
    Als u een firewallregel op serverniveau wilt verwijderen, voert u de opgeslagen procedure sp_delete_firewall_rule uit. In het volgende voorbeeld wordt de regel ContosoFirewallRule verwijderd.
   
        EXECUTE sp_delete_firewall_rule @name = N'ContosoFirewallRule'
   
   Voor meer informatie over deze opgeslagen procedures raadpleegt u [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx) en [sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx).

## <a name="database-level-firewall-rules"></a>Firewallregels op databaseniveau
Alleen een databasegebruiker met de machtiging **BEHEER** voor de database (zoals de database-eigenaar) kan een firewallregel op databaseniveau maken.

1. Start na het maken van een firewallregel op serverniveau voor uw IP-adres een queryvenster via de klassieke portal of SQL Server Management Studio.
2. Maak verbinding met de database waarvoor u een firewallregel op databaseniveau wilt maken.
   
    Voer de opgeslagen procedure `sp_set_database_firewall_rule` uit als u een nieuwe firewallregel op databaseniveau wilt maken of een bestaande regel wilt bijwerken. In het volgende voorbeeld wordt een nieuwe regel met de naam ContosoFirewallRule gemaakt.
   
        EXEC sp_set_database_firewall_rule @name = N'ContosoFirewallRule', 
            @start_ip_address = '192.168.1.11', @end_ip_address = '192.168.1.11'
   
    Voer de opgeslagen procedure `sp_delete_database_firewall_rule` uit als u een firewallregel op databaseniveau wilt verwijderen. In het volgende voorbeeld wordt de regel ContosoFirewallRule verwijderd.
   `
   
        EXEC sp_delete_database_firewall_rule @name = N'ContosoFirewallRule'

Voor meer informatie over deze opgeslagen procedures raadpleegt u [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) en [sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx).

## <a name="next-steps"></a>Volgende stappen
Voor artikelen met procedures voor het maken van firewallregels op serverniveau met andere methoden raadpleegt u: 

* [Configure Azure SQL Database server-level firewall rules using the Azure Portal](sql-database-configure-firewall-settings.md) (Firewallregels voor Azure SQL Database op serverniveau configureren met Azure Portal)
* [Configure Azure SQL Database server-level firewall rules using PowerShell](sql-database-configure-firewall-settings-powershell.md) (Firewallregels voor Azure SQL Database op serverniveau configureren met PowerShell)
* [Configure Azure SQL Database server-level firewall rules using the REST API](sql-database-configure-firewall-settings-rest.md) (Firewallregels voor Azure SQL Database op serverniveau configureren met de REST API)

Voor een zelfstudie over het maken van een database raadpleegt u [Maak in slechts enkele minuten een SQL Database met behulp van Azure Portal](sql-database-get-started.md).
Voor hulp bij het maken van een verbinding met een Azure SQL-database vanuit open-source toepassingen of toepassingen van derden raadpleegt u [Client quick-start code samples to SQL Database](https://msdn.microsoft.com/library/azure/ee336282.aspx) (Snelstartcodevoorbeelden voor clients met SQL Database).
Zie [Manage database access and login security](https://msdn.microsoft.com/library/azure/ee336235.aspx) (Databasetoegang en aanmeldingsbeveiliging beheren) als u wilt weten hoe u naar databases navigeert.

## <a name="additional-resources"></a>Aanvullende bronnen
* [Securing your database](sql-database-security.md) (Uw database beveiligen)
* [Security Center for SQL Server Database Engine and Azure SQL Database](https://msdn.microsoft.com/library/bb510589) (Security Center voor SQL Server Database Engine en Azure SQL Database)




<!--HONumber=Dec16_HO1-->


