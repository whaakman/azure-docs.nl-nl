---
title: Veelgestelde vragen over Azure SQL Database | Microsoft Docs
description: Antwoorden op algemene vragen klanten vragen over clouddatabases en Azure SQL Database, van Microsoft relationeel databasebeheersysteem (RDBMS) en -database als een service in de cloud.
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 10/15/2018
ms.openlocfilehash: 46df47bf145e5fc4a1846c33fdf0426840befdcd
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49340703"
---
# <a name="sql-database-frequently-asked-questions-faq"></a>Veelgestelde vragen (FAQ) SQL-Database

## <a name="what-is-the-current-version-of-sql-database"></a>Wat is de huidige versie van SQL Database

De huidige versie van SQL Database is V12. Versie V11 buiten gebruik gesteld.

## <a name="what-is-the-sla-for-sql-database"></a>Wat is de SLA voor SQL-Database

Wij garanderen ten minste 99,99% van de tijd, krijgt u connectiviteit tussen uw Microsoft Azure SQL Database en onze Internet-gateway, ongeacht uw servicelaag. 0,01% is gereserveerd voor patches en upgrades failovers. Zie voor meer informatie, [SLA](http://azure.microsoft.com/support/legal/sla/). Zie voor meer informatie over de architectuur van de beschikbaarheid van Azure SQL Database [hoge beschikbaarheid en Azure SQL Database](sql-database-high-availability.md).

## <a name="can-i-control-when-patching-downtime-occurs"></a>Ik kan bepalen wanneer patches downtime plaatsvindt

Nee. De gevolgen van het toepassen van patches is doorgaans niet opvallen als u [maken gebruik van logica voor opnieuw proberen](sql-database-develop-overview.md#resiliency) in uw app.

## <a name="what-is-the-new-vcore-based-purchasing-model-for-azure-sql-database"></a>Wat is de nieuwe vCore gebaseerde aankoopmodel voor Azure SQL Database

Het nieuwe aanschafmodel is een aanvulling op het bestaande DTU-model. Het vCore-model is ontworpen om klanten flexibiliteit, controle, transparantie, en een eenvoudige manier te vertalen on-premises vereisten workloads naar de cloud. Ook kunnen klanten hun reken- en rsources op basis van hun werkbelasting schalen. Individuele databases en elastische pool-opties met behulp van het vCore-model zijn ook in aanmerking komen voor van 30 procent besparen met de [Azure Hybrid Use Benefit voor SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md). Zie [DTU gebaseerde aankoopmodel](sql-database-service-tiers-dtu.md) en [vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md) voor meer informatie.

## <a name="what-is-a-vcore"></a>Wat is een vCore

Een virtuele kern staat voor de logische CPU met een optie te kiezen tussen verschillende hardwaregeneraties. Gen 4 logische CPU's zijn gebaseerd op Intel E5-2673 v3 (Haswell)-processors voor 2,4 GHz en Gen 5 logische CPU's zijn gebaseerd op Intel E5-2673 v4 (Broadwell) 2,3 GHz-processors.

## <a name="is-moving-to-the-vcore-based-model-required"></a>Wordt verplaatst naar het vCore-model vereist

Nee, inclusief de introductie van het vCore-model voor de elastische Pool en implementatieopties voor één Database onze toewijding aan keuze en flexibiliteit. Als klanten willen hebben om door te gaan met behulp van het dtu model, niet hoeven te doen met deze aankondiging en hun ervaring en facturering blijven ongewijzigd.

In veel gevallen kunnen toepassingen profiteren van de eenvoud van een vooraf geconfigureerde bundel met resources. Daarom gaan we bieden en ondersteuning van deze opties op basis van DTU voor onze klanten. Als u deze gebruikt en deze voldoet aan uw zakelijke vereisten, moet u dit niet doet, blijven.

Beide modellen, zowel die op basis van DTU als vCores, blijven naast elkaar bestaan. We starten van het vCore-model in reactie op aanvragen van klanten om meer duidelijkheid over hun databaseresources en de mogelijkheid om hun reken- en opslagresources afzonderlijk schalen. Het vCore-model kan ook nog meer te besparen voor klanten met actieve Software Assurance via de Azure Hybrid Benefit voor SQL Server.

## <a name="how-should-i-choose-between-the-dtu-based-purchasing-model-vs-the-vcore-based-purchasing-model"></a>Hoe moet ik kiezen tussen de vs aankopen model op basis van DTU op vCore gebaseerde aankoopmodel

De Data Transmission Unit (DTU) is gebaseerd op gecombineerde meetgegevens van CPU, geheugen en lees- en schrijfbewerkingen. De grootte op basis van DTU compute vertegenwoordigen vooraf geconfigureerde bundels van resources op verschillende niveaus van de schijf van de prestaties van toepassingen. Klanten die u geen wilt zorgen te maken over de onderliggende resources en de voorkeur van een vooraf geconfigureerde bundel tijdens een vast bedrag per maand betalen vinden het dtu model beter geschikt is voor hun behoeften. Het vCore-model wordt echter voor klanten die meer inzicht in de onderliggende resources nodig of moeten ze onafhankelijk van elkaar voor optimale prestaties schalen, worden de beste keuze.  Bovendien, als een klant een actieve Software Assurance (SA) voor SQL Server heeft, ze kunnen gebruikmaken van hun bestaande investeringen en bespaar tot wel 30% met [Azure Hybrid Use Benefit voor SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md).  Binnen elk aanschafmodellen bieden de voordelen van een volledig beheerde service zoals geautomatiseerde back-ups, software-updates en patches.

## <a name="what-is-the-azure-hybrid-benefit-for-sql-server"></a>Wat is Azure Hybrid Benefit voor SQL Server

De [Azure Hybrid Use Benefit voor SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md) helpt u bij de waarde van uw huidige licentie-investeringen maximaliseren en Versnel de migratie naar de cloud. Azure Hybrid Benefit voor SQL Server is een Azure-voordeel waarmee u uw SQL Server-licenties met Software Assurance gebruiken voor een gereduceerd tarief ('basistarief') voor SQL-Database. Azure Hybrid Benefit voor SQL Server is beschikbaar in openbare preview-versie van het op vCore gebaseerde aankoopmodel voor SQL-Database, individuele databases en elastische pools. U kunt dit voordeel toepassen, zelfs als de SKU actief is, maar houd er rekening mee dat het basistarief wordt toegepast vanaf het moment dat u dit in de Azure-portal selecteren. Er worden niet met terugwerkende kracht tegoeden verleend.

## <a name="are-there-dual-use-rights-with-azure-hybrid-benefit-for-sql-server"></a>Zijn er twee gebruiksrechten met Azure Hybrid Benefit voor SQL Server

U hebt 180 dagen van dual de gebruiksrechten van de licentie om te controleren of de migraties naadloos worden uitgevoerd. Na die periode van 180 dagen, de SQL Server-licentie kan alleen worden gebruikt in de cloud in SQL-Database, en heeft geen dubbele rechten on-premises gebruiken en in de cloud.

## <a name="how-does-azure-hybrid-benefit-for-sql-server-differ-from-license-mobility"></a>Hoe verschilt Azure Hybrid Benefit voor SQL Server van mobiliteit van licenties

Vandaag, bieden we van mobiliteit van licenties voor SQL Server-klanten met Software Assurance waarmee hernieuwde toewijzing van licenties aan gedeelde servers van derden. Dit voordeel kan worden gebruikt op Azure IaaS- en AWS EC2.
Azure Hybrid Benefit voor SQL Server verschilt van mobiliteit van licenties in twee belangrijke gebieden:

- Het biedt economische voordelen voor maximaal gevirtualiseerde werkbelastingen naar Azure verplaatst. SQL EE klanten krijgen 4 kernen in Azure in de SKU voor algemeen gebruik voor elke kern ze on-premises voor maximaal gevirtualiseerde toepassingen eigenaar. Mobiliteit van licenties staat niet toe dat geen voor speciale kostenvoordelen voor het verplaatsen van gevirtualiseerde werkbelastingen naar de cloud.
- Het biedt voor een PaaS-bestemming op Azure (SQL Database Managed Instance) dat zeer compatibel is met on-premises SQL Server

## <a name="what-are-the-specific-rights-of-the-azure-hybrid-benefit-for-sql-server"></a>Wat zijn de specifieke rechten van de Azure Hybrid Benefit voor SQL Server

SQL Database-klanten hebben de volgende rechten voor in verband met Azure Hybrid Benefit voor SQL Server:

|Licentie-Footprint|Wat doet Azure Hybrid Benefit voor SQL Server ophalen?|
|---|---|
|SQL Server Enterprise Edition core klanten met Software Assurance|<li>Kan Base-tarief voor algemeen gebruik of bedrijfskritieke SKU betalen</li><br><li>1 core on-premises = 4 kernen in SKU voor algemeen gebruik</li><br><li>1 core on-premises = 1 kern in bedrijfskritieke SKU</li>|
|SQL Server Standard Edition core klanten met Software Assurance|<li>Kan Base tarief voor betalen op SKU voor algemeen gebruik alleen</li><br><li>1 core on-premises = 1 kern in SKU voor algemeen gebruik</li>|
|||

## <a name="is-the-vcore-based-model-available-to-sql-database-managed-instance"></a>Het vCore-model is beschikbaar voor SQL Database Managed Instance

[Beheerd exemplaar](sql-database-managed-instance.md) is alleen beschikbaar bij het vCore-model. Zie voor meer informatie, ook de [pagina met prijzen van SQL-Database](https://azure.microsoft.com/pricing/details/sql-database/managed/).

## <a name="does-the-cost-of-compute-and-storage-depend-on-the-service-tier-that-i-choose"></a>De kosten van reken- en is afhankelijk van de servicelaag die ik kiezen

De kosten zijn de totale compute-capaciteit die is ingericht voor de toepassing. Automatisch toewijzen we in de laag bedrijfskritiek service ten minste 3 replica's. Aanleiding van deze extra toewijzing van compute-resources, is de prijs vCore ongeveer 2.7 x hoger in bedrijfskritiek. De hogere opslag prijs per GB in de laag bedrijfskritiek is om dezelfde reden inclusief de hoge i/o- en lage latentie van de SSD-opslag. Op hetzelfde moment is niet de kosten voor back-upopslag verschillend, omdat in beide gevallen we een klasse standard-opslag gebruiken.

## <a name="how-am-i-charged-for-storage---based-on-what-i-configure-upfront-or-on-what-the-database-uses"></a>Hoe moet ik dan betalen voor opslag - gebaseerd op wat kan ik vooraf configureren of wat de database wordt gebruikt

Verschillende typen opslag worden anders gefactureerd. Voor opslag van gegevens in rekening gebracht voor de ingerichte opslag op basis van de database of pool maximumgrootte die u selecteert. De kosten wordt niet gewijzigd, tenzij u verlagen of te die maximaal verhogen. Back-upopslag hoort bij geautomatiseerde back-ups van uw exemplaar en dynamisch wordt toegewezen. Door de retentieperiode van uw back-ups te vergroten, zal er meer back-upopslag door uw exemplaar worden verbruikt. Er worden geen extra kosten in rekening gebracht voor back-upopslag voor maximaal 100% van uw totale ingerichte serveropslag. Voor aanvullend verbruik van back-upopslag wordt in rekening gebracht in GB per maand. Als u bijvoorbeeld een database-opslag van 100 GB hebt, krijgt u 100 GB aan back-upopslag, zonder extra kosten. Maar als de back-up 110 GB is, betaalt u voor de extra 10 GB.

Voor back-upopslag van één database in rekening gebracht op het pro rata gefactureerd voor de opslag die is toegewezen aan de databaseback-ups minus de grootte van de database. Voor back-upopslag van een elastische pool in rekening gebracht op het pro rata gefactureerd voor de opslag die is toegewezen aan de databaseback-ups van alle databases in de groep minus de maximale grootte van de elastische pool. Een toename van de grootte van de database of elastische pool of een toename van de Transactiesnelheid meer opslag vereist en dus verhoogt uw back-upopslag-factuur.  Als u de maximale gegevensgrootte verhoogt, wordt deze nieuwe bedrag afgetrokken van de grootte van de back-upopslag worden gefactureerd.

## <a name="how-do-i-select-the-right-sku-when-converting-an-existing-database-to-the-new-service-tiers"></a>Hoe selecteer ik de juiste SKU bij het converteren van een bestaande database naar de nieuwe service-lagen

Voor bestaande SQL Database-toepassingen met behulp van het dtu model, is de categorie Algemeen gebruik-service worden vergeleken met de Standard-laag. De service-laag bedrijfskritiek is vergelijkbaar met de Premium-laag. In beide gevallen moet u ten minste 1 vCore toewijzen voor elke 100 DTU die gebruikmaakt van uw toepassing in het dtu model.

## <a name="do-the-new-vcore-based-service-tiers-offer-the-compute-sizes-compatible-with-all-existing-compute-sizes"></a>De nieuwe op vCore gebaseerde Servicelagen bieden dat de compute-grootten die compatibel is met alle bestaande compute grootten

De nieuwe op vCore gebaseerde Servicelagen bieden vergelijkbare keuzes voor alle elastische pools en databases met behulp van 100 dtu's of meer.  We blijven er meer rekenkracht grootten na verloop van tijd voor sub 100 DTU-werkbelastingen toevoegen.

## <a name="are-there-any-database-feature-differences-between-the-existing-dtu-based-and-new-vcore-based-service-tiers"></a>Zijn er database Functieverschillen tussen de bestaande lagen, op basis van DTU en de nieuwe vCore-gebaseerde service

De nieuwe Servicelagen ondersteuning voor een hoofdverzameling van de functies die beschikbaar zijn met de huidige DTU gebaseerde aanbiedingen. De aanvullende functies bevatten een aantal extra dynamische beheerweergave (DMV's) en extra configuratie-opties.

## <a name="if-my-database-is-cpu-bound-and-does-not-use-much-storage-can-i-increase-the-compute-without-paying-for-extra-storage"></a>Als mijn database afhankelijk van de CPU is en veel opslag niet gebruikt, kan ik vergroten de compute zonder te hoeven betalen voor extra opslagruimte

Ja, kunt u het niveau van de compute uw toepassing nodig heeft en de opslag die ongewijzigd blijven afzonderlijk selecteren. De opslag kan worden ingesteld als minimaal 32GB.

## <a name="will-the-new-vcore-based-tiers-support-point-in-time-restore-pitr-for-35-days-as-today"></a>De nieuwe op vCore gebaseerde lagen wordt ondersteuning punt in tijd herstel (PITR) 35 dagen geleden vandaag

U kunt de bewaarperiode voor back-up configureren voor PITR tussen 7 en 35 dagen. De back-ups-opslag wordt verrekend afzonderlijk op basis van verbruik van de werkelijke opslagruimte als deze de gelijk zijn aan de grootte van de maximale hoeveelheid opslagruimte overschrijdt. Preview-versie, wordt standaard is de bewaarperiode PITR ingesteld op 7 dagen. In veel gevallen is de maximale gegevensgrootte voldoende zijn voor het opslaan van zeven dagen aan back-ups.

## <a name="why-do-you-allow-selection-of-the-hardware-generation-for-compute"></a>Waarom kan u de selectie van de generatie hardware voor compute toestaan

Ons doel is om in te schakelen van maximale flexibiliteit, zodat u kunt een prestatieconfiguratie die overeenkomt met de behoeften van de toepassing. Gen4 hardware biedt aanzienlijk meer geheugen per vCore. Gen5 hardware kunt u echter veel hoger rekenresources kan worden uitgebreid. Zie voor meer informatie, [aankoopmodel vCore](sql-database-service-tiers-vcore.md)

## <a name="do-i-need-to-take-my-application-offline-to-convert-from-a-dtu-based-database-to-a-vcore-based-service-tier"></a>Moet ik mijn toepassing offline te converteren van een database op basis van DTU naar een vCore-servicelaag

De nieuwe servicelagen bieden een eenvoudige onlineconversiemethode die vergelijkbaar is met het bestaande upgradeproces van de Standard- naar de Premium-servicelaag en omgekeerd. Deze conversie kan worden gestart via de Azure-portal, PowerShell, Azure CLI, T-SQL of de REST-API. Zie [individuele databases beheren](sql-database-single-database-scale.md) en [elastische pools beheren](sql-database-elastic-pool.md).

## <a name="can-i-convert-a-database-from-a-vcore-based-service-tier-to-a-dtu-based-one"></a>Ik kan een database van een op vCore gebaseerde servicelaag converteren naar een op basis van een DTU

Ja, kunt u uw database omzetten naar een ondersteunde prestaties doelstelling met behulp van Azure portal, PowerShell, Azure CLI, T-SQL of de REST-API. Zie [individuele databases beheren](sql-database-single-database-scale.md) en [elastische pools beheren](sql-database-elastic-pool.md).

## <a name="can-i-upgrade-or-downgrade-between-the-general-purpose-and-business-critical-service-tiers"></a>Ik kan upgraden en downgraden tussen de lagen algemeen gebruik en bedrijfskritiek

Ja, met enkele beperkingen. De SKU van de bestemming moet voldoen aan de maximale database of elastische pool-grootte die u hebt geconfigureerd voor uw bestaande implementatie. Als u [Azure Hybrid Use Benefit voor SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md), de bedrijfskritieke SKU is alleen beschikbaar voor klanten met Enterprise Edition-licenties. Alleen klanten die zijn gemigreerd vanuit on-premises naar algemeen gebruik met behulp van Azure Hybrid Benefit voor SQL Server Enterprise Edition-licenties kunnen upgraden naar bedrijfskritiek. Zie voor meer informatie [wat zijn de specifieke rechten van de Azure Hybrid Use Benefit voor SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md)?

Deze conversie een hoeveelheid niet leidt tot downtime en kan worden gestart via Azure portal, PowerShell, Azure CLI, T-SQL of de REST-API. Zie [individuele databases beheren](sql-database-single-database-scale.md) en [elastische pools beheren](sql-database-elastic-pool.md).

## <a name="i-am-using-a-premium-rs-database-that-will-not-be-generally-available---can-i-upgrade-it-to-a-new-tier-and-achieve-a-similar-priceperformance-benefit"></a>Ik een Premium RS-database die niet beschikbaar zijn in het algemeen gebruik - kan ik een upgrade uitvoert naar een nieuwe laag en een soortgelijk prijs-prestatieverhouding voordeel behalen

Omdat het vCore-model onafhankelijke controle over de hoeveelheid ingerichte Computing en opslag kunt, kunt u effectiever beheren de kosten van de resulterende, waardoor het een aantrekkelijke bestemming voor Premium RS-databases. Bovendien de [Azure Hybrid Use Benefit voor SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md) biedt een aanzienlijke korting wanneer het vCore-model wordt gebruikt.

## <a name="how-often-can-i-adjust-the-resources-per-pool"></a>Hoe vaak kan ik de bronnen per groep van toepassingen aanpassen

Zo vaak als u wilt. Zie [elastische pools beheren](sql-database-elastic-pool.md).

## <a name="how-long-does-it-take-to-change-the-service-tier-or-compute-size-of-a-single-database-or-move-a-database-in-and-out-of-an-elastic-pool"></a>Hoe lang duurt het wijzigen van de servicelaag of compute van de grootte van een individuele database of een database verplaatsen naar en uit een elastische pool

Wijzigen van de servicelaag van een database en verplaatsen naar en uit een pool, moet de database worden gekopieerd op het platform als een achtergrondbewerking. Wijzigen van de servicelaag kan enkele minuten duren tot enkele uren, afhankelijk van de grootte van de databases. In beide gevallen worden in de databases online en beschikbaar blijven tijdens de verplaatsing. Zie voor meer informatie over het wijzigen van individuele databases [wijzigen van de servicelaag van een database](sql-database-service-tiers-dtu.md).

## <a name="when-should-i-use-a-single-database-vs-elastic-databases"></a>Wanneer moet ik een individuele database en elastische databases gebruiken

In het algemeen elastische pools zijn ontworpen voor een typische [software-as-a-service (SaaS)-toepassingspatroon](sql-database-design-patterns-multi-tenancy-saas-applications.md), waarbij er één database per klant of de tenant. Kopen van afzonderlijke databases en capaciteit om te voldoen aan de variabele vraag en de piekvraag voor elke database in te richten is vaak niet kostenefficiënt. Met pools, u de collectieve prestaties van de pool beheren en de databases omhoog en omlaag schalen automatisch. Intelligente engine van Azure beveelt aan een pool voor databases wanneer een gebruikspatroon gerechtvaardigd. Zie voor meer informatie, [elastische pool richtlijnen](sql-database-elastic-pool.md).

## <a name="how-does-the-usage-of-sql-database-using-the-dtu-based-purchasing-model-show-up-on-my-bill"></a>Hoe wordt het gebruik van SQL Database met behulp van het op DTU gebaseerde aankoopmodel weergegeven op mijn factuur

Facturen van de SQL-Database op een voorspelbaar uurtarief op basis van de [aanschaffen model](sql-database-service-tiers-dtu.md). Werkelijk gebruik wordt berekend en pro-rata per uur betaald, zodat uw rekening kunnen delen van een uur weergeven. Als een database 12 uur per maand bestaat, ziet uw factuur u bijvoorbeeld het gebruik van 0,5 dag.

## <a name="what-if-a-single-database-is-active-for-less-than-an-hour-or-uses-a-higher-service-tier-for-less-than-an-hour"></a>Wat gebeurt er als een individuele database korter dan een uur actief is of korter dan een uur een hogere servicelaag gebruikt

U worden in rekening gebracht voor elk uur bestaat in een database met behulp van de hoogste servicelaag + compute-grootte die tijdens dat uur, ongeacht het gebruik of of de database minder dan een uur actief is toegepast. Als u een individuele database maken en deze vijf minuten later verwijdert weerspiegelt uw factuur een post voor één database-uur.

Voorbeelden:

- Als u een Basic-database maakt en vervolgens meteen een naar Standard S1 upgrade, er worden in rekening gebracht tegen het tarief voor Standard S1 voor het eerste uur.
- Als u een database van Basic naar Premium om 10:00 uur bijwerken en de upgrade is voltooid om 1:35 uur de volgende dag u in rekening gebracht tegen het Premium-tarief begint bij 1:00 uur
- Als u een database van Premium naar standaard om 11:00 uur downgraden en het proces is voltooid om 2:15 uur en vervolgens de database wordt in rekening gebracht tegen het Premium-tarief tot en met 3:00 uur, waarna deze wordt in rekening gebracht tegen de Basic-tarieven.

## <a name="how-does-elastic-pool-usage-using-the-dtu-based-purchasing-model-show-up-on-my-bill"></a>Hoe wordt gebruik van elastische pool met behulp van het op DTU gebaseerde aankoopmodel weergegeven op mijn factuur

Elastische pool kosten weergeven om op uw factuur als elastische dtu's (edtu's) of vCores en opslag in de stappen die worden weergegeven op [de pagina met prijzen](https://azure.microsoft.com/pricing/details/sql-database/). Er zijn geen kosten per database voor elastische pools. U wordt gefactureerd voor elk uur dat een pool bestaat op het hoogste edtu's of vCores, ongeacht het gebruik of of de groep korter dan een uur actief was.

Op basis van DTU aankopen model voorbeelden:

- Als u een Standard elastische pool met minimaal 200 edtu's om 11:18 uur maken, vijf databases toe te voegen aan de groep, in rekening gebracht voor minimaal 200 edtu's voor het hele uur beginnen bij 11: 00 in de rest van de dag.
- Op dag 2 om 5:05 uur, 1-Database begint 50 edtu's verbruiken en onveranderlijk via de dag bevat. Databases 2-5 variëren tussen 0 en 80 edtu's. Gedurende de dag, moet u vijf andere databases die verschillende edtu's gedurende de hele dag verbruiken toevoegen. Dag 2 is een volledige dag in rekening gebracht op 200 eDTU.
- Op dag 3, 5 uur u toevoegen een andere 15 databases. Databasegebruik wordt verhoogd gedurende de dag op het punt waarop u besluit om te verhogen edtu's voor de groep van 200 tot 400 om 8:05 uur Kosten in rekening gebracht op het niveau van 200 edtu's zijn van kracht tot en met 8 uur en voor de resterende vier uur wordt verhoogd tot 400 edtu's.

## <a name="how-are-elastic-pool-billed-for-the-dtu-based-purchasing-model"></a>Hoe aanschaft elastische pool in rekening gebracht voor het op DTU gebaseerde model

Elastische pools worden gefactureerd via de volgende kenmerken:

- Een elastische pool wordt gefactureerd nadat deze de is gemaakt, zelfs wanneer er geen databases in de groep zijn.
- Een elastische pool is per uur gefactureerd. Dit is dezelfde softwarelicentiecontrole frequentie als voor de compute-grootten van individuele databases.
- Als een elastische pool wordt uitgebreid, klikt u vervolgens de groep is niet in rekening gebracht op basis van de nieuwe hoeveelheid resources tot het wijzigen van de grootte bewerking is voltooid. Dit volgt hetzelfde patroon als het wijzigen van de compute-grootte van afzonderlijke databases.
- De prijs van een elastische pool is gebaseerd op de resources van de groep. De prijs van een elastische pool is onafhankelijk van het aantal en het gebruik van de elastische databases binnen het.

Zie voor meer informatie, [prijzen van SQL Database](https://azure.microsoft.com/pricing/details/sql-database/), [DTU gebaseerde aankoopmodel](sql-database-service-tiers-dtu.md), en [vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md).

## <a name="how-does-the-vcore-based-usage-show-up-in-my-bill"></a>Hoe wordt het op vCore gebaseerde gebruik weergegeven op mijn factuur vermeld

In het vCore-model, de service gefactureerd met een voorspelbare en per uur op basis van de servicelaag, ingerichte Computing in vCores, ingerichte opslag in GB/maand en verbruikte back-upopslag. Als de opslag voor back-ups langer is dan de totale databasegrootte (dat wil zeggen, 100% van de databasegrootte), zijn er extra kosten verbonden. vCore-uren, geconfigureerde database-opslag, verbruikte i/o- en back-upopslag worden duidelijk per item vermeld in de factuur, waardoor het gemakkelijker wordt om de details van de resources die u hebt gebruikt. Back-upopslag tot 100% van de maximale databasegrootte is inbegrepen, dan dat u wordt gefactureerd GB/maand verbruikt in een maand.

Bijvoorbeeld:

- Als de SQL-database 12 uur in een maand bestaat, de factuur Hiermee wordt het gebruik van 12 uur Vcores. Als de SQL-database een extra 100 GB aan opslag hebt ingericht, bevat de factuur opslaggebruik in eenheden van GB/maand per uur naar rato en het aantal verbruikte in een maand.
- Als de SQL-database minder dan een uur actief is, in rekening gebracht voor elk uur dat de database bestaat met behulp van de hoogste geselecteerde servicelaag en de ingerichte opslag en i/o-die tijdens dat uur, ongeacht het gebruik of de database actief is geweest voor minder dan toegepast een uur.
- Als u een beheerd exemplaar maakt en dit vijf minuten later verwijdert, wordt u voor één database-uur gefactureerd.
- Als u een Managed Instance maakt in de categorie Algemeen gebruik met 8 vCores en vervolgens meteen een upgrade naar 16 vCores uitvoert, wordt voor het eerste uur het tarief voor 16 vCores in rekening gebracht.

> [!NOTE]
> Voor een beperkte periode, zijn de kosten voor back-up en i/o-kosten in rekening gebracht zijn gratis.

## <a name="how-does-the-use-of-active-geo-replication-in-an-elastic-pool-show-up-on-my-bill"></a>Hoe gaat het gebruik van actieve geo-replicatie in een elastische pool worden weergegeven op mijn factuur

In tegenstelling tot individuele databases, met behulp van [actieve geo-replicatie](sql-database-geo-replication-overview.md) met elastische databases een directe invloed facturering niet hebt.  U betaalt alleen voor de resources die zijn ingericht voor elk van de groepen (groep van primaire en secundaire groep)

## <a name="how-does-the-use-of-the-auditing-feature-impact-my-bill"></a>Hoe het gebruik van de auditingfunctie van invloed zijn op mijn factuur

Controle is ingebouwd in de service SQL Database zonder extra kosten en is beschikbaar op alle service-lagen. Echter, voor het opslaan van de auditlogboeken van de controle functie gebruikt die een Azure Storage-account en voor tabellen en wachtrijen in Azure Storage-tarieven zijn van toepassing op basis van de grootte van uw auditlogboek.

## <a name="how-do-i-reset-the-password-for-the-server-admin"></a>Hoe ik het wachtwoord voor de serverbeheerder opnieuw instellen

In de [Azure-portal](https://portal.azure.com), klikt u op **SQL-Servers**, selecteert u de server in de lijst en klik vervolgens op **wachtwoord opnieuw instellen**.

## <a name="how-do-i-manage-databases-and-logins"></a>Hoe kan ik databases en aanmeldingen beheren

Zie [databases en aanmeldingen beheren](sql-database-manage-logins.md).

> [!NOTE]
> U kunt de naam van het serverbeheerdersaccount niet wijzigen nadat deze is gemaakt.

## <a name="how-do-i-make-sure-only-authorized-ip-addresses-are-allowed-to-access-a-server"></a>Hoe maak ik ervoor dat alleen geautoriseerde IP-adressen zijn toegestaan voor toegang tot een server

Zie [hoe: firewallinstellingen voor SQL-Database configureren](sql-database-configure-firewall-settings.md).

## <a name="what-is-an-expected-replication-lag-when-geo-replicating-a-database-between-two-regions-within-the-same-azure-geography"></a>Wat is er een vertraging van het verwachte replicatie wanneer geo-replicatie een database tussen twee regio's binnen dezelfde Geografie Azure

We ondersteunen momenteel een RPO van vijf seconden en de vertraging van replicatie is minder dan dat wanneer de geo-secundaire wordt gehost in Azure gekoppelde regio aanbevolen en op dezelfde servicelaag.

## <a name="what-is-an-expected-replication-lag-when-geo-secondary-is-created-in-the-same-region-as-the-primary-database"></a>Wat is een verwachte replicatievertraging wanneer geo-secundaire in dezelfde regio bevinden als de primaire database wordt gemaakt

Op basis van empirische gegevens, is er niet te veel verschil tussen intra-regio en een vertraging van replicatie tussen regio's wanneer de door Azure aanbevolen gekoppelde regio wordt gebruikt.

## <a name="if-there-is-a-network-failure-between-two-regions-how-does-the-retry-logic-work-when-geo-replication-is-set-up"></a>Als er een netwerkstoring tussen twee regio's, hoe de logica voor opnieuw proberen werkt wanneer geo-replicatie is ingesteld

Als er een verbinding verbreken, wordt opnieuw geprobeerd aan elke tien seconden opnieuw om verbindingen te maken.

## <a name="what-can-i-do-to-guarantee-that-a-critical-change-on-the-primary-database-is-replicated"></a>Wat kan ik doen om ervoor te zorgen dat een cruciale veranderingen op de primaire database worden gerepliceerd

De geo-secundaire replica van een asynchrone is en probeer we niet te houden in de volledige synchronisatie met de primaire. Maar we bieden een methode voor het afdwingen van synchronisatie om te controleren of de replicatie van belangrijke wijzigingen (bijvoorbeeld bijwerken van wachtwoorden). Geforceerde synchronisatie heeft invloed op prestaties omdat de aanroepende thread geblokkeerd totdat alle doorgevoerde transacties worden gerepliceerd. Zie voor meer informatie, [sp_wait_for_database_copy_sync](https://msdn.microsoft.com/library/dn467644.aspx).

## <a name="what-tools-are-available-to-monitor-the-replication-lag-between-the-primary-database-and-geo-secondary"></a>Welke hulpprogramma's zijn beschikbaar voor het bewaken van de vertraging van replicatie tussen de primaire database en geo-secundaire database

We stellen de vertraging tussen de primaire database en geo-secundaire database via een DMV-realtime replicatie beschikbaar. Zie voor meer informatie, [sys.dm_geo_replication_link_status](https://msdn.microsoft.com/library/mt575504.aspx).

## <a name="to-move-a-database-to-a-different-server-in-the-same-subscription"></a>Een database te verplaatsen naar een andere server in hetzelfde abonnement

In de [Azure-portal](https://portal.azure.com), klikt u op **SQL-databases**, selecteert u een database in de lijst en klik vervolgens op **kopie**. Zie [een Azure SQL database kopiëren](sql-database-copy.md) voor meer informatie.

## <a name="to-move-a-database-between-subscriptions"></a>Een database verplaatsen tussen abonnementen

In de [Azure-portal](https://portal.azure.com), klikt u op **SQL-servers** en selecteer vervolgens de server die als host fungeert voor uw database in de lijst. Klik op **verplaatsen**, en kies vervolgens de resources wilt verplaatsen en het abonnement om naar te verplaatsen.