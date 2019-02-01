---
title: Azure SQL Database modellen kopen | Microsoft Docs
description: Meer informatie over de aankoopmodel van modellen die zijn beschikbare databases in de Azure SQL Database-service.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: f2e330f37c823b0815558bbc8ea10fb58439e043
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55509300"
---
# <a name="azure-sql-database-purchasing-models"></a>Azure SQL Database aanschaffen van modellen

Azure SQL Database kunt u gemakkelijk volledig beheerde PaaS-database-engine die past bij de behoeften van uw prestaties en kosten kunt kopen. Afhankelijk van het implementatiemodel van Azure SQL Database, kunt u de aankopen model dat aansluit bij uw behoeften:

- [op vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md) (aanbevolen) waarmee u de exacte hoeveelheid opslagcapaciteit kiezen en compute die u nodig hebt voor uw workload.
- [DTU gebaseerde aankoopmodel](sql-database-service-tiers-dtu.md) kunt u de berekenings- en pakketten met gelijke taakverdeling voor veelvoorkomende workloads gebundeld.

Verschillende aankopen modellen zijn beschikbaar in Azure SQL Database-implementatiemodellen:

- [Enkelvoudige databases](sql-database-single-databases-manage.md) en [elastische pools](sql-database-elastic-pool.md) in [Azure SQL Database](sql-database-technical-overview.md) bieden beide [DTU gebaseerde aankoopmodel](sql-database-service-tiers-dtu.md) en [op vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md). 
- [Beheerde exemplaren](sql-database-managed-instance.md) in enige aanbieding van Azure SQL Database de [vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md).

> [!IMPORTANT]
> [Zeer grootschalige databases (preview)](sql-database-service-tier-hyperscale.md) nu in openbare preview, alleen voor individuele databases met behulp van de vCore model kopen.

De volgende tabel en de grafiek vergelijken en deze twee aankopen modellen contrast.

