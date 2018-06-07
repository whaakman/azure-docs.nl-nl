---
title: Azure SQL Database-service - vCore | Microsoft Docs
description: Meer informatie over Servicelagen voor één en groep databases prestatieniveaus en opslaggrootte opgeven.
services: sql-database
author: CarlRabeler
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 05/14/2018
manager: craigg
ms.author: carlrab
ms.openlocfilehash: d37bf4fd131e700d4f4c3b07c84754b4014ca228
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34648350"
---
# <a name="vcore-based-purchasing-model-for-azure-sql-database-preview"></a>op basis van vCore aankoopmodel voor Azure SQL Database (preview)

[Azure SQL Database](sql-database-technical-overview.md) biedt twee aankopen modellen voor compute, storage en i/o-resources: een DTU-aankoopmodel en een vCore gebaseerde aankoopmodel (preview). De volgende tabel en grafiek vergelijken en contrast van deze twee aankopen modellen.

> [!IMPORTANT]
> Zie voor de DTU gebaseerde aankoopmodel, [aankoopmodel DTU gebaseerde](sql-database-service-tiers-dtu.md).


|**Aankoopmodel**|**Beschrijving**|**Bij beste voor**|
|---|---|---|
|Op basis van het DTU-model|Dit model is gebaseerd op een gecombineerde meting van de berekenings-, opslag- en i/o-resources. Prestatieniveaus worden uitgedrukt in termen van DTU’s (DTU’s: Database Transaction Units) voor enkelvoudige databases en eDTU’s (eDTU’s: elastische DTU’s) voor elastische pools. Zie voor meer informatie over dtu's en edtu's [wat dtu's en edtu's zijn](sql-database-what-is-a-dtu.md)?|Bij beste voor klanten die eenvoudige, vooraf geconfigureerde opties willen.| 
|op basis van vCore model|Dit model kunt u de berekenings- en resources - maximaal 80 vCores, de opslag van gegevens van 4 TB en 200000 IOP's onafhankelijk van elkaar schalen. Ook kunt u Azure hybride voordeel voor SQL Server gebruiken om te krijgen van kosten te besparen.|Bij beste voor flexibiliteit, controle en transparantie value-klanten.|
||||  

![Prijsmodel](./media/sql-database-service-tiers/pricing-model.png)

## <a name="vcore-based-purchasing-model--preview"></a>op basis van vCore aankoopmodel (preview)

Een virtuele core vertegenwoordigt de logische CPU aangeboden met de optie te kiezen tussen generaties van hardware. De vCore gebaseerde aankoopmodel (preview) biedt uw flexibiliteit, de controle, de transparantie van afzonderlijke brongebruik en een eenvoudige manier om te vertalen naar on-premises werklastvereisten naar de cloud. Dit model kunt u scale compute, geheugen en opslag op basis van hun werkbelasting behoeften. In de vCore gebaseerde aankoopmodel (preview), klanten kunnen kiezen tussen algemeen en zakelijke kritische Servicelagen (preview) voor zowel [databases eenmalige](sql-database-single-database-resources.md) en [elastische pools](sql-database-elastic-pool.md). 

Servicelagen van elkaar worden onderscheiden door een bereik van prestatieniveaus ontwerp voor maximale beschikbaarheid, fout isolatie, typen opslag en i/o-bereik. De klant moet de vereiste periode voor opslag en retentie voor back-ups afzonderlijk configureren. Wanneer u het model vCore, individuele databases en elastische pools in aanmerking komen voor up aan 30 procent besparingen met het [Azure hybride gebruik voordeel voor SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md).

In de vCore gebaseerde aankopen model (preview) klanten betalen voor:
- Berekenen (servicelaag + aantal vCores + generatie hardware) *
- Type en de hoeveelheid gegevens en logboekbestanden opslag 
- Aantal IOs **
- Back-up van opslagaccount (RA-GRS) ** 

\* In de eerste public preview de Gen 4 logische CPU's zijn gebaseerd op Intel E5-2673 v3 (Haswell) 2,4 GHz-processors

\*\* Tijdens de preview zijn 7 dagen van de back-ups en IOs gratis

