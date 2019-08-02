---
title: Azure SQL Database op DTU gebaseerde resource limieten voor afzonderlijke data bases | Microsoft Docs
description: Op deze pagina worden enkele algemene DTU-gebaseerde resource limieten voor afzonderlijke data bases in Azure SQL Database beschreven.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 03/20/2019
ms.openlocfilehash: 84e39a727d5be842b356e5bd30333cb154df86aa
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68568773"
---
# <a name="resource-limits-for-single-databases-using-the-dtu-based-purchasing-model"></a>Resource limieten voor afzonderlijke data bases met behulp van het DTU-gebaseerd inkoop model

In dit artikel vindt u gedetailleerde resource limieten voor Azure SQL Database afzonderlijke data bases met behulp van het op DTU gebaseerde aankoop model.

Zie voor op DTU gebaseerde resource limieten voor elastische Pools op [DTU gebaseerde resource limieten: elastische Pools](sql-database-dtu-resource-limits-elastic-pools.md). Zie voor vCore resource limieten op [vCore gebaseerde resource limieten-afzonderlijke data bases](sql-database-vcore-resource-limits-single-databases.md) en [vCore resource limieten: elastische Pools](sql-database-vcore-resource-limits-elastic-pools.md). Zie [Inkoop modellen en service lagen](sql-database-purchase-models.md)voor meer informatie over de verschillende aankoop modellen.

## <a name="single-database-storage-sizes-and-compute-sizes"></a>Eén Data Base: Opslag grootten en berekenings grootten

In de volgende tabellen ziet u de beschik bare resources voor één data base in elke servicelaag en de reken grootte. U kunt de servicelaag, de reken grootte en de opslag hoeveelheid voor één data base instellen met behulp van de [Azure Portal](sql-database-single-databases-manage.md#manage-an-existing-sql-database-server), [Transact-SQL](sql-database-single-databases-manage.md#transact-sql-manage-sql-database-servers-and-single-databases), [Power shell](sql-database-single-databases-manage.md#powershell-manage-sql-database-servers-and-single-databases), de [Azure cli](sql-database-single-databases-manage.md#azure-cli-manage-sql-database-servers-and-single-databases)of de [rest API](sql-database-single-databases-manage.md#rest-api-manage-sql-database-servers-and-single-databases).

> [!IMPORTANT]
> Zie [Eén data base schalen](sql-database-single-database-scale.md) voor meer informatie over schaling en overwegingen

### <a name="basic-service-tier"></a>Servicelaag Basic

| **Reken grootte** | **Basic** |
| :--- | --: |
| Maximaal aantal DTU's | 5 |
| Inbegrepen opslag (GB) | 2 |
| Maximum aantal opslag opties (GB) | 2 |
| Maximale OLTP-opslag in het geheugen (GB) |N/A |
| Maxi maal aantal gelijktijdige werk nemers (aanvragen) | 30 |
| Maximaal aantal gelijktijdige sessies | 300 |
|||

### <a name="standard-service-tier"></a>Servicelaag Standard

| **Reken grootte** | **S0** | **S1** | **S2** | **S3** |
| :--- |---:| ---:|---:|---:|
| Maximaal aantal DTU's | 10 | 20 | 50 | 100 |
| Inbegrepen opslag (GB) | 250 | 250 | 250 | 250 |
| Maximum aantal opslag opties (GB) | 250 | 250 | 250 | 250, 500, 750, 1024 |
| Maximale OLTP-opslag in het geheugen (GB) | N/A | N/A | N/A | N/A |
| Maxi maal aantal gelijktijdige werk nemers (aanvragen)| 60 | 90 | 120 | 200 |
| Maximaal aantal gelijktijdige sessies |600 | 900 | 1200 | 2400 |
||||||

### <a name="standard-service-tier-continued"></a>Standard-servicelaag (vervolg)

| **Reken grootte** | **S4** | **S6** | **S7** | **S9** | **S12** |
| :--- |---:| ---:|---:|---:|---:|
| Maximaal aantal DTU's | 200 | 400 | 800 | 1600 | 3000 |
| Inbegrepen opslag (GB) | 250 | 250 | 250 | 250 | 250 |
| Maximum aantal opslag opties (GB) | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 |
| Maximale OLTP-opslag in het geheugen (GB) | N/A | N/A | N/A | N/A |N/A |
| Maxi maal aantal gelijktijdige werk nemers (aanvragen)| 400 | 800 | 1600 | 3200 |6000 |
| Maximaal aantal gelijktijdige sessies |4800 | 9600 | 19200 | 30000 |30000 |
|||||||

### <a name="premium-service-tier"></a>Premium servicelaag

| **Reken grootte** | **P1** | **P2** | **P4** | **P6** | **P11** | **P15** |
| :--- |---:|---:|---:|---:|---:|---:|
| Maximaal aantal DTU's | 125 | 250 | 500 | 1000 | 1750 | 4000 |
| Inbegrepen opslag (GB) | 500 | 500 | 500 | 500 | 4096* | 4096* |
| Maximum aantal opslag opties (GB) | 500, 750, 1024 | 500, 750, 1024 | 500, 750, 1024 | 500, 750, 1024 | 4096* | 4096* |
| Maximale OLTP-opslag in het geheugen (GB) | 1 | 2 | 4 | 8 | 14 | 32 |
| Maxi maal aantal gelijktijdige werk nemers (aanvragen)| 200 | 400 | 800 | 1600 | 2400 | 6400 |
| Maximaal aantal gelijktijdige sessies | 30000 | 30000 | 30000 | 30000 | 30000 | 30000 |
|||||||

\*Van 1024 GB tot 4096 GB in stappen van 256 GB

> [!IMPORTANT]
> Voor de Premium-laag is er meer dan 1 TB aan opslagruimte beschikbaar in alle regio's, met uitzondering van: China - oost, China - noord, Duitsland - centraal, Duitsland - noordoost, US - west-centraal, US - DoD-regio's en US Government - centraal. In deze regio’s is de maximale opslagruimte in de Premium-laag beperkt tot 1 TB.  Raadpleeg [P11-P15 huidige beperkingen](sql-database-single-database-scale.md#p11-and-p15-constraints-when-max-size-greater-than-1-tb) voor meer informatie.  
> [!NOTE]
> Zie `tempdb` voor limieten [tempdb](https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database?view=sql-server-2017#tempdb-database-in-sql-database)-limieten.

## <a name="next-steps"></a>Volgende stappen

- Zie [resource limieten voor afzonderlijke data bases met behulp van het op vCore gebaseerde inkoop model](sql-database-vcore-resource-limits-single-databases.md) voor vCore resource limieten voor één data base
- Zie [resource limieten voor elastische Pools met behulp van het op vCore gebaseerde inkoop model](sql-database-vcore-resource-limits-elastic-pools.md) voor vCore resource limieten voor elastische Pools
- Zie resource limieten voor elastische Pools [met behulp van het op DTU gebaseerde aankoop model voor de](sql-database-dtu-resource-limits-elastic-pools.md) limieten voor DTU-bronnen voor elastische Pools
- Zie [resource limieten voor beheerde](sql-database-managed-instance-resource-limits.md)exemplaren voor resource limieten voor beheerde instanties.
- Zie [Azure-abonnement en service limieten, quota's en beperkingen](../azure-subscription-service-limits.md)voor meer informatie over algemene Azure-limieten.
- Zie [overzicht van resource limieten op een SQL database server](sql-database-resource-limits-database-server.md) voor informatie over limieten op het niveau van de server en het abonnement voor meer informatie over de limieten voor bronnen op een database server.
