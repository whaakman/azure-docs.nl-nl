---
title: Geheugen en gelijktijdigheid beperkingen - Azure SQL Data Warehouse | Microsoft Docs
description: Bekijk de limieten voor geheugen en gelijktijdigheid toegewezen aan de verschillende prestatieniveaus en resourceklassen in Azure SQL Data Warehouse.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 10/04/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: cc42a0289316116b843696c984f9fc3c2114eb6c
ms.sourcegitcommit: a8948ddcbaaa22bccbb6f187b20720eba7a17edc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56592890"
---
# <a name="memory-and-concurrency-limits-for-azure-sql-data-warehouse"></a>Limieten voor geheugen en gelijktijdigheid voor Azure SQL Data Warehouse
Bekijk de limieten voor geheugen en gelijktijdigheid toegewezen aan de verschillende prestatieniveaus en resourceklassen in Azure SQL Data Warehouse. Zie voor meer informatie en om toe te passen van deze mogelijkheden voor uw abonnement van de management workload [resourceklassen voor het beheer van de werkbelasting](resource-classes-for-workload-management.md). 

Er zijn momenteel twee generaties op verschillende manieren beschikbaar met SQL Data Warehouse: Gen1 en Gen2. U wordt aangeraden u gebruikmaken van Gen2 van SQL Data Warehouse voor de beste prestaties voor uw datawarehouse-workload. Gen2 introduceert een nieuwe NVMe Solid State Disk-cache, waardoor de meestgebruikte gegevens dicht bij de CPU's. Hiermee verwijdert u de externe i/o's voor uw meest intensieve en veeleisende workloads. Naast de prestaties biedt Gen2 het hoogste niveau van de schaal zodat u kunt maximaal 30.000 datawarehouse-eenheden schalen en het geven van de onbeperkte opslag in kolommen. We nog steeds ondersteuning biedt voor de vorige generatie (Gen1) van SQL Data Warehouse en de dezelfde functies; behouden maar we raden u aan [een upgrade uitvoeren naar Gen2](upgrade-to-latest-generation.md) zo snel mogelijk. 

## <a name="data-warehouse-capacity-settings"></a>Instellingen voor datawarehouse-capaciteit
De volgende tabellen ziet u de maximale capaciteit voor het datawarehouse op verschillende prestatieniveaus. Zie het wijzigen van het prestatieniveau [Computing kunt schalen - portal](quickstart-scale-compute-portal.md).

### <a name="gen2"></a>Gen2

Gen2 biedt 2,5 x meer geheugen per query dan de Gen1. Deze extra geheugen helpt bij het leveren van de snelle prestaties Gen2.  De prestaties voor het bereik Gen2 van DW100c tot DW30000c. 

| Prestatieniveau | Rekenknooppunten | Distributies per knooppunt | Geheugen per datawarehouse (GB) |
|:-----------------:|:-------------:|:------------------------------:|:------------------------------:|
| DW100c            | 1             | 60                             |    60                          |
| DW200c            | 1             | 60                             |   120                          |
| DW300c            | 1             | 60                             |   180                          |
| DW400c            | 1             | 60                             |   240                          |
| DW500c            | 1             | 60                             |   300                          |
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

De maximale Gen2 DWU is DW30000c met 60 rekenknooppunten en één distributiepunt per knooppunt. Een datawarehouse 600 TB op DW30000c verwerkt bijvoorbeeld ongeveer 10 TB per knooppunt.

### <a name="gen1"></a>Gen1

Het niveau van de service voor Gen1 tussen DW100 en DW6000. 

| Prestatieniveau | Rekenknooppunten | Distributies per knooppunt | Geheugen per datawarehouse (GB) |
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

## <a name="concurrency-maximums"></a>Maximumwaarden voor gelijktijdigheid
Om ervoor te zorgen voor dat elke query heeft onvoldoende bronnen voor het efficiënt uitvoeren, houdt SQL Data Warehouse gebruik van resources door gelijktijdigheidssleuven toewijzen aan elke query. Het systeem worden query's geplaatst in een wachtrij waar ze wachten tot en met voldoende [gelijktijdigheidssleuven](resource-classes-for-workload-management.md#concurrency-slots) beschikbaar zijn. Gelijktijdigheidssleuven bepalen ook CPU-prioriteit. Zie voor meer informatie, [uw werkbelasting analyseren](analyze-your-workload.md)

### <a name="gen2"></a>Gen2
 
**Statische resourceklassen**

De volgende tabel ziet u het maximum aantal gelijktijdige query's en gelijktijdigheidssleuven voor elk [statische resourceklasse](resource-classes-for-workload-management.md).  

