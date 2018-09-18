---
title: Azure SQL Database-service - vCore | Microsoft Docs
description: Het op vCore gebaseerde aankoopmodel kunt u onafhankelijk van elkaar schalen reken- en opslagresources, overeenkomen met de on-premises prestaties en prijs te optimaliseren.
services: sql-database
author: CarlRabeler
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 09/14/2018
manager: craigg
ms.author: carlrab
ms.openlocfilehash: 93b017482006507d616d9125cd17fd2f14389d59
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "45983043"
---
# <a name="choosing-a-vcore-service-tier-compute-memory-storage-and-io-resources"></a>Een vCore-servicelaag kiezen, Reken-, geheugen, opslag- en i/o-resources

Het op vCore gebaseerde aankoopmodel kunt u onafhankelijk van elkaar schalen reken- en opslagresources, overeenkomen met de on-premises prestaties en prijs te optimaliseren. Ook kunt u generatie van de hardware kiezen:
- Gen 4 - maximaal 24 logische CPU's gebaseerd op Intel E5-2673 v3 (Haswell) processors van 2,4 GHz, vCore = 1 PP (fysieke kernen), 7 GB per kern, SSD aangesloten
- Gen 5 - maximaal 80 logische CPU's die zijn gebaseerd op Intel E5-2673 v4 (Broadwell) processors van 2,3 GHz, vCore = 1 LP (hyper-thread), 5.1. GB per kern, snelle eNVM SSD

vCore-model kunt u gebruiken [Azure Hybrid Use Benefit voor SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md) te krijgen van de kosten te besparen.

## <a name="service-tier-characteristics"></a>Service tier kenmerken

Het vCore-model biedt twee Servicelagen voor algemeen gebruik en bedrijfskritiek. Service-lagen van elkaar worden onderscheiden door een bereik van de compute-grootten, ontwerp voor hoge beschikbaarheid, foutisolatie, soorten opslag en i/o-bereik. De klant moet afzonderlijk configureren voor de vereiste periode voor de opslag en de bewaarperiode voor back-ups.

De volgende tabel kunt u weten wat de verschillen tussen deze twee lagen:

||**Algemeen gebruik**|**Bedrijfskritiek**|
|---|---|---|
|Ideaal voor|Meeste zakelijke workloads. Aanbiedingen budget documentgeoriënteerde uitgebalanceerde en schaalbare Computing- en opslagopties.|Zakelijke toepassingen die snelle I/O vereisen. Maakt gebruik van verschillende geïsoleerde replica's voor de hoogste mate van flexibiliteit wat storingen betreft.|
|Compute|Gen4: 1-24 vCore<br/>Gen5: 1 en 80 vCore|Gen4: 1-24 vCore<br/>Gen5: 1 en 80 vCore|
|Geheugen|Gen4: 7 GB per kern<br>Gen5: 5.1 GB per kern | Gen4: 7 GB per kern<br>Gen5: 5.1 GB per kern |
|Storage|[Externe premiumopslag](../virtual-machines/windows/premium-storage.md),<br/>Individuele Database: 5 GB – 4 TB<br/>Beheerd exemplaar: 32 GB - 8 TB |Lokale SSD-opslag<br/>Individuele Database: 5 GB – 1 TB<br/>Beheerd exemplaar: 32 GB - 4 TB |
|I/o-doorvoer (bij benadering)|Individuele Database: 500 IOP's per vCore met 7000 maximale IOPS</br>Beheerd exemplaar: Afhankelijk [bestandsgrootte](../virtual-machines/windows/premium-storage-performance.md#premium-storage-disk-sizes)|5000 IOP's per kern met 200000 maximale IOPS|
|Beschikbaarheid|1 replica, geen lees-schaal|3 replica's, 1 [leesschaal replica](sql-database-read-scale-out.md),<br/>zone-redundante HA|
|Back-ups|[RA-GRS](../storage/common/storage-designing-ha-apps-with-ragrs.md), 7 en 35 dagen (7 dagen standaard)|[RA-GRS](../storage/common/storage-designing-ha-apps-with-ragrs.md), 7 en 35 dagen (7 dagen standaard)|
|In het geheugen|N/A|Ondersteund|
|||

Zie voor meer informatie, [resourcelimieten vCore in de Singelton Database](sql-database-vcore-resource-limits-single-databases.md) en [resourcelimieten vCore in het beheerde exemplaar](sql-database-managed-instance.md#vcore-based-purchasing-model). 

> [!IMPORTANT]
> Als u minder dan één vCore rekencapaciteit nodig hebt, gebruikt u het op DTU gebaseerde aankoopmodel.

Zie [Veelgestelde vragen over SQL-Database](sql-database-faq.md) voor antwoorden op veelgestelde vragen. 

## <a name="storage-considerations"></a>Opslagoverwegingen

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

## <a name="backups-and-storage"></a>Back-ups en opslag

Opslag voor back-ups ter ondersteuning van het punt in tijd herstellen (PITR) is toegewezen en [lange termijn Retention (LTR)](sql-database-long-term-retention.md) mogelijkheden van SQL Database. Deze opslag is toegewezen afzonderlijk voor elke database en wordt gefactureerd als twee afzonderlijke database kosten in rekening gebracht. 

- **PITR**: afzonderlijke databaseback-ups worden gekopieerd naar [RA-GRS-opslag](../storage/common/storage-designing-ha-apps-with-ragrs.md) worden automatisch. De grootte van de dynamisch wordt verhoogd wanneer de nieuwe back-ups worden gemaakt.  De opslag wordt gebruikt voor wekelijkse volledige back-ups, dagelijkse differentiële back-ups en back-ups van transactielogboeken die om de vijf minuten worden gekopieerd. Het opslagverbruik, is afhankelijk van de wijzigingssnelheid van de database en de bewaarperiode. U kunt een afzonderlijke bewaarperiode voor elke database tussen 7 en 35 dagen configureren. Een opslagruimte ter gelijk zijn aan 1 x de grootte is opgegeven zonder extra kosten. Voor de meeste databases is dit bedrag voldoende voor het opslaan van zeven dagen aan back-ups.
- **LTR**: SQL Database biedt de mogelijkheid om langetermijnretentie voor volledige back-ups configureren voor maximaal 10 jaar. Als LTR-beleid is ingeschakeld, wordt deze back-ups worden opgeslagen in de RA-GRS-opslag automatisch, maar u kunt bepalen hoe vaak de back-ups worden gekopieerd. Om te voldoen aan verschillende nalevingsvereiste, kunt u verschillende bewaartermijnen voor wekelijkse, maandelijkse en/of jaarlijkse back-ups. Deze configuratie wordt gedefinieerd hoeveel opslagruimte wordt gebruikt voor het LTR-back-ups. Om in te schatten van de kosten voor LTR-opslag kunt u de LTR-prijscalculator. Zie [Langetermijnretentie](sql-database-long-term-retention.md) voor meer informatie.

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
