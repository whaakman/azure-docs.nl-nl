---
title: Maken en beheren van Azure SQL-servers en databases | Microsoft Docs
description: Meer informatie over Azure SQL Database-server en database-concepten en over het maken en beheren van servers en -databases.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 04/10/2018
ms.author: carlrab
ms.openlocfilehash: 2600e39dec91fc6916fa7bbd02e318d33cfa3c99
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34649054"
---
# <a name="create-and-manage-azure-sql-database-servers-and-databases"></a>Azure SQL Database-servers en databases maken en beheren

SQL-Database biedt drie typen databases:

- Een individuele database gemaakt binnen een [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) met een [gecombineerd berekenings-en opslagbronnen set](sql-database-service-tiers-dtu.md) of een [onafhankelijk van de schaal van de berekenings- en resources](sql-database-service-tiers-vcore.md). Een Azure SQL database is gekoppeld aan een logische Azure SQL Database-server op, die wordt gemaakt binnen een specifieke Azure-regio.
- Een database gemaakt als onderdeel van een [pool van databases](sql-database-elastic-pool.md) binnen een [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) met een [gecombineerd aantal resources voor berekeningen en opslag (DTU gebaseerd)](sql-database-service-tiers-dtu.md) of een [onafhankelijk van de schaal van de berekenings- en -resources (vCore gebaseerd)](sql-database-service-tiers-vcore.md) die worden gedeeld door alle databases in de groep. Een Azure SQL database is gekoppeld aan een logische Azure SQL Database-server op, die wordt gemaakt binnen een specifieke Azure-regio.
- Een [exemplaar van een SQL-server](sql-database-managed-instance.md) (een beheerde exemplaar) gemaakt binnen een [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) met een gedefinieerde set berekenings- en resources voor alle databases op die server-exemplaar. Een beheerde exemplaar bevat zowel systeem- en gebruikersdatabases. Beheerde exemplaar is ontworpen om in te schakelen database lift-en-verschuiving naar een volledig beheerde PaaS zonder de toepassing opnieuw. Beheerde exemplaar biedt hoge compatibiliteit met het lokale SQL Server-programmeermodel en ondersteunt de grote meerderheid van SQL Server-functies en bijbehorende hulpprogramma's en services.  

Microsoft Azure SQL Database ondersteunt tabular data stream (TDS)-protocol clientversie 7.3 of hoger en alleen versleutelde TCP/IP-verbindingen.

> [!IMPORTANT]
> Exemplaar van SQL Database-beheerd biedt die momenteel in de openbare preview een enkel servicelaag voor algemene doeleinden. Zie [SQL Database Managed Instance](sql-database-managed-instance.md) voor meer informatie. De rest van dit artikel niet van toepassing op beheerde exemplaar.

## <a name="what-is-an-azure-sql-logical-server"></a>Wat is een logische Azure SQL-server?

Een logische server fungeert als een administratieve middelpunt voor meerdere één of [gegroepeerde](sql-database-elastic-pool.md) databases, [aanmeldingen](sql-database-manage-logins.md), [firewall-regels](sql-database-firewall-configure.md), [regelscontrole](sql-database-auditing.md), [dreiging beleidsregels](sql-database-threat-detection.md), en [failover groepen](sql-database-geo-replication-overview.md). Een logische server kan zich in een andere regio dan de resourcegroep. De logische server moet bestaan voordat u de Azure SQL database kunt maken. Alle databases op een server zijn gemaakt in dezelfde regio bevinden als de logische server.

Een logische server is een logische constructie die verschilt van een SQL Server-exemplaar dat u mogelijk kent met in de lokale wereld. Zo geeft de service SQL Database geen garanties met betrekking tot de locatie van de databases ten opzichte van hun logische servers en zijn er geen toegang of functies op exemplaarniveau zichtbaar. Daarentegen is een server in een SQL-Database beheerd-exemplaar vergelijkbaar met een SQL Server-exemplaar dat u mogelijk kent met in de lokale wereld.

Wanneer u een logische server maakt, Geef een beheerserver aanmeldingsaccount en het wachtwoord die beheerdersrechten voor de database master op die server en alle databases die zijn gemaakt op die server heeft. Dit eerste account is een SQL-aanmeldings-account. Azure SQL Database ondersteunt SQL-verificatie en Azure Active Directory-verificatie voor verificatie. Zie voor informatie over aanmeldingen en verificatie, [het beheren van Databases en aanmeldingen in Azure SQL Database](sql-database-manage-logins.md). Windows-verificatie wordt niet ondersteund. 

