---
title: Failover-groepen en actieve geo-replicatie - Azure SQL Database | Microsoft Docs
description: Gebruik van automatische failovergroepen met actieve geo-replicatie en failover autoomatic na een storing.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: carlrab
manager: craigg
ms.date: 09/14/2018
ms.openlocfilehash: e3296430cd58c8ba33f7cc9acd4cded2925b5be9
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47166279"
---
# <a name="overview-active-geo-replication-and-auto-failover-groups"></a>Overzicht: Actieve geo-replicatie en automatische failover-groepen

Actieve geo-replicatie is Azure SQL Database-functie waarmee u leesbare replica's van uw database maken in het hetzelfde of een ander datacenter (regio).

![Geo-replicatie](./media/sql-database-geo-replication-failover-portal/geo-replication.png )

Actieve geo-replicatie is bedoeld als een zakelijke continuïteit-oplossing waarmee de toepassing snel noodherstel in het geval van de schaal van een datacenterstoring uitvoeren. Als geo-replicatie is ingeschakeld, kan de toepassing failover naar een secundaire database in een andere Azure-regio kunt starten. Maximaal vier secundaire databases worden ondersteund in de dezelfde of verschillende regio's en de secundaire replica's kunnen ook worden gebruikt voor toegang voor alleen-lezen query's. De failover moet handmatig worden gestart door de toepassing of de gebruiker. Na een failover is de nieuwe primaire het eindpunt van een andere verbinding. 

> [!NOTE]
> Actieve geo-replicatie is beschikbaar voor alle databases in alle service-lagen in alle regio's.
> Actieve Geo-replicatie is niet beschikbaar in het beheerde exemplaar.
>  

Automatische failover-groepen is een uitbreiding van actieve geo-replicatie. Het is ontworpen voor het beheren van failovers van meerdere geo-replicatie databases tegelijkertijd met behulp van een failover van de toepassing gestart of door over te dragen van failover worden uitgevoerd door de SQL-Database-service op basis van een gebruiker gedefinieerde criteria. De laatste kunt u meerdere verwante databases in een secundaire regio wordt automatisch hersteld na een onherstelbare fout of andere niet-geplande gebeurtenis optreedt die in het volledige of gedeeltelijke verlies van beschikbaarheid van de service SQL Database in de primaire regio resulteert. Bovendien kunt u de leesbare secundaire databases voor de offload van alleen-lezen querywerkbelastingen. Omdat automatische failover-groepen hebben betrekking op meerdere databases, moeten deze databases worden geconfigureerd op de primaire server. Primaire en secundaire servers voor de databases in de failovergroep moeten zich in hetzelfde abonnement. Automatische failovergroepen ondersteuning bieden voor replicatie van alle databases in de groep met slechts één secundaire server in een andere regio.

> [!NOTE]
> Gebruik actieve geo-replicatie als meerdere secundaire replica's vereist zijn.
>  

Als u met behulp van actieve geo-replicatie voor reden dan ook uw primaire database mislukt of gewoon moet offline worden genomen, kunt u failover starten op een van de secundaire databases. Wanneer failover wordt geactiveerd op een van de secundaire databases, worden alle andere secundaire replica's automatisch gekoppeld aan de nieuwe primaire. Als u automatische failover-groepen voor het beheren van de database herstellen en een storing voordoet die van invloed is op een of meer van de databases in de Groepsresultaten van de in automatische failover. U kunt de automatische failover-beleid die het beste aan uw toepassingsbehoeften configureren of kunt u zich afmelden en handmatig activeren. Bovendien, automatische failover-groepen bieden de lezen / schrijven en alleen-lezen-listener-eindpunten die blijven ongewijzigd tijdens failovers. Of u nu handmatige of automatische failover-activering, verandert failover alle secundaire databases in de groep in de primaire. Nadat de databasefailover is voltooid, wordt de DNS-record wordt automatisch bijgewerkt om te leiden van de eindpunten naar de nieuwe regio.

U kunt replicatie en failover van een individuele database of een set van databases op een server of in een elastische pool met behulp van actieve geo-replicatie beheren. U kunt doen dat met 

- [Azure Portal](sql-database-geo-replication-portal.md)
- [PowerShell: Één database](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)
- [PowerShell: Elastische pool](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md)
- [PowerShell: Failover-groep](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md)
- [Transact-SQL: De enkele database of elastische pool](/sql/t-sql/statements/alter-database-azure-sql-database)
- [REST-API: Individuele database](/rest/api/sql/replicationlinks/failover)
- [REST-API: Failover-groep](/rest/api/sql/failovergroups/failover). 
 
Controleer of dat de verificatievereisten voor uw server en database zijn geconfigureerd op de nieuwe primaire na een failover. Zie voor meer informatie, [SQL Database-beveiliging na herstel na noodgevallen](sql-database-geo-replication-security-config.md). Dit geldt zowel voor actieve geo-replicatie en automatische failover-groepen.

