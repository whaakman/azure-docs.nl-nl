---
title: Azure SQL Database modellen kopen | Microsoft Docs
description: Meer informatie over het aanschaffen van model voor Azure SQL Database.
services: sql-database
author: CarlRabeler
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 07/16/2018
manager: craigg
ms.author: carlrab
ms.openlocfilehash: f40fe3da6874d8656c9c0a0ddce9fed602cb25f9
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39091912"
---
# <a name="azure-sql-database-purchasing-models-and-resources"></a>Azure SQL Database-modellen en -bronnen aanschaffen 

Logische servers in [Azure SQL Database](sql-database-technical-overview.md) biedt twee aankopen modellen voor berekening, opslag en i/o-resources: een op DTU gebaseerde aankoopmodel en een op vCore gebaseerde aankoopmodel. 

> [!NOTE]
> [Beheerde exemplaren](sql-database-managed-instance.md) in Azure SQL Database alleen bieden het op vCore gebaseerde aankoopmodel.

De volgende tabel en de grafiek vergelijken en deze twee aankopen modellen contrast.

> [!IMPORTANT]
> Zie voor vCore gebaseerde aankoopmodel, [vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md)

|**Aankopen model**|**Beschrijving**|**Het meest geschikt voor**|
|---|---|---|
|Model op basis van DTU|Dit model is gebaseerd op een gecombineerde meting van compute, opslag- en i/o-resources. Prestatieniveaus worden uitgedrukt in termen van DTU’s (DTU’s: Database Transaction Units) voor enkelvoudige databases en eDTU’s (eDTU’s: elastische DTU’s) voor elastische pools. Zie voor meer informatie over dtu's en Edtu's [wat zijn dtu's en edtu's](sql-database-service-tiers.md#what-are-database-transaction-units-dtus)?|Bij beste voor klanten die eenvoudige, vooraf geconfigureerde opties willen.| 
|Model op basis van vCore|Dit model kunt u reken- en opslagresources onafhankelijk van elkaar schalen. Ook kunt u Azure Hybrid Benefit voor SQL Server gebruiken om te krijgen van kosten te besparen.|Bij beste voor klanten die flexibiliteit, controle en transparantie waarde.|
||||  

![prijsmodel](./media/sql-database-service-tiers/pricing-model.png)

## <a name="vcore-based-purchasing-model"></a>op vCore gebaseerde aankoopmodel 

Een virtuele kern staat voor de logische CPU met een optie te kiezen tussen verschillende hardwaregeneraties. De vCore-aanschafmodel biedt de flexibiliteit, controle, transparantie van afzonderlijke resources en een eenvoudige manier te vertalen on-premises vereisten workloads naar de cloud. Dit model kunt u Computing kunt schalen, het geheugen en opslag op basis van hun werkbelasting. In het op vCore gebaseerde aankoopmodel, klanten kunnen kiezen uit voor algemeen gebruik en kritieke zakelijke-Servicelagen voor zowel [enkelvoudige databases](sql-database-single-database-scale.md) en [elastische pools](sql-database-elastic-pool.md). 

Het op vCore gebaseerde aankoopmodel kunt u onafhankelijk van elkaar schalen reken- en opslagresources, overeenkomen met de on-premises prestaties en prijs te optimaliseren. Als uw database of elastische pool meer dan 300 DTU conversie naar vCore verbruikt kan uw kosten verlagen. U kunt converteren met behulp van uw API van keuze of de Azure-portal, zonder uitvaltijd. Conversie is echter niet vereist. Als het DTU-aankoopmodel voldoet aan uw prestaties en zakelijke vereisten, moet u deze blijven gebruiken. Als u converteren van het DTU-model naar het vCore-model wilt, moet u het prestatieniveau dat met behulp van de volgende vuistregel: elke 100 DTU in Standard-laag moet ten minste 1 vCore in de categorie Algemeen gebruik; elke 125 DTU in Premium-laag moet ten minste 1 vCore in de laag bedrijfskritiek.

In het op vCore gebaseerde aankoopmodel betalen klanten voor:
- COMPUTE (servicelaag + aantal vCores + generatie van de hardware) *
- Type en de hoeveelheid opslag van gegevens en logboekbestanden 
- Aantal IOs **
- Back-upopslag (RA-GRS) ** 

\* In de eerste openbare preview-versie, de Gen 4 logische CPU's zijn gebaseerd op Intel E5-2673 v3 (Haswell)-processors voor 2,4 GHz.

\*\* Tijdens de preview zijn 7 dagen van de back-ups en IOs gratis.

> [!IMPORTANT]
> Reken-, IOs-, gegevens en opslaan van Logboeken worden in rekening gebracht per database of elastische pool. Back-ups opslag wordt in rekening gebracht per elke database. Raadpleeg voor meer informatie van Managed Instance kosten [Azure SQL Database Managed Instance](sql-database-managed-instance.md).
> **Beperkingen regio:** het vCore-aanschafmodel is nog niet beschikbaar in de volgende regio's: West-Europa, Frankrijk-centraal, UK-Zuid, UK-West en Australië-Zuidoost.

## <a name="dtu-based-purchasing-model"></a>DTU gebaseerde aankoopmodel

De Database Throughput Unit (DTU) vertegenwoordigt een samengestelde meting van CPU, geheugen, leest en schrijft. Het op DTU gebaseerde aankoopmodel biedt een reeks vooraf geconfigureerde bundels van rekenresources en inbegrepen opslag op verschillende niveaus van de schijf van de prestaties van toepassingen. Klanten die de voorkeur geeft aan de eenvoud van een vooraf geconfigureerde bundel en vaste betalingen elke maand, vindt het dtu model beter geschikt is voor hun behoeften. In de DTU gebaseerde aankoopmodel, klanten kunnen kiezen uit **Basic**, **Standard**, en **Premium** Servicelagen voor zowel [enkelvoudige databases](sql-database-single-database-scale.md) en [elastische pools](sql-database-elastic-pool.md). 

### <a name="what-are-database-transaction-units-dtus"></a>Wat zijn Database Transaction Units (dtu's)?
Voor een individuele Azure SQL-database op een bepaald prestatieniveau binnen een [servicelaag](sql-database-single-database-scale.md), garandeert Microsoft een bepaald niveau van resources voor die database (onafhankelijk van een andere database in de Azure-cloud), biedt een voorspelbare prestatieniveau. De hoeveelheid resources wordt berekend als een getal van Database Transaction Units of dtu's en is een gecombineerde meting van compute, opslag- en i/o-resources. De verhouding tussen deze resources werd oorspronkelijk bepaald door een [OLTP-benchmark werkbelasting](sql-database-benchmark-overview.md), ontworpen om u te zijn van echte OLTP-workloads. Wanneer uw workload hoger is dan de hoeveelheid van elk van deze resources, is de doorvoer van uw beperkte - wat resulteert in tragere prestaties en time-outs. De resources die worden gebruikt voor uw werkbelasting niet invloed hebben op de beschikbare resources voor andere SQL-databases in de Azure-cloud en de resources die worden gebruikt door andere werkbelastingen niet invloed hebben op de beschikbare resources voor uw SQL-database.

![selectiekader](./media/sql-database-what-is-a-dtu/bounding-box.png)

Dtu's zijn handig voor het begrijpen van de relatieve hoeveelheid resources tussen Azure SQL-Databases op verschillende prestatieniveaus en service-lagen. Bijvoorbeeld, verdubbeling van het aantal dtu's door het verhogen van het prestatieniveau van een database is gelijk aan de set met resources waarover die database verdubbelen. Zo biedt een Premium P11-database met 1750 DTU's 350 keer meer DTU aan rekenvermogen dan een Basic-database met 5 DTU's.  

Gebruiken om te krijgen van inzicht in het gebruik van resources (DTU) van uw werkbelasting, [Azure SQL Database Query Performance Insight](sql-database-query-performance.md) aan:

- Identificeer de populairste query's op aantal CPU/duur/uitvoeringen die mogelijk kan worden afgestemd voor betere prestaties. Bijvoorbeeld, een i/o-intensieve query mogelijk baat bij het gebruik van [in het geheugen optimalisatietechnieken](sql-database-in-memory.md) beter gebruikmaken van het beschikbare geheugen op een bepaalde laag en de prestaties serviceniveau.
- Zoom in op de details van een query, de tekst en de geschiedenis van Resourcegebruik weergeven.
- Prestaties voor gegevenstoegang aanbevelingen die laten zien van acties die worden uitgevoerd door voor het afstemmen [SQL Database Advisor](sql-database-advisor.md).

U kunt wijzigen [DTU-Servicelagen](sql-database-service-tiers-dtu.md) op elk gewenst moment met minimale downtime voor uw toepassing (doorgaans minder dan vier seconden). Voor veel bedrijven en apps is het kunnen maken van databases en het naar wens omhoog of omlaag schalen van de prestaties al voldoende, vooral als de gebruikspatronen redelijk voorspelbaar zijn. Bij onvoorspelbare gebruikspatronen kan het echter lastig zijn uw kosten en bedrijfsmodel effectief te beheren. Voor dit scenario gebruikt u een elastische pool met een bepaald aantal edtu's die worden gedeeld tussen meerdere databases in de groep.

![Inleiding tot SQL Database: DTU's van individuele database per laag en niveau](./media/sql-database-what-is-a-dtu/single_db_dtus.png)

### <a name="what-are-elastic-database-transaction-units-edtus"></a>Wat zijn de elastic Database Transaction Units (edtu's)?
Dan bieden een toegewezen set met resources (dtu's) die mogelijk niet altijd nodig zijn voor een SQL-Database die altijd beschikbaar is, kunt u in plaats van databases plaats een [elastische pool](sql-database-elastic-pool.md) op een SQL-Database-server die een groep resources tussen deelt Deze databases. De gedeelde bronnen in een elastische pool worden gemeten op basis van elastische Database Transaction Units of edtu's. Elastische pools bieden een eenvoudige en kosteneffectieve oplossing voor het beheren van de prestatiedoelen voor meerdere databases met breed uiteenlopende en onvoorspelbare gebruikspatronen. Een elastische pool garandeert resources kunnen niet worden gebruikt door een database in de pool, terwijl altijd ervoor te zorgen dat elke database in de pool een minimale hoeveelheid benodigde bronnen beschikbaar heeft. 

![Inleiding tot SQL Database: eDTU's per laag en niveau](./media/sql-database-what-is-a-dtu/sqldb_elastic_pools.png)

Een pool krijgt een bepaald aantal edtu's voor een vaste prijs. Binnen de elastische pool hebben afzonderlijke databases de flexibiliteit om de schaal automatisch aan te passen binnen de geconfigureerde grenzen. Een database zwaarder belast verbruiken meer edtu's om te voldoen aan de vraag. Databases bij lichter belasting wordt minder edtu's verbruiken. Databases met geen belasting verbruiken geen edtu's. Door de inrichting van resources voor de hele pool, in plaats van per database, worden beheertaken vereenvoudigd, bieden een voorspelbaar budget voor de pool.

Er kunnen extra eDTU's aan een bestaande pool worden toegevoegd zonder uitvaltijd van de database en zonder gevolgen voor de databases in de pool. En als de extra eDTU's niet meer nodig zijn, kunnen ze op elk moment uit een bestaande pool worden verwijderd. U kunt toevoegen of verwijderen uit de groep of de limiet voor de hoeveelheid edtu's een database kunt gebruiken onder zware belasting te reserveren edtu's voor andere databases. Als een database is naar verwachting minder resources, die u kunt deze uit de pool verplaatsen en configureren als een individuele database met een voorspelbare hoeveelheid vereiste resources.

### <a name="how-can-i-determine-the-number-of-dtus-needed-by-my-workload"></a>Hoe bepaal ik hoeveel DTU's ik met mijn workload nodig heb?
Als u een bestaande workload voor virtuele on-premises of SQL Server-machines wilt migreren naar Azure SQL Database, kunt u de [DTU-rekenmachine](http://dtucalculator.azurewebsites.net/) gebruiken om een schatting te maken van het aantal benodigde DTU's. Voor een bestaande Azure SQL Database-workload, kunt u [SQL Database Query Performance Insight](sql-database-query-performance.md) om uw verbruik van databaseresources (dtu's) om te krijgen van inzicht voor het optimaliseren van uw werkbelasting te begrijpen. U kunt ook de [sys.dm_db_ resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) DMV om gebruik van resources voor het afgelopen uur weer te geven. U kunt ook de catalogusweergave [sys.resource_stats](http://msdn.microsoft.com/library/dn269979.aspx) resourceverbruik voor de afgelopen 14 dagen, maar op een lagere betrouwbaarheid van gemiddeld vijf minuten worden weergegeven.

### <a name="how-do-i-know-if-i-could-benefit-from-an-elastic-pool-of-resources"></a>Hoe weet ik of een elastische pool met resources voor mij voordeliger is?
Pools zijn geschikt voor een groot aantal databases met specifieke gebruikspatronen. Voor een bepaalde database, wordt dit patroon gekenmerkt door een gemiddelde laag gebruik met relatief incidentele gebruikspieken. SQL Database evalueert automatisch het historisch resourcegebruik van databases in een bestaande SQL Database-server en op basis daarvan wordt de juiste poolconfiguratie in de Azure-portal aanbevolen. Zie [Wanneer moet een elastische pool worden gebruikt?](sql-database-elastic-pool.md) voor meer informatie.

### <a name="what-happens-when-i-hit-my-maximum-dtus"></a>Wat gebeurt er wanneer ik mijn maximale dtu's?
Prestatieniveaus worden gekalibreerd en gelden voor de resources die nodig zijn om de werkbelasting van uw database tot de maximaal toegestane aantal voor het geselecteerde service tier/prestatieniveau te voeren. Als uw werkbelasting een van de CPU/gegevens-i/o/Log i/o-limieten bereikt is, blijft u het maximum aantal toegestane resources ontvangen, maar u waarschijnlijk ook toegenomen query latenties ondervindt. Deze limieten resulteren niet in fouten, maar wel in een vertraging van de workload, tenzij de vertraging zo ernstig wordt dat query's een time-out geven. Als u de maximale toegestane gelijktijdige sessies/gebruikersaanvragen (werkthreads) hebt bereikt, ziet u expliciete fouten. Zie [Azure SQL Database-resourcebeperkingen]( sql-database-resource-limits.md#what-happens-when-database-resource-limits-are-reached) voor informatie over de resourcelimieten niet met betrekking tot CPU, geheugen, gegevens-i/o- of i/o-transactie-logboek.

### <a name="correlating-benchmark-results-to-real-world-database-performance"></a>Benchmarkresultaten op de prestaties van de echte wereld database correleren
Het is belangrijk om te begrijpen dat alle benchmarks representatieve en indicatieve slechts zijn. De transactietarieven bereikt met de benchmark-toepassing zich niet hetzelfde als die kunnen worden behaald met andere toepassingen. De benchmark bestaat uit een verzameling van andere transactie typen worden uitgevoerd op basis van een schema met een scala aan tabellen en gegevenstypen. Terwijl de benchmark de dezelfde basisbewerkingen die gemeenschappelijk voor alle OLTP-werkbelastingen oefent zijn, vertegenwoordigt een specifieke klasse van de database of een toepassing niet meer. Het doel van de benchmark-is voor een redelijke handleiding voor de relatieve prestaties van een database die kan worden verwacht voor het omhoog of omlaag schalen tussen prestatieniveaus. Verschillende combinaties van werkbelastingen optreden en reageren op verschillende manieren in werkelijkheid databases van verschillende grootte en complexiteit zijn. Bijvoorbeeld, een i/o-intensieve toepassingen sneller i/o-drempelwaarden kan bereikt of een toepassing CPU-intensieve CPU-limieten sneller bereikt. Er is geen garantie dat een bepaalde database wordt geschaald op dezelfde manier als u de benchmark onder toenemende belasting.

De benchmark- en de methoden worden hieronder in detail beschreven.

### <a name="benchmark-summary"></a>Benchmark-overzicht
ASDB meet de prestaties van een combinatie van de voornaamste databasebewerkingen die treden het vaakst in online workloads (OLTP) voor transactieverwerking. Hoewel de benchmark is ontworpen met cloudcomputing in gedachten, het databaseschema, invullen van de gegevens en transacties zijn ontworpen voor breed representatief zijn voor de basiselementen die het meest gebruikt in een OLTP-workloads.

### <a name="schema"></a>Schema
Het schema is ontworpen voor onvoldoende diversiteit en complexiteit voor de ondersteuning van een breed scala aan bewerkingen hebben. De benchmark wordt uitgevoerd op basis van een database die bestaat uit zes tabellen. De tabellen kunnen worden onderverdeeld in drie categorieën: vaste grootte, schalen en groeit. Er zijn twee vaste grootte tabellen. drie vergroten/verkleinen tabellen. en één groeiende tabel. Vaste grootte tabellen hebben een constant aantal rijen. Vergroten/verkleinen tabellen hebben een kardinaliteit die evenredig is met de prestaties van de database is, maar niet wijzigen tijdens de benchmark. Het formaat van de groeiende tabel wordt aangepast als een tabel vergroten/verkleinen op de eerste keer wordt geladen, maar de kardinaliteit wijzigingen tijdens het uitvoeren van de benchmark rijen zijn ingevoegd en verwijderd.

Het schema bevat een combinatie van gegevenstypen, inclusief geheel getal, numerieke tekens en datum/tijd. Het schema bevat primaire en secundaire sleutels, maar geen refererende sleutels - dat wil zeggen, er zijn geen beperkingen voor referentiële integriteit tussen tabellen.

De gegevens voor de initiële database wordt gegenereerd door een programma van gegevens genereren. Geheel getal zijn en numerieke gegevens wordt gegenereerd met verschillende strategieën gehanteerd. In sommige gevallen worden waarden willekeurig verdeeld over een bereik. In andere gevallen wordt een set waarden is willekeurig gepermuteerde om ervoor te zorgen dat een specifieke-distributie die wordt onderhouden. Tekstvelden zijn gegenereerd op basis van een lijst met woorden realistische uitziende gegevens produceren.

De database wordt de grootte op basis van een 'schaalfactor'. De schaalfactor (afgekort als SF) bepaalt de kardinaliteit van de schaal en tabellen groeit. Zoals hieronder wordt beschreven in de sectie gebruikers en Pacing, de grootte van de database, het aantal gebruikers en de maximale prestaties, al naar gelang van elkaar schalen.

### <a name="transactions"></a>Transacties
De werkbelasting bestaat uit negen transactietypen, zoals weergegeven in de onderstaande tabel. Elke transactie is ontworpen voor het markeren van een bepaalde set kenmerken in de database-engine en systeem hardware, met een hoog contrast uit de andere transacties. Deze aanpak maakt het eenvoudiger om de impact van de verschillende onderdelen voor de algehele prestaties vast te stellen. De transactie "Lezen zware" levert bijvoorbeeld een groot aantal leesbewerkingen van schijf.

| Transactietype | Beschrijving |
| --- | --- |
| Lite lezen |SELECTEER; in het geheugen; alleen-lezen |
| Gemiddeld lezen |SELECTEER; voornamelijk in het geheugen; alleen-lezen |
| Zware lezen |SELECTEER; meestal niet in het geheugen; alleen-lezen |
| Update Lite |BIJWERKEN. in het geheugen; lezen / schrijven |
| Zware bijwerken |BIJWERKEN. meestal niet in het geheugen; lezen / schrijven |
| Lite invoegen |INVOEGEN. in het geheugen; lezen / schrijven |
| Zware invoegen |INVOEGEN. meestal niet in het geheugen; lezen / schrijven |
| Verwijderen |VERWIJDEREN; combinatie van in het geheugen en niet in het geheugen; lezen / schrijven |
| CPU-intensief |SELECTEER; in het geheugen; relatief intensief CPU-belasting; alleen-lezen |

### <a name="workload-mix"></a>Combinatie van de werkbelasting
Transacties worden willekeurig geselecteerd uit een gewogen distributie met de volgende algemene combinatie. De algehele mix heeft een verhouding lezen/schrijven van ongeveer 2:1.

| Transactietype | % van de combinatie |
| --- | --- |
| Lite lezen |35 |
| Gemiddeld lezen |20 |
| Zware lezen |5 |
| Update Lite |20 |
| Zware bijwerken |3 |
| Lite invoegen |3 |
| Zware invoegen |2 |
| Verwijderen |2 |
| CPU-intensief |10 |

### <a name="users-and-pacing"></a>Gebruikers en pacing
De benchmark-werkbelasting wordt aangestuurd vanuit een hulpprogramma waarmee transacties worden verzonden via een reeks verbindingen om te simuleren dat het gedrag van een aantal gelijktijdige gebruikers. Hoewel alle verbindingen en transacties machine gegenereerd zijn, voor het gemak verwijzen we naar deze verbindingen als 'gebruikers'. Hoewel elke gebruiker onafhankelijk van alle andere gebruikers werkt, voert u alle gebruikers de dezelfde cyclus van stappen die hieronder wordt weergegeven:

1. Een databaseverbinding tot stand brengen.
2. Herhalen totdat een signaal ontvangen om af te sluiten:
   * Selecteer een transactie in willekeurige volgorde (uit een gewogen distributie).
   * De geselecteerde transactie uitvoeren en de reactietijd meten.
   * Wachten op een pacing vertraging.
3. Sluit de verbinding met de database.
4. Afsluiten.

De pacing vertraging (in stap 2c) willekeurig is geselecteerd, maar met een distributie die een gemiddelde van 1.0 seconde heeft. Elke gebruiker kan dus gemiddeld genereren voor maximaal één transactie per seconde.

### <a name="scaling-rules"></a>Regels voor vergroten/verkleinen
Het aantal gebruikers wordt bepaald door de grootte van de database (in eenheden van de schaal van meerdere factoren). Er is een gebruiker voor elke vijf schaalfactor eenheden. Vanwege de pacing vertraging, één gebruiker gemiddeld hoogstens één transactie per seconde genereren.

Bijvoorbeeld, een scale-factor van 500 (SF = 500) database heeft 100 gebruikers en kunnen maar liefst maximaal 100 TPS. Om een hogere TPS vereist tarief meer gebruikers en een grotere database.

De onderstaande tabel ziet u het aantal gebruikers daadwerkelijk aanhoudend voor elke servicelaag en het prestatieniveau van een serviceniveau.

| Service Tier (prestatieniveau) | Gebruikers | Databaseomvang |
| --- | --- | --- |
| Basic |5 |720 MB |
| Standard (S0) |10 |1 GB |
| Standard (S1) |20 |2.1 GB |
| Standard (S2) |50 |7.1 GB |
| Premium (P1) |100 |14 GB |
| Premium (P2) |200 |28 GB |
| Premium (P6) |800 |114 GB |

### <a name="measurement-duration"></a>Duur van de meting
Een geldige benchmark uitvoeren vereist een stabiele status meting duur van ten minste één uur.

### <a name="metrics"></a>Metrische gegevens
De belangrijkste metrische gegevens in de benchmark zijn doorvoer en reactietijd.

* Doorvoer is de maateenheid voor essentiële prestaties in de benchmark. Doorvoer wordt gerapporteerd transacties per van-tijdseenheid, alle transactietypen tellen.
* Reactietijd is een maateenheid voor voorspelbare prestaties. De beperking van de tijd reactie is afhankelijk van de klasse van de service, met hogere klassen van de service met een strengere antwoord tijd nodig is, zoals hieronder wordt weergegeven.

| Klasse van Service | Meten van doorvoer | Antwoord tijd nodig is |
| --- | --- | --- |
| Premium |Transacties per seconde |95e percentiel op 0,5 seconden |
| Standard |Transacties per minuut |90e percentiel op 1.0 seconden |
| Basic |Transacties per uur |80e percentiel op 2.0 seconden |

## <a name="next-steps"></a>Volgende stappen

- Zie voor vCore gebaseerde aankoopmodel, [vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md)
- Zie voor de DTU gebaseerde aankoopmodel, [DTU gebaseerde aankoopmodel](sql-database-service-tiers-dtu.md).