> [!TIP]
> Zie voor een geldige groep en server Resourcenamen [naamgeving van regels en beperkingen](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).
>

Een logische server met Azure Database:

- Wordt gemaakt binnen een Azure-abonnement, maar kan met ingesloten resources naar een ander abonnement worden verplaatst
- Is de bovenliggende resource voor databases, elastische groepen en datawarehouses
- Biedt een naamruimte voor de databases en elastische pools datawarehouses
- Een logische container is een server met sterke levensduur semantiek - verwijderen en de ingesloten databases, elastische pools en datawarehouses worden verwijderd
- Maakt deel uit van [Azure op rollen gebaseerde toegangsbeheer (RBAC)](/azure/role-based-access-control/overview) -databases en elastische pools datawarehouses binnen een server rechten overnemen van de server
- Is een hogere-element van de identiteit van de databases en elastische pools datawarehouses voor Azure resource management-toepassing (Zie het URL-schema voor de databases en pools)
- Groepeert resources in een regio
- Biedt een verbindingseindpunt voor databasetoegang (<serverName>.database.windows.net)
- Biedt toegang tot metagegevens van ingesloten resources via DMV's door verbinding te maken met een hoofddatabase 
- Voorziet in het bereik voor management-beleidsregels die van toepassing op de databases - aanmeldingen, firewall-, controleren, dreiging detectie, enz. 
- Wordt beperkt door een quotum binnen het bovenliggende abonnement (zes servers per abonnement standaard - [Zie abonnement hier beperkt](../azure-subscription-service-limits.md))
- Voorziet in het bereik voor de databasequotum en DTU of vCore quotum voor de resources die deze (zoals 45.000 DTU bevat)
- Het bereik versiebeheer voor mogelijkheden ingeschakeld op ingesloten bronnen 
- Hoofdaanmeldingen op serverniveau kunnen alle databases op een server beheren
- Kunnen aanmeldingen bevatten die vergelijkbaar zijn met die in de exemplaren van SQL Server op uw locaties die toegang hebben tot een of meer databases op de server, en waaraan beperkte beheerrechten kunnen worden verleend. Zie [Aanmeldingen](sql-database-manage-logins.md) voor meer informatie.
- De standaardsortering voor alle gebruikersdatabases gemaakt op een logische server is `SQL_LATIN1_GENERAL_CP1_CI_AS`, waarbij `LATIN1_GENERAL` Engels (Verenigde Staten), `CP1` is codetabel 1252, `CI` is niet hoofdlettergevoelig, en `AS` accentgevoelig is.

## <a name="azure-sql-databases-protected-by-sql-database-firewall"></a>Azure SQL databases hebt beveiligd met SQL Database-firewall

