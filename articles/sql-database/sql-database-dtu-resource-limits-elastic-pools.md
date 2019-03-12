---
title: Azure SQL Database DTU gebaseerde resource limieten elastische pools | Microsoft Docs
description: Deze pagina beschrijft enkele algemene DTU gebaseerde resourcelimieten voor elastische pools in Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: sachinpMSFT
ms.author: sachinp
ms.reviewer: carlrab
manager: craigg
ms.date: 03/06/2019
ms.openlocfilehash: 6de55ec66c4185a198ce819484b71550b05ba8c9
ms.sourcegitcommit: dd1a9f38c69954f15ff5c166e456fda37ae1cdf2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57569090"
---
# <a name="resources-limits-for-elastic-pools-using-the-dtu-based-purchasing-model"></a>Limieten voor elastische pools met behulp van het op DTU gebaseerde aankoopmodel resources

Dit artikel bevat de gedetailleerde resourcelimieten voor Azure SQL Database elastische pools en gepoolde databases met behulp van het op DTU gebaseerde aankoopmodel.

Zie voor DTU gebaseerde aankopen model resourcelimieten voor individuele databases, [DTU gebaseerde resourcelimieten - individuele databases](sql-database-vcore-resource-limits-elastic-pools.md). Zie voor vCore gebaseerde resourcelimieten [vCore gebaseerde resourcelimieten - individuele databases](sql-database-vcore-resource-limits-single-databases.md) en [vCore gebaseerde resourcelimieten - elastische pools](sql-database-vcore-resource-limits-elastic-pools.md).

> [!IMPORTANT]
> In sommige gevallen is het wellicht voor het verkleinen van een database voor het vrijmaken van ongebruikte ruimte. Zie voor meer informatie, [bestandsruimte in Azure SQL Database beheren](sql-database-file-space-management.md).

## <a name="elastic-pool-storage-sizes-and-compute-sizes"></a>Elastische pool: Opslaggrootte en compute-grootten

