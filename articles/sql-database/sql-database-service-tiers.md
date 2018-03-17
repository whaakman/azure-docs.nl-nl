---
title: Azure SQL Database-service | Microsoft Docs
description: "Meer informatie over Servicelagen voor één en groep databases prestatieniveaus en opslaggrootte opgeven."
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: article
ms.date: 03/15/2018
ms.author: carlrab
ms.openlocfilehash: 6153616de763eee1b20fff40d38816eca8b455de
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/17/2018
---
# <a name="what-are-azure-sql-database-service-tiers"></a>Wat is Azure SQL Database Servicelagen?

[Azure SQL Database](sql-database-technical-overview.md) biedt **Basic**, **standaard**, en **Premium** Servicelagen voor beide [databases eenmalige](sql-database-single-database-resources.md) en [elastische pools](sql-database-elastic-pool.md). SQL-Database biedt een servicelaag voor algemene doeleinden voor [Azure SQL Database-beheerd instantie](sql-database-managed-instance.md#managed-instance-service-tier). Servicelagen worden voornamelijk onderscheiden door een bereik van het prestatieniveau en opties voor opslag en prijs.  Alle Servicelagen bieden flexibiliteit bij het wijzigen van de prestaties niveau en opslaggrootte.  Individuele databases en elastische pools wordt gefactureerd per uur op basis van de servicelaag, prestatieniveau en maximale grootte.   

> [!IMPORTANT]
> Exemplaar van SQL Database-beheerd biedt die momenteel in de openbare preview een enkele algemene servicelaag. Zie voor meer informatie [Azure SQL Database-beheerd instantie](sql-database-managed-instance.md). De rest van dit artikel niet van toepassing op beheerde exemplaar.

## <a name="choosing-a-service-tier"></a>Het kiezen van een servicelaag

Het kiezen van een servicelaag is vooral afhankelijk van zakelijke continuïteit-, opslag- en prestatie-eisen.
| | **Basic** | **Standard** |**Premium**  |
| :-- | --: |--:| --:| --:| 
|Doel werkbelasting|Ontwikkeling en productie|Ontwikkeling en productie|Ontwikkeling en productie||
|SLA voor actieve tijdsduur|99,99%|99,99%|99,99%|N.V.T. in preview|
|Retentie van back-ups|7 dagen|35 dagen|35 dagen|
|CPU|Laag|Laag, Gemiddeld, hoog|Gemiddeld, hoog|
|I/o-doorvoer (bij benadering) |2.5 IOP's per DTU  | 2.5 IOP's per DTU | 48 IOP's per DTU|
|I/o-latentie (bij benadering)|5 ms (lezen), 10 ms (schrijven)|5 ms (lezen), 10 ms (schrijven)|2 ms (lezen/schrijven)|
|Columnstore indexeren en in het geheugen OLTP|N/A|N/A|Ondersteund|
|||||

## <a name="performance-level-and-storage-size-limits"></a>Maximale grootte van prestaties en de opslag

Prestatieniveaus worden uitgedrukt in termen van DTU’s (DTU’s: Database Transaction Units) voor enkelvoudige databases en eDTU’s (eDTU’s: elastische DTU’s) voor elastische pools. Zie voor meer informatie over dtu's en edtu's [wat zijn dtu's en edtu's?](sql-database-what-is-a-dtu.md)

### <a name="single-databases"></a>Individuele databases

|  | **Basic** | **Standard** | **Premium** | 
| :-- | --: | --: | --: | --: |
| Maximale grootte * | 2 GB | 1 TB | 4 TB  | 
| Maximale aantal dtu 's | 5 | 3000 | 4000 | |
||||||

### <a name="elastic-pools"></a>Pools voor Elastic Database

| | **Basic** | **Standard** | **Premium** | 
| :-- | --: | --: | --: | --: |
| Maximum opslagruimte per database *  | 2 GB | 1 TB | 1 TB | 
| Maximum opslagruimte per groep * | 156 GB | 4 TB | 4 TB | 
| Maximum aantal edtu's per database | 5 | 3000 | 4000 | 
| Maximum aantal edtu's per groep | 1600 | 3000 | 4000 | 
| Maximum aantal databases per groep | 500  | 500 | 100 | 
||||||

> [!IMPORTANT]
> \* Opslagruimten groter dan de hoeveelheid inbegrepen opslagruimte zijn in preview en hiervoor gelden extra kosten. Zie [de prijsinformatie voor SQL Database](https://azure.microsoft.com/pricing/details/sql-database/) voor meer informatie. 
>
> \* In de Premium-laag is momenteel ruim 1 TB opslag beschikbaar voor de volgende regio's: Australië - oost, Australië - zuidoost, Brazilië - zuid, Canada - centraal, Canada - oost, VS - midden, Frankrijk - centraal, Duitsland - centraal, Japan - oost, Japan - west, Korea - centraal, VS Noord-Centraal, Noord-Europa, VS Zuid-Centraal, Zuidoost-Azië, UK - zuid, UK - west, VS - oost2, VS - west, VS (overheid) - Virginia, en West-Europa. Zie [P11-P15: huidige beperkingen](sql-database-resource-limits.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  
> 

Zie voor meer informatie over specifieke prestatieniveaus en opslag beschikbare keuzen [limieten voor SQL-Database](sql-database-resource-limits.md).


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [databaseresources eenmalige](sql-database-single-database-resources.md).
- Meer informatie over elastische pools, Zie [elastische pools](sql-database-elastic-pool.md).
- Meer informatie over [Azure-abonnement en Servicelimieten, quota's en beperkingen](../azure-subscription-service-limits.md)
* Meer informatie over [dtu's en edtu's](sql-database-what-is-a-dtu.md).
* Lees over het DTU-gebruik controleren [bewaking en prestatieafstemming](sql-database-troubleshoot-performance.md).

