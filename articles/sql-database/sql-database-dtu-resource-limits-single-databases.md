---
title: Azure SQL Database DTU gebaseerde resource beperkt individuele databases | Microsoft Docs
description: Deze pagina beschrijft enkele algemene DTU gebaseerde resourcelimieten voor individuele databases in Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 03/06/2019
ms.openlocfilehash: 25a76133bfecfb08c1e413efea93f1466926c533
ms.sourcegitcommit: dd1a9f38c69954f15ff5c166e456fda37ae1cdf2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57569209"
---
# <a name="resource-limits-for-single-databases-using-the-dtu-based-purchasing-model"></a>Resourcelimieten voor individuele databases met behulp van het op DTU gebaseerde aankoopmodel

In dit artikel bevat de gedetailleerde resourcelimieten voor individuele databases van Azure SQL Database met behulp van het op DTU gebaseerde aankoopmodel.

Zie voor DTU gebaseerde aankopen model resourcelimieten voor elastische pools, [DTU gebaseerde resourcelimieten - elastische pools](sql-database-vcore-resource-limits-elastic-pools.md). Zie voor vCore gebaseerde resourcelimieten [vCore gebaseerde resourcelimieten - individuele databases](sql-database-vcore-resource-limits-single-databases.md) en [vCore gebaseerde resourcelimieten - elastische pools](sql-database-vcore-resource-limits-elastic-pools.md). Zie voor meer informatie over de verschillende modellen aankopen [aanschaffen van modellen en service-lagen](sql-database-purchase-models.md).

