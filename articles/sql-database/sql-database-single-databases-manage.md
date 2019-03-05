---
title: Maken en beheren van Azure SQL Database-servers en individuele databases | Microsoft Docs
description: Meer informatie over het maken en beheren van SQL Database-servers en individuele databases.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 02/08/2019
ms.openlocfilehash: 531597f4cd66ba3250ef5e62e64ef36916b1014b
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/04/2019
ms.locfileid: "57317081"
---
# <a name="create-and-manage-sql-database-servers-and-single-databases-in-azure-sql-database"></a>SQL Database-servers en individuele databases in Azure SQL Database maken en beheren

U kunt maken en beheren van SQL Database-servers en één met de Azure portal, PowerShell, Azure CLI, REST-API en Transact-SQL-databases.

## <a name="azure-portal-manage-sql-database-servers-and-single-databases"></a>Azure Portal: SQL Database-servers en individuele databases beheren

U kunt de resourcegroep van de Azure SQL-database vooraf of tijdens het maken van de server zelf maken. Er zijn meerdere methoden voor het ophalen van een nieuwe SQL server-formulier, met het maken van een nieuwe SQL-server of als onderdeel van het maken van een nieuwe database.

### <a name="create-a-blank-sql-database-server"></a>Een lege SQL-databaseserver maken

