---
title: Azure SQL Database aanschaffen modellen | Microsoft Docs
description: Meer informatie over het aanschaffen van een model voor Azure SQL Database.
services: sql-database
author: CarlRabeler
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 06/20/2018
manager: craigg
ms.author: carlrab
ms.openlocfilehash: 0b38054ec623dc34daa3d18612e3356475655f35
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "36331322"
---
# <a name="azure-sql-database-purchasing-models-and-resources"></a>Azure SQL Database aanschaffen van modellen en resources 

Logische-servers in [Azure SQL Database](sql-database-technical-overview.md) biedt twee aankopen modellen voor compute, storage en i/o-resources: een DTU-aankoopmodel en een vCore gebaseerde aankoopmodel (preview). 

> [!NOTE]
> [Beheerde exemplaren](sql-database-managed-instance.md) in Azure SQL Database alleen bieden op basis van vCore aankoopmodel.

De volgende tabel en grafiek vergelijken en contrast van deze twee aankopen modellen.

> [!IMPORTANT]
> Zie voor vCore gebaseerde aankoopmodel (preview), [aankoopmodel vCore gebaseerde](sql-database-service-tiers-vcore.md)

|**Aankoopmodel**|**Beschrijving**|**Bij beste voor**|
|---|---|---|
|Model op basis van DTU|Dit model is gebaseerd op een gecombineerde meting van de berekenings-, opslag- en i/o-resources. Prestatieniveaus worden uitgedrukt in termen van DTU’s (DTU’s: Database Transaction Units) voor enkelvoudige databases en eDTU’s (eDTU’s: elastische DTU’s) voor elastische pools. Zie voor meer informatie over dtu's en edtu's [wat dtu's en edtu's zijn](sql-database-what-is-a-dtu.md)?|Bij beste voor klanten die eenvoudige, vooraf geconfigureerde opties willen.| 
|Model op basis van vCore|Dit model kunt u de berekenings-en opslagbronnen onafhankelijk van elkaar schalen. Ook kunt u Azure hybride voordeel voor SQL Server gebruiken om te krijgen van kosten te besparen.|Bij beste voor flexibiliteit, controle en transparantie value-klanten.|
||||  

![prijsmodel](./media/sql-database-service-tiers/pricing-model.png)

## <a name="vcore-based-purchasing-model--preview"></a>op basis van vCore aankoopmodel (preview)

Een virtuele core vertegenwoordigt de logische CPU aangeboden met de optie te kiezen tussen generaties van hardware. De vCore gebaseerde aankoopmodel (preview) biedt uw flexibiliteit, de controle, de transparantie van afzonderlijke brongebruik en een eenvoudige manier om te vertalen naar on-premises werklastvereisten naar de cloud. Dit model kunt u scale compute, geheugen en opslag op basis van hun werkbelasting behoeften. In de vCore gebaseerde aankoopmodel (preview), klanten kunnen kiezen tussen algemeen en zakelijke kritische Servicelagen (preview) voor zowel [databases eenmalige](sql-database-single-database-scale.md) en [elastische pools](sql-database-elastic-pool.md). 

hij vCore gebaseerde aankoopmodel (preview) kunt u onafhankelijk van elkaar schalen berekenings-en opslagbronnen, overeenkomen met de prestaties van de lokale en prijs optimaliseren. Als de database of de elastische groep meer dan 300 DTU conversie naar vCore verbruikt, kan uw kosten verminderen. U kunt met behulp van uw keuze-API of met behulp van de Azure-portal, zonder uitvaltijd converteren. Conversie is echter niet vereist. Als de aankoopmodel DTU voldoet aan uw prestatie- en bedrijfsvereisten, moet u blijven gebruiken. Als u besluit het DTU-model converteren naar vCore-model, moet u het prestatieniveau met behulp van de volgende vuistregel: elke 100 DTU in Standard-laag moet ten minste 1 vCore in algemeen gebruik laag; elke 125 DTU in Premium-laag moet ten minste 1 vCore in kritieke zakelijke laag.

