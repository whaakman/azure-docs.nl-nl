---
title: Actieve geo-replicatie - Azure SQL Database | Microsoft Docs
description: Actieve geo-replicatie gebruiken om te maken van leesbare secundaire databases van afzonderlijke databases in het hetzelfde of een ander datacenter (regio).
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
manager: craigg
ms.date: 03/26/2019
ms.openlocfilehash: ca53f4bfa80d6fdead24dc7d562c2240bb3fa86d
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58498482"
---
# <a name="creating-and-using-active-geo-replication"></a>Het maken en gebruiken van actieve geo-replicatie

Actieve geo-replicatie is Azure SQL Database-functie waarmee u leesbare secundaire databases van afzonderlijke databases maken in een SQL Database-server in het hetzelfde of een ander datacenter (regio).

> [!NOTE]
> Actieve geo-replicatie wordt niet ondersteund door beheerde exemplaar. Gebruik voor geografische failover van beheerde exemplaren [automatische failovergroepen](sql-database-auto-failover-group.md).

Actieve geo-replicatie is bedoeld als een zakelijke continuïteit-oplossing waarmee de toepassing snel noodherstel van afzonderlijke databases in het geval van een regionale rampenscenario of een grootschalige storing uitvoeren. Als geo-replicatie is ingeschakeld, kan de toepassing failover naar een secundaire database in een andere Azure-regio kunt starten. Maximaal vier secundaire databases worden ondersteund in de dezelfde of verschillende regio's en de secundaire replica's kunnen ook worden gebruikt voor toegang voor alleen-lezen query's. De failover moet handmatig worden gestart door de toepassing of de gebruiker. Na een failover is de nieuwe primaire het eindpunt van een andere verbinding. Het volgende diagram illustreert een typische configuratie van een geografisch redundante cloudtoepassing met behulp van actieve geo-replicatie.

![Actieve geo-replicatie](./media/sql-database-active-geo-replication/geo-replication.png )

> [!IMPORTANT]
> SQL Database biedt ook ondersteuning voor automatische failover-groepen. Zie voor meer informatie, met behulp van [automatische failovergroepen](sql-database-auto-failover-group.md). Actieve geo-replicatie wordt ook niet ondersteund voor databases die zijn gemaakt in een beheerd exemplaar. Overweeg het gebruik van [failovergroepen](sql-database-auto-failover-group.md) met beheerde instanties.

Als voor uw primaire database mislukt reden, of gewoon moet offline worden genomen, kunt u failover starten op een van de secundaire databases. Wanneer failover wordt geactiveerd op een van de secundaire databases, worden alle andere secundaire replica's automatisch gekoppeld aan de nieuwe primaire.

U kunt replicatie en failover van een individuele database of een set van databases op een server of in een elastische pool met behulp van actieve geo-replicatie beheren. U kunt doen dat met:

