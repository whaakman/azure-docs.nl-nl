---
title: Failover groepen en actieve geo-replicatie - Azure SQL Database | Microsoft Docs
description: Gebruik automatische failover groepen met actieve geo-replicatie en failover autoomatic inschakelen in het geval van een storing.
services: sql-database
documentationcenter: na
author: anosov1960
manager: jhubbard
editor: monicar
ms.assetid: 2a29f657-82fb-4283-9a83-e14a144bfd93
ms.service: sql-database
ms.custom: business continuity
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Active
ms.date: 10/11/2017
ms.author: sashan
ms.openlocfilehash: ef9463e464928b8fa8e64019037a41711cb77830
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="overview-failover-groups-and-active-geo-replication"></a>Overzicht: Failover-groepen en actieve geo-replicatie
Actieve geo-replicatie kunt u maximaal vier leesbare secundaire databases in de locaties van dezelfde of verschillende data center (regio's) configureren. Secundaire databases zijn beschikbaar voor het uitvoeren van query's en voor failover als er een datacentrum of het onvermogen om verbinding met de primaire database. De failover moet handmatig worden gestart door de toepassing van de gebruiker. Na een failover heeft de nieuwe primaire het eindpunt van een andere verbinding. 

> [!NOTE]
> Actieve geo-replicatie is beschikbaar voor alle databases in alle Servicelagen in alle regio's.
>  

Azure SQL Database automatische failover groepen (in preview) is een SQL-Database-onderdeel ontworpen automatisch relatie geo-replicatie, connectiviteit en failover op schaal worden beheerd. Krijgen de klanten de mogelijkheid om automatisch te herstellen meerdere verwante databases in de secundaire regio na onherstelbare regionale fouten of andere niet-geplande gebeurtenissen die leiden tot volledige of gedeeltelijke verlies van beschikbaarheid van de SQL-Database-service in de primaire regio. Bovendien kan de leesbare secundaire databases worden gebruikt voor het offloaden van alleen-lezen-werkbelastingen.  Omdat automatische failover groepen om meerdere databases, moeten ze worden geconfigureerd op de primaire server. Primaire en secundaire servers moeten zich in hetzelfde abonnement. Groepen met automatische failover ondersteuning voor replicatie van alle databases in de groep met slechts één secundaire server in een andere regio. Actieve geo-replicatie, zonder automatische failover-groepen, kan maximaal vier secundaire databases in elke regio.

Als u met behulp van actieve geo-replicatie en voor de primaire database mislukt reden of moet offline worden gehaald, kunt u failover naar een van de secundaire databases starten. Wanneer failover is geactiveerd op een van de secundaire databases, worden alle andere secundaire replica's automatisch gekoppeld aan de nieuwe primaire. Als u automatische failover-groepen (in preview) gebruikt voor het herstel van database en een storing die van invloed is op een of meer van de databases in de resultaten van Groepsbeleid beheren in automatische failover. U kunt de automatische failover-beleid die het beste voldoet aan de behoeften van uw toepassing configureren of kunt u opt-out en handmatige activering gebruiken. Bovendien automatisch een failover-groepen (in preview) Geef alleen-lezen en alleen-lezen-listener-eindpunten die blijven ongewijzigd tijdens failovers. Of u handmatig of automatisch failover-activering gebruikt, verandert failover alle secundaire databases in de groep naar primaire. Nadat de failover van de database is voltooid, wordt de DNS-record wordt automatisch bijgewerkt voor de omleiding van de eindpunten voor de nieuwe regio.

U kunt replicatie en failover van een individuele database of een set van databases op een server of in een elastische pool van actieve geo-replicatie beheren. U kunt doen dat met 

- De [Azure-portal](sql-database-geo-replication-portal.md)
- [PowerShell: Individuele database](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)
- [PowerShell: De elastische groep](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md)
- [PowerShell: Failover-groep](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md)
- [Transact-SQL: Één database of elastische groep](/sql/t-sql/statements/alter-database-azure-sql-database)
- [REST-API: Één database](/rest/api/sql/replicationlinks/failover)
- [REST-API: Failover groep](/rest/api/sql/failovergroups/failover). 
 
Controleer dat de verificatievereisten voor uw server en database zijn geconfigureerd op de nieuwe primaire na een failover. Zie voor meer informatie [beveiliging van SQL Database na het herstel na noodgevallen](sql-database-geo-replication-security-config.md). Dit geldt zowel voor actieve geo-replicatie en automatische failover groepen (in preview).

Actieve geo-replicatie maakt gebruik van de [altijd op](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server) technologie van SQL Server asynchroon gerepliceerd doorgevoerde transacties op de primaire database naar een secundaire database met lezen committed-momentopname-isolatie (RCSI). Automatische failover groepen bieden de semantiek voor groep boven op actieve geo-replicatie, maar het mechanisme voor dezelfde asynchrone replicatie wordt gebruikt. Wanneer ze op elk gewenst moment de secundaire database mogelijk enigszins achter de primaire database, de secundaire gegevens is om nooit gedeeltelijke transacties gegarandeerd. Regio-overschrijdende redundantie kan toepassingen snel herstellen vanuit een permanente verlies van een hele datacenter of delen van een datacenter veroorzaakt door een natuurramp, onherstelbare menselijke fouten of schadelijke acties uit. De RPO gegevens kan worden gevonden op [overzicht van zakelijke continuïteit](sql-database-business-continuity.md).

De volgende afbeelding toont een voorbeeld van actieve geo-replicatie geconfigureerd met een primaire in de regio Noord-centraal VS en secundaire in Zuid-centraal VS regio.

![geo-replicatie-relatie](./media/sql-database-active-geo-replication/geo-replication-relationship.png)

Omdat de secundaire databases kunnen gelezen worden, kunnen ze worden gebruikt voor het offloaden van alleen-lezen werkbelastingen, zoals rapportage van taken. Als u actieve geo-replicatie gebruikt, is het mogelijk maken van de secundaire database in dezelfde regio met de primaire, maar niet wordt verhoogd tot de toepassing herstelmogelijkheden bij kritieke fouten optreden. Als u automatische failover-groepen (in preview) gebruikt, wordt de secundaire database altijd gemaakt in een andere regio.

Naast noodherstel kan herstel van actieve geo-replicatie worden gebruikt in de volgende scenario's:

* **Migratie van de database**: U kunt actieve geo-replicatie gebruiken voor het migreren van een database van de ene server naar een andere online met minimale downtime.
* **Toepassingsupgrades**: U kunt een extra secundaire als een mislukken back-up maken tijdens toepassingsupgrades.

Databaseredundantie tussen datacentra toe te voegen zodat de echte zakelijke continuïteit is slechts een deel van de oplossing. Herstellen van een toepassing (service) end-to-end nadat een onherstelbare fout is vereist voor herstel van alle onderdelen die deel uitmaken van de service en afhankelijke services. Voorbeelden van deze onderdelen zijn de clientsoftware (bijvoorbeeld een browser met een aangepaste JavaScript), web-front-ends, opslag- en DNS. Het is essentieel dat alle onderdelen flexibel omgaan met de dezelfde storingen zijn en beschikbaar binnen de beoogde hersteltijd (RTO) van uw toepassing. Daarom moet u alle afhankelijke services identificeren en u begrijpt de garanties en mogelijkheden bieden. Vervolgens moet u de juiste stappen om ervoor te zorgen dat uw servicefuncties tijdens de failover van de services waarvan deze afhankelijk is, nemen. Zie voor meer informatie over het ontwerpen van oplossingen voor herstel na noodgevallen [Cloudoplossingen voor Disaster Recovery met actieve geo-replicatie ontwerpen](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

## <a name="active-geo-replication-capabilities"></a>Mogelijkheden van actieve geo-replicatie
De functie van actieve geo-replicatie biedt de volgende essentiële mogelijkheden:
* **Automatische asynchrone replicatie**: U kunt alleen een secundaire database maken door toe te voegen aan een bestaande database. De secundaire kan worden gemaakt in een Azure SQL Database-server. Zodra gemaakt, wordt de secundaire database ingevuld met de gegevens die zijn gekopieerd uit de primaire database. Dit proces staat bekend als seeding. Nadat de secundaire database is gemaakt en seeding, worden updates naar de primaire database asynchroon gerepliceerd naar de secundaire database automatisch. Asynchrone replicatie betekent dat transacties worden vastgelegd op de primaire database voordat ze worden gerepliceerd naar de secundaire database. 
* **Leesbare secundaire databases**: een toepassing heeft toegang tot een secundaire database voor alleen-lezen bewerkingen met dezelfde of verschillende beveiligingsprincipals gebruikt voor toegang tot de primaire database. De secundaire databases werken in momentopname isolatiemodus om te controleren of de replicatie van de updates van de primaire (logboek replay) niet is vertraagd door query's die worden uitgevoerd op de secundaire server.

   > [!NOTE]
   > Het logboek opnieuw afspelen is vertraagd op de secundaire database als de primaire schema-updates bevat. De laatste vereist een schemavergrendeling voor de secundaire database. 
   > 

* **Meerdere secundaire databases**: twee of meer secundaire databases verhogen redundantie en het niveau van beveiliging voor de primaire database en de toepassing. Als er meerdere secundaire databases bestaan, is de toepassing blijft beveiligd zelfs als een van de secundaire databases mislukt. Als er slechts één secundaire database, en dit mislukt, wordt de toepassing wordt blootgesteld aan een hoger risico totdat er een nieuwe secundaire database is gemaakt.

   > [!NOTE]
   > Als u het bouwen van een globaal gedistribueerde toepassing en hoeft op te geven alleen-lezen toegang tot gegevens in meer dan vier segions actieve geo-replicatie gebruikt, kunt u secundaire van een secundaire (dit proces wordt '-koppeling genoemd). Op deze manier kunt u vrijwel onbeperkt schalen van databasereplicatie bereiken. Bovendien vermindert chaining de overhead van replicatie vanaf de primaire database. De verhouding is de vertraging meer replicatie van de leaf meest secundaire databases. 
   >

* **Ondersteuning van de elastische groep databases**: actieve geo-replicatie kan worden geconfigureerd voor een database in een elastische pool. De secundaire database kan zich in een andere elastische pool. Voor gewone databases kan de secundaire een elastische pool en vice versa zijn, zolang de servicecategorieën hetzelfde zijn. 
* **Configureerbare prestatieniveau van de secundaire database**: primaire en secundaire databases zijn vereist om de dezelfde servicelaag (Basic, Standard, Premium). Een secundaire database kan worden gemaakt met een lager prestatieniveau (dtu's) dan de primaire. Deze optie wordt niet aanbevolen voor toepassingen met hoge database schrijfactiviteiten omdat de vertraging verbeterde replication het risico van gegevensverlies aanzienlijke na een failover verhoogt. Bovendien na een failover van de toepassing prestaties beïnvloed totdat de nieuwe primaire is bijgewerkt naar een hoger prestatieniveau. De grafiek logboek IO percentage in Azure portal biedt een goede manier om in te schatten het minimale prestatieniveau van de secundaire die vereist is voor het handhaven van de belasting van de replicatie. Bijvoorbeeld, als uw primaire database P6 (1000 DTU) en het logboek-i/o-procent is 50% de secundaire moet ten minste P4 (500 DTU). U kunt ook de logboek-i/o-gegevens met behulp ophalen [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) of [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) weergaven van de database.  Zie voor meer informatie over de prestaties van de SQL-Database [SQL Database-opties en prestaties](sql-database-service-tiers.md). 
* **Gebruiker beheerde failover en failback**: een secundaire database kunt expliciet worden overgeschakeld naar de primaire rol op elk gewenst moment door de toepassing of de gebruiker. Tijdens een werkelijke storing moet de optie 'niet-geplande' worden gebruikt, die direct bijdraagt aan een secundaire moet de primaire. Als de mislukte primaire herstelt weer beschikbaar is, wordt het systeem automatisch de herstelde primaire als een secundaire markeert en brengt u het up-to-date blijven met de nieuwe primaire. Als gevolg van de asynchrone aard van replicatie, kan een kleine hoeveelheid gegevens worden verbroken tijdens niet-geplande failovers als een primaire mislukt voordat de meest recente wijzigingen naar de secundaire worden gerepliceerd. Als een primaire met meerdere secundaire replica's failover wordt uitgevoerd, wordt het systeem automatisch de Replicatierelaties opnieuw geconfigureerd en de resterende secundaire replica's is gekoppeld aan de onlangs bevorderd primaire zonder tussenkomst van de gebruiker. Nadat de onderbreking, waardoor de failover wordt verholpen, kan het wenselijk om te retourneren van de toepassing op de primaire regio zijn. Hiertoe moet de failoveropdracht met de optie 'geplande' worden aangeroepen. 
* **Referenties en firewallregels synchroon te houden**: wordt u aangeraden [database firewallregels](sql-database-firewall-configure.md) voor databases geogerepliceerde zodat deze regels kunnen worden gerepliceerd met de database, zodat alle secundaire databases hebben de dezelfde firewallregels als de primaire. Deze aanpak hoeven voor klanten om handmatig te configureren en onderhouden firewallregels op servers die als host fungeert voor zowel de primaire en secundaire databases. Op deze manier met behulp van [databasegebruikers opgenomen](sql-database-manage-logins.md) voor gegevens toegang zorgt ervoor dat zowel primaire als secundaire databases hebben altijd dezelfde gebruikersreferenties zodat tijdens een failover geen onderbrekingen vanwege verschillen met aanmeldingen en wachtwoorden is. Met de toevoeging van [Azure Active Directory](../active-directory/active-directory-whatis.md), klanten gebruikerstoegang tot zowel primaire als secundaire databases kunnen beheren en hoeft u voor het beheren van referenties in databases kan worden overgeslagen.

## <a name="auto-failover-group-capabilities"></a>Mogelijkheden voor automatische failover-groep

Automatische failover groepsfunctie biedt een krachtige abstractie van actieve geo-replicatie door de groep niveau replicatie en automatische failover-ondersteuning. Daarnaast verwijdert deze de noodzaak tot het wijzigen van de SQL-verbindingsreeks na een failover aan de hand van de aanvullende listener-eindpunten. 

* **Failover-groep**: een of meer groepen van failover kunnen worden gemaakt tussen twee servers in verschillende regio's (primaire en secundaire servers). Elke groep kan een of meer databases die worden hersteld als een eenheid voor het geval alle of sommige primaire databases niet meer beschikbaar als gevolg van een storing in de primaire regio bevatten.  
* **Primaire server**: een server die als host fungeert voor de primaire databases in de groep failover.
* **Secundaire server**: een server die als host fungeert voor de secundaire databases in de groep failover. De secundaire server kan niet in dezelfde regio bevinden als de primaire server.
* **Databases toevoegen aan groep failover**: U kunt verschillende databases in een server of in een elastische pool plaatsen in dezelfde groep failover. Als u een zelfstandige database aan de groep toevoegt, wordt automatisch een secundaire database met dezelfde versie en prestatieniveau gemaakt. Als de primaire database in een elastische groep is, wordt de secundaire automatisch gemaakt in de elastische groep met dezelfde naam. Als u een database die al een secundaire database in de secundaire server toevoegt, wordt die geo-replicatie wordt overgenomen door de groep.

   > [!NOTE]
   > Wanneer u een database die al een secundaire database in een server die geen deel uitmaakt van de failover-groep toevoegt, wordt een nieuwe secundaire gemaakt in de secundaire server. 
   >

* **Failover-groep lezen-schrijven listener**: een DNS CNAME-record wordt gevormd als  **&lt;failover groepsnaam&gt;. database.windows.net** die verwijst naar de huidige primaire server-URL. Kunt u de lezen-schrijven SQL-toepassingen op transparante wijze opnieuw verbinding maken met de primaire database wanneer de primaire gewijzigd na een failover. 
* **Failover-groep alleen-lezen-listener**: een DNS CNAME-record wordt gevormd als  **&lt;failover groepsnaam&gt;. secondary.database.windows.net** die verwijst naar de URL van de secundaire server. Kunt u de toepassingen alleen-lezen SQL transparant verbinding maken met de secundaire database van de opgegeven regels voor taakverdeling. U kunt eventueel opgeven als u wilt dat het alleen-lezen verkeer automatisch wordt omgeleid naar de primaire server als de secundaire server niet beschikbaar is.
* **Beleid voor automatische failover**: de failover-groep is standaard geconfigureerd met een beleid voor automatische failover. Het systeem activeert failover zodra de storing wordt gedetecteerd. Als u bepalen van de werkstroom failover van de toepassing wilt, kunt u automatische failover uitschakelen. 
* **Handmatige failover**: U kunt failover handmatig op elk gewenst moment ongeacht de configuratie van de automatische failover. Als automatische failover-beleid niet is geconfigureerd, is handmatige failover vereist in de groep failover-databases te herstellen. U kunt de geforceerde of beschrijvende failover (met volledige gegevenssynchronisatie) starten. De laatste kan worden gebruikt om aan de actieve server verplaatsen naar de primaire regio. Als de failover is voltooid, wordt de DNS-records worden automatisch bijgewerkt om te controleren of de verbinding met de juiste server.
* **Respijtperiode met gegevensverlies**: omdat de primaire en secundaire databases worden gesynchroniseerd met behulp van asynchrone replicatie, de failover kan leiden tot verlies van gegevens. U kunt het beleid automatische failover naar aanleiding van uw toepassing tolerantie tot gegevensverlies. Door het configureren van **GracePeriodWithDataLossHours**, kunt u bepalen hoe lang het systeem wacht voordat u de failover is waarschijnlijk tot gegevensverlies resultaat start. 

   > [!NOTE]
   > Wanneer systeem detecteert dat de databases in de groep nog steeds online zijn (bijvoorbeeld de onderbreking alleen van invloed op het vlak van service control), wordt onmiddellijk de failover met volledige gegevenssynchronisatie (beschrijvende failover) geactiveerd ongeacht de waarde die is ingesteld door  **GracePeriodWithDataLossHours**. Dit gedrag zorgt ervoor dat er geen gegevens verloren gaan tijdens het herstel. De respijtperiode heeft alleen effect als een beschrijvende failover niet mogelijk is. Als de onderbreking is verholpen voordat de respijtperiode is verlopen, worden de failover is niet geactiveerd.
   >

* **Meerdere failover groepen**: U kunt meerdere failover groepen voor dezelfde twee servers om te bepalen van de schaal van failovers configureren. Elke groep onafhankelijk failover wordt uitgevoerd. Als uw toepassing met meerdere tenants elastische pools gebruikt, kunt u deze mogelijkheid te mengen primaire en secundaire databases in elke groep. Op deze manier die u de gevolgen van een storing tot alleen de helft van de tenants beperken kunt.

## <a name="best-practices-of-building-highly-available-service"></a>Aanbevolen procedures van het bouwen van maximaal beschikbare service

Als u wilt maken van een maximaal beschikbare service die gebruikmaakt van Azure SQL-database, moet u deze richtlijnen volgen:

- **Failover-groep gebruiken**: een of meer groepen van failover kunnen worden gemaakt tussen twee servers in verschillende regio's (primaire en secundaire servers). Elke groep kan een of meer databases die worden hersteld als een eenheid voor het geval alle of sommige primaire databases niet meer beschikbaar als gevolg van een storing in de primaire regio bevatten. De failover-groep maakt geo-secundaire database met de doelstelling van dezelfde als de primaire. Als u een bestaande relatie in de geo-replicatie aan de groep failover toevoegt, zorg er dan voor dat de secundaire geo-database is geconfigureerd met de dezelfde serviceniveaudoelstelling als de primaire.
- **Gebruik alleen-lezen-listener voor OLTP-werkbelasting**: gebruiken bij het uitvoeren van bewerkingen OLTP  **&lt;failover groepsnaam&gt;. database.windows.net** als de server-URL en de verbindingen zijn automatisch omgeleid naar de primaire. Deze URL wordt niet gewijzigd na de failover.  
Noteer dat de failover omvat het bijwerken van die de DNS-record, zodat de clientverbindingen worden omgeleid naar de nieuwe primaire pas nadat de client DNS-cache vernieuwd wordt.
- **Gebruik van alleen-lezen-listener voor de werkbelasting van alleen-lezen**: als u een logisch is geïsoleerd alleen-lezen-werkbelasting die is gevoelig voor bepaalde veroudering van gegevens hebt, kunt u de secundaire database in de toepassing. Gebruik voor alleen-lezen-sessies,  **&lt;failover groepsnaam&gt;. secondary.database.windows.net** als de server de URL en de verbinding automatisch omgeleid naar de secundaire. Het is ook raadzaam die u hebt opgegeven in de verbindingsreeks bedoeling lezen met behulp van **ApplicationIntent ReadOnly =**. 
- **Worden voorbereid voor de prestaties nadelig beïnvloeden**: SQL failover besluit is onafhankelijk van de rest van de toepassing of andere services die worden gebruikt. De toepassing worden "gecombineerd" met een aantal onderdelen in één regio en sommige in een andere. Zorg ervoor dat de implementatie van de redundante toepassing in de regio DR om te voorkomen de verslechtering van, en volg de richtlijnen in dit artikel.  
Opmerking van de toepassing in de regio DR heeft geen een andere verbindingsreeks gebruiken.  
- **Voorbereiden voor gegevensverlies**: als er een storing wordt gedetecteerd, SQL lezen-schrijven failover automatisch geactiveerd als er geen gegevens verloren gaan naar de beste van onze kennis. Anders dat wordt gewacht totdat de periode die u hebt opgegeven door **GracePeriodWithDataLossHours**. Als u hebt opgegeven **GracePeriodWithDataLossHours**, worden voorbereid op verlies van gegevens. In het algemeen tijdens uitval, Azure meer gericht op beschikbaarheid. Als u geen enkele gegevens verloren gaan, controleert u of in te stellen **GracePeriodWithDataLossHours** met een voldoende aantal, zoals 24 uur. 

> [!IMPORTANT]
> Elastische pools met dtu's van 800 of minder en meer dan 250 databases van geo-replicatie ondervinden problemen, inclusief meer geplande failovers en verminderde prestatie.  Deze problemen zijn geneigd om te worden uitgevoerd voor schrijven intensieve werkbelastingen, wanneer geo-replicatie eindpunten zijn geografisch ver uit elkaar per Geografie of wanneer er meerdere secundaire eindpunten worden gebruikt voor elke database.  Symptomen van deze problemen worden aangegeven wanneer de vertraging van geo-replicatie na verloop van tijd toeneemt.  Deze vertraging kan worden gecontroleerd via [sys.dm_geo_replication_link_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database).  Als deze problemen optreden, zijn oplossingen waardoor het aantal dtu's van toepassingen, of het aantal databases in dezelfde groep geogerepliceerde te verminderen.

## <a name="upgrading-or-downgrading-a-primary-database"></a>Een upgrade of een primaire database downgraden
U kunt een upgrade of een primaire database naar een andere prestatieniveau (binnen dezelfde servicelaag) downgraden zonder te verbreken geen secundaire databases. Wanneer u een upgrade uitvoert, raden we aan dat u eerst de secundaire database upgraden en werk vervolgens de primaire. Wanneer downgraden, de volgorde: eerst downgrade voor de primaire en vervolgens gebruik maken van de secundaire. Wanneer u bijwerkt of downgrade voor de database naar een andere servicelaag, wordt deze aanbeveling wordt afgedwongen. 

> [!NOTE]
> Als u een secundaire database als onderdeel van de configuratie van de failover gemaakt is het niet raadzaam downgraden van de secundaire database. Dit is om te controleren of dat uw gegevenslaag heeft onvoldoende capaciteit om te verwerken van uw reguliere werkbelasting nadat de failover is geactiveerd. 
>

## <a name="preventing-the-loss-of-critical-data"></a>Zo wordt voorkomen dat het verlies van kritieke gegevens
Doorlopend kopiëren maakt gebruik van een asynchrone replicatiemechanisme vanwege de hoge latentie van wide area network. Asynchrone replicatie maakt gegevensverlies onvermijdelijke als er een fout optreedt. Sommige toepassingen vereisen echter geen verlies van gegevens. Ter bescherming van deze essentiële updates, een toepassingsontwikkelaar kunt aanroepen de [sp_wait_for_database_copy_sync](/sql/relational-databases/system-stored-procedures/active-geo-replication-sp-wait-for-database-copy-sync) system procedure onmiddellijk na het vastleggen van de transactie. Het aanroepen van **sp_wait_for_database_copy_sync** blokkeert de aanroepende thread totdat de laatst doorgevoerde transactie is verzonden naar de secundaire database. Echter, deze wacht niet totdat de verzonden transacties worden cookies en doorgevoerd op de secundaire server. **sp_wait_for_database_copy_sync** is afgestemd op de koppeling van een specifieke doorlopend kopiëren. Elke gebruiker met de rechten van de verbinding naar de primaire database kunt deze procedure aanroept.

> [!NOTE]
> **sp_wait_for_database_copy_sync** wordt voorkomen dat gegevens verloren gaan na een failover, maar garandeert geen volledige synchronisatie voor leestoegang. De vertraging veroorzaakt door een **sp_wait_for_database_copy_sync** procedureaanroep kan aanzienlijke en is afhankelijk van de grootte van het transactielogboek op het moment van de aanroep. 
> 

## <a name="programmatically-managing-failover-groups-and-active-geo-replication"></a>Failover-groepen en actieve geo-replicatie programmatisch te beheren
Zoals eerder besproken automatische failover-groepen (in preview) en active kan geo-replicatie ook worden beheerd programmatisch met behulp van Azure PowerShell en de REST-API. De volgende tabellen beschrijven de reeks opdrachten die beschikbaar zijn.

**Azure Resource Manager-API en op rollen gebaseerde beveiliging**: actieve geo-replicatie bevat een set met Azure Resource Manager-API's voor beheer, met inbegrip van de [REST-API van Azure SQL Database](https://docs.microsoft.com/rest/api/sql/) en [Azure PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/overview). Deze API's vereisen het gebruik van resourcegroepen en ondersteuning voor beveiliging op basis van rollen (RBAC). Zie voor meer informatie over het implementeren van toegang tot rollen [rollen gebaseerd toegangsbeheer](../active-directory/role-based-access-control-what-is.md).

## <a name="manage-sql-database-failover-using-transact-sql"></a>Failover van SQL database Transact-SQL met beheren

| Opdracht | Beschrijving |
| --- | --- |
| [ALTER DATABASE (Azure SQL Database)](/sql/t-sql/statements/alter-database-azure-sql-database) |SECUNDAIRE ON-SERVER toevoegen argument gebruiken voor het maken van een secundaire database voor een bestaande database en de gegevensreplicatie wordt gestart |
| [ALTER DATABASE (Azure SQL Database)](/sql/t-sql/statements/alter-database-azure-sql-database) |FAILOVER of FORCE_FAILOVER_ALLOW_DATA_LOSS gebruiken om over te schakelen van een secundaire database als primaire failover initiëren |
| [ALTER DATABASE (Azure SQL Database)](/sql/t-sql/statements/alter-database-azure-sql-database) |Gebruik secundaire ON-SERVER verwijderen om te beëindigen van een gegevensreplicatie tussen een SQL-Database en de opgegeven secundaire database. |
| [sys.geo_replication_links (Azure SQL Database)](/sql/relational-databases/system-dynamic-management-views/sys-geo-replication-links-azure-sql-database) |Retourneert informatie over alle bestaande replicatiekoppelingen voor elke database op de logische Azure SQL Database-server. |
| [sys.dm_geo_replication_link_status (Azure SQL Database)](/sql/relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database) |Hiermee wordt de tijd van laatste replicatie, laatste vertraging van replicatie en andere informatie over de replicatiekoppeling voor een bepaalde SQL-database. |
| [sys.dm_operation_status bevat (Azure SQL Database)](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) |Geeft de status voor alle databasebewerkingen, inclusief de status van de replicatiekoppelingen. |
| [sp_wait_for_database_copy_sync (Azure SQL Database)](/sql/relational-databases/system-stored-procedures/active-geo-replication-sp-wait-for-database-copy-sync) |zorgt ervoor dat de toepassing moet worden gewacht totdat alle doorgevoerde transacties zijn gerepliceerd en bevestigd door de actieve secundaire database. |
|  | |

## <a name="manage-sql-database-failover-using-powershell"></a>Failover van SQL database met behulp van PowerShell beheren

| Cmdlet | Beschrijving |
| --- | --- |
| [Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase) |Hiermee haalt u een of meer databases. |
| [Nieuwe AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/new-azurermsqldatabasesecondary) |Hiermee maakt u een secundaire database voor een bestaande database en start gegevensreplicatie. |
| [Set-AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/set-azurermsqldatabasesecondary) |Een secundaire database als primaire initiëren failover verandert. |
| [Verwijder AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/remove-azurermsqldatabasesecondary) |Beëindigt gegevensreplicatie tussen een SQL-Database en de opgegeven secundaire database. |
| [Get-AzureRmSqlDatabaseReplicationLink](/powershell/module/azurerm.sql/get-azurermsqldatabasereplicationlink) |Hiermee haalt u de geo-replicatiekoppelingen tussen een Azure SQL Database en een resourcegroep of SQL Server. |
| [Nieuwe AzureRmSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/set-azurermsqldatabasefailovergroup) |   Deze opdracht maakt een failover-groep en registreert deze op de primaire en secundaire servers|
| [Verwijder AzureRmSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/remove-azurermsqldatabasefailovergroup) | Hiermee verwijdert u de groep failover van de server en verwijdert u alle secundaire databases opgenomen in de groep |
| [Get-AzureRmSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/get-azurermsqldatabasefailovergroup) | Haalt de configuratie van de failover |
| [Set-AzureRmSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/set-azurermsqldatabasefailovergroup) |   Hiermee wijzigt u de configuratie van de failover-groep |
| [Switch-AzureRMSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/switch-azurermsqldatabasefailovergroup) | Triggers failover van de groep failover naar de secundaire server |
|  | |

> [!IMPORTANT]
> Zie voor voorbeelden van scripts, [en failover een individuele database met behulp van actieve geo-replicatie configureren](scripts/sql-database-setup-geodr-and-failover-database-powershell.md), [en failover een gegroepeerde database met behulp van actieve geo-replicatie configureren](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md), en [ Configureren en failover een failover-groep voor één database (preview)](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md).
>

## <a name="manage-sql-database-failover-using-the-rest-api"></a>Failover van SQL database met de REST API beheren
| API | Beschrijving |
| --- | --- |
| [Maken of de Database-Update (createMode = herstellen)](/rest/api/sql/Databases/CreateOrUpdate) |Gemaakt, bijgewerkt of hersteld van een primaire of secundaire database. |
| [Get maken of bijwerken van de databasestatus](/rest/api/sql/Databases/CreateOrUpdate) |Retourneert de status tijdens een bewerking voor maken. |
| [Secundaire Database instellen als primaire (geplande Failover)](/rest/api/sql/replicationlinks/failover) |Hiermee wordt ingesteld welke replica-database primaire door storing worden overgenomen uit de huidige primaire replica-database is. |
| [Secundaire Database instellen als primaire (niet-geplande Failover)](/rest/api/sql/replicationlinks/failoverallowdataloss) |Hiermee wordt ingesteld welke replica-database primaire door storing worden overgenomen uit de huidige primaire replica-database is. Deze bewerking kan leiden tot verlies van gegevens. |
| [Replicatiekoppeling ophalen](/rest/api/sql/replicationlinks/get) |Hiermee haalt een bepaalde replicatiekoppeling voor een gegeven SQL-database in een associatie geo-replicatie. Het ophalen van de informatie in de weergave van de catalogus sys.geo_replication_links zichtbaar. |
| [Koppelingen voor databasereplicatie - lijst per Database](/rest/api/sql/replicationlinks/listbydatabase) | Hiermee haalt alle koppelingen voor databasereplicatie voor een gegeven SQL-database in een associatie geo-replicatie. Het ophalen van de informatie in de weergave van de catalogus sys.geo_replication_links zichtbaar. |
| [Replicatiekoppeling verwijderen](/rest/api/sql/databases/delete) | Hiermee verwijdert u een databasereplicatiekoppeling. Tijdens de failover kan niet worden uitgevoerd. |
| [Maken of bijwerken van failover-groep](/rest/api/sql/failovergroups/createorupdate) | Maken of bijwerken van een failover-groep |
| [Failover-groep verwijderen](/rest/api/sql/failovergroups/delete) | Hiermee verwijdert u de groep failover van de server |
| [Failover (gepland)](/rest/api/sql/failovergroups/failover) | Failover uit van de huidige primaire server naar deze server. |
| [Force Failover verlies van gegevens toestaan](/rest/api/sql/failovergroups/forcefailoverallowdataloss) |ails via van de huidige primaire server naar deze server. Deze bewerking kan leiden tot verlies van gegevens. |
| [Get-Failover-groep](/rest/api/sql/failovergroups/get) | Hiermee haalt u een failover-groep. |
| [Lijst met failover-groepen door Server](/rest/api/sql/failovergroups/listbyserver) | Hier worden de groepen failover in een server. |
| [Failover-Updategroep](/rest/api/sql/failovergroups/update) | Een groep failover-updates. |
|  | |

## <a name="next-steps"></a>Volgende stappen
* Zie voor voorbeeldscripts:
   - [Configureren en failover een individuele database met behulp van actieve geo-replicatie](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)
   - [Configureren en failover een gegroepeerde database met behulp van actieve geo-replicatie](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md)
   - [Configureren en failover een failover-groep voor één database (preview)](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md)
* Zie voor een overzicht van zakelijke continuïteit en scenario's [Business continuity-overzicht](sql-database-business-continuity.md)
* Voor meer informatie over Azure SQL Database geautomatiseerde back-ups, Zie [geautomatiseerde back-ups van SQL-Database](sql-database-automated-backups.md).
* Zie voor meer informatie over het gebruik van automatische back-ups voor herstel, [een database herstellen vanuit back-ups service geïnitieerde](sql-database-recovery-using-backups.md).
* Zie voor meer informatie over vereisten voor gebruikersverificatie voor een nieuwe primaire server en database, [beveiliging van SQL Database na het herstel na noodgevallen](sql-database-geo-replication-security-config.md).