|**Aankopen model**|**Beschrijving**|**Het meest geschikt voor**|
|---|---|---|
|Model op basis van DTU|Dit model is gebaseerd op een gecombineerde meting van compute, opslag- en i/o-resources. COMPUTE-grootten worden uitgedrukt in termen van Database Transaction Units (dtu's) voor individuele databases en elastische Database Transaction Units (edtu's) voor elastische pools. Zie voor meer informatie over dtu's en Edtu's [wat zijn dtu's en edtu's](sql-database-service-tiers.md#dtu-based-purchasing-model)?|Bij beste voor klanten die eenvoudige, vooraf geconfigureerde opties willen.|
|Model op basis van vCore|Dit model kunt u reken- en opslagresources onafhankelijk van elkaar te kiezen. Ook kunt u Azure Hybrid Benefit voor SQL Server gebruiken om te krijgen van kosten te besparen.|Bij beste voor klanten die flexibiliteit, controle en transparantie waarde.|
||||  

![prijsmodel](./media/sql-database-service-tiers/pricing-model.png)

## <a name="vcore-based-purchasing-model"></a>Op vCore gebaseerd aanschafmodel

Een virtuele kern staat voor de logische CPU met een optie te kiezen tussen verschillende hardwaregeneraties en fysieke kenmerken van hardware (bijvoorbeeld aantal kernen, geheugen, opslagruimte). Het vCore-aanschafmodel biedt u flexibiliteit, controle, transparantie van afzonderlijke resources en een eenvoudige manier te vertalen van de vereisten van de on-premises workloads naar de cloud. Dit model kunt u rekenkracht, geheugen en opslag op basis van hun behoeften workload kiezen. In het op vCore gebaseerde aankoopmodel, kunt u kiezen tussen [algemeen](sql-database-high-availability.md#basic-standard-and-general-purpose-service-tier-availability) en [bedrijfskritiek](sql-database-high-availability.md#premium-and-business-critical-service-tier-availability) Servicelagen voor zowel [enkelvoudige databases](sql-database-single-database-scale.md), [ beheerde exemplaren](sql-database-managed-instance.md), en [elastische pools](sql-database-elastic-pool.md). Voor individuele databases, u kunt ook de [grootschalige (preview)](sql-database-service-tier-hyperscale.md) servicelaag.

Het op vCore gebaseerde aankoopmodel kunt u onafhankelijk reken- en opslagresources kiest, overeenkomt met de on-premises prestaties en optimaliseren van de prijs. In het op vCore gebaseerde aankoopmodel betalen klanten voor:

- COMPUTE (servicelaag + aantal vCores en de hoeveelheid geheugen en de generatie van de hardware)
- Type en de hoeveelheid opslag van gegevens en logboekbestanden
- Back-upopslag (RA-GRS)

> [!IMPORTANT]
> Reken-, IOs-, gegevens en opslaan van Logboeken worden in rekening gebracht per database of elastische pool. Back-ups opslag wordt in rekening gebracht per elke database. Raadpleeg voor meer informatie van Managed Instance kosten [Azure SQL Database Managed Instance](sql-database-managed-instance.md).
> **De volgende beperkingen regio:** Het vCore-aanschafmodel is nog niet beschikbaar in de volgende regio's: West-Europa, Frankrijk-centraal, VK Zuid, UK-West en Australië-Zuidoost.

Als uw database of elastische pool meer dan 300 DTU conversie naar vCore verbruikt kan uw kosten verlagen. U kunt converteren met behulp van uw API van keuze of de Azure-portal, zonder uitvaltijd. Conversie is echter niet vereist. Als het DTU-aankoopmodel voldoet aan uw prestaties en zakelijke vereisten, moet u deze blijven gebruiken. Als u converteren van het DTU-model naar het vCore-model wilt, moet u de grootte van de compute met behulp van de volgende vuistregel: elke 100 DTU in Standard-laag moet ten minste 1 vCore in de categorie Algemeen gebruik; elke 125 DTU in Premium-laag moet ten minste 1 vCore in de laag bedrijfskritiek.

## <a name="dtu-based-purchasing-model"></a>DTU gebaseerde aankoopmodel

De Database Transaction Unit (DTU) vertegenwoordigt een samengestelde meting van CPU, geheugen, leest en schrijft. Het op DTU gebaseerde aankoopmodel biedt een reeks vooraf geconfigureerde bundels van rekenresources en inbegrepen opslag op verschillende niveaus van de schijf van de prestaties van toepassingen. Klanten die de voorkeur geeft aan de eenvoud van een vooraf geconfigureerde bundel en vaste betalingen elke maand, vindt het dtu model beter geschikt is voor hun behoeften. In de DTU gebaseerde aankoopmodel, klanten kunnen kiezen uit **Basic**, **Standard**, en **Premium** Servicelagen voor zowel [enkelvoudige databases](sql-database-single-database-scale.md) en [elastische pools](sql-database-elastic-pool.md). Dit aankoopmodel is niet beschikbaar in [instanties die worden beheerd](sql-database-managed-instance.md).

### <a name="database-transaction-units-dtus"></a>Database Transaction Units (dtu's)

Compute voor één database op een specifieke grootte binnen een [servicelaag](sql-database-single-database-scale.md), garandeert Microsoft een bepaald niveau van resources voor die database (onafhankelijk van een andere database in de Azure-cloud), bieden een voorspelbaar niveau van prestaties. De hoeveelheid resources wordt berekend als een getal van Database Transaction Units of dtu's en is een gecombineerde meting van compute, opslag- en i/o-resources. De verhouding tussen deze resources werd oorspronkelijk bepaald door een [OLTP-benchmark werkbelasting](sql-database-benchmark-overview.md), ontworpen om u te zijn van echte OLTP-workloads. Wanneer uw workload hoger is dan de hoeveelheid van elk van deze resources, is de doorvoer van uw beperkte - wat resulteert in tragere prestaties en time-outs. De resources die worden gebruikt voor uw werkbelasting niet invloed hebben op de beschikbare resources voor andere SQL-databases in de Azure-cloud en de resources die worden gebruikt door andere werkbelastingen niet invloed hebben op de beschikbare resources voor uw SQL-database.

![selectiekader](./media/sql-database-what-is-a-dtu/bounding-box.png)

Dtu's zijn handig voor het begrijpen van de relatieve hoeveelheid resources tussen Azure SQL-databases op verschillende en service-lagen. Bijvoorbeeld, verdubbeling van het aantal dtu's door het compute vergroten van een database is gelijk aan de set met resources waarover die database verdubbelen. Zo biedt een Premium P11-database met 1750 DTU's 350 keer meer DTU aan rekenvermogen dan een Basic-database met 5 DTU's.  

Gebruiken om te krijgen van inzicht in het gebruik van resources (DTU) van uw werkbelasting, [Azure SQL Database Query Performance Insight](sql-database-query-performance.md) aan:

- Identificeer de populairste query's op aantal CPU/duur/uitvoeringen die mogelijk kan worden afgestemd voor betere prestaties. Bijvoorbeeld, een i/o-intensieve query mogelijk baat bij het gebruik van [in het geheugen optimalisatietechnieken](sql-database-in-memory.md) beter gebruikmaken van het beschikbare geheugen op een bepaalde servicelaag en compute-grootte.
- Zoom in op de details van een query, de tekst en de geschiedenis van Resourcegebruik weergeven.
- Prestaties voor gegevenstoegang aanbevelingen die laten zien van acties die worden uitgevoerd door voor het afstemmen [SQL Database Advisor](sql-database-advisor.md).

### <a name="elastic-database-transaction-units-edtus"></a>Elastic Database Transaction Units (edtu's)

Dan bieden een toegewezen set met resources (dtu's) die mogelijk niet altijd nodig zijn voor een SQL-Database die altijd beschikbaar is, kunt u in plaats van databases plaats een [elastische pool](sql-database-elastic-pool.md) op een SQL-Database-server die een groep resources tussen deelt Deze databases. De gedeelde bronnen in een elastische pool worden gemeten op basis van elastische Database Transaction Units of edtu's. Elastische pools bieden een eenvoudige en kosteneffectieve oplossing voor het beheren van de prestatiedoelen voor meerdere databases met breed uiteenlopende en onvoorspelbare gebruikspatronen. Een elastische pool garandeert resources kunnen niet worden gebruikt door een database in de pool, terwijl altijd ervoor te zorgen dat elke database in de pool een minimale hoeveelheid benodigde bronnen beschikbaar heeft.

Een pool krijgt een bepaald aantal edtu's voor een vaste prijs. Binnen de elastische pool hebben afzonderlijke databases de flexibiliteit om de schaal automatisch aan te passen binnen de geconfigureerde grenzen. Een database zwaarder belast verbruiken meer edtu's om te voldoen aan de vraag. Databases bij lichter belasting wordt minder edtu's verbruiken. Databases met geen belasting verbruiken geen edtu's. Door de inrichting van resources voor de hele pool, in plaats van per database, worden beheertaken vereenvoudigd, bieden een voorspelbaar budget voor de pool.

Er kunnen extra eDTU's aan een bestaande pool worden toegevoegd zonder uitvaltijd van de database en zonder gevolgen voor de databases in de pool. En als de extra eDTU's niet meer nodig zijn, kunnen ze op elk moment uit een bestaande pool worden verwijderd. U kunt toevoegen of verwijderen uit de groep of de limiet voor de hoeveelheid edtu's een database kunt gebruiken onder zware belasting te reserveren edtu's voor andere databases. Als een database is naar verwachting minder resources, die u kunt deze uit de pool verplaatsen en configureren als een individuele database met een voorspelbare hoeveelheid vereiste resources.

### <a name="determine-the-number-of-dtus-needed-by-a-workload"></a>Het aantal dtu's die nodig zijn voor een werkbelasting bepalen

Als u een bestaande workload voor virtuele on-premises of SQL Server-machines wilt migreren naar Azure SQL Database, kunt u de [DTU-rekenmachine](http://dtucalculator.azurewebsites.net/) gebruiken om een schatting te maken van het aantal benodigde DTU's. Voor een bestaande Azure SQL Database-workload, kunt u [SQL Database Query Performance Insight](sql-database-query-performance.md) om uw verbruik van databaseresources (dtu's) om te krijgen van inzicht voor het optimaliseren van uw werkbelasting te begrijpen. U kunt ook de [sys.dm_db_ resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) DMV om gebruik van resources voor het afgelopen uur weer te geven. U kunt ook de catalogusweergave [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) resourceverbruik voor de afgelopen 14 dagen, maar op een lagere betrouwbaarheid van gemiddeld vijf minuten worden weergegeven.

### <a name="workloads-that-benefit-from-an-elastic-pool-of-resources"></a>Workloads die baat bij een elastische pool met resources hebben

Pools zijn geschikt voor een groot aantal databases met specifieke gebruikspatronen. Voor een bepaalde database, wordt dit patroon gekenmerkt door een gemiddelde laag gebruik met relatief incidentele gebruikspieken. SQL Database evalueert automatisch het historisch resourcegebruik van databases in een bestaande SQL Database-server en op basis daarvan wordt de juiste poolconfiguratie in de Azure-portal aanbevolen. Zie [Wanneer moet een elastische pool worden gebruikt?](sql-database-elastic-pool.md) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

- Zie voor vCore gebaseerde aankoopmodel, [vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md)
- Zie voor de DTU gebaseerde aankoopmodel, [DTU gebaseerde aankoopmodel](sql-database-service-tiers-dtu.md).