Actieve geo-replicatie maakt gebruik van de [Always On](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server) technologie van SQL Server voor het repliceren van asynchroon doorgevoerde transacties op de primaire database naar een secundaire database met behulp van de snapshot-isolatie. Automatische failover-groepen bieden de semantiek voor groep boven op de actieve geo-replicatie, maar het dezelfde mechanisme voor asynchrone replicatie wordt gebruikt. Terwijl u ze op elk gewenst moment de secundaire database mogelijk iets achter de primaire database, de secundaire gegevens kan worden gegarandeerd nooit hebt gedeeltelijke transacties. Interregionale redundantie kan toepassingen snel herstellen van een permanente verlies van een heel datacenter of delen van een datacenter veroorzaakt door natuurrampen, onherstelbare menselijke fouten of schadelijke acties uit. De specifieke RPO-gegevens kunt u vinden op [overzicht van bedrijfscontinuïteit](sql-database-business-continuity.md).

De volgende afbeelding toont een voorbeeld van actieve geo-replicatie geconfigureerd met een primaire in de regio Noord-centraal VS en secundaire in de regio Zuid-centraal VS.

![geo-replicatie-relatie](./media/sql-database-active-geo-replication/geo-replication-relationship.png)

Omdat de secundaire databases kunnen gelezen worden, kunnen ze worden gebruikt voor de offload van alleen-lezen-werkbelastingen, zoals rapportage van taken. Als u actieve geo-replicatie gebruikt, is het mogelijk te maken van de secundaire database in dezelfde regio met de primaire, maar de herstelmogelijkheden van de toepassing bij onherstelbare fouten niet wordt verhoogd. Als u automatische failover-groepen gebruikt, wordt altijd de secundaire database gemaakt in een andere regio.

Naast noodherstel kan herstel van actieve geo-replicatie worden gebruikt in de volgende scenario's:

* **Migratie van de database**: kunt u actieve geo-replicatie voor het migreren van een database van de ene server naar een andere online met minimale downtime.
* **Upgrades van toepassingen**: U kunt een extra secundaire maken als een back up mislukken tijdens upgrades van toepassingen.

