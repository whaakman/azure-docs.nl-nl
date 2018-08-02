---
title: Azure SQL Database-Servicelagen - DTU | Microsoft Docs
description: Meer informatie over Servicelagen voor enkele en groep databases om prestatieniveaus en opslagruimten te bieden.
services: sql-database
author: sachinpMSFT
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 08/01/2018
manager: craigg
ms.author: carlrab
ms.openlocfilehash: 5d16763fc8f3331082b98216d25190b945d95b60
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39411817"
---
# <a name="choosing-a-dtu-based-service-tier-performance-level-and-storage-resources"></a>Een DTU-servicelaag en prestatieniveau storage-resources te kiezen 

Service-lagen van elkaar worden onderscheiden door een bereik van de prestaties met een vaste hoeveelheid inbegrepen opslag, vaste bewaarperiode voor back-ups en vaste prijs. Alle service-lagen bieden flexibiliteit van prestatieniveau wisselt zonder uitvaltijd. Individuele databases en elastische pools worden gefactureerd per uur op basis van de servicelaag en prestaties.

> [!IMPORTANT]
> SQL Database Managed Instance, ondersteunt momenteel in openbare preview-versie geen een op DTU gebaseerde aankoopmodel. Zie voor meer informatie, [Azure SQL Database Managed Instance](sql-database-managed-instance.md). 

## <a name="choosing-a-dtu-based-service-tier"></a>Een DTU gebaseerde servicelaag kiezen

Een servicelaag kiezen afhankelijk is voornamelijk van zakelijke continuïteit-, opslag- en prestatievereisten.
||Basic|Standard|Premium|
| :-- | --: |--:| --:| --:| 
|Specifieke workload|Ontwikkeling en productie|Ontwikkeling en productie|Ontwikkeling en productie||
|SLA voor actieve tijdsduur|99,99%|99,99%|99,99%|N.V.T. Preview-versie|
|Retentie van back-ups|7 dagen|35 dagen|35 dagen|
|CPU|Laag|Laag, Gemiddeld, hoog|Gemiddeld, hoog|
|I/o-doorvoer (bij benadering) |2.5 IOP's per DTU| 2.5 IOP's per DTU | 48 IOP's per DTU|
|I/o-latentie (bij benadering)|5 ms (lezen), 10 ms (schrijven)|5 ms (lezen), 10 ms (schrijven)|2 ms (lezen/schrijven)|
|Columnstore-indexering |N/A|S3 en hoger|Ondersteund|
|In-memory OLTP|N/A|N/A|Ondersteund|
|||||

## <a name="single-database-dtu-and-storage-limits"></a>DTU- en opslaglimieten van individuele database

Prestatieniveaus worden uitgedrukt in termen van DTU’s (DTU’s: Database Transaction Units) voor enkelvoudige databases en eDTU’s (eDTU’s: elastische DTU’s) voor elastische pools. Zie voor meer informatie over dtu's en Edtu's [wat zijn dtu's en edtu's](sql-database-service-tiers.md#what-are-database-transaction-units-dtus)?

||Basic|Standard|Premium|
| :-- | --: | --: | --: | --: |
| Maximale opslagruimte | 2 GB | 1 TB | 4 TB  | 
| Maximum aantal dtu 's | 5 | 3000 | 4000 | |
||||||

> [!IMPORTANT]
> In sommige gevallen is het wellicht voor het verkleinen van een database voor het vrijmaken van ongebruikte ruimte. Zie voor meer informatie, [bestandsruimte in Azure SQL Database beheren](sql-database-file-space-management.md).

## <a name="elastic-pool-edtu-storage-and-pooled-database-limits"></a>Elastische pool-eDTU-, opslag- en limieten voor gegroepeerde databases

| | **Basic** | **Standard** | **Premium** | 
| :-- | --: | --: | --: | --: |
| Maximale opslagruimte per database  | 2 GB | 1 TB | 1 TB | 
| Maximale opslagruimte per groep | 156 GB | 4 TB | 4 TB | 
| Maximum aantal edtu's per database | 5 | 3000 | 4000 | 
| Maximum aantal edtu's per groep | 1600 | 3000 | 4000 | 
| Maximum aantal databases per pool | 500  | 500 | 100 | 
||||||

> [!IMPORTANT]
> Meer dan 1 TB aan opslag in de Premium-laag is momenteel beschikbaar in alle regio's behalve het volgende: West-Centraal VS, China-Oost, USDoDCentral, Duitsland-centraal, USDoDEast, VS (overheid)-zuidwesten, USGov Iowa, Duitsland-Noordoost, China-Noord. In andere regio’s is de maximale opslagruimte in de Premium-laag beperkt tot 1 TB. Zie [P11-P15: huidige beperkingen](sql-database-dtu-resource-limits-single-databases.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  

> [!IMPORTANT]
> In sommige gevallen is het wellicht voor het verkleinen van een database voor het vrijmaken van ongebruikte ruimte. Zie voor meer informatie, [bestandsruimte in Azure SQL Database beheren](sql-database-file-space-management.md).

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over specifieke prestatieniveaus en opties voor opslag beschikbaar voor individuele databases, [SQL Database DTU gebaseerde resourcelimieten voor individuele databases](sql-database-dtu-resource-limits-single-databases.md#single-database-storage-sizes-and-performance-levels).
- Zie voor meer informatie over specifieke prestatieniveaus en opties voor opslag beschikbaar voor elastische pools, [SQL Database DTU gebaseerde resourcelimieten](sql-database-dtu-resource-limits-elastic-pools.md#elastic-pool-storage-sizes-and-performance-levels).
