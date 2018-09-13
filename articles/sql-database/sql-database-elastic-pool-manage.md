---
title: Maken en beheren van elastische pools - Azure SQL-database | Microsoft Docs
description: Maken en beheren van elastische Azure SQL-pools.
keywords: meerdere databases, database-resources en prestaties van de database
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.date: 08/01/2018
ms.author: ninarn
ms.topic: conceptual
ms.reviewer: carlrab
ms.openlocfilehash: 4c5bb0ab3431fa951871e64d7bdd8a3a313f480f
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "44721497"
---
# <a name="create-and-manage-elastic-pools-in-azure-sql-database"></a>Elastische pools in Azure SQL Database maken en beheren

Met een elastische pool bepaalt u de hoeveelheid resources die de elastische groep is vereist voor het afhandelen van de werkbelasting van de databases en de hoeveelheid resources voor elke gepoolde database. 

## <a name="azure-portal-manage-elastic-pools-and-pooled-databases"></a>Azure-portal: elastische pools en gepoolde databases beheren

Instellingen voor alle toepassingen vindt u op één plek: de **groep configureren** blade. Als u hier, een elastische pool niet vinden in de portal en klik op **groep configureren** vanaf de bovenkant van de blade of in het resourcemenu aan de linkerkant.

Hier kunt u een combinatie van de volgende wijzigingen en sla ze allemaal op één batch:
1. De servicelaag van de groep wijzigen
2. De prestaties (DTU of vCores) en de opslag omhoog of omlaag schalen
3. Toevoegen of verwijderen van databases naar/van de groep
4. Stel een min (gegarandeerd) en maximale prestatielimiet voor de databases in de groepen
5. Overzicht van de kosten om eventuele wijzigingen in uw factuur als gevolg van de nieuwe instellingen weer te geven

![Elastische pool-blade voor configuratie](./media/sql-database-elastic-pool-manage-portal/configure-pool.png)

## <a name="powershell-manage-elastic-pools-and-pooled-databases"></a>PowerShell: Elastische pools en gepoolde databases beheren 

