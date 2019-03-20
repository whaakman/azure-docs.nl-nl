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
ms.date: 02/08/2019
ms.openlocfilehash: 0e9001111d6aa48f0dad69a2fb3b2186bfc37ab7
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58010505"
---
# <a name="azure-sql-database-purchasing-models"></a>Azure SQL Database aanschaffen van modellen

Azure SQL Database kunt u gemakkelijk volledig beheerde PaaS-database-engine die past bij de behoeften van uw prestaties en kosten kunt kopen. Afhankelijk van het implementatiemodel van Azure SQL Database, kunt u de aankopen model dat aansluit bij uw behoeften:

- [op vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md) (aanbevolen) waarmee u de exacte hoeveelheid opslagcapaciteit kiezen en compute die u nodig hebt voor uw workload.
- [DTU gebaseerde aankoopmodel](sql-database-service-tiers-dtu.md) kunt u de reken- en pakketten met gelijke taakverdeling voor veelvoorkomende workloads gebundeld.

Verschillende aankopen modellen zijn beschikbaar in Azure SQL Database-implementatiemodellen:

- De [individuele database](sql-database-single-databases-manage.md) en [elastische pool](sql-database-elastic-pool.md) opties voor implementatie in [Azure SQL Database](sql-database-technical-overview.md) bieden zowel de [DTU gebaseerde aankoopmodel](sql-database-service-tiers-dtu.md) en de [vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md).
- De [beheerd exemplaar](sql-database-managed-instance.md) Implementatieoptie in Azure SQL Database biedt alleen de [vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md).

> [!IMPORTANT]
> De [grootschalige servicelaag (preview)](sql-database-service-tier-hyperscale.md) is in openbare preview, alleen voor individuele databases met behulp van de vCore model kopen.

De volgende tabel en de grafiek vergelijken en deze twee aankopen modellen contrast.

