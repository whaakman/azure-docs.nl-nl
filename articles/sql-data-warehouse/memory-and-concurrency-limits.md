---
title: Geheugen en gelijktijdigheid limieten - Azure SQL Data Warehouse | Microsoft Docs
description: Het geheugen weergeven en gelijktijdigheid van limieten die zijn toegewezen aan de verschillende prestatieniveaus en resource klassen in Azure SQL Data Warehouse.
services: sql-data-warehouse
author: ronortloff
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 05/07/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 46d41e3ee85deb20f189bc9c82a255178f3d7eee
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="memory-and-concurrency-limits-for-azure-sql-data-warehouse"></a>Geheugen en gelijktijdigheid limieten voor Azure SQL Data Warehouse
Het geheugen weergeven en gelijktijdigheid van limieten die zijn toegewezen aan de verschillende prestatieniveaus en resource klassen in Azure SQL Data Warehouse. Zie voor meer informatie en deze mogelijkheden toepassen op uw plan voor werkbelasting [Resource klassen voor het beheer van de werkbelasting](resource-classes-for-workload-management.md). 

Er zijn momenteel twee generaties beschikbaar met SQL Data Warehouse – Gen1 en Gen2. U wordt aangeraden gebruikmaken van Gen2 van SQL Data Warehouse om de beste prestaties voor uw datawarehouse-workload. Gen2 introduceert een nieuwe cache NVMe effen status schijf de meest gebruikte gegevens dicht bij de CPU's houdt. Hiermee verwijdert u de externe i/o's voor uw meest intensieve en veeleisende werkbelastingen. Naast de prestaties biedt Gen2 het hoogste niveau van de schaal zodat u kunt maximaal 30.000 Data Warehouse Units schalen en onbeperkte kolommen opslag bieden. We nog steeds ondersteuning biedt voor de vorige generatie (Gen1) van SQL Data Warehouse en de functies dezelfde; behouden echter, raden we u aan [upgrade naar Gen2](upgrade-to-latest-generation.md) zo snel mogelijk. 

## <a name="data-warehouse-capacity-settings"></a>Capaciteitsinstellingen voor de datawarehouse
De volgende tabellen geven de maximale capaciteit voor het datawarehouse op verschillende prestatieniveaus. Zie het wijzigen van het prestatieniveau [Scale compute - portal](quickstart-scale-compute-portal.md).

### <a name="gen2"></a>Gen2

Gen2 biedt 2,5 x meer geheugen per query dan de Gen1. Deze extra geheugen helpt de Gen2 zijn snelle prestaties leveren.  De prestaties voor het bereik Gen2 van DW1000c tot DW30000c. 

| Prestatieniveau | Rekenknooppunten | Distributies per rekenknooppunt | Geheugen per datawarehouse (GB) |
|:-----------------:|:-------------:|:------------------------------:|:------------------------------:|
| DW1000c           | 2             | 30                             |   600                          |
| DW1500c           | 3             | 20                             |   900                          |
| DW2000c           | 4             | 15                             |  1200                          |
| DW2500c           | 5             | 12                             |  1500                          |
| DW3000c           | 6             | 10                             |  1800                          |
| DW5000c           | 10            | 6                              |  3000                          |
| DW6000c           | 12            | 5                              |  3600                          |
| DW7500c           | 15            | 4                              |  4500                          |
| DW10000c          | 20            | 3                              |  6000                          |
| DW15000c          | 30            | 2                              |  9000                          |
| DW30000c          | 60            | 1                              | 18000                          |

De maximale Gen2 DWU is DW30000c met 60 rekenknooppunten en één distributiepunt per rekenknooppunt. Een datawarehouse 600 TB op DW30000c verwerkt bijvoorbeeld ongeveer 10 TB per rekenknooppunt.

### <a name="gen1"></a>Gen1

Het niveau van de service voor Gen1 tussen DW100 en DW6000. 

| Prestatieniveau | Rekenknooppunten | Distributies per rekenknooppunt | Geheugen per datawarehouse (GB) |
|:-----------------:|:-------------:|:------------------------------:|:------------------------------:|
| DW100             | 1             | 60                             |  24                            |
| DW200             | 2             | 30                             |  48                            |
| DW300             | 3             | 20                             |  72                            |
| DW400             | 4             | 15                             |  96                            |
| DW500             | 5             | 12                             | 120                            |
| DW600             | 6             | 10                             | 144                            |
| DW1000            | 10            | 6                              | 240                            |
| DW1200            | 12            | 5                              | 288                            |
| DW1500            | 15            | 4                              | 360                            |
| DW2000            | 20            | 3                              | 480                            |
| DW3000            | 30            | 2                              | 720                            |
| DW6000            | 60            | 1                              | 1440                           |

## <a name="concurrency-maximums"></a>Maximumwaarden gelijktijdigheid van taken
Zodat elke query bevat onvoldoende bronnen om uit te voeren efficiënt houdt SQL Data Warehouse Resourcegebruik door gelijktijdigheid sleuven toewijzen aan elke query. Het systeem worden query's in een wachtrij waar ze pas voldoende [gelijktijdigheid sleuven](resource-classes-for-workload-management.md#concurrency-slots) beschikbaar zijn. Gelijktijdigheid sleuven ook bepalen CPU prioriteitsaanduiding. Zie voor meer informatie [analyseren van uw werkbelasting](analyze-your-workload.md)

### <a name="gen2"></a>Gen2
 
**Statische resource klassen**

