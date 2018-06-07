---
title: Azure SQL Database-service - DTU | Microsoft Docs
description: Meer informatie over Servicelagen voor één en groep databases prestatieniveaus en opslaggrootte opgeven.
services: sql-database
author: CarlRabeler
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 05/22/2018
manager: craigg
ms.author: carlrab
ms.openlocfilehash: 075e04db27006719ec7d6e08eb2696436d1010f4
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34648646"
---
# <a name="dtu-based-purchasing-model-for-azure-sql-database"></a>Op basis van DTU aankoopmodel voor Azure SQL Database 


[Azure SQL Database](sql-database-technical-overview.md) biedt twee aankopen modellen voor compute, storage en i/o-resources: een DTU-aankoopmodel en een vCore gebaseerde aankoopmodel (preview). De volgende tabel en grafiek vergelijken en contrast van deze twee aankopen modellen.

> [!IMPORTANT]
> Zie voor vCore gebaseerde aankoopmodel (preview), [aankoopmodel vCore gebaseerde](sql-database-service-tiers-vcore.md)


|**Aankoopmodel**|**Beschrijving**|**Bij beste voor**|
|---|---|---|
|Op basis van het DTU-model|Dit model is gebaseerd op een gecombineerde meting van de berekenings-, opslag- en i/o-resources. Prestatieniveaus worden uitgedrukt in termen van DTU’s (DTU’s: Database Transaction Units) voor enkelvoudige databases en eDTU’s (eDTU’s: elastische DTU’s) voor elastische pools. Zie voor meer informatie over dtu's en edtu's [wat dtu's en edtu's zijn](sql-database-what-is-a-dtu.md)?|Bij beste voor klanten die eenvoudige, vooraf geconfigureerde opties willen.| 
|op basis van vCore model|Dit model kunt u de berekenings-en opslagbronnen onafhankelijk van elkaar schalen. Ook kunt u Azure hybride voordeel voor SQL Server gebruiken om te krijgen van kosten te besparen.|Bij beste voor flexibiliteit, controle en transparantie value-klanten.|
||||  

![Prijsmodel](./media/sql-database-service-tiers/pricing-model.png)

## <a name="dtu-based-purchasing-model"></a>Op basis van DTU aankoopmodel

De doorvoer DTU (Database Unit) vertegenwoordigt een gecombineerde meting van CPU, geheugen, leest en schrijft. De DTU-aankoopmodel biedt een reeks vooraf geconfigureerde bundels van rekenresources en opslag op verschillende niveaus van toepassingsprestaties station opgenomen. Klanten die de voorkeur aan van een vooraf geconfigureerde bundel en vaste betalingen elke maand, vinden het model op basis van DTU meer geschikt zijn voor hun behoeften. In de DTU gebaseerde aankoopmodel, klanten kunnen kiezen tussen **Basic**, **standaard**, en **Premium** Servicelagen voor beide [eenmalige databases](sql-database-single-database-resources.md) en [elastische pools](sql-database-elastic-pool.md). Servicelagen van elkaar worden onderscheiden door een bereik van prestatieniveaus met een vaste hoeveelheid opgenomen opslag, vaste bewaarperiode voor back-ups en vaste prijs. Alle Servicelagen bieden flexibiliteit voor het wijzigen van prestatieniveaus zonder uitvaltijd. Individuele databases en elastische pools wordt gefactureerd per uur op basis van prijscategorie en prestatieniveau serviceniveau.

> [!IMPORTANT]
> Exemplaar van SQL Database-beheerd biedt openbare preview momenteel geen ondersteuning een DTU-aankoopmodel. Zie voor meer informatie [Azure SQL Database-beheerd instantie](sql-database-managed-instance.md). 

## <a name="choosing-a-service-tier-in-the-dtu-based-purchasing-model"></a>Een servicetier kiezen in de DTU-aankoopmodel

