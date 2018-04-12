---
title: Azure SQL Data Warehouse prestatielagen | Microsoft Docs
description: Inleiding tot elasticiteit en geoptimaliseerd voor compute prestatielagen beschikbaar in Azure SQL Data Warehouse.
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 11/10/2017
ms.author: jrj;barbkess
ms.openlocfilehash: 03881c12faed723999e97431e4a69fdeb6bfa10d
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="azure-sql-data-warehouse-performance-tiers-preview"></a>Azure SQL Data Warehouse prestatielagen (Preview)
SQL Data Warehouse biedt twee prestatielagen die zijn geoptimaliseerd voor analytische workloads. Dit artikel wordt uitgelegd van de concepten van prestatielagen bij het kiezen van de meest geschikte prestatielaag voor uw workload. 

## <a name="what-is-a-performance-tier"></a>Wat is een prestatielaag?
Een prestatielaag is een optie die de configuratie van uw datawarehouse bepaalt. Deze optie is een van de eerste keuzes die u bij het maken van een datawarehouse.  

> [!VIDEO https://channel9.msdn.com/Events/Connect/2017/T140/player]

- In de prestatielaag **Geoptimaliseerd voor elasticiteit** worden de reken- en opslaglaag in de architectuur gescheiden. Deze optie werkt uitstekend bij werkbelastingen die optimaal profiteren van de scheiding tussen berekening en opslag, door regelmatig te schalen ter ondersteuning van korte perioden van piekactiviteit. Deze rekenlaag heeft de laagste toegangspuntprijs en kan worden geschaald ter ondersteuning van het merendeel van de klantwerkbelastingen.

- De prestatielaag **Geoptimaliseerd voor berekenen** maakt gebruik van de nieuwste Azure-hardware om een nieuwe NVMe Solid State Disk-cache te introduceren waarin de meestgebruikte gegevens dicht bij de CPU's worden opgeslagen, precies waar u deze wilt hebben. Doordat de opslag automatisch in lagen wordt verdeeld, is deze prestatielaag uitstekend geschikt voor complexe query's, omdat alle IO lokaal wordt opgeslagen in de rekenlaag. Bovendien wordt de kolomopslag uitgebreid voor het opslaan van een onbeperkte hoeveelheid gegevens in uw SQL-datawarehouse. De prestatielaag Geoptimaliseerd voor berekenen biedt het hoogste schaalbaarheidsniveau, zodat u maximaal 30.000 cDWU's (compute Data Warehouse Units) kunt schalen. Kies deze laag voor werkbelastingen waarvoor continue, zeer snelle prestaties zijn vereist.

## <a name="service-levels"></a>Serviceniveaus
De Service Level Objective (SLO) is de instelling voor schaalbaarheid die de kosten en prestaties van uw datawarehouse bepaalt. Het niveau van de service voor het optimaliseren voor Compute prestaties laag schaal worden gemeten in compute datawarehouse-eenheden (cDWU), bijvoorbeeld DW2000c. De geoptimaliseerde voor serviceniveaus elasticiteit worden gemeten in dwu's, bijvoorbeeld dw2000 zijn. Zie voor meer informatie [wat is er een datawarehouse-eenheid?](what-is-a-data-warehouse-unit-dwu-cdwu.md)

De instelling SERVICE_OBJECTIVE bepaalt het serviceniveau en de prestatielaag voor uw datawarehouse op T-SQL.

```sql
--Optimized for Elasticity
CREATE DATABASE myElasticSQLDW
WITH
(    SERVICE_OBJECTIVE = 'DW1000'
)
;

--Optimized for Compute
CREATE DATABASE myComputeSQLDW
WITH
(    SERVICE_OBJECTIVE = 'DW1000c'
)
;
```

## <a name="memory-maximums"></a>Maximumwaarden geheugen
De prestatielagen hebben verschillende geheugen profielen omgezet in een andere hoeveelheid geheugen per query. De optimaliseren voor de prestatielaag Compute biedt 2,5 x meer geheugen per query dan de optimaliseren voor de prestatielaag elasticiteit. Deze extra geheugen helpt de optimaliseren voor de prestatielaag Compute zijn razendsnel snelle prestaties leveren. Extra geheugen per query ook kunt u meer query's uitvoeren gelijktijdig omdat query's kunnen gebruiken lagere [resource klassen](resource-classes-for-workload-management.md). 

### <a name="optimized-for-elasticity"></a>Geoptimaliseerd voor flexibiliteit

Het niveau van de service voor het optimaliseren voor elasticiteit prestaties laag tussen DW100 en DW6000. 

| Serviceniveau | Maximum aantal gelijktijdige query 's | Rekenknooppunten | Distributies per rekenknooppunt | Maximaal geheugen per distributiepunt (MB) | Maximaal geheugen per datawarehouse (GB) |
|:-------------:|:----------------------:|:-------------:|:------------------------------:|:--------------------------------:|:----------------------------------:|
| DW100         | 4                      | 1             | 60                             | 400                              |  24                                |
| DW200         | 8                      | 2             | 30                             | 800                              |  48                                |
| DW300         | 12                     | 3             | 20                             | 1,200                            |  72                                |
| DW400         | 16                     | 4             | 15                             | 1,600                            |  96                                |
| DW500         | 20                     | 5             | 12                             | 2,000                            | 120                                |
| DW600         | 24                     | 6             | 10                             | 2,400                            | 144                                |
| DW1000        | 32                     | 10            | 6                              | 4,000                            | 240                                |
| DW1200        | 32                     | 12            | 5                              | 4,800                            | 288                                |
| DW1500        | 32                     | 15            | 4                              | 6,000                            | 360                                |
| DW2000 ZIJN        | 32                     | 20            | 3                              | 8,000                            | 480                                |
| DW3000        | 32                     | 30            | 2                              | 12,000                           | 720                                |
| DW6000        | 32                     | 60            | 1                              | 24,000                           | 1440                               |

### <a name="optimized-for-compute"></a>Geoptimaliseerd voor rekenen

Het niveau van de service voor het optimaliseren voor Compute prestaties laag bereik van DW1000c tot DW30000c. 

| Serviceniveau | Maximum aantal gelijktijdige query 's | Rekenknooppunten | Distributies per rekenknooppunt | Maximaal geheugen per distributiepunt (GB) | Maximaal geheugen per datawarehouse (GB) |
|:-------------:|:----------------------:|:-------------:|:------------------------------:|:--------------------------------:|:----------------------------------:|
| DW1000c       | 32                     | 2             | 30                             |  10                              |   600                              |
| DW1500c       | 32                     | 3             | 20                             |  15                              |   900                              |
| DW2000c       | 32                     | 4             | 15                             |  20                              |  1200                              |
| DW2500c       | 32                     | 5             | 12                             |  25                              |  1500                              |
| DW3000c       | 32                     | 6             | 10                             |  30                              |  1800                              |
| DW5000c       | 32                     | 10            | 6                              |  50                              |  3000                              |
| DW6000c       | 32                     | 12            | 5                              |  60                              |  3600                              |
| DW7500c       | 32                     | 15            | 4                              |  75                              |  4500                              |
| DW10000c      | 32                     | 20            | 3                              | 100                              |  6000                              |
| DW15000c      | 32                     | 30            | 2                              | 150                              |  9000                              |
| DW30000c      | 32                     | 60            | 1                              | 300                              | 18000                              |

De maximale cDWU is DW30000c met 60 rekenknooppunten en één distributiepunt per rekenknooppunt. Een datawarehouse 600 TB op DW30000c verwerkt bijvoorbeeld ongeveer 10 TB per rekenknooppunt.

## <a name="concurrency-maximums"></a>Maximumwaarden gelijktijdigheid van taken
SQL Data Warehouse biedt toonaangevende gelijktijdigheid van taken op een enkele datawarehouse. Zodat elke query bevat onvoldoende bronnen om uit te voeren efficiënt, het systeem berekenen houdt Resourcegebruik gelijktijdigheid sleuven toewijzen aan elke query. Het systeem worden query's in een wachtrij waar ze wacht totdat er onvoldoende sleuven gelijktijdigheid van taken beschikbaar zijn. 

Gelijktijdigheid sleuven ook bepalen CPU prioriteitsaanduiding. Zie voor meer informatie [analyseren van uw werkbelasting](analyze-your-workload.md)

### <a name="concurrency-slots"></a>Gelijktijdigheid sleuven
Er zijn een handige manier om bij te houden van de beschikbare resources voor uitvoering van de query voor gelijktijdigheid sleuven. Ze zijn vergelijkbaar met tickets die u aanschaft om te plaatsen op een overleg reserveren, omdat zitplaatsen beperkt is. SQL Data Warehouse heeft ook een beperkt aantal rekenresources. Query's reserveren compute-bronnen te verkrijgen gelijktijdigheid sleuven. Voordat een query kunt uitvoeren, moet het mogelijk om toegang te reserveren onvoldoende sleuven gelijktijdigheid van taken. Wanneer een query is voltooid, worden de sleuven gelijktijdigheid vrijgegeven. 

* De geoptimaliseerde voor de prestatielaag elasticiteit wordt aangepast aan 240 gelijktijdigheid sleuven.
* De geoptimaliseerde voor compute prestaties laag schalen naar 1200 gelijktijdigheid sleuven.

Elke query verbruiken nul, een of meer gelijktijdigheid sleuven. Verbruikt sleuven geen System-query's en enkele trivial query's. Query's die worden geregeld door resource klassen vereisen standaard één gelijktijdigheid sleuf. Complexere query's kunnen extra gelijktijdigheid sleuven vereisen.  

- Een query uitgevoerd met 10 gelijktijdigheid sleuven hebben toegang tot 5 keer meer computerresources dan een query uitgevoerd met 2 gelijktijdigheid sleuven.
- Als u elke query 10 gelijktijdigheid sleuven vereist en er zijn 40 gelijktijdigheid sleuven, kunnen gelijktijdig alleen 4 query's uitvoeren.
 
Alleen resource geregeld query verbruiken gelijktijdigheid sleuven. Het exacte aantal gelijktijdigheid sleuven verbruikt wordt bepaald door de query [bronklasse](resource-classes-for-workload-management.md).

### <a name="optimized-for-compute"></a>Geoptimaliseerd voor rekenen
De volgende tabel ziet u het maximum aantal gelijktijdige query's en gelijktijdigheid sleuven voor elk [dynamische bronklasse](resource-classes-for-workload-management.md).  Deze gelden voor het optimaliseren voor de prestatielaag Compute.

**Dynamische Bronklassen**
| Servicelaag | Maximum aantal gelijktijdige query 's | Gelijktijdigheid sleuven beschikbaar | Gebruikt door smallrc sleuven | Gebruikt door mediumrc sleuven | Gebruikt door largerc sleuven | Gebruikt door xlargerc sleuven |
|:-------------:|:--------------------------:|:---------------------------:|:---------------------:|:----------------------:|:---------------------:|:----------------------:|
| DW1000c       | 32                         |   40                        | 1                     |  8                     |  16                   |  32                    |
| DW1500c       | 32                         |   60                        | 1                     |  8                     |  16                   |  32                    |
| DW2000c       | 32                         |   80                        | 1                     | 16                     |  32                   |  64                    |
| DW2500c       | 32                         |  100                        | 1                     | 16                     |  32                   |  64                    |
| DW3000c       | 32                         |  120                        | 1                     | 16                     |  32                   |  64                    |
| DW5000c       | 32                         |  200                        | 1                     | 32                     |  64                   | 128                    |
| DW6000c       | 32                         |  240                        | 1                     | 32                     |  64                   | 128                    |
| DW7500c       | 32                         |  300                        | 1                     | 64                     | 128                   | 128                    |
| DW10000c      | 32                         |  400                        | 1                     | 64                     | 128                   | 256                    |
| DW15000c      | 32                         |  600                        | 1                     | 64                     | 128                   | 256                    |
| DW30000c      | 32                         | 1200                        | 1                     | 64                     | 128                   | 256                    |

**Statische resource klassen**

De volgende tabel ziet u het maximum aantal gelijktijdige query's en gelijktijdigheid sleuven voor elk [statische bronklasse](resource-classes-for-workload-management.md).  

| Servicelaag | Maximum aantal gelijktijdige query 's | Gelijktijdigheid sleuven beschikbaar |staticrc10 | staticrc20 | staticrc30 | staticrc40 | staticrc50 | staticrc60 | staticrc70 | staticrc80 |
|:-------------:|:--------------------------:|:---------------------------:|:---------:|:----------:|:----------:|:----------:|:----------:|:----------:|:----------:|:----------:|
| DW1000c       | 32                         |   40                        | 1         | 2          | 4          | 8          | 16         | 32         | 32         |  32        |
| DW1500c       | 32                         |   60                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         |  64        |
| DW2000c       | 32                         |   80                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         |  64        |
| DW2500c       | 32                         |  100                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         |  64        |
| DW3000c       | 32                         |  120                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW5000c       | 32                         |  200                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW6000c       | 32                         |  240                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW7500c       | 32                         |  300                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW10000c      | 32                         |  400                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW15000c      | 32                         |  600                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW30000c      | 32                         | 1200                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |

### <a name="optimized-for-elasticity"></a>Geoptimaliseerd voor flexibiliteit
De volgende tabel ziet u het maximum aantal gelijktijdige query's en gelijktijdigheid sleuven voor elk [dynamische bronklasse](resource-classes-for-workload-management.md).  Deze gelden voor het optimaliseren voor de prestatielaag elasticiteit.

**Dynamische Bronklassen**

| Serviceniveau | Maximum aantal gelijktijdige query 's | Gelijktijdigheid sleuven beschikbaar | smallrc | mediumrc | largerc | xlargerc |
|:-------------:|:--------------------------:|:---------------------------:|:-------:|:--------:|:-------:|:--------:|
| DW100         |  4                         |   4                         | 1       |  1       |  2      |   4      |
| DW200         |  8                         |   8                         | 1       |  2       |  4      |   8      |
| DW300         | 12                         |  12                         | 1       |  2       |  4      |   8      |
| DW400         | 16                         |  16                         | 1       |  4       |  8      |  16      |
| DW500         | 20                         |  20                         | 1       |  4       |  8      |  16      |
| DW600         | 24                         |  24                         | 1       |  4       |  8      |  16      |
| DW1000        | 32                         |  40                         | 1       |  8       | 16      |  32      |
| DW1200        | 32                         |  48                         | 1       |  8       | 16      |  32      |
| DW1500        | 32                         |  60                         | 1       |  8       | 16      |  32      |
| DW2000 ZIJN        | 32                         |  80                         | 1       | 16       | 32      |  64      |
| DW3000        | 32                         | 120                         | 1       | 16       | 32      |  64      |
| DW6000        | 32                         | 240                         | 1       | 32       | 64      | 128      |

**Statische resource klassen** in de volgende tabel ziet u de maximum aantal gelijktijdige query's en gelijktijdigheid sleuven voor elk [statische bronklasse](resource-classes-for-workload-management.md).  Deze gelden voor het optimaliseren voor de prestatielaag elasticiteit.

| Serviceniveau | Maximum aantal gelijktijdige query 's | Maximale gelijktijdigheid sleuven |staticrc10 | staticrc20 | staticrc30 | staticrc40 | staticrc50 | staticrc60 | staticrc70 | staticrc80 |
|:-------------:|:--------------------------:|:-------------------------:|:---------:|:----------:|:----------:|:----------:|:----------:|:----------:|:----------:|:----------:|
| DW100         | 4                          |   4                       | 1         | 2          | 4          | 4          |  4         |  4         |  4         |   4        |
| DW200         | 8                          |   8                       | 1         | 2          | 4          | 8          |  8         |  8         |  8         |   8        |
| DW300         | 12                         |  12                       | 1         | 2          | 4          | 8          |  8         |  8         |  8         |   8        |
| DW400         | 16                         |  16                       | 1         | 2          | 4          | 8          | 16         | 16         | 16         |  16        |
| DW500         | 20                         |  20                       | 1         | 2          | 4          | 8          | 16         | 16         | 16         |  16        |
| DW600         | 24                         |  24                       | 1         | 2          | 4          | 8          | 16         | 16         | 16         |  16        |
| DW1000        | 32                         |  40                       | 1         | 2          | 4          | 8          | 16         | 32         | 32         |  32        |
| DW1200        | 32                         |  48                       | 1         | 2          | 4          | 8          | 16         | 32         | 32         |  32        |
| DW1500        | 32                         |  60                       | 1         | 2          | 4          | 8          | 16         | 32         | 32         |  32        |
| DW2000 ZIJN        | 32                         |  80                       | 1         | 2          | 4          | 8          | 16         | 32         | 64         |  64        |
| DW3000        | 32                         | 120                       | 1         | 2          | 4          | 8          | 16         | 32         | 64         |  64        |
| DW6000        | 32                         | 240                       | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |

Als een van deze drempels wordt voldaan, worden de nieuwe query's in de wachtrij en de eerste in, First-out ' op basis van een uitgevoerd.  Als een query's is voltooid en het aantal query's en sleuven onder de limieten valt, versies SQL Data Warehouse in de wachtrij query's. 

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over hoe u resource klassen voor het optimaliseren van uw werkbelasting verdere Lees de volgende artikelen:
* [Resource-klassen voor het beheer van de werkbelasting](resource-classes-for-workload-management.md)
* [Uw werkbelasting analyseren](analyze-your-workload.md)