Het maken van een SQL Database server met de [Azure-portal](https://portal.azure.com), gaat u naar een leeg formulier van de SQL-server (logische server).  

### <a name="create-a-blank-or-sample-sql-single-database"></a>Een lege of voorbeeld SQL individuele database maken

Het maken van een Azure SQL database met met de [Azure-portal](https://portal.azure.com), gaat u naar een lege SQL-Database-formulier en geef de gevraagde informatie. U kunt de resourcegroep en de SQL-databaseserver tevoren de Azure SQL-database maken of bij het maken van de ene database zelf. U kunt een lege database maken of maak een voorbeelddatabase gebaseerd op Adventure Works LT.

  ![database-1 maken](./media/sql-database-get-started-portal/create-database-1.png)

> [!IMPORTANT]
> Zie voor meer informatie over het selecteren van de prijscategorie voor uw database [DTU gebaseerde aankoopmodel](sql-database-service-tiers-dtu.md) en [vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md).

Zie voor het maken van een beheerd exemplaar, [een beheerd exemplaar maken](sql-database-managed-instance-get-started.md)

## <a name="manage-an-existing-sql-database-server"></a>Een bestaande SQL Database-server beheren

Navigeer naar de server met behulp van een aantal methoden - zoals van specifieke pagina voor SQL-database, het beheren van een bestaande SQL Database-server de **SQL-servers** pagina of het **alle resources** pagina.

Voor het beheren van een bestaande database, gaat u naar de **SQL-databases** pagina en klik op de database die u wilt beheren. De volgende schermafbeelding ziet u hoe u om te beginnen met het instellen van een firewall op serverniveau voor een database van de **overzicht** pagina voor een database.

   ![serverfirewallregel](./media/sql-database-get-started-portal/server-firewall-rule.png)

> [!IMPORTANT]
> Zie configureren van eigenschappen van de prestaties voor een database [DTU gebaseerde aankoopmodel](sql-database-service-tiers-dtu.md) en [vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md).
> [!TIP]
> Zie voor een Azure portal-quickstart, [maken van een Azure SQL database in Azure portal](sql-database-single-database-get-started.md).

## <a name="powershell-manage-sql-database-servers-and-single-databases"></a>PowerShell: SQL Database-servers en individuele databases beheren

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Als u wilt maken en beheren van Azure SQL Database-servers, één en gegroepeerde databases en firewalls voor SQL Database-server met Azure PowerShell, gebruikt u de volgende PowerShell-cmdlets. Als u wilt installeren of upgraden van PowerShell, Zie [Azure PowerShell-module installeren](/powershell/azure/install-az-ps).

> [!TIP]
> Zie voor PowerShell-voorbeeldscripts, [PowerShell gebruiken om te maken van een individuele Azure SQL-database en een firewallregel voor SQL Database-server configureren](scripts/sql-database-create-and-configure-database-powershell.md) en [bewaken en schalen van een SQL database met behulp van PowerShell enkel](scripts/sql-database-monitor-and-scale-database-powershell.md) .

| Cmdlet | Description |
| --- | --- |
|[New-AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase)|Hiermee maakt u een database |
|[Get-AzSqlDatabase](/powershell/module/az.sql/get-azsqldatabase)|Een of meer databases opgehaald|
|[Set-AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase)|Hiermee stelt u eigenschappen voor een database of een bestaande database is verplaatst naar een elastische pool|
|[Remove-AzSqlDatabase](/powershell/module/az.sql/remove-azsqldatabase)|Hiermee verwijdert u een database|
|[New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup)|Hiermee maakt u een resourcegroep|
|[New-AzSqlServer](/powershell/module/az.sql/new-azsqlserver)|Hiermee maakt u een server|
|[Get-AzSqlServer](/powershell/module/az.sql/get-azsqlserver)|Retourneert informatie over de servers|
|[Set-AzSqlServer](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlserver)|Hiermee wijzigt u de eigenschappen van een server|
|[Remove-AzSqlServer](/powershell/module/az.sql/remove-azsqlserver)|Hiermee verwijdert u een server|
|[New-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule)|Hiermee maakt u een firewallregel op serverniveau |
|[Get-AzSqlServerFirewallRule](/powershell/module/az.sql/get-azsqlserverfirewallrule)|Hiermee haalt u firewallregels voor een server|
|[Set-AzSqlServerFirewallRule](/powershell/module/az.sql/set-azsqlserverfirewallrule)|Hiermee wijzigt u een firewallregel in een server|
|[Remove-AzSqlServerFirewallRule](/powershell/module/az.sql/remove-azsqlserverfirewallrule)|Hiermee verwijdert u een firewallregel van een server.|
| New-AzSqlServerVirtualNetworkRule | Hiermee maakt u een [ *regel voor virtuele netwerken*](sql-database-vnet-service-endpoint-rule-overview.md), op basis van een subnet plaatsen dat is een service-eindpunt voor Virtueelnetwerk. |

## <a name="azure-cli-manage-sql-database-servers-and-single-databases"></a>Azure CLI: SQL Database-servers en individuele databases beheren

Maken en beheren van Azure SQL-server, databases en firewalls met [Azure CLI](/cli/azure), gebruikt u de volgende [SQL-Database van Azure CLI](/cli/azure/sql/db) opdrachten. Gebruik de [Cloud Shell](/azure/cloud-shell/overview) om de CLI in uw browser uit te voeren of [installeer](/cli/azure/install-azure-cli) de CLI op macOS, Linux of Windows. Zie voor het maken en beheren van elastische pools, [elastische pools](sql-database-elastic-pool.md).

> [!TIP]
> Zie voor een Azure CLI-Quick Start [maken van een enkele Azure SQL-database met de Azure CLI](sql-database-cli-samples.md). Zie voor de Azure CLI-voorbeeldscripts, [met CLI een individuele Azure SQL-database maken en configureren van een SQL-Database-firewallregel](scripts/sql-database-create-and-configure-database-cli.md) en [CLI gebruiken om te controleren en schalen van één Azure SQL-database](scripts/sql-database-monitor-and-scale-database-cli.md).
>

| Cmdlet | Description |
| --- | --- |
|[az sql db create](/cli/azure/sql/db#az-sql-db-create) |Hiermee maakt u een database|
|[AZ sql db list](/cli/azure/sql/db#az-sql-db-list)|Geeft een lijst van alle databases en datawarehouses in een server of alle databases in een elastische pool|
|[az sql db list-editions](/cli/azure/sql/db#az-sql-db-list-editions)|Een lijst met beschikbare service doelstellingen en opslagbeperkingen|
|[az sql db list-usages](/cli/azure/sql/db#az-sql-db-list-usages)|Retourneert database het gebruik van|
|[AZ sql db show](/cli/azure/sql/db#az-sql-db-show)|Een database of het datawarehouse opgehaald|
|[az sql db update](/cli/azure/sql/db#az-sql-db-update)|Een database-updates|
|[AZ sql db delete](/cli/azure/sql/db#az-sql-db-delete)|Hiermee verwijdert u een database|
|[az group create](/cli/azure/group#az-group-create)|Hiermee maakt u een resourcegroep|
|[az sql server create](/cli/azure/sql/server#az-sql-server-create)|Hiermee maakt u een server|
|[lijst met AZ sql server](/cli/azure/sql/server#az-sql-server-list)|Een lijst met servers|
|[AZ sql server list-usages](/cli/azure/sql/server#az-sql-server-list-usages)|Retourneert het gebruik van server|
|[AZ sql server show](/cli/azure/sql/server#az-sql-server-show)|Een server opgehaald|
|[AZ sql server-update](/cli/azure/sql/server#az-sql-server-update)|Een server worden bijgewerkt|
|[AZ sql server verwijderen](/cli/azure/sql/server#az-sql-server-delete)|Hiermee verwijdert u een server|
|[AZ sql server firewall-regel maken](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-create)|Hiermee maakt u een serverfirewallregel|
|[AZ sql server firewall-rule list](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-list)|Geeft een lijst van de firewall-regels op een server|
|[AZ sql server firewall-rule show](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-show)|Geeft de details van een firewall-regel|
|[AZ sql server firewall-rule update](/cli/azure/sql/server/firewall-rule##az-sql-server-firewall-rule-update)|Een firewallregel bijgewerkt|
|[AZ sql server firewall-rule delete](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-delete)|Hiermee verwijdert u een firewall-regel|

## <a name="transact-sql-manage-sql-database-servers-and-single-databases"></a>Transact-SQL: SQL Database-servers en individuele databases beheren

Als u wilt maken en beheren van Azure SQL-server, databases en firewalls met Transact-SQL, moet u de volgende T-SQL-opdrachten gebruiken. U kunt deze opdrachten met de Azure-portal, de opdracht [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Visual Studio Code](https://code.visualstudio.com/docs), of andere programma's die kan verbinding maken met een Azure SQL Database-server en doorgeven van Transact-SQL de opdrachten. Zie voor het beheren van elastische pools, [elastische pools](sql-database-elastic-pool.md).

> [!TIP]
> Zie voor een snelstartgids met behulp van SQL Server Management Studio op Microsoft Windows, [Azure SQL Database: SQL Server Management Studio gebruiken om te verbinden en gegevens op te vragen](sql-database-connect-query-ssms.md). Zie voor een snelstartgids met behulp van Visual Studio Code op de Mac OS, Linux of Windows, [Azure SQL Database: Visual Studio Code gebruiken om te verbinden en gegevens op te vragen](sql-database-connect-query-vscode.md).
> [!IMPORTANT]
> U kunt maken of verwijderen van een server met behulp van Transact-SQL.

| Opdracht | Description |
| --- | --- |
|[CREATE DATABASE](https://docs.microsoft.com/sql/t-sql/statements/create-database-transact-sql?view=azuresqldb-current)|Hiermee maakt u een nieuwe database. U moet zijn verbonden met de hoofddatabase om een nieuwe database te maken.|
| [ALTER DATABASE (Azure SQL Database)](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current) |Hiermee wijzigt u een Azure SQL database. |
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

## <a name="rest-api-manage-sql-database-servers-and-single-databases"></a>REST-API: SQL Database-servers en individuele databases beheren

Als u wilt maken en beheren van Azure SQL-server, databases en firewalls, gebruikt u deze REST-API-aanvragen.

| Opdracht | Description |
| --- | --- |
|[Servers - maken of bijwerken](https://docs.microsoft.com/rest/api/sql/servers/createorupdate)|Hiermee of een nieuwe server bijgewerkt.|
|[Servers - verwijderen](https://docs.microsoft.com/rest/api/sql/servers/delete)|Hiermee verwijdert u een SQL-server.|
|[Servers - Get](https://docs.microsoft.com/rest/api/sql/servers/get)|Hiermee haalt u een server.|
|[Servers - lijst](https://docs.microsoft.com/rest/api/sql/servers/list)|Retourneert een lijst met servers in een abonnement.|
|[Servers - lijst per groep](https://docs.microsoft.com/rest/api/sql/servers/listbyresourcegroup)|Retourneert een lijst met servers in een resourcegroep.|
|[Servers - Update](https://docs.microsoft.com/rest/api/sql/servers/update)|Een bestaande server worden bijgewerkt.|
|[Databases - maken of bijwerken](https://docs.microsoft.com/rest/api/sql/databases/createorupdate)|Maakt een nieuwe database gemaakt of bijgewerkt van een bestaande database.|
|[Databases - verwijderen](https://docs.microsoft.com/rest/api/sql/databases/delete)|Hiermee verwijdert u een database.|
|[Databases - Get](https://docs.microsoft.com/rest/api/sql/databases/get)|Hiermee haalt u een database.|
|[Databases - lijst op de elastische pool](https://docs.microsoft.com/rest/api/sql/databases/listbyelasticpool)|Retourneert een lijst met databases in een elastische pool.|
|[Databases - lijst met door de server](https://docs.microsoft.com/rest/api/sql/databases/listbyserver)|Retourneert een lijst met databases in een server.|
|[Databases - Update](https://docs.microsoft.com/rest/api/sql/databases/update)|Werkt een bestaande database.|
|[Firewall-regels - maken of bijwerken](https://docs.microsoft.com/rest/api/sql/firewallrules/createorupdate)|Hiermee of een firewall-regel bijgewerkt.|
|[Firewall-regels - verwijderen](https://docs.microsoft.com/rest/api/sql/firewallrules/delete)|Hiermee verwijdert u een firewallregel.|
|[Firewall-regels - Get](https://docs.microsoft.com/rest/api/sql/firewallrules/get)|Hiermee haalt u een firewallregel.|
|[Firewall-regels - lijst met door de server](https://docs.microsoft.com/rest/api/sql/firewallrules/listbyserver)|Retourneert een lijst van firewall-regels.|

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het migreren van een SQL Server-database naar Azure, [migreren naar Azure SQL Database](sql-database-single-database-migrate.md).
- Zie [Functies](sql-database-features.md) voor meer informatie over ondersteunde functies.