In de vCore gebaseerde aankoopmodel (preview) betalen klanten voor:
- Berekenen (servicelaag + aantal vCores + generatie hardware) *
- Type en de hoeveelheid gegevens en logboekbestanden opslag 
- Aantal IOs **
- Back-up van opslagaccount (RA-GRS) ** 

\* In de eerste public preview de Gen 4 logische CPU's zijn gebaseerd op Intel E5-2673 v3 (Haswell) 2,4 GHz-processors.

\*\* Tijdens de preview zijn 7 dagen van de back-ups en IOs beschikbaar.

> [!IMPORTANT]
> Reken-, IOs, gegevens en opslag voor toepassingslogboeken per database of elastische pool in rekening worden gebracht. Back-ups opslag wordt in rekening gebracht per elke database. Raadpleeg voor meer informatie van beheerde exemplaar kosten [Azure SQL Database-beheerd instantie](sql-database-managed-instance.md).
> **Beperkingen van de regio:** de vCore gebaseerde aankoopmodel (preview) is nog niet beschikbaar in de volgende gebieden: West-Europa, Frankrijk centraal, VK Zuid, VK West en Australië-Zuidoost.

## <a name="dtu-based-purchasing-model"></a>Op basis van DTU aankoopmodel

De doorvoer DTU (Database Unit) vertegenwoordigt een gecombineerde meting van CPU, geheugen, leest en schrijft. De DTU-aankoopmodel biedt een reeks vooraf geconfigureerde bundels van rekenresources en opslag op verschillende niveaus van toepassingsprestaties station opgenomen. Klanten die de voorkeur aan van een vooraf geconfigureerde bundel en vaste betalingen elke maand, vinden het model op basis van DTU meer geschikt zijn voor hun behoeften. In de DTU gebaseerde aankoopmodel, klanten kunnen kiezen tussen **Basic**, **standaard**, en **Premium** Servicelagen voor beide [eenmalige databases](sql-database-single-database-scale.md) en [elastische pools](sql-database-elastic-pool.md). 

### <a name="what-are-database-transaction-units-dtus"></a>Wat zijn's (Database Transaction Units)?
Voor één Azure SQL database op een specifiek prestatieniveau binnen een [servicelaag](sql-database-single-database-scale.md), Microsoft garandeert een bepaalde mate van resources voor die database (onafhankelijk van een andere database in de Azure-cloud) bieden een voorspelbare niveau van de prestaties. De hoeveelheid resources wordt berekend als een aantal Database Transaction Units of dtu's en is een gebundelde meting van de berekenings-, opslag- en i/o-resources. De verhouding tussen deze resources oorspronkelijk is bepaald door een [benchmark-OLTP-werkbelasting](sql-database-benchmark-overview.md), leidt tot die kenmerkend zijn voor echte OLTP-werkbelastingen. Wanneer uw werkbelasting het bedrag van elk van deze resources overschrijdt, is uw doorvoer beperkte - resulterende in tragere prestaties en time-outs. De beschikbare bronnen voor andere SQL-databases in de Azure-cloud hebben geen invloed op de bronnen die worden gebruikt door de werkbelasting en de resources die worden gebruikt door andere werkbelastingen hebben geen invloed op de beschikbare bronnen voor de SQL-database.

![selectiekader](./media/sql-database-what-is-a-dtu/bounding-box.png)

Dtu's zijn vooral handig zijn om de relatieve hoeveelheid resources tussen Azure SQL-Databases op verschillende prestatieniveaus en Servicelagen te begrijpen. Bijvoorbeeld, verdubbeling van het aantal dtu's door het verhogen van het prestatieniveau van een database is gelijk aan de set beschikbare bronnen voor die database verdubbelen. Zo biedt een Premium P11-database met 1750 DTU's 350 keer meer DTU aan rekenvermogen dan een Basic-database met 5 DTU's.  

Gebruik voor het verkrijgen van meer inzicht in het brongebruik (DTU) van uw werkbelasting [Azure SQL Database Query Performance Insight](sql-database-query-performance.md) naar:

- De top-query's op het aantal CPU/duur/uitvoering die mogelijk kan worden afgestemd voor verbeterde prestaties identificeren. Bijvoorbeeld, een i/o-intensieve query kan profiteren van het gebruik van [in het geheugen optimalisatietechnieken](sql-database-in-memory.md) beter gebruikmaken van het beschikbare geheugen op een bepaalde prijscategorie en prestatieniveau serviceniveau.
- Inzoomen op de details van een query, geven de tekst en de geschiedenis van bronnen beter worden benut.
- Access-prestaties afstemmen van de aanbevelingen die acties uitgevoerd door weergeven [SQL Database Advisor](sql-database-advisor.md).

U kunt wijzigen [DTU Servicelagen](sql-database-service-tiers-dtu.md) op elk gewenst moment met minimale downtime voor uw toepassing (gemiddeld over het algemeen binnen vier seconden). Voor veel bedrijven en apps is het kunnen maken van databases en het naar wens omhoog of omlaag schalen van de prestaties al voldoende, vooral als de gebruikspatronen redelijk voorspelbaar zijn. Bij onvoorspelbare gebruikspatronen kan het echter lastig zijn uw kosten en bedrijfsmodel effectief te beheren. In dit scenario gebruikt u een elastische pool met een bepaald aantal edtu's die worden gedeeld door meerdere databases in de pool.