> [!IMPORTANT]
> Reken-, IOs, gegevens en opslag voor toepassingslogboeken per database of elastische pool in rekening worden gebracht. Back-ups opslag wordt in rekening gebracht per elke database. Raadpleeg voor meer informatie van beheerde exemplaar kosten [Azure SQL Database-beheerd instantie](sql-database-managed-instance.md).

> [!IMPORTANT]
> Regio beperkingen: 
>
> De vCore gebaseerde aankoopmodel (preview) is nog niet beschikbaar in Australië-Zuidoost. De Preview-versie is niet beschikbaar in de volgende gebieden: West-Europa, Frankrijk centrale VK Zuid en VK West.
> 

## <a name="choosing-service-tier-compute-memory-storage-and-io-resources"></a>Servicelaag, compute, geheugen, opslag en i/o-resources te kiezen

Converteren naar de vCore gebaseerde aankoopmodel (preview) kunt u onafhankelijk van elkaar schalen berekenings-en opslagbronnen, overeenkomen met de prestaties van de lokale en prijs optimaliseren. Als de database of de elastische groep meer dan 300 DTU conversie naar vCore verbruikt, kan uw kosten verminderen. U kunt met behulp van uw keuze-API of met behulp van de Azure-portal, zonder uitvaltijd converteren. Conversie is echter niet vereist. Als de aankoopmodel DTU voldoet aan uw prestatie- en bedrijfsvereisten, moet u blijven gebruiken. Als u besluit het DTU-model converteren naar vCore-model, moet u het prestatieniveau met behulp van de volgende vuistregel: elke 100 DTU in Standard-laag moet ten minste 1 vCore in algemeen gebruik laag; elke 125 DTU in Premium-laag moet ten minste 1 vCore in kritieke zakelijke laag.

De volgende tabel biedt meer inzicht in de verschillen tussen deze twee lagen:

||**Voor algemene doeleinden**|**Kritieke zakelijke**|
|---|---|---|
|Ideaal voor|De meeste zakelijke workloads. Aanbiedingen budget objectgeoriënteerde taakverdeling en schaalbare berekenings- en -opties.|Zakelijke toepassingen die snelle I/O vereisen. Maakt gebruik van verschillende geïsoleerde replica's voor de hoogste mate van flexibiliteit wat storingen betreft.|
|Compute|1 tot 80 vCore, generatie 4 en 5 van de generatie |1 tot 80 vCore, generatie 4 en 5 van de generatie|
|Geheugen|7 GB per core |7 GB per core |
|Storage|Premium externe opslag, 5 GB – 4 TB|Lokale SSD-opslag, 5 GB – 4 TB|
|I/o-doorvoer (bij benadering)|500 IOP's per vCore met 7000 maximumaantal IOPS|5000 IOP's per core met 200000 maximumaantal IOPS|
|Beschikbaarheid|1 replica, geen lees-schaal|3-replica's, 1 [lezen scale](sql-database-read-scale-out.md), zone redundant HA|
|Back-ups|RA-GRS, 7-35 dagen (7 dagen standaard)|RA-GRS, 7-35 dagen (7 dagen standaard) *|
|In het geheugen|N/A|Ondersteund|
|||

\* Tijdens de preview, worden de back-ups bewaarperiode kan niet worden geconfigureerd en vast is ingesteld op 7 dagen.

> [!IMPORTANT]
> Als u minder dan één vCore van rekencapaciteit nodig hebt, gebruikt u de DTU-aankoopmodel.

