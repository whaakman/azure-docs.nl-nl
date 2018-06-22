---
title: Maken, beheren van Azure SQL-servers en individuele databases | Microsoft Docs
description: Meer informatie over logische servers en individuele databases maken en beheren.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: carlrab
ms.openlocfilehash: 918cfd0257c82e84451e07ef904dbda331f47b95
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36313309"
---
# <a name="create-and-manage-logical-servers-and-single-databases-in-azure-sql-database"></a>Logische servers en individuele databases in Azure SQL Database maken en beheren 

U kunt maken en beheren van Azure SQL-Databases logische servers en één met de Azure-portal, PowerShell, Azure CLI, REST-API en Transact-SQL-databases.

## <a name="azure-portal-manage-logical-servers-and-databases"></a>Azure-portal: logische servers en -databases beheren

U kunt de Azure SQL database resourcegroep tevoren of tijdens het maken van de server zelf maken. Er zijn meerdere methoden voor het ophalen van een formulier nieuwe SQL server door het maken van een nieuwe SQL-server of als onderdeel van het maken van een nieuwe database. 

### <a name="create-a-blank-sql-server-logical-server"></a>Maak een lege SQL-server (logische server)

Maken van een Azure SQL Database-server (zonder een database) met de [Azure-portal](https://portal.azure.com), gaat u naar een leeg formulier voor SQL server (logische server).  

### <a name="create-a-blank-or-sample-sql-database"></a>Maak een lege of voorbeeld SQL-database

Maken van een Azure SQL database met de [Azure-portal](https://portal.azure.com), navigeer naar een leeg formulier voor SQL-Database en voer de vereiste gegevens. U kunt de resourcegroep en de logische server tevoren of tijdens het maken van de database zelf de Azure SQL-database maken. U kunt een lege database maken of maken van een voorbeelddatabase van Adventure Works LT. gebaseerd 

  ![database-1 maken](./media/sql-database-get-started-portal/create-database-1.png)

> [!IMPORTANT]
> Zie voor meer informatie over het selecteren van de prijscategorie voor uw database [aankoopmodel DTU gebaseerde](sql-database-service-tiers-dtu.md) en [vCore gebaseerde aankoopmodel (preview)](sql-database-service-tiers-vcore.md).

Zie het maken van een exemplaar beheerd [-exemplaar van een beheerd maken](sql-database-managed-instance-create-tutorial-portal.md)

### <a name="manage-an-existing-sql-server"></a>Een bestaande SQL server beheren

Een bestaande server beheren, gaat u naar de server met een aantal methoden - pagina voor specifieke SQL-database, zoals de **SQL-servers** pagina of de **alle resources** pagina. 

Voor het beheren van een bestaande database, gaat u naar de **SQL-databases** pagina en klik op de database die u wilt beheren. De volgende schermafbeelding ziet u hoe om te beginnen met het instellen van een firewall van het niveau van de server voor een database van de **overzicht** pagina voor een database. 

   ![serverfirewallregel](./media/sql-database-get-started-portal/server-firewall-rule.png) 

> [!IMPORTANT]
> Zie voor het configureren van eigenschappen van de prestaties voor een database [aankoopmodel DTU gebaseerde](sql-database-service-tiers-dtu.md) en [vCore gebaseerde aankoopmodel (preview)](sql-database-service-tiers-vcore.md).
>

> [!TIP]
> Zie voor een Azure portal Quick Start, [maken van een Azure SQL database in de Azure portal](sql-database-get-started-portal.md).

## <a name="powershell-manage-logical-servers-and-databases"></a>PowerShell: Logische servers en -databases beheren

Gebruik de volgende PowerShell-cmdlets voor het maken en beheren van Azure SQL-server, databases en firewalls met Azure PowerShell. Als u wilt installeren of upgraden van PowerShell, Zie [Installeer Azure PowerShell-module](/powershell/azure/install-azurerm-ps). 

> [!TIP]
> Zie voor een PowerShell-Quick Start [maken van één Azure SQL database met behulp van PowerShell](sql-database-get-started-portal.md). Zie voor PowerShell-voorbeeldscripts, [Gebruik PowerShell één Azure SQL database maken en configureren van een firewallregel](scripts/sql-database-create-and-configure-database-powershell.md) en [bewaken en schalen van een enkele SQL-database met behulp van PowerShell](scripts/sql-database-monitor-and-scale-database-powershell.md).
>

| Cmdlet | Beschrijving |
| --- | --- |
|[New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase)|Maakt een database |
|[Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)|Een of meer databases opgehaald|
|[Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase)|Stelt eigenschappen van een database of een bestaande database is verplaatst naar een elastische pool|
|[Remove-AzureRmSqlDatabase](/powershell/module/azurerm.sql/remove-azurermsqldatabase)|Hiermee verwijdert u een database|
|[New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup)|Maakt een resourcegroep|
|[New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver)|Hiermee maakt u een server|
|[Get-AzureRmSqlServer](/powershell/module/azurerm.sql/get-azurermsqlserver)|Retourneert informatie over servers|
|[Set-AzureRmSqlServer](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqlserver)|Hiermee wijzigt u de eigenschappen van een server|
|[Remove-AzureRmSqlServer](/powershell/module/azurerm.sql/remove-azurermsqlserver)|Hiermee verwijdert u een server|
|[New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule)|Hiermee maakt u een firewallregel op serverniveau |
|[Get-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/get-azurermsqlserverfirewallrule)|Firewallregels voor server opgehaald|
|[Set-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/set-azurermsqlserverfirewallrule)|Hiermee wijzigt u een firewallregel in een server|
|[Remove-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/remove-azurermsqlserverfirewallrule)|Hiermee verwijdert u een firewallregel van een server.|
| New-AzureRmSqlServerVirtualNetworkRule | Maakt een [ *virtueel netwerk regel*](sql-database-vnet-service-endpoint-rule-overview.md), op basis van een subnet plaatsen dat is een service-eindpunt van het virtuele netwerk. |

## <a name="azure-cli-manage-logical-servers-and-databases"></a>Azure CLI: Logische servers en -databases beheren

Maken en beheren van Azure SQL-server, databases en firewalls met [Azure CLI](/cli/azure), gebruikt u de volgende [Azure CLI SQL Database](/cli/azure/sql/db) opdrachten. Gebruik de [Cloud Shell](/azure/cloud-shell/overview) om de CLI in uw browser uit te voeren of [installeer](/cli/azure/install-azure-cli) de CLI op macOS, Linux of Windows. Zie voor het maken en beheren van elastische pools, [elastische pools](sql-database-elastic-pool.md).

> [!TIP]
> Zie voor een Azure CLI-Quick Start [maken van één Azure SQL database met de Azure CLI](sql-database-get-started-cli.md). Zie voor Azure CLI-voorbeeldscripts, [CLI gebruik één Azure SQL database maken en configureren van een firewallregel](scripts/sql-database-create-and-configure-database-cli.md) en [gebruik CLI bewaken en schalen van een enkele SQL-database](scripts/sql-database-monitor-and-scale-database-cli.md).
>

| Cmdlet | Beschrijving |
| --- | --- |
|[az sql db create](/cli/azure/sql/db#az_sql_db_create) |Maakt een database|
|[AZ sql db-lijst](/cli/azure/sql/db#az_sql_db_list)|Geeft een lijst van alle databases en datawarehouses in een server of alle databases in een elastische pool|
|[AZ sql db-edities](/cli/azure/sql/db#az_sql_db_list_editions)|Een lijst met beschikbare service doelstellingen en opslaglimieten|
|[AZ sql db lijst-gebruik](/cli/azure/sql/db#az_sql_db_list_usages)|Retourneert het gebruik van de database|
|[AZ sql db weergeven](/cli/azure/sql/db#az_sql_db_show)|Een database of de data warehouse opgehaald|
|[az sql db update](/cli/azure/sql/db#az_sql_db_update)|Een database bijwerkt|
|[AZ sql db verwijderen](/cli/azure/sql/db#az_sql_db_delete)|Hiermee verwijdert u een database|
|[az group create](/cli/azure/group#az_group_create)|Maakt een resourcegroep|
|[az sql server create](/cli/azure/sql/server#az_sql_server_create)|Hiermee maakt u een server|
|[lijst met AZ sql server](/cli/azure/sql/server#az_sql_server_list)|Een lijst met servers|
|[AZ sql server lijst-gebruik](/cli/azure/sql/server#az_sql_server_list_usages)|Retourneert het gebruik van server|
|[AZ sql server weergeven](/cli/azure/sql/server#az_sql_server_show)|Een server opgehaald|
|[update van sql server AZ](/cli/azure/sql/server#az_sql_server_update)|Een server worden bijgewerkt|
|[AZ sql server verwijderen](/cli/azure/sql/server#az_sql_server_delete)|Een server verwijderen|
|[AZ sql server-firewallregel maken](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_create)|Hiermee maakt u een firewallregel op server|
|[lijst van AZ sql server-firewallregel](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_list)|Geeft een lijst van de firewall-regels op een server|
|[AZ sql server-firewallregel weergeven](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_show)|Geeft de details van een firewallregel|
|[update van AZ sql server-firewallregel](/cli/azure/sql/server/firewall-rule##az_sql_server_firewall_rule_update)|Een firewallregel bijgewerkt|
|[AZ sql server-firewallregel verwijderen](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_delete)|Hiermee verwijdert u een firewallregel|

## <a name="transact-sql-manage-logical-servers-and-databases"></a>Transact-SQL: Logische servers en -databases beheren

Gebruik de volgende T-SQL-opdrachten voor het maken en beheren van Azure SQL-server, databases en firewalls met Transact-SQL. U kunt deze opdrachten met de Azure portal, de opdracht [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Visual Studio Code](https://code.visualstudio.com/docs), of een ander programma dat kan verbinding maken met een Azure SQL Database-server en doorgeven Transact-SQL-opdrachten. Zie voor het beheren van elastische pools [elastische pools](sql-database-elastic-pool.md).


> [!TIP]
> Zie voor een snel aan de slag met SQL Server Management Studio op Microsoft Windows, [Azure SQL Database: SQL Server Management Studio gebruiken om verbinding te en een query over gegevens](sql-database-connect-query-ssms.md). Zie voor een snel aan de slag met Visual Studio Code op de Mac OS-, Linux- of Windows, [Azure SQL Database: Gebruik Visual Studio Code verbinding maakt en gegevens opvragen](sql-database-connect-query-vscode.md).

> [!IMPORTANT]
> U kunt maken of verwijderen van een server met behulp van Transact-SQL.
>

| Opdracht | Beschrijving |
| --- | --- |
|[DATABASE (Azure SQL Database) maken](/sql/t-sql/statements/create-database-azure-sql-database)|Maakt een nieuwe database. U moet verbonden zijn met de database master om een nieuwe database te maken.|
| [ALTER DATABASE (Azure SQL Database)](/sql/t-sql/statements/alter-database-azure-sql-database) |Hiermee wijzigt u een Azure SQL database. |
|[ALTER DATABASE (Azure SQL datawarehouse)](/sql/t-sql/statements/alter-database-azure-sql-data-warehouse)|Hiermee wijzigt u een Azure SQL datawarehouse.|
|[DROP DATABASE (Transact-SQL)](/sql/t-sql/statements/drop-database-transact-sql)|Hiermee verwijdert u een database.|
|[sys.database_service_objectives (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-service-objectives-azure-sql-database)|Retourneert de edition (servicelaag), de servicedoelstelling (prijscategorie) en de naam van de elastische groep, indien aanwezig, voor een Azure SQL database of een Azure SQL Data Warehouse. Als u aangemeld bent op de database master in een Azure SQL Database-server, retourneert de informatie voor alle databases. Voor Azure SQL Data Warehouse, moet u verbonden zijn met de database master.|
|[sys.dm_db_resource_stats (Azure SQL Database)](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database)| Retourneert de CPU, IO en geheugen verbruik voor een Azure SQL Database-database. Er bestaat een rij voor elke 15 seconden zelfs als er geen activiteit in de database.|
|[sys.resource_stats (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database)|Retourneert de gegevens in CPU-gebruik en opslag voor een Azure SQL Database. De gegevens worden verzameld en geaggregeerd binnen vijf minuten.|
|[sys.database_connection_stats (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-connection-stats-azure-sql-database)|Statistieken voor SQL-Database database connectiviteitsgebeurtenissen, met een overzicht van de database verbinding successen en mislukkingen bevat. |
|[sys.event_log (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-event-log-azure-sql-database)|Geslaagde Azure SQL Database-databaseverbindingen verbindingsfouten en impassen retourneert. U kunt deze informatie gebruiken bij te houden of de activiteit van uw database met SQL Database oplossen.|
|[sp_set_firewall_rule (Azure SQL Database)](/sql/relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database)|Maken of bijwerken van het niveau van de server firewall-instellingen voor uw SQL Database-server. Deze opgeslagen procedure is alleen beschikbaar in de hoofddatabase voor de principal-aanmelding op serverniveau. Een firewallregel op serverniveau kan alleen worden gemaakt met behulp van Transact-SQL nadat de eerste firewallregel op serverniveau is gemaakt door een gebruiker met machtigingen op Azure-niveau|
|[sys.firewall_rules (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-firewall-rules-azure-sql-database)|Retourneert informatie over het niveau van de server-firewall-instellingen die zijn gekoppeld aan uw Microsoft Azure SQL Database.|
|[sp_delete_firewall_rule (Azure SQL Database)](/sql/relational-databases/system-stored-procedures/sp-delete-firewall-rule-azure-sql-database)|Hiermee verwijdert u serverniveau firewall-instellingen van uw SQL-Database-server. Deze opgeslagen procedure is alleen beschikbaar in de hoofddatabase voor de principal-aanmelding op serverniveau.|
|[sp_set_database_firewall_rule (Azure SQL Database)](/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database)|Maken of bijwerken van de firewallregel op databaseniveau regels voor uw Azure SQL Database of SQL Data Warehouse. Database-firewall-regels kunnen worden geconfigureerd voor de database master en gebruikersdatabases op SQL-Database. Database-firewallregels zijn nuttig wanneer databasegebruikers met behulp van opgenomen. |
|[sys.database_firewall_rules (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-firewall-rules-azure-sql-database)|Retourneert informatie over de firewallregel op databaseniveau-instellingen die zijn gekoppeld aan uw Microsoft Azure SQL Database. |
|[sp_delete_database_firewall_rule (Azure SQL Database)](/sql/relational-databases/system-stored-procedures/sp-delete-database-firewall-rule-azure-sql-database)|Hiermee verwijdert u de instelling van de firewallregel op databaseniveau van uw Azure SQL Database of SQL Data Warehouse. |



## <a name="rest-api-manage-logical-servers-and-databases"></a>REST-API: Logische servers en -databases beheren

Als u wilt maken en beheren van Azure SQL-server, databases en firewalls, gebruiken deze REST-API-aanvragen.

| Opdracht | Beschrijving |
| --- | --- |
|[Servers - maken of bijwerken](/rest/api/sql/servers/createorupdate)|Maken of bijwerken van een nieuwe server.|
|[Servers - verwijderen](/rest/api/sql/servers/delete)|Hiermee verwijdert u een SQL-server.|
|[Servers - Get](/rest/api/sql/servers/get)|Hiermee haalt u een server.|
|[Servers - lijst](/rest/api/sql/servers/list)|Retourneert een lijst met servers.|
|[Servers - lijst door resourcegroep](/rest/api/sql/servers/listbyresourcegroup)|Retourneert een lijst met servers in een resourcegroep.|
|[Servers - Update](/rest/api/sql/servers/update)|Een bestaande server-updates.|
|[Databases - maken of bijwerken](/rest/api/sql/databases/createorupdate)|Een nieuwe database maken of bijwerken van een bestaande database.|
|[Databases - Get](/rest/api/sql/databases/get)|Hiermee haalt u een database.|
|[Databases - verkrijgen door de elastische groep](/rest/api/sql/databases/getbyelasticpool)|Hiermee haalt u een database in een elastische pool.|
|[Databases - verkrijgen door aanbevolen elastische Pool](/rest/api/sql/databases/getbyrecommendedelasticpool)|Hiermee haalt u een database binnen een recommented elastische pool.|
|[Databases - lijst door de elastische groep](/rest/api/sql/databases/listbyelasticpool)|Retourneert een lijst met databases in een elastische pool.|
|[Databases - lijst met aanbevolen elastische Pool](/rest/api/sql/databases/listbyrecommendedelasticpool)|Retourneert een lijst met databases binnen een aanbevolen elastische pool.|
|[Databases - lijst per Server](/rest/api/sql/databases/listbyserver)|Retourneert een lijst met databases in een server.|
|[Databases - Update](/rest/api/sql/databases/update)|Een bestaande database-updates.|
|[Firewall-regels - maken of bijwerken](/rest/api/sql/firewallrules/createorupdate)|Maken of bijwerken van een firewallregel.|
|[Firewallregels - verwijderen](/rest/api/sql/firewallrules/delete)|Hiermee verwijdert u een firewallregel.|
|[Firewallregels - Get](/rest/api/sql/firewallrules/get)|Hiermee haalt u een firewallregel.|
|[Firewallregels - lijst per Server](/rest/api/sql/firewallrules/listbyserver)|Retourneert een lijst van firewallregels.|

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het migreren van een SQL Server-database naar Azure, [migreren naar Azure SQL Database](sql-database-cloud-migrate.md).
- Zie [Functies](sql-database-features.md) voor meer informatie over ondersteunde functies.