Voor SQL Database elastische pools in de volgende tabellen weergeven van de beschikbare resources in elke servicelaag en compute-grootte. U kunt de servicelaag, rekencapaciteit en opslag met behulp van bedrag instellen de [Azure-portal](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](sql-database-elastic-pool-manage.md#powershell-manage-elastic-pools-and-pooled-databases), wordt de [Azure CLI](sql-database-elastic-pool-manage.md#azure-cli-manage-elastic-pools-and-pooled-databases), of de [REST-API](sql-database-elastic-pool-manage.md#rest-api-manage-elastic-pools-and-pooled-databases).

> [!IMPORTANT]
> Zie voor richtlijnen en overwegingen bij schalen, [een elastische pool schalen](sql-database-elastic-pool-scale.md)
> [!NOTE]
> De resourcelimieten van afzonderlijke databases in elastische pools zijn algemeen hetzelfde als voor individuele databases buiten groepen op basis van dtu's en de service tier. De maximale gelijktijdige werknemers voor een S2-database is bijvoorbeeld 120 werknemers. De maximale gelijktijdige werknemers voor een database in een Standard-pool is dus ook 120 werknemers als het maximale aantal DTU per database in de pool is 50 dtu's (dit is gelijk aan S2).

### <a name="basic-elastic-pool-limits"></a>Basisbeperkingen voor elastische groepen

| eDTU's per groep | **50** | **100** | **200** | **300** | **400** | **800** | **1200** | **1600** |
|:---|---:|---:|---:| ---: | ---: | ---: | ---: | ---: |
| Inbegrepen opslag per pool (GB) | 5 | 10 | 20 | 29 | 39 | 78 | 117 | 156 |
| Opties van de maximale opslag per groep (GB) | 5 | 10 | 20 | 29 | 39 | 78 | 117 | 156 |
| Maximum aantal In-Memory OLTP-opslag per pool (GB) | N/A | N/A | N/A | N/A | N/A | N/A | N/A | N/A |
| Maximaal aantal databases per pool | 100 | 200 | 500 | 500 | 500 | 500 | 500 | 500 |
| Maximaal aantal gelijktijdige werknemers (aanvragen) per pool | 100 | 200 | 400 | 600 | 800 | 1600 | 2400 | 3200 |
| Maximaal aantal gelijktijdige sessies per pool | 30.000 | 30.000 | 30.000 | 30.000 |30.000 | 30.000 | 30.000 | 30.000 |
| Opties voor min-edtu's per database | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 |
| Opties van de maximale edtu's per database | 5 | 5 | 5 | 5 | 5 | 5 | 5 | 5 |
| Maximale opslag per database (GB) | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 |
||||||||

### <a name="standard-elastic-pool-limits"></a>Standaardbeperkingen voor elastische pools

| eDTU's per groep | **50** | **100** | **200** | **300** | **400** | **800**|
|:---|---:|---:|---:| ---: | ---: | ---: |
| Inbegrepen opslag per pool (GB) | 50 | 100 | 200 | 300 | 400 | 800 |
| Opties van de maximale opslag per groep (GB) | 50, 250, 500 | 100, 250, 500, 750 | 200, 250, 500, 750, 1024 | 300, 500, 750, 1024, 1280 | 400, 500, 750, 1024, 1280, 1536 | 800, 1024, 1280, 1536, 1792, 2048 |
| Maximum aantal In-Memory OLTP-opslag per pool (GB) | N/A | N/A | N/A | N/A | N/A | N/A |
| Maximaal aantal databases per pool | 100 | 200 | 500 | 500 | 500 | 500 |
| Maximaal aantal gelijktijdige werknemers (aanvragen) per pool | 100 | 200 | 400 | 600 | 800 | 1600 |
| Maximaal aantal gelijktijdige sessies per pool | 30.000 | 30.000 | 30.000 | 30.000 | 30.000 | 30.000 |
| Opties voor min-edtu's per database | 0, 10, 20, 50 | 0, 10, 20, 50, 100 | 0, 10, 20, 50, 100, 200 | 0, 10, 20, 50, 100, 200, 300 | 0, 10, 20, 50, 100, 200, 300, 400 | 0, 10, 20, 50, 100, 200, 300, 400, 800 |
| Opties van de maximale edtu's per database | 10, 20, 50 | 10, 20, 50, 100 | 10, 20, 50, 100, 200 | 10, 20, 50, 100, 200, 300 | 10, 20, 50, 100, 200, 300, 400 | 10, 20, 50, 100, 200, 300, 400, 800 |
| Maximale opslag per database (GB) | 500 | 750 | 1024 | 1024 | 1024 | 1024 |
||||||||

### <a name="standard-elastic-pool-limits-continued"></a>Limieten voor Standard elastische pools (vervolg)

| eDTU's per groep | **1200** | **1600** | **2000** | **2500** | **3000** |
|:---|---:|---:|---:| ---: | ---: |
| Inbegrepen opslag per pool (GB) | 1200 | 1600 | 2000 | 2500 | 3000 |
| Opties van de maximale opslag per groep (GB) | 1200, 1280, 1536, 1792, 2048, 2304, 2560 | 1600, 1792, 2048, 2304, 2560, 2816, 3072 | 2000, 2048, 2304, 2560, 2816, 3072, 3328, 3584 | 2500, 2560, 2816, 3072, 3328, 3584, 3840, 4096 | 3000, 3072, 3328, 3584, 3840, 4096 |
| Maximum aantal In-Memory OLTP-opslag per pool (GB) | N/A | N/A | N/A | N/A | N/A |
| Maximaal aantal databases per pool | 500 | 500 | 500 | 500 | 500 |
| Maximaal aantal gelijktijdige werknemers (aanvragen) per pool | 2400 | 3200 | 4000 | 5000 | 6000 |
| Maximaal aantal gelijktijdige sessies per pool | 30.000 | 30.000 | 30.000 | 30.000 | 30.000 |
| Opties voor min-edtu's per database | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500, 3000 |
| Opties van de maximale edtu's per database | 10, 20, 50, 100, 200, 300, 400, 800, 1200 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500, 3000 |
| Opties van de maximale opslag per database (GB) | 1024 | 1024 | 1024 | 1024 | 1024 |
||||||||

### <a name="premium-elastic-pool-limits"></a>Limieten voor Premium elastische pools

| eDTU's per groep | **125** | **250** | **500** | **1000** | **1500**|
|:---|---:|---:|---:| ---: | ---: |
| Inbegrepen opslag per pool (GB) | 250 | 500 | 750 | 1024 | 1536 |
| Opties van de maximale opslag per groep (GB) | 250, 500, 750, 1024 | 500, 750, 1024 | 750, 1024 | 1024 | 1536 |
| Maximum aantal In-Memory OLTP-opslag per pool (GB) | 1 | 2 | 4 | 10 | 12 |
| Maximaal aantal databases per pool | 50 | 100 | 100 | 100 | 100 |
| Maximaal aantal gelijktijdige werknemers per pool (aanvragen) | 200 | 400 | 800 | 1600 | 2400 |
| Maximaal aantal gelijktijdige sessies per pool | 30.000 | 30.000 | 30.000 | 30.000 | 30.000 |
| Minimaal aantal eDTU’s per database | 0, 25, 50, 75, 125 | 0, 25, 50, 75, 125, 250 | 0, 25, 50, 75, 125, 250, 500 | 0, 25, 50, 75, 125, 250, 500, 1000 | 0, 25, 50, 75, 125, 250, 500, 1000, 1500 |
| Maximaal aantal eDTU’s per database | 25, 50, 75, 125 | 25, 50, 75, 125, 250 | 25, 50, 75, 125, 250, 500 | 25, 50, 75, 125, 250, 500, 1000 | 25, 50, 75, 125, 250, 500, 1000, 1500 |
| Maximale opslag per database (GB) | 1024 | 1024 | 1024 | 1024 | 1024 |
||||||||

### <a name="premium-elastic-pool-limits-continued"></a>Limieten voor Premium elastische pools (vervolg)

| eDTU's per groep | **2000** | **2500** | **3000** | **3500** | **4000**|
|:---|---:|---:|---:| ---: | ---: |
| Inbegrepen opslag per pool (GB) | 2048 | 2560 | 3072 | 3548 | 4096 |
| Opties van de maximale opslag per groep (GB) | 2048 | 2560 | 3072 | 3548 | 4096|
| Maximum aantal In-Memory OLTP-opslag per pool (GB) | 16 | 20 | 24 | 28 | 32 |
| Maximaal aantal databases per pool | 100 | 100 | 100 | 100 | 100 |
| Maximaal aantal gelijktijdige werknemers (aanvragen) per pool | 3200 | 4000 | 4800 | 5600 | 6400 |
| Maximaal aantal gelijktijdige sessies per pool | 30.000 | 30.000 | 30.000 | 30.000 | 30.000 |
| Opties voor min-edtu's per database | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750, 4000 |
| Opties van de maximale edtu's per database | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750, 4000 |
| Maximale opslag per database (GB) | 1024 | 1024 | 1024 | 1024 | 1024 |
||||||||

> [!IMPORTANT]
> Voor de Premium-laag is er meer dan 1 TB aan opslagruimte beschikbaar in alle regio's, met uitzondering van: China - oost, China - noord, Duitsland - centraal, Duitsland - noordoost, US - west-centraal, US - DoD-regio's en US Government - centraal. In deze regio’s is de maximale opslagruimte in de Premium-laag beperkt tot 1 TB.  Raadpleeg [P11-P15 huidige beperkingen](sql-database-single-database-scale.md#dtu-based-purchasing-model-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb) voor meer informatie.  

Als alle DTU's van een elastische groep zijn gebruikt, ontvangt elke database in de groep een gelijke hoeveelheid resources om query's te verwerken. De service SQL Database verdeelt resources eerlijk over databases door gelijke hoeveelheden rekentijd te garanderen. Gelijke verdeling van resources in een elastische groep vindt plaats naast een bepaalde hoeveelheid resources die sowieso voor elke database wordt gegarandeerd wanneer het aantal DTU's per database is ingesteld op een andere waarde dan nul.

### <a name="database-properties-for-pooled-databases"></a>Database-eigenschappen voor databases in pools

De volgende tabel beschrijft de eigenschappen voor databases in pools.

| Eigenschap | Description |
|:--- |:--- |
| Maximaal aantal eDTU’s per database |Het maximum aantal eDTU's dat elke database in de groep mag gebruiken, is afhankelijk van het gebruik door andere databases in de groep. Het maximum aantal eDTU's per database is geen resourcegarantie voor een database. Het is een algemene instelling voor alle databases in de groep. Stel het maximum aantal eDTU's per database hoog genoeg zijn om pieken in databasegebruik te kunnen afhandelen. Enige mate van overtoewijzing is normaal, omdat de groep in het algemeen uitgaat van pieken en dalen in gebruikspatronen voor databases, waarbij alle databases niet tegelijkertijd pieken. Stel, het piekgebruik per database is twintig eDTU's en slechts 20% van de 100 databases in de groep pieken op hetzelfde moment. Als het maximum aantal eDTU's per database is ingesteld op twintig eDTU's, kan de groep redelijkerwijs vijf keer worden overtoegewezen en kan het aantal eDTU's per groep worden ingesteld op 400. |
| Minimaal aantal eDTU’s per database |Het minimum aantal eDTU's dat voor elke database in de groep wordt gegarandeerd. Het is een algemene instelling voor alle databases in de groep. Het minimum aantal eDTU’s per database kan worden ingesteld op 0. Dat is ook de standaardwaarde. Deze eigenschap wordt ingesteld op een waarde tussen 0 en het gemiddelde eDTU-gebruik per database. Het product van het aantal databases in de groep en het minimum aantal eDTU's per database mag niet groter zijn dan het aantal eDTU's per groep. Als een groep bijvoorbeeld twintig databases heeft en het minimum aantal eDTU's per database is ingesteld op tien eDTU's, dan moet het aantal eDTU's per groep minimaal 200 eDTU's zijn. |
| Maximale opslagruimte per database |De maximale databasegrootte ingesteld door de gebruiker voor een database in een pool. Echter delen gepoolde databases poolopslag toegewezen. Zelfs als de maximale totale opslag *per database* is ingesteld op niet groter zijn dan de totale beschikbare opslag *ruimte van de pool*, de totale ruimte die daadwerkelijk wordt gebruikt door alle databases worden niet langer zijn dan de beschikbare groep beperken. Maximale databasegrootte verwijst naar de maximale grootte van de gegevensbestanden en exclusief de ruimte die logboekbestanden innemen. |
|||

## <a name="next-steps"></a>Volgende stappen

- Zie voor vCore resourcelimieten voor een individuele database, [resourcelimieten voor individuele databases met behulp van de vCore-aanschafmodel](sql-database-vcore-resource-limits-single-databases.md)
- Zie voor DTU resourcelimieten voor een individuele database, [resourcelimieten voor individuele databases met behulp van het op DTU gebaseerde aankoopmodel](sql-database-dtu-resource-limits-single-databases.md)
- Zie voor vCore resourcelimieten voor elastische pools, [resourcelimieten voor elastische pools met behulp van de vCore-aanschafmodel](sql-database-vcore-resource-limits-elastic-pools.md)
- Zie voor resourcelimieten voor beheerde exemplaren, [beheerd exemplaar resourcelimieten](sql-database-managed-instance-resource-limits.md).
- Zie voor meer informatie over algemene Azure-limieten [Azure-abonnement en Servicelimieten, quotums en beperkingen](../azure-subscription-service-limits.md).
- Zie voor meer informatie over de resourcelimieten op een databaseserver [overzicht van resourcelimieten voor een SQL-databaseserver](sql-database-resource-limits-database-server.md) voor informatie over de beperkingen op het niveau van de server en -abonnement.