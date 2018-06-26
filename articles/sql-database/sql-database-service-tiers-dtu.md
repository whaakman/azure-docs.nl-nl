---
title: Azure SQL Database Servicelagen - DTU | Microsoft Docs
description: Meer informatie over Servicelagen voor één en groep databases prestatieniveaus en opslaggrootte opgeven.
services: sql-database
author: CarlRabeler
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 06/20/2018
manager: craigg
ms.author: carlrab
ms.openlocfilehash: cf17ec616819da94678f2ae4f0f0ca283f99f629
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36750421"
---
# <a name="choosing-a-dtu-based-service-tier-performance-level-and-storage-resources"></a>Een servicelaag met DTU prestatieniveau en opslagbronnen kiezen 

Servicelagen van elkaar worden onderscheiden door een bereik van prestatieniveaus met een vaste hoeveelheid opgenomen opslag, vaste bewaarperiode voor back-ups en vaste prijs. Alle Servicelagen bieden flexibiliteit voor het wijzigen van prestatieniveaus zonder uitvaltijd. Individuele databases en elastische pools wordt gefactureerd per uur op basis van prijscategorie en prestatieniveau serviceniveau.

> [!IMPORTANT]
> Exemplaar van SQL Database-beheerd biedt openbare preview momenteel geen ondersteuning een DTU-aankoopmodel. Zie voor meer informatie [Azure SQL Database-beheerd instantie](sql-database-managed-instance.md). 

## <a name="choosing-a-dtu-based-service-tier"></a>Een servicelaag met DTU kiezen

Het kiezen van een servicelaag is vooral afhankelijk van zakelijke continuïteit-, opslag- en prestatie-eisen.
||Basic|Standard|Premium|
| :-- | --: |--:| --:| --:| 
|Doel werkbelasting|Ontwikkeling en productie|Ontwikkeling en productie|Ontwikkeling en productie||
|SLA voor actieve tijdsduur|99,99%|99,99%|99,99%|N.V.T. in preview|
|Back-upretentie|7 dagen|35 dagen|35 dagen|
|CPU|Laag|Laag, Gemiddeld, hoog|Gemiddeld, hoog|
|I/o-doorvoer (bij benadering) |2.5 IOP's per DTU| 2.5 IOP's per DTU | 48 IOP's per DTU|
|I/o-latentie (bij benadering)|5 ms (lezen), 10 ms (schrijven)|5 ms (lezen), 10 ms (schrijven)|2 ms (lezen/schrijven)|
|Columnstore indexeren |N/A|S3 en hoger|Ondersteund|
|In het geheugen OLTP|N/A|N/A|Ondersteund|
|||||

## <a name="single-database-dtu-and-storage-limits"></a>Individuele database DTU en opslaglimieten

Prestatieniveaus worden uitgedrukt in termen van DTU’s (DTU’s: Database Transaction Units) voor enkelvoudige databases en eDTU’s (eDTU’s: elastische DTU’s) voor elastische pools. Zie voor meer informatie over dtu's en edtu's [wat dtu's en edtu's zijn](sql-database-service-tiers.md#what-are-database-transaction-units-dtus)?

||Basic|Standard|Premium|
| :-- | --: | --: | --: | --: |
| Maximale opslaggrootte | 2 GB | 1 TB | 4 TB  | 
| Maximale aantal dtu 's | 5 | 3000 | 4000 | |
||||||

## <a name="elastic-pool-edtu-storage-and-pooled-database-limits"></a>Elastische pool-eDTU-, opslag- en limieten voor gegroepeerde database

| | **Basic** | **Standard** | **Premium** | 
| :-- | --: | --: | --: | --: |
| Maximum opslagruimte per database  | 2 GB | 1 TB | 1 TB | 
| Maximum opslagruimte per groep | 156 GB | 4 TB | 4 TB | 
| Maximum aantal edtu's per database | 5 | 3000 | 4000 | 
| Maximum aantal edtu's per groep | 1600 | 3000 | 4000 | 
| Maximum aantal databases per groep | 500  | 500 | 100 | 
||||||

> [!IMPORTANT]
> Meer dan 1 TB aan opslag in de laag Premium is momenteel beschikbaar in alle regio's, behalve het volgende: VK Noord, West-Centraal VS, UK South2, China Oost, USDoDCentral, Duitsland centraal, USDoDEast, VS Gov linksonder, ons Gov Zuid-centraal, Duitsland noordoosten, China Noord, Gov VS-Oost. In andere regio’s is de maximale opslagruimte in het Premium-niveau beperkt tot 1 TB. Zie [P11-P15: huidige beperkingen](sql-database-dtu-resource-limits-single-databases.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over specifieke prestatieniveaus en opties voor opslag beschikbaar voor individuele databases [resource op basis van SQL Database DTU-limieten voor individuele databases](sql-database-dtu-resource-limits-single-databases.md#single-database-storage-sizes-and-performance-levels).
- Zie voor meer informatie over specifieke prestatieniveaus en opties voor opslag beschikbaar voor elastische pools [resource op basis van SQL Database DTU-limieten](sql-database-dtu-resource-limits-elastic-pools.md#elastic-pool-storage-sizes-and-performance-levels).