---
title: Azure SQL Database-service | Microsoft Docs
description: "Meer informatie over Servicelagen voor één en groep databases prestatieniveaus en opslaggrootte opgeven."
keywords: 
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: f5c5c596-cd1e-451f-92a7-b70d4916e974
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Active
ms.date: 08/20/2017
ms.author: carlrab
ms.openlocfilehash: 55f59fddee008eb42b7252d6368a56873a6abd16
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="what-are-azure-sql-database-service-tiers"></a>Wat is Azure SQL Database Servicelagen

[Azure SQL Database](sql-database-technical-overview.md) biedt **Basic**, **standaard**, **Premium**, en **Premium RS** Servicelagen voor beide [databases eenmalige](sql-database-single-database-resources.md) en [elastische pools](sql-database-elastic-pool.md). Servicelagen worden voornamelijk onderscheiden door een bereik van het prestatieniveau en opties voor opslag en prijs.  Alle Servicelagen bieden flexibiliteit bij het wijzigen van de prestaties niveau en opslaggrootte.  Individuele databases en elastische pools wordt gefactureerd per uur op basis van de servicelaag, prestatieniveau en maximale grootte.   

## <a name="choosing-a-service-tier"></a>Het kiezen van een servicelaag

Het kiezen van een servicelaag is vooral afhankelijk van zakelijke continuïteit-, opslag- en prestatie-eisen.
| | **Basic** | **Standard** |**Premium** |**Premium-RS** |
| :-- | --: |--:| --:| --:| 
|Doel werkbelasting|Ontwikkeling en productie|Ontwikkeling en productie|Ontwikkeling en productie|Werkbelasting die gegevensverlies tot 5 minuten vanwege servicefouten tolereert|
|SLA voor actieve tijdsduur|99,99%|99,99%|99,99%|N.V.T. in preview|
|Retentie van back-ups|7 dagen|35 dagen|35 dagen|35 dagen|
|CPU|Laag|Laag, Gemiddeld, hoog|Gemiddeld, hoog|Middelgroot|
|I/o-doorvoer|Laag  | Middelgroot | Orde van grootte hoger is dan de standaard|Zelfde als Premium|
|I/o-latentie|Hoger is dan Premium|Hoger is dan Premium|Lager is dan Basic en Standard|Zelfde als Premium|
|Columnstore indexeren en in het geheugen OLTP|N.v.t.|N.v.t.|Ondersteund|Ondersteund|
|||||

## <a name="performance-level-and-storage-size-limits"></a>Maximale grootte van prestaties en de opslag

Prestatieniveaus worden uitgedrukt in termen van de Database Transaction Units (dtu's) voor individuele databases en elastische Database Transaction Units (edtu's) voor elastische pools. Zie voor meer informatie over dtu's en edtu's [wat zijn dtu's en edtu's?](sql-database-what-is-a-dtu.md).

### <a name="single-databases"></a>Individuele databases

|  | **Basic** | **Standard** | **Premium** | **Premium-RS**|
| :-- | --: | --: | --: | --: |
| Maximale grootte * | 2 GB | 1 TB | 4 TB  | 1 TB  |
| Maximale aantal dtu 's | 5 | 3000 | 4000 | 1000 |
||||||

### <a name="elastic-pools"></a>Pools voor Elastic Database

| | **Basic** | **Standard** | **Premium** | **Premium-RS**|
| :-- | --: | --: | --: | --: |
| Maximum opslagruimte per database *  | 2 GB | 1 TB | 1 TB | 1 TB |
| Maximum opslagruimte per groep * | 156 GB | 4 TB | 4 TB | 1 TB |
| Maximum aantal edtu's per database | 5 | 3000 | 4000 | 1000 |
| Maximum aantal edtu's per groep | 1600 | 3000 | 4000 | 1000 |
| Maximum aantal databases per groep | 500  | 500 | 100 | 100 |
||||||

> [!IMPORTANT]
> \* Opslagruimten groter dan de hoeveelheid inbegrepen opslagruimte zijn in preview en hiervoor gelden extra kosten. Zie [de prijsinformatie voor SQL Database](https://azure.microsoft.com/pricing/details/sql-database/) voor meer informatie. 
>
> \* In de Premium-laag is op dit moment opslag van meer dan 1 TB beschikbaar in de volgende regio's: VS Oost 2, VS West, VS (overheid) Virginia, West-Europa, Duitsland Centraal, Zuidoost-Azië, Japan - oost, Australië - oost, Canada Centraal en Canada Oost. Zie [P11-P15: huidige beperkingen](sql-database-resource-limits.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  
> 

Zie voor meer informatie over specifieke prestatieniveaus en opslag beschikbare keuzen [limieten voor SQL-Database](sql-database-resource-limits.md).


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [databaseresources eenmalige](sql-database-single-database-resources.md).
- Meer informatie over elastische pools, Zie [elastische pools](sql-database-elastic-pool.md).
- Meer informatie over [Azure-abonnement en Servicelimieten, quota's en beperkingen](../azure-subscription-service-limits.md)
* Meer informatie over [dtu's en edtu's](sql-database-what-is-a-dtu.md).
* Lees over het DTU-gebruik controleren [bewaking en prestatieafstemming](sql-database-troubleshoot-performance.md).