Voor het bereiken van echte zakelijke continuïteit, toe te voegen databaseredundantie tussen datacenters is slechts een deel van de oplossing. Herstellen van een toepassing (service) end-to-end nadat een onherstelbare fout is vereist voor herstel van alle onderdelen die deel uitmaken van de service en afhankelijke services. Voorbeelden van deze onderdelen zijn de clientsoftware (bijvoorbeeld een browser met een aangepaste JavaScript), web-front-ends, opslag en DNS. Het is essentieel dat alle onderdelen die tolerant omgaan met de dezelfde fouten en beschikbaar is in de beoogde hersteltijd (RTO) van uw toepassing. Daarom moet u voor het identificeren van alle afhankelijke services en informatie over de garanties en mogelijkheden bieden. Vervolgens moet u de juiste stappen om ervoor te zorgen dat uw servicefuncties tijdens de failover van de services waarvan deze afhankelijk is, nemen. Zie voor meer informatie over het ontwerpen van oplossingen voor herstel na noodgevallen, [oplossingen voor Cloudopslag ontwerpen voor Disaster Recovery met behulp van actieve geo-replicatie](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

## <a name="active-geo-replication-capabilities"></a>Actieve geo-replicatie-mogelijkheden
De functie van actieve geo-replicatie biedt de volgende essentiële mogelijkheden:
* **Automatische asynchrone replicatie**: U kunt alleen een secundaire database maken door toe te voegen aan een bestaande database. De secundaire server kan worden gemaakt in een Azure SQL Database-server. Nadat u hebt gemaakt, wordt de secundaire database gevuld met de gegevens die zijn gekopieerd uit de primaire database. Dit proces staat bekend als seeding. Nadat de secundaire database is gemaakt en seeding, worden updates naar de primaire database asynchroon gerepliceerd naar de secundaire database automatisch. Asynchrone replicatie betekent dat transacties worden vastgelegd op de primaire database voordat ze worden gerepliceerd naar de secundaire database. 
* **Leesbare secundaire databases**: een toepassing toegang heeft tot een secundaire database voor alleen-lezen bewerkingen met behulp van de dezelfde of verschillende beveiligings-principals die is gebruikt voor toegang tot de primaire database. De secundaire databases worden uitgevoerd in de modus voor snapshot-isolatie om te controleren of replicatie van de updates van de primaire (log opnieuw afspelen) niet is vertraagd door query's uitgevoerd op de secundaire.

> [!NOTE]
> Het logboek opnieuw afspelen is vertraagd op de secundaire database als er schema-updates op de primaire. De laatste moet een schemavergrendeling op de secundaire database. 
> 

* **Meerdere leesbare secundaire databases**: twee of meer secundaire databases verhogen redundantie en niveau van beveiliging voor de primaire database en de toepassing. Als er meerdere secundaire databases bestaan, is de toepassing blijft beveiligd, zelfs als een van de secundaire databases is mislukt. Als er slechts één secundaire database, en dit mislukt, wordt de toepassing wordt blootgesteld aan hoger risico totdat er een nieuwe secundaire database is gemaakt.

> [!NOTE]
> Als u actieve geo-replicatie gebruikt voor het bouwen van een wereldwijd gedistribueerde toepassing en moet alleen-lezen toegang tot gegevens in meer dan vier regio's opgeven, kunt u secundaire van een secundaire (een proces-koppeling genoemd) maken. Op deze manier kunt u vrijwel onbeperkte schaal van de databasereplicatie bereiken. Bovendien vermindert chaining de overhead van replicatie vanaf de primaire database. De verhouding is de vertraging verbeterde replicatie voor de meest rechtse leaf secundaire databases. 
>

* **Ondersteuning van de elastische groep databases**: elke replica kan afzonderlijk deel uitmaken van een elastische Pool of worden niet in een elastische pool helemaal. De keuze van toepassingen voor elke replica die is gescheiden en niet afhankelijk is van de configuratie van een andere replica (of primaire of secundaire). Elke elastische groep is opgenomen in één regio, daarom meerdere replica's in dezelfde topologie nooit een elastische Pool kunnen delen.
* **Configureerbaar compute grootte van de secundaire database**: primaire en secundaire databases zijn vereist om dezelfde servicelaag. Het is ook raadzaam dat deze secundaire database wordt gemaakt met dezelfde compute grootte (dtu's of vCores) als de primaire. Een secundaire met lagere compute grootte loopt het risico van een verbeterde replicatievertraging, mogelijk niet beschikbaar zijn van de secundaire server, en als gevolg daarvan risico aanzienlijk verlies van gegevens na een failover. Als gevolg hiervan, de gepubliceerde RPO = 5 per seconde kan niet worden gegarandeerd. Het andere risico is dat na een failover van de toepassing is van invloed op vanwege onvoldoende computercapaciteit van de nieuwe primaire totdat deze is bijgewerkt naar een hogere compute-grootte. De tijd van de upgrade is afhankelijk van de grootte van de database. Daarnaast heeft nodig momenteel deze upgrade zowel primaire als secundaire databases online zijn en daarom kunnen niet worden voltooid nadat de onderbreking is verholpen. Als u besluit te maken van de secundaire met lagere compute-grootte, biedt het logboek i/o-percentage diagram in Azure portal een goede manier om te schatten van de minimale compute-grootte van de secundaire server die is vereist voor het handhaven van de belasting van de replicatie. Bijvoorbeeld, als uw primaire database is P6 (1000 dtu's) en het logboek-i/o-percentage is 50% is de secundaire server moet ten minste P4 (500 dtu's). U kunt ook de logboekgegevens van de i/o-gebruik ophalen [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) of [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) weergaven van de database.  Zie voor meer informatie over de compute-grootten voor SQL-Database [wat SQL Database-Servicelagen zijn](sql-database-service-tiers.md). 
* **Gebruiker beheerde failover en failback**: een secundaire database kan expliciet worden overgeschakeld naar de primaire rol op elk gewenst moment door de toepassing of de gebruiker. Tijdens een storing in de echte de "niet-geplande" optie moet worden gebruikt, die direct bevordert een secundaire moet de primaire. Wanneer de mislukte primaire herstelt en weer beschikbaar is, wordt het systeem automatisch de herstelde primaire als een secundaire markeert en deze up-to-date zijn met de nieuwe primaire. Vanwege de asynchrone aard van de replicatie, kan een kleine hoeveelheid gegevens worden verbroken tijdens niet-geplande failovers als een primaire mislukt voordat deze de meest recente wijzigingen wordt gerepliceerd naar de secundaire server. Wanneer failover optreedt van een primaire met meerdere secundaire replica's, wordt het systeem automatisch de Replicatierelaties geconfigureerd en de resterende secundaire databases is gekoppeld aan de nieuwe opgewaardeerde primaire zonder tussenkomst van de gebruiker. Nadat de onderbreking waardoor de failover is verholpen, kan het wenselijk om terug te keren van de toepassing naar de primaire regio zijn. Om dit te doen, moet de failoveropdracht met de optie 'gepland' worden aangeroepen. 
* **Synchroon houden van referenties en firewallregels**: wordt u aangeraden [database-firewallregels](sql-database-firewall-configure.md) voor databases met geo-replicatie, zodat deze regels kunnen worden gerepliceerd met de database om te controleren of alle secundaire databases hebben de dezelfde firewallregels als de primaire. Deze aanpak elimineert de noodzaak voor klanten om handmatig te configureren en firewallregels op servers die als host fungeert voor zowel de primaire en secundaire databases te onderhouden. Op dezelfde manier met behulp van [ingesloten databasegebruikers](sql-database-manage-logins.md) voor gegevens toegang zorgt ervoor dat de primaire en secundaire databases hebben altijd hetzelfde gebruikersreferenties zodat tijdens een failover er geen onderbrekingen vanwege problemen met aanmeldingen en wachtwoorden is. Met de toevoeging van [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md), klanten kunnen gebruikerstoegang tot zowel primaire als secundaire databases beheren en hoeft u voor het beheren van referenties in databases die kan worden overgeslagen.

## <a name="auto-failover-group-capabilities"></a>Mogelijkheden voor automatische failover-groep

Automatische failover-groepen functie biedt een krachtige abstractie van actieve geo-replicatie door de ondersteuning van de groep op replicatie en automatische failover. Bovendien wordt de noodzaak tot het wijzigen van de SQL-verbindingsreeks na een failover door te geven van de aanvullende listener-eindpunten verwijderd. 

* **Failovergroep**: een of meer failover-groepen kunnen worden gemaakt tussen twee servers in verschillende regio's (primaire en secundaire servers). Elke groep kan een of meer databases die worden hersteld als een eenheid in het geval alle of een primaire databases niet beschikbaar vanwege een storing in de primaire regio bevatten.  
* **Primaire server**: een server die als host fungeert voor de primaire databases in de failovergroep.
* **Secundaire server**: een server die als host fungeert voor de secundaire databases in de failovergroep. De secundaire server kan niet in dezelfde regio als de primaire server.
* **Databases toevoegen aan de failovergroep**: U kunt verschillende databases binnen een server of in een elastische pool in dezelfde failover-groep plaatsen. Als u een individuele database aan de groep toevoegt, wordt automatisch een secundaire database met behulp van dezelfde grootte edition en de rekenkracht gemaakt. Als de primaire database zich in een elastische pool, wordt de secundaire server wordt automatisch gemaakt in de elastische pool met dezelfde naam. Als u een database die al een secundaire database in de secundaire server toevoegt, wordt deze geo-replicatie wordt overgenomen door de groep.

> [!NOTE]
> Bij het toevoegen van een database die al een secundaire database in een server die geen deel uitmaakt van de failovergroep, wordt een nieuwe secundaire gemaakt in de secundaire server. 
>

* **Failover-listener voor lezen / schrijven**: een DNS CNAME-record gevormd als  **&lt;failover-groepsnaam&gt;. database.windows.net** die verwijst naar de URL voor de huidige primaire server. Hierdoor kan de SQL-toepassingen voor lezen / schrijven transparant opnieuw verbinding maken met de primaire database als de primaire gewijzigd na een failover. 
* **Failover-groep alleen-lezen-listener**: een DNS CNAME-record gevormd als  **&lt;failover-groepsnaam&gt;. secondary.database.windows.net** die verwijst naar de URL van de secundaire server. Hierdoor kan de alleen-lezen SQL-toepassingen op transparante wijze verbinding maken met de secundaire database met behulp van de opgegeven regels voor taakverdeling. 
* **Automatische failover-beleid**: de failovergroep is standaard geconfigureerd met een automatische failover-beleid. Het systeem wordt de failover geactiveerd nadat de fout wordt gedetecteerd en de respijtperiode is verlopen. Het systeem moet verifiëren dat de onderbreking kan niet worden verholpen door de infrastructuur van de ingebouwde hoge beschikbaarheid van de service SQL Database vanwege de schaal van de impact. Als u wilt voor het beheren van de werkstroom van de toepassing, kunt u automatische failover uitschakelen. 
* **Alleen-lezen failoverbeleid**: standaard, de failover van de alleen-lezen-listener is uitgeschakeld. Het zorgt ervoor dat de prestaties van de primaire wordt niet negatief beïnvloed wanneer de secundaire offline is. Echter, het betekent ook dat de alleen-lezen-sessies wordt pas weer verbinding maken, totdat de secundaire server is hersteld. Als u downtime voor de alleen-lezen-sessies niet kan tolereren en OK tijdelijk de primaire te gebruiken voor zowel alleen-lezen als lezen / schrijven-verkeer ten koste van de afname van de mogelijke prestaties van de primaire, kunt u failover voor de alleen-lezen-listener inschakelen. In dat geval wordt het alleen-lezen-verkeer automatisch omgeleid naar de primaire server als de secundaire server niet beschikbaar is.  
* **Handmatige failover**: U kunt failover handmatig starten op elk gewenst moment, ongeacht de configuratie van de automatische failover. Als automatische failover-beleid niet is geconfigureerd, is handmatige failover vereist om databases in de failovergroep te herstellen. Geforceerde of beschrijvende failover (met volledige gegevenssynchronisatie), kunt u starten. De laatste kan worden gebruikt om aan de actieve server verplaatsen naar de primaire regio. Wanneer failover is voltooid, wordt de DNS-records worden automatisch bijgewerkt om te controleren of de verbinding met de juiste server.
* **Respijtperiode verlies van gegevens**: omdat de primaire en secundaire databases worden gesynchroniseerd met behulp van asynchrone replicatie, failover kan leiden tot verlies van gegevens. U kunt de automatische failover-beleid om de tolerantie van uw toepassing tot verlies van gegevens weer te geven. Door het configureren van **GracePeriodWithDataLossHours**, kunt u bepalen hoe lang het systeem moet wachten voordat u begint de failover die waarschijnlijk zal resultaat gegevens verloren gaan. 

> [!NOTE]
> Wanneer wordt gedetecteerd dat de databases in de groep nog steeds online zijn (bijvoorbeeld de onderbreking alleen negatieve gevolgen hebben voor de controlelaag service), het onmiddellijk de failover met volledige gegevenssynchronisatie (beschrijvende failover), ongeacht de ingestelde waarde bij wordtgeactiveerd **GracePeriodWithDataLossHours**. Dit gedrag zorgt ervoor dat er geen gegevens verloren gaan tijdens het herstel. De respijtperiode van kracht alleen wanneer een beschrijvende failover niet mogelijk is. Als de onderbreking is verholpen voordat de respijtperiode is verlopen, wordt de failover niet geactiveerd.
>

* **Meerdere failovergroepen**: U kunt meerdere failover-groepen voor de dezelfde combinatie van servers voor het beheren van de schaal van failovers configureren. Elke groep failover-schakeling onafhankelijk van elkaar. Als uw toepassing met meerdere tenants maakt gebruik van elastische pools, kunt u deze mogelijkheid om primaire en secundaire databases in elke groep. Op deze manier die u de impact van een storing tot slechts de helft van de tenants beperken kunt.

## <a name="best-practices-of-using-failover-groups-for-business-continuity"></a>Aanbevolen procedures van het gebruik van failover-groepen voor bedrijfscontinuïteit
Bij het ontwerpen van een service met zakelijke continuïteit in gedachten, moet u deze richtlijnen volgen:

- **Een of meerdere failovergroepen gebruiken voor het beheren van failovers van meerdere databases**: een of meer failover-groepen kunnen worden gemaakt tussen twee servers in verschillende regio's (primaire en secundaire servers). Elke groep kan een of meer databases die worden hersteld als een eenheid in het geval alle of een primaire databases niet beschikbaar vanwege een storing in de primaire regio bevatten. De failovergroep wordt geo-secundaire database gemaakt met de dezelfde servicedoelstelling als de primaire. Als u een bestaande geo-replicatie-relatie aan de failovergroep toevoegt, zorg er dan voor dat de geo-secundaire is geconfigureerd met de dezelfde servicelaag en de grootte van de compute als de primaire.
- **Gebruik van lezen / schrijven-listener voor OLTP-werkbelasting**: bij het uitvoeren van OLTP-bewerkingen, gebruikt u  **&lt;failover-groepsnaam&gt;. database.windows.net** als de server-URL en de verbindingen zijn automatisch omgeleid naar de primaire. Deze URL wordt niet gewijzigd na de failover. Houd er rekening mee dat de failover omvat het bijwerken van die de DNS-record, zodat de clientverbindingen worden omgeleid naar de nieuwe primaire pas nadat de client-DNS-cache vernieuwd wordt.
- **Gebruik van alleen-lezen-listener voor de werkbelasting van alleen-lezen**: als u een logisch geïsoleerde alleen-lezen-werkbelasting die is gevoelig voor bepaalde veroudering van de gegevens hebt, kunt u de secundaire database in de toepassing. Gebruik voor alleen-lezen-sessies,  **&lt;failover-groepsnaam&gt;. secondary.database.windows.net** als de server de URL en de verbinding automatisch omgeleid naar de secundaire server. Het is ook raadzaam dat u aan te in de verbindingsreeks kunt u lezen wat lezen geven met behulp van **ApplicationIntent = alleen-lezen**. 
- **Worden voorbereid voor verslechtering van prestaties**: SQL failover beslissing is onafhankelijk van de rest van de toepassing of andere services die worden gebruikt. De toepassing kan worden "gemengde" met bepaalde onderdelen in één regio en sommige in een andere. Om te voorkomen dat de degradatie, zorg ervoor dat de implementatie van de redundante toepassing in de DR-regio en volg de richtlijnen in dit artikel. Houd er rekening mee de toepassing in de DR-regio heeft geen om een andere verbindingsreeks te gebruiken.  
- **Voorbereiden op het verlies van gegevens**: als er een storing wordt gedetecteerd, SQL lezen / schrijven-failover automatisch geactiveerd als er geen gegevens verloren gaan naar het beste van onze kennis. Anders, dat wordt gewacht totdat de periode die u hebt opgegeven door **GracePeriodWithDataLossHours**. Als u hebt opgegeven **GracePeriodWithDataLossHours**, worden voorbereid op verlies van gegevens. Tijdens onderbrekingen, omdat hiermee Azure in het algemeen beschikbaar. Als u geen enkele gegevens verloren gaan, zorg ervoor dat ingesteld **GracePeriodWithDataLossHours** op een groot genoeg getal, zoals 24 uur. 

> [!IMPORTANT]
> Elastische pools met 800 of minder dtu's en meer dan 250 databases met behulp van geo-replicatie mogelijk problemen met inbegrip van meer geplande failovers en verminderde prestaties.  Deze problemen zijn vaker optreden voor intensieve werkbelastingen schrijven wanneer geo-replicatie-eindpunten zijn ver uit elkaar liggen op basis van Geografie, of wanneer er meerdere secundaire eindpunten worden gebruikt voor elke database.  Symptomen van deze problemen worden aangegeven wanneer de vertraging van geo-replicatie na verloop van tijd toeneemt.  Deze vertraging kan worden bewaakt met behulp van [sys.dm_geo_replication_link_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database).  Als deze problemen optreden, zijn oplossingen verhogen van het aantal pool-dtu's of verlaag het aantal databases in dezelfde groep geo-replicatie.

## <a name="failover-groups-and-network-security"></a>Failover-groepen en -netwerkbeveiliging 

Voor sommige toepassingen die de beveiligingsregels vereisen dat de netwerktoegang tot de gegevenslaag is beperkt tot een specifiek onderdeel of -onderdelen, zoals een virtuele machine, web service, enzovoort. Deze vereiste geeft enkele uitdagingen voor zakelijke continuïteit ontwerp en het gebruik van de failover-groepen. U moet rekening houden met de volgende opties bij het implementeren van dergelijke beperkte toegang.

### <a name="using-failover-groups-and-virtual-network-rules"></a>Met behulp van failover-groepen en regels voor virtueel netwerk

Als u [Virtual Network-service-eindpunten en regels](sql-database-vnet-service-endpoint-rule-overview.md) om toegang te beperken tot uw SQL-database, houd er rekening mee dat elke Virtual Network service-eindpunt is van toepassing op slechts één Azure-regio. Het eindpunt is niet ingeschakeld voor andere regio's communicatie accepteert van het subnet. Daarom worden alleen de clienttoepassingen die zijn geïmplementeerd in dezelfde regio kunnen verbinden met de primaire database. Nadat de failover resulteert in de SQL-client-sessies omgeleid naar een server in een andere regio (secundair), mislukt deze sessies als afkomstig is van een client buiten deze regio. Om die reden kan niet het beleid automatische failover worden ingeschakeld als de deelnemende servers zijn opgenomen in de regels van het Virtueelnetwerk. Ter ondersteuning van handmatige failover, de volgende stappen uit:

1.  Inrichten van de redundante exemplaren van de front-end-onderdelen van uw toepassing in de secundaire regio (webservice, virtuele machines enz.)
2.  Configureer de [virtuele netwerkregels](sql-database-vnet-service-endpoint-rule-overview.md) afzonderlijk voor de primaire en secundaire server
3.  Schakel de [front-failover met behulp van een configuratie van Traffic manager](sql-database-designing-cloud-solutions-for-disaster-recovery.md#scenario-1-using-two-azure-regions-for-business-continuity-with-minimal-downtime)
4.  Handmatige failover geïnitieerd wanneer de onderbreking is gedetecteerd

Deze optie is geoptimaliseerd voor de toepassingen waarvoor consistente latentie tussen de front-end en de gegevenslaag en biedt ondersteuning voor herstel wanneer front-end, gegevenslaag of beide worden beïnvloed door de storing. 

> [!NOTE]
> Als u de **alleen-lezen-listener** voor het verdelen van de werkbelasting van een alleen-lezen, zorg ervoor dat deze workload wordt uitgevoerd in een virtuele machine of een andere resource in de secundaire regio, zodat deze verbinding met de secundaire database maken kan.
>

 ### <a name="using-failover-groups-and-sql-database-firewall-rules"></a>Met behulp van failover-groepen en firewallregels voor SQL database

Als uw bedrijfscontinuïteitsplan failover met behulp van groepen met automatische failover vereist, kunt u toegang tot uw SQL-database met behulp van de traditionele firewallregels beperken.  Ter ondersteuning van automatische failover, de volgende stappen uit:

1.  [Een openbaar IP-adres maken](../virtual-network/virtual-network-public-ip-address.md#create-a-public-ip-address) 
2.  [Maken van een openbare load balancer](../load-balancer/quickstart-create-basic-load-balancer-portal.md#create-a-basic-load-balancer) en het openbare IP-adres toewijzen aan deze. 
3.  [Maak een virtueel netwerk en de virtuele machines](../load-balancer/quickstart-create-basic-load-balancer-portal.md#create-back-end-servers) voor uw front-end-onderdelen 
4.  [Netwerkbeveiligingsgroep maken](../virtual-network/security-overview.md) en binnenkomende verbindingen te configureren. 
5. Zorg ervoor dat de uitgaande verbindingen geopend met Azure SQL database zijn met behulp van 'Sql' [servicetag](../virtual-network/security-overview.md#service-tags). 
5.  Maak een [SQL database-firewallregel](sql-database-firewall-configure.md) waarmee inkomend verkeer van het openbare IP-adres dat u in stap 1 hebt gemaakt. 

Zie voor meer informatie over op het configureren van uitgaande toegang en welke IP-adres in de firewallregels [Load balancer uitgaande verbindingen](../load-balancer/load-balancer-outbound-connections.md).

De bovenstaande configuratie zorgt ervoor dat de automatische failover wordt de verbindingen van de front-endcomponenten niet geblokkeerd en wordt ervan uitgegaan dat de toepassing de langere latentie tussen de front-end en de gegevenslaag kan tolereren.

> [!IMPORTANT]
> Om te garanderen van bedrijfscontinuïteit voor regionale storingen moet u ervoor zorgen dat geografische redundantie voor zowel de front-end-onderdelen en de databases. 
>

## <a name="upgrading-or-downgrading-a-primary-database"></a>Het upgraden of downgraden van een primaire database
U kunt upgraden en downgraden van een primaire database op een andere compute-grootte (in dezelfde servicelaag) zonder te verbreken alle secundaire databases. Bij een upgrade uitvoert, wordt het aanbevolen dat u eerst de secundaire database upgraden en werk vervolgens de primaire. Wanneer de Downgrade uitvoert, de volgorde omgekeerd: eerst downgraden van de primaire en vervolgens gebruik maken van de secundaire server. Wanneer u upgraden en downgraden van de database naar een andere service-laag, worden deze aanbeveling wordt afgedwongen. 

> [!NOTE]
> Het verdient aanbeveling niet downgraden van de secundaire database als u een secundaire database als onderdeel van de configuratie van de failover gemaakt. Dit is om te controleren of dat uw gegevenslaag heeft onvoldoende capaciteit voor het verwerken van uw reguliere werkbelasting nadat failover is geactiveerd. 
>

## <a name="preventing-the-loss-of-critical-data"></a>Zo wordt voorkomen dat het verlies van kritieke gegevens
Doorlopend kopiëren gebruikt vanwege de hoge latentie voor wide area network, een asynchroon replicatiemechanisme. Asynchrone replicatie maakt enig gegevensverlies onvermijdelijk als er een fout optreedt. Sommige toepassingen vereisen echter zonder verlies van gegevens. Ter bescherming van deze essentiële updates, een ontwikkelaar kan aanroepen de [sp_wait_for_database_copy_sync](/sql/relational-databases/system-stored-procedures/active-geo-replication-sp-wait-for-database-copy-sync) system procedure onmiddellijk na het doorvoeren van de transactie. Aanroepen van **sp_wait_for_database_copy_sync** de aanroepende thread geblokkeerd totdat de laatst vastgelegde transactie is verzonden naar de secundaire database. Echter, het wacht niet totdat de verzonden transacties worden opnieuw afgespeeld en doorgevoerd op de secundaire server. **sp_wait_for_database_copy_sync** is afgestemd op de koppeling van een specifieke doorlopend kopiëren. Elke gebruiker met de rechten van de verbinding met de primaire database, kunt deze procedure aanroept.

> [!NOTE]
> **sp_wait_for_database_copy_sync** wordt voorkomen dat gegevens verloren gaan na een failover, maar is geen garantie voor volledige synchronisatie voor leestoegang. De vertraging is veroorzaakt door een **sp_wait_for_database_copy_sync** procedureaanroep kan aanzienlijk zijn en is afhankelijk van de grootte van het transactielogboek op het moment van de aanroep. 
> 

## <a name="programmatically-managing-failover-groups-and-active-geo-replication"></a>Programmatisch beheren van failover-groepen en actieve geo-replicatie
Zoals eerder besproken automatische failover-groepen en actieve kan geo-replicatie ook worden beheerd via een programma met behulp van Azure PowerShell en de REST-API. De volgende tabellen beschrijven de reeks opdrachten die beschikbaar zijn.

**Azure Resource Manager-API en op rollen gebaseerde beveiliging**: actieve geo-replicatie bevat een set met Azure Resource Manager-API's voor beheer, met inbegrip van de [REST-API van Azure SQL Database](https://docs.microsoft.com/rest/api/sql/) en [Azure PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/overview). Deze API's vereisen het gebruik van resourcegroepen en ondersteuning voor beveiliging op basis van rollen (RBAC). Zie voor meer informatie over het implementeren van toegang tot rollen [toegangsbeheer](../role-based-access-control/overview.md).

## <a name="manage-sql-database-failover-using-transact-sql"></a>Beheren van een failover van SQL database met behulp van Transact-SQL

| Opdracht | Beschrijving |
| --- | --- |
| [ALTER DATABASE (Azure SQL Database)](/sql/t-sql/statements/alter-database-azure-sql-database) |SECUNDAIRE ON SERVER toevoegen argument gebruiken om te maken van een secundaire database voor een bestaande database en replicatie van gegevens wordt gestart |
| [ALTER DATABASE (Azure SQL Database)](/sql/t-sql/statements/alter-database-azure-sql-database) |FAILOVER of FORCE_FAILOVER_ALLOW_DATA_LOSS gebruiken om over te schakelen van een secundaire database als primaire failover initiëren |
| [ALTER DATABASE (Azure SQL Database)](/sql/t-sql/statements/alter-database-azure-sql-database) |Gebruik secundaire ON-SERVER verwijderen een replicatie van gegevens tussen een SQL-Database en de opgegeven secundaire database is beëindigd. |
| [sys.geo_replication_links (Azure SQL Database)](/sql/relational-databases/system-dynamic-management-views/sys-geo-replication-links-azure-sql-database) |Retourneert informatie over alle bestaande koppelingen voor databasereplicatie voor elke database op de logische Azure SQL Database-server. |
| [sys.dm_geo_replication_link_status (Azure SQL Database)](/sql/relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database) |Hiermee haalt de laatste keer dat replicatie, laatste vertraging van replicatie en andere informatie over de replicatiekoppeling voor een bepaalde SQL-database. |
| [sys.dm_operation_status (Azure SQL Database)](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) |Geeft de status voor alle databasebewerkingen, waaronder de status van de koppelingen voor databasereplicatie. |
| [sp_wait_for_database_copy_sync (Azure SQL Database)](/sql/relational-databases/system-stored-procedures/active-geo-replication-sp-wait-for-database-copy-sync) |zorgt ervoor dat de toepassing moet worden gewacht tot alle doorgevoerde transacties worden gerepliceerd en bevestigd door de actieve secundaire database. |
|  | |

## <a name="manage-sql-database-failover-using-powershell"></a>Failover van SQL database met behulp van PowerShell beheren

| Cmdlet | Beschrijving |
| --- | --- |
| [Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase) |Hiermee haalt u een of meer databases op. |
| [New-AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/new-azurermsqldatabasesecondary) |Hiermee maakt u een secundaire database voor een bestaande database en wordt gegevensreplicatie gestart. |
| [Set-AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/set-azurermsqldatabasesecondary) |Hiermee wordt overgeschakeld op een secundaire database als primaire om de failover te initiëren. |
| [Remove-AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/remove-azurermsqldatabasesecondary) |Hiermee wordt gegevensreplicatie tussen een SQL Database en de opgegeven secundaire database beëindigd. |
| [Get-AzureRmSqlDatabaseReplicationLink](/powershell/module/azurerm.sql/get-azurermsqldatabasereplicationlink) |Hiermee haalt u de geo-replicatiekoppelingen tussen een Azure SQL Database en een resourcegroep of SQL Server op. |
| [New-AzureRmSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/set-azurermsqldatabasefailovergroup) |   Met deze opdracht wordt een failovergroep gemaakt en geregistreerd op de primaire en secundaire servers|
| [Remove-AzureRmSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/remove-azurermsqldatabasefailovergroup) | Hiermee verwijdert u de failovergroep van de server en verwijdert alle secundaire databases opgenomen in de groep |
| [Get-AzureRmSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/get-azurermsqldatabasefailovergroup) | Hiermee haalt de configuratie van de failover |
| [Set-AzureRmSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/set-azurermsqldatabasefailovergroup) |   Hiermee wijzigt u de configuratie van de failovergroep |
| [Switch-AzureRMSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/switch-azurermsqldatabasefailovergroup) | Triggers failover van de failovergroep naar de secundaire server |
|  | |

> [!IMPORTANT]
> Zie voor voorbeelden van scripts, [en failover een individuele database met behulp van actieve geo-replicatie configureren](scripts/sql-database-setup-geodr-and-failover-database-powershell.md), [en failover een gegroepeerde-database met behulp van actieve geo-replicatie configureren](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md), en [ Configureren en failover een failover-groep voor een individuele database](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md).
>

## <a name="manage-sql-database-failover-using-the-rest-api"></a>Beheren van een failover van SQL database met behulp van de REST-API
| API | Beschrijving |
| --- | --- |
| [Maken of bijwerken-Database (createMode = herstellen)](/rest/api/sql/Databases/CreateOrUpdate) |Gemaakt, bijgewerkt of hersteld van een primaire of secundaire database. |
| [Get maken of bijwerken van de Status van Database](/rest/api/sql/Databases/CreateOrUpdate) |Retourneert de status tijdens een bewerking voor maken. |
| [Stel de secundaire Database als primaire (geplande Failover)](/rest/api/sql/replicationlinks/failover) |Sets die replica-database primaire door failover wordt uitgevoerd vanuit de huidige primaire replica-database is. |
| [Stel de secundaire Database als primaire (niet-geplande Failover)](/rest/api/sql/replicationlinks/failoverallowdataloss) |Sets die replica-database primaire door failover wordt uitgevoerd vanuit de huidige primaire replica-database is. Met deze bewerking kan leiden tot verlies van gegevens. |
| [Ophalen van replicatiekoppeling](/rest/api/sql/replicationlinks/get) |Hiermee haalt een bepaalde replicatiekoppeling voor een bepaalde SQL-database in een partnerschap geo-replicatie. Het ophalen van de informatie in de catalogusweergave sys.geo_replication_links zichtbaar. |
| [Koppelingen voor databasereplicatie - lijst per Database](/rest/api/sql/replicationlinks/listbydatabase) | Hiermee haalt alle koppelingen voor databasereplicatie voor een bepaalde SQL-database in een partnerschap geo-replicatie. Het ophalen van de informatie in de catalogusweergave sys.geo_replication_links zichtbaar. |
| [Verwijderen van replicatiekoppeling](/rest/api/sql/databases/delete) | Hiermee verwijdert u een databasereplicatiekoppeling. Kan niet worden uitgevoerd tijdens de failover. |
| [Maken of bijwerken van de Failovergroep](/rest/api/sql/failovergroups/createorupdate) | Hiermee maken of bijwerken van een failovergroep |
| [Failover-groep verwijderen](/rest/api/sql/failovergroups/delete) | Hiermee verwijdert u de failovergroep van de server |
| [Failover (gepland)](/rest/api/sql/failovergroups/failover) | Failover-schakeling van de huidige primaire server naar deze server. |
| [Geforceerde Failover verlies van gegevens toestaan](/rest/api/sql/failovergroups/forcefailoverallowdataloss) |ails via van de huidige primaire server naar deze server. Met deze bewerking kan leiden tot verlies van gegevens. |
| [Get-Failovergroep](/rest/api/sql/failovergroups/get) | Hiermee haalt u een failovergroep. |
| [Lijst met Failover-groepen door Server](/rest/api/sql/failovergroups/listbyserver) | Geeft een lijst van de failover-groepen in een server. |
| [Failover-groep bijwerken](/rest/api/sql/failovergroups/update) | Werkt een failovergroep. |
|  | |

## <a name="next-steps"></a>Volgende stappen
* Zie voor voorbeelden van scripts:
   - [PowerShell gebruiken voor het configureren van actieve geo-replicatie voor één Azure SQL-database](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)
   - [PowerShell gebruiken voor het configureren van actieve geo-replicatie voor een gepoolde Azure SQL-database](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md)
   - [Een failovergroep configureren en een failover uitvoeren voor een individuele database](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md)
* Zie voor een overzicht voor zakelijke continuïteit en scenario's, [overzicht voor zakelijke continuïteit](sql-database-business-continuity.md)
* Voor meer informatie over Azure SQL Database geautomatiseerde back-ups, Zie [geautomatiseerde back-ups van SQL-Database](sql-database-automated-backups.md).
* Zie voor meer informatie over het gebruik van geautomatiseerde back-ups voor herstel, [een database herstellen vanuit back-ups service geïnitieerde](sql-database-recovery-using-backups.md).
* Zie voor meer informatie over vereisten voor een nieuwe primaire server en database authentication, [SQL Database-beveiliging na herstel na noodgevallen](sql-database-geo-replication-security-config.md).

