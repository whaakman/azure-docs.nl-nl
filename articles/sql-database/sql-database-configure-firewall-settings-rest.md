---
title: 'REST-API: Firewallregels op serverniveau voor Azure SQL Database | Microsoft Docs'
description: Meer informatie over hoe u firewallregels op serverniveau configureert voor IP-adressen die via de REST-API toegang hebben tot Azure SQL-databases.
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: fc874f3c-c623-4924-8cb7-32a8c31e666c
ms.service: sql-database
ms.custom: authentication and authorization
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 08/09/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: 86bc7d89bb5725add8ba05b6f0978467147fd3ca
ms.openlocfilehash: 6ac9bdf34e8cf0a8ca8e06ae3ed6cd2dd1cf408b


---
# <a name="configure-azure-sql-database-server-level-firewall-rules-using-the-rest-api"></a>Firewallregels op serverniveau voor Azure SQL Database configureren met de REST-API
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

## <a name="manage-server-level-firewall-rules-through-rest-api"></a>Firewallregels op serverniveau beheren met behulp van de REST-API
1. Het beheer van firewallregels via de REST-API moet worden geverifieerd. Zie [Developer's guide to authorization with the Azure Resource Manager API](../azure-resource-manager/resource-manager-api-authentication.md) (Ontwikkelaarsgids voor verificatie met de Azure Resource Manager API) voor meer informatie.
2. U kunt regels op serverniveau maken, bijwerken en verwijderen met de REST-API
   
    Als u een firewallregel op serverniveau wilt maken of bijwerken, voert u de PUT-methode uit met het volgende:
   
        https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/firewallRules/{rule-name}?api-version={api-version}
   
    Aanvraagtekst
   
        {
         "properties": { 
            "startIpAddress": "{start-ip-address}", 
            "endIpAddress": "{end-ip-address}
            }
        } 

    Als u een bestaande firewallregel op serverniveau wilt verwijderen, voert u de DELETE-methode uit met het volgende:

        https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/firewallRules/{rule-name}?api-version={api-version}


## <a name="manage-firewall-rules-using-the-rest-api"></a>Firewallregels beheren met behulp van de REST-API
* [Firewallregels maken of bijwerken](https://msdn.microsoft.com/library/azure/mt445501.aspx)
* [Delete Firewall Rule](https://msdn.microsoft.com/library/azure/mt445502.aspx) (Firewallregel verwijderen)
* [Firewallregels instellen](https://msdn.microsoft.com/library/azure/mt445503.aspx)
* [Lijst met alle firewallregels](https://msdn.microsoft.com/library/azure/mt604478.aspx)

## <a name="next-steps"></a>Volgende stappen
Voor een artikel over het gebruik van Transact-SQL om firewallregels op server- en databaseniveau te maken raadpleegt u [Firewallregels op server- en databaseniveau voor Azure SQL Database configureren met T-SQL](sql-database-configure-firewall-settings-tsql.md). 

Voor artikelen met procedures voor het maken van firewallregels op serverniveau met andere methoden raadpleegt u: 

* [Configure Azure SQL Database server-level firewall rules using the Azure Portal](sql-database-configure-firewall-settings.md) (Firewallregels voor Azure SQL Database op serverniveau configureren met Azure Portal)
* [Configure Azure SQL Database server-level firewall rules using PowerShell](sql-database-configure-firewall-settings-powershell.md) (Firewallregels voor Azure SQL Database op serverniveau configureren met PowerShell)

Voor een zelfstudie over het maken van een database raadpleegt u [Maak in slechts enkele minuten een SQL Database met behulp van Azure Portal](sql-database-get-started.md).
Voor hulp bij het maken van een verbinding met een Azure SQL-database vanuit open-source toepassingen of toepassingen van derden raadpleegt u [Client quick-start code samples to SQL Database](https://msdn.microsoft.com/library/azure/ee336282.aspx) (Snelstartcodevoorbeelden voor clients met SQL Database).
Zie [Manage database access and login security](https://msdn.microsoft.com/library/azure/ee336235.aspx) (Databasetoegang en aanmeldingsbeveiliging beheren) als u wilt weten hoe u naar databases navigeert.

## <a name="additional-resources"></a>Aanvullende bronnen
* [Securing your database](sql-database-security-overview.md) (Uw database beveiligen)
* [Security Center for SQL Server Database Engine and Azure SQL Database](https://msdn.microsoft.com/library/bb510589) (Security Center voor SQL Server Database Engine en Azure SQL Database)

<!--Image references-->
[1]: ./media/sql-database-configure-firewall-settings/AzurePortalBrowseForFirewall.png
[2]: ./media/sql-database-configure-firewall-settings/AzurePortalFirewallSettings.png
<!--anchors-->





<!--HONumber=Jan17_HO1-->


