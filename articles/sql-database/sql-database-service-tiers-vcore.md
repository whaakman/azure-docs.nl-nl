---
title: Azure SQL Database-service - vCore | Microsoft Docs
description: Het op vCore gebaseerde aankoopmodel kunt u onafhankelijk van elkaar schalen reken- en opslagresources, overeenkomen met de on-premises prestaties en prijs te optimaliseren.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: sashan, moslake
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: 240715fe6d3e8344c15cddf80a23829ec84e81a2
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55511986"
---
# <a name="vcore-service-tiers-azure-hybrid-benefit-and-migration"></a>vCore-servicelaag, Azure Hybrid Benefit en migratie

Het op vCore gebaseerde aankoopmodel kunt u onafhankelijk van elkaar schalen reken- en opslagresources, overeenkomen met de on-premises prestaties en prijs te optimaliseren. Ook kunt u generatie van de hardware kiezen:

- Gen 4 - maximaal 24 logische CPU's gebaseerd op Intel E5-2673 v3 (Haswell) processors van 2,4 GHz, vCore = 1 PP (fysieke kernen), 7 GB per kern, SSD aangesloten
- Gen 5 - maximaal 80 logische CPU's die zijn gebaseerd op Intel E5-2673 v4 (Broadwell) processors van 2,3 GHz, vCore = 1 LP (hyper-thread), 5.1 GB per kern, snelle eNVM SSD