|**Aankopen model**|**Beschrijving**|**Het meest geschikt voor**|
|---|---|---|
|Model op basis van DTU|Dit model is gebaseerd op een gecombineerde meting van compute, opslag- en i/o-resources. COMPUTE-grootten worden uitgedrukt in termen van Database Transaction Units (dtu's) voor individuele databases en elastische Database Transaction Units (edtu's) voor elastische pools. Zie voor meer informatie over dtu's en Edtu's [wat zijn dtu's en edtu's?](sql-database-purchase-models.md#dtu-based-purchasing-model).|Bij beste voor klanten die eenvoudige, vooraf geconfigureerde opties willen.|
|Model op basis van vCore|Dit model kunt u reken- en opslagresources onafhankelijk van elkaar te kiezen. Het op vCore gebaseerde aankoopmodel kunt u gebruiken [Azure Hybrid Benefit voor SQL Server](https://azure.microsoft.com/pricing/hybrid-benefit/) te krijgen van de kosten te besparen.|Bij beste voor klanten die flexibiliteit, controle en transparantie waarde.|
||||  

![prijsmodel](./media/sql-database-service-tiers/pricing-model.png)

## <a name="compute-costs"></a>De kosten voor rekenuren

De kosten zijn de totale compute-capaciteit die is ingericht voor de toepassing. Automatisch toewijzen we in de bedrijfslaag kritieke-ten minste 3 replica's. Om te weerspiegelen deze extra toewijzing van compute-resources, is de prijs in het op vCore gebaseerde aankoopmodel ongeveer 2.7 x hoger in de kritieke-bedrijfslaag dan in de categorie Algemeen gebruik-service. De hogere opslag prijs per GB in de bedrijfslaag kritieke-is om dezelfde reden inclusief de hoge i/o- en lage latentie van de SSD-opslag. Op hetzelfde moment is niet de kosten voor back-upopslag verschil is tussen deze twee Servicelagen, omdat in beide gevallen we een klasse standard-opslag gebruiken.

## <a name="storage-costs"></a>Opslagkosten

Verschillende typen opslag worden anders gefactureerd. Voor opslag van gegevens in rekening gebracht voor de ingerichte opslag op basis van de database of pool maximumgrootte die u selecteert. De kosten wordt niet gewijzigd, tenzij u verlagen of te die maximaal verhogen. Back-upopslag hoort bij geautomatiseerde back-ups van uw exemplaar en dynamisch wordt toegewezen. Door de retentieperiode van uw back-ups te vergroten, zal er meer back-upopslag door uw exemplaar worden verbruikt. 

Zeven dagen aan geautomatiseerde back-ups van uw databases worden standaard naar RA-GRS-standaardblobopslag gekopieerd. De opslag wordt gebruikt voor wekelijkse volledige back-ups, dagelijkse differentiële back-ups en back-ups van transactielogboeken die om de vijf minuten worden gekopieerd. De grootte van het transactielogboek is afhankelijk van de wijzigingssnelheid van de database. U kunt gratis gebruik maken van opslagruimte ter waarde van 100% van de databasegrootte. Voor aanvullend verbruik van back-upopslag worden GB/maand berekend.

Zie voor meer informatie over prijzen voor gegevensopslag, de [prijzen](https://azure.microsoft.com/pricing/details/sql-database/single/) pagina. 

## <a name="vcore-based-purchasing-model"></a>Op vCore gebaseerd aanschafmodel

Een virtuele kern staat voor de logische CPU met een optie te kiezen tussen verschillende hardwaregeneraties en fysieke kenmerken van hardware (bijvoorbeeld aantal kernen, geheugen, opslagruimte). Het vCore-aanschafmodel biedt u flexibiliteit, controle, transparantie van afzonderlijke resources en een eenvoudige manier te vertalen van de vereisten van de on-premises workloads naar de cloud. Dit model kunt u rekenkracht, geheugen en opslag op basis van hun behoeften workload kiezen. In het op vCore gebaseerde aankoopmodel, kunt u kiezen tussen [algemeen](sql-database-high-availability.md#basic-standard-and-general-purpose-service-tier-availability) en [bedrijfskritiek](sql-database-high-availability.md#premium-and-business-critical-service-tier-availability) Servicelagen voor [enkelvoudige databases](sql-database-single-database-scale.md), [ elastische pools](sql-database-elastic-pool.md), en [instanties die worden beheerd](sql-database-managed-instance.md). Voor individuele databases, u kunt ook de [grootschalige servicelaag (preview)](sql-database-service-tier-hyperscale.md).

Het op vCore gebaseerde aankoopmodel kunt u onafhankelijk reken- en opslagresources kiest, overeenkomt met de on-premises prestaties en optimaliseren van de prijs. In het op vCore gebaseerde aankoopmodel betalen klanten voor:

- COMPUTE (servicelaag + aantal vCores en de hoeveelheid geheugen en de generatie van de hardware)
- Type en de hoeveelheid opslag van gegevens en logboekbestanden
- Back-upopslag (RA-GRS)

> [!IMPORTANT]
> Reken-, IOs-, gegevens en opslaan van Logboeken worden in rekening gebracht per database of elastische pool. Back-ups opslag wordt in rekening gebracht per elke database. Zie voor meer informatie over beheerd exemplaar kosten [instanties die worden beheerd](sql-database-managed-instance.md).
> **De volgende beperkingen regio:** Zie voor de huidige lijst met ondersteunde regio's, [producten beschikbaar per regio](https://azure.microsoft.com/global-infrastructure/services/?products=sql-database&regions=all). Als u maken van een beheerd exemplaar in de regio die wordt momenteel niet ondersteund wilt, kunt u [verzoek om ondersteuning te verzenden via Azure portal](sql-database-managed-instance-resource-limits.md#obtaining-a-larger-quota-for-sql-managed-instance).
.

Converteren naar het vCore-aanschafmodel kan als uw individuele database of elastische pool meer dan 300 dtu's verbruikt, de kosten verminderen. Als u converteren wilt, kunt u uw API naar keuze gebruiken of met behulp van de Azure-portal, zonder uitvaltijd kunt converteren. Echter conversie is niet vereist en niet automatisch gebeurt. Als het op DTU gebaseerde aankoopmodel voldoet aan uw prestaties en zakelijke vereisten, moet u deze blijven gebruiken. Als u converteren van het op DTU gebaseerde aankoopmodel naar het vCore-aanschafmodel wilt, selecteert u de grootte van de compute met behulp van de volgende vuistregels: 

- Elke 100 DTU in Standard-laag moet ten minste 1 vCore in de categorie Algemeen gebruik
- Elke 125 DTU in Premium-laag moet ten minste 1 vCore in de laag bedrijfskritiek

## <a name="dtu-based-purchasing-model"></a>DTU gebaseerde aankoopmodel

De Database Transaction Unit (DTU) vertegenwoordigt een samengestelde meting van CPU, geheugen, leest en schrijft. Het op DTU gebaseerde aankoopmodel biedt een reeks vooraf geconfigureerde bundels van rekenresources en inbegrepen opslag op verschillende niveaus van de schijf van de prestaties van toepassingen. Klanten die de voorkeur geeft aan de eenvoud van een vooraf geconfigureerde bundel en vaste betalingen elke maand, vindt het dtu model beter geschikt is voor hun behoeften. In de DTU gebaseerde aankoopmodel, klanten kunnen kiezen uit **basic**, **standard**, en **premium** Servicelagen voor zowel [enkelvoudige databases](sql-database-single-database-scale.md) en [elastische pools](sql-database-elastic-pool.md). Dit aankoopmodel is niet beschikbaar in [instanties die worden beheerd](sql-database-managed-instance.md).

### <a name="database-transaction-units-dtus"></a>Database transaction units (dtu's)

Compute voor één database op een specifieke grootte binnen een [servicelaag](sql-database-single-database-scale.md), garandeert Microsoft een bepaald niveau van resources voor die database (onafhankelijk van een andere database in de Azure-cloud), bieden een voorspelbaar niveau van prestaties. De hoeveelheid resources wordt berekend als een getal van Database Transaction Units of dtu's en is een gecombineerde meting van compute, opslag- en i/o-resources. De verhouding tussen deze resources werd oorspronkelijk bepaald door een [OLTP-benchmark werkbelasting](sql-database-benchmark-overview.md), ontworpen om u te zijn van echte OLTP-workloads. Wanneer uw workload hoger is dan de hoeveelheid van elk van deze resources, is de doorvoer van uw beperkte - wat resulteert in tragere prestaties en time-outs. De resources die worden gebruikt voor uw werkbelasting niet invloed hebben op de beschikbare resources voor andere SQL-databases in de Azure-cloud en de resources die worden gebruikt door andere werkbelastingen niet invloed hebben op de beschikbare resources voor uw SQL-database.

![selectiekader](./media/sql-database-what-is-a-dtu/bounding-box.png)

Dtu's zijn handig voor het begrijpen van de relatieve hoeveelheid resources tussen Azure SQL-databases op verschillende en service-lagen. Bijvoorbeeld, verdubbeling van het aantal dtu's door het compute vergroten van een database is gelijk aan de set met resources waarover die database verdubbelen. Zo biedt een Premium P11-database met 1750 DTU's 350 keer meer DTU aan rekenvermogen dan een Basic-database met 5 DTU's.  

Gebruiken om te krijgen van inzicht in het gebruik van resources (DTU) van uw werkbelasting, [query prestatie-inzichten](sql-database-query-performance.md) aan:

- Identificeer de populairste query's op aantal CPU/duur/uitvoeringen die mogelijk kan worden afgestemd voor betere prestaties. Bijvoorbeeld, een i/o-intensieve query mogelijk baat bij het gebruik van [in het geheugen optimalisatietechnieken](sql-database-in-memory.md) beter gebruikmaken van het beschikbare geheugen op een bepaalde servicelaag en compute-grootte.
- Zoom in op de details van een query, de tekst en de geschiedenis van Resourcegebruik weergeven.
- Prestaties voor gegevenstoegang aanbevelingen die laten zien van acties die worden uitgevoerd door voor het afstemmen [SQL Database Advisor](sql-database-advisor.md).

### <a name="elastic-database-transaction-units-edtus"></a>Elastic Database Transaction Units (edtu's)

Dan bieden een toegewezen set met resources (dtu's) die mogelijk niet altijd nodig zijn voor een SQL-Database die altijd beschikbaar is, kunt u in plaats van databases plaats een [elastische pool](sql-database-elastic-pool.md) op een SQL-Database-server die een groep resources tussen deelt Deze databases. De gedeelde bronnen in een elastische pool worden gemeten op basis van elastische Database Transaction Units of edtu's. Elastische pools bieden een eenvoudige en kosteneffectieve oplossing voor het beheren van de prestatiedoelen voor meerdere databases met breed uiteenlopende en onvoorspelbare gebruikspatronen. Een elastische pool garandeert resources kunnen niet worden gebruikt door een database in de pool, terwijl altijd ervoor te zorgen dat elke database in de pool een minimale hoeveelheid benodigde bronnen beschikbaar heeft.

Een pool krijgt een bepaald aantal edtu's voor een vaste prijs. Binnen de elastische pool hebben afzonderlijke databases de flexibiliteit om de schaal automatisch aan te passen binnen de geconfigureerde grenzen. Een database zwaarder belast verbruiken meer edtu's om te voldoen aan de vraag. Databases bij lichter belasting wordt minder edtu's verbruiken. Databases met geen belasting verbruiken geen edtu's. Door de inrichting van resources voor de hele pool, in plaats van per database, worden beheertaken vereenvoudigd, bieden een voorspelbaar budget voor de pool.

Er kunnen extra eDTU's aan een bestaande pool worden toegevoegd zonder uitvaltijd van de database en zonder gevolgen voor de databases in de pool. En als de extra eDTU's niet meer nodig zijn, kunnen ze op elk moment uit een bestaande pool worden verwijderd. U kunt toevoegen of verwijderen uit de groep of de limiet voor de hoeveelheid edtu's een database kunt gebruiken onder zware belasting te reserveren edtu's voor andere databases. Als een database is naar verwachting minder resources, die u kunt deze uit de pool verplaatsen en configureren als een individuele database met een voorspelbare hoeveelheid vereiste resources.

### <a name="determine-the-number-of-dtus-needed-by-a-workload"></a>Het aantal dtu's die nodig zijn voor een werkbelasting bepalen

Als u wilt migreren als u een bestaande on-premises of virtuele machine-werkbelasting SQL Server naar Azure SQL Database, kunt u de [DTU-Rekenmachine](https://dtucalculator.azurewebsites.net/) om het aantal benodigde dtu's te schatten. Voor een bestaande Azure SQL Database-workload, kunt u [query performance insight](sql-database-query-performance.md) om uw verbruik van databaseresources (dtu's) om te krijgen van inzicht voor het optimaliseren van uw werkbelasting te begrijpen. U kunt ook de [sys.dm_db_ resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) DMV om gebruik van resources voor het afgelopen uur weer te geven. U kunt ook de catalogusweergave [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) resourceverbruik voor de afgelopen 14 dagen, maar op een lagere betrouwbaarheid van gemiddeld vijf minuten worden weergegeven.

### <a name="workloads-that-benefit-from-an-elastic-pool-of-resources"></a>Workloads die baat bij een elastische pool met resources hebben

Pools zijn geschikt voor een groot aantal databases met specifieke gebruikspatronen. Voor een bepaalde database, wordt dit patroon gekenmerkt door een gemiddelde laag gebruik met relatief incidentele gebruikspieken. SQL Database evalueert automatisch het historisch resourcegebruik van databases in een bestaande SQL Database-server en op basis daarvan wordt de juiste poolconfiguratie in de Azure-portal aanbevolen. Zie [Wanneer moet een elastische pool worden gebruikt?](sql-database-elastic-pool.md) voor meer informatie.

## <a name="purchase-model-frequently-asked-questions-faq"></a>Aankoopmodel Veelgestelde vragen (FAQ)

### <a name="do-i-need-to-take-my-application-offline-to-convert-from-a-dtu-based-database-to-a-vcore-based-service-tier"></a>Moet ik mijn toepassing offline te converteren van een database op basis van DTU naar een vCore-servicelaag

De nieuwe servicelagen bieden een eenvoudige onlineconversiemethode die vergelijkbaar is met het bestaande upgradeproces van de Standard- naar de Premium-servicelaag en omgekeerd. Deze conversie kan worden gestart via de Azure-portal, PowerShell, Azure CLI, T-SQL of de REST-API. Zie [individuele databases beheren](sql-database-single-database-scale.md) en [elastische pools beheren](sql-database-elastic-pool.md).

### <a name="can-i-convert-a-database-from-a-service-tier-using-the-vcore-based-purchase-to-a-service-tier-using-the-dtu-based-purchasing-model"></a>Ik kan een database converteren van een servicelaag met behulp van de aankoop op vCore gebaseerde aan een servicelaag met behulp van het op DTU gebaseerde aankoopmodel

Ja, kunt u uw database omzetten naar een ondersteunde prestaties doelstelling met behulp van Azure portal, PowerShell, Azure CLI, T-SQL of de REST-API. Zie [individuele databases beheren](sql-database-single-database-scale.md) en [elastische pools beheren](sql-database-elastic-pool.md).

## <a name="next-steps"></a>Volgende stappen

- Zie voor vCore gebaseerde aankoopmodel, [vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md)
- Zie voor de DTU gebaseerde aankoopmodel, [DTU gebaseerde aankoopmodel](sql-database-service-tiers-dtu.md).