Zie voor meer informatie over specifieke prestatieniveaus en opties voor opslag beschikbaar voor één database [SQL-Database op basis van vCore limieten voor individuele databases](sql-database-vcore-resource-limits.md#single-database-storage-sizes-and-performance-levels) en voor de elastische pools Zie [SQL-Database op basis van vCore limieten voor elastische pools](sql-database-vcore-resource-limits.md#elastic-pool-storage-sizes-and-performance-levels).

Zie [SQL Database Veelgestelde vragen over](sql-database-faq.md) voor antwoorden op veelgestelde vragen. 

## <a name="storage-considerations"></a>Opslagoverwegingen

Overweeg de volgende:
- Toegewezen opslag wordt gebruikt door de gegevensbestanden (MDF) en logboekbestanden (LDF)-bestanden.
- Elke prestatieniveau ondersteunt een maximale databasegrootte met een maximale grootte van de standaard van 32 GB.
- Wanneer u de grootte van de vereiste database (grootte van het MDF) configureert, wordt 30% van extra opslagruimte automatisch toegevoegd ter ondersteuning van LDF
- U kunt databasegrootte ligt tussen 10 GB en het ondersteunde maximum selecteren
 - Voor Standard-opslag vergroten of verkleinen in stappen van 10 GB
 - Voor Premium-opslag vergroten of verkleinen in stappen van 250 GB
- In de servicetier algemeen `tempdb` maakt gebruik van een gekoppelde SSD en deze kosten opslag is opgenomen in de prijs vCore.
- In de servicelaag kritieke zakelijke `tempdb` de gekoppelde SSD met de MDF en LDF-bestanden en de kosten tempDB-opslag is opgenomen in de prijs vCore shares.

> [!IMPORTANT]
> Worden in rekening gebracht voor de totale opslagruimte voor MDF en LDF toegewezen.

Gebruik voor het controleren van de huidige totale grootte van het MDF en LDF [sp_spaceused](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-spaceused-transact-sql). Voor het controleren van de huidige grootte van de afzonderlijke MDF en LDF-bestanden gebruiken [sys.database_files](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-database-files-transact-sql).

## <a name="backups-and-storage"></a>Back-ups en opslag

Opslag voor databaseback-ups is toegewezen voor ondersteuning van het punt in tijd herstellen (PITR) en lange termijn bewaren (LTR) mogelijkheden van SQL-Database. Deze opslag is toegewezen afzonderlijk voor elke database en als twee aparte database kosten in rekening gebracht. 

- **PITR**: worden automatisch één database back-ups naar RA-GRS opslag worden gekopieerd. Maximale grootte van het duurt dynamisch omdat de nieuwe back-ups worden gemaakt.  De opslag wordt gebruikt voor wekelijkse volledige back-ups, dagelijkse differentiële back-ups en back-ups van transactielogboeken die om de vijf minuten worden gekopieerd. Het opslagverbruik is afhankelijk van de snelheid van de wijziging van de database en de bewaarperiode. U kunt een afzonderlijke bewaarperiode voor elke database tussen 7 en 35 dagen configureren. Een minimale opslaghoeveelheid gelijk zijn aan 1 x van gegevensgrootte wordt geleverd zonder extra kosten. Dit bedrag is voor de meeste databases voldoende is voor het opslaan van 7 dagen van back-ups.
- **LTR**: SQL-Database biedt de mogelijkheid om lange bewaartermijn van volledige back-ups configureren voor maximaal tien jaar. Als LTR beleid is ingeschakeld, wordt bij deze back-ups worden opgeslagen in de opslag van de RA-GRS automatisch, maar u kunt bepalen hoe vaak de back-ups worden gekopieerd. Om te voldoen aan verschillende nalevingsvereiste, kunt u verschillende bewaartermijnen voor wekelijkse, maandelijkse en/of jaarlijkse back-ups. Deze configuratie definieert hoeveel opslagruimte wordt gebruikt voor de back-ups van links naar rechts. De prijscategorie Rekenmachine LTR kunt u schat de kosten voor opslag van links naar rechts. Zie [Langetermijnretentie](sql-database-long-term-retention.md) voor meer informatie.

## <a name="azure-hybrid-use-benefit"></a>Azure Hybrid Use Benefit

In de vCore gebaseerde aankoopmodel (preview), kunt u uw bestaande licenties voor kortingen voor het gebruik van SQL-Database uitwisselen de [Azure hybride gebruik voordeel voor SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md). Azure voordeel kunt u met uw lokale SQL Server-licenties op te slaan van maximaal 30% op Azure SQL Database met behulp van uw lokale SQL Server-licenties met Software Assurance.

![prijzen](./media/sql-database-service-tiers/pricing.png)

## <a name="migration-of-single-databases-with-geo-replication-links"></a>Migratie van individuele databases met geo-replicatiekoppelingen

Migreren naar uit op basis van het DTU-model op basis van vCore model is vergelijkbaar met een upgrade of een downgrade van de relaties geo-replicatie tussen Standard en Premium-databases. Hoeven niet aan de regels sequentiëren geo-replicatie, maar de gebruiker beëindigd moet voldoen. Bij het bijwerken, moet u eerst de secundaire database upgraden en werk vervolgens de primaire. Wanneer downgraden, de volgorde: u moet eerst de primaire database downgraden en vervolgens gebruik maken van de secundaire. 

Wanneer u geo-replicatie tussen de twee elastische pools, is het raadzaam dat u één groep als de primaire en de andere – als de secundaire aanwijzen. Migreren elastische pools moeten in dat geval dezelfde richtlijnen gebruiken.  Het is echter dat technisch is het mogelijk dat een elastische pool zowel primaire als secundaire databases bevat. In dit geval moeten als u wilt migreren goed u behandelen de groep met het gebruik van de hogere als 'primaire' en volg de regels sequentiëren dienovereenkomstig.  

De volgende tabel bevat richtlijnen voor de specifieke migratiescenario's: 

|Huidige servicelaag|Doel-servicelaag|Migratietype|Gebruikerssacties|
|---|---|---|---|
|Standard|Algemeen doel|Lateral|Kan in elke volgorde migreren, maar moet een juiste vCore sizing *|
|Premium|Bedrijfskritiek|Lateral|Kan in elke volgorde migreren, maar moet ervoor zorgen juiste vCore sizing *|
|Standard|Bedrijfskritiek|Upgraden|Eerst moet secundaire migreren|
|Bedrijfskritiek|Standard|Downgrade uitvoeren|Primaire moet eerst migreren|
|Premium|Algemeen doel|Downgrade uitvoeren|Primaire moet eerst migreren|
|Algemeen doel|Premium|Upgraden|Eerst moet secundaire migreren|
|Bedrijfskritiek|Algemeen doel|Downgrade uitvoeren|Primaire moet eerst migreren|
|Algemeen doel|Bedrijfskritiek|Upgraden|Eerst moet secundaire migreren|
||||

\* Elke 100 DTU in Standard-laag vereist ten minste 1 vCore en elke 125 DTU in de laag Premium vereist ten minste 1 vCore

## <a name="migration-of-failover-groups"></a>Migratie van failover-groepen 

Migreren van failover-groepen met meerdere databases, moeten afzonderlijke migratie van de primaire en secundaire databases. Tijdens dat proces toepassen de dezelfde overwegingen en sequentiëren regels. Nadat u de databases worden geconverteerd naar het model op basis van vCore, blijft de failover-groep van kracht met dezelfde beleidsinstellingen. 

## <a name="creation-of-a-geo-replication-secondary"></a>Het maken van een secundaire geo-replicatie

U kunt alleen een geografisch secundaire database met behulp van de servicelaag dezelfde als de primaire maken. Voor de database met de snelheid voor het genereren van hoge log wordt sterk aanbevolen dat de secundaire is gemaakt met hetzelfde prestatieniveau als het primaire. Als u een secundaire geo-database in de elastische pool voor een enkele primaire database maakt, wordt sterk aanbevolen dat de groep heeft de `maxVCore` instelling die overeenkomt met het prestatieniveau van de primaire database. Als u een secundaire geo-database in de elastische pool voor een primaire in een andere elastische pool maakt, wordt sterk aanbevolen dat de groepen dezelfde hebben `maxVCore` instellingen

## <a name="using-database-copy-to-convert-a-dtu-based-database-to-a-vcore-based-database"></a>Een database op basis van DTU converteren naar een database op basis van vCore via een databasekopie.

U kunt database met een DTU gebaseerde prestatieniveau kopiëren naar een database met een vCore-prestaties niveau zonder beperkingen of speciale sequentiëren zolang het prestatieniveau van het doel de maximale grootte van de brondatabase ondersteunt. Dit is omdat de databasekopie een momentopname van gegevens vanaf de begintijd van de kopieerbewerking maakt en synchroniseren van gegevens tussen de bron en het doel geen uitvoert. 

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over specifieke prestatieniveaus en opslag beschikbare keuzen [resource op basis van SQL Database DTU-limieten](sql-database-dtu-resource-limits.md) en [SQL-Database op basis van vCore limieten](sql-database-vcore-resource-limits.md).
- Zie [SQL Database Veelgestelde vragen over](sql-database-faq.md) voor antwoorden op veelgestelde vragen.
- Meer informatie over [Azure-abonnement en Servicelimieten, quota's en beperkingen](../azure-subscription-service-limits.md)
