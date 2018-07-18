---
title: Azure SQL Database-service - vCore | Microsoft Docs
description: Het op vCore gebaseerde aankoopmodel kunt u onafhankelijk van elkaar schalen reken- en opslagresources, overeenkomen met de on-premises prestaties en prijs te optimaliseren.
services: sql-database
author: CarlRabeler
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 07/16/2018
manager: craigg
ms.author: carlrab
ms.openlocfilehash: d18076486704d5f03acd2253650762c3bd24b0af
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39091489"
---
# <a name="choosing-a-vcore-service-tier-compute-memory-storage-and-io-resources"></a>Een vCore-servicelaag kiezen, Reken-, geheugen, opslag- en i/o-resources

Service-lagen van elkaar worden onderscheiden door een bereik van de prestaties, hoge beschikbaarheid ontwerpen, foutisolatie, soorten opslag en i/o-bereik. De klant moet afzonderlijk configureren voor de vereiste periode voor de opslag en de bewaarperiode voor back-ups. Met het vCore-model, enkelvoudige databases en elastische pools in aanmerking komt voor van 30 procent besparen met de [Azure Hybrid Use Benefit voor SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md).

De volgende tabel kunt u weten wat de verschillen tussen deze twee lagen:

||**Algemeen gebruik**|**Bedrijfskritiek**|
|---|---|---|
|Ideaal voor|Meeste zakelijke workloads. Aanbiedingen budget documentgeoriënteerde uitgebalanceerde en schaalbare Computing- en opslagopties.|Zakelijke toepassingen die snelle I/O vereisen. Maakt gebruik van verschillende geïsoleerde replica's voor de hoogste mate van flexibiliteit wat storingen betreft.|
|Compute|1 en 80 vCore, Gen4 als Gen5 |1 en 80 vCore, Gen4 als Gen5|
|Geheugen|Gen4: 7 GB per kern<br>Gen5: 5.5 GB per kern | Gen4: 7 GB per kern<br>Gen5: 5.5 GB per kern |
|Storage|Premium externe opslag, 5 GB – 4 TB|Lokale SSD-opslag, 5 GB – 4 TB|
|I/o-doorvoer (bij benadering)|500 IOP's per vCore met 7000 maximale IOPS|5000 IOP's per kern met 200000 maximale IOPS|
|Beschikbaarheid|1 replica, geen lees-schaal|3 replica's, 1 [leesschaal](sql-database-read-scale-out.md), zone redundant HA|
|Back-ups|RA-GRS, 7 en 35 dagen (7 dagen standaard)|RA-GRS, 7 en 35 dagen (7 dagen standaard)|
|In het geheugen|N/A|Ondersteund|
|||

> [!IMPORTANT]
> Als u minder dan één vCore rekencapaciteit nodig hebt, gebruikt u het op DTU gebaseerde aankoopmodel.

Zie [Veelgestelde vragen over SQL-Database](sql-database-faq.md) voor antwoorden op veelgestelde vragen. 

## <a name="storage-considerations"></a>Opslagoverwegingen

Overweeg de volgende:
- De toegewezen opslag wordt gebruikt door de gegevensbestanden (MDF) en logboekbestanden bestanden (LDF).
- Elke prestatieniveau biedt ondersteuning voor een maximale databasegrootte, met een maximale standaardgrootte van 32 GB.
- Wanneer u de grootte van de vereiste database (grootte van het MDF) configureert, wordt 30% van de extra opslag automatisch toegevoegd ter ondersteuning van LDF
- U kunt een databasegrootte ligt tussen 10 GB en de maximale ondersteunde selecteren
 - Voor Standard-opslag, vergroten of verkleinen in stappen van 10 GB
 - Voor Premium-opslag, vergroten of verkleinen in stappen van 250 GB
- In de categorie Algemeen gebruik-service `tempdb` maakt gebruik van een gekoppelde SSD en deze kosten voor opslag is opgenomen in het vCore-prijs.
- In de laag bedrijfskritiek service `tempdb` shares de gekoppelde SSD met de MDF en LDF-bestanden en opslagkosten van de tempDB is opgenomen in het vCore-prijs.

> [!IMPORTANT]
> De totale opslag toegewezen voor MDF en LDF in rekening worden gebracht.

Voor het controleren van de huidige totale grootte van het MDF en LDF gebruikt [sp_spaceused](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-spaceused-transact-sql). Voor het controleren van de huidige grootte van de afzonderlijke MDF en LDF-bestanden gebruikt [sys.database_files](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-database-files-transact-sql).

## <a name="backups-and-storage"></a>Back-ups en opslag

Opslag voor back-ups wordt ter ondersteuning van het punt in tijd herstellen (PITR) en de lange termijn Retention (LTR) mogelijkheden van SQL-Database toegewezen. Deze opslag is toegewezen afzonderlijk voor elke database en wordt gefactureerd als twee afzonderlijke database kosten in rekening gebracht. 

