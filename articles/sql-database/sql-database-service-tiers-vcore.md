---
title: Azure SQL Database-service - vCore | Microsoft Docs
description: Het op vCore gebaseerde aankoopmodel kunt u onafhankelijk van elkaar schalen reken- en opslagresources, overeenkomen met de on-premises prestaties en prijs te optimaliseren.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: sashan, moslake, carlrab
manager: craigg
ms.date: 06/26/2019
ms.openlocfilehash: e9d1ce3bcd3bf958be0a7837e8416300af03f5a2
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67449736"
---
# <a name="choose-among-the-vcore-service-tiers-and-migrate-from-the-dtu-service-tiers"></a>Kies een van de vCore-servicelaag en het migreren van de DTU-Servicelagen

De virtuele kern (vCore)-gebaseerde aankoopmodel kunt u onafhankelijk van elkaar schalen reken- en opslagresources, overeenkomen met de on-premises prestaties en prijs te optimaliseren. Ook kunt u de generatie van de hardware kiezen:

- **Gen4**: Maximaal 24 logische CPU's die zijn gebaseerd op Intel E5-2673 v3-processors vCore-processors 2,4 GHz (Haswell) = 1 PP (fysieke kernen), 7 GB per kern, SSD aangesloten
- **Gen5**: Maximaal 80 logische CPU's die zijn gebaseerd op Intel E5-2673 v4-processors vCore-processors 2,3 GHz (Broadwell) = 1 LP (hyper-thread), 5.1 GB per kern, snelle eNVM SSD

Gen4 hardware biedt aanzienlijk meer geheugen per vCore. Gen5 hardware kunt u echter veel hoger rekenresources kan worden uitgebreid.

> [!IMPORTANT]
> Nieuwe Gen4 databases worden niet meer ondersteund in de regio AustraliaEast.
> [!NOTE]
> Zie voor meer informatie over het op DTU gebaseerde Servicelagen [Servicelagen voor het op DTU gebaseerde aankoopmodel](sql-database-service-tiers-dtu.md). Zie voor meer informatie over de verschillen tussen de Servicelagen voor het op DTU gebaseerde en aankopen modellen op vCore gebaseerde [modellen aanschaffen van Azure SQL Database](sql-database-purchase-models.md).

## <a name="service-tier-characteristics"></a>Kenmerken van de service-laag

Het vCore-aanschafmodel biedt drie Servicelagen: algemeen gebruik, flexibele en bedrijfskritiek. Deze Servicelagen van elkaar worden onderscheiden door een bereik van de compute-grootten, hoge beschikbaarheid ontwerpen, foutisolatie methoden, typen en -grootten van opslag en i/o-bereiken.

U moet de vereiste periode voor de opslag en de bewaarperiode voor back-ups afzonderlijk configureren. Aan de back-up-bewaarperiode instellen, opent u Azure portal, gaat u naar de server (niet de database) en ga vervolgens naar **back-ups beheren** > **beleid configureren**  >   **Punt In tijd herstel configuratie** > **7 en 35 dagen**.

De volgende tabel bevat uitleg over de verschillen tussen de drie lagen:

||**Algemeen doel**|**Bedrijfskritiek**|**Zeer grootschalige**|
|---|---|---|---|
|Ideaal voor|Meeste zakelijke workloads. Aanbiedingen budget gebaseerde, uitgebalanceerde en schaalbare Computing- en opslagopties.|Zakelijke toepassingen met hoge i/o-vereisten. Biedt de hoogste herstelmogelijkheden bij fouten met behulp van verschillende geïsoleerde replica's.|De meeste zakelijke workloads met uiterst schaalbare opslag en lees-en schaalvereisten.|
|Compute|**Compute ingericht**:<br/>Gen4: 1-24 vCores<br/>Gen5: 2 tot 80 vCores<br/>**Serverless Computing**:<br/>Gen5: 0,5 - 4 vCores|**Compute ingericht**:<br/>Gen4: 1-24 vCores<br/>Gen5: 2 tot 80 vCores|**Compute ingericht**:<br/>Gen4: 1-24 vCores<br/>Gen5: 2 tot 80 vCores|
|Geheugen|**Compute ingericht**:<br/>Gen4: 7 GB per vCore<br/>Gen5: 5.1 GB per vCore<br/>**Serverless Computing**:<br/>Gen5: 3 GB per vCore|**Compute ingericht**:<br/>Gen4: 7 GB per vCore<br/>Gen5: 5.1 GB per vCore |**Compute ingericht**:<br/>Gen4: 7 GB per vCore<br/>Gen5: 5.1 GB per vCore|
|Opslag|Maakt gebruik van externe opslag.<br/>**Individuele database ingericht compute**:<br/>5 GB – 4 TB<br/>**Individuele database serverless Computing**:<br/>5 GB - 1 TB<br/>**Beheerd exemplaar**: 32 GB - 8 TB |Maakt gebruik van lokale SSD-opslag.<br/>**Individuele database ingericht compute**:<br/>5 GB – 4 TB<br/>**Beheerd exemplaar**:<br/>32 GB - 4 TB |Flexibele autogrow van storage naar behoefte. Biedt ondersteuning voor maximaal 100 TB aan opslag. Maakt gebruik van lokale SSD-opslag voor de buffergroep van lokale cache en de opslag van lokale gegevens. Maakt gebruik van Azure externe opslag als laatste gegevensopslag op lange termijn. |
|I/o-doorvoer (bij benadering)|**Individuele database**: 500 IOP's per vCore met 7000 maximale IOPS.<br/>**Beheerd exemplaar**: Afhankelijk van [bestandsgrootte](../virtual-machines/windows/premium-storage-performance.md#premium-storage-disk-sizes).|5000 IOP's per kern met 200.000 maximale IOPS|Zeer grootschalige is een architectuur met meerdere lagen met caching op meerdere niveaus. Effectieve IOP's afhankelijk van de werkbelasting.|
|Beschikbaarheid|1 replica, geen leesschaal replica 's|3 replica's, 1 [leesschaal replica](sql-database-read-scale-out.md),<br/>zone-redundante hoge beschikbaarheid (HA)|1 replica voor lezen / schrijven, plus 0-4 [leesschaal replica's](sql-database-read-scale-out.md)|
|Back-ups|[Geo-redundante opslag met leestoegang (RA-GRS)](../storage/common/storage-designing-ha-apps-with-ragrs.md), 7 en 35 dagen (7 dagen standaard)|[RA-GRS](../storage/common/storage-designing-ha-apps-with-ragrs.md), 7 en 35 dagen (7 dagen standaard)|Op basis van een momentopname van back-ups in Azure, externe opslag. Hiermee herstelt u deze momentopnamen gebruiken voor snel herstel. Back-ups zijn onmiddellijk en niet van invloed zijn op compute i/o-prestaties. Herstelbewerkingen zijn snel en niet een bewerking van de grootte van gegevens (nemen minuten in plaats van uren of dagen).|
|In het geheugen|Niet ondersteund|Ondersteund|Niet ondersteund|
|||

> [!NOTE]
> U krijgt een gratis Azure SQL-database op de basis-servicelaag in combinatie met een gratis Azure-account. Zie voor meer informatie, [maken van een beheerde clouddatabase met uw gratis Azure-account](https://azure.microsoft.com/free/services/sql-database/).

- Zie voor meer informatie over de resourcelimieten vCore [resourcelimieten vCore in een individuele database](sql-database-vcore-resource-limits-single-databases.md) en [resourcelimieten vCore in een beheerd exemplaar](sql-database-managed-instance.md#vcore-based-purchasing-model).
- Zie voor meer informatie over het algemeen gebruik en de bedrijfstiers van kritieke- [Servicelagen voor algemeen gebruik en bedrijfskritiek](sql-database-service-tiers-general-purpose-business-critical.md).
- Zie voor meer informatie over de grootschalige service tier in het op vCore gebaseerde aankoopmodel [grootschalige servicelaag](sql-database-service-tier-hyperscale.md).  

## <a name="azure-hybrid-benefit"></a>Azure Hybrid Benefit

In de ingerichte Computing-laag van de vCore gebaseerde aankoopmodel kunt u uw bestaande licenties voor gereduceerde tarieven voor SQL-Database met behulp van exchange [Azure Hybrid Benefit voor SQL Server](https://azure.microsoft.com/pricing/hybrid-benefit/). Dit voordeel van Azure kunt u tot wel 30% besparen op Azure SQL Database met behulp van uw on-premises SQL Server-licenties met Software Assurance.

![Prijzen](./media/sql-database-service-tiers/pricing.png)

Met Azure Hybrid Benefit kunt u betaalt alleen voor de onderliggende Azure-infrastructuur met behulp van uw bestaande SQL Server-licentie voor de SQL database-engine zelf (Basisrekentarief prijs) of u kunt betalen voor de onderliggende infrastructuur en de SQL-Server licentie (-prijs voor inbegrepen licentie).

U kunt kiezen of uw licentiemodel wijzigen met behulp van de Azure-portal of met behulp van een van de volgende API's:

- Instellen of het licentietype bijwerken met behulp van PowerShell:

  - [New-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabase)
  - [Set-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabase)
  - [New-AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlinstance)
  - [Set-AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlinstance)

- Instellen of het licentietype bijwerken met behulp van de Azure CLI:

  - [az sql db create](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-create)
  - [az sql db update](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-update)
  - [AZ sql mi maken](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-create)
  - [AZ sql mi-update](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-update)

- Instellen of het licentietype bijwerken met behulp van de REST-API:

  - [Databases - maken of bijwerken](https://docs.microsoft.com/rest/api/sql/databases/createorupdate)
  - [Databases - Update](https://docs.microsoft.com/rest/api/sql/databases/update)
  - [Beheerde exemplaren - maken of bijwerken](https://docs.microsoft.com/rest/api/sql/managedinstances/createorupdate)
  - [Beheerde exemplaren - Update](https://docs.microsoft.com/rest/api/sql/managedinstances/update)

## <a name="migrate-from-the-dtu-based-model-to-the-vcore-based-model"></a>Migreren van het dtu model naar het vCore-model

### <a name="migrate-a-database"></a>Een database migreren

Migreren van een database van het op DTU gebaseerde aankoopmodel naar het vCore-aanschafmodel is vergelijkbaar met het upgraden of downgraden tussen de Servicelagen standard en premium-in het op DTU gebaseerde aankoopmodel.

### <a name="migrate-databases-with-geo-replication-links"></a>Migreren van databases met geo-replicatiekoppelingen

Migreren van het dtu model naar het vCore-aanschafmodel is vergelijkbaar met het upgraden of downgraden van de relaties geo-replicatie tussen databases in de standard en premium-servicelaag. Tijdens de migratie, u hoeft te stoppen geo-replicatie, maar u moet deze regels sequentiëren volgen:

- Wanneer een upgrade uitvoert, moet u eerst de secundaire database upgraden en werk vervolgens de primaire.
- Wanneer de Downgrade uitvoert, de volgorde omgekeerd: u moet eerst downgraden van de primaire database en vervolgens gebruik maken van de secundaire server.

Wanneer u van geo-replicatie tussen twee elastische pools gebruikmaakt, wordt u aangeraden dat u één groep als de primaire en de andere als secundaire opgeven. In dat geval moet u dezelfde sequentiëren richtlijnen gebruiken wanneer u bij het migreren van elastische pools. Hebt u Elastische pools die zowel primaire als secundaire databases bevatten, de groep met het gebruik van de hogere behandelen als de primaire en volgt u de regels sequentiëren dienovereenkomstig.  

De volgende tabel bevat richtlijnen voor bepaalde migratiescenario's:

|Huidige servicelaag|Gewenste servicelaag|Migratietype|Acties van de gebruiker|
|---|---|---|---|
|Standard|Algemeen doel|Laterale|Kunnen in willekeurige volgorde worden gemigreerd, maar moet ervoor zorgen dat juiste vCore sizing *|
|Premium|Bedrijfskritiek|Laterale|Kunnen in willekeurige volgorde worden gemigreerd, maar moet ervoor zorgen dat juiste vCore sizing *|
|Standard|Bedrijfskritiek|Upgraden|Secundaire moeten eerst migreren|
|Bedrijfskritiek|Standard|Downgrade|Primaire moeten eerst migreren|
|Premium|Algemeen doel|Downgrade|Primaire moeten eerst migreren|
|Algemeen doel|Premium|Upgraden|Secundaire moeten eerst migreren|
|Bedrijfskritiek|Algemeen doel|Downgrade|Primaire moeten eerst migreren|
|Algemeen doel|Bedrijfskritiek|Upgraden|Secundaire moeten eerst migreren|
||||

\* Elke 100 dtu's in de standard-laag moeten ten minste 1 vCore en elke 125 dtu's in de premium-laag moeten ten minste 1 vCore.

### <a name="migrate-failover-groups"></a>Failover-groepen migreren

Migratie van failover-groepen met meerdere databases moet afzonderlijke migratie van de primaire en secundaire databases. Tijdens dat proces wordt toepassen de dezelfde overwegingen en sequentiëren regels. Nadat de databases worden geconverteerd naar het vCore-aanschafmodel, wordt de failovergroep blijft van kracht met de dezelfde beleidsinstellingen.

### <a name="create-a-geo-replication-secondary-database"></a>Een secundaire geo-replicatie maken

U kunt een geo-replicatie secundaire database (een geo-secundaire) alleen met behulp van dezelfde servicelaag als u voor de primaire database gebruikt maken. Voor databases met een snelheid voor het hoge log-genereren, wordt u aangeraden de geo-secundaire te maken met de dezelfde compute groot is als de primaire.

Als u een geo-secundaire in de elastische pool voor een enkele primaire database maakt, zorg ervoor dat de `maxVCore` instellen voor de groep komt overeen met de compute-grootte van de primaire database. Als u een geo-secundaire voor een primaire in een andere elastische pool maakt, raden wij aan dat de groepen dezelfde hebben `maxVCore` instellingen.

### <a name="use-database-copy-to-convert-a-dtu-based-database-to-a-vcore-based-database"></a>Database-exemplaar een database op basis van DTU te converteren naar een op vCore gebaseerde database gebruiken

U kunt een database met een grootte op basis van DTU compute kopiëren naar een database met een grootte vCore gebaseerde computercapaciteit zonder beperkingen of speciale combineren, zolang de doelgrootte berekenen biedt ondersteuning voor de maximale grootte van de brondatabase. Het database-exemplaar wordt gemaakt van een momentopname van de gegevens vanaf de begintijd van de kopieerbewerking en gegevens tussen de bron en het doel niet synchroniseren.

## <a name="next-steps"></a>Volgende stappen

- Zie voor de specifieke compute-grootten en opties voor opslag beschikbaar voor individuele databases, [SQL Database vCore gebaseerde resourcelimieten voor individuele databases](sql-database-vcore-resource-limits-single-databases.md).
- Zie voor de specifieke compute-grootten en opties voor opslag beschikbaar voor elastische pools [SQL Database vCore gebaseerde resourcelimieten voor elastische pools](sql-database-vcore-resource-limits-elastic-pools.md#general-purpose-service-tier-storage-sizes-and-compute-sizes).