De volgende tabel ziet u het maximum aantal gelijktijdige query's en gelijktijdigheid sleuven voor elk [statische bronklasse](resource-classes-for-workload-management.md).  

| Servicelaag | Maximum aantal gelijktijdige query 's | Gelijktijdigheid sleuven beschikbaar |staticrc10 | staticrc20 | staticrc30 | staticrc40 | staticrc50 | staticrc60 | staticrc70 | staticrc80 |
|:-------------:|:--------------------------:|:---------------------------:|:---------:|:----------:|:----------:|:----------:|:----------:|:----------:|:----------:|:----------:|
| DW1000c       | 32                         |   40                        | 1         | 2          | 4          | 8          | 16         | 32         | 32         |  32        |
| DW1500c       | 32                         |   60                        | 1         | 2          | 4          | 8          | 16         | 32         | 32         |  32        |
| DW2000c       | 48                         |   80                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         |  64        |
| DW2500c       | 48                         |  100                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         |  64        |
| DW3000c       | 64                         |  120                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW5000c       | 64                         |  200                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW6000c       | 128                        |  240                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW7500c       | 128                        |  300                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW10000c      | 128                        |  400                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW15000c      | 128                        |  600                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW30000c      | 128                        | 1200                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |

**Dynamische Bronklassen**

> [!NOTE]
> De bronklasse smallrc op Gen2 dynamisch geheugen wordt toegevoegd als het serviceniveau verhoogt en biedt alleen ondersteuning voor een maximale 32 gelijktijdige query's.  De sleuven gelijktijdigheid van taken en het geheugen gebruikt door smallrc toeneemt als de service wordt verhoogd. 
>
>

De volgende tabel ziet u het maximum aantal gelijktijdige query's en gelijktijdigheid sleuven voor elk [dynamische bronklasse](resource-classes-for-workload-management.md). In tegenstelling tot Gen1 zijn dynamische Bronklassen op Gen2 echt dynamisch.  Gen2 gebruikt een percentage van 3-10-22-70 geheugentoewijzing voor klein-medium-grote-xlarge resource klassen op alle serviceniveaus.

| Servicelaag | Maximum aantal gelijktijdige query 's | Gelijktijdigheid sleuven beschikbaar | Gebruikt door smallrc sleuven | Gebruikt door mediumrc sleuven | Gebruikt door largerc sleuven | Gebruikt door xlargerc sleuven |
|:-------------:|:--------------------------:|:---------------------------:|:---------------------:|:----------------------:|:---------------------:|:----------------------:|
| DW1000c       | 32                         |   40                        | 1                     |  4                     |  8                    |  28                    |
| DW1500c       | 32                         |   60                        | 1                     |  6                     |  13                   |  42                    |
| DW2000c       | 32                         |   80                        | 2                     |  8                     |  17                   |  56                    |
| DW2500c       | 32                         |  100                        | 3                     | 10                     |  22                   |  70                    |
| DW3000c       | 32                         |  120                        | 3                     | 12                     |  26                   |  84                    |
| DW5000c       | 32                         |  200                        | 6                     | 20                     |  44                   | 140                    |
| DW6000c       | 32                         |  240                        | 7                     | 24                     |  52                   | 168                    |
| DW7500c       | 32                         |  300                        | 9                     | 30                     |  66                   | 210                    |
| DW10000c      | 32                         |  400                        | 12                    | 40                     |  88                   | 280                    |
| DW15000c      | 32                         |  600                        | 18                    | 60                     | 132                   | 420                    |
| DW30000c      | 32                         | 1200                        | 36                    | 120                    | 264                   | 840                    |



#### <a name="gen1"></a>Gen1

Statische resource klassen

De volgende tabel ziet u het maximum aantal gelijktijdige query's en gelijktijdigheid sleuven voor elk [statische bronklasse](resource-classes-for-workload-management.md) op **Gen1**.

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
| DW2000        | 48                         |  80                       | 1         | 2          | 4          | 8          | 16         | 32         | 64         |  64        |
| DW3000        | 64                         | 120                       | 1         | 2          | 4          | 8          | 16         | 32         | 64         |  64        |
| DW6000        | 128                        | 240                       | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |

Dynamische Bronklassen
> [!NOTE]
> De bronklasse smallrc op Gen1 wijst een vaste hoeveelheid geheugen per query, lijkt op wijze op de klasse statische resource staticrc10.  Omdat smallrc statisch is, heeft de mogelijkheid om te worden uitgebreid naar 128 gelijktijdige query's. 
>
>

De volgende tabel ziet u het maximum aantal gelijktijdige query's en gelijktijdigheid sleuven voor elk [dynamische bronklasse](resource-classes-for-workload-management.md) op **Gen1**.

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
| DW2000        | 48                         |  80                         | 1       | 16       | 32      |  64      |
| DW3000        | 64                         | 120                         | 1       | 16       | 32      |  64      |
| DW6000        | 128                        | 240                         | 1       | 32       | 64      | 128      |


Als een van deze drempels wordt voldaan, worden de nieuwe query's in de wachtrij en de eerste in, First-out ' op basis van een uitgevoerd.  Als een query's is voltooid en het aantal query's en sleuven onder de limieten valt, versies SQL Data Warehouse in de wachtrij query's. 

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over hoe u resource klassen voor het optimaliseren van uw werkbelasting verdere Lees de volgende artikelen:
* [Resource-klassen voor het beheer van de werkbelasting](resource-classes-for-workload-management.md)
* [Uw werkbelasting analyseren](analyze-your-workload.md)