![Inleiding tot SQL Database: DTU's van individuele database per laag en niveau](./media/sql-database-what-is-a-dtu/single_db_dtus.png)

### <a name="what-are-elastic-database-transaction-units-edtus"></a>Wat zijn de elastische Database Transaction Units (edtu's)?
In plaats daarvan dan bieden een specifieke set resources (dtu's) die mogelijk niet altijd nodig, voor een SQL-Database die altijd beschikbaar is, u kunt plaatsen databases in een [elastische pool](sql-database-elastic-pool.md) op een SQL-Database-server die een pool van resources tussen deelt Deze databases. De gedeelde bronnen in een elastische pool worden gemeten met behulp van de elastische Database Transaction Units of edtu's. Elastische pools bieden een eenvoudige, rendabele oplossing voor het beheren van de prestatiedoelstellingen voor meerdere databases met breed uiteenlopende en onvoorspelbare gebruikspatronen. Een elastische pool wordt gegarandeerd dat resources kunnen niet worden gebruikt door een database in de groep, terwijl elke database in de groep altijd gezorgd een minimale hoeveelheid benodigde resources beschikbaar heeft. 

![Inleiding tot SQL Database: eDTU's per laag en niveau](./media/sql-database-what-is-a-dtu/sqldb_elastic_pools.png)

Een pool krijgt een bepaald aantal edtu's voor de prijs van een set. Binnen de elastische pool hebben afzonderlijke databases de flexibiliteit om de schaal automatisch aan te passen binnen de geconfigureerde grenzen. Een database zwaarder belast verbruikt meer edtu's om vraag te voldoen. Databases onder lichter belasting verbruikt minder edtu's. Databases zonder belasting verbruiken geen edtu's. Door het inrichten van resources voor de hele pool zijn plaats per database beheertaken vereenvoudigd, een voorspelbare budget voorzien in de groep.

Er kunnen extra eDTU's aan een bestaande pool worden toegevoegd zonder uitvaltijd van de database en zonder gevolgen voor de databases in de pool. En als de extra eDTU's niet meer nodig zijn, kunnen ze op elk moment uit een bestaande pool worden verwijderd. U kunt toevoegen of databases aan de pool afgetrokken of limiet voor de hoeveelheid edtu's dat een database onder een zware belasting kunt gebruiken om te reserveren edtu's voor andere databases. Als een database is zoals verwacht onder-gebruik van bronnen die u kunt verplaatsen buiten de toepassingen en configureren als een individuele database met een voorspelbare hoeveelheid vereiste resources.

### <a name="how-can-i-determine-the-number-of-dtus-needed-by-my-workload"></a>Hoe bepaal ik hoeveel DTU's ik met mijn workload nodig heb?
Als u een bestaande workload voor virtuele on-premises of SQL Server-machines wilt migreren naar Azure SQL Database, kunt u de [DTU-rekenmachine](http://dtucalculator.azurewebsites.net/) gebruiken om een schatting te maken van het aantal benodigde DTU's. Voor de werkbelasting van een bestaande Azure SQL Database, kunt u [SQL Database Query Performance Insight](sql-database-query-performance.md) verbruik (Dtu) diepere inzichten voor het optimaliseren van uw werkbelasting van uw database begrijpen. U kunt ook de [sys.dm_db_ resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) DMV om brongebruik voor het afgelopen uur weer te geven. U kunt ook de catalogusweergave [sys.resource_stats](http://msdn.microsoft.com/library/dn269979.aspx) verbruik van de laatste 14 dagen, maar in een lagere betrouwbaarheid van vijf minuten gemiddelden wordt weergegeven.

### <a name="how-do-i-know-if-i-could-benefit-from-an-elastic-pool-of-resources"></a>Hoe weet ik of een elastische pool met resources voor mij voordeliger is?
Pools zijn geschikt voor een groot aantal databases met specifieke gebruikspatronen. Voor een bepaalde database wordt dit patroon gekenmerkt door een laag gebruik gemiddelde met relatief incidentele gebruik pieken. SQL Database evalueert automatisch het historisch resourcegebruik van databases in een bestaande SQL Database-server en op basis daarvan wordt de juiste poolconfiguratie in de Azure-portal aanbevolen. Zie [Wanneer moet een elastische pool worden gebruikt?](sql-database-elastic-pool.md) voor meer informatie.

### <a name="what-happens-when-i-hit-my-maximum-dtus"></a>Wat gebeurt er wanneer ik mijn maximale aantal dtu's bereikt?
Prestaties zijn kalibreren en om de resources die nodig zijn voor het uitvoeren van de werkbelasting van uw database tot het toegestane maximum voor de geselecteerde service laag/prestatieniveau geregeld. Als uw werkbelasting is kunt u door een van de CPU/gegevens i/o/logboek-i/o-limieten, u ontvangt nog altijd het maximumniveau van toegestane bronnen, maar u zult waarschijnlijk ook toegenomen query latenties ervaren. Deze limieten resulteren niet in fouten, maar wel in een vertraging van de workload, tenzij de vertraging zo ernstig wordt dat query's een time-out geven. Als u de maximale toegestane gelijktijdige sessies/gebruikersaanvragen (werkthreads) bereikt, ziet u expliciete fouten. Zie [limieten voor Azure SQL Database]( sql-database-resource-limits.md#what-happens-when-database-resource-limits-are-reached) voor meer informatie over limieten niet gerelateerd aan CPU, geheugen, i/o gegevens of transactie logboek IO.

### <a name="correlating-benchmark-results-to-real-world-database-performance"></a>Benchmarkresultaten werkelijkheid databaseprestaties correleren
Het is belangrijk te weten dat alle benchmarks representatief en indicatief alleen. Deze transactie worden verkregen met de toepassing benchmark niet hetzelfde als die kunnen worden behaald met andere toepassingen. De benchmark bestaat uit een verzameling van een andere transactie typen uitvoeren op een schema met een bereik van tabellen en gegevenstypen. Terwijl de benchmark de dezelfde basisbewerkingen die gemeenschappelijk voor alle OLTP-werkbelastingen oefent zijn, vormt een specifieke klasse van de database of de toepassing niet meer. Het doel van de benchmark is om een redelijke handleiding voor de relatieve prestaties van een database die kan worden verwacht voor het omhoog of omlaag schalen tussen prestatieniveaus. Verschillende combinaties van werkbelastingen optreden en reageert op verschillende manieren in werkelijkheid databases zijn van verschillende grootte en complexiteit. Bijvoorbeeld, een i/o-intensieve toepassingen mogelijk sneller i/o-drempelwaarden bereikt of een toepassing CPU-intensief CPU-limieten sneller mogelijk bereikt. Er is geen garantie dat een bepaalde database kunnen worden geschaald op dezelfde manier als de benchmark onder een toenemende belasting.

De benchmark en de methoden worden hieronder in detail beschreven.

### <a name="benchmark-summary"></a>Samenvatting van de benchmark
ASDB meet de prestaties van een combinatie van basic databasebewerkingen waarvoor een treden het vaakst in online transactieverwerking (OLTP)-workloads. Hoewel de benchmark is ontworpen met cloud computing in gedachten, het databaseschema, vullen met gegevens en transacties zijn ontworpen voor grote schaal representatief zijn voor de basiselementen meest worden gebruikt in een OLTP-werkbelastingen.

### <a name="schema"></a>Schema
Het schema is ontworpen om voldoende diverse en complexiteit ter ondersteuning van een breed scala aan bewerkingen. De benchmark wordt uitgevoerd op basis van een database bestaat uit zes tabellen. De tabellen die zijn onderverdeeld in drie categorieën: vaste grootte, schaalbaarheid en groeit. Er zijn twee tabellen van vaste grootte. drie vergroten/verkleinen tabellen. en één groeiende tabel. Vaste grootte tabellen hebben een constant aantal rijen. Schalen tabellen hebben een kardinaliteit die evenredig is met de prestaties van de database is, maar niet wijzigen tijdens de benchmark. De groeiende tabel wordt aangepast als een vergroten/verkleinen tabel op de eerste keer wordt geladen, maar de kardinaliteit wijzigingen tijdens het uitvoeren van de benchmark als rijen worden ingevoegd of verwijderd.

Het schema bevat een combinatie van gegevenstypen, met inbegrip van geheel getal, numerieke tekens en datum/tijd. Het schema bevat primaire en secundaire sleutels, maar geen refererende sleutels - dat wil zeggen, er zijn geen referentiële integriteitsbeperkingen tussen tabellen.

Een programma van de generatie gegevens genereert de gegevens voor de oorspronkelijke database. Geheel getal en numerieke gegevens wordt met verschillende strategieën gegenereerd. In sommige gevallen worden waarden willekeurig verdeeld over een bereik. In andere gevallen is een set waarden is willekeurig gepermuteerde om ervoor te zorgen dat een specifiek distributiepunt behouden blijft. Tekstvelden worden gegenereerd vanuit een lijst met woorden om naar te realistisch ogende gegevens produceren.

De database wordt aangepast op basis van een 'schaalfactor'. De schaalfactor (afgekort als SF) bepaalt de kardinaliteit van de schaal en groei van tabellen. Zoals hieronder wordt beschreven in de sectie gebruikers en Pacing, de grootte van de database, aantal gebruikers en de maximale prestaties alle schalen in verhouding tot elkaar.

### <a name="transactions"></a>Transacties
De werkbelasting bestaat uit negen transactietypen, zoals weergegeven in de onderstaande tabel. Elke transactie is ontworpen voor het markeren van een bepaalde set kenmerken in de database-engine en system hardware, met een hoog contrast uit de andere transacties. Deze aanpak gemakkelijker om de impact van de verschillende onderdelen van de algehele prestaties vast te stellen. De transactie 'Zware lezen' levert bijvoorbeeld een groot aantal leesbewerkingen van de schijf.

| Transactietype | Beschrijving |
| --- | --- |
| Lite lezen |SELECTEER; in het geheugen; alleen-lezen |
| Gemiddeld lezen |SELECTEER; voornamelijk in het geheugen; alleen-lezen |
| Lees zware |SELECTEER; meestal niet in het geheugen; alleen-lezen |
| Update Lite |UPDATE; in het geheugen; lezen / schrijven |
| Zware bijwerken |UPDATE; meestal niet in het geheugen; lezen / schrijven |
| INSERT Lite |INSERT; in het geheugen; lezen / schrijven |
| Zware invoegen |INSERT; meestal niet in het geheugen; lezen / schrijven |
| Delete |VERWIJDEREN; combinatie van in het geheugen en niet in het geheugen; lezen / schrijven |
| CPU-zware |SELECTEER; in het geheugen; relatief intensief CPU-belasting; alleen-lezen |

### <a name="workload-mix"></a>Combinatie van de werkbelasting
Transacties worden willekeurig geselecteerd uit een gewogen distributiepunt met de volgende algemene combinatie. De algehele mix heeft een verhouding lezen/schrijven van ongeveer 2:1.

| Transactietype | % van de combinatie |
| --- | --- |
| Lite lezen |35 |
| Gemiddeld lezen |20 |
| Lees zware |5 |
| Update Lite |20 |
| Zware bijwerken |3 |
| INSERT Lite |3 |
| Zware invoegen |2 |
| Delete |2 |
| CPU-zware |10 |

### <a name="users-and-pacing"></a>Gebruikers- en pacing
De benchmark werkbelasting wordt aangedreven vanuit een hulpprogramma waarmee transacties worden verzonden over een set van verbindingen met het gedrag van een aantal gelijktijdige gebruikers simuleren. Hoewel alle verbindingen en transacties machine gegenereerd zijn, voor het gemak noemen we deze verbindingen 'gebruikers'. Hoewel elke gebruiker onafhankelijk van alle andere gebruikers werkt, wordt in alle gebruikers de dezelfde cyclus van de onderstaande stappen uitvoeren:

1. Een databaseverbinding tot stand brengen.
2. Herhaal totdat een signaal ontvangen om af te sluiten:
   * Selecteer een transactie in willekeurige volgorde (uit een gewogen distributiepunt).
   * De geselecteerde transactie uitvoeren en de reactietijd meten.
   * Wachten op een pacing vertraging.
3. Sluit de verbinding met de database.
4. Afsluiten.

De pacing vertraging (in stap 2c) willekeurig is geselecteerd, maar met een distributiepunt die een gemiddelde van tweede 1.0 heeft. Elke gebruiker kan dus gemiddeld genereren voor maximaal één transactie per seconde.

### <a name="scaling-rules"></a>Regels voor vergroten/verkleinen
Het aantal gebruikers wordt bepaald door de grootte van de database (in schaalfactor units). Er is een gebruiker voor elke vijf schaalfactor eenheden. Vanwege de pacing vertraging één gebruiker gemiddeld maximaal één transactie per seconde genereren.

Bijvoorbeeld, een scale-factor van 500 (SF = 500)-database heeft 100 gebruikers en een maximale snelheid van 100 TPS kunt bereiken. Om een hogere TPS vereist snelheid meer gebruikers en een grotere database.

De onderstaande tabel ziet het aantal gebruikers daadwerkelijk aanhoudend voor elke prijscategorie en prestatieniveau serviceniveau.

| Servicelaag (prestatieniveau) | Gebruikers | Databasegrootte |
| --- | --- | --- |
| Standaard |5 |720 MB |
| Standard (S0) |10 |1 GB |
| Standard (S1) |20 |2.1 GB |
| Standard (S2) |50 |7.1 GB |
| Premium (P1) |100 |14 GB |
| Premium (P2) |200 |28 GB |
| Premium (P6) |800 |114 GB |

### <a name="measurement-duration"></a>Duur van de meting
Een geldig benchmark uitvoeren vereist een actieve status meting duur van ten minste één uur.

### <a name="metrics"></a>Waarden
De belangrijkste metrische gegevens in de benchmark zijn doorvoer en reactietijd.

* Doorvoer is de essentiële prestatiemeting die in de benchmark. Doorvoer wordt vermeld in transacties per eenheid-van-time, waarbij alle transactietypen wordt geteld.
* Reactietijd is een meting van prestaties, voorspelbaarheid. De beperking van de tijd antwoord verschillen afhankelijk van de klasse van de service met hogere klassen van de service met een strengere antwoord tijd vergt, zoals hieronder wordt weergegeven.

| Klasse van Service | Doorvoer meting | Antwoord tijd vergt |
| --- | --- | --- |
| Premium |Transacties per seconde |95e percentiel op 0,5 seconden |
| Standaard |Transacties per minuut |90 percentiel op 1,0 seconden |
| Standaard |Transacties per uur |80e percentiel op 2.0 seconden |

## <a name="next-steps"></a>Volgende stappen

- Zie voor vCore gebaseerde aankoopmodel (preview), [aankoopmodel vCore gebaseerde](sql-database-service-tiers-vcore.md)
- Zie voor de DTU gebaseerde aankoopmodel, [aankoopmodel DTU gebaseerde](sql-database-service-tiers-dtu.md).