Als u wilt maken en beheren van SQL Database elastische pools en gepoolde databases met Azure PowerShell, gebruikt u de volgende PowerShell-cmdlets. Als u wilt installeren of upgraden van PowerShell, Zie [Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u wilt maken en beheren van de logische servers voor een elastische pool, Zie [maken en beheerde logische servers](sql-database-logical-servers.md). Als u wilt maken en beheren van firewall-regels, Zie [maken en beheren van firewallregels met behulp van PowerShell](sql-database-firewall-configure.md#manage-firewall-rules-using-azure-powershell).

> [!TIP]
> Zie voor PowerShell-voorbeeldscripts, [maken van elastische pools en databases verplaatsen tussen pools en uit een pool met behulp van PowerShell](scripts/sql-database-move-database-between-pools-powershell.md) en [PowerShell gebruiken om te controleren en schalen van een elastische SQL-pool in Azure SQL Database](scripts/sql-database-monitor-and-scale-pool-powershell.md).
>

| Cmdlet | Beschrijving |
| --- | --- |
|[New-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/new-azurermsqlelasticpool)|Hiermee maakt een pool voor elastische database op een logische SQL-server.|
|[Get-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/get-azurermsqlelasticpool)|Hiermee haalt elastische pools en hun eigenschapswaarden op een logische SQL-server.|
|[Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool)|Hiermee wijzigt u de eigenschappen van een pool voor elastische database op een logische SQL-server. Gebruik bijvoorbeeld de **StorageMB** eigenschap te wijzigen van de maximale opslag van een elastische pool.|
|[Remove-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/remove-azurermsqlelasticpool)|Hiermee verwijdert u een pool voor elastische database op een logische SQL-server.|
|[Get-AzureRmSqlElasticPoolActivity](/powershell/module/azurerm.sql/get-azurermsqlelasticpoolactivity)|Hiermee wordt de status van bewerkingen in een elastische pool op een logische SQL-server opgehaald.|
|[New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase)|Hiermee maakt een nieuwe database in een bestaande groep of als een individuele database. |
|[Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)|Hiermee haalt u een of meer databases op.|
|[Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase)|Hiermee stelt u eigenschappen voor een database of een bestaande database worden verplaatst naar, uit of tussen elastische pools.|
|[Remove-AzureRmSqlDatabase](/powershell/module/azurerm.sql/remove-azurermsqldatabase)|Hiermee verwijdert u een database.|


> [!TIP]
> Het maken van een groot aantal databases in een elastische pool kan even duren wanneer wordt gedaan via de portal of PowerShell-cmdlets die slechts één database tegelijkertijd maken. Als u wilt maken naar een pool voor elastische automatiseren, Zie [CreateOrUpdateElasticPoolAndPopulate](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).
>

## <a name="azure-cli-manage-elastic-pools-and-pooled-databases"></a>Azure CLI: Elastische pools en gepoolde databases beheren

Maken en beheren van elastische pools voor SQL Database met de [Azure CLI](/cli/azure), gebruikt u de volgende [SQL-Database van Azure CLI](/cli/azure/sql/db) opdrachten. Gebruik de [Cloud Shell](/azure/cloud-shell/overview) om de CLI in uw browser uit te voeren of [installeer](/cli/azure/install-azure-cli) de CLI op macOS, Linux of Windows.

> [!TIP]
> Zie voor de Azure CLI-voorbeeldscripts, [CLI gebruiken om te verplaatsen van een Azure SQL database in een elastische SQL-groep](scripts/sql-database-move-database-between-pools-cli.md) en [gebruik Azure CLI voor het schalen van een elastische SQL-pool in Azure SQL Database](scripts/sql-database-scale-pool-cli.md).
>

| Cmdlet | Beschrijving |
| --- | --- |
|[AZ sql elastic-pool maken](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_create)|Hiermee maakt u een elastische pool.|
|[AZ sql elastic-pool list](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_list)|Retourneert een lijst van elastische pools in een server.|
|[AZ sql elastic-pool list-databases](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_list_dbs)|Retourneert een lijst met databases in een elastische pool.|
|[az sql elastic-pool list-editions](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_list_editions)|Ook bevat beschikbare groep DTU-instellingen, de maximale opslag, en per database-instellingen. Om te reduceren uitgebreidheid, extra opslaglimieten en per database instellingen standaard zijn verborgen.|
|[AZ sql elastic-pool update](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_update)|Een elastische pool-updates.|
|[AZ sql elastic-pool delete](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_delete)|Hiermee verwijdert u de elastische pool.|

## <a name="transact-sql-manage-pooled-databases"></a>Transact-SQL: Databases in pools beheren

Om te maken en verplaatsen van databases binnen de bestaande elastische groepen of haalt u informatie over een elastische pool van de SQL-Database met behulp van Transact-SQL, gebruikt u de volgende T-SQL-opdrachten. U kunt deze opdrachten met de Azure-portal, de opdracht [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Visual Studio Code](https://code.visualstudio.com/docs), of andere programma's die kan verbinding maken met een Azure SQL Database-server en doorgeven van Transact-SQL de opdrachten. Als u wilt maken en beheren van firewallregels met behulp van T-SQL, Zie [firewallregels beheren met behulp van Transact-SQL](sql-database-firewall-configure.md#manage-firewall-rules-using-transact-sql).

> [!IMPORTANT]
> U kan maken, bijwerken of verwijderen van een Azure SQL Database elastische pool met behulp van Transact-SQL. U kunt toevoegen of verwijderen van databases in een elastische pool en u kunt de DMV's gebruiken om informatie over bestaande elastische pools te retourneren.
>

| Opdracht | Beschrijving |
| --- | --- |
|[DATABASE (Azure SQL Database) maken](/sql/t-sql/statements/create-database-azure-sql-database)|Hiermee maakt een nieuwe database in een bestaande groep of als een individuele database. U moet zijn verbonden met de hoofddatabase om een nieuwe database te maken.|
| [ALTER DATABASE (Azure SQL Database)](/sql/t-sql/statements/alter-database-azure-sql-database) |Een database verplaatsen naar, uit of tussen elastische pools.|
|[DROP DATABASE (Transact-SQL)](/sql/t-sql/statements/drop-database-transact-sql)|Hiermee verwijdert u een database.|
|[sys.elastic_pool_resource_stats (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database)|Retourneert de gebruiksstatistieken resource voor alle pools voor elastische databases in een logische server. Voor elke pool voor elastic database moet er één rij voor elke 15 seconden rapportage venster (vier rijen per minuut). Dit omvat CPU, i/o-, Log, gebruik van opslag en gelijktijdige aanvraag/sessie-gebruik door alle databases in de groep.|
|[sys.database_service_objectives (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-service-objectives-azure-sql-database)|Retourneert de edition (servicelaag), de servicedoelstelling (prijscategorie) en de naam van de elastische groep, indien aanwezig, voor een Azure SQL-database of een Azure SQL Data Warehouse. Als u aangemeld bent op de database master in Azure SQL Database-server, retourneert de informatie voor alle databases. Voor Azure SQL Data Warehouse, moet u zijn verbonden met de hoofddatabase.|

## <a name="rest-api-manage-elastic-pools-and-pooled-databases"></a>REST-API: Elastische pools en gepoolde databases beheren

Als u wilt maken en beheren van SQL Database elastische pools en gepoolde databases, gebruikt u deze REST-API-aanvragen.

| Opdracht | Beschrijving |
| --- | --- |
|[Elastische pools - maken of bijwerken](/rest/api/sql/elasticpools/createorupdate)|Hiermee maakt u een nieuwe elastische pool of een bestaande elastische pool-updates.|
|[Elastische pools - verwijderen](/rest/api/sql/elasticpools/delete)|Hiermee verwijdert u de elastische pool.|
|[Elastische pools - Get](/rest/api/sql/elasticpools/get)|Hiermee haalt u een elastische pool.|
|[Elastische pools - lijst door Server](/rest/api/sql/elasticpools/listbyserver)|Retourneert een lijst van elastische pools in een server.|
|[Elastische pools - Update](/rest/api/sql/elasticpools/update)|Een bestaande elastische pool-updates.|
|[Elastische pool activiteiten](/rest/api/sql/elasticpoolactivities)|Retourneert de elastische groep activiteiten.|
|[Elastische pool Database-activiteiten](/rest/api/sql/elasticpooldatabaseactivities)|Retourneert de activiteit voor databases binnen een elastische pool.|
|[Databases - maken of bijwerken](/rest/api/sql/databases/createorupdate)|Maakt een nieuwe database gemaakt of bijgewerkt van een bestaande database.|
|[Databases - Get](/rest/api/sql/databases/get)|Hiermee haalt u een database.|
|[Databases - lijst op de elastische Pool](/rest/api/sql/databases/listbyelasticpool)|Retourneert een lijst met databases in een elastische pool.|
|[Databases - lijst met door de Server](/rest/api/sql/databases/listbyserver)|Retourneert een lijst met databases in een server.|
|[Databases - Update](/rest/api/sql/databases/update)|Werkt een bestaande database.|

## <a name="next-steps"></a>Volgende stappen

* Zie voor een video [videocursus van Microsoft Virtual Academy over elastische mogelijkheden van Azure SQL Database](https://mva.microsoft.com/training-courses/elastic-database-capabilities-with-azure-sql-db-16554)
* Zie [Ontwerppatronen voor SaaS-toepassingen met meerdere tenants met behulp van Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md) voor meer informatie over ontwerppatronen voor SaaS-toepassingen met elastische groepen.
* Zie voor een SaaS-zelfstudie met elastische pools, [Inleiding tot de Wingtip SaaS-toepassing](sql-database-wtp-overview.md).