> [!IMPORTANT]
> In sommige gevallen is het wellicht voor het verkleinen van een database voor het vrijmaken van ongebruikte ruimte. Zie voor meer informatie, [bestandsruimte in Azure SQL Database beheren](sql-database-file-space-management.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="single-database-storage-sizes-and-compute-sizes"></a>Individuele database: Opslaggrootte en compute-grootten

In de volgende tabellen geven de beschikbare resources voor een individuele database op elke servicelaag en compute-grootte. U kunt de servicelaag, rekencapaciteit en hoeveelheid opslagruimte voor het gebruik van een individuele database instellen de [Azure-portal](sql-database-single-databases-manage.md#manage-an-existing-sql-database-server), [Transact-SQL](sql-database-single-databases-manage.md#transact-sql-manage-sql-database-servers-and-single-databases), [PowerShell](sql-database-single-databases-manage.md#powershell-manage-sql-database-servers-and-single-databases), wordt de [ Azure CLI](sql-database-single-databases-manage.md#azure-cli-manage-sql-database-servers-and-single-databases), of de [REST-API](sql-database-single-databases-manage.md#rest-api-manage-sql-database-servers-and-single-databases).

> [!IMPORTANT]
> Zie voor richtlijnen en overwegingen bij schalen, [een individuele database schalen](sql-database-single-database-scale.md)

### <a name="basic-service-tier"></a>Basisservicelaag

| **COMPUTE-grootte** | **Basic** |
| :--- | --: |
| Maximaal aantal DTU's | 5 |
| Inbegrepen opslag (GB) | 2 |
| Maximale opslagopties (GB) | 2 |
| Maximale OLTP-opslag in het geheugen (GB) |N/A |
| Maximaal aantal gelijktijdige werknemers (aanvragen) | 30 |
| Maximaal aantal gelijktijdige sessies | 300 |
|||

### <a name="standard-service-tier"></a>Standaardservicelaag

| **COMPUTE-grootte** | **S0** | **S1** | **S2** | **S3** |
| :--- |---:| ---:|---:|---:|
| Maximaal aantal DTU's | 10 | 20 | 50 | 100 |
| Inbegrepen opslag (GB) | 250 | 250 | 250 | 250 |
| Maximale opslagopties (GB) | 250 | 250 | 250 | 250, 500, 750, 1024 |
| Maximale OLTP-opslag in het geheugen (GB) | N/A | N/A | N/A | N/A |
| Maximaal aantal gelijktijdige werknemers (aanvragen)| 60 | 90 | 120 | 200 |
| Maximaal aantal gelijktijdige sessies |600 | 900 | 1200 | 2400 |
||||||

### <a name="standard-service-tier-continued"></a>Standard-servicelaag (vervolg)

| **COMPUTE-grootte** | **S4** | **S6** | **S7** | **S9** | **S12** |
| :--- |---:| ---:|---:|---:|---:|
| Maximaal aantal DTU's | 200 | 400 | 800 | 1600 | 3000 |
| Inbegrepen opslag (GB) | 250 | 250 | 250 | 250 | 250 |
| Maximale opslagopties (GB) | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 |
| Maximale OLTP-opslag in het geheugen (GB) | N/A | N/A | N/A | N/A |N/A |
| Maximaal aantal gelijktijdige werknemers (aanvragen)| 400 | 800 | 1600 | 3200 |6000 |
| Maximaal aantal gelijktijdige sessies |4800 | 9600 | 19200 | 30.000 |30.000 |
|||||||

### <a name="premium-service-tier"></a>Premium servicelaag

| **COMPUTE-grootte** | **P1** | **P2** | **P4** | **P6** | **P11** | **P15** |
| :--- |---:|---:|---:|---:|---:|---:|
| Maximaal aantal DTU's | 125 | 250 | 500 | 1000 | 1750 | 4000 |
| Inbegrepen opslag (GB) | 500 | 500 | 500 | 500 | 4096 | 4096 |
| Maximale opslagopties (GB) | 500, 750, 1024 | 500, 750, 1024 | 500, 750, 1024 | 500, 750, 1024 | 4096 | 4096 |
| Maximale OLTP-opslag in het geheugen (GB) | 1 | 2 | 4 | 8 | 14 | 32 |
| Maximaal aantal gelijktijdige werknemers (aanvragen)| 200 | 400 | 800 | 1600 | 2400 | 6400 |
| Maximaal aantal gelijktijdige sessies | 30.000 | 30.000 | 30.000 | 30.000 | 30.000 | 30.000 |
|||||||

> [!IMPORTANT]
> Voor de Premium-laag is er meer dan 1 TB aan opslagruimte beschikbaar in alle regio's, met uitzondering van: China - oost, China - noord, Duitsland - centraal, Duitsland - noordoost, US - west-centraal, US - DoD-regio's en US Government - centraal. In deze regio’s is de maximale opslagruimte in de Premium-laag beperkt tot 1 TB.  Raadpleeg [P11-P15 huidige beperkingen](#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb) voor meer informatie.  

## <a name="next-steps"></a>Volgende stappen

- Zie voor vCore resourcelimieten voor een individuele database, [resourcelimieten voor individuele databases met behulp van de vCore-aanschafmodel](sql-database-vcore-resource-limits-single-databases.md)
- Zie voor vCore resourcelimieten voor elastische pools, [resourcelimieten voor elastische pools met behulp van de vCore-aanschafmodel](sql-database-vcore-resource-limits-elastic-pools.md)
- Zie voor DTU resource-limieten voor elastische pools, [resourcelimieten voor elastische pools met behulp van het op DTU gebaseerde aankoopmodel](sql-database-dtu-resource-limits-elastic-pools.md)
- Zie voor resourcelimieten voor beheerde exemplaren, [beheerd exemplaar resourcelimieten](sql-database-managed-instance-resource-limits.md).
- Zie voor meer informatie over algemene Azure-limieten [Azure-abonnement en Servicelimieten, quotums en beperkingen](../azure-subscription-service-limits.md).
- Zie voor meer informatie over de resourcelimieten op een databaseserver [overzicht van resourcelimieten voor een SQL-databaseserver](sql-database-resource-limits-database-server.md) voor informatie over de beperkingen op het niveau van de server en -abonnement.