| Servicelaag | Maximum aantal gelijktijdige query 's | Gelijktijdigheidssleuven beschikbaar | Sleuven die worden gebruikt door staticrc10 | Sleuven die worden gebruikt door staticrc20 | Sleuven die worden gebruikt door staticrc30 | Sleuven die worden gebruikt door staticrc40 | Sleuven die worden gebruikt door staticrc50 | Sleuven die worden gebruikt door staticrc60 | Sleuven die worden gebruikt door staticrc70 | Sleuven die worden gebruikt door staticrc80 |
|:-------------:|:--------------------------:|:---------------------------:|:---------:|:----------:|:----------:|:----------:|:----------:|:----------:|:----------:|:----------:|
| DW100c        |  4                         |    4                        | 1         | 2          | 4          | 4          | 4         |  4         |  4         |  4         |
| DW200c        |  8                         |    8                        | 1         | 2          | 4          | 8          |  8         |  8         |  8         |  8        |
| DW300c        | 12                         |   12                        | 1         | 2          | 4          | 8          |  8         |  8         |  8         |   8        |
| DW400c        | 16                         |   16                        | 1         | 2          | 4          | 8          | 16         | 16         | 16         |  16        |
| DW500c        | 20                         |   20                        | 1         | 2          | 4          | 8          | 16         | 16         | 16         |  16        |
| DW1000c       | 32                         |   40                        | 1         | 2          | 4          | 8          | 16         | 32         | 32         |  32        |
| DW1500c       | 32                         |   60                        | 1         | 2          | 4          | 8          | 16         | 32         | 32         |  32        |
| DW2000c       | 48                         |   80                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         |  64        |
| DW2500c       | 48                         |  100                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         |  64        |
| DW3000c       | 64                         |  120                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         |  64        |
| DW5000c       | 64                         |  200                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW6000c       | 128                        |  240                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW7500c       | 128                        |  300                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW10000c      | 128                        |  400                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW15000c      | 128                        |  600                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW30000c      | 128                        | 1200                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |

**Dynamische resourceklassen**

> [!NOTE]
> De resourceklasse smallrc in Gen2 dynamisch geheugen wordt toegevoegd als het serviceniveau van de wordt verhoogd en biedt alleen ondersteuning voor een maximaal 32 gelijktijdige query's op DW1000c en 4 en DW100c.  Zodra het exemplaar is geschaald DW1500c, gelijktijdigheidssleuven en geheugen dat wordt gebruikt door niet verhoogt smallrc als de service wordt verhoogd. 
>
>

De volgende tabel ziet u het maximum aantal gelijktijdige query's en gelijktijdigheidssleuven voor elk [dynamische resourceklasse](resource-classes-for-workload-management.md). In tegenstelling tot Gen1 zijn dynamische resourceklassen in Gen2 echt dynamisch.  Gen2 maakt gebruik van een hulpprogramma voor het percentage van de 3-10-22-70-geheugentoewijzing voor kleine-medium-groot-xlarge resourceklassen voor alle serviceniveaus.

| Servicelaag | Maximum aantal gelijktijdige query 's | Gelijktijdigheidssleuven beschikbaar | Sleuven die worden gebruikt door smallrc | Sleuven die worden gebruikt door mediumrc | Sleuven die worden gebruikt door largerc | Sleuven die worden gebruikt door xlargerc |
|:-------------:|:--------------------------:|:---------------------------:|:---------------------:|:----------------------:|:---------------------:|:----------------------:|
| DW100c        |  4                         |    4                        | 1                     |  1                     |  1                    |   2                    |
| DW200c        |  8                         |    8                        | 1                     |  1                     |  1                    |   5                    |
| DW300c        | 12                         |   12                        | 1                     |  1                     |  2                    |   8                    |
| DW400c        | 16                         |   16                        | 1                     |  1                     |  3                    |  11                    |
| DW500c        | 20                         |   20                        | 1                     |  2                     |  4                    |  14                    |
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

Statische resourceklassen

De volgende tabel ziet u het maximum aantal gelijktijdige query's en gelijktijdigheidssleuven voor elk [statische resourceklasse](resource-classes-for-workload-management.md) op **Gen1**.

| Serviceniveau | Maximum aantal gelijktijdige query 's | Maximum aantal gelijktijdige plekken gebruiken | Sleuven die worden gebruikt door staticrc10 | Sleuven die worden gebruikt door staticrc20 | Sleuven die worden gebruikt door staticrc30 | Sleuven die worden gebruikt door staticrc40 | Sleuven die worden gebruikt door staticrc50 | Sleuven die worden gebruikt door staticrc60 | Sleuven die worden gebruikt door staticrc70 | Sleuven die worden gebruikt door staticrc80 |
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

Dynamische resourceklassen
> [!NOTE]
> De resourceklasse smallrc op Gen1 wijst een vaste hoeveelheid geheugen per query, op dezelfde wijze aan de klasse statische resource staticrc10 toe.  Omdat smallrc statisch is, heeft de mogelijkheid om 128 gelijktijdige query's te schalen. 
>
>

De volgende tabel ziet u het maximum aantal gelijktijdige query's en gelijktijdigheidssleuven voor elk [dynamische resourceklasse](resource-classes-for-workload-management.md) op **Gen1**.

| Serviceniveau | Maximum aantal gelijktijdige query 's | Gelijktijdigheidssleuven beschikbaar | Sleuven die worden gebruikt door smallrc | Sleuven die worden gebruikt door mediumrc | Sleuven die worden gebruikt door largerc | Sleuven die worden gebruikt door xlargerc |
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


Als een van deze drempelwaarden wordt voldaan, worden de nieuwe query's in de wachtrij geplaatst en de uitgevoerd op basis first-in, First out.  Als een query is voltooid en het aantal query's en -sleuven onder de limieten vallen, versies SQL Data Warehouse in de wachtrij query's. 

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over hoe u resource-klassen voor het optimaliseren van uw workload meer Lees de volgende artikelen:
* [Resourceklassen voor het beheer van de werkbelasting](resource-classes-for-workload-management.md)
* [Uw werkbelasting analyseren](analyze-your-workload.md)

