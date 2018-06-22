---
title: Maken en beheren van elastische pools - Azure SQL database | Microsoft Docs
description: Maken en beheren van Azure SQL elastische pools.
keywords: meerdere databases, databaseresources en prestaties van de database
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.date: 06/20/2018
ms.author: ninarn
ms.topic: conceptual
ms.reviewer: carlrab
ms.openlocfilehash: 3cdc82d71c05298aa5822b87c22edcc5d8e73385
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36313323"
---
# <a name="create-and-manage-elastic-pools-in-azure-sql-database"></a>Maken en beheren van elastische pools in Azure SQL Database

Met een elastische pool bepaalt u de hoeveelheid resources die de elastische groep is vereist voor het afhandelen van de werkbelasting van de databases en de hoeveelheid resources voor elke database gegroepeerde. 

## <a name="azure-portal-manage-elastic-pools-and-pooled-databases"></a>Azure-portal: elastische pools en gegroepeerde databases beheren

Instellingen voor alle toepassingen vindt u op één plek: de **pool configureren** blade. Als u hier, een elastische groep niet vinden in de portal en klik op **pool configureren** vanaf de bovenkant van de blade of vanuit het menu resource aan de linkerkant.

Hier kunt u een combinatie van de volgende wijzigingen en sla ze allemaal in één batch:
1. De servicelaag van de groep wijzigen
2. Omhoog of omlaag schalen van de prestaties (DTU of vCores) en opslag
3. Toevoegen of verwijderen van de databases uit de groep
4. Stel een min (gegarandeerd) en de maximale prestatielimiet voor voor de databases van de groepen
5. Overzicht van de kosten om eventuele wijzigingen in uw factuur als gevolg van de nieuwe instellingen weer te geven

![Elastische groep configuratie-blade](./media/sql-database-elastic-pool-manage-portal/configure-pool.png)

## <a name="powershell-manage-elastic-pools-and-pooled-databases"></a>PowerShell: Elastische pools en gegroepeerde databases beheren 

