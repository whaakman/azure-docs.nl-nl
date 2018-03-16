---
title: Beheer van gelijktijdigheid van taken en de belasting in SQL Data Warehouse | Microsoft Docs
description: Begrijpen en de belasting van gelijktijdigheid management in Azure SQL Data Warehouse om oplossingen te ontwikkelen.
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: ef170f39-ae24-4b04-af76-53bb4c4d16d3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 08/23/2017
ms.author: joeyong;barbkess;kavithaj
ms.openlocfilehash: eaf2d43286dbaa52ada1430fbb7ce1e37f41c0d4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="concurrency-and-workload-management-in-sql-data-warehouse"></a>Beheer van gelijktijdigheid van taken en de belasting in SQL Data Warehouse
Voor het leveren van voorspelbare prestaties op schaal, helpt Microsoft Azure SQL Data Warehouse bepaalt u gelijktijdigheid niveaus en toewijzingen zoals geheugen en CPU-prioriteit. In dit artikel vindt u de concepten van gelijktijdigheid van taken en de belasting van beheer, waarin wordt uitgelegd hoe u beide functies geïmplementeerd en hoe u ze kunt beheren in uw datawarehouse. Beheer van SQL Data Warehouse werkbelasting is bedoeld om u ondersteuning voor omgevingen met meerdere gebruikers te helpen. Het is niet bedoeld voor werkbelastingen van meerdere tenants.

## <a name="concurrency-limits"></a>Limieten voor gelijktijdigheid van taken
SQL Data Warehouse kan maximaal 1024 gelijktijdige verbindingen. Alle 1024 verbindingen kunnen query's tegelijk verzenden. SQL Data Warehouse kan echter voor het optimaliseren van doorvoer wachtrij sommige query's om ervoor te zorgen dat elke query een minimale geheugentoekenning ontvangt. Queuing treedt op tijdens het uitvoeren van een query. Door queuing query's wanneer gelijktijdigheid limieten zijn bereikt, kunt SQL Data Warehouse verhogen totale doorvoer door ervoor te zorgen dat actieve query's toegang tot uiterste benodigde geheugenresources krijgen.  

Limieten voor gelijktijdigheid van taken worden bepaald door twee concepten: *gelijktijdige query's* en *gelijktijdigheid sleuven*. Voor een query uit te voeren, moet deze worden uitgevoerd binnen de limiet van query gelijktijdigheid van taken en van de toewijzing van de sleuf gelijktijdigheid van taken.

* Gelijktijdige query's zijn de query's op hetzelfde moment wordt uitgevoerd. SQL Data Warehouse ondersteunt maximaal 32 gelijktijdige query's op de grotere van DWU.
* Gelijktijdigheid sleuven worden toegewezen op basis van DWU. Elke DWU 100 biedt 4 gelijktijdigheid sleuven. Bijvoorbeeld, een DW100 toewijst 4 gelijktijdigheid sleuven en DW1000 40 toewijst. Elke query maakt gebruik van een of meer gelijktijdigheid sleuven, afhankelijk van de [bronklasse](#resource-classes) van de query. Query uitvoeren in de bronklasse smallrc één gelijktijdigheid sleuf in beslag nemen. Query uitvoeren in een hogere bronklasse verbruikt meer gelijktijdigheid sleuven.

De volgende tabel beschrijft de limieten voor gelijktijdige query's en gelijktijdigheid sleuven op de verschillende grootten van DWU.

### <a name="concurrency-limits"></a>Limieten voor gelijktijdigheid van taken
| DWU | Maximum aantal gelijktijdige query 's | Gelijktijdigheid sleuven toegewezen |
|:--- |:---:|:---:|
| DW100 |4 |4 |
| DW200 |8 |8 |
| DW300 |12 |12 |
| DW400 |16 |16 |
| DW500 |20 |20 |
| DW600 |24 |24 |
| DW1000 |32 |40 |
| DW1200 |32 |48 |
| DW1500 |32 |60 |
| DW2000 ZIJN |32 |80 |
| DW3000 |32 |120 |
| DW6000 |32 |240 |

Als een van deze drempels wordt voldaan, worden de nieuwe query's in de wachtrij en de eerste in, First-out ' op basis van een uitgevoerd.  Als een query's is voltooid en het aantal query's en sleuven onder de limieten valt, worden in de wachtrij query's vrijgegeven. 

> [!NOTE]  
> *Selecteer* query's uitvoeren op uitsluitend dynamische Beheerweergave bekijkt (DMV's) of catalogusweergaven niet worden geregeld door een van de limieten gelijktijdigheid van taken. U kunt het systeem ongeacht het aantal query's worden uitgevoerd op deze bewaken.
> 
> 

## <a name="resource-classes"></a>Resourceklassen
Resourceklassen helpen u de toewijzing van geheugen en CPU-cycli die worden toegewezen aan een query te beheren. U kunt twee soorten klassen resource toewijzen aan een gebruiker in de vorm van databaserollen. De twee typen resource klassen zijn als volgt:
1. Dynamische Resource-klassen (**smallrc, mediumrc, largerc, xlargerc**) een variabele hoeveelheid geheugen, afhankelijk van de huidige DWU toewijzen. Dit betekent dat wanneer u naar een grotere DWU opschalen, uw query's automatisch krijg meer geheugen. 
2. Statische Resource-klassen (**staticrc10, staticrc20, staticrc30, staticrc40, staticrc50, staticrc60, staticrc70, staticrc80**) dezelfde hoeveelheid geheugen ongeacht de huidige DWU toewijzen (mits de DWU zelf onvoldoende geheugen beschikbaar heeft). Dit betekent dat op een groter aantal dwu's, u meer query's in elke bronklasse gelijktijdig uitvoeren kunt.

Gebruikers in **smallrc** en **staticrc10** krijgen een kleinere hoeveelheid geheugen en kunnen profiteren van de hogere gelijktijdigheid van taken. Daarentegen gebruikers toegewezen aan **xlargerc** of **staticrc80** grote hoeveelheden geheugen, zijn opgegeven en daarom minder van de query's kunnen tegelijkertijd worden uitgevoerd.

Standaard elke gebruiker is lid van de resourceklasse kleine **smallrc**. De procedure `sp_addrolemember` wordt gebruikt voor het verhogen van de bronklasse en `sp_droprolemember` wordt gebruikt om te verlagen de bronklasse. Bijvoorbeeld deze opdracht de resourceklasse van loaduser naar zou verhogen **largerc**:

```sql
EXEC sp_addrolemember 'largerc', 'loaduser'
```


### <a name="queries-that-do-not-honor-resource-classes"></a>Query's die niet voldoen aan voor resource-klassen

