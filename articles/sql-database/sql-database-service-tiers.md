---
title: Azure SQL Database-service | Microsoft Docs
description: Meer informatie over Servicelagen voor één en groep databases prestatieniveaus en opslaggrootte opgeven.
services: sql-database
author: CarlRabeler
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: article
ms.date: 04/04/2018
manager: craigg
ms.author: carlrab
ms.openlocfilehash: a4474aec212084006becd02f317dabae6e731d98
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
# <a name="what-are-azure-sql-database-service-tiers"></a>Wat is Azure SQL Database Servicelagen?

[Azure SQL Database](sql-database-technical-overview.md) biedt twee aankopen modellen voor compute, storage en i/o-resources: een DTU-aankoopmodel en een vCore gebaseerde aankoopmodel (preview). De volgende tabel en grafiek vergelijken en contrast van deze twee aankopen modellen.

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

### <a name="choosing-a-service-tier-in-the-dtu-based-purchasing-model"></a>Een servicetier kiezen in de DTU-aankoopmodel

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

### <a name="performance-level-and-storage-size-limits-in-the-dtu-based-purchasing-model"></a>Prestaties niveau en opslaglimieten voor grootte in de DTU-aankoopmodel

Prestatieniveaus worden uitgedrukt in termen van DTU’s (DTU’s: Database Transaction Units) voor enkelvoudige databases en eDTU’s (eDTU’s: elastische DTU’s) voor elastische pools. Zie voor meer informatie over dtu's en edtu's [wat dtu's en edtu's zijn](sql-database-what-is-a-dtu.md)?

#### <a name="single-databases"></a>Individuele databases

||Basic|Standard|Premium|
| :-- | --: | --: | --: | --: |
| Maximale grootte * | 2 GB | 1 TB | 4 TB  | 
| Maximale aantal dtu 's | 5 | 3000 | 4000 | |
||||||