- [Azure Portal](sql-database-geo-replication-portal.md)
- [PowerShell: Individuele database](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)
- [PowerShell: Elastische pool](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md)
- [Transact-SQL: Individuele database of elastische pool](/sql/t-sql/statements/alter-database-azure-sql-database)
- [REST API: Individuele database](https://docs.microsoft.com/rest/api/sql/replicationlinks)

Controleer of dat de verificatievereisten voor uw server en database zijn geconfigureerd op de nieuwe primaire na een failover. Zie voor meer informatie, [SQL Database-beveiliging na herstel na noodgevallen](sql-database-geo-replication-security-config.md).

Actieve geo-replicatie maakt gebruik van de [Always On](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server) technologie van SQL Server voor het repliceren van asynchroon doorgevoerde transacties op de primaire database naar een secundaire database met behulp van de snapshot-isolatie. Automatische failover-groepen bieden de semantiek voor groep boven op de actieve geo-replicatie, maar het dezelfde mechanisme voor asynchrone replicatie wordt gebruikt. Terwijl u ze op elk gewenst moment de secundaire database mogelijk iets achter de primaire database, de secundaire gegevens kan worden gegarandeerd nooit hebt gedeeltelijke transacties. Interregionale redundantie kan toepassingen snel herstellen van een permanente verlies van een heel datacenter of delen van een datacenter veroorzaakt door natuurrampen, onherstelbare menselijke fouten of schadelijke acties uit. De specifieke RPO-gegevens kunt u vinden op [overzicht van bedrijfscontinuïteit](sql-database-business-continuity.md).

> [!NOTE]
> Als er een netwerkstoring tussen twee regio's, proberen we elke tien seconden opnieuw om verbindingen te maken.
> [!IMPORTANT]
> Om ervoor te zorgen dat een cruciale veranderingen op de primaire database worden gerepliceerd naar een secundaire voordat u een failover, kunt u afdwingen dat synchronisatie om te controleren of de replicatie van belangrijke wijzigingen (bijvoorbeeld bijwerken van wachtwoorden). Geforceerde synchronisatie heeft invloed op prestaties omdat de aanroepende thread geblokkeerd totdat alle doorgevoerde transacties worden gerepliceerd. Zie voor meer informatie, [sp_wait_for_database_copy_sync](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/active-geo-replication-sp-wait-for-database-copy-sync). Zie voor het controleren van de vertraging van replicatie tussen de primaire database en geo-secundaire [sys.dm_geo_replication_link_status](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database).

De volgende afbeelding toont een voorbeeld van actieve geo-replicatie geconfigureerd met een primaire in de regio Noord-centraal VS en secundaire in de regio Zuid-centraal VS.

![geo-replicatie-relatie](./media/sql-database-active-geo-replication/geo-replication-relationship.png)

Omdat de secundaire databases kunnen gelezen worden, kunnen ze worden gebruikt voor de offload van alleen-lezen-werkbelastingen, zoals rapportage van taken. Als u actieve geo-replicatie gebruikt, is het mogelijk te maken van de secundaire database in dezelfde regio met de primaire, maar de herstelmogelijkheden van de toepassing bij onherstelbare fouten niet wordt verhoogd. Als u automatische failover-groepen gebruikt, wordt altijd de secundaire database gemaakt in een andere regio.

Naast noodherstel kan herstel van actieve geo-replicatie worden gebruikt in de volgende scenario's:

- **Migratie van de database**: U kunt actieve geo-replicatie gebruiken voor het migreren van een database van de ene server naar een andere online met minimale downtime.
- **Upgrades van toepassingen**: U kunt een extra secundaire maken als een back up mislukken tijdens upgrades van toepassingen.

Voor het bereiken van echte zakelijke continuïteit, toe te voegen databaseredundantie tussen datacenters is slechts een deel van de oplossing. Herstellen van een toepassing (service) end-to-end nadat een onherstelbare fout is vereist voor herstel van alle onderdelen die deel uitmaken van de service en afhankelijke services. Voorbeelden van deze onderdelen zijn de clientsoftware (bijvoorbeeld een browser met een aangepaste JavaScript), web-front-ends, opslag en DNS. Het is essentieel dat alle onderdelen die tolerant omgaan met de dezelfde fouten en beschikbaar is in de beoogde hersteltijd (RTO) van uw toepassing. Daarom moet u voor het identificeren van alle afhankelijke services en informatie over de garanties en mogelijkheden bieden. Vervolgens moet u de juiste stappen om ervoor te zorgen dat uw servicefuncties tijdens de failover van de services waarvan deze afhankelijk is, nemen. Zie voor meer informatie over het ontwerpen van oplossingen voor herstel na noodgevallen, [oplossingen voor Cloudopslag ontwerpen voor Disaster Recovery met behulp van actieve geo-replicatie](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

## <a name="active-geo-replication-terminology-and-capabilities"></a>Actieve geo-replicatie-terminologie en mogelijkheden

- **Automatische asynchrone replicatie**

  U kunt alleen een secundaire database maken door toe te voegen aan een bestaande database. De secundaire server kan worden gemaakt in een Azure SQL Database-server. Nadat u hebt gemaakt, wordt de secundaire database gevuld met de gegevens die zijn gekopieerd uit de primaire database. Dit proces staat bekend als seeding. Nadat de secundaire database is gemaakt en seeding, worden updates naar de primaire database asynchroon gerepliceerd naar de secundaire database automatisch. Asynchrone replicatie betekent dat transacties worden vastgelegd op de primaire database voordat ze worden gerepliceerd naar de secundaire database.

- **Leesbare secundaire databases**

  Een toepassing hebben toegang tot een secundaire database voor alleen-lezen bewerkingen met behulp van de dezelfde of verschillende beveiligings-principals die is gebruikt voor toegang tot de primaire database. De secundaire databases worden uitgevoerd in de modus voor snapshot-isolatie om te controleren of replicatie van de updates van de primaire (log opnieuw afspelen) niet is vertraagd door query's uitgevoerd op de secundaire.

> [!NOTE]
> Het logboek opnieuw afspelen is vertraagd op de secundaire database als er schema-updates op de primaire. De laatste moet een schemavergrendeling op de secundaire database.
> [!IMPORTANT]
> U kunt geo-replicatie gebruiken om te maken van een secundaire database in dezelfde regio als de primaire. U kunt deze secundaire server voor de taakverdeling op een alleen-lezen van workloads in dezelfde regio. Echter een secundaire database in dezelfde regio biedt geen aanvullende fouttolerantie en daarom is geen geschikte failoverdoel voor herstel na noodgevallen. Het biedt ook geen garantie voor avaialability zone isolatie. Gebruik bedrijfskritiek of Premium-servicelaag met [zone-redundante configuratie](sql-database-high-availability.md#zone-redundant-configuration) groeperen avaialability zone isolatie.   
>

- **Geplande failover**

  Geplande failover uitvoert volledige synchronisatie tussen primaire en secundaire databases voordat u de secundaire switches naar de primaire rol. Dit garandeert een zonder verlies van gegevens. Geplande failover wordt gebruikt de volgende scenario's: (a) om uit te voeren van DR-oefeningen in productie wanneer het verlies van gegevens niet toegestaan is. (b) om aan de database te verplaatsen naar een andere regio; en (c) om te retourneren van de database naar de primaire regio, nadat de onderbreking is verholpen (failback).

- **Niet-geplande failover**

  Niet-geplande of geforceerde failover wordt onmiddellijk de secundaire naar de primaire rol zonder een synchronisatie met de primaire verandert. Met deze bewerking leidt tot verlies van gegevens. Niet-geplande failover wordt gebruikt als een methode voor het herstellen tijdens storingen wanneer de primaire niet toegankelijk is. Als de oorspronkelijke primaire database weer online is, wordt automatisch opnieuw verbinding te maken zonder synchronisatie en een nieuwe secundaire geworden.

- **Meerdere leesbare secundaire databases**

  Voor elke primaire kan maximaal 4 secundaire databases worden gemaakt. Als er slechts één secundaire database, en dit mislukt, wordt de toepassing wordt blootgesteld aan hoger risico totdat er een nieuwe secundaire database is gemaakt. Als er meerdere secundaire databases bestaan, is de toepassing blijft beveiligd, zelfs als een van de secundaire databases is mislukt. De extra secundaire replica's kunnen ook worden gebruikt voor het opschalen van de alleen-lezen-workloads

  > [!NOTE]
  > Als u actieve geo-replicatie gebruikt voor het bouwen van een wereldwijd gedistribueerde toepassing en moet alleen-lezen toegang tot gegevens in meer dan vier regio's opgeven, kunt u secundaire van een secundaire (een proces-koppeling genoemd) maken. Op deze manier kunt u vrijwel onbeperkte schaal van de databasereplicatie bereiken. Bovendien vermindert chaining de overhead van replicatie vanaf de primaire database. De verhouding is de vertraging verbeterde replicatie voor de meest rechtse leaf secundaire databases.

- **Geo-replicatie van databases in een elastische pool**

  Elke secundaire database kan afzonderlijk deel uitmaken van een elastische pool of worden niet in een elastische pool helemaal. De keuze van toepassingen voor elke secundaire database is gescheiden en niet afhankelijk is van de configuratie van een andere secundaire database (of primaire of secundaire). Elke elastische groep is opgenomen in één regio, daarom meerdere secundaire databases in dezelfde topologie nooit een elastische pool kunnen delen.

- **Configureerbare compute-grootte van de secundaire database**

  Primaire en secundaire databases moeten dezelfde servicelaag. Het is ook raadzaam dat deze secundaire database wordt gemaakt met dezelfde compute grootte (dtu's of vCores) als de primaire. Een secundaire met lagere compute grootte loopt het risico van een verbeterde replicatievertraging, mogelijk niet beschikbaar zijn van de secundaire server, en als gevolg daarvan risico aanzienlijk verlies van gegevens na een failover. Als gevolg hiervan, de gepubliceerde RPO = 5 per seconde kan niet worden gegarandeerd. Het andere risico is dat na een failover van de toepassing is van invloed op vanwege een gebrek aan rekencapaciteit van de nieuwe primaire totdat deze is bijgewerkt naar een hogere compute-grootte. De tijd van de upgrade is afhankelijk van de grootte van de database. Daarnaast heeft nodig momenteel deze upgrade zowel primaire als secundaire databases online zijn en daarom kunnen niet worden voltooid nadat de onderbreking is verholpen. Als u besluit te maken van de secundaire met lagere compute-grootte, biedt het logboek i/o-percentage diagram in Azure portal een goede manier om te schatten van de minimale compute-grootte van de secundaire server die is vereist voor het handhaven van de belasting van de replicatie. Bijvoorbeeld, als uw primaire database is P6 (1000 dtu's) en het logboek-i/o-percentage is 50% is de secundaire server moet ten minste P4 (500 dtu's). U kunt ook de logboekgegevens van de i/o-gebruik ophalen [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) of [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) weergaven van de database.  Zie voor meer informatie over de compute-grootten voor SQL-Database [wat SQL Database-Servicelagen zijn](sql-database-purchase-models.md).

- **Gebruiker beheerde failover en failback**

  Een secundaire database kan expliciet worden overgeschakeld naar de primaire rol op elk gewenst moment door de toepassing of de gebruiker. Tijdens een storing in de echte de "niet-geplande" optie moet worden gebruikt, die direct bevordert een secundaire moet de primaire. Wanneer de mislukte primaire herstelt en weer beschikbaar is, wordt het systeem automatisch de herstelde primaire als een secundaire markeert en deze up-to-date zijn met de nieuwe primaire. Vanwege de asynchrone aard van de replicatie, kan een kleine hoeveelheid gegevens worden verbroken tijdens niet-geplande failovers als een primaire mislukt voordat deze de meest recente wijzigingen wordt gerepliceerd naar de secundaire server. Wanneer failover optreedt van een primaire met meerdere secundaire replica's, wordt het systeem automatisch de Replicatierelaties geconfigureerd en de resterende secundaire databases is gekoppeld aan de nieuwe opgewaardeerde primaire zonder tussenkomst van de gebruiker. Nadat de onderbreking waardoor de failover is verholpen, kan het wenselijk om terug te keren van de toepassing naar de primaire regio zijn. Om dit te doen, moet de failoveropdracht met de optie 'gepland' worden aangeroepen.

- **Referenties en firewallregels synchroon houden**

Wordt u aangeraden [IP-firewallregels op databaseniveau database](sql-database-firewall-configure.md) voor databases, zodat deze regels kunnen worden gerepliceerd met de database om te controleren of alle secundaire databases hebben de dezelfde IP-firewall-regels als de primaire geo-replicatie. Deze aanpak elimineert de noodzaak voor klanten om handmatig te configureren en firewallregels op servers die als host fungeert voor zowel de primaire en secundaire databases te onderhouden. Op dezelfde manier met behulp van [ingesloten databasegebruikers](sql-database-manage-logins.md) voor gegevens toegang zorgt ervoor dat de primaire en secundaire databases hebben altijd hetzelfde gebruikersreferenties zodat tijdens een failover er geen onderbrekingen vanwege problemen met aanmeldingen en wachtwoorden is. Met de toevoeging van [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md), klanten kunnen gebruikerstoegang tot zowel primaire als secundaire databases beheren en hoeft u voor het beheren van referenties in databases die kan worden overgeslagen.

## <a name="upgrading-or-downgrading-a-primary-database"></a>Het upgraden of downgraden van een primaire database

U kunt upgraden en downgraden van een primaire database op een andere compute-grootte (in dezelfde servicelaag, niet tussen algemeen gebruik en bedrijfskritiek) zonder te verbreken alle secundaire databases. Bij een upgrade uitvoert, wordt het aanbevolen dat u eerst de secundaire database upgraden en werk vervolgens de primaire. Wanneer de Downgrade uitvoert, de volgorde omgekeerd: eerst downgraden van de primaire en vervolgens gebruik maken van de secundaire server. Wanneer u upgraden en downgraden van de database naar een andere service-laag, worden deze aanbeveling wordt afgedwongen.

> [!NOTE]
> Het verdient aanbeveling niet downgraden van de secundaire database als u een secundaire database als onderdeel van de configuratie van de failover gemaakt. Dit is om te controleren of dat uw gegevenslaag heeft onvoldoende capaciteit voor het verwerken van uw reguliere werkbelasting nadat failover is geactiveerd.

> [!IMPORTANT]
> De primaire database in een failovergroep kan niet schalen naar een hogere laag, tenzij de secundaire database eerst bij de hogere laag wordt geschaald. Als u probeert te schalen van de primaire database voordat de secundaire database wordt geschaald, ontvangt u mogelijk de volgende fout:
>
> `Error message: The source database 'Primaryserver.DBName' cannot have higher edition than the target database 'Secondaryserver.DBName'. Upgrade the edition on the target before upgrading the source.`
>

## <a name="preventing-the-loss-of-critical-data"></a>Zo wordt voorkomen dat het verlies van kritieke gegevens

Doorlopend kopiëren gebruikt vanwege de hoge latentie voor wide area network, een asynchroon replicatiemechanisme. Asynchrone replicatie maakt enig gegevensverlies onvermijdelijk als er een fout optreedt. Sommige toepassingen vereisen echter zonder verlies van gegevens. Ter bescherming van deze essentiële updates, een ontwikkelaar kan aanroepen de [sp_wait_for_database_copy_sync](/sql/relational-databases/system-stored-procedures/active-geo-replication-sp-wait-for-database-copy-sync) system procedure onmiddellijk na het doorvoeren van de transactie. Aanroepen van **sp_wait_for_database_copy_sync** de aanroepende thread geblokkeerd totdat de laatst vastgelegde transactie is verzonden naar de secundaire database. Echter, het wacht niet totdat de verzonden transacties worden opnieuw afgespeeld en doorgevoerd op de secundaire server. **sp_wait_for_database_copy_sync** is afgestemd op de koppeling van een specifieke doorlopend kopiëren. Elke gebruiker met de rechten van de verbinding met de primaire database, kunt deze procedure aanroept.

> [!NOTE]
> **sp_wait_for_database_copy_sync** wordt voorkomen dat gegevens verloren gaan na een failover, maar is geen garantie voor volledige synchronisatie voor leestoegang. De vertraging is veroorzaakt door een **sp_wait_for_database_copy_sync** procedureaanroep kan aanzienlijk zijn en is afhankelijk van de grootte van het transactielogboek op het moment van de aanroep.

## <a name="monitoring-geo-replication-lag"></a>Vertraging van geo-replicatie controleren

U kunt controleren lag met betrekking tot RPO met *replication_lag_sec* kolom van [sys.dm_geo_replication_link_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database) op de primaire database. Hier ziet u vertraging in seconden tussen de transacties doorgevoerd op de primaire en opgeslagen op de secundaire server. Bijvoorbeeld Als de waarde van de vertraging is 1 seconde, betekent dit als de primaire wordt beïnvloed door een storing op dit moment en failover is intiated, 1 seconde van de meest recente transtions worden niet opgeslagen. 

Als u wilt meten lag met betrekking tot wijzigingen op de primaire database die zijn toegepast op de secundaire, dat wil zeggen beschikbaar voor het lezen van de secundaire vergelijken *last_commit* tijd op de secundaire database met dezelfde waarde op de primaire de database.

> [!NOTE]
> Soms *replication_lag_sec* op de primaire database heeft een NULL-waarde, wat betekent dat de primaire op dit moment weet niet hoe ver de secundaire server is.   Dit gebeurt gewoonlijk nadat het proces opnieuw is opgestart en moet ook een tijdelijke toestand. Houd rekening met waarschuwingen van de toepassing als de *replication_lag_sec* wordt er NULL geretourneerd voor een lange periode. Dit zou betekenen dat de secundaire database kan niet met de primaire vanwege een probleem met de permanente communiceren. Er zijn ook situaties die ervoor zorgen het verschil tussen dat kunnen *last_commit* tijd op de secundaire server en op de primaire database te groot. Bijvoorbeeld Als een wijziging wordt doorgevoerd op de primaire na een lange periode geen wijzigingen, wordt het verschil voordat u snel terugkeert naar de 0 gaan tot een grote waarde. Houd rekening met het een fout bij het verschil tussen deze twee waarden grote gedurende een lange periode blijft.


## <a name="programmatically-managing-active-geo-replication"></a>Actieve geo-replicatie programmatisch te beheren

Zoals eerder besproken, kan actieve geo-replicatie ook worden beheerd via een programma met behulp van Azure PowerShell en de REST-API. De volgende tabellen beschrijven de reeks opdrachten die beschikbaar zijn. Actieve geo-replicatie bevat een set met Azure Resource Manager-API's voor beheer, met inbegrip van de [REST-API van Azure SQL Database](https://docs.microsoft.com/rest/api/sql/) en [Azure PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/overview). Deze API's vereisen het gebruik van resourcegroepen en ondersteuning voor beveiliging op basis van rollen (RBAC). Zie voor meer informatie over het implementeren van toegang tot rollen [toegangsbeheer](../role-based-access-control/overview.md).

### <a name="t-sql-manage-failover-of-single-and-pooled-databases"></a>T-SQL: Failover van één en gepoolde databases beheren

> [!IMPORTANT]
> Deze Transact-SQL-opdrachten zijn alleen van toepassing op actieve geo-replicatie en niet van toepassing op failover-groepen. Als zodanig ze ook niet van toepassing op beheerde instanties, omdat ze alleen ondersteuning voor failover-groepen.

| Opdracht | Description |
| --- | --- |
| [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current) |SECUNDAIRE ON SERVER toevoegen argument gebruiken om te maken van een secundaire database voor een bestaande database en replicatie van gegevens wordt gestart |
| [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current) |FAILOVER of FORCE_FAILOVER_ALLOW_DATA_LOSS gebruiken om over te schakelen van een secundaire database als primaire failover initiëren |
| [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current) |Gebruik secundaire ON-SERVER verwijderen een replicatie van gegevens tussen een SQL-Database en de opgegeven secundaire database is beëindigd. |
| [sys.geo_replication_links](/sql/relational-databases/system-dynamic-management-views/sys-geo-replication-links-azure-sql-database) |Retourneert informatie over alle bestaande koppelingen voor databasereplicatie voor elke database op de Azure SQL Database-server. |
| [sys.dm_geo_replication_link_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database) |Hiermee haalt de laatste keer dat replicatie, laatste vertraging van replicatie en andere informatie over de replicatiekoppeling voor een bepaalde SQL-database. |
| [sys.dm_operation_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) |Geeft de status voor alle databasebewerkingen, waaronder de status van de koppelingen voor databasereplicatie. |
| [sp_wait_for_database_copy_sync](/sql/relational-databases/system-stored-procedures/active-geo-replication-sp-wait-for-database-copy-sync) |zorgt ervoor dat de toepassing moet worden gewacht tot alle doorgevoerde transacties worden gerepliceerd en bevestigd door de actieve secundaire database. |
|  | |

### <a name="powershell-manage-failover-of-single-and-pooled-databases"></a>PowerShell: Failover van één en gepoolde databases beheren

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> De PowerShell Azure Resource Manager-module nog steeds wordt ondersteund door Azure SQL Database, maar alle toekomstige ontwikkeling is voor de module Az.Sql. Zie voor deze cmdlets [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). De argumenten voor de opdrachten in de Az-module en de AzureRm-modules zijn vrijwel identiek zijn.

| Cmdlet | Description |
| --- | --- |
| [Get-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabase) |Hiermee haalt u een of meer databases op. |
| [New-AzSqlDatabaseSecondary](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabasesecondary) |Hiermee maakt u een secundaire database voor een bestaande database en wordt gegevensreplicatie gestart. |
| [Set-AzSqlDatabaseSecondary](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabasesecondary) |Hiermee wordt overgeschakeld op een secundaire database als primaire om de failover te initiëren. |
| [Remove-AzSqlDatabaseSecondary](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqldatabasesecondary) |Hiermee wordt gegevensreplicatie tussen een SQL Database en de opgegeven secundaire database beëindigd. |
| [Get-AzSqlDatabaseReplicationLink](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasereplicationlink) |Hiermee haalt u de geo-replicatiekoppelingen tussen een Azure SQL Database en een resourcegroep of SQL Server op. |
|  | |

> [!IMPORTANT]
> Zie voor voorbeelden van scripts, [en failover een individuele database met behulp van actieve geo-replicatie configureren](scripts/sql-database-setup-geodr-and-failover-database-powershell.md) en [en failover een gegroepeerde-database met behulp van actieve geo-replicatie configureren](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md).

### <a name="rest-api-manage-failover-of-single-and-pooled-databases"></a>REST-API: Failover van één en gepoolde databases beheren

| API | Description |
| --- | --- |
| [Maken of bijwerken-Database (createMode = herstellen)](https://docs.microsoft.com/rest/api/sql/databases/createorupdate) |Gemaakt, bijgewerkt of hersteld van een primaire of secundaire database. |
| [Get maken of bijwerken van de Status van Database](https://docs.microsoft.com/rest/api/sql/databases/createorupdate) |Retourneert de status tijdens een bewerking voor maken. |
| [Stel de secundaire Database als primaire (geplande Failover)](https://docs.microsoft.com/rest/api/sql/replicationlinks/failover) |Sets welke secundaire database primaire door failover wordt uitgevoerd vanuit de huidige primaire database is. **Deze optie wordt niet ondersteund voor Managed Instance.**|
| [Stel de secundaire Database als primaire (niet-geplande Failover)](https://docs.microsoft.com/rest/api/sql/replicationlinks/failoverallowdataloss) |Sets welke secundaire database primaire door failover wordt uitgevoerd vanuit de huidige primaire database is. Met deze bewerking kan leiden tot verlies van gegevens. **Deze optie wordt niet ondersteund voor Managed Instance.**|
| [Ophalen van replicatiekoppeling](https://docs.microsoft.com/rest/api/sql/replicationlinks/get) |Hiermee haalt een bepaalde replicatiekoppeling voor een bepaalde SQL-database in een partnerschap geo-replicatie. Het ophalen van de informatie in de catalogusweergave sys.geo_replication_links zichtbaar. **Deze optie wordt niet ondersteund voor Managed Instance.**|
| [Koppelingen voor databasereplicatie - lijst per Database](https://docs.microsoft.com/rest/api/sql/replicationlinks/listbydatabase) | Hiermee haalt alle koppelingen voor databasereplicatie voor een bepaalde SQL-database in een partnerschap geo-replicatie. Het ophalen van de informatie in de catalogusweergave sys.geo_replication_links zichtbaar. |
| [Verwijderen van replicatiekoppeling](https://docs.microsoft.com/rest/api/sql/replicationlinks/delete) | Hiermee verwijdert u een databasereplicatiekoppeling. Kan niet worden uitgevoerd tijdens de failover. |
|  | |

## <a name="next-steps"></a>Volgende stappen

- Zie voor voorbeelden van scripts:
  - [PowerShell gebruiken voor het configureren van actieve geo-replicatie voor één Azure SQL-database](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)
  - [PowerShell gebruiken voor het configureren van actieve geo-replicatie voor een gepoolde Azure SQL-database](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md)
- SQL Database biedt ook ondersteuning voor automatische failover-groepen. Zie voor meer informatie, met behulp van [automatische failovergroepen](sql-database-auto-failover-group.md).
- Zie voor een overzicht voor zakelijke continuïteit en scenario's, [overzicht voor zakelijke continuïteit](sql-database-business-continuity.md)
- Voor meer informatie over Azure SQL Database geautomatiseerde back-ups, Zie [geautomatiseerde back-ups van SQL-Database](sql-database-automated-backups.md).
- Zie voor meer informatie over het gebruik van geautomatiseerde back-ups voor herstel, [een database herstellen vanuit back-ups service geïnitieerde](sql-database-recovery-using-backups.md).
- Zie voor meer informatie over vereisten voor een nieuwe primaire server en database authentication, [SQL Database-beveiliging na herstel na noodgevallen](sql-database-geo-replication-security-config.md).
