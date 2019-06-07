---
title: Azure SQL Database modellen kopen | Microsoft Docs
description: Meer informatie over de aankoop modellen die beschikbaar voor Azure SQL Database zijn.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
manager: craigg
ms.date: 04/26/2019
ms.openlocfilehash: 98c19732c372fbcda3ca8e746d002f94c2687b22
ms.sourcegitcommit: 087ee51483b7180f9e897431e83f37b08ec890ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/31/2019
ms.locfileid: "66431365"
---
# <a name="choose-between-the-vcore-and-the-dtu-purchasing-models"></a>Kiezen tussen het vCore- en het aanschaffen van modellen DTU

Azure SQL Database kunt u eenvoudig een volledig beheerd platform kopen als een service (PaaS) database-engine die past bij de behoeften van uw prestaties en kosten. Afhankelijk van het implementatiemodel dat u hebt gekozen voor Azure SQL Database, kunt u de aankopen model dat bij u past:

- [Virtuele kern (vCore)-gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md) (aanbevolen). Deze aankoopmodel kunt u kiezen tussen een ingerichte Computing-laag en een rekenlaag zonder server (preview). Aan de ingerichte Computing-laag kiest u de exacte hoeveelheid rekenresources die altijd zijn ingericht voor uw workload. Met de serverloze compute-laag, kunt u de automatische schaalaanpassing van de compute-resources via een configureerbare compute-bereik opgeven. Met deze compute-laag, kunt u ook automatisch onderbreken en hervatten van de database op basis van workload-activiteit. De prijs per eenheid vCore per tijdseenheid is lager in de laag van de ingerichte Computing dan in de serverloze compute-laag.
- [Database transaction Units (DTU)-gebaseerde aankoopmodel](sql-database-service-tiers-dtu.md). Deze aankoopmodel biedt gebundelde agentpakketten voor berekeningen en opslag met gelijke taakverdeling voor veelvoorkomende workloads.

Verschillende aankopen modellen zijn beschikbaar voor verschillende implementatiemodellen voor Azure SQL Database:

- De [individuele database](sql-database-single-databases-manage.md) en [elastische pool](sql-database-elastic-pool.md) opties voor implementatie in [Azure SQL Database](sql-database-technical-overview.md) bieden zowel de [DTU gebaseerde aankoopmodel](sql-database-service-tiers-dtu.md) en de [vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md).
- De [beheerd exemplaar](sql-database-managed-instance.md) Implementatieoptie in Azure SQL Database biedt alleen de [vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md).
- De [grootschalige servicelaag](sql-database-service-tier-hyperscale.md) is beschikbaar voor individuele databases die gebruikmaken van de [vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md).

De volgende tabel en de grafiek vergelijken en het op vCore gebaseerde en aankopen modellen op basis van DTU contrast:

|**Aankopen model**|**Beschrijving**|**Het meest geschikt voor**|
|---|---|---|
|Model op basis van DTU|Dit model is gebaseerd op een gecombineerde meting van compute, opslag- en i/o-resources. COMPUTE-grootten worden uitgedrukt in dtu's voor individuele databases en elastische databases transactie-eenheden (edtu's) voor elastische pools. Zie voor meer informatie over dtu's en edtu's, [wat zijn dtu's en edtu's?](sql-database-purchase-models.md#dtu-based-purchasing-model).|Vooral handig voor klanten willen eenvoudig, vooraf geconfigureerde opties voor de resource.|
|Model op basis van vCore|Dit model kunt u reken- en opslagresources onafhankelijk van elkaar te kiezen. Het op vCore gebaseerde aankoopmodel kunt u gebruiken [Azure Hybrid Benefit voor SQL Server](https://azure.microsoft.com/pricing/hybrid-benefit/) te krijgen van de kosten te besparen.|Bij beste voor klanten die flexibiliteit, controle en transparantie waarde.|
||||  

![vergelijking van prijzen modellen](./media/sql-database-service-tiers/pricing-model.png)

## <a name="compute-costs"></a>De kosten voor rekenuren

### <a name="provisioned-compute-costs"></a>Kosten van de ingerichte Computing

In de laag van de ingerichte Computing weerspiegelen de compute-kosten zijn de totale compute-capaciteit die is ingericht voor de toepassing.

Automatisch toewijzen we in de bedrijfslaag kritieke-ten minste 3 replica's. Om te weerspiegelen deze extra toewijzing van compute-resources, is de prijs in het op vCore gebaseerde aankoopmodel ongeveer 2.7 x hoger in de bedrijfslaag kritieke-dan in de categorie Algemeen gebruik-service. Evenzo, de hogere opslag prijs per GB in de bedrijfslaag kritieke-weerspiegelt de hoge i/o en lage latentie van de SSD-opslag.

De kosten voor back-upopslag is hetzelfde voor de kritieke-bedrijfslaag en de categorie Algemeen gebruik-service omdat beide lagen standard-opslag gebruikt.

### <a name="serverless-compute-costs"></a>Serverloze compute-kosten

Zie voor een beschrijving van hoe rekencapaciteit wordt gedefinieerd en de kosten worden berekend voor de serverloze compute-laag, [serverloze SQL-Database (preview)](sql-database-serverless.md).

## <a name="storage-costs"></a>Opslagkosten

Verschillende typen opslag worden anders gefactureerd. Voor de opslag van gegevens, moet u betalen voor de ingerichte opslag op basis van de database of pool maximumgrootte die u selecteert. De kosten verandert niet, tenzij u verlagen of te die maximaal verhogen. Back-upopslag hoort bij geautomatiseerde back-ups van uw exemplaar en dynamisch wordt toegewezen. Uw back-up-bewaarperiode verhoogt, wordt de back-upopslag door uw exemplaar worden verbruikt.

Standaard zeven dagen aan geautomatiseerde back-ups van uw databases gekopieerd naar een geo-redundante opslag met leestoegang (RA-GRS) standaard Blob storage-account. Deze opslag wordt gebruikt voor wekelijkse volledige back-ups, dagelijkse differentiële back-ups en transactielogboekback-ups, deze om de 5 minuten worden gekopieerd. De grootte van de transactielogboeken, is afhankelijk van de wijzigingssnelheid van de database. Een minimale opslaghoeveelheid gelijk is aan 100 procent van de databasegrootte is opgegeven zonder extra kosten. Voor aanvullend verbruik van back-upopslag wordt in rekening gebracht in GB per maand.

Zie voor meer informatie over prijzen voor gegevensopslag, de [prijzen](https://azure.microsoft.com/pricing/details/sql-database/single/) pagina.

## <a name="vcore-based-purchasing-model"></a>Aankoopmodel op basis van vCore

Een virtuele kern (vCore) staat voor een logische CPU en biedt u de mogelijkheid om te kiezen tussen verschillende hardwaregeneraties en de fysieke kenmerken van de hardware (bijvoorbeeld het aantal kernen, het geheugen en de grootte van de). Het vCore-aanschafmodel biedt u flexibiliteit, controle, transparantie van afzonderlijke resources, en een eenvoudige manier te vertalen on-premises vereisten workloads naar de cloud. Dit model kunt u kiezen Reken-, geheugen- en resources op basis van uw workloadbehoefte.

In het op vCore gebaseerde aankoopmodel, kunt u kiezen tussen de [algemeen](sql-database-high-availability.md#basic-standard-and-general-purpose-service-tier-availability) en [bedrijfskritiek](sql-database-high-availability.md#premium-and-business-critical-service-tier-availability) Servicelagen voor [enkelvoudige databases](sql-database-single-database-scale.md), [ elastische pools](sql-database-elastic-pool.md), en [instanties die worden beheerd](sql-database-managed-instance.md). Voor individuele databases, u kunt ook de [grootschalige servicelaag](sql-database-service-tier-hyperscale.md).

Het op vCore gebaseerde aankoopmodel kunt u onafhankelijk reken- en opslagresources kiest, overeenkomt met de on-premises prestaties en optimaliseren van de prijs. In het op vCore gebaseerde aankoopmodel betaalt u voor:

- Rekenresources (de servicelaag en het aantal vCores en de hoeveelheid geheugen en de generatie van hardware).
- Het type en de hoeveelheid opslag van gegevens en logboekbestanden.
- Back-upopslag (RA-GRS).

> [!IMPORTANT]
> COMPUTE-resources, i/o en opslag van gegevens en logboekbestanden worden in rekening gebracht per database of elastische pool. Back-upopslag wordt in rekening gebracht per elke database. Zie voor meer informatie over beheerd exemplaar kosten [instanties die worden beheerd](sql-database-managed-instance.md).
> **De volgende beperkingen regio:** Zie voor de huidige lijst met ondersteunde regio's, [producten beschikbaar per regio](https://azure.microsoft.com/global-infrastructure/services/?products=sql-database&regions=all). Een beheerd exemplaar maken in een regio die momenteel niet wordt ondersteund, [verzenden van een ondersteuningsaanvraag via Azure portal](sql-database-managed-instance-resource-limits.md#obtaining-a-larger-quota-for-sql-managed-instance).

Converteren naar het vCore-aanschafmodel mogelijk als uw individuele database of elastische pool meer dan 300 dtu's verbruikt, uw kosten verlagen. U kunt converteren met behulp van uw API van keuze of met behulp van de Azure portal, zonder uitvaltijd. Conversie wordt echter niet vereist en is niet automatisch uitgevoerd. Als het op DTU gebaseerde aankoopmodel voldoet aan uw prestaties en zakelijke vereisten, moet u deze blijven gebruiken.

Als u wilt converteren van het op DTU gebaseerde aankoopmodel naar het vCore-aanschafmodel, selecteert u de grootte van de compute met behulp van de volgende vuistregels:

- Elke 100 dtu's in de standard-laag moeten ten minste 1 vCore in de categorie Algemeen gebruik-service.
- Elke 125 dtu's in de premium-laag moeten ten minste 1 vCore in de bedrijfslaag kritieke service.

## <a name="dtu-based-purchasing-model"></a>DTU gebaseerde aankoopmodel

Een database transaction Units (DTU) vertegenwoordigt een samengestelde meting van CPU-, geheugen-, lees- en schrijfbewerkingen. Het op DTU gebaseerde aankoopmodel biedt een reeks vooraf geconfigureerde bundels van rekenresources en inbegrepen opslag op verschillende niveaus van de schijf van de prestaties van toepassingen. Als u liever de eenvoud van een vooraf geconfigureerde bundel en vaste betalingen per maand, is het mogelijk dat het dtu model beter geschikt is voor uw behoeften.

In de DTU gebaseerde aankoopmodel, kunt u kiezen tussen de basic, standard en premium-Servicelagen voor zowel [enkelvoudige databases](sql-database-single-database-scale.md) en [elastische pools](sql-database-elastic-pool.md). Het op DTU gebaseerde aankoopmodel is niet beschikbaar voor [instanties die worden beheerd](sql-database-managed-instance.md).

### <a name="database-transaction-units-dtus"></a>Database transaction units (dtu's)

Compute voor één database op een specifieke grootte binnen een [servicelaag](sql-database-single-database-scale.md), garandeert Microsoft van een bepaalde mate van resources voor die database (onafhankelijk van een andere database in de Azure-cloud). Deze garantie biedt een voorspelbare prestaties. De hoeveelheid resources die zijn toegewezen voor een database wordt dan berekend als een aantal dtu's en is een gecombineerde meting van compute, opslag- en i/o-resources.

De verhouding tussen deze resources is oorspronkelijk bepaald door een [online transaction processing (OLTP) benchmark werkbelasting](sql-database-benchmark-overview.md) is bedoeld voor van echte OLTP-werkbelastingen. Wanneer uw workload hoger is dan de hoeveelheid van elk van deze resources, wordt de doorvoer van uw beperkt, leidt tot tragere prestaties en time-outs.

De resources die worden gebruikt voor uw werkbelasting niet van invloed zijn op de beschikbare resources voor andere SQL-databases in de Azure-cloud. Evenzo, de resources die worden gebruikt door andere werkbelastingen niet van invloed zijn op de beschikbare resources voor uw SQL-database.

![selectiekader](./media/sql-database-what-is-a-dtu/bounding-box.png)

Dtu's zijn handig voor het begrijpen van de relatieve resources die voor Azure SQL-databases op verschillende en service-lagen zijn toegewezen. Bijvoorbeeld:

- Verdubbeling van het aantal dtu's door het compute vergroten van een database is gelijk aan de set met resources waarover die database verdubbelen.
- Een premium service tier P11-database met 1750 dtu's biedt 350 keer meer DTU-rekenkracht dan met een database van de laag basic-service met 5 dtu's.  

Gebruiken om te krijgen van inzicht in het gebruik van resources (DTU) van uw werkbelasting, [query-prestatie-inzichten](sql-database-query-performance.md) aan:

- Identificeer de populairste query's op aantal CPU/duur/uitvoeringen die mogelijk kan worden afgestemd voor betere prestaties. Bijvoorbeeld, een I/O-intensieve query mogelijk baat bij [in het geheugen optimalisatietechnieken](sql-database-in-memory.md) beter gebruikmaken van het beschikbare geheugen op een bepaalde servicelaag en compute-grootte.
- Inzoomen op de details van een query om de tekst en de geschiedenis van Resourcegebruik weer te geven.
- Toegang tot de aanbevelingen voor het afstemmen van prestaties die laten zien van acties die worden uitgevoerd door [SQL Database Advisor](sql-database-advisor.md).

### <a name="elastic-database-transaction-units-edtus"></a>Elastische database transaction units (edtu's)

Voor SQL-databases die altijd beschikbaar zijn, in plaats daarvan dan bieden een toegewezen set met resources (dtu's) die mogelijk niet altijd nodig, kunt u plaatsen deze databases in een [elastische pool](sql-database-elastic-pool.md). De databases in een elastische pool op een enkele Azure SQL Database-server en een groep resources delen.

De gedeelde bronnen in een elastische pool worden gemeten op basis van elastische database transaction units (edtu's). Elastische pools bieden een eenvoudige en kosteneffectieve oplossing voor het beheer van prestatiedoelen voor meerdere databases met uiteenlopende en onvoorspelbare gebruikspatronen. Een elastische pool zorgt ervoor dat alle resources kunnen niet worden gebruikt door een database in de pool, terwijl elke database in de pool heeft altijd een minimale hoeveelheid benodigde bronnen beschikbaar.

Een pool krijgt een bepaald aantal edtu's voor een vaste prijs. In de elastische pool kunnen afzonderlijke databases voor automatisch schalen binnen de geconfigureerde grenzen. Een database onder een zwaardere belasting wordt meer edtu's om te voldoen aan de vraag verbruiken. Databases bij lichter belasting wordt minder edtu's verbruiken. Databases met geen belasting verbruiken geen edtu's. Omdat er resources zijn ingericht voor de hele pool in plaats van per database, kunnen elastische pools management taken te vereenvoudigen en bieden een voorspelbaar budget voor de groep.

U kunt extra edtu's toevoegen aan een bestaande pool met uitvaltijd en zonder gevolgen voor de databases in de groep. Op dezelfde manier als u extra edtu's niet meer nodig hebt, deze verwijderen uit een bestaande pool op elk gewenst moment. U kunt ook databases toevoegen of verwijderen uit van een groep van toepassingen op elk gewenst moment. Als u wilt reserveren edtu's voor andere databases, het aantal edtu's die onder zware belasting kan worden gebruikt door een database te beperken. Als een database underuses consistent resources, uit de pool verplaatsen en configureren als een individuele database met een voorspelbare hoeveelheid vereiste resources.

### <a name="determine-the-number-of-dtus-needed-by-a-workload"></a>Het aantal dtu's die nodig zijn voor een werkbelasting bepalen

Als u wilt migreren van een bestaande on-premises of virtuele machine-werkbelasting SQL Server naar Azure SQL Database, gebruik de [DTU-Rekenmachine](https://dtucalculator.azurewebsites.net/) om het aantal benodigde dtu's te schatten. Gebruik voor een bestaande Azure SQL Database-workload [query-prestatie-inzichten](sql-database-query-performance.md) om te begrijpen van het gebruik van de database-resources (dtu's) en meer inzicht voor het optimaliseren van uw workload. De [sys.dm_db_ resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) dynamische beheerweergave (DMV) kunt u gebruik van resources voor het afgelopen uur weergeven. De [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) catalogusweergave resourceverbruik voor de afgelopen 14 dagen, maar op een lagere betrouwbaarheid van gemiddeld vijf minuten worden weergegeven.

### <a name="workloads-that-benefit-from-an-elastic-pool-of-resources"></a>Workloads die baat bij een elastische pool met resources hebben

Pools zijn geschikt voor databases met een laag Resourcegebruik gemiddelde en relatief onregelmatige gebruikspieken. SQL-Database wordt automatisch het historisch Resourcegebruik van databases op een bestaande SQL Database-server evalueert en raadt aan om de juiste poolconfiguratie in Azure portal. Zie voor meer informatie, [wanneer moet u rekening houden met een SQL Database elastische pool?](sql-database-elastic-pool.md).

## <a name="frequently-asked-questions-faqs"></a>Veelgestelde vragen (FAQ's)

### <a name="do-i-need-to-take-my-application-offline-to-convert-from-a-dtu-based-service-tier-to-a-vcore-based-service-tier"></a>Moet ik mijn toepassing offline te converteren van een servicelaag op basis van DTU naar een vCore-servicelaag?

Nee. U hoeft niet offline zetten van de toepassing. De nieuwe Servicelagen bieden een eenvoudige-onlineconversiemethode die vergelijkbaar is met het bestaande upgradeproces van het upgraden van de databases van de standaard naar de premium-servicelaag en andersom. U kunt deze conversie een hoeveelheid starten met behulp van de Azure-portal, PowerShell, de Azure CLI, T-SQL of de REST-API. Zie [individuele databases beheren](sql-database-single-database-scale.md) en [elastische pools beheren](sql-database-elastic-pool.md).

### <a name="can-i-convert-a-database-from-a-service-tier-in-the-vcore-based-purchasing-model-to-a-service-tier-in-the-dtu-based-purchasing-model"></a>Kan ik een database van een servicelaag in het op vCore gebaseerde aankoopmodel converteren naar een servicelaag in het op DTU gebaseerde aankoopmodel?

Ja, u kunt omzetten uw database naar een ondersteunde prestaties doelstelling met behulp van de Azure-portal, PowerShell, de Azure CLI, T-SQL of de REST-API. Zie [individuele databases beheren](sql-database-single-database-scale.md) en [elastische pools beheren](sql-database-elastic-pool.md).

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het op vCore gebaseerde aankoopmodel [vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md).
- Zie voor meer informatie over het op DTU gebaseerde aankoopmodel [DTU gebaseerde aankoopmodel](sql-database-service-tiers-dtu.md).