Zie voor meer informatie over specifieke prestatieniveaus en opties voor opslag beschikbaar voor individuele databases [resource op basis van SQL Database DTU-limieten voor individuele databases](sql-database-dtu-resource-limits.md#single-database-storage-sizes-and-performance-levels).

#### <a name="elastic-pools"></a>Pools voor Elastic Database

| | **Basic** | **Standard** | **Premium** | 
| :-- | --: | --: | --: | --: |
| Maximum opslagruimte per database *  | 2 GB | 1 TB | 1 TB | 
| Maximum opslagruimte per groep * | 156 GB | 4 TB | 4 TB | 
| Maximum aantal edtu's per database | 5 | 3000 | 4000 | 
| Maximum aantal edtu's per groep | 1600 | 3000 | 4000 | 
| Maximum aantal databases per groep | 500  | 500 | 100 | 
||||||

> [!IMPORTANT]
> \* Opslagruimten groter dan de hoeveelheid inbegrepen opslagruimte zijn in preview en hiervoor gelden extra kosten. Zie [de prijsinformatie voor SQL Database](https://azure.microsoft.com/pricing/details/sql-database/) voor meer informatie. 
>
> \* In de Premium-laag is momenteel ruim 1 TB opslag beschikbaar voor de volgende regio's: Australië - oost, Australië - zuidoost, Brazilië - zuid, Canada - centraal, Canada - oost, VS - midden, Frankrijk - centraal, Duitsland - centraal, Japan - oost, Japan - west, Korea - centraal, VS Noord-Centraal, Noord-Europa, VS Zuid-Centraal, Zuidoost-Azië, UK - zuid, UK - west, VS - oost2, VS - west, VS (overheid) - Virginia, en West-Europa. Zie [P11-P15: huidige beperkingen](sql-database-dtu-resource-limits.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  
> 

Zie voor meer informatie over specifieke prestatieniveaus en opties voor opslag beschikbaar voor elastische pools [resource op basis van SQL Database DTU-limieten](sql-database-dtu-resource-limits.md#elastic-pool-storage-sizes-and-performance-levels).

## <a name="vcore-based-purchasing-model-preview"></a>op basis van vCore aankoopmodel (preview)

Een virtuele core vertegenwoordigt de logische CPU aangeboden met de optie te kiezen tussen generaties van hardware. De vCore gebaseerde aankoopmodel (preview) biedt uw flexibiliteit, de controle, de transparantie van afzonderlijke brongebruik en een eenvoudige manier om te vertalen naar on-premises werklastvereisten naar de cloud. Dit model kunt u scale compute, geheugen en opslag op basis van hun werkbelasting behoeften. In het aankopen model op basis van vCore klanten kunnen kiezen tussen algemeen en zakelijke kritische Servicelagen (preview) voor zowel [databases eenmalige](sql-database-single-database-resources.md) en [elastische pools](sql-database-elastic-pool.md). 

Servicelagen van elkaar worden onderscheiden door een bereik van prestatieniveaus ontwerp voor maximale beschikbaarheid, fout isolatie, typen opslag en i/o-bereik. De klant moet de vereiste periode voor opslag en retentie voor back-ups afzonderlijk configureren. Wanneer u het model vCore, individuele databases en elastische pools in aanmerking komen voor up aan 30 procent besparingen met het [Azure hybride gebruik voordeel voor SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md).

In de vCore gebaseerde aankopen model klanten betalen voor:
- Berekenen (servicelaag + aantal vCores + generatie hardware) *
- Type en de hoeveelheid gegevens en logboekbestanden opslag 
- Aantal IOs **
- Back-up van opslagaccount (RA-GRS) ** 

\* In de eerste public preview de Gen 4 logische CPU's zijn gebaseerd op Intel E5-2673 v3 (Haswell) 2,4 GHz-processors

\*\* Tijdens de preview zijn 7 dagen van de back-ups en IOs gratis

> [!IMPORTANT]
> Reken-, IOs, gegevens en opslag voor toepassingslogboeken per database of elastische pool in rekening worden gebracht. Back-ups opslag wordt in rekening gebracht per elke database. Raadpleeg voor meer informatie van beheerde exemplaar kosten [Azure SQL Database-beheerd instantie](sql-database-managed-instance.md).

### <a name="choosing-service-tier-compute-memory-storage-and-io-resources"></a>Servicelaag, compute, geheugen, opslag en i/o-resources te kiezen

Converteren naar de vCore gebaseerde aankoopmodel, kunt u onafhankelijk van elkaar schalen berekenings-en opslagbronnen, overeenkomen met de prestaties van de lokale en prijs optimaliseren. Als de database of de elastische groep meer dan 300 DTU conversie naar vCore verbruikt, kan uw kosten verminderen. U kunt met behulp van uw keuze-API of met behulp van de Azure-portal, zonder uitvaltijd converteren. Conversie is echter niet vereist. Als de aankoopmodel DTU voldoet aan uw prestatie- en bedrijfsvereisten, moet u blijven gebruiken. Als u besluit het DTU-model converteren naar vCore-model, moet u het prestatieniveau met behulp van de volgende vuistregel: elke 100 DTU in Standard-laag vereist ten minste 1 vCore en elke 125 DTU in de laag Premium vereist ten minste 1 vCore.

De volgende tabel biedt meer inzicht in de verschillen tussen deze twee lagen:

||**Voor algemene doeleinden**|**Kritieke zakelijke**|
|---|---|---|
|Ideaal voor|De meeste zakelijke workloads. Aanbiedingen budget objectgeoriënteerde taakverdeling en schaalbare berekenings- en -opties.|Zakelijke toepassingen die snelle I/O vereisen. Maakt gebruik van verschillende geïsoleerde replica's voor de hoogste mate van flexibiliteit wat storingen betreft.|
|Compute|1 tot en met 16 vCore|1 tot en met 16 vCore|
|Geheugen|7 GB per core |7 GB per core |
|Storage|Premium externe opslag, 5 GB – 4 TB|Lokale SSD-opslag, 5 GB – 1 TB|
|I/o-doorvoer (bij benadering)|500 IOP's per vCore met 7500 maximumaantal IOPS|5000 IOP's per core|
|Beschikbaarheid|1 replica, geen lees-schaal|3-replica's, 1 [lezen scale](sql-database-read-scale-out.md), zone redundant HA|
|Back-ups|RA-GRS, 7-35 dagen (7 dagen standaard)|RA-GRS, 7-35 dagen (7 dagen standaard) *|
|In het geheugen|N/A|Ondersteund|
|||

\* Tijdens de preview, worden de back-ups bewaarperiode kan niet worden geconfigureerd en vast is ingesteld op 7 dagen.

> [!IMPORTANT]
> Als u minder dan één vCore van rekencapaciteit nodig hebt, gebruikt u de DTU-aankoopmodel.

Zie voor meer informatie over specifieke prestatieniveaus en opties voor opslag beschikbaar voor één database [SQL-Database op basis van vCore limieten voor individuele databases](sql-database-vcore-resource-limits.md#single-database-storage-sizes-and-performance-levels) en voor de elastische pools Zie [SQL-Database op basis van vCore limieten voor elastische pools](sql-database-vcore-resource-limits.md#elastic-pool-storage-sizes-and-performance-levels).

Zie [SQL Database Veelgestelde vragen over](sql-database-faq.md) voor antwoorden op veelgestelde vragen. 

### <a name="storage-considerations"></a>Opslagoverwegingen

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

### <a name="backups-and-storage"></a>Back-ups en opslag

Opslag voor databaseback-ups is toegewezen voor ondersteuning van het punt in tijd herstellen (PITR) en lange termijn bewaren (LTR) mogelijkheden van SQL-Database. Deze opslag is toegewezen afzonderlijk voor elke database en als twee aparte per databases kosten in rekening gebracht. 

- **PITR**: worden automatisch één database back-ups naar RA-GRS opslag worden gekopieerd. Maximale grootte van het duurt dynamisch omdat de nieuwe back-ups worden gemaakt.  De opslag wordt gebruikt voor wekelijkse volledige back-ups, dagelijkse differentiële back-ups en back-ups van transactielogboeken die om de vijf minuten worden gekopieerd. Het opslagverbruik is afhankelijk van de snelheid van de wijziging van de database en de bewaarperiode. U kunt een afzonderlijke bewaarperiode voor elke database tussen 7 en 35 dagen configureren. Een minimale opslaghoeveelheid gelijk zijn aan 1 x van gegevensgrootte wordt geleverd zonder extra kosten. Dit bedrag is voor de meeste databases voldoende is voor het opslaan van 7 dagen van back-ups.
- **LTR**: SQL-Database biedt de mogelijkheid om lange bewaartermijn van volledige back-ups configureren voor maximaal tien jaar. Als LTR beleid is ingeschakeld, wordt bij deze back-ups worden opgeslagen in de opslag van de RA-GRS automatisch, maar u kunt bepalen hoe vaak de back-ups worden gekopieerd. Om te voldoen aan verschillende nalevingsvereiste, kunt u verschillende bewaartermijnen voor wekelijkse, maandelijkse en/of jaarlijkse back-ups. Deze configuratie definieert hoeveel opslagruimte wordt gebruikt voor de back-ups van links naar rechts. De prijscategorie Rekenmachine LTR kunt u schat de kosten voor opslag van links naar rechts. Zie [Langetermijnretentie](sql-database-long-term-retention.md) voor meer informatie.

### <a name="azure-hybrid-use-benefit"></a>Azure Hybrid Use Benefit

In de vCore gebaseerde aankoopmodel, kun je de bestaande licenties voor kortingen op SQL-Database met behulp van de [Azure hybride gebruik voordeel voor SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md). Azure voordeel kunt u met uw lokale SQL Server-licenties op te slaan van maximaal 30% op Azure SQL Database met behulp van uw lokale SQL Server-licenties met Software Assurance.

![prijzen](./media/sql-database-service-tiers/pricing.png)

#### <a name="migration-of-single-databases-with-geo-replication-links"></a>Migratie van individuele databases met geo-replicatiekoppelingen

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

#### <a name="migration-of-failover-groups"></a>Migratie van failover-groepen 

Migreren van failover-groepen met meerdere databases, moeten afzonderlijke migratie van de primaire en secundaire databases. Tijdens dat proces toepassen de dezelfde overwegingen en sequentiëren regels. Nadat u de databases worden geconverteerd naar het model op basis van vCore, blijft de failover-groep van kracht met dezelfde beleidsinstellingen. 

#### <a name="creation-of-a-geo-replication-secondary"></a>Het maken van een secundaire geo-replicatie

U kunt alleen een geografisch secundaire database met behulp van de servicelaag dezelfde als de primaire maken. Voor de database met de snelheid voor het genereren van hoge log wordt sterk aanbevolen dat de secundaire is gemaakt met hetzelfde prestatieniveau als het primaire. Als u een secundaire geo-database in de elastische pool voor een enkele primaire database maakt, wordt sterk aanbevolen dat de groep heeft de `maxVCore` instelling die overeenkomt met het prestatieniveau van de primaire database. Als u een secundaire geo-database in de elastische pool voor een primaire in een andere elastische pool maakt, wordt sterk aanbevolen dat de groepen dezelfde hebben `maxVCore` instellingen

#### <a name="using-database-copy-to-convert-a-dtu-based-database-to-a-vcore-based-database"></a>Een database op basis van DTU converteren naar een database op basis van vCore via een databasekopie.

U kunt database met een DTU gebaseerde prestatieniveau kopiëren naar een database met een vCore-prestaties niveau zonder beperkingen of speciale sequentiëren zolang het prestatieniveau van het doel de maximale grootte van de brondatabase ondersteunt. Dit is omdat de databasekopie een momentopname van gegevens vanaf de begintijd van de kopieerbewerking maakt en synchroniseren van gegevens tussen de bron en het doel geen uitvoert. 

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over specifieke prestatieniveaus en opslag beschikbare keuzen [resource op basis van SQL Database DTU-limieten](sql-database-dtu-resource-limits.md) en [SQL-Database op basis van vCore limieten](sql-database-vcore-resource-limits.md).
- Zie [SQL Database Veelgestelde vragen over](sql-database-faq.md) voor antwoorden op veelgestelde vragen.
- Meer informatie over [Azure-abonnement en Servicelimieten, quota's en beperkingen](../azure-subscription-service-limits.md)
