---
title: Maken en beheren van Azure SQL-servers en individuele databases | Microsoft Docs
description: Meer informatie over het maken en beheren van logische servers en individuele databases.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 07/16/2018
ms.author: carlrab
ms.openlocfilehash: 1b431cdd49fb0ad32bfc8c06d731781afa38ac71
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39091886"
---
# <a name="create-and-manage-logical-servers-and-single-databases-in-azure-sql-database"></a>Logische servers en individuele databases in Azure SQL Database maken en beheren 

U kunt maken en beheren van Azure SQL-Databases logische servers en één met de Azure portal, PowerShell, Azure CLI, REST-API en Transact-SQL-databases.

## <a name="azure-portal-manage-logical-servers-and-databases"></a>Azure-portal: logische servers en databases beheren

U kunt de resourcegroep van de Azure SQL-database vooraf of tijdens het maken van de server zelf maken. Er zijn meerdere methoden voor het ophalen van een nieuwe SQL server-formulier, met het maken van een nieuwe SQL-server of als onderdeel van het maken van een nieuwe database. 

### <a name="create-a-blank-sql-server-logical-server"></a>Maak een lege SQL-server (logische server)

Het maken van een Azure SQL Database server (zonder een database) met de [Azure-portal](https://portal.azure.com), gaat u naar een leeg formulier van de SQL-server (logische server).  

### <a name="create-a-blank-or-sample-sql-database"></a>Maak een lege of voorbeeld SQL-database

Maken van een Azure SQL database met behulp van de [Azure-portal](https://portal.azure.com), gaat u naar een lege SQL-Database-formulier en geef de gevraagde informatie. U kunt de resourcegroep van de Azure SQL-database en de logische server tevoren of tijdens het maken van de database zelf maken. U kunt een lege database maken of maak een voorbeelddatabase gebaseerd op Adventure Works LT. 

  ![database-1 maken](./media/sql-database-get-started-portal/create-database-1.png)

> [!IMPORTANT]
> Zie voor meer informatie over het selecteren van de prijscategorie voor uw database [DTU gebaseerde aankoopmodel](sql-database-service-tiers-dtu.md) en [vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md).

Zie voor het maken van een beheerd exemplaar, [een beheerd exemplaar maken](sql-database-managed-instance-create-tutorial-portal.md)

### <a name="manage-an-existing-sql-server"></a>Een bestaande SQL server beheren

Een bestaande server beheren, gaat u naar de server met behulp van een aantal methoden - zoals van specifieke pagina voor SQL-database, de **SQL-servers** pagina of het **alle resources** pagina. 

Voor het beheren van een bestaande database, gaat u naar de **SQL-databases** pagina en klik op de database die u wilt beheren. De volgende schermafbeelding ziet u hoe u om te beginnen met het instellen van een firewall op serverniveau voor een database van de **overzicht** pagina voor een database. 

   ![serverfirewallregel](./media/sql-database-get-started-portal/server-firewall-rule.png) 

> [!IMPORTANT]
> Zie configureren van eigenschappen van de prestaties voor een database [DTU gebaseerde aankoopmodel](sql-database-service-tiers-dtu.md) en [vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md).
>

> [!TIP]
> Zie voor een Azure portal-quickstart, [maken van een Azure SQL database in Azure portal](sql-database-get-started-portal.md).

## <a name="powershell-manage-logical-servers-and-databases"></a>PowerShell: Logische servers en databases beheren

Als u wilt maken en beheren van Azure SQL-server, databases en firewalls met Azure PowerShell, gebruikt u de volgende PowerShell-cmdlets. Als u wilt installeren of upgraden van PowerShell, Zie [Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). 

> [!TIP]
> Zie voor een PowerShell-snelstartgids [maken van een individuele Azure SQL-database met behulp van PowerShell](sql-database-get-started-portal.md). Zie voor PowerShell-voorbeeldscripts, [PowerShell gebruiken om te maken van een individuele Azure SQL-database en een firewallregel configureren](scripts/sql-database-create-and-configure-database-powershell.md) en [bewaken en schalen van een enkele SQL-database met behulp van PowerShell](scripts/sql-database-monitor-and-scale-database-powershell.md).
>

| Cmdlet | Beschrijving |
| --- | --- |
|[New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase)|Hiermee maakt u een database |
|[Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)|Een of meer databases opgehaald|
|[Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase)|Hiermee stelt u eigenschappen voor een database of een bestaande database is verplaatst naar een elastische pool|
|[Remove-AzureRmSqlDatabase](/powershell/module/azurerm.sql/remove-azurermsqldatabase)|Hiermee verwijdert u een database|
|[New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup)|Hiermee maakt u een resourcegroep|
|[New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver)|Hiermee maakt u een server|
|[Get-AzureRmSqlServer](/powershell/module/azurerm.sql/get-azurermsqlserver)|Retourneert informatie over de servers|
|[Set-AzureRmSqlServer](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqlserver)|Hiermee wijzigt u de eigenschappen van een server|
|[Remove-AzureRmSqlServer](/powershell/module/azurerm.sql/remove-azurermsqlserver)|Hiermee verwijdert u een server|
|[New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule)|Hiermee maakt u een firewallregel op serverniveau |
|[Get-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/get-azurermsqlserverfirewallrule)|Hiermee haalt u firewallregels voor een server|
|[Set-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/set-azurermsqlserverfirewallrule)|Hiermee wijzigt u een firewallregel in een server|
|[Remove-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/remove-azurermsqlserverfirewallrule)|Hiermee verwijdert u een firewallregel van een server.|
| New-AzureRmSqlServerVirtualNetworkRule | Hiermee maakt u een [ *regel voor virtuele netwerken*](sql-database-vnet-service-endpoint-rule-overview.md), op basis van een subnet plaatsen dat is een service-eindpunt voor Virtueelnetwerk. |

## <a name="azure-cli-manage-logical-servers-and-databases"></a>Azure CLI: Logische servers en databases beheren

Maken en beheren van Azure SQL-server, databases en firewalls met [Azure CLI](/cli/azure), gebruikt u de volgende [SQL-Database van Azure CLI](/cli/azure/sql/db) opdrachten. Gebruik de [Cloud Shell](/azure/cloud-shell/overview) om de CLI in uw browser uit te voeren of [installeer](/cli/azure/install-azure-cli) de CLI op macOS, Linux of Windows. Zie voor het maken en beheren van elastische pools, [elastische pools](sql-database-elastic-pool.md).

> [!TIP]
> Zie voor een Azure CLI-Quick Start [maken van een individuele Azure SQL-database met de Azure CLI](sql-database-get-started-cli.md). Zie voor de Azure CLI-voorbeeldscripts, [CLI gebruiken om te maken van een individuele Azure SQL-database en een firewallregel configureren](scripts/sql-database-create-and-configure-database-cli.md) en [CLI gebruiken om te controleren en schalen van één SQL-database](scripts/sql-database-monitor-and-scale-database-cli.md).
>

| Cmdlet | Beschrijving |
| --- | --- |
|[az sql db create](/cli/azure/sql/db#az_sql_db_create) |Hiermee maakt u een database|
|[AZ sql db list](/cli/azure/sql/db#az_sql_db_list)|Geeft een lijst van alle databases en datawarehouses in een server of alle databases in een elastische pool|
|[AZ sql db-edities](/cli/azure/sql/db#az_sql_db_list_editions)|Een lijst met beschikbare service doelstellingen en opslagbeperkingen|
|[AZ sql db list-usages](/cli/azure/sql/db#az_sql_db_list_usages)|Retourneert database het gebruik van|
|[AZ sql db show](/cli/azure/sql/db#az_sql_db_show)|Een database of het datawarehouse opgehaald|
|[az sql db update](/cli/azure/sql/db#az_sql_db_update)|Een database-updates|
|[AZ sql db delete](/cli/azure/sql/db#az_sql_db_delete)|Hiermee verwijdert u een database|
|[az group create](/cli/azure/group#az_group_create)|Hiermee maakt u een resourcegroep|
|[az sql server create](/cli/azure/sql/server#az_sql_server_create)|Hiermee maakt u een server|
|[lijst met AZ sql server](/cli/azure/sql/server#az_sql_server_list)|Een lijst met servers|
|[AZ sql server list-usages](/cli/azure/sql/server#az_sql_server_list_usages)|Retourneert het gebruik van server|
|[AZ sql server show](/cli/azure/sql/server#az_sql_server_show)|Een server opgehaald|
|[AZ sql server-update](/cli/azure/sql/server#az_sql_server_update)|Een server worden bijgewerkt|
|[AZ sql server verwijderen](/cli/azure/sql/server#az_sql_server_delete)|Hiermee verwijdert u een server|
|[AZ sql server firewall-regel maken](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_create)|Hiermee maakt u een serverfirewallregel|
|[AZ sql server firewall-rule list](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_list)|Geeft een lijst van de firewall-regels op een server|
|[AZ sql server firewall-rule show](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_show)|Geeft de details van een firewall-regel|
|[AZ sql server firewall-rule update](/cli/azure/sql/server/firewall-rule##az_sql_server_firewall_rule_update)|Een firewallregel bijgewerkt|
|[AZ sql server firewall-rule delete](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_delete)|Hiermee verwijdert u een firewall-regel|

## <a name="transact-sql-manage-logical-servers-and-databases"></a>Transact-SQL: Logische servers en databases beheren

Als u wilt maken en beheren van Azure SQL-server, databases en firewalls met Transact-SQL, moet u de volgende T-SQL-opdrachten gebruiken. U kunt deze opdrachten met de Azure-portal, de opdracht [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Visual Studio Code](https://code.visualstudio.com/docs), of andere programma's die kan verbinding maken met een Azure SQL Database-server en doorgeven van Transact-SQL de opdrachten. Zie voor het beheren van elastische pools, [elastische pools](sql-database-elastic-pool.md).


> [!TIP]
> Zie voor een snelstartgids met behulp van SQL Server Management Studio op Microsoft Windows, [Azure SQL Database: gebruikt SQL Server Management Studio om verbinding maken met en gegevens op te vragen](sql-database-connect-query-ssms.md). Zie voor een snelstartgids met behulp van Visual Studio Code op de Mac OS, Linux of Windows, [Azure SQL Database: Visual Studio-Code gebruiken om te verbinden en gegevens op te vragen](sql-database-connect-query-vscode.md).

> [!IMPORTANT]
> U kunt maken of verwijderen van een server met behulp van Transact-SQL.
>

| Opdracht | Beschrijving |
| --- | --- |
|[DATABASE (Azure SQL Database) maken](/sql/t-sql/statements/create-database-azure-sql-database)|Hiermee maakt u een nieuwe database. U moet zijn verbonden met de hoofddatabase om een nieuwe database te maken.|
| [ALTER DATABASE (Azure SQL Database)](/sql/t-sql/statements/alter-database-azure-sql-database) |Hiermee wijzigt u een Azure SQL database. |
|[ALTER DATABASE (Azure SQL datawarehouse)](/sql/t-sql/statements/alter-database-azure-sql-data-warehouse)|Hiermee wijzigt u een Azure SQL datawarehouse.|
|[DROP DATABASE (Transact-SQL)](/sql/t-sql/statements/drop-database-transact-sql)|Hiermee verwijdert u een database.|
|[sys.database_service_objectives (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-service-objectives-azure-sql-database)|Retourneert de edition (servicelaag), de servicedoelstelling (prijscategorie) en de naam van de elastische groep, indien aanwezig, voor een Azure SQL-database of een Azure SQL Data Warehouse. Als u aangemeld bent op de database master in Azure SQL Database-server, retourneert de informatie voor alle databases. Voor Azure SQL Data Warehouse, moet u zijn verbonden met de hoofddatabase.|
|[sys.dm_db_resource_stats (Azure SQL Database)](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database)| Retourneert verbruik van CPU, IO en geheugen voor een Azure SQL Database-database. Er bestaat één rij voor elke 15 seconden, zelfs als er geen activiteit in de database.|
|[sys.resource_stats (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database)|CPU-gebruik en de opslag-gegevens geretourneerd voor een Azure SQL Database. De gegevens worden verzameld en samengevoegd binnen vijf minuten durende intervallen.|
|[sys.database_connection_stats (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-connection-stats-azure-sql-database)|Statistieken voor SQL Database-database-connectiviteitsgebeurtenissen, biedt een overzicht van de database verbinding successen en mislukkingen bevat. |
|[sys.event_log (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-event-log-azure-sql-database)|Geslaagde verbindingen in Azure SQL Database-database, verbindingsfouten en impassen geretourneerd. U kunt deze informatie gebruiken bij te houden of oplossen van de activiteit van uw database met SQL Database.|
|[sp_set_firewall_rule (Azure SQL Database)](/sql/relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database)|Hiermee of de instellingen van de firewall op serverniveau voor uw SQL Database-server bijgewerkt. Deze opgeslagen procedure is alleen beschikbaar in de hoofddatabase voor de principal-aanmelding op serverniveau. Een firewallregel op serverniveau kan alleen worden gemaakt met behulp van Transact-SQL nadat de eerste firewallregel op serverniveau is gemaakt door een gebruiker met machtigingen op Azure-niveau|
|[sys.firewall_rules (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-firewall-rules-azure-sql-database)|Retourneert informatie over de firewall op serverniveau-instellingen die zijn gekoppeld aan uw Microsoft Azure SQL-Database.|
|[sp_delete_firewall_rule (Azure SQL Database)](/sql/relational-databases/system-stored-procedures/sp-delete-firewall-rule-azure-sql-database)|Hiermee verwijdert u de instellingen van de firewall op serverniveau van uw SQL-Database-server. Deze opgeslagen procedure is alleen beschikbaar in de hoofddatabase voor de principal-aanmelding op serverniveau.|
|[sp_set_database_firewall_rule (Azure SQL Database)](/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database)|Hiermee of bijwerken van de firewallregels op databaseniveau voor uw Azure SQL Database of SQL Data Warehouse. Database-firewall-regels kunnen worden geconfigureerd voor de database master en gebruikersdatabases op SQL-Database. Database-firewallregels zijn nuttig wanneer u met behulp van ingesloten databasegebruikers. |
|[sys.database_firewall_rules (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-firewall-rules-azure-sql-database)|Retourneert informatie over de firewall op databaseniveau-instellingen die zijn gekoppeld aan uw Microsoft Azure SQL-Database. |
|[sp_delete_database_firewall_rule (Azure SQL Database)](/sql/relational-databases/system-stored-procedures/sp-delete-database-firewall-rule-azure-sql-database)|Hiermee verwijdert u de firewall op databaseniveau instellen van uw Azure SQL Database of SQL Data Warehouse. |



## <a name="rest-api-manage-logical-servers-and-databases"></a>REST-API: Beheer van logische servers en databases

Als u wilt maken en beheren van Azure SQL-server, databases en firewalls, gebruikt u deze REST-API-aanvragen.

| Opdracht | Beschrijving |
| --- | --- |
|[Servers - maken of bijwerken](/rest/api/sql/servers/createorupdate)|Hiermee of een nieuwe server bijgewerkt.|
|[Servers - verwijderen](/rest/api/sql/servers/delete)|Hiermee verwijdert u een SQL-server.|
|[Servers - Get](/rest/api/sql/servers/get)|Hiermee haalt u een server.|
|[Servers - lijst](/rest/api/sql/servers/list)|Retourneert een lijst met servers.|
|[Servers - lijst per groep](/rest/api/sql/servers/listbyresourcegroup)|Retourneert een lijst met servers in een resourcegroep.|
|[Servers - Update](/rest/api/sql/servers/update)|Een bestaande server worden bijgewerkt.|
|[Databases - maken of bijwerken](/rest/api/sql/databases/createorupdate)|Maakt een nieuwe database gemaakt of bijgewerkt van een bestaande database.|
|[Databases - Get](/rest/api/sql/databases/get)|Hiermee haalt u een database.|
|[Databases - ophalen door de elastische Pool](/rest/api/sql/databases/getbyelasticpool)|Hiermee haalt u een database in een elastische pool.|
|[Databases - opvragen aanbevolen elastische Pool](/rest/api/sql/databases/getbyrecommendedelasticpool)|Hiermee haalt u een database binnen een recommented elastische pool.|
|[Databases - lijst op de elastische Pool](/rest/api/sql/databases/listbyelasticpool)|Retourneert een lijst met databases in een elastische pool.|
|[Databases - lijst met aanbevolen elastische Pool](/rest/api/sql/databases/listbyrecommendedelasticpool)|Retourneert een lijst met databases binnen een aanbevolen elastische pool.|
|[Databases - lijst met door de Server](/rest/api/sql/databases/listbyserver)|Retourneert een lijst met databases in een server.|
|[Databases - Update](/rest/api/sql/databases/update)|Werkt een bestaande database.|
|[Firewall-regels - maken of bijwerken](/rest/api/sql/firewallrules/createorupdate)|Hiermee of een firewall-regel bijgewerkt.|
|[Firewall-regels - verwijderen](/rest/api/sql/firewallrules/delete)|Hiermee verwijdert u een firewallregel.|
|[Firewall-regels - Get](/rest/api/sql/firewallrules/get)|Hiermee haalt u een firewallregel.|
|[Firewall-regels - lijst met door de Server](/rest/api/sql/firewallrules/listbyserver)|Retourneert een lijst van firewall-regels.|

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het migreren van een SQL Server-database naar Azure, [migreren naar Azure SQL Database](sql-database-cloud-migrate.md).
- Zie [Functies](sql-database-features.md) voor meer informatie over ondersteunde functies.
