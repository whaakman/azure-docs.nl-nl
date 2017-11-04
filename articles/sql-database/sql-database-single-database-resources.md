---
title: "Azure SQL Database één database | Microsoft Docs"
description: "De servicelaag, het prestatieniveau en de hoeveelheid storagea voor één Azure SQL database beheren."
services: sql-database
documentationcenter: na
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: On Demand
ms.date: 10/11/2017
ms.author: carlrab
ms.openlocfilehash: f2dca5ac40dff077f9e5ce983b15fcb5b2624a14
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="manage-resources-for-a-single-database-in-azure-sql-database"></a>Resources beheren voor een individuele database in Azure SQL Database

Met een individuele database, moet u de hoeveelheid resources die de database is vereist voor het afhandelen van de werkbelasting op de servicelaag, het prestatieniveau en de hoeveelheid opslagruimte vereist bepalen. 

## <a name="manage-single-database-resources-using-the-azure-portal"></a>Individuele databaseresources met Azure portal beheren

Als u wilt instellen of wijzigen van de servicelaag, prestatieniveau of opslagruimte voor een nieuwe of bestaande Azure SQL database met de Azure portal, opent u de **prestaties configureren** venster voor de database door te klikken op **prijscategorie (schaal dtu's)** - zoals weergegeven in de volgende schermafbeelding. 

- Instellen of wijzigen van de servicetier met het selecteren van de servicetier voor uw workload. 
- Instellen of wijzigen van het prestatieniveau (**dtu's**) binnen een service tier met de **DTU** schuifregelaar.
- Instellen of wijzigen van de opslagruimte voor de prestaties niveau met de **opslag** schuifregelaar. 

![Prijscategorie en prestatieniveau serviceniveau configureren](./media/sql-database-single-database-resources/change-service-tier.png)

> [!IMPORTANT]
> Bekijk [huidige beperkingen van P11 en P15 databases met een maximale grootte van 4 TB](sql-database-resource-limits.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb) bij het selecteren van een servicelaag P11 of P15.
>

## <a name="manage-single-database-resources-using-powershell"></a>Individuele databaseresources met behulp van PowerShell beheren

Als u wilt instellen of wijzigen van Azure SQL-databases Servicelagen en prestatieniveaus opslaghoeveelheid met behulp van PowerShell, deze PowerShell-cmdlets te gebruiken. Als u wilt installeren of upgraden van PowerShell, Zie [Installeer Azure PowerShell-module](/powershell/azure/install-azurerm-ps). 

| Cmdlet | Beschrijving |
| --- | --- |
|[New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase)|Maakt een database |
|[Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)|Een of meer databases opgehaald|
|[Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase)|Stelt eigenschappen van een database of een bestaande database is verplaatst naar een elastische pool. Gebruik bijvoorbeeld de **MaxSizeBytes** eigenschap in op de maximale grootte voor een database.|


> [!TIP]
> Zie voor een PowerShell-voorbeeldscript die bewaakt de maatstaven voor prestaties van een database naar een hoger prestatieniveau op schaal en een waarschuwingsregel maakt op een van de prestatiewaarden [bewaken en schalen van een enkele SQL-database met behulp van PowerShell](scripts/sql-database-monitor-and-scale-database-powershell.md).

## <a name="manage-single-database-resources-using-the-azure-cli"></a>Beheren van één databaseresources met de Azure CLI

Instellen of wijzigen van Azure SQL-databases Servicelagen en prestatieniveaus opslaghoeveelheid met de Azure CLI, gebruikt u deze [Azure CLI SQL Database](/cli/azure/sql/db) opdrachten. Gebruik de [Cloud Shell](/azure/cloud-shell/overview) om de CLI in uw browser uit te voeren of [installeer](/cli/azure/install-azure-cli) de CLI op macOS, Linux of Windows. Zie voor het maken en beheren van de elastische pools SQL, [elastische pools](sql-database-elastic-pool.md).

| Cmdlet | Beschrijving |
| --- | --- |
|[AZ sql server-firewallregel maken](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_create)|Hiermee maakt u een firewallregel op server|
|[lijst van AZ sql server-firewallregel](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_list)|Geeft een lijst van de firewall-regels op een server|
|[AZ sql server-firewallregel weergeven](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_show)|Geeft de details van een firewallregel|
|[update van AZ sql server-firewallregel](/cli/azure/sql/server/firewall-rule##az_sql_server_firewall_rule_update)|Een firewallregel bijgewerkt|
|[AZ sql server-firewallregel verwijderen](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_delete)|Hiermee verwijdert u een firewallregel|


> [!TIP]
> Zie voor een Azure CLI-voorbeeldscript dat één Azure SQL database naar een andere prestatieniveau schaalt na het uitvoeren van query's de informatie over de grootte van de database, [gebruik CLI bewaken en schalen van een enkele SQL-database](scripts/sql-database-monitor-and-scale-database-cli.md).
>

## <a name="manage-single-database-resources-using-transact-sql"></a>Beheren van één database-resources met behulp van Transact-SQL

Als u wilt instellen of wijzigen van Azure SQL-databases Servicelagen en prestatieniveaus opslaghoeveelheid met Transact-SQL, moet u deze T-SQL-opdrachten gebruiken. U kunt deze opdrachten met de Azure portal, de opdracht [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Visual Studio Code](https://code.visualstudio.com/docs), of een ander programma dat kan verbinding maken met een Azure SQL Database-server en doorgeven Transact-SQL-opdrachten. 

| Opdracht | Beschrijving |
| --- | --- |
|[DATABASE (Azure SQL Database) maken](/sql/t-sql/statements/create-database-azure-sql-database)|Maakt een nieuwe database. U moet verbonden zijn met de database master om een nieuwe database te maken.|
| [ALTER DATABASE (Azure SQL Database)](/sql/t-sql/statements/alter-database-azure-sql-database) |Hiermee wijzigt u een Azure SQL database. |
|[sys.database_service_objectives (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-service-objectives-azure-sql-database)|Retourneert de edition (servicelaag), de servicedoelstelling (prijscategorie) en de naam van de elastische groep, indien aanwezig, voor een Azure SQL database of een Azure SQL Data Warehouse. Als u aangemeld bent op de database master in een Azure SQL Database-server, retourneert de informatie voor alle databases. Voor Azure SQL Data Warehouse, moet u verbonden zijn met de database master.|
|[sys.database_usage (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-usage-azure-sql-database)|Geeft een lijst van het aantal, het type en de duur van de databases op een Azure SQL Database-server.|

Het volgende voorbeeld ziet u de maximale grootte voor een database met de opdracht ALTER DATABASE wordt gewijzigd:

 ```sql
ALTER DATABASE <myDatabaseName> 
   MODIFY (MAXSIZE = 4096 GB);
```

## <a name="manage-single-database-resources-using-the-rest-api"></a>Individuele databaseresources met de REST API beheren

Gebruik deze REST-API-aanvragen wilt instellen of wijzigen van Azure SQL-databases Servicelagen en prestatieniveaus opslaghoeveelheid.

| Opdracht | Beschrijving |
| --- | --- |
|[Databases - maken of bijwerken](/rest/api/sql/databases/createorupdate)|Een nieuwe database maken of bijwerken van een bestaande database.|
|[Databases - Get](/rest/api/sql/databases/get)|Hiermee haalt u een database.|
|[Databases - verkrijgen door de elastische groep](/rest/api/sql/databases/getbyelasticpool)|Hiermee haalt u een database in een elastische pool.|
|[Databases - verkrijgen door aanbevolen elastische Pool](/rest/api/sql/databases/getbyrecommendedelasticpool)|Hiermee haalt u een database binnen een recommented elastische pool.|
|[Databases - lijst door de elastische groep](/rest/api/sql/databases/listbyelasticpool)|Retourneert een lijst met databases in een elastische pool.|
|[Databases - lijst met aanbevolen elastische Pool](/rest/api/sql/databases/listbyrecommendedelasticpool)|Retourneert een lijst met databases binnen een recommented elastische pool.|
|[Databases - lijst per Server](/rest/api/sql/databases/listbyserver)|Retourneert een lijst met databases in een server.|
|[Databases - Update](/rest/api/sql/databases/update)|Een bestaande database-updates.|



## <a name="next-steps"></a>Volgende stappen

- Meer informatie over Servicelagen en prestatieniveaus opslag bedragen, Zie [Servicelagen](sql-database-service-tiers.md).
- Meer informatie over elastische pools, Zie [elastische pools](sql-database-elastic-pool.md).
- Meer informatie over [Azure-abonnement en Servicelimieten, quota's en beperkingen](../azure-subscription-service-limits.md)