Er zijn enkele typen query's die niet van een grotere geheugentoewijzing profiteren. Het systeem hun resourcetoewijzing klasse worden genegeerd en altijd deze query's in plaats daarvan in de resourceklasse kleine uitvoeren. Als deze query's worden altijd uitgevoerd in de kleine bronklasse, kunnen ze uitvoeren wanneer gelijktijdigheid sleuven onder druk zijn en ze meer sleuven dan nodig won't verbruiken. Zie [Resource klasse uitzonderingen](#query-exceptions-to-concurrency-limits) voor meer informatie.

## <a name="details-on-resource-class-assignment"></a>Meer informatie over klasse resourcetoewijzing


Enkele meer informatie over de bronklasse:

* *Rol ALTER* machtiging is vereist voor de resourceklasse van een gebruiker wijzigen.
* Hoewel u een gebruiker aan een of meer van de hogere resource-klassen toevoegen kunt, voorrang dynamische Bronklassen statische resource klassen. Dat wil zeggen, als een gebruiker is toegewezen aan beide **mediumrc**(dynamische) en **staticrc80**(statische) **mediumrc** is de resourceklasse die wordt gehonoreerd.
 * Wanneer een gebruiker is toegewezen aan meer dan één resourceklasse in een specifieke klasse brontype (meer dan één dynamische bronklasse of meer dan één statische bronklasse), wordt de hoogste bronklasse gehonoreerd. Dat wil zeggen, als een gebruiker is toegewezen aan zowel mediumrc als largerc, is de hogere bronklasse (largerc) gehonoreerd. En als een gebruiker is toegewezen aan beide **staticrc20** en **statirc80**, **staticrc80** wordt gebruikt.
* De bronklasse van de gebruiker met beheerdersrechten systeem kan niet worden gewijzigd.

Zie voor een gedetailleerd voorbeeld [Changing gebruiker resource klasse voorbeeld](#changing-user-resource-class-example).

## <a name="memory-allocation"></a>Toewijzing van geheugen
Er zijn voor- en nadelen voor het toenemende bronklasse van een gebruiker op. Een bronklasse voor een gebruiker verhogen, beschikt de query's meer geheugen waardoor kan betekenen dat query's sneller uitgevoerd.  Hogere resource klassen Verminder echter ook het aantal gelijktijdige query's die kunnen worden uitgevoerd. Dit is de verhouding tussen het toewijzen van grote hoeveelheden geheugen aan een enkele query of om andere query's, die ook moeten geheugentoewijzingen gelijktijdig worden uitgevoerd. Als een gebruiker kan hoog toewijzingen van geheugen voor een query is opgegeven, worden voor andere gebruikers geen toegang tot die hetzelfde geheugen een query uit te voeren.

De volgende tabel wordt het geheugen toegewezen aan elk distributiepunt door DWU en resource klasse toegewezen.

### <a name="memory-allocations-per-distribution-for-dynamic-resource-classes-mb"></a>Geheugentoewijzingen per distributiepunt voor dynamische Bronklassen (MB)
| DWU | smallrc | mediumrc | largerc | xlargerc |
|:--- |:---:|:---:|:---:|:---:|
| DW100 |100 |100 |200 |400 |
| DW200 |100 |200 |400 |800 |
| DW300 |100 |200 |400 |800 |
| DW400 |100 |400 |800 |1,600 |
| DW500 |100 |400 |800 |1,600 |
| DW600 |100 |400 |800 |1,600 |
| DW1000 |100 |800 |1,600 |3,200 |
| DW1200 |100 |800 |1,600 |3,200 |
| DW1500 |100 |800 |1,600 |3,200 |
| DW2000 ZIJN |100 |1,600 |3,200 |6,400 |
| DW3000 |100 |1,600 |3,200 |6,400 |
| DW6000 |100 |3,200 |6,400 |12,800 |

De volgende tabel wordt het geheugen toegewezen aan elk distributiepunt door DWU en statische bronklasse toegewezen. Houd er rekening mee dat de hogere resource-klassen hebben hun geheugen verlaagd om te voldoen aan de globale DWU-limieten.

### <a name="memory-allocations-per-distribution-for-static-resource-classes-mb"></a>Geheugentoewijzingen per distributiepunt voor statische resource klassen (MB)
| DWU | staticrc10 | staticrc20 | staticrc30 | staticrc40 | staticrc50 | staticrc60 | staticrc70 | staticrc80 |
|:--- |:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| DW100 |100 |200 |400 |400 |400 |400 |400 |400 |
| DW200 |100 |200 |400 |800 |800 |800 |800 |800 |
| DW300 |100 |200 |400 |800 |800 |800 |800 |800 |
| DW400 |100 |200 |400 |800 |1,600 |1,600 |1,600 |1,600 |
| DW500 |100 |200 |400 |800 |1,600 |1,600 |1,600 |1,600 |
| DW600 |100 |200 |400 |800 |1,600 |1,600 |1,600 |1,600 |
| DW1000 |100 |200 |400 |800 |1,600 |3,200 |3,200 |3,200 |
| DW1200 |100 |200 |400 |800 |1,600 |3,200 |3,200 |3,200 |
| DW1500 |100 |200 |400 |800 |1,600 |3,200 |3,200 |3,200 |
| DW2000 ZIJN |100 |200 |400 |800 |1,600 |3,200 |6,400 |6,400 |
| DW3000 |100 |200 |400 |800 |1,600 |3,200 |6,400 |6,400 |
| DW6000 |100 |200 |400 |800 |1,600 |3,200 |6,400 |12,800 |

In de voorgaande tabel ziet u dat een query uitgevoerd op een dw2000 zijn in de **xlargerc** bronklasse toegang hebt tot 6400 MB aan geheugen in elk van de 60 gedistribueerde databases.  In SQL Data Warehouse zijn er 60 distributies. Voor het berekenen van de toewijzing van het totale geheugen voor een query in een bepaalde bron-klasse moeten daarom de bovenstaande waarden worden vermenigvuldigd met 60.

### <a name="memory-allocations-system-wide-gb"></a>Geheugen toewijzingen hele systeem (GB)
| DWU | smallrc | mediumrc | largerc | xlargerc |
|:--- |:---:|:---:|:---:|:---:|
| DW100 |6 |6 |12 |23 |
| DW200 |6 |12 |23 |47 |
| DW300 |6 |12 |23 |47 |
| DW400 |6 |23 |47 |94 |
| DW500 |6 |23 |47 |94 |
| DW600 |6 |23 |47 |94 |
| DW1000 |6 |47 |94 |188 |
| DW1200 |6 |47 |94 |188 |
| DW1500 |6 |47 |94 |188 |
| DW2000 ZIJN |6 |94 |188 |375 |
| DW3000 |6 |94 |188 |375 |
| DW6000 |6 |188 |375 |750 |

In deze tabel van geheugentoewijzingen van het hele systeem, kunt u zien dat een query uitgevoerd op een dw2000 zijn in de bronklasse xlargerc een totaal van 375 GB geheugen wordt toegewezen (distributies 6400 MB * 60 * 1024 omzetten in GB) via het geheel van uw SQL Data Warehouse.

Dezelfde berekening geldt voor statische resource klassen.
 
## <a name="concurrency-slot-consumption"></a>Gelijktijdigheid sleuf verbruik  
SQL Data Warehouse verleent meer geheugen op query's uitgevoerd in de hogere resource-klassen. Geheugen is een vaste resource.  Daarom het toegewezen per query meer geheugen, de minder gelijktijdige query's kunnen uitvoeren. De volgende tabel herhaalt alle vorige concepten in één weergave die het aantal gelijktijdigheid sleuven beschikbaar door DWU en de sleuven die wordt gebruikt door de resourceklasse van elke toont.  

### <a name="allocation-and-consumption-of-concurrency-slots-for-dynamic-resource-classes"></a>Toewijzing en het verbruik van gelijktijdigheid sleuven voor dynamische Bronklassen  
| DWU | Maximum aantal gelijktijdige query 's | Gelijktijdigheid sleuven toegewezen | Gebruikt door smallrc sleuven | Gebruikt door mediumrc sleuven | Gebruikt door largerc sleuven | Gebruikt door xlargerc sleuven |
|:--- |:---:|:---:|:---:|:---:|:---:|:---:|
| DW100 |4 |4 |1 |1 |2 |4 |
| DW200 |8 |8 |1 |2 |4 |8 |
| DW300 |12 |12 |1 |2 |4 |8 |
| DW400 |16 |16 |1 |4 |8 |16 |
| DW500 |20 |20 |1 |4 |8 |16 |
| DW600 |24 |24 |1 |4 |8 |16 |
| DW1000 |32 |40 |1 |8 |16 |32 |
| DW1200 |32 |48 |1 |8 |16 |32 |
| DW1500 |32 |60 |1 |8 |16 |32 |
| DW2000 ZIJN |32 |80 |1 |16 |32 |64 |
| DW3000 |32 |120 |1 |16 |32 |64 |
| DW6000 |32 |240 |1 |32 |64 |128 |

### <a name="allocation-and-consumption-of-concurrency-slots-for-static-resource-classes"></a>Toewijzing en het verbruik van gelijktijdigheid sleuven voor statische resource klassen  
| DWU | Maximum aantal gelijktijdige query 's | Gelijktijdigheid sleuven toegewezen |staticrc10 | staticrc20 | staticrc30 | staticrc40 | staticrc50 | staticrc60 | staticrc70 | staticrc80 |
|:--- |:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| DW100 |4 |4 |1 |2 |4 |4 |4 |4 |4 |4 |
| DW200 |8 |8 |1 |2 |4 |8 |8 |8 |8 |8 |
| DW300 |12 |12 |1 |2 |4 |8 |8 |8 |8 |8 |
| DW400 |16 |16 |1 |2 |4 |8 |16 |16 |16 |16 |
| DW500 | 20| 20| 1| 2| 4| 8| 16| 16| 16| 16|
| DW600 | 24| 24| 1| 2| 4| 8| 16| 16| 16| 16|
| DW1000 | 32| 40| 1| 2| 4| 8| 16| 32| 32| 32|
| DW1200 | 32| 48| 1| 2| 4| 8| 16| 32| 32| 32|
| DW1500 | 32| 60| 1| 2| 4| 8| 16| 32| 32| 32|
| DW2000 ZIJN | 32| 80| 1| 2| 4| 8| 16| 32| 64| 64|
| DW3000 | 32| 120| 1| 2| 4| 8| 16| 32| 64| 64|
| DW6000 | 32| 240| 1| 2| 4| 8| 16| 32| 64| 128|

Uit deze tabellen ziet u dat SQL Data Warehouse wordt uitgevoerd als DW1000 een maximum van 32 gelijktijdige query's en een totaal van 40 gelijktijdigheid sleuven wijst. Als alle gebruikers worden uitgevoerd in smallrc, zou 32 gelijktijdige query's worden toegestaan, omdat elke query 1 gelijktijdigheid sleuf zou gebruiken. Als alle gebruikers op een DW1000 in mediumrc werd uitgevoerd, elke query 800 MB per distributiepunt voor een toewijzing van het totale geheugen van 47 GB per query toegewezen zou en gelijktijdigheid beperkt tot 5 gebruikers worden zou (40 gelijktijdigheid sleuven / 8-sleuven per gebruiker mediumrc).

## <a name="selecting-proper-resource-class"></a>Juiste bronklasse selecteren  
Er is een goede gewoonte permanent gebruikers toewijzen aan een resourceklasse in plaats van de resource-klassen te wijzigen. Bijvoorbeeld maken laadpogingen geclusterde columnstore-tabellen voor hogere kwaliteit indexen wanneer meer geheugen toegewezen. Om ervoor te zorgen dat belasting toegang tot hogere geheugen hebben, een gebruiker specifiek voor het laden van gegevens maken en deze gebruiker permanent toewijzen aan een hogere bronklasse.
Er zijn enkele aanbevolen procedures voor hier. Zoals eerder vermeld, SQL DW ondersteunt twee soorten klasse brontypen: statische resource klassen en dynamische Bronklassen.
### <a name="loading-best-practices"></a>Bij het laden van aanbevolen procedures
1.  Als de verwachtingen van het geladen met normale hoeveelheid gegevens, is een statische bronklasse een goede keuze. Later, wanneer het omhoog schalen meer verwerkingskracht ophalen, het datawarehouse kan worden meer gelijktijdige query's out-of-the-box, zoals de load-gebruiker niet meer geheugen verbruikt is worden uitgevoerd.
2.  Als de verwachtingen van grotere hoeveelheden in sommige gevallen, is een dynamische bronklasse een goede keuze. Later, wanneer het omhoog schalen meer verwerkingskracht ophalen, krijgt load de gebruiker meer geheugen out-of-the-box, daarom zodat de belasting sneller uitvoeren.

De hoeveelheid geheugen die nodig zijn voor de belasting efficiënt verwerken, is afhankelijk van de aard van de tabel die is geladen en de hoeveelheid gegevens verwerkt. Laden van gegevens in tabellen CCI vereist bijvoorbeeld geheugen te laten CCI rowgroups optimalisatie bereiken. Zie voor meer informatie de Columnstore-indexen - richtlijnen laden van gegevens.

Als een best practice raden wij u ten minste 200MB aan geheugen gebruiken voor belastingen.

### <a name="querying-best-practices"></a>Aanbevolen procedures voor het opvragen
Query's hebben verschillende vereisten, afhankelijk van de complexiteit. Geheugen per query verhogen of een uitbreiding van de concurrency zijn beide geldige manieren om te vergroten de totale doorvoer afhankelijk van de behoeften van de query.
1.  Als de verwachtingen van reguliere, complexe query's (bijvoorbeeld om dagelijkse en wekelijkse rapporten te genereren) en hoeft niet te profiteren van gelijktijdigheid van taken, is een dynamische bronklasse een goede keuze. Als het systeem meer gegevens heeft worden verwerkt, bieden het datawarehouse schaalt daarom automatisch meer geheugen aan de gebruiker de query wordt uitgevoerd.
2.  Als de verwachtingen van variabele of Dagverloop gelijktijdigheid patronen (bijvoorbeeld als de database een query wordt uitgevoerd via een webgebruikersinterface grotendeels toegankelijk), een statische bronklasse is een goede keuze. Later, wanneer het omhoog schalen naar datawarehouse, zich de gebruiker gekoppeld aan de statische bronklasse automatisch meer gelijktijdige query's uitvoeren.

Selecteren juiste geheugentoekenning afhankelijk van de behoeften van uw query is niet-triviale omdat deze afhankelijk van meerdere factoren, zoals de hoeveelheid gegevens die zijn opgevraagd is, de aard van het tabelschema en verschillende join, selectie en groep predicaten. Een algemeen verwerkt toewijzen van meer geheugen wordt kunnen query's sneller zijn voltooid, maar zou de gelijktijdigheid van de algehele verminderen. Als geen gelijktijdigheid van taken een probleem is, komt te veel toewijzen van geheugen niet schadelijk zijn. Om af te stemmen doorvoer, probeert verschillende versies van het resource-klassen mogelijk zijn vereist.

U kunt de volgende opgeslagen procedure gebruiken om te achterhalen gelijktijdigheid van taken en het geheugen verlenen per bronklasse op een gegeven SLO en de dichtstbijzijnde best bronklasse voor geheugen rekenintensieve CCI bewerkingen op niet-gepartitioneerde CCI tabel op een bepaalde bron-klasse:

#### <a name="description"></a>Beschrijving:  
Dit is het doel van deze opgeslagen procedure:  
1. Om te achterhalen gebruiker verlenen gelijktijdigheid van taken en het geheugen per bronklasse op een gegeven SLO. Gebruiker moet NULL bieden voor schema- en tablename voor deze, zoals weergegeven in het onderstaande voorbeeld.  
2. Om gebruiker te achterhalen van de dichtstbijzijnde best bronklasse voor het geheugen intensed CCI operations (belasting, tabel kopiëren, opnieuw opbouwen index, enzovoort) in niet-gepartitioneerde CCI tabel op een bepaalde bron-klasse. De opgeslagen procedure gebruikt tabelschema om erachter te komen de vereiste geheugentoekenning voor deze.

#### <a name="dependencies--restrictions"></a>Afhankelijkheden en beperkingen:
- Deze opgeslagen procedure is niet ontworpen voor het berekenen van de geheugenvereiste voor de tabel is gepartitioneerd cci.    
- Deze opgeslagen procedure neemt geheugenvereiste in aanmerking voor de component SELECT van INSERT-CTAS-selecteren en wordt ervan uitgegaan dat het een eenvoudige SELECT.
- Deze opgeslagen procedure wordt een tijdelijke tabel gebruikt, zodat deze kan worden gebruikt in de sessie waarin deze opgeslagen procedure is gemaakt.    
- Deze opgeslagen procedure is afhankelijk van de huidige aanbiedingen (zoals de hardwareconfiguratie, DMS config) en als een van die verandert vervolgens deze opgeslagen procedure werkt niet correct.  
- Deze opgeslagen procedure is afhankelijk van bestaande aangeboden gelijktijdigheid limiet en als die wordt gewijzigd vervolgens deze opgeslagen procedure werkt niet correct.  
- Deze opgeslagen procedure is afhankelijk van bestaande resource klasse aanbiedingen en als die vervolgens dit wijzigingen juist proc wuold werkt het programma niet opgeslagen.  

>  [!NOTE]  
>  Als u geen uitvoer na uitvoering van opgeslagen procedure met parameters die zijn opgegeven, kan er twee situaties. <br />1. Beide DW-Parameter bevat ongeldige SLO waarde <br />2. OF er zijn geen overeenkomende bronklasse voor CCI bewerking als de tabelnaam is opgegeven. <br />Bijvoorbeeld, is DW100, hoogste geheugentoekenning beschikbaar 400MB en als tabelschema is groot genoeg is om te passeren de vereiste van 400MB.
      
#### <a name="usage-example"></a>Voorbeeld van gebruik:
Syntaxis:  
`EXEC dbo.prc_workload_management_by_DWU @DWU VARCHAR(7), @SCHEMA_NAME VARCHAR(128), @TABLE_NAME VARCHAR(128)`  
1. @DWU:Ofwel een NULL-parameter voor de huidige DWU extraheren uit de DW-database of geef een ondersteunde DWU in de vorm van 'DW100' opgeven
2. @SCHEMA_NAME:Geef een schemanaam van de tabel
3. @TABLE_NAME:Geef de tabelnaam van een met het belang

Voorbeelden voor het uitvoeren van deze opgeslagen procedure:  
```sql  
EXEC dbo.prc_workload_management_by_DWU 'DW2000', 'dbo', 'Table1';  
EXEC dbo.prc_workload_management_by_DWU NULL, 'dbo', 'Table1';  
EXEC dbo.prc_workload_management_by_DWU 'DW6000', NULL, NULL;  
EXEC dbo.prc_workload_management_by_DWU NULL, NULL, NULL;  
```

Table1 gebruikt in de bovenstaande voorbeelden kan worden gemaakt zoals hieronder:  
`CREATE TABLE Table1 (a int, b varchar(50), c decimal (18,10), d char(10), e varbinary(15), f float, g datetime, h date);`

#### <a name="heres-the-stored-procedure-definition"></a>Hier volgt de definitie van de opgeslagen procedure:
```sql  
-------------------------------------------------------------------------------
-- Dropping prc_workload_management_by_DWU procedure if it exists.
-------------------------------------------------------------------------------
IF EXISTS (SELECT * FROM sys.objects WHERE type = 'P' AND name = 'prc_workload_management_by_DWU')
DROP PROCEDURE dbo.prc_workload_management_by_DWU
GO

-------------------------------------------------------------------------------
-- Creating prc_workload_management_by_DWU.
-------------------------------------------------------------------------------
CREATE PROCEDURE dbo.prc_workload_management_by_DWU
(   @DWU VARCHAR(7),
      @SCHEMA_NAME VARCHAR(128),
       @TABLE_NAME VARCHAR(128)
)
AS
IF @DWU IS NULL
BEGIN
-- Selecting proper DWU for the current DB if not specified.
SET @DWU = (
  SELECT 'DW'+CAST(COUNT(*)*100 AS VARCHAR(10))
  FROM sys.dm_pdw_nodes
  WHERE type = 'COMPUTE')
END

DECLARE @DWU_NUM INT
SET @DWU_NUM = CAST (SUBSTRING(@DWU, 3, LEN(@DWU)-2) AS INT)

-- Raise error if either schema name or table name is supplied but not both them supplied
--IF ((@SCHEMA_NAME IS NOT NULL AND @TABLE_NAME IS NULL) OR (@TABLE_NAME IS NULL AND @SCHEMA_NAME IS NOT NULL))
--     RAISEERROR('User need to supply either both Schema Name and Table Name or none of them')
       
-- Dropping temp table if exists.
IF OBJECT_ID('tempdb..#ref') IS NOT NULL
BEGIN
  DROP TABLE #ref;
END

-- Creating ref. temptable (CTAS) to hold mapping info.
-- CREATE TABLE #ref
CREATE TABLE #ref
WITH (DISTRIBUTION = ROUND_ROBIN)
AS 
WITH
-- Creating concurrency slots mapping for various DWUs.
alloc
AS
(
  SELECT 'DW100' AS DWU, 4 AS max_queries, 4 AS max_slots, 1 AS slots_used_smallrc, 1 AS slots_used_mediumrc,
        2 AS slots_used_largerc, 4 AS slots_used_xlargerc, 1 AS slots_used_staticrc10, 2 AS slots_used_staticrc20,
        4 AS slots_used_staticrc30, 4 AS slots_used_staticrc40, 4 AS slots_used_staticrc50,
        4 AS slots_used_staticrc60, 4 AS slots_used_staticrc70, 4 AS slots_used_staticrc80
  UNION ALL
    SELECT 'DW200', 8, 8, 1, 2, 4, 8, 1, 2, 4, 8, 8, 8, 8, 8
  UNION ALL
    SELECT 'DW300', 12, 12, 1, 2, 4, 8, 1, 2, 4, 8, 8, 8, 8, 8
  UNION ALL
    SELECT 'DW400', 16, 16, 1, 4, 8, 16, 1, 2, 4, 8, 16, 16, 16, 16
  UNION ALL
     SELECT 'DW500', 20, 20, 1, 4, 8, 16, 1, 2, 4, 8, 16, 16, 16, 16
  UNION ALL
    SELECT 'DW600', 24, 24, 1, 4, 8, 16, 1, 2, 4, 8, 16, 16, 16, 16
  UNION ALL
    SELECT 'DW1000', 32, 40, 1, 8, 16, 32, 1, 2, 4, 8, 16, 32, 32, 32
  UNION ALL
    SELECT 'DW1200', 32, 48, 1, 8, 16, 32, 1, 2, 4, 8, 16, 32, 32, 32
  UNION ALL
    SELECT 'DW1500', 32, 60, 1, 8, 16, 32, 1, 2, 4, 8, 16, 32, 32, 32
  UNION ALL
    SELECT 'DW2000', 32, 80, 1, 16, 32, 64, 1, 2, 4, 8, 16, 32, 64, 64
  UNION ALL
   SELECT 'DW3000', 32, 120, 1, 16, 32, 64, 1, 2, 4, 8, 16, 32, 64, 64
  UNION ALL
    SELECT 'DW6000', 32, 240, 1, 32, 64, 128, 1, 2, 4, 8, 16, 32, 64, 128
)
-- Creating workload mapping to their corresponding slot consumption and default memory grant.
,map
AS
(
  SELECT 'SloDWGroupC00' AS wg_name,1 AS slots_used,100 AS tgt_mem_grant_MB
  UNION ALL
    SELECT 'SloDWGroupC01',2,200
  UNION ALL
    SELECT 'SloDWGroupC02',4,400
  UNION ALL
    SELECT 'SloDWGroupC03',8,800
  UNION ALL
    SELECT 'SloDWGroupC04',16,1600
  UNION ALL
    SELECT 'SloDWGroupC05',32,3200
  UNION ALL
    SELECT 'SloDWGroupC06',64,6400
  UNION ALL
    SELECT 'SloDWGroupC07',128,12800
)
-- Creating ref based on current / asked DWU.
, ref
AS
(
  SELECT  a1.*
  ,       m1.wg_name          AS wg_name_smallrc
  ,       m1.tgt_mem_grant_MB AS tgt_mem_grant_MB_smallrc
  ,       m2.wg_name          AS wg_name_mediumrc
  ,       m2.tgt_mem_grant_MB AS tgt_mem_grant_MB_mediumrc
  ,       m3.wg_name          AS wg_name_largerc
  ,       m3.tgt_mem_grant_MB AS tgt_mem_grant_MB_largerc
  ,       m4.wg_name          AS wg_name_xlargerc
  ,       m4.tgt_mem_grant_MB AS tgt_mem_grant_MB_xlargerc
  ,       m5.wg_name          AS wg_name_staticrc10
  ,       m5.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc10
  ,       m6.wg_name          AS wg_name_staticrc20
  ,       m6.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc20
  ,       m7.wg_name          AS wg_name_staticrc30
  ,       m7.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc30
  ,       m8.wg_name          AS wg_name_staticrc40
  ,       m8.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc40
  ,       m9.wg_name          AS wg_name_staticrc50
  ,       m9.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc50
  ,       m10.wg_name          AS wg_name_staticrc60
  ,       m10.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc60
  ,       m11.wg_name          AS wg_name_staticrc70
  ,       m11.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc70
  ,       m12.wg_name          AS wg_name_staticrc80
  ,       m12.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc80
  FROM alloc a1
  JOIN map   m1  ON a1.slots_used_smallrc     = m1.slots_used
  JOIN map   m2  ON a1.slots_used_mediumrc    = m2.slots_used
  JOIN map   m3  ON a1.slots_used_largerc     = m3.slots_used
  JOIN map   m4  ON a1.slots_used_xlargerc    = m4.slots_used
  JOIN map   m5  ON a1.slots_used_staticrc10    = m5.slots_used
  JOIN map   m6  ON a1.slots_used_staticrc20    = m6.slots_used
  JOIN map   m7  ON a1.slots_used_staticrc30    = m7.slots_used
  JOIN map   m8  ON a1.slots_used_staticrc40    = m8.slots_used
  JOIN map   m9  ON a1.slots_used_staticrc50    = m9.slots_used
  JOIN map   m10  ON a1.slots_used_staticrc60    = m10.slots_used
  JOIN map   m11  ON a1.slots_used_staticrc70    = m11.slots_used
  JOIN map   m12  ON a1.slots_used_staticrc80    = m12.slots_used
-- WHERE   a1.DWU = @DWU
  WHERE   a1.DWU = UPPER(@DWU)
)
SELECT  DWU
,       max_queries
,       max_slots
,       slots_used
,       wg_name
,       tgt_mem_grant_MB
,       up1 as rc
,       (ROW_NUMBER() OVER(PARTITION BY DWU ORDER BY DWU)) as rc_id
FROM
(
    SELECT  DWU
    ,       max_queries
    ,       max_slots
    ,       slots_used
    ,       wg_name
    ,       tgt_mem_grant_MB
    ,       REVERSE(SUBSTRING(REVERSE(wg_names),1,CHARINDEX('_',REVERSE(wg_names),1)-1)) as up1
    ,       REVERSE(SUBSTRING(REVERSE(tgt_mem_grant_MBs),1,CHARINDEX('_',REVERSE(tgt_mem_grant_MBs),1)-1)) as up2
    ,       REVERSE(SUBSTRING(REVERSE(slots_used_all),1,CHARINDEX('_',REVERSE(slots_used_all),1)-1)) as up3
    FROM    ref AS r1
    UNPIVOT
    (
        wg_name FOR wg_names IN (wg_name_smallrc,wg_name_mediumrc,wg_name_largerc,wg_name_xlargerc,
        wg_name_staticrc10, wg_name_staticrc20, wg_name_staticrc30, wg_name_staticrc40, wg_name_staticrc50,
        wg_name_staticrc60, wg_name_staticrc70, wg_name_staticrc80)
    ) AS r2
    UNPIVOT
    (
        tgt_mem_grant_MB FOR tgt_mem_grant_MBs IN (tgt_mem_grant_MB_smallrc,tgt_mem_grant_MB_mediumrc,
        tgt_mem_grant_MB_largerc,tgt_mem_grant_MB_xlargerc, tgt_mem_grant_MB_staticrc10, tgt_mem_grant_MB_staticrc20,
        tgt_mem_grant_MB_staticrc30, tgt_mem_grant_MB_staticrc40, tgt_mem_grant_MB_staticrc50,
        tgt_mem_grant_MB_staticrc60, tgt_mem_grant_MB_staticrc70, tgt_mem_grant_MB_staticrc80)
    ) AS r3
    UNPIVOT
    (
        slots_used FOR slots_used_all IN (slots_used_smallrc,slots_used_mediumrc,slots_used_largerc,
        slots_used_xlargerc, slots_used_staticrc10, slots_used_staticrc20, slots_used_staticrc30,
        slots_used_staticrc40, slots_used_staticrc50, slots_used_staticrc60, slots_used_staticrc70,
        slots_used_staticrc80)
    ) AS r4
) a
WHERE   up1 = up2
AND     up1 = up3
;
-- Getting current info about workload groups.
WITH  
dmv  
AS  
(
  SELECT
          rp.name                                           AS rp_name
  ,       rp.max_memory_kb*1.0/1048576                      AS rp_max_mem_GB
  ,       (rp.max_memory_kb*1.0/1024)
          *(request_max_memory_grant_percent/100)           AS max_memory_grant_MB
  ,       (rp.max_memory_kb*1.0/1048576)
          *(request_max_memory_grant_percent/100)           AS max_memory_grant_GB
  ,       wg.name                                           AS wg_name
  ,       wg.importance                                     AS importance
  ,       wg.request_max_memory_grant_percent               AS request_max_memory_grant_percent
  FROM    sys.dm_pdw_nodes_resource_governor_workload_groups wg
  JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools rp    ON  wg.pdw_node_id  = rp.pdw_node_id
                                                                  AND wg.pool_id      = rp.pool_id
  WHERE   rp.name = 'SloDWPool'
  GROUP BY
          rp.name
  ,       rp.max_memory_kb
  ,       wg.name
  ,       wg.importance
  ,       wg.request_max_memory_grant_percent
)
-- Creating resource class name mapping.
,names
AS
(
  SELECT 'smallrc' as resource_class, 1 as rc_id
  UNION ALL
    SELECT 'mediumrc', 2
  UNION ALL
    SELECT 'largerc', 3
  UNION ALL
    SELECT 'xlargerc', 4
  UNION ALL
    SELECT 'staticrc10', 5
  UNION ALL
    SELECT 'staticrc20', 6
  UNION ALL
    SELECT 'staticrc30', 7
  UNION ALL
    SELECT 'staticrc40', 8
  UNION ALL
    SELECT 'staticrc50', 9
  UNION ALL
    SELECT 'staticrc60', 10
  UNION ALL
    SELECT 'staticrc70', 11
  UNION ALL
    SELECT 'staticrc80', 12
)
,base AS
(   SELECT  schema_name
    ,       table_name
    ,       SUM(column_count)                   AS column_count
    ,       ISNULL(SUM(short_string_column_count),0)   AS short_string_column_count
    ,       ISNULL(SUM(long_string_column_count),0)    AS long_string_column_count
    FROM    (   SELECT  sm.name                                             AS schema_name
                ,       tb.name                                             AS table_name
                ,       COUNT(co.column_id)                                 AS column_count
                           ,       CASE    WHEN co.system_type_id IN (36,43,106,108,165,167,173,175,231,239) 
                                AND  co.max_length <= 32 
                                THEN COUNT(co.column_id) 
                        END                                                 AS short_string_column_count
                ,       CASE    WHEN co.system_type_id IN (165,167,173,175,231,239) 
                                AND  co.max_length > 32 and co.max_length <=8000
                                THEN COUNT(co.column_id) 
                        END                                                 AS long_string_column_count
                FROM    sys.schemas AS sm
                JOIN    sys.tables  AS tb   on sm.[schema_id] = tb.[schema_id]
                JOIN    sys.columns AS co   ON tb.[object_id] = co.[object_id]
                           WHERE tb.name = @TABLE_NAME AND sm.name = @SCHEMA_NAME
                GROUP BY sm.name
                ,        tb.name
                ,        co.system_type_id
                ,        co.max_length            ) a
GROUP BY schema_name
,        table_name
)
, size AS
(
SELECT  schema_name
,       table_name
,       75497472                                            AS table_overhead

,       column_count*1048576*8                              AS column_size
,       short_string_column_count*1048576*32                       AS short_string_size,       (long_string_column_count*16777216) AS long_string_size
FROM    base
UNION
SELECT CASE WHEN COUNT(*) = 0 THEN 'EMPTY' END as schema_name
         ,CASE WHEN COUNT(*) = 0 THEN 'EMPTY' END as table_name
         ,CASE WHEN COUNT(*) = 0 THEN 0 END as table_overhead
         ,CASE WHEN COUNT(*) = 0 THEN 0 END as column_size
         ,CASE WHEN COUNT(*) = 0 THEN 0 END as short_string_size

,CASE WHEN COUNT(*) = 0 THEN 0 END as long_string_size
FROM   base
)
, load_multiplier as 
(
SELECT  CASE 
                     WHEN FLOOR(8 * (CAST (@DWU_NUM AS FLOAT)/6000)) > 0 THEN FLOOR(8 * (CAST (@DWU_NUM AS FLOAT)/6000)) 
                     ELSE 1 
              END AS multipliplication_factor
) 
       SELECT  r1.DWU
       , schema_name
       , table_name
       , rc.resource_class as closest_rc_in_increasing_order
       , max_queries_at_this_rc = CASE
             WHEN (r1.max_slots / r1.slots_used > r1.max_queries)
                  THEN r1.max_queries
             ELSE r1.max_slots / r1.slots_used
                  END
       , r1.max_slots as max_concurrency_slots
       , r1.slots_used as required_slots_for_the_rc
       , r1.tgt_mem_grant_MB  as rc_mem_grant_MB
       , CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multipliplication_factor/1048576    AS DECIMAL(18,2)) AS est_mem_grant_required_for_cci_operation_MB       
       FROM    size, load_multiplier, #ref r1, names  rc
       WHERE r1.rc_id=rc.rc_id
                     AND CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multipliplication_factor/1048576    AS DECIMAL(18,2)) < r1.tgt_mem_grant_MB
       ORDER BY ABS(CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multipliplication_factor/1048576    AS DECIMAL(18,2)) - r1.tgt_mem_grant_MB)
GO
```

## <a name="query-importance"></a>Query urgentie
SQL Data Warehouse implementeert resource klassen met behulp van werkbelastinggroepen. Er zijn een totaal van acht werkbelastinggroepen die het gedrag van de resource-klassen over de verschillende grootten van DWU controleren. Voor elke DWU SQL Data Warehouse maakt gebruik van vier van de werkbelastinggroepen acht. Dit is wel zinvol omdat elke werkbelastinggroep is toegewezen aan een van de vier resource klassen: smallrc, mediumrc, largerc, of xlargerc. Het belang van het begrijpen van de werkbelastinggroepen is dat sommige van deze werkbelastinggroepen zijn ingesteld tot hogere *belang*. Urgentie wordt gebruikt voor CPU planning. Query's uitvoeren met een hoge urgentie krijgt drie keer meer CPU-cycli dan die met een gemiddeld urgentie. Daarom bepalen gelijktijdigheid sleuf toewijzingen ook voor CPU-prioriteit. Als een query 16 of meer sleuven verbruikt, voert deze als hoge urgentie.

De volgende tabel toont de toewijzingen belang voor elke werkbelastinggroep.

### <a name="workload-group-mappings-to-concurrency-slots-and-importance"></a>Groepstoewijzingen werkbelasting gelijktijdigheid sleuven en urgentie
| Werkbelastinggroepen | Gelijktijdigheid sleuf toewijzing | MB / distributie | Toewijzing van belang |
|:--- |:---:|:---:|:--- |
| SloDWGroupC00 |1 |100 |Middelgroot |
| SloDWGroupC01 |2 |200 |Middelgroot |
| SloDWGroupC02 |4 |400 |Middelgroot |
| SloDWGroupC03 |8 |800 |Middelgroot |
| SloDWGroupC04 |16 |1,600 |Hoog |
| SloDWGroupC05 |32 |3,200 |Hoog |
| SloDWGroupC06 |64 |6,400 |Hoog |
| SloDWGroupC07 |128 |12,800 |Hoog |

Van de **toewijzing en het verbruik van gelijktijdigheid sleuven** grafiek, kunt u zien dat een DW500 1, 4, 8 of 16 gelijktijdigheid sleuven voor smallrc, mediumrc largerc en xlargerc, respectievelijk gebruikt. U kunt deze waarden opzoeken in het voorgaande diagram het belang voor elke objectklasse bron vinden.

### <a name="dw500-mapping-of-resource-classes-to-importance"></a>Toewijzing van klassen op belang resource DW500
| Bronklasse | Werkbelastinggroep | Gelijktijdigheid sleuven gebruikt | MB / distributie | Urgentie |
|:--- |:--- |:---:|:---:|:--- |
| smallrc |SloDWGroupC00 |1 |100 |Middelgroot |
| mediumrc |SloDWGroupC02 |4 |400 |Middelgroot |
| largerc |SloDWGroupC03 |8 |800 |Middelgroot |
| xlargerc |SloDWGroupC04 |16 |1,600 |Hoog |
| staticrc10 |SloDWGroupC00 |1 |100 |Middelgroot |
| staticrc20 |SloDWGroupC01 |2 |200 |Middelgroot |
| staticrc30 |SloDWGroupC02 |4 |400 |Middelgroot |
| staticrc40 |SloDWGroupC03 |8 |800 |Middelgroot |
| staticrc50 |SloDWGroupC03 |16 |1,600 |Hoog |
| staticrc60 |SloDWGroupC03 |16 |1,600 |Hoog |
| staticrc70 |SloDWGroupC03 |16 |1,600 |Hoog |
| staticrc80 |SloDWGroupC03 |16 |1,600 |Hoog |

U kunt de volgende DMV-query om te kijken naar de verschillen in de geheugenbrontoewijzing in detail vanuit het perspectief van de resourceregeling of voor het analyseren van actieve en historische informatie over het gebruik van de werkbelastinggroepen bij het oplossen.

```sql
WITH rg
AS
(   SELECT  
     pn.name                        AS node_name
    ,pn.[type]                        AS node_type
    ,pn.pdw_node_id                    AS node_id
    ,rp.name                        AS pool_name
    ,rp.max_memory_kb*1.0/1024                AS pool_max_mem_MB
    ,wg.name                        AS group_name
    ,wg.importance                    AS group_importance
    ,wg.request_max_memory_grant_percent        AS group_request_max_memory_grant_pcnt
    ,wg.max_dop                        AS group_max_dop
    ,wg.effective_max_dop                AS group_effective_max_dop
    ,wg.total_request_count                AS group_total_request_count
    ,wg.total_queued_request_count            AS group_total_queued_request_count
    ,wg.active_request_count                AS group_active_request_count
    ,wg.queued_request_count                AS group_queued_request_count
    FROM    sys.dm_pdw_nodes_resource_governor_workload_groups wg
    JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools rp    
            ON  wg.pdw_node_id  = rp.pdw_node_id
            AND wg.pool_id      = rp.pool_id
    JOIN    sys.dm_pdw_nodes pn
            ON    wg.pdw_node_id    = pn.pdw_node_id
    WHERE   wg.name like 'SloDWGroup%'
        AND     rp.name = 'SloDWPool'
)
SELECT    pool_name
,        pool_max_mem_MB
,        group_name
,        group_importance
,        (pool_max_mem_MB/100)*group_request_max_memory_grant_pcnt AS max_memory_grant_MB
,        node_name
,        node_type
,       group_total_request_count
,       group_total_queued_request_count
,       group_active_request_count
,       group_queued_request_count
FROM    rg
ORDER BY
    node_name
,    group_request_max_memory_grant_pcnt
,    group_importance
;
```

## <a name="queries-that-honor-concurrency-limits"></a>Query's die worden geacht gelijktijdigheid limieten
De meeste query's worden geregeld door de resource-klassen. Deze query's moeten binnen zowel de gelijktijdige query's en gelijktijdigheid sleuf drempelwaarden passen. Een gebruiker kiezen niet, moeten worden uitgesloten van een query uit het model van de sleuf gelijktijdigheid van taken.

De volgende instructies geacht om vast, resource klassen:

* INSERT SELECT
* UPDATE
* VERWIJDEREN
* Selecteer (tijdens het opvragen van gebruikerstabellen)
* ALTER INDEX REBUILD
* ALTER INDEX REORGANIZE
* ALTER TABLE REBUILD
* INDEX MAKEN
* GECLUSTERDE COLUMNSTORE-INDEX MAKEN
* TABLE AS SELECT (CTAS) MAKEN
* Gegevens laden
* Data movement-bewerkingen die worden uitgevoerd door de Data Movement Service (DMS)

## <a name="query-exceptions-to-concurrency-limits"></a>Query-uitzonderingen gelijktijdigheid limieten
Sommige query's komen niet voldoen aan de resourceklasse waaraan de gebruiker is toegewezen. Deze uitzonderingen op de limieten gelijktijdigheid worden aangebracht als de geheugenbronnen die nodig zijn voor een bepaalde opdracht laag, vaak zijn omdat de opdracht een Metagegevensbewerking is. Het doel van deze uitzonderingen is om te voorkomen dat groter geheugentoewijzingen voor query's die wordt nooit nodig. In dergelijke gevallen worden de standaard kleine resourceklasse (smallrc) wordt altijd gebruikt ongeacht de werkelijke bronklasse toegewezen aan de gebruiker. Bijvoorbeeld: `CREATE LOGIN` altijd in smallrc wordt uitgevoerd. De resources die vereist zijn om te voldoen aan deze bewerking zijn zeer lage, dus het is niet verstandig om op te nemen van de query in het model van de sleuf gelijktijdigheid van taken.  Deze query's worden ook niet beperkt door de limiet van 32 gebruiker gelijktijdigheid, een onbeperkt aantal deze query's kunt uitvoeren om aan de sessielimiet van 1024-sessies.

De volgende instructies doen resource klassen niet van toepassing:

* MAKEN of DROP TABLE
* ALTER TABLE... SWITCH, GESPLITSTE of partitie samenvoegen
* ALTER INDEX UITSCHAKELEN
* INDEX VERWIJDEREN
* CREATE, UPDATE of DROP STATISTICS
* TABEL AFKAPPEN
* ALTER AUTORISATIE
* AANMELDING MAKEN
* CREATE, ALTER of DROP USER
* CREATE, ALTER of DROP PROCEDURE
* MAKEN of weergave verwijderen
* WAARDEN INVOEGEN
* Selecteer in het systeemweergaven en DMV 's
* UITLEGGEN
* DBCC

<!--
Removed as these two are not confirmed / supported under SQLDW
- CREATE REMOTE TABLE AS SELECT
- CREATE EXTERNAL TABLE AS SELECT
- REDISTRIBUTE
-->

##  <a name="changing-user-resource-class-example"></a>Een voorbeeld van een gebruiker resource klasse wijzigen
1. **Aanmelding maken:** verbinding maken met uw **master** database op de SQL-server die als host fungeert voor uw SQL Data Warehouse-database en voer de volgende opdrachten.
   
    ```sql
    CREATE LOGIN newperson WITH PASSWORD = 'mypassword';
    CREATE USER newperson for LOGIN newperson;
    ```
   
   > [!NOTE]
   > Dit is een goed idee om te maken van een gebruiker in de hoofddatabase voor gebruikers van Azure SQL Data Warehouse. Maken van een gebruiker in master kan een gebruiker aanmelden met behulp van hulpprogramma's zoals SSMS zonder op te geven van een databasenaam op.  Ook kunnen ze de Objectverkenner gebruiken om weer te geven van alle databases op een SQL server.  Zie voor meer informatie over het maken en beheren van gebruikers [beveiligen van een database in SQL Data Warehouse][Secure a database in SQL Data Warehouse].
   > 
   > 
2. **SQL Data Warehouse-gebruiker maken:** verbinding maken met de **SQL Data Warehouse** database en voer de volgende opdracht uit.
   
    ```sql
    CREATE USER newperson FOR LOGIN newperson;
    ```
3. **Machtigingen toekennen:** in het volgende voorbeeld verleent `CONTROL` op de **SQL Data Warehouse** database. `CONTROL`op de database is niveau het equivalent van db_owner in SQL Server.
   
    ```sql
    GRANT CONTROL ON DATABASE::MySQLDW to newperson;
    ```
4. **Bronklasse verhogen:** gebruik van de volgende query naar een gebruiker toevoegen aan een hogere beheerrol van de werkbelasting.
   
    ```sql
    EXEC sp_addrolemember 'largerc', 'newperson'
    ```
5. **Bronklasse verkleinen:** gebruik van de volgende query naar een gebruiker verwijderen uit een beheerrol werkbelasting.
   
    ```sql
    EXEC sp_droprolemember 'largerc', 'newperson';
    ```
   
   > [!NOTE]
   > Het is niet mogelijk een gebruiker verwijderen uit smallrc.
   > 
   > 

## <a name="queued-query-detection-and-other-dmvs"></a>In de wachtrij query detectie- en andere DMV 's
U kunt de `sys.dm_pdw_exec_requests` DMV voor het identificeren van query's die in een wachtrij gelijktijdigheid wachten. Query's te wachten op een site gelijktijdigheid van taken heeft een status van **onderbroken**.

```sql
SELECT      r.[request_id]                 AS Request_ID
        ,r.[status]                 AS Request_Status
        ,r.[submit_time]             AS Request_SubmitTime
        ,r.[start_time]                 AS Request_StartTime
        ,DATEDIFF(ms,[submit_time],[start_time]) AS Request_InitiateDuration_ms
        ,r.resource_class                         AS Request_resource_class
FROM    sys.dm_pdw_exec_requests r;
```

Werkbelasting management-rollen kunnen worden bekeken met `sys.database_principals`.

```sql
SELECT  ro.[name]           AS [db_role_name]
FROM    sys.database_principals ro
WHERE   ro.[type_desc]      = 'DATABASE_ROLE'
AND     ro.[is_fixed_role]  = 0;
```

De volgende query ziet u welke rol u elke gebruiker is toegewezen.

```sql
SELECT     r.name AS role_principal_name
        ,m.name AS member_principal_name
FROM    sys.database_role_members rm
JOIN    sys.database_principals AS r            ON rm.role_principal_id        = r.principal_id
JOIN    sys.database_principals AS m            ON rm.member_principal_id    = m.principal_id
WHERE    r.name IN ('mediumrc','largerc', 'xlargerc');
```

SQL Data Warehouse heeft de volgende typen wachten:

* **LocalQueriesConcurrencyResourceType**: query's die zich buiten het kader van de sleuf gelijktijdigheid van taken. Functies, zoals DMV-query's en system `SELECT @@VERSION` zijn voorbeelden van lokale query's.
* **UserConcurrencyResourceType**: query's die zich in het kader van de sleuf gelijktijdigheid van taken bevinden. Query's op tabellen van de eindgebruiker vertegenwoordigen voorbeelden die dit brontype wilt gebruiken.
* **DmsConcurrencyResourceType**: wacht ten gevolge van data movement-bewerkingen.
* **BackupConcurrencyResourceType**: deze wachttijd geeft aan dat een database worden back-up. De maximale waarde voor dit resourcetype is 1. Als meerdere back-ups op hetzelfde moment hebt aangevraagd, wordt de andere wachtrij.

De `sys.dm_pdw_waits` DMV kan worden gebruikt om te zien welke bronnen op een aanvraag wacht.

```sql
SELECT  w.[wait_id]
,       w.[session_id]
,       w.[type]            AS Wait_type
,       w.[object_type]
,       w.[object_name]
,       w.[request_id]
,       w.[request_time]
,       w.[acquire_time]
,       w.[state]
,       w.[priority]
,    SESSION_ID()            AS Current_session
,    s.[status]            AS Session_status
,    s.[login_name]
,    s.[query_count]
,    s.[client_id]
,    s.[sql_spid]
,    r.[command]            AS Request_command
,    r.[label]
,    r.[status]            AS Request_status
,    r.[submit_time]
,    r.[start_time]
,    r.[end_compile_time]
,    r.[end_time]
,    DATEDIFF(ms,r.[submit_time],r.[start_time])        AS Request_queue_time_ms
,    DATEDIFF(ms,r.[start_time],r.[end_compile_time])    AS Request_compile_time_ms
,    DATEDIFF(ms,r.[end_compile_time],r.[end_time])        AS Request_execution_time_ms
,    r.[total_elapsed_time]
FROM    sys.dm_pdw_waits w
JOIN    sys.dm_pdw_exec_sessions s  ON w.[session_id] = s.[session_id]
JOIN    sys.dm_pdw_exec_requests r  ON w.[request_id] = r.[request_id]
WHERE    w.[session_id] <> SESSION_ID();
```

De `sys.dm_pdw_resource_waits` DMV toont alleen de resource-wacht dat wordt gebruikt door een bepaalde query. Wachttijd resource wordt alleen de tijd die wachten op resources worden opgegeven, in plaats van de wachttijd signaal, is de tijd die nodig is voor de onderliggende SQL servers plannen van de query naar de CPU.

```sql
SELECT  [session_id]
,       [type]
,       [object_type]
,       [object_name]
,       [request_id]
,       [request_time]
,       [acquire_time]
,       DATEDIFF(ms,[request_time],[acquire_time])  AS acquire_duration_ms
,       [concurrency_slots_used]                    AS concurrency_slots_reserved
,       [resource_class]
,       [wait_id]                                   AS queue_position
FROM    sys.dm_pdw_resource_waits
WHERE    [session_id] <> SESSION_ID();
```

De `sys.dm_pdw_wait_stats` DMV kan worden gebruikt voor analyse van de historische trend van wacht.

```sql
SELECT    w.[pdw_node_id]
,        w.[wait_name]
,        w.[max_wait_time]
,        w.[request_count]
,        w.[signal_time]
,        w.[completed_count]
,        w.[wait_time]
FROM    sys.dm_pdw_wait_stats w;
```

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het beheren van databasegebruikers en beveiliging [beveiligen van een database in SQL Data Warehouse][Secure a database in SQL Data Warehouse]. Zie voor meer informatie over hoe groter resource klassen kan worden verbeterd geclusterde columnstore-index kwaliteit [opnieuw opbouwen van indexen voor het segment verbeteren].

<!--Image references-->

<!--Article references-->
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md
[opnieuw opbouwen van indexen voor het segment verbeteren]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md

<!--MSDN references-->
[Managing Databases and Logins in Azure SQL Database]:https://msdn.microsoft.com/library/azure/ee336235.aspx

<!--Other Web references-->
