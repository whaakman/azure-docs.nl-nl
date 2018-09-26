---
title: Azure SQL Database-service - vCore | Microsoft Docs
description: Het op vCore gebaseerde aankoopmodel kunt u onafhankelijk van elkaar schalen reken- en opslagresources, overeenkomen met de on-premises prestaties en prijs te optimaliseren.
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: sashan, moslake
manager: craigg
ms.date: 09/14/2018
ms.openlocfilehash: 8947a34f43f09281712c0e211c3dc6b8db9da6b3
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47160669"
---
# <a name="choosing-a-vcore-service-tier-compute-memory-storage-and-io-resources"></a>Een vCore-servicelaag kiezen, Reken-, geheugen, opslag- en i/o-resources

Het op vCore gebaseerde aankoopmodel kunt u onafhankelijk van elkaar schalen reken- en opslagresources, overeenkomen met de on-premises prestaties en prijs te optimaliseren. Ook kunt u generatie van de hardware kiezen:
- Gen 4 - maximaal 24 logische CPU's gebaseerd op Intel E5-2673 v3 (Haswell) processors van 2,4 GHz, vCore = 1 PP (fysieke kernen), 7 GB per kern, SSD aangesloten
- Gen 5 - maximaal 80 logische CPU's die zijn gebaseerd op Intel E5-2673 v4 (Broadwell) processors van 2,3 GHz, vCore = 1 LP (hyper-thread), 5.5. GB per kern, snelle eNVM SSD

vCore-model kunt u gebruiken [Azure Hybrid Use Benefit voor SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md) te krijgen van de kosten te besparen.

## <a name="service-tier-characteristics"></a>Service tier kenmerken

Het vCore-model biedt twee Servicelagen voor algemeen gebruik en bedrijfskritiek. Service-lagen van elkaar worden onderscheiden door een bereik van de compute-grootten, ontwerp voor hoge beschikbaarheid, foutisolatie, soorten opslag en i/o-bereik. De klant moet afzonderlijk configureren voor de vereiste periode voor de opslag en de bewaarperiode voor back-ups.

De volgende tabel kunt u weten wat de verschillen tussen deze twee lagen:

||**Algemeen gebruik**|**Bedrijfskritiek**|**Zeer grootschalige (preview)**|
|---|---|---|---|
|Ideaal voor|Meeste zakelijke workloads. Aanbiedingen budget documentgeoriënteerde uitgebalanceerde en schaalbare Computing- en opslagopties.|Zakelijke toepassingen die snelle I/O vereisen. Maakt gebruik van verschillende geïsoleerde replica's voor de hoogste mate van flexibiliteit wat storingen betreft.|De meeste zakelijke workloads met uiterst schaalbare opslag en leesschaal vereisten|
|Compute|Gen4: 1-24 vCore<br/>Gen5: 1 en 80 vCore|Gen4: 1-24 vCore<br/>Gen5: 1 en 80 vCore|Gen4: 1-24 vCore<br/>Gen5: 1 en 80 vCore|
|Geheugen|Gen4: 7 GB per kern<br>Gen5: 5.5 GB per kern | Gen4: 7 GB per kern<br>Gen5: 5.5 GB per kern |Gen4: 7 GB per kern<br>Gen5: 5.5 GB per kern|
|Storage|[Externe premiumopslag](../virtual-machines/windows/premium-storage.md),<br/>Individuele database: 5 GB – 4 TB<br/>Beheerd exemplaar: 32 GB - 8 TB |Lokale SSD-opslag<br/>Individuele database: 5 GB – 4 TB<br/>Beheerd exemplaar: 32 GB - 4 TB |Flexibele, auto-opslag naar behoefte worden uitgebreid. Ondersteunt maximaal 100 TB aan opslag en daarbuiten. Lokale SSD-opslag voor het lokale cachegeheugen van toepassingen en opslag van lokale gegevens. Externe opslag in Azure als laatste gegevensopslag op lange termijn. |
|I/o-doorvoer (bij benadering)|Individuele database: 500 IOP's per vCore met 7000 maximale IOPS</br>Beheerd exemplaar: Afhankelijk [bestandsgrootte](../virtual-machines/windows/premium-storage-performance.md#premium-storage-disk-sizes)|5000 IOP's per kern met 200.000 maximale IOPS|NOG TE BEPALEN|
|Beschikbaarheid|1 replica, geen lees-schaal|3 replica's, 1 [leesschaal replica](sql-database-read-scale-out.md),<br/>zone-redundante HA|?|
|Back-ups|[RA-GRS](../storage/common/storage-designing-ha-apps-with-ragrs.md), 7 en 35 dagen (7 dagen standaard)|[RA-GRS](../storage/common/storage-designing-ha-apps-with-ragrs.md), 7 en 35 dagen (7 dagen standaard)|back-up op basis van een momentopname in Azure, externe opslag en herstelt u deze momentopnamen gebruiken voor snel herstel. Back-ups zijn onmiddellijk en niet van invloed op de i/o-prestaties van de rekencapaciteit. Herstelbewerkingen zijn zeer snel en niet van de grootte van bewerkingen voor gegevens (in minuten niet uren/dagen).|
|In het geheugen|Niet ondersteund|Ondersteund|Niet ondersteund|
|||

