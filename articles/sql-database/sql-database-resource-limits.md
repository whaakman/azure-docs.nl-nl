---
title: Limieten voor Azure SQL Database | Microsoft Docs
description: Deze pagina beschrijft enkele algemene resource beperkingen voor Azure SQL Database.
services: sql-database
documentationcenter: na
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 884e519f-23bb-4b73-a718-00658629646a
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Active
ms.date: 01/12/2018
ms.author: carlrab
ms.openlocfilehash: 13641b190c3c157f5b302314f88a42a160a1f2e0
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/13/2018
---
# <a name="azure-sql-database-resource-limits"></a>Limieten voor Azure SQL Database

## <a name="single-database-storage-sizes-and-performance-levels"></a>Individuele database: opslaggrootte en prestatieniveaus

De volgende tabellen tonen voor individuele databases, de beschikbare resources voor één database op elke prijscategorie en prestatieniveau serviceniveau. U kunt de servicelaag, het prestatieniveau en de opslagruimte voor het gebruik van een individuele database instellen de [Azure-portal](sql-database-single-database-resources.md#manage-single-database-resources-using-the-azure-portal), [Transact-SQL](sql-database-single-database-resources.md#manage-single-database-resources-using-transact-sql), [PowerShell](sql-database-single-database-resources.md#manage-single-database-resources-using-powershell), de [Azure CLI](sql-database-single-database-resources.md#manage-single-database-resources-using-the-azure-cli), of de [REST-API](sql-database-single-database-resources.md#manage-single-database-resources-using-the-rest-api).

[!INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

## <a name="single-database-change-storage-size"></a>Individuele database: opslaggrootte wijzigen

- De DTU prijs voor één database bevat een bepaalde hoeveelheid opslag zonder extra kosten. Extra opslagruimte boven het inbegrepen bedrag kan worden ingericht voor extra kosten tot de maximale grootte bereikt, in stappen van 250 GB tot 1 TB en vervolgens in stappen van 256 GB dan 1 TB. Zie voor opgenomen opslag bedragen en limieten voor de maximale berichtgrootte [één database: opslaggrootte en prestatieniveaus](#single-database-storage-sizes-and-performance-levels).
- Extra opslag voor één database kan worden ingericht met de toename van het gebruik van de maximale grootte van de [Azure-portal](sql-database-single-database-resources.md#manage-single-database-resources-using-the-azure-portal), [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), wordt de [Azure CLI](/cli/azure/sql/db#az_sql_db_update), of de [REST-API](/rest/api/sql/databases/update).
- De prijs van extra opslag voor één database is de extra opslagruimte vermenigvuldigd met de eenheidsprijs extra opslagruimte van de servicetier. Zie voor meer informatie over de prijs van extra opslagruimte [prijzen SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="single-database-change-dtus"></a>Individuele database: dtu's wijzigen

Nadat u in eerste instantie een servicelaag, prestatieniveau en opslaghoeveelheid, u kunt een individuele database omhoog of omlaag schalen dynamisch op basis van het werkelijke ervaring met behulp van de [Azure-portal](sql-database-single-database-resources.md#manage-single-database-resources-using-the-azure-portal), [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), wordt de [Azure CLI](/cli/azure/sql/db#az_sql_db_update), of de [REST-API](/rest/api/sql/databases/update). 

De volgende video toont dynamisch wijzigen van de prestatielaag voor een verhoging van de beschikbare dtu's voor één database.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-dynamically-scale-up-or-scale-down/player]
>

Als u de servicelaag en/of het prestatieniveau van een database wijzigt, wordt er een replica van de oorspronkelijke database gemaakt op het nieuwe prestatieniveau en worden vervolgens de verbindingen overgeschakeld op de replica. Er gaan geen gegevens verloren tijdens dit proces, maar tijdens het korte moment waarop we overschakelen naar de replica, worden verbindingen met de database uitgeschakeld, zodat sommige actieve transacties kunnen worden teruggedraaid. De tijdsduur voor de overgeschakeld verschilt, maar wordt doorgaans onder 4 seconden is minder dan 30 seconden 99% van de tijd. Als er grote aantallen transacties tijdens de vlucht op het moment dat verbindingen zijn uitgeschakeld, wordt de tijdsduur voor de overgeschakeld mogelijk ook langer. 

De duur van het volledige proces voor omhoog schalen is afhankelijk van de grootte en de servicelaag van de database vóór en na de wijziging. Bijvoorbeeld, moet een 250 GB-database die wordt gewijzigd naar, uit of binnen een laag Standard-service uitvoeren binnen zes uur. Voor een database dezelfde grootte hebben prestatieniveaus binnen de Premium servicecategorie wordt gewijzigd, moet de scale-up uitvoeren binnen drie uur.

> [!TIP]
> Voor het bewaken van bewerkingen in-nu wordt uitgevoerd, Zie: [bewerkingen met de REST-API van SQL beheren](/rest/api/sql/Operations/List), [beheren met CLI](/cli/azure/sql/db/op), [bewaak de bewerkingen met T-SQL](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) en deze twee PowerShell-opdrachten: [Get-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/get-azurermsqldatabaseactivity) en [Stop AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/stop-azurermsqldatabaseactivity).

* Als u een naar een hogere laag of prestaties serviceniveau upgrade, wordt de maximale grootte van de database niet verhogen tenzij u expliciet een groter formaat (maxsize) opgeeft.
* Om het downgraden van een database, moet de ruimte van de database die wordt gebruikt kleiner zijn dan de maximaal toegestane grootte van het doel prijscategorie en prestatieniveau serviceniveau. 
* Wanneer het downgraden van **Premium** of **Premium RS** naar de **standaard** laag, een extra opslagkosten van toepassing als beide (1) de maximale grootte van de database wordt ondersteund in het doel prestatieniveau en (2) de maximale grootte overschrijdt de opgenomen opslaghoeveelheid van het prestatieniveau doel. Bijvoorbeeld, als een database P1 met een maximale grootte van 500 GB wordt verkleind aan S3, is een extra opslagkosten van toepassing omdat S3 een maximale grootte van 500 GB ondersteunt en de opgenomen opslaghoeveelheid slechts 250 GB is. De extra opslagruimte is dus 500 GB – 250 GB = 250 GB. Zie voor de prijzen van extra opslagruimte [prijzen SQL Database](https://azure.microsoft.com/pricing/details/sql-database/). Als de werkelijke hoeveelheid schijfruimte minder dan de opgenomen opslaghoeveelheid is, kan klikt u vervolgens dit extra kosten verbonden worden vermeden door de maximale grootte van de database voor het bedrag opgenomen te beperken. 
* Bij een upgrade van een database met [geo-replicatie](sql-database-geo-replication-portal.md) ingeschakeld, de secundaire databases upgraden naar de gewenste prestatielaag vóór de upgrade van de primaire database (algemene richtlijnen). Bij een upgrade naar een andere, is upgraden van de secundaire database eerst vereist.
* Wanneer een database met downgraden [geo-replicatie](sql-database-geo-replication-portal.md) ingeschakeld, de primaire databases naar de gewenste prestatielaag downgraden voordat het downgraden van de secundaire database (algemene richtlijnen). Wanneer het downgraden naar een andere editie, is de primaire database downgraden eerst vereist.
* De mogelijkheden om de service te herstellen verschillen voor de verschillende servicelagen. Als u uw abonnement tot beperkt de **Basic** laag, er is een lagere back-up bewaarperiode - Zie [back-ups van Azure SQL Database](sql-database-automated-backups.md).
* De nieuwe eigenschappen voor de database worden pas toegepast nadat de wijzigingen zijn voltooid.

## <a name="single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb"></a>Individuele database: beperkingen van P11 en P15 wanneer de maximale grootte groter is dan 1 TB

Een maximale grootte groter is dan 1 TB voor P11 en P15 database wordt ondersteund in de volgende gebieden: ons East2, VS-West, Gov ons Virginia, West-Europa, Duitsland centraal, Zuid-Oost-Azië, Japan-Oost, Australië-Oost, Canada centraal en Canada-Oost. De volgende overwegingen en beperkingen van toepassing op P11 en P15 databases met een maximale grootte groter is dan 1 TB:

- Als u kiest een maximale grootte groter is dan 1 TB bij het maken van een database (met behulp van een waarde van 4 TB en 4096 GB), mislukt de opdracht maken met een fout als de database in een niet-ondersteunde regio is ingericht.
- Voor bestaande P11 en P15 databases zich bevinden in een van de ondersteunde regio's, kunt u de maximale opslag naar dan 1 TB in stappen van 256 GB verhogen maximaal 4 TB. Als u wilt weten of een groter formaat wordt ondersteund in uw regio, gebruiken de [DATABASEPROPERTYEX](/sql/t-sql/functions/databasepropertyex-transact-sql) functioneren of controleren van de grootte van de database in de Azure portal. Upgraden van een bestaande P11 of P15 kan database alleen worden uitgevoerd door een principal-aanmelding op serverniveau of door leden van de databaserol dbmanager. 
- Als u een upgrade bewerking wordt uitgevoerd in een ondersteunde regio die wordt onmiddellijk worden bijgewerkt in de configuratie. De database blijft online tijdens de upgrade. U kan niet de volledige hoeveelheid opslag dan 1 TB aan opslag echter gebruiken als de eigenlijke databasebestanden zijn bijgewerkt naar de nieuwe maximale grootte. Voor de grootte van de database wordt bijgewerkt, is afhankelijk van de lengte van de tijd die nodig is. 
- Bij het maken of bijwerken van een database P11 of P15, kunt u alleen kiezen tussen de maximale grootte van 1 TB en 4 TB in stappen van 256 GB. Wanneer u een P11/P15 maakt, is de standaardoptie voor opslag van 1 TB vooraf geselecteerd. Voor de databases zich bevinden in een van de ondersteunde regio's, verhoogt u de maximale opslag voor maximaal 4 TB voor één nieuwe of bestaande database. Voor alle andere regio's, kan niet de maximale grootte worden verhoogd tot boven 1 TB. De prijs verandert niet wanneer u opgenomen opslag van 4 TB selecteert.
- Als de maximale grootte van een database is ingesteld op die groter is dan 1 TB, kan niet moet deze worden gewijzigd tot 1 TB zelfs als de feitelijke opslag gebruikt lager dan 1 TB is. Dus kunt niet u downgraden een P11 of P15 met een maximale grootte groter is dan 1 TB aan een 1 TB P11 of 1 TB P15 of lagere prestaties laag, zoals P1 P6). Deze beperking geldt ook voor het terugzetten en de kopie-scenario's punt in tijd, inclusief geo-restore lange-termijn-back-up-bewaren en database-exemplaar. Wanneer een database is geconfigureerd met een maximale grootte groter is dan 1 TB, moeten alle bewerkingen voor het herstellen van deze database worden uitgevoerd in een P11/P15 met een maximale grootte groter is dan 1 TB.
- Voor scenario's van actieve geo-replicatie:
   - Instellen van een relatie geo-replicatie: als de primaire database P11 of P15, de secondary(ies) ook moet P11 of P15; lagere prestatielagen worden geweigerd als de secundaire replica's omdat ze niet kunnen ondersteunen meer dan 1 TB.
   - Upgraden van de primaire database in een relatie geo-replicatie: de maximale grootte wijzigen naar meer dan 1 TB voor een primaire database dezelfde wijziging op de secundaire database wordt geactiveerd. Beide upgrades moeten zijn gelukt om de wijziging op de primaire pas van kracht zijn. Er gelden beperkingen van de regio voor de optie meer dan 1 TB. Als de secundaire zich in een regio die meer dan 1 TB niet ondersteunt, wordt de primaire niet bijgewerkt.
- Met de service voor het laden van P11/P15 databases met meer dan 1 TB voor importeren/exporteren wordt niet ondersteund. Gebruik SqlPackage.exe naar [importeren](sql-database-import.md) en [exporteren](sql-database-export.md) gegevens.

## <a name="elastic-pool-storage-sizes-and-performance-levels"></a>Elastische pool: opslaggrootte en prestatieniveaus

De volgende tabellen geven voor de elastische pools SQL-Database, de beschikbare bronnen op elke prijscategorie en prestatieniveau serviceniveau. U kunt instellen, de servicelaag, het prestatieniveau en de opslag met behulp van de mate waarin de [Azure-portal](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-the-azure-portal), [PowerShell](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-powershell), wordt de [Azure CLI](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-the-azure-cli), of de [REST-API](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-the-rest-api).

> [!NOTE]
> De limieten van de afzonderlijke databases in een elastische pools zijn in het algemeen hetzelfde als voor individuele databases buiten groepen op basis van dtu's en de servicetier. Bijvoorbeeld, is het maximumaantal gelijktijdige werknemers voor een database S2 120 werknemers. Het maximumaantal gelijktijdige werknemers voor een database in een Standard-pool is dus ook 120 werknemers als het maximale aantal DTU per database in de groep is 50 dtu's (dit is gelijk aan S2).
>

[!INCLUDE [SQL DB service tiers table for elastic pools](../../includes/sql-database-service-tiers-table-elastic-pools.md)]

Als alle DTU's van een elastische groep zijn gebruikt, ontvangt elke database in de groep een gelijke hoeveelheid resources om query's te verwerken. De service SQL Database verdeelt resources eerlijk over databases door gelijke hoeveelheden rekentijd te garanderen. Gelijke verdeling van resources in een elastische groep vindt plaats naast een bepaalde hoeveelheid resources die sowieso voor elke database wordt gegarandeerd wanneer het aantal DTU's per database is ingesteld op een andere waarde dan nul.

### <a name="database-properties-for-pooled-databases"></a>Database-eigenschappen voor gegroepeerde databases

De volgende tabel beschrijft de eigenschappen voor gegroepeerde databases.

| Eigenschap | Beschrijving |
|:--- |:--- |
| Maximaal aantal eDTU’s per database |Het maximum aantal eDTU's dat elke database in de groep mag gebruiken, is afhankelijk van het gebruik door andere databases in de groep. Het maximum aantal eDTU's per database is geen resourcegarantie voor een database. Het is een algemene instelling voor alle databases in de groep. Stel het maximum aantal eDTU's per database hoog genoeg zijn om pieken in databasegebruik te kunnen afhandelen. Enige mate van overtoewijzing is normaal, omdat de groep in het algemeen uitgaat van pieken en dalen in gebruikspatronen voor databases, waarbij alle databases niet tegelijkertijd pieken. Stel, het piekgebruik per database is twintig eDTU's en slechts 20% van de 100 databases in de groep pieken op hetzelfde moment. Als het maximum aantal eDTU's per database is ingesteld op twintig eDTU's, kan de groep redelijkerwijs vijf keer worden overtoegewezen en kan het aantal eDTU's per groep worden ingesteld op 400. |
| Minimaal aantal eDTU’s per database |Het minimum aantal eDTU's dat voor elke database in de groep wordt gegarandeerd. Het is een algemene instelling voor alle databases in de groep. Het minimum aantal eDTU’s per database kan worden ingesteld op 0. Dat is ook de standaardwaarde. Deze eigenschap wordt ingesteld op een waarde tussen 0 en het gemiddelde eDTU-gebruik per database. Het product van het aantal databases in de groep en het minimum aantal eDTU's per database mag niet groter zijn dan het aantal eDTU's per groep. Als een groep bijvoorbeeld twintig databases heeft en het minimum aantal eDTU's per database is ingesteld op tien eDTU's, dan moet het aantal eDTU's per groep minimaal 200 eDTU's zijn. |
| Maximale opslag per database |De maximale opslag voor een database in een groep. Databases in pools delen de poolopslag. Database-opslag is daarom beperkt tot de resterende poolopslag en de maximumopslag per database, afhankelijk van wat het kleinste is. De maximumopslag per database verwijst naar de maximale opslag van de gegevensbestanden, exclusief de ruimte die logboekbestanden innemen. |
|||
 
## <a name="elastic-pool-change-storage-size"></a>Elastische pool: opslaggrootte wijzigen

- De prijs van de eDTU voor een elastische pool bevat een bepaalde hoeveelheid opslag zonder extra kosten. Extra opslagruimte boven het inbegrepen bedrag kan worden ingericht voor extra kosten tot de maximale grootte bereikt, in stappen van 250 GB tot 1 TB en vervolgens in stappen van 256 GB dan 1 TB. Zie voor opgenomen opslag bedragen en limieten voor de maximale berichtgrootte [elastische pool: opslaggrootte en prestatieniveaus](#elastic-pool-storage-sizes-and-performance-levels).
- Extra opslag voor een elastische pool kan worden ingericht met de toename van het gebruik van de maximale grootte van de [Azure-portal](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-the-azure-portal), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqlelasticpool), wordt de [Azure CLI](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_update), of de [REST-API ](/rest/api/sql/elasticpools/update).
- De prijs van extra opslagruimte voor een elastische pool is de extra opslagruimte vermenigvuldigd met de eenheidsprijs extra opslagruimte van de servicetier. Zie voor meer informatie over de prijs van extra opslagruimte [prijzen SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="elastic-pool-change-edtus"></a>Elastische pool: edtu's wijzigen

U kunt vergroten of verkleinen van de beschikbare bronnen voor een elastische pool op basis van de resource die behoeften met behulp van de [Azure-portal](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-the-azure-portal), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqlelasticpool), wordt de [Azure CLI](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_update), of de [ REST-API](/rest/api/sql/elasticpools/update).

- Wanneer schaling aanpassen groepen met edtu's, worden de databaseverbindingen kort verwijderd. Dit is hetzelfde gedrag als deze gebeurtenis treedt op wanneer schaling dtu's aanpassen voor een individuele database (niet in een groep). Zie voor meer informatie over de duur en de gevolgen van het verbroken verbindingen voor een database tijdens bewerkingen schaling aanpassen [dtu's schaling aanpassen voor een individuele database](#single-database-change-storage-size). 
- De duur van groepen met edtu's oorspronkelijke kan afhankelijk van de totale hoeveelheid opslagruimte die wordt gebruikt door alle databases in de groep. In het algemeen de rescaling latentie gemiddelde 90 minuten of minder per 100 GB. Bijvoorbeeld, als de totale ruimte gebruikt door alle databases in de pool is 200 GB dan drie uur is de verwachte latentie voor schaling aanpassen van de groep of minder. In sommige gevallen binnen de categorie Standard of Basic mag de rescaling latentie onder vijf minuten ongeacht de hoeveelheid ruimte die wordt gebruikt.
- De duur voor het wijzigen van de edtu min's per database of max edtu's per database is in het algemeen vijf minuten of minder.
- Wanneer downsizing groepen met edtu's, moet de toepassingen die wordt gebruikt-ruimte kleiner zijn dan de maximaal toegestane grootte van de doel-service tier en de pool edtu's.
- Wanneer schaling groepen met edtu's aanpassen, wordt een extra opslagkosten geldt als (1) de maximale opslaggrootte van de groep wordt ondersteund door de doelgroep en (2) de maximale opslaggrootte de opgenomen opslaghoeveelheid van de doelgroep overschrijdt. Bijvoorbeeld, als een 100 eDTU Standard-pool met een maximale grootte van 100 GB aan een 50 eDTU Standard-pool wordt verkleind, is een extra opslagkosten van toepassing omdat doelgroep een maximale grootte van 100 GB ondersteunt en de opgenomen opslaghoeveelheid slechts 50 GB is. De extra opslagruimte is dus 100 – 50 GB = 50 GB. Zie voor de prijzen van extra opslagruimte [prijzen SQL Database](https://azure.microsoft.com/pricing/details/sql-database/). Als de werkelijke hoeveelheid schijfruimte minder dan de opgenomen opslaghoeveelheid is, kan klikt u vervolgens dit extra kosten verbonden worden vermeden door de maximale grootte van de database voor het bedrag opgenomen te beperken. 

## <a name="what-happens-when-database-and-elastic-pool-resource-limits-are-reached"></a>Wat gebeurt er wanneer de database en limieten elastische pool wordt bereikt?

### <a name="compute-dtus-and-edtus"></a>Berekenen (dtu's en edtu's)

Wanneer database compute-gebruik (gemeten door dtu's en edtu's) toeneemt, query uitvoeren op de toename in latentie en kan zelfs time-out. In deze omstandigheden query's kunnen in de wachtrij geplaatst door de service en vindt u bronnen voor uitvoering als resource komen.
Wanneer zich voordoen tijdens een hoge compute-gebruik, wordt een risicobeperking opties zijn onder andere:

- Het prestatieniveau van de database of de elastische groep om te voorzien meer dtu's of edtu's van de database te vergroten. Zie [één database: dtu's wijzigen](#single-database-change-dtus) en [elastische pool: wijzigen edtu's](#elastic-pool-change-edtus).
- Query's te verlagen van het Resourcegebruik van elke query te optimaliseren. Zie voor meer informatie [Query afstemmen/Hinting](sql-database-performance-guidance.md#query-tuning-and-hinting).

### <a name="storage"></a>Storage

Wanneer databaseruimte gebruikt de maximale limiet bereikt, database worden ingevoegd en updates die Verhoog de gegevensgrootte niet als clients ontvangen een [foutbericht](sql-database-develop-error-messages.md). Database selecteert en VERWIJDERINGEN blijven slagen.

Wanneer zich voordoen tijdens een hoge ruimtegebruik, wordt een risicobeperking opties zijn onder andere:

- De maximale grootte van de database of elastische pool, of wijzig het prestatieniveau verkrijgen van meer opgenomen opslag verhogen. Zie [limieten voor SQL Database](sql-database-resource-limits.md).
- Als de database zich in een elastische groep, kan klikt u vervolgens ook de database worden verplaatst buiten de groep zodat de opslagruimte wordt niet gedeeld met andere databases.

### <a name="sessions-and-workers-requests"></a>Sessies en werknemers (aanvragen) 

Het maximum aantal gelijktijdige sessies en werknemers worden bepaald door de prijscategorie en prestatieniveau serviceniveau (dtu's en edtu's).  Nieuwe aanvragen worden geweigerd als sessie- of worker limieten zijn bereikt en clients een foutbericht weergegeven wordt. Terwijl het aantal verbindingen beschikbaar kan worden beheerd door de toepassing, is het aantal gelijktijdige werknemers vaak moeilijker te schatten en te beheren. Dit is vooral van toepassing tijdens de piekuren van load wanneer database limieten zijn bereikt en werknemers opstapelen vanwege langer actieve query's. 

Wanneer zich voordoen tijdens een hoog gebruik van sessie- of worker, wordt een risicobeperking opties zijn onder andere:
- Het serviceniveau voor laag of de prestaties van de database of elastische pool te vergroten. Zie [één database: opslaggrootte wijzigen](#single-database-change-storage-size), [één database: dtu's wijzigen](#single-database-change-dtus), [elastische pool: opslaggrootte wijzigen](#elastic-pool-change-storage-size), en [elastische pool: edtu's wijzigen ](#elastic-pool-change-edtus).
- Optimaliseren query's te verlagen van het Resourcegebruik van elke query als de oorzaak van het gebruik van verhoogde worker vanwege conflicten over berekenen bronnen. Zie voor meer informatie [Query afstemmen/Hinting](sql-database-performance-guidance.md#query-tuning-and-hinting).

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over Servicelagen [Servicelagen](sql-database-service-tiers.md).
- Zie voor meer informatie over individuele databases [databaseresources eenmalige](sql-database-resource-limits.md).
- Zie voor meer informatie over elastische pools [elastische pools](sql-database-elastic-pool.md).
- Zie voor meer informatie over de algemene Azure limieten [Azure-abonnement en Servicelimieten, quota's en beperkingen](../azure-subscription-service-limits.md).
- Zie voor meer informatie over dtu's en edtu's [dtu's en edtu's](sql-database-what-is-a-dtu.md).
- Zie https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database voor informatie over de maximale grootte tempdb.