Als u wilt maken en beheren van de SQL-Database elastische pools en gegroepeerde databases met Azure PowerShell, gebruik de volgende PowerShell-cmdlets. Als u wilt installeren of upgraden van PowerShell, Zie [Installeer Azure PowerShell-module](/powershell/azure/install-azurerm-ps). Als u wilt maken en beheren van databases, servers en firewall-regels, Zie [maken en beheren van Azure SQL Database-servers en -databases met behulp van PowerShell](sql-database-servers-databases.md#manage-azure-sql-servers-databases-and-firewalls-using-powershell).

> [!TIP]
> Zie voor PowerShell-voorbeeldscripts, [maken van elastische pools en databases verplaatsen tussen groepen en van een groep met behulp van PowerShell](scripts/sql-database-move-database-between-pools-powershell.md) en [Gebruik PowerShell om te bewaken en schalen van een elastische SQL-groep in Azure SQL Database](scripts/sql-database-monitor-and-scale-pool-powershell.md).
>

| Cmdlet | Beschrijving |
| --- | --- |
|[New-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/new-azurermsqlelasticpool)|Een pool voor elastische database maakt op een logische SQL-server.|
|[Get-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/get-azurermsqlelasticpool)|Elastische pools en hun eigenschapswaarden op een logische SQL-server opgehaald.|
|[Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool)|Hiermee wijzigt u de eigenschappen van een pool voor elastische database op een logische SQL-server. Gebruik bijvoorbeeld de **StorageMB** eigenschap te wijzigen van de maximale opslag van een elastische pool.|
|[Remove-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/remove-azurermsqlelasticpool)|Hiermee verwijdert u een pool voor elastische database op een logische SQL-server.|
|[Get-AzureRmSqlElasticPoolActivity](/powershell/module/azurerm.sql/get-azurermsqlelasticpoolactivity)|Hiermee wordt de status van bewerkingen in een elastische pool op een logische SQL-server opgehaald.|
|[New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase)|Maakt een nieuwe database in een bestaande pool of als een individuele database. |
|[Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)|Hiermee haalt u een of meer databases op.|
|[Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase)|Stelt eigenschappen van een database of een bestaande database verplaatst naar buiten of tussen elastische pools.|
|[Remove-AzureRmSqlDatabase](/powershell/module/azurerm.sql/remove-azurermsqldatabase)|Hiermee verwijdert u een database.|


> [!TIP]
> Maken van veel databases in een elastische pool kan duren wanneer het wordt gedaan via de portal of PowerShell-cmdlets die slechts één database tegelijkertijd maken. Als u wilt maken voor een elastische pool automatiseren, Zie [CreateOrUpdateElasticPoolAndPopulate](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).
>

## <a name="azure-cli-manage-elastic-pools-and-pooled-databases"></a>Azure CLI: Elastische pools en gegroepeerde databases beheren

Maken en beheren van elastische pools van de SQL-Database met de [Azure CLI](/cli/azure), gebruikt u de volgende [Azure CLI SQL Database](/cli/azure/sql/db) opdrachten. Gebruik de [Cloud Shell](/azure/cloud-shell/overview) om de CLI in uw browser uit te voeren of [installeer](/cli/azure/install-azure-cli) de CLI op macOS, Linux of Windows.

> [!TIP]
> Zie voor Azure CLI-voorbeeldscripts, [gebruik CLI verplaatsen van een Azure SQL database in een elastische SQL-groep](scripts/sql-database-move-database-between-pools-cli.md) en [gebruik Azure CLI voor het schalen van een elastische SQL-groep in Azure SQL Database](scripts/sql-database-scale-pool-cli.md).
>

| Cmdlet | Beschrijving |
| --- | --- |
|[elastische sql-AZ-groep maken](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_create)|Maakt een elastische pool.|
|[lijst met AZ sql elastische pool](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_list)|Retourneert een lijst met elastische pools in een server.|
|[AZ sql elastische pool lijst-databases](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_list_dbs)|Retourneert een lijst met databases in een elastische pool.|
|[az sql elastic-pool list-editions](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_list_editions)|Omvat ook beschikbaar DTU groepsinstellingen, opslaglimieten, en per database-instellingen. Om te reduceren uitgebreidheid aanvullende opslaglimieten en per database instellingen standaard zijn verborgen.|
|[update van de elastische pool AZ sql](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_update)|Een elastische pool-updates.|
|[AZ sql elastische groep verwijderen](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_delete)|Hiermee wordt de elastische groep verwijderd.|

## <a name="transact-sql-manage-pooled-databases"></a>Transact-SQL: Gegroepeerde databases beheren

Maken en het verplaatsen van databases binnen bestaande elastische pools of om informatie over de elastische groep van een SQL-Database met Transact-SQL te retourneren, gebruikt u de volgende T-SQL-opdrachten. U kunt deze opdrachten met de Azure portal, de opdracht [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Visual Studio Code](https://code.visualstudio.com/docs), of een ander programma dat kan verbinding maken met een Azure SQL Database-server en doorgeven Transact-SQL-opdrachten. Als u wilt maken en beheren van databases, servers en firewall-regels, Zie [maken en beheren van Azure SQL Database-servers en met Transact-SQL-databases](sql-database-servers-databases.md#manage-azure-sql-servers-databases-and-firewalls-using-transact-sql).

> [!IMPORTANT]
> U niet kunt maken, bijwerken of verwijderen van een Azure SQL Database elastische pool met Transact-SQL. U kunt toevoegen of verwijderen van de databases uit een elastische pool en u kunt DMV's gebruiken om informatie over bestaande elastische pools te retourneren.
>

| Opdracht | Beschrijving |
| --- | --- |
|[DATABASE (Azure SQL Database) maken](/sql/t-sql/statements/create-database-azure-sql-database)|Maakt een nieuwe database in een bestaande pool of als een individuele database. U moet verbonden zijn met de database master om een nieuwe database te maken.|
| [ALTER DATABASE (Azure SQL Database)](/sql/t-sql/statements/alter-database-azure-sql-database) |Een database verplaatsen naar, uit of tussen elastische pools.|
|[DROP DATABASE (Transact-SQL)](/sql/t-sql/statements/drop-database-transact-sql)|Hiermee verwijdert u een database.|
|[sys.elastic_pool_resource_stats (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database)|Retourneert de gebruiksstatistieken resource voor alle pools voor elastische databases in een logische server. Voor elke elastische databasegroep moet er één rij voor elke 15 seconden reporting venster (vier rijen per minuut). Dit omvat CPU, IO, logboek, opslagverbruik en gelijktijdige aanvraag/sessie gebruik door alle databases in de groep.|
|[sys.database_service_objectives (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-service-objectives-azure-sql-database)|Retourneert de edition (servicelaag), de servicedoelstelling (prijscategorie) en de naam van de elastische groep, indien aanwezig, voor een Azure SQL database of een Azure SQL Data Warehouse. Als u aangemeld bent op de database master in een Azure SQL Database-server, retourneert de informatie voor alle databases. Voor Azure SQL Data Warehouse, moet u verbonden zijn met de database master.|

## <a name="rest-api-manage-elastic-pools-and-pooled-databases"></a>REST-API: Elastische pools en gegroepeerde databases beheren

Als u wilt maken en beheren van de SQL-Database elastische pools en gegroepeerde databases, gebruiken deze REST-API-aanvragen.

| Opdracht | Beschrijving |
| --- | --- |
|[Elastische Pools - maken of bijwerken](/rest/api/sql/elasticpools/createorupdate)|Een nieuwe elastische pool maken of bijwerken van een bestaande elastische pool.|
|[Elastische Pools - verwijderen](/rest/api/sql/elasticpools/delete)|Hiermee wordt de elastische groep verwijderd.|
|[Elastische Pools - ophalen](/rest/api/sql/elasticpools/get)|Hiermee haalt u een elastische pool.|
|[Elastische Pools - lijst per Server](/rest/api/sql/elasticpools/listbyserver)|Retourneert een lijst met elastische pools in een server.|
|[Elastische Pools - Update](/rest/api/sql/elasticpools/update)|Een bestaande elastische pool-updates.|
|[Aanbevolen elastische Pools - ophalen](/rest/api/sql/recommendedelasticpools/get)|Hiermee haalt u een aanbevolen elastische pool.|
|[Aanbevolen elastische Pools - lijst per Server](/rest/api/sql/recommendedelasticpools/listbyserver)|Retourneert aanbevolen elastische pools.|
|[Aanbevolen elastische Pools - lijst metrische gegevens](/rest/api/sql/recommendedelasticpools/listmetrics)|Retourneert aanbevolen elastische pool metrische gegevens.|
|[Activiteiten van de elastische groep](/rest/api/sql/elasticpoolactivities)|Retourneert de elastische groep activiteiten.|
|[Elastische Pool databaseactiviteiten](/rest/api/sql/elasticpooldatabaseactivities)|Retourneert de activiteit op databases binnen een elastische pool.|
|[Databases - maken of bijwerken](/rest/api/sql/databases/createorupdate)|Een nieuwe database maken of bijwerken van een bestaande database.|
|[Databases - Get](/rest/api/sql/databases/get)|Hiermee haalt u een database.|
|[Databases - verkrijgen door de elastische groep](/rest/api/sql/databases/getbyelasticpool)|Hiermee haalt u een database in een elastische pool.|
|[Databases - verkrijgen door aanbevolen elastische Pool](/rest/api/sql/databases/getbyrecommendedelasticpool)|Hiermee haalt u een database binnen een aanbevolen elastische pool.|
|[Databases - lijst door de elastische groep](/rest/api/sql/databases/listbyelasticpool)|Retourneert een lijst met databases in een elastische pool.|
|[Databases - lijst met aanbevolen elastische Pool](/rest/api/sql/databases/listbyrecommendedelasticpool)|Retourneert een lijst met databases binnen een aanbevolen elastische pool.|
|[Databases - lijst per Server](/rest/api/sql/databases/listbyserver)|Retourneert een lijst met databases in een server.|
|[Databases - Update](/rest/api/sql/databases/update)|Een bestaande database-updates.|

## <a name="next-steps"></a>Volgende stappen

* Zie voor een video [video loop van de Microsoft Virtual Academy op elastische mogelijkheden van Azure SQL Database](https://mva.microsoft.com/training-courses/elastic-database-capabilities-with-azure-sql-db-16554)
* Zie [Ontwerppatronen voor SaaS-toepassingen met meerdere tenants met behulp van Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md) voor meer informatie over ontwerppatronen voor SaaS-toepassingen met elastische groepen.
* Zie voor een SaaS-zelfstudie met elastische pools, [Inleiding tot de Wingtip SaaS-toepassing](sql-database-wtp-overview.md).