Zie voor meer informatie, [vCore resourcelimieten in één database](sql-database-vcore-resource-limits-single-databases.md) en [resourcelimieten vCore in het beheerde exemplaar](sql-database-managed-instance.md#vcore-based-purchasing-model). 

> [!IMPORTANT]
> Als u minder dan één vCore rekencapaciteit nodig hebt, gebruikt u het op DTU gebaseerde aankoopmodel.

Zie [Veelgestelde vragen over SQL-Database](sql-database-faq.md) voor antwoorden op veelgestelde vragen. 

## <a name="storage-considerations"></a>Opslagoverwegingen

### <a name="general-purpose-and-business-critical-service-tiers"></a>Algemeen gebruik en bedrijfskritiek service-lagen
Overweeg de volgende:
- De toegewezen opslag wordt gebruikt door de gegevensbestanden (MDF) en logboekbestanden bestanden (LDF).
- Elke individuele database compute-grootte ondersteunt een maximale databasegrootte, met een maximale standaardgrootte van 32 GB.
- Wanneer u de grootte van de vereiste één database (grootte van het MDF) configureert, wordt 30% van de extra opslag automatisch toegevoegd ter ondersteuning van LDF
- Maximale grootte in het beheerde exemplaar moet worden opgegeven in veelvouden van 32 GB.
- U kunt de grootte van een individuele database tussen 10 GB en de maximale ondersteunde selecteren
 - Voor Standard-opslag, vergroten of verkleinen in stappen van 10 GB
 - Voor Premium-opslag, vergroten of verkleinen in stappen van 250 GB
- In de categorie Algemeen gebruik-service `tempdb` maakt gebruik van een gekoppelde SSD en deze kosten voor opslag is opgenomen in het vCore-prijs.
- In de laag bedrijfskritiek service `tempdb` shares de gekoppelde SSD met de MDF en LDF-bestanden en opslagkosten van de tempDB is opgenomen in het vCore-prijs.

> [!IMPORTANT]
> De totale opslag toegewezen voor MDF en LDF in rekening worden gebracht.

Voor het controleren van de huidige totale grootte van het MDF en LDF gebruikt [sp_spaceused](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-spaceused-transact-sql). Voor het controleren van de huidige grootte van de afzonderlijke MDF en LDF-bestanden gebruikt [sys.database_files](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-database-files-transact-sql).

> [!IMPORTANT]
> In sommige gevallen is het wellicht voor het verkleinen van een database voor het vrijmaken van ongebruikte ruimte. Zie voor meer informatie, [bestandsruimte in Azure SQL Database beheren](sql-database-file-space-management.md).

### <a name="hyperscale-service-tier-preview"></a>Zeer grootschalige servicelaag (preview)

Opslag wordt automatisch beheerd voor grootschalige database. Opslag neemt toe naarmate er nodig is. "Oneindige aanmelden" opslag op snel Azure premium storage SSD's met geen frequente logboekafkapping die nodig zijn.

## <a name="backups-and-storage"></a>Back-ups en opslag

### <a name="general-purpose-and-business-critical-service-tiers"></a>Algemeen gebruik en bedrijfskritiek service-lagen

Opslag voor back-ups ter ondersteuning van het punt in tijd herstellen (PITR) is toegewezen en [lange termijn Retention (LTR)](sql-database-long-term-retention.md) mogelijkheden van SQL Database. Deze opslag is toegewezen afzonderlijk voor elke database en wordt gefactureerd als twee afzonderlijke database kosten in rekening gebracht. 

- **PITR**: afzonderlijke databaseback-ups worden gekopieerd naar [RA-GRS-opslag](../storage/common/storage-designing-ha-apps-with-ragrs.md) worden automatisch. De grootte van de dynamisch wordt verhoogd wanneer de nieuwe back-ups worden gemaakt.  De opslag wordt gebruikt voor wekelijkse volledige back-ups, dagelijkse differentiële back-ups en back-ups van transactielogboeken die om de vijf minuten worden gekopieerd. Het opslagverbruik, is afhankelijk van de wijzigingssnelheid van de database en de bewaarperiode. U kunt een afzonderlijke bewaarperiode voor elke database tussen 7 en 35 dagen configureren. Een opslagruimte ter gelijk zijn aan 1 x de grootte is opgegeven zonder extra kosten. Voor de meeste databases is dit bedrag voldoende voor het opslaan van zeven dagen aan back-ups.
- **LTR**: SQL Database biedt de mogelijkheid om langetermijnretentie voor volledige back-ups configureren voor maximaal 10 jaar. Als LTR-beleid is ingeschakeld, wordt deze back-ups worden opgeslagen in de RA-GRS-opslag automatisch, maar u kunt bepalen hoe vaak de back-ups worden gekopieerd. Om te voldoen aan verschillende nalevingsvereiste, kunt u verschillende bewaartermijnen voor wekelijkse, maandelijkse en/of jaarlijkse back-ups. Deze configuratie wordt gedefinieerd hoeveel opslagruimte wordt gebruikt voor het LTR-back-ups. Om in te schatten van de kosten voor LTR-opslag kunt u de LTR-prijscalculator. Zie [Langetermijnretentie](sql-database-long-term-retention.md) voor meer informatie.

### <a name="hyperscale-service-tier-preview"></a>Zeer grootschalige servicelaag (preview)

back-up op basis van een momentopname in Azure, externe opslag en herstelt u deze momentopnamen gebruiken voor snel herstel. Back-ups zijn onmiddellijk en niet van invloed op de i/o-prestaties van de rekencapaciteit. Herstelbewerkingen zijn zeer snel en niet van de grootte van bewerkingen voor gegevens (in minuten niet uren/dagen).

## <a name="azure-hybrid-use-benefit"></a>Azure Hybrid Use Benefit

In het op vCore gebaseerde aankoopmodel, kunt u uw bestaande licenties voor gereduceerde tarieven voor SQL-Database met behulp van exchange de [Azure Hybrid Use Benefit voor SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md). Dit voordeel van Azure kunt u uw on-premises SQL Server-licenties gebruiken tot 30% besparen op Azure SQL Database met behulp van uw on-premises SQL Server-licenties met Software Assurance.

![prijzen](./media/sql-database-service-tiers/pricing.png)

## <a name="migration-from-dtu-model-to-vcore-model"></a>Migratie van DTU-model naar het vCore-model

### <a name="migration-of-single-databases-with-geo-replication-links"></a>Migratie van afzonderlijke databases met geo-replicatiekoppelingen

Migreren naar uit op basis van DTU-model voor het vcores-model is vergelijkbaar met het upgraden of downgraden van de relaties geo-replicatie tussen Standard en Premium-databases. Het is niet vereist geo-replicatie, maar de gebruiker beëindigd, moet de sequentiëren van regels naleven. Wanneer een upgrade uitvoert, moet u eerst de secundaire database upgraden en werk vervolgens de primaire. Wanneer de Downgrade uitvoert, de volgorde omgekeerd: u moet eerst downgraden van de primaire database en vervolgens gebruik maken van de secundaire server. 

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

### <a name="using-database-copy-to-convert-a-dtu-based-database-to-a-vcore-based-database"></a>Met behulp van database-exemplaar een database op basis van DTU te converteren naar een op vCore gebaseerde database.

U kunt een database met een grootte op basis van DTU compute kopiëren naar een database met een grootte vCore gebaseerde computercapaciteit zonder beperkingen of speciale combineren, zolang de doelgrootte berekenen biedt ondersteuning voor de maximale grootte van de brondatabase. Dit is omdat het database-exemplaar wordt gemaakt van een momentopname van gegevens vanaf de begintijd van de kopieerbewerking en geen synchronisatie van gegevens tussen de bron en het doel voert. 

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over specifieke-grootten en opties voor opslag beschikbaar voor individuele database Reken, [SQL Database vCore gebaseerde resourcelimieten voor individuele databases](sql-database-vcore-resource-limits-single-databases.md#single-database-storage-sizes-and-compute-sizes)
- Voor meer informatie over specifieke grootten COMPUTE en opties voor opslag beschikbaar voor elastische pools Zie [SQL Database vCore gebaseerde resourcelimieten voor elastische pools](sql-database-vcore-resource-limits-elastic-pools.md#elastic-pool-storage-sizes-and-compute-sizes).