Het kiezen van een servicelaag is vooral afhankelijk van zakelijke continuïteit-, opslag- en prestatie-eisen.
||Basic|Standard|Premium|
| :-- | --: |--:| --:| --:| 
|Doel werkbelasting|Ontwikkeling en productie|Ontwikkeling en productie|Ontwikkeling en productie||
|SLA voor actieve tijdsduur|99,99%|99,99%|99,99%|N.V.T. in preview|
|Retentie van back-ups|7 dagen|35 dagen|35 dagen|
|CPU|Laag|Laag, Gemiddeld, hoog|Gemiddeld, hoog|
|I/o-doorvoer (bij benadering) |2.5 IOP's per DTU| 2.5 IOP's per DTU | 48 IOP's per DTU|
|I/o-latentie (bij benadering)|5 ms (lezen), 10 ms (schrijven)|5 ms (lezen), 10 ms (schrijven)|2 ms (lezen/schrijven)|
|Columnstore indexeren |N/A|S3 en hoger|Ondersteund|
|In het geheugen OLTP|N/A|N/A|Ondersteund|
|||||

## <a name="performance-level-and-storage-size-limits-in-the-dtu-based-purchasing-model"></a>Prestaties niveau en opslaglimieten voor grootte in de DTU-aankoopmodel

Prestatieniveaus worden uitgedrukt in termen van DTU’s (DTU’s: Database Transaction Units) voor enkelvoudige databases en eDTU’s (eDTU’s: elastische DTU’s) voor elastische pools. Zie voor meer informatie over dtu's en edtu's [wat dtu's en edtu's zijn](sql-database-what-is-a-dtu.md)?

### <a name="single-databases"></a>Individuele databases

||Basic|Standard|Premium|
| :-- | --: | --: | --: | --: |
| Maximale grootte * | 2 GB | 1 TB | 4 TB  | 
| Maximale aantal dtu 's | 5 | 3000 | 4000 | |
||||||

Zie voor meer informatie over specifieke prestatieniveaus en opties voor opslag beschikbaar voor individuele databases [resource op basis van SQL Database DTU-limieten voor individuele databases](sql-database-dtu-resource-limits.md#single-database-storage-sizes-and-performance-levels).

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
> - Opslagruimten groter dan de hoeveelheid inbegrepen opslagruimte zijn in preview en hiervoor gelden extra kosten. Zie [de prijsinformatie voor SQL Database](https://azure.microsoft.com/pricing/details/sql-database/) voor meer informatie. 
> - Meer dan 1 TB aan opslag in de laag Premium is beschikbaar in alle regio's met de volgende uitzondering: VK Noord, West-Centraal VS, UK South2, China Oost, USDoDCentral, Duitsland centraal, USDoDEast, VS Gov linksonder, ons Gov Zuid-centraal, Duitsland noordoosten, China Noord, Gov ons Oost. Het ligt in de bedoeling de optie in meer regio's beschikbaar te maken. In andere regio’s is de maximale opslagruimte in het Premium-niveau beperkt tot 1 TB. Zie [P11-P15: huidige beperkingen](sql-database-dtu-resource-limits.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  
> 
Zie voor meer informatie over specifieke prestatieniveaus en opties voor opslag beschikbaar voor elastische pools [resource op basis van SQL Database DTU-limieten](sql-database-dtu-resource-limits.md#elastic-pool-storage-sizes-and-performance-levels).



## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over specifieke prestatieniveaus en opslag beschikbare keuzen [resource op basis van SQL Database DTU-limieten](sql-database-dtu-resource-limits.md) en [SQL-Database op basis van vCore limieten](sql-database-vcore-resource-limits.md).
- Zie [SQL Database Veelgestelde vragen over](sql-database-faq.md) voor antwoorden op veelgestelde vragen.
- Meer informatie over [Azure-abonnement en Servicelimieten, quota's en beperkingen](../azure-subscription-service-limits.md)