- **PITR**: afzonderlijke database back-ups worden gekopieerd naar RA-GRS-opslag worden automatisch. De grootte van de dynamisch wordt verhoogd wanneer de nieuwe back-ups worden gemaakt.  De opslag wordt gebruikt voor wekelijkse volledige back-ups, dagelijkse differentiële back-ups en back-ups van transactielogboeken die om de vijf minuten worden gekopieerd. Het opslagverbruik, is afhankelijk van de wijzigingssnelheid van de database en de bewaarperiode. U kunt een afzonderlijke bewaarperiode voor elke database tussen 7 en 35 dagen configureren. Een opslagruimte ter gelijk zijn aan 1 x de grootte is opgegeven zonder extra kosten. Voor de meeste databases is dit bedrag voldoende voor het opslaan van zeven dagen aan back-ups.
- **LTR**: SQL Database biedt de mogelijkheid om langetermijnretentie voor volledige back-ups configureren voor maximaal 10 jaar. Als LTR-beleid is ingeschakeld, wordt deze back-ups worden opgeslagen in de RA-GRS-opslag automatisch, maar u kunt bepalen hoe vaak de back-ups worden gekopieerd. Om te voldoen aan verschillende nalevingsvereiste, kunt u verschillende bewaartermijnen voor wekelijkse, maandelijkse en/of jaarlijkse back-ups. Deze configuratie wordt gedefinieerd hoeveel opslagruimte wordt gebruikt voor het LTR-back-ups. Om in te schatten van de kosten voor LTR-opslag kunt u de LTR-prijscalculator. Zie [Langetermijnretentie](sql-database-long-term-retention.md) voor meer informatie.

## <a name="azure-hybrid-use-benefit"></a>Azure Hybrid Use Benefit

In het op vCore gebaseerde aankoopmodel, kunt u uw bestaande licenties voor gereduceerde tarieven voor SQL-Database met behulp van exchange de [Azure Hybrid Use Benefit voor SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md). Dit voordeel van Azure kunt u uw on-premises SQL Server-licenties gebruiken tot 30% besparen op Azure SQL Database met behulp van uw on-premises SQL Server-licenties met Software Assurance.

![prijzen](./media/sql-database-service-tiers/pricing.png)

## <a name="migration-of-single-databases-with-geo-replication-links"></a>Migratie van afzonderlijke databases met geo-replicatiekoppelingen

Migreren naar uit op basis van DTU-model voor het vcores-model is vergelijkbaar met het upgraden of downgraden van de relaties geo-replicatie tussen Standard en Premium-databases. Het is niet vereist geo-replicatie, maar de gebruiker beëindigd, moet de sequentiëren van regels naleven. Wanneer een upgrade uitvoert, moet u eerst de secundaire database upgraden en werk vervolgens de primaire. Wanneer de Downgrade uitvoert, de volgorde omgekeerd: u moet eerst downgraden van de primaire database en vervolgens gebruik maken van de secundaire server. 

Bij het gebruik van geo-replicatie tussen twee elastische pools, is het raadzaam dat u één groep als de primaire en de andere – als de secundaire opgeven. Migreren elastische pools moeten in dat geval gebruikt u dezelfde richtlijnen.  Het is echter dat technisch gezien is het mogelijk dat een elastische pool zowel primaire als secundaire databases bevat. In dit geval, moeten als u wilt migreren naar behoren u behandelen de adresgroep met het gebruik van de hogere als 'primaire' en volg de regels sequentiëren dienovereenkomstig.  

De volgende tabel bevat richtlijnen voor de specifieke migratiescenario's: 

|Huidige servicelaag|Gewenste servicelaag|Migratietype|Acties van de gebruiker|
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

## <a name="migration-of-failover-groups"></a>Migratie van failover-groepen 

Migratie van failover-groepen met meerdere databases moet afzonderlijke migratie van de primaire en secundaire databases. Tijdens dat proces wordt toepassen de dezelfde overwegingen en sequentiëren regels. Nadat de databases worden geconverteerd naar het vCore-model, wordt de failovergroep blijft van kracht met de dezelfde beleidsinstellingen. 

## <a name="creation-of-a-geo-replication-secondary"></a>Het maken van een secundaire geo-replicatie

U kunt alleen een geo-secundaire met behulp van dezelfde servicelaag als de primaire maken. Voor de database met de snelheid voor het genereren van hoge logboek, is het raadzaam dat de secundaire server met hetzelfde prestatieniveau als de primaire is gemaakt. Als u een geo-secundaire in de elastische pool voor een enkele primaire database maakt, wordt aanbevolen dat de groep heeft de `maxVCore` instellen die overeenkomt met het prestatieniveau van de primaire database. Als u een geo-secundaire in de elastische pool voor een primaire in een andere elastische pool maakt, wordt aanbevolen dat de groepen dezelfde hebben `maxVCore` instellingen

## <a name="using-database-copy-to-convert-a-dtu-based-database-to-a-vcore-based-database"></a>Met behulp van database-exemplaar een database op basis van DTU te converteren naar een op vCore gebaseerde database.

U kunt een database met een prestatieniveau van DTU gebaseerde kopiëren naar een database met een op vCore gebaseerde prestaties niveau zonder beperkingen of speciale combineren, zolang de doel-prestatieniveau biedt ondersteuning voor de maximale grootte van de brondatabase. Dit is omdat het database-exemplaar wordt gemaakt van een momentopname van gegevens vanaf de begintijd van de kopieerbewerking en geen synchronisatie van gegevens tussen de bron en het doel voert. 

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over specifieke prestatieniveaus en opties voor opslag beschikbaar voor individuele database [SQL Database vCore gebaseerde resourcelimieten voor individuele databases](sql-database-vcore-resource-limits-single-databases.md#single-database-storage-sizes-and-performance-levels)
- Zie voor meer informatie over specifieke prestatieniveaus en opslag beschikbare keuzen voor elastische pools [SQL Database vCore gebaseerde resourcelimieten voor elastische pools](sql-database-vcore-resource-limits-elastic-pools.md#elastic-pool-storage-sizes-and-performance-levels).
