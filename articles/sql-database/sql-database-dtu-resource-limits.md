---
title: Azure resource op basis van SQL Database DTU-limieten | Microsoft Docs
description: Deze pagina beschrijft een aantal algemene resource op basis van het DTU-limieten voor Azure SQL Database.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: article
ms.date: 04/04/2018
ms.author: carlrab
ms.openlocfilehash: c4c85395856756e8ec6a788aa958b479a297892d
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
# <a name="azure-sql-database-dtu-based-resource-model-limits"></a>Limieten voor het model van Azure SQL Database DTU gebaseerde-resource

> [!IMPORTANT]
> Zie voor ondergrenzen voor de resource op basis van vCore [SQL-Database op basis van vCore limieten](sql-database-vcore-resource-limits.md).

## <a name="single-database-storage-sizes-and-performance-levels"></a>Individuele database: opslaggrootte en prestatieniveaus

De volgende tabellen tonen voor individuele databases, de beschikbare resources voor één database op elke prijscategorie en prestatieniveau serviceniveau. U kunt de servicelaag, het prestatieniveau en de opslagruimte voor het gebruik van een individuele database instellen de [Azure-portal](sql-database-single-database-resources.md#manage-single-database-resources-using-the-azure-portal), [Transact-SQL](sql-database-single-database-resources.md#manage-single-database-resources-using-transact-sql), [PowerShell](sql-database-single-database-resources.md#manage-single-database-resources-using-powershell), de [Azure CLI](sql-database-single-database-resources.md#manage-single-database-resources-using-the-azure-cli), of de [REST-API](sql-database-single-database-resources.md#manage-single-database-resources-using-the-rest-api).

### <a name="basic-service-tier"></a>Basisservicelaag
| **Prestatieniveau** | **Basic** |
| :--- | --: |
| Maximaal aantal DTU's | 5 |
| Opgenomen opslag (GB) | 2 |
| Maximum aantal opslagopties (GB) | 2 |
| Maximale OLTP-opslag in het geheugen (GB) |N/A |
| Maximum aantal gelijktijdige werknemers (aanvragen) | 30 |
| Maximaal aantal gelijktijdige aanmeldingen | 30 |
| Maximaal aantal gelijktijdige sessies | 300 |
|||

### <a name="standard-service-tier"></a>Standaardservicelaag
| **Prestatieniveau** | **S0** | **S1** | **S2** | **S3** |
| :--- |---:| ---:|---:|---:|---:|
| Maximale aantal dtu's ** | 10 | 20 | 50 | 100 |
| Opgenomen opslag (GB) | 250 | 250 | 250 | 250 |
| Maximum aantal opslagopties (GB) * | 250 | 250 | 250 | 250, 500, 750, 1024 |
| Maximale OLTP-opslag in het geheugen (GB) | N/A | N/A | N/A | N/A |
| Maximum aantal gelijktijdige werknemers (aanvragen)| 60 | 90 | 120 | 200 |
| Maximaal aantal gelijktijdige aanmeldingen | 60 | 90 | 120 | 200 |
| Maximaal aantal gelijktijdige sessies |600 | 900 | 1200 | 2400 |
||||||

### <a name="standard-service-tier-continued"></a>Standaard-servicelaag (vervolg)
| **Prestatieniveau** | **S4** | **S6** | **S7** | **S9** | **S12** |
| :--- |---:| ---:|---:|---:|---:|---:|
| Maximale aantal dtu's ** | 200 | 400 | 800 | 1600 | 3000 |
| Opgenomen opslag (GB) | 250 | 250 | 250 | 250 | 250 |
| Maximum aantal opslagopties (GB) * | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 |
| Maximale OLTP-opslag in het geheugen (GB) | N/A | N/A | N/A | N/A |N/A |
| Maximum aantal gelijktijdige werknemers (aanvragen)| 400 | 800 | 1600 | 3200 |6000 |
| Maximaal aantal gelijktijdige aanmeldingen | 400 | 800 | 1600 | 3200 |6000 |
| Maximaal aantal gelijktijdige sessies |4800 | 9600 | 19200 | 30.000 |30.000 |
|||||||

### <a name="premium-service-tier"></a>Premium servicelaag 
| **Prestatieniveau** | **P1** | **P2** | **P4** | **P6** | **P11** | **P15** | 
| :--- |---:|---:|---:|---:|---:|---:|
| Maximaal aantal DTU's | 125 | 250 | 500 | 1000 | 1750 | 4000 |
| Opgenomen opslag (GB) | 500 | 500 | 500 | 500 | 4096 | 4096 |
| Maximum aantal opslagopties (GB) * | 500, 750, 1024 | 500, 750, 1024 | 500, 750, 1024 | 500, 750, 1024 | 4096 | 4096 |
| Maximale OLTP-opslag in het geheugen (GB) | 1 | 2 | 4 | 8 | 14 | 32 |
| Maximum aantal gelijktijdige werknemers (aanvragen)| 200 | 400 | 800 | 1600 | 2400 | 6400 |
| Maximaal aantal gelijktijdige aanmeldingen | 200 | 400 | 800 | 1600 | 2400 | 6400 |
| Maximaal aantal gelijktijdige sessies | 30.000 | 30.000 | 30.000 | 30.000 | 30.000 | 30.000 |
|||||||


> [!IMPORTANT]
> \* Opslagruimten groter dan de hoeveelheid inbegrepen opslagruimte zijn in preview en hiervoor gelden extra kosten. Zie [de prijsinformatie voor SQL Database](https://azure.microsoft.com/pricing/details/sql-database/) voor meer informatie. 
>
>\* In de Premium-laag is momenteel ruim 1 TB opslag beschikbaar voor de volgende regio's: Australië - oost, Australië - zuidoost, Brazilië - zuid, Canada - centraal, Canada - oost, VS - midden, Frankrijk - centraal, Duitsland - centraal, Japan - oost, Japan - west, Korea - centraal, VS Noord-Centraal, Noord-Europa, VS Zuid-Centraal, Zuidoost-Azië, UK - zuid, UK - west, VS - oost2, VS - west, VS (overheid) - Virginia, en West-Europa. Zie [P11-P15: huidige beperkingen](#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  
> 
>\*\* Maximale aantal dtu's per database starten op 200 dtu's en hoger in de standaardmodus zijn Preview-versie.
>


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
* Wanneer het downgraden van **Premium** naar de **standaard** laag, een extra opslagkosten van toepassing als zowel (1) de maximale grootte van de database wordt ondersteund in de doel-prestatieniveau en (2) de maximale grootte overschrijdt de opgenomen opslagruimte van het prestatieniveau doel. Bijvoorbeeld, als een database P1 met een maximale grootte van 500 GB wordt verkleind aan S3, is een extra opslagkosten van toepassing omdat S3 een maximale grootte van 500 GB ondersteunt en de opgenomen opslaghoeveelheid slechts 250 GB is. De extra opslagruimte is dus 500 GB – 250 GB = 250 GB. Zie voor de prijzen van extra opslagruimte [prijzen SQL Database](https://azure.microsoft.com/pricing/details/sql-database/). Als de werkelijke hoeveelheid schijfruimte minder dan de opgenomen opslaghoeveelheid is, kan klikt u vervolgens dit extra kosten verbonden worden vermeden door de maximale grootte van de database voor het bedrag opgenomen te beperken. 
* Bij een upgrade van een database met [geo-replicatie](sql-database-geo-replication-portal.md) ingeschakeld, de secundaire databases upgraden naar de gewenste prestatielaag vóór de upgrade van de primaire database (algemene richtlijnen voor de beste prestaties). Bij een upgrade naar een andere, is upgraden van de secundaire database eerst vereist.
* Wanneer een database met downgraden [geo-replicatie](sql-database-geo-replication-portal.md) ingeschakeld, de primaire databases naar de gewenste prestatielaag downgraden voordat het downgraden van de secundaire database (algemene richtlijnen voor de beste prestaties). Wanneer het downgraden naar een andere editie, is de primaire database downgraden eerst vereist.
* De mogelijkheden om de service te herstellen verschillen voor de verschillende servicelagen. Als u uw abonnement tot beperkt de **Basic** laag, er is een lagere back-up bewaarperiode - Zie [back-ups van Azure SQL Database](sql-database-automated-backups.md).
* De nieuwe eigenschappen voor de database worden pas toegepast nadat de wijzigingen zijn voltooid.

## <a name="single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb"></a>Individuele database: beperkingen van P11 en P15 wanneer de maximale grootte groter is dan 1 TB

Een maximale grootte groter is dan 1 TB voor P11 en P15 database wordt ondersteund in de volgende gebieden: Australië-Oost, Australië-Zuidoost, Brazilië-Zuid, Canada centraal, Canada Oost, VS-midden, Frankrijk centraal, Duitsland centraal, Japan-Oost, Japan-West, Korea Central, Noord-centraal VS, Noord-Europa, Zuid-centraal VS, Zuidoost-Azië, VK Zuid, VK West, VS East2, VS-West, VS Gov Virginia en West-Europa. De volgende overwegingen en beperkingen van toepassing op P11 en P15 databases met een maximale grootte groter is dan 1 TB:

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

### <a name="basic-elastic-pool-limits"></a>Basisbeperkingen voor elastische groepen

| eDTU's per groep | **50** | **100** | **200** | **300** | **400** | **800** | **1200** | **1600** |
|:---|---:|---:|---:| ---: | ---: | ---: | ---: | ---: |
| Opgenomen opslag per groep van toepassingen (GB) | 5 | 10 | 20 | 29 | 39 | 78 | 117 | 156 |
| Maximum aantal opslagopties per groep (GB) | 5 | 10 | 20 | 29 | 39 | 78 | 117 | 156 |
| Maximum aantal In het geheugen OLTP-opslag per pool (GB) | N/A | N/A | N/A | N/A | N/A | N/A | N/A | N/A |
| Maximaal aantal databases per pool | 100 | 200 | 500 | 500 | 500 | 500 | 500 | 500 |
| Maximaal aantal gelijktijdige werknemers (aanvragen) per pool | 100 | 200 | 400 | 600 | 800 | 1600 | 2400 | 3200 |
| Maximaal aantal gelijktijdige aanmeldingen per pool | 100 | 200 | 400 | 600 | 800 | 1600 | 2400 | 3200 |
| Maximaal aantal gelijktijdige sessies per pool | 30.000 | 30.000 | 30.000 | 30.000 |30.000 | 30.000 | 30.000 | 30.000 |
| Opties voor min-edtu's per database | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 |
| Opties van de Max edtu's per database | 5 | 5 | 5 | 5 | 5 | 5 | 5 | 5 |
| Maximale opslag per database (GB) | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 
||||||||

### <a name="standard-elastic-pool-limits"></a>Standaardbeperkingen voor elastische pools

| eDTU's per groep | **50** | **100** | **200** | **300** | **400** | **800**| 
|:---|---:|---:|---:| ---: | ---: | ---: | 
| Opgenomen opslag per groep van toepassingen (GB) | 50 | 100 | 200 | 300 | 400 | 800 | 
| Maximum aantal opslagopties per groep (GB) * | 50, 250, 500 | 100, 250, 500, 750 | 200, 250, 500, 750, 1024 | 300, 500, 750, 1024, 1280 | 400, 500, 750, 1024, 1280, 1536 | 800, 1024, 1280, 1536, 1792, 2048 | 
| Maximum aantal In het geheugen OLTP-opslag per pool (GB) | N/A | N/A | N/A | N/A | N/A | N/A | 
| Maximaal aantal databases per pool | 100 | 200 | 500 | 500 | 500 | 500 | 
| Maximaal aantal gelijktijdige werknemers (aanvragen) per pool | 100 | 200 | 400 | 600 | 800 | 1600 |
| Maximaal aantal gelijktijdige aanmeldingen per pool | 100 | 200 | 400 | 600 | 800 | 1600 |
| Maximaal aantal gelijktijdige sessies per pool | 30.000 | 30.000 | 30.000 | 30.000 | 30.000 | 30.000 |
| Opties voor min-edtu's per database | 0, 10, 20, 50 | 0, 10, 20, 50, 100 | 0, 10, 20, 50, 100, 200 | 0, 10, 20, 50, 100, 200, 300 | 0, 10, 20, 50, 100, 200, 300, 400 | 0, 10, 20, 50, 100, 200, 300, 400, 800 |
| Opties van de Max edtu's per database | 10, 20, 50 | 10, 20, 50, 100 | 10, 20, 50, 100, 200 | 10, 20, 50, 100, 200, 300 | 10, 20, 50, 100, 200, 300, 400 | 10, 20, 50, 100, 200, 300, 400, 800 | 
| Maximale opslag per database (GB)* | 500 | 750 | 1024 | 1024 | 1024 | 1024 |
||||||||

### <a name="standard-elastic-pool-limits-continued"></a>Limieten voor Standard elastische pools (vervolg) 

| eDTU's per groep | **1200** | **1600** | **2000** | **2500** | **3000** |
|:---|---:|---:|---:| ---: | ---: |
| Opgenomen opslag per groep van toepassingen (GB) | 1200 | 1600 | 2000 | 2500 | 3000 | 
| Maximum aantal opslagopties per groep (GB) * | 1200, 1280, 1536, 1792, 2048, 2304, 2560 | 1600, 1792, 2048, 2304, 2560, 2816, 3072 | 2000, 2048, 2304, 2560, 2816, 3072, 3328, 3584 | 2500, 2560, 2816, 3072, 3328, 3584, 3840, 4096 | 3000, 3072, 3328, 3584, 3840, 4096 |
| Maximum aantal In het geheugen OLTP-opslag per pool (GB) | N/A | N/A | N/A | N/A | N/A | 
| Maximaal aantal databases per pool | 500 | 500 | 500 | 500 | 500 | 
| Maximaal aantal gelijktijdige werknemers (aanvragen) per pool | 2400 | 3200 | 4000 | 5000 | 6000 |
| Maximaal aantal gelijktijdige aanmeldingen per pool | 2400 | 3200 | 4000 | 5000 | 6000 |
| Maximaal aantal gelijktijdige sessies per pool | 30.000 | 30.000 | 30.000 | 30.000 | 30.000 | 
| Opties voor min-edtu's per database | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500, 3000 |
| Opties van de Max edtu's per database | 10, 20, 50, 100, 200, 300, 400, 800, 1200 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500, 3000 | 
| Maximum aantal opslagopties per database (GB) * | 1024 | 1024 | 1024 | 1024 | 1024 | 
||||||||

### <a name="premium-elastic-pool-limits"></a>Limieten voor Premium elastische pools

| eDTU's per groep | **125** | **250** | **500** | **1000** | **1500**| 
|:---|---:|---:|---:| ---: | ---: | 
| Opgenomen opslag per groep van toepassingen (GB) | 250 | 500 | 750 | 1024 | 1536 | 
| Maximum aantal opslagopties per groep (GB) * | 250, 500, 750, 1024 | 500, 750, 1024 | 750, 1024 | 1024 | 1536 |
| Maximum aantal In het geheugen OLTP-opslag per pool (GB) | 1 | 2 | 4 | 10 | 12 | 
| Maximaal aantal databases per pool | 50 | 100 | 100 | 100 | 100 | 
| Maximaal aantal gelijktijdige werknemers per pool (aanvragen) | 200 | 400 | 800 | 1600 | 2400 | 
| Maximaal aantal gelijktijdige aanmeldingen per pool | 200 | 400 | 800 | 1600 | 2400 |
| Maximaal aantal gelijktijdige sessies per pool | 30.000 | 30.000 | 30.000 | 30.000 | 30.000 | 
| Minimaal aantal eDTU’s per database | 0, 25, 50, 75, 125 | 0, 25, 50, 75, 125, 250 | 0, 25, 50, 75, 125, 250, 500 | 0, 25, 50, 75, 125, 250, 500, 1000 | 0, 25, 50, 75, 125, 250, 500, 1000, 1500 | 
| Maximaal aantal eDTU’s per database | 25, 50, 75, 125 | 25, 50, 75, 125, 250 | 25, 50, 75, 125, 250, 500 | 25, 50, 75, 125, 250, 500, 1000 | 25, 50, 75, 125, 250, 500, 1000, 1500 |
| Maximale opslag per database (GB)* | 1024 | 1024 | 1024 | 1024 | 1024 | 
||||||||

### <a name="premium-elastic-pool-limits-continued"></a>Limieten voor Premium elastische pools (vervolg) 

| eDTU's per groep | **2000** | **2500** | **3000** | **3500** | **4000**|
|:---|---:|---:|---:| ---: | ---: | 
| Opgenomen opslag per groep van toepassingen (GB) | 2048 | 2560 | 3072 | 3548 | 4096 |
| Maximum aantal opslagopties per groep (GB) * | 2048 | 2560 | 3072 | 3548 | 4096|
| Maximum aantal In het geheugen OLTP-opslag per pool (GB) | 16 | 20 | 24 | 28 | 32 |
| Maximaal aantal databases per pool | 100 | 100 | 100 | 100 | 100 | 
| Maximaal aantal gelijktijdige werknemers (aanvragen) per pool | 3200 | 4000 | 4800 | 5600 | 6400 |
| Maximaal aantal gelijktijdige aanmeldingen per pool | 3200 | 4000 | 4800 | 5600 | 6400 |
| Maximaal aantal gelijktijdige sessies per pool | 30.000 | 30.000 | 30.000 | 30.000 | 30.000 | 
| Opties voor min-edtu's per database | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750, 4000 | 
| Opties van de Max edtu's per database | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750, 4000 | 
| Maximale opslag per database (GB)* | 1024 | 1024 | 1024 | 1024 | 1024 | 
||||||||

> [!IMPORTANT]
> \* Opslagruimten groter dan de hoeveelheid inbegrepen opslagruimte zijn in preview en hiervoor gelden extra kosten. Zie voor meer informatie de [pagina met prijzen SQL-Database](https://azure.microsoft.com/pricing/details/sql-database/). Opslag groter zijn dan de hoeveelheid opslagruimte die opgenomen zijn in preview en extra kosten gelden. Zie voor meer informatie de [pagina met prijzen SQL-Database](https://azure.microsoft.com/pricing/details/sql-database/).
>
> \* In de Premium-laag is momenteel ruim 1 TB opslag beschikbaar voor de volgende regio's: Australië - oost, Australië - zuidoost, Brazilië - zuid, Canada - centraal, Canada - oost, VS - midden, Frankrijk - centraal, Duitsland - centraal, Japan - oost, Japan - west, Korea - centraal, VS Noord-Centraal, Noord-Europa, VS Zuid-Centraal, Zuidoost-Azië, UK - zuid, UK - west, VS - oost2, VS - west, VS (overheid) - Virginia, en West-Europa. Zie [P11-P15: huidige beperkingen](#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  
>

Als alle DTU's van een elastische groep zijn gebruikt, ontvangt elke database in de groep een gelijke hoeveelheid resources om query's te verwerken. De service SQL Database verdeelt resources eerlijk over databases door gelijke hoeveelheden rekentijd te garanderen. Gelijke verdeling van resources in een elastische groep vindt plaats naast een bepaalde hoeveelheid resources die sowieso voor elke database wordt gegarandeerd wanneer het aantal DTU's per database is ingesteld op een andere waarde dan nul.

### <a name="database-properties-for-pooled-databases"></a>Database-eigenschappen voor gegroepeerde databases

De volgende tabel beschrijft de eigenschappen voor gegroepeerde databases.

| Eigenschap | Beschrijving |
|:--- |:--- |
| Maximaal aantal eDTU’s per database |Het maximum aantal eDTU's dat elke database in de groep mag gebruiken, is afhankelijk van het gebruik door andere databases in de groep. Het maximum aantal eDTU's per database is geen resourcegarantie voor een database. Het is een algemene instelling voor alle databases in de groep. Stel het maximum aantal eDTU's per database hoog genoeg zijn om pieken in databasegebruik te kunnen afhandelen. Enige mate van overtoewijzing is normaal, omdat de groep in het algemeen uitgaat van pieken en dalen in gebruikspatronen voor databases, waarbij alle databases niet tegelijkertijd pieken. Stel, het piekgebruik per database is twintig eDTU's en slechts 20% van de 100 databases in de groep pieken op hetzelfde moment. Als het maximum aantal eDTU's per database is ingesteld op twintig eDTU's, kan de groep redelijkerwijs vijf keer worden overtoegewezen en kan het aantal eDTU's per groep worden ingesteld op 400. |
| Minimaal aantal eDTU’s per database |Het minimum aantal eDTU's dat voor elke database in de groep wordt gegarandeerd. Het is een algemene instelling voor alle databases in de groep. Het minimum aantal eDTU’s per database kan worden ingesteld op 0. Dat is ook de standaardwaarde. Deze eigenschap wordt ingesteld op een waarde tussen 0 en het gemiddelde eDTU-gebruik per database. Het product van het aantal databases in de groep en het minimum aantal eDTU's per database mag niet groter zijn dan het aantal eDTU's per groep. Als een groep bijvoorbeeld twintig databases heeft en het minimum aantal eDTU's per database is ingesteld op tien eDTU's, dan moet het aantal eDTU's per groep minimaal 200 eDTU's zijn. |
| Maximale opslag per database |De maximale databasegrootte ingesteld door de gebruiker voor een database in een pool. Gegroepeerde databases Groepsopslag wordt gedeeld door toegewezen, zodat de grootte van een database kan bereiken beperkt tot is de kleinste van de resterende servergroep van opslag en grootte van de database. Maximale grootte van de database verwijst naar de maximale grootte van de gegevensbestanden en omvat niet de ruimte die wordt gebruikt door logboekbestanden. |
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

## <a name="what-is-the-maximum-number-of-servers-and-databases"></a>Wat is het maximum aantal servers en databases?

| Maximum | Waarde |
| :--- | :--- |
| Databases per server | 5000 |
| Aantal servers per abonnement per regio | 20 |
|||

> [!IMPORTANT]
> Als het aantal databases de limiet is per server nadert, kan het volgende gebeuren:
> <br> • Verhogen latentie in met het uitvoeren van query's in de database master.  Het gaat hierbij om weergaven van de gebruiksstatistieken resource zoals sys.resource_stats.
> <br> • Latentie in beheerbewerkingen vergroten en opbouwen van de portal standpunten die betrekking hebben op inventariseren van de server-databases.

## <a name="what-happens-when-database-and-elastic-pool-resource-limits-are-reached"></a>Wat gebeurt er wanneer de database en limieten elastische pool wordt bereikt?

### <a name="compute-dtus-and-edtus"></a>Berekenen (dtu's en edtu's)

Wanneer database compute-gebruik (gemeten door dtu's en edtu's) toeneemt, query uitvoeren op de toename in latentie en kan zelfs time-out. In deze omstandigheden query's kunnen in de wachtrij geplaatst door de service en vindt u bronnen voor uitvoering als resource komen.
Wanneer zich voordoen tijdens een hoge compute-gebruik, wordt een risicobeperking opties zijn onder andere:

- Het prestatieniveau van de database of de elastische groep om te voorzien meer dtu's of edtu's van de database te vergroten. Zie [één database: dtu's wijzigen](#single-database-change-dtus) en [elastische pool: wijzigen edtu's](#elastic-pool-change-edtus).
- Query's te verlagen van het Resourcegebruik van elke query te optimaliseren. Zie voor meer informatie [Query afstemmen/Hinting](sql-database-performance-guidance.md#query-tuning-and-hinting).

### <a name="storage"></a>Storage

Wanneer databaseruimte gebruikt de maximale limiet bereikt, database worden ingevoegd en updates die Verhoog de gegevensgrootte niet als clients ontvangen een [foutbericht](sql-database-develop-error-messages.md). Database selecteert en VERWIJDERINGEN blijven slagen.

Wanneer zich voordoen tijdens een hoge ruimtegebruik, wordt een risicobeperking opties zijn onder andere:

- De maximale grootte van de database of elastische pool, of wijzig het prestatieniveau verkrijgen van meer opgenomen opslag verhogen. Zie [limieten voor SQL-Database](sql-database-dtu-resource-limits.md).
- Als de database zich in een elastische groep, kan klikt u vervolgens ook de database worden verplaatst buiten de groep zodat de opslagruimte wordt niet gedeeld met andere databases.

### <a name="sessions-and-workers-requests"></a>Sessies en werknemers (aanvragen) 

Het maximum aantal sessies en werknemers worden bepaald door de prijscategorie en prestatieniveau serviceniveau (dtu's en edtu's). Nieuwe aanvragen worden geweigerd als sessie- of worker limieten zijn bereikt en clients een foutbericht weergegeven wordt. Terwijl het aantal verbindingen beschikbaar kan worden beheerd door de toepassing, is het aantal gelijktijdige werknemers vaak moeilijker te schatten en te beheren. Dit is vooral van toepassing tijdens de piekuren van load wanneer database limieten zijn bereikt en werknemers opstapelen vanwege langer actieve query's. 

Wanneer zich voordoen tijdens een hoog gebruik van sessie- of worker, wordt een risicobeperking opties zijn onder andere:
- Het serviceniveau voor laag of de prestaties van de database of elastische pool te vergroten. Zie [één database: opslaggrootte wijzigen](#single-database-change-storage-size), [één database: dtu's wijzigen](#single-database-change-dtus), [elastische pool: opslaggrootte wijzigen](#elastic-pool-change-storage-size), en [elastische pool: edtu's wijzigen ](#elastic-pool-change-edtus).
- Optimaliseren query's te verlagen van het Resourcegebruik van elke query als de oorzaak van het gebruik van verhoogde worker vanwege conflicten over berekenen bronnen. Zie voor meer informatie [Query afstemmen/Hinting](sql-database-performance-guidance.md#query-tuning-and-hinting).

## <a name="next-steps"></a>Volgende stappen

- Zie [SQL Database Veelgestelde vragen over](sql-database-faq.md) voor antwoorden op veelgestelde vragen.
- Zie voor meer informatie over de algemene Azure limieten [Azure-abonnement en Servicelimieten, quota's en beperkingen](../azure-subscription-service-limits.md).
- Zie voor meer informatie over dtu's en edtu's [dtu's en edtu's](sql-database-what-is-a-dtu.md).
- Zie voor meer informatie over de maximale grootte tempdb https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database.
