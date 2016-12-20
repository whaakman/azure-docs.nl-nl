---
title: Firewallregels voor Azure SQL Database op serverniveau configureren met PowerShell | Microsoft Docs
description: Informatie over het configureren van de firewall voor IP-adressen die toegang hebben tot Azure SQL-databases.
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 30dcea72-61c1-48b6-8e1d-b1db2eb61567
ms.service: sql-database
ms.custom: authentication and authorization
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 08/09/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 890ca8d9b7a3a76c6584f9d06cc8bdf3b36f87c0


---
# <a name="configure-azure-sql-database-server-level-firewall-rules-by-using-powershell"></a>Firewallregels voor Azure SQL Database op serverniveau configureren met PowerShell
> [!div class="op_single_selector"]
> * [Overzicht](sql-database-firewall-configure.md)
> * [Azure Portal](sql-database-configure-firewall-settings.md)
> * [TSQL](sql-database-configure-firewall-settings-tsql.md)
> * [PowerShell](sql-database-configure-firewall-settings-powershell.md)
> * [REST-API](sql-database-configure-firewall-settings-rest.md)
> 
> 

Azure SQL Database maakt gebruik van firewallregels om verbindingen met uw servers en databases toe te staan. Als u toegang tot de database selectief wilt toestaan, kunt u op uw SQL Database-server firewallinstellingen definiëren op serverniveau of op databaseniveau, voor zowel de hoofddatabase als voor een gebruikersdatabase.

> [!IMPORTANT]
> Azure-verbindingen moeten zijn ingeschakeld, zodat toepassingen vanuit Azure verbinding kunnen maken met uw databaseserver. Zie [Firewall voor Azure SQL Database](sql-database-firewall-configure.md) voor meer informatie over firewallregels en het inschakelen van verbindingen vanuit Azure. Als u verbindingen maakt binnen de grenzen van de Azure-cloud, moet u mogelijk enkele aanvullende TCP-poorten openen. Voor meer informatie raadpleegt u de sectie V12 of SQL Database: Outside vs inside (V12 SQL Database: buiten vs binnen) van [Ports beyond 1433 for ADO.NET 4.5 and SQL Database V12](sql-database-develop-direct-route-ports-adonet-v12.md) (Andere poorten dan 1433 voor ADO.NET 4.5 en SQL Database V12)
> 
> 

[!INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="create-server-firewall-rules"></a>Serverfirewallregels maken
Met Azure PowerShell kunnen firewallregels op serverniveau worden gemaakt, bijgewerkt of verwijderd .

Voer de cmdlet [New-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603860\(v=azure.300\).aspx) uit als u een nieuwe firewallregel op serverniveau wilt maken. In het volgende voorbeeld wordt een bereik van IP-adressen op de Contoso-server ingeschakeld.

    New-AzureRmSqlServerFirewallRule -ResourceGroupName 'resourcegroup1' -ServerName 'Contoso' -FirewallRuleName "ContosoFirewallRule" -StartIpAddress '192.168.1.1' -EndIpAddress '192.168.1.10'        

Voer de cmdlet [Set-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603789\(v=azure.300\).aspx) uit als u een bestaande firewallregel op serverniveau wilt wijzigen. In het volgende voorbeeld wordt het bereik van acceptabele IP-adressen voor de regel ContosoFirewallRule gewijzigd.

    Set-AzureRmSqlServerFirewallRule -ResourceGroupName 'resourcegroup1' –StartIPAddress 192.168.1.4 –EndIPAddress 192.168.1.10 –FirewallRuleName 'ContosoFirewallRule' –ServerName 'Contoso'

Voer de cmdlet [Remove-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603588\(v=azure.300\).aspx) uit als u een bestaande firewallregel op serverniveau wilt verwijderen. In het volgende voorbeeld wordt de regel ContosoFirewallRule verwijderd.

    Remove-AzureRmSqlServerFirewallRule –FirewallRuleName 'ContosoFirewallRule' –ServerName 'Contoso'


## <a name="manage-firewall-rules-by-using-powershell"></a>Firewallregels beheren met PowerShell
U kunt PowerShell ook gebruiken firewallregels te beheren. Zie de volgende onderwerpen voor meer informatie:

* [New-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603860\(v=azure.300\).aspx)
* [Remove-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603588\(v=azure.300\).aspx)
* [Set-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603789\(v=azure.300\).aspx)
* [Get-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603586\(v=azure.300\).aspx)

## <a name="next-steps"></a>Volgende stappen
Voor informatie over het gebruik van Transact-SQL om firewallregels op server- en databaseniveau te maken raadpleegt u [Configure Azure SQL Database server-level and database-level firewall rules using T-SQL](sql-database-configure-firewall-settings-tsql.md) (Firewallregels op server- en databaseniveau voor Azure SQL Database configureren met T-SQL).

Voor informatie over het maken van firewallregels op serverniveau met andere methoden raadpleegt u:

* [Configure Azure SQL Database server-level firewall rules using the Azure Portal](sql-database-configure-firewall-settings.md) (Firewallregels voor Azure SQL Database op serverniveau configureren met Azure Portal)
* [Configure Azure SQL Database server-level firewall rules using the REST API](sql-database-configure-firewall-settings-rest.md) (Firewallregels voor Azure SQL Database op serverniveau configureren met de REST API)

Voor een zelfstudie over het maken van een database raadpleegt u [Maak in slechts enkele minuten een SQL Database met behulp van Azure Portal](sql-database-get-started.md).
Voor hulp bij het maken van een verbinding met een Azure SQL-database vanuit open-source toepassingen of toepassingen van derden raadpleegt u [Client quick-start code samples to SQL Database (Snelstartcodevoorbeelden voor clients met SQL Database)](https://msdn.microsoft.com/library/azure/ee336282.aspx).
Zie [Manage database access and login security](https://msdn.microsoft.com/library/azure/ee336235.aspx) (Databasetoegang en aanmeldingsbeveiliging beheren) als u wilt weten hoe u naar databases navigeert.

## <a name="additional-resources"></a>Aanvullende bronnen
* [Securing your database](sql-database-security.md) (Uw database beveiligen)
* [Security Center for SQL Server Database Engine and Azure SQL Database](https://msdn.microsoft.com/library/bb510589) (Security Center voor SQL Server Database Engine en Azure SQL Database)

<!--Image references-->
[1]: ./media/sql-database-configure-firewall-settings/AzurePortalBrowseForFirewall.png
[2]: ./media/sql-database-configure-firewall-settings/AzurePortalFirewallSettings.png
<!--anchors-->



<!--HONumber=Dec16_HO1-->