Om uw gegevens te beveiligen een [SQL Database-firewall](sql-database-firewall-configure.md) voorkomt u dat alle toegang tot de database-server of een van de databases van buiten uw verbinding met de server die rechtstreeks via de verbinding van uw Azure-abonnement. Om extra connectiviteit inschakelt, moet u [maken van een of meer firewallregels](sql-database-firewall-configure.md#creating-and-managing-firewall-rules). Zie voor het maken en beheren van elastische pools, [elastische pools](sql-database-elastic-pool.md).

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-the-azure-portal"></a>Azure SQL-servers, databases, en firewalls beheren met behulp van de Azure-portal

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
>

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-powershell"></a>Azure SQL-servers, databases, en firewalls beheren met behulp van PowerShell

Gebruik de volgende PowerShell-cmdlets voor het maken en beheren van Azure SQL-server, databases en firewalls met Azure PowerShell. Als u wilt installeren of upgraden van PowerShell, Zie [Installeer Azure PowerShell-module](/powershell/azure/install-azurerm-ps). Zie voor het maken en beheren van elastische pools, [elastische pools](sql-database-elastic-pool.md).

| Cmdlet | Beschrijving |
| --- | --- |
|[New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase)|Maakt een database |
|[Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)|Een of meer databases opgehaald|
|[Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase)|Stelt eigenschappen van een database of een bestaande database is verplaatst naar een elastische pool|
|[Remove-AzureRmSqlDatabase](/powershell/module/azurerm.sql/remove-azurermsqldatabase)|Hiermee verwijdert u een database|
|[New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup)|Maakt een resourcegroep]
|[New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver)|Hiermee maakt u een server|
|[Get-AzureRmSqlServer](/powershell/module/azurerm.sql/get-azurermsqlserver)|Retourneert informatie over servers|
|[Set-AzureRmSqlServer](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqlserver)|Hiermee wijzigt u de eigenschappen van een server|
|[Remove-AzureRmSqlServer](/powershell/module/azurerm.sql/remove-azurermsqlserver)|Hiermee verwijdert u een server|
|[New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule)|Hiermee maakt u een firewallregel op serverniveau |
|[Get-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/get-azurermsqlserverfirewallrule)|Firewallregels voor server opgehaald|
|[Set-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/set-azurermsqlserverfirewallrule)|Hiermee wijzigt u een firewallregel in een server|
|[Remove-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/remove-azurermsqlserverfirewallrule)|Hiermee verwijdert u een firewallregel van een server.|
| New-AzureRmSqlServerVirtualNetworkRule | Maakt een [ *virtueel netwerk regel*](sql-database-vnet-service-endpoint-rule-overview.md), op basis van een subnet plaatsen dat is een service-eindpunt van het virtuele netwerk. |

> [!TIP]
> Zie voor een PowerShell-Quick Start [maken van één Azure SQL database met behulp van PowerShell](sql-database-get-started-portal.md). Zie voor PowerShell-voorbeeldscripts, [Gebruik PowerShell één Azure SQL database maken en configureren van een firewallregel](scripts/sql-database-create-and-configure-database-powershell.md) en [bewaken en schalen van een enkele SQL-database met behulp van PowerShell](scripts/sql-database-monitor-and-scale-database-powershell.md).
>

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-the-azure-cli"></a>Azure SQL-servers, databases, en firewalls beheren met de Azure CLI

Maken en beheren van Azure SQL-server, databases en firewalls met de [Azure CLI](/cli/azure), gebruikt u de volgende [Azure CLI SQL Database](/cli/azure/sql/db) opdrachten. Gebruik de [Cloud Shell](/azure/cloud-shell/overview) om de CLI in uw browser uit te voeren of [installeer](/cli/azure/install-azure-cli) de CLI op macOS, Linux of Windows. Zie voor het maken en beheren van elastische pools, [elastische pools](sql-database-elastic-pool.md).

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

> [!TIP]
> Zie voor een Azure CLI-Quick Start [maken van één Azure SQL database met de Azure CLI](sql-database-get-started-cli.md). Zie voor Azure CLI-voorbeeldscripts, [CLI gebruik één Azure SQL database maken en configureren van een firewallregel](scripts/sql-database-create-and-configure-database-cli.md) en [gebruik CLI bewaken en schalen van een enkele SQL-database](scripts/sql-database-monitor-and-scale-database-cli.md).
>

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-transact-sql"></a>Azure SQL-servers, databases, en firewalls beheren met behulp van Transact-SQL

Gebruik de volgende T-SQL-opdrachten voor het maken en beheren van Azure SQL-server, databases en firewalls met Transact-SQL. U kunt deze opdrachten met de Azure portal, de opdracht [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Visual Studio Code](https://code.visualstudio.com/docs), of een ander programma dat kan verbinding maken met een Azure SQL Database-server en doorgeven Transact-SQL-opdrachten. Zie voor het beheren van elastische pools [elastische pools](sql-database-elastic-pool.md).

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


> [!TIP]
> Zie voor een snel aan de slag met SQL Server Management Studio op Microsoft Windows, [Azure SQL Database: SQL Server Management Studio gebruiken om verbinding te en een query over gegevens](sql-database-connect-query-ssms.md). Zie voor een snel aan de slag met Visual Studio Code op de Mac OS-, Linux- of Windows, [Azure SQL Database: Gebruik Visual Studio Code verbinding maakt en gegevens opvragen](sql-database-connect-query-vscode.md).

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-the-rest-api"></a>Azure SQL-servers, databases, en firewalls beheren met behulp van de REST-API

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