vCore-model kunt u gebruiken [Azure Hybrid Benefit voor SQL Server](https://azure.microsoft.com/pricing/hybrid-benefit/) te krijgen van de kosten te besparen.

> [!NOTE]
> Zie voor meer informatie over Servicelagen op basis van DTU [Servicelagen op basis van DTU](sql-database-service-tiers-dtu.md). Zie voor meer informatie over Servicelagen op basis van DTU en vCore-gebaseerde service-lagen differentiëren [modellen aanschaffen van Azure SQL Database](sql-database-service-tiers.md).

## <a name="service-tier-characteristics"></a>Service tier kenmerken

Het vCore-model biedt drie Servicelagen voor algemeen gebruik, flexibele en bedrijfskritiek. Service-lagen van elkaar worden onderscheiden door een bereik van de compute-grootten, ontwerp voor hoge beschikbaarheid, foutisolatie, typen en grootte van opslag en i/o-bereik. U moet de vereiste periode voor de opslag en de bewaarperiode voor back-ups afzonderlijk configureren. In de Azure-portal, gaat u naar de Server (niet de database) > beheerde back-ups > beleid configureren > punt In tijd herstel configuratie > 7 en 35 dagen.

De volgende tabel kunt u weten wat de verschillen tussen de drie lagen:

||**Algemeen gebruik**|**Bedrijfskritiek**|**Zeer grootschalige (preview)**|
|---|---|---|---|
|Ideaal voor|Meeste zakelijke workloads. Aanbiedingen budget documentgeoriënteerde uitgebalanceerde en schaalbare Computing- en opslagopties.|Zakelijke toepassingen die snelle I/O vereisen. Maakt gebruik van verschillende geïsoleerde replica's voor de hoogste mate van flexibiliteit wat storingen betreft.|De meeste zakelijke workloads met uiterst schaalbare opslag en leesschaal vereisten|
|Compute|Gen4: 1-24 vCore<br/>Gen5: 1 en 80 vCore|Gen4: 1-24 vCore<br/>Gen5: 1 en 80 vCore|Gen4: 1-24 vCore<br/>Gen5: 1 en 80 vCore|
|Geheugen|Gen4: 7 GB per kern<br>Gen5: 5.1 GB per kern | Gen4: 7 GB per kern<br>Gen5: 5.1 GB per kern |Gen4: 7 GB per kern<br>Gen5: 5.1 GB per kern|
|Storage|Maakt gebruik van [premium externe opslag](../virtual-machines/windows/premium-storage.md):<br/>Individuele database: 5 GB – 4 TB<br/>Beheerd exemplaar: 32 GB - 8 TB |Maakt gebruik van lokale SSD-opslag:<br/>Individuele database: 5 GB – 4 TB<br/>Beheerd exemplaar: 32 GB - 4 TB |Flexibele, automatische groei van de opslag naar behoefte. Ondersteunt maximaal 100 TB aan opslag en daarbuiten. Lokale SSD-opslag voor het lokale cachegeheugen van toepassingen en opslag van lokale gegevens. Externe opslag in Azure als laatste gegevensopslag op lange termijn. |
|I/o-doorvoer (bij benadering)|Individuele database: 500 IOP's per vCore met 7000 maximale IOPS</br>Beheerd exemplaar: Afhankelijk van [bestandsgrootte](../virtual-machines/windows/premium-storage-performance.md#premium-storage-disk-sizes)|5000 IOP's per kern met 200.000 maximale IOPS|NOG TE BEPALEN|
|Beschikbaarheid|1 replica, geen lees-schaal|3 replica's, 1 [leesschaal replica](sql-database-read-scale-out.md),<br/>zone-redundante HA|?|
|Back-ups|[RA-GRS](../storage/common/storage-designing-ha-apps-with-ragrs.md), 7 en 35 dagen (7 dagen standaard)|[RA-GRS](../storage/common/storage-designing-ha-apps-with-ragrs.md), 7 en 35 dagen (7 dagen standaard)|back-up op basis van een momentopname in Azure, externe opslag en herstelt u deze momentopnamen gebruiken voor snel herstel. Back-ups zijn onmiddellijk en niet van invloed op de i/o-prestaties van de rekencapaciteit. Herstelbewerkingen zijn zeer snel en niet een grootte van gegevensbewerking (waarbij minuten in plaats van uren of dagen).|
|In het geheugen|Niet ondersteund|Ondersteund|Niet ondersteund|
|||

> [!NOTE]
> U krijgt een gratis Azure SQL-database op de basis-servicelaag in combinatie met een gratis Azure-account om Azure te verkennen. Zie voor meer informatie, [maken van een beheerde clouddatabase met uw gratis Azure-account](https://azure.microsoft.com/free/services/sql-database/).

- Zie voor meer informatie, [vCore resourcelimieten in één database](sql-database-vcore-resource-limits-single-databases.md) en [resourcelimieten vCore in het beheerde exemplaar](sql-database-managed-instance.md#vcore-based-purchasing-model).
- Zie voor meer informatie over de Servicelagen voor algemeen gebruik en bedrijfskritiek [Servicelagen voor algemeen gebruik en bedrijfskritiek](sql-database-service-tiers-general-purpose-business-critical.md).
- Zie voor meer informatie over de servicelaag grootschalige in het op vCore gebaseerde aankoopmodel [grootschalige servicelaag](sql-database-service-tier-hyperscale.md).  

> [!IMPORTANT]
> Als u minder dan één vCore rekencapaciteit nodig hebt, gebruikt u het op DTU gebaseerde aankoopmodel.

Zie [Veelgestelde vragen over SQL-Database](sql-database-faq.md) voor antwoorden op veelgestelde vragen.

## <a name="azure-hybrid-benefit"></a>Azure Hybrid Benefit

In het op vCore gebaseerde aankoopmodel, kunt u uw bestaande licenties voor gereduceerde tarieven voor SQL-Database met behulp van exchange de [Azure Hybrid Benefit voor SQL Server](https://azure.microsoft.com/pricing/hybrid-benefit/). Dit voordeel van Azure kunt u uw on-premises SQL Server-licenties gebruiken tot 30% besparen op Azure SQL Database met behulp van uw on-premises SQL Server-licenties met Software Assurance.

![prijzen](./media/sql-database-service-tiers/pricing.png)

Met de Azure Hybrid Benefit kunt u alleen betaalt voor de onderliggende Azure-infrastructuur met behulp van uw bestaande SQL Server-licentie voor de SQL database-engine zelf (**BasePrice**) of te betalen voor de onderliggende infrastructuur en de SQL Server-licentie (**LicenseIncluded**). U kunt kiezen of wijzig uw licentiemodel met behulp van de Azure-portal of een van de volgende API's.

- Instellen of bijwerken van het licentietype met behulp van PowerShell:

  - [New-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabase):
  - [Set-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql)
  - [New-AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlinstance)
  - [Set-AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql)

- Instellen of bijwerken van het licentietype met behulp van Azure CLI:

  - [az sql db create](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-create)
  - [az sql db update](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-update)
  - [AZ sql mi maken](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-create)
  - [AZ sql mi-update](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-update)

- Instellen of bijwerken van het licentietype met behulp van de REST-API:

  - [Databases - maken of bijwerken](https://docs.microsoft.com/rest/api/sql/databases/createorupdate)
  - [Databases - Update](https://docs.microsoft.com/rest/api/sql/databases/update)
  - [Beheerde exemplaren - maken of bijwerken](https://docs.microsoft.com/rest/api/sql/managedinstances/createorupdate)
  - [Beheerde exemplaren - Update](https://docs.microsoft.com/rest/api/sql/managedinstances/update)

## <a name="migration-from-dtu-model-to-vcore-model"></a>Migratie van DTU-model naar het vCore-model

### <a name="migration-of-a-database"></a>Migratie van een database

Migreren van een database van het op DTU gebaseerde aankoopmodel naar het vCore-aanschafmodel is vergelijkbaar met het upgraden of downgraden tussen Standard en Premium-databases in het op DTU gebaseerde aankoopmodel.

### <a name="migration-of-databases-with-geo-replication-links"></a>Migratie van databases met geo-replicatiekoppelingen

Migreren van het dtu model naar het vCore-model is vergelijkbaar met het upgraden of downgraden van de relaties geo-replicatie tussen Standard en Premium-databases. Het is niet vereist geo-replicatie, maar de gebruiker beëindigd, moet de sequentiëren van regels naleven. Wanneer een upgrade uitvoert, moet u eerst de secundaire database upgraden en werk vervolgens de primaire. Wanneer de Downgrade uitvoert, de volgorde omgekeerd: u moet eerst downgraden van de primaire database en vervolgens gebruik maken van de secundaire server.

Bij het gebruik van geo-replicatie tussen twee elastische pools, is het raadzaam dat u één groep als de primaire en de andere – als de secundaire opgeven. Migreren elastische pools moeten in dat geval gebruikt u dezelfde richtlijnen.  Het is echter dat technisch gezien is het mogelijk dat een elastische pool zowel primaire als secundaire databases bevat. In dit geval, moeten als u wilt migreren naar behoren u behandelen de adresgroep met het gebruik van de hogere als 'primaire' en volg de regels sequentiëren dienovereenkomstig.  

De volgende tabel bevat richtlijnen voor de specifieke migratiescenario's:

|Huidige servicelaag|Gewenste servicelaag|Migratietype|Gebruikerssacties|
|---|---|---|---|
|Standard|Algemeen doel|Laterale|Kunnen in willekeurige volgorde worden gemigreerd, maar moet ervoor zorgen dat een juiste vCore sizing *|
|Premium|Bedrijfskritiek|Laterale|Kunnen in willekeurige volgorde worden gemigreerd, maar moet ervoor zorgen dat juiste vCore sizing *|
|Standard|Bedrijfskritiek|Upgraden|Secundaire moeten eerst migreren|
|Bedrijfskritiek|Standard|Downgrade uitvoeren|Primaire moeten eerst migreren|
|Premium|Algemeen doel|Downgrade uitvoeren|Primaire moeten eerst migreren|
|Algemeen doel|Premium|Upgraden|Secundaire moeten eerst migreren|
|Bedrijfskritiek|Algemeen doel|Downgrade uitvoeren|Primaire moeten eerst migreren|
|Algemeen doel|Bedrijfskritiek|Upgraden|Secundaire moeten eerst migreren|
||||

\* Elke 100 DTU in Standard-laag vereist ten minste 1 vCore en elke 125 DTU in Premium-laag vereist ten minste 1 vCore

### <a name="migration-of-failover-groups"></a>Migratie van failover-groepen

Migratie van failover-groepen met meerdere databases moet afzonderlijke migratie van de primaire en secundaire databases. Tijdens dat proces wordt toepassen de dezelfde overwegingen en sequentiëren regels. Nadat de databases worden geconverteerd naar het vCore-model, wordt de failovergroep blijft van kracht met de dezelfde beleidsinstellingen.

### <a name="creation-of-a-geo-replication-secondary"></a>Het maken van een secundaire geo-replicatie

U kunt alleen een geo-secundaire met behulp van dezelfde servicelaag als de primaire maken. Voor de database met de snelheid voor het genereren van hoge logboek, is het raadzaam dat de secundaire wordt gemaakt met de dezelfde compute groot is als de primaire. Als u een geo-secundaire in de elastische pool voor een enkele primaire database maakt, wordt aanbevolen dat de groep heeft de `maxVCore` instellen die overeenkomt met de compute-grootte van de primaire database. Als u een geo-secundaire in de elastische pool voor een primaire in een andere elastische pool maakt, wordt aanbevolen dat de groepen dezelfde hebben `maxVCore` instellingen

### <a name="using-database-copy-to-convert-a-dtu-based-database-to-a-vcore-based-database"></a>Met behulp van de database-exemplaar een database op basis van DTU te converteren naar een database op basis van vCore

U kunt een database met een grootte op basis van DTU compute kopiëren naar een database met een grootte vCore gebaseerde computercapaciteit zonder beperkingen of speciale combineren, zolang de doelgrootte berekenen biedt ondersteuning voor de maximale grootte van de brondatabase. Het database-exemplaar wordt gemaakt van een momentopname van gegevens vanaf de begintijd van de kopieerbewerking en voert geen synchronisatie van gegevens tussen de bron en het doel.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over specifieke-grootten en opties voor opslag beschikbaar voor individuele database Reken, [SQL Database vCore gebaseerde resourcelimieten voor individuele databases](sql-database-vcore-resource-limits-single-databases.md#general-purpose-service-tier-storage-sizes-and-compute-sizes)
- Zie voor meer informatie over specifieke-grootten en opties voor opslag beschikbaar voor elastische pools Reken, [SQL Database vCore gebaseerde resourcelimieten voor elastische pools](sql-database-vcore-resource-limits-elastic-pools.md#general-purpose-service-tier-storage-sizes-and-compute-sizes).
