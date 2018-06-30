---
title: Azure SQL Database DTU op basis van een resource beperkt individuele databases | Microsoft Docs
description: Deze pagina beschrijft enkele algemene resource op basis van het DTU-limieten voor individuele databases in Azure SQL Database.
services: sql-database
author: sachinpMSFT
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 06/29/2018
ms.author: carlrab
ms.openlocfilehash: 2d6660e1064959f2d04424ae1c3e9bc668231c92
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37131318"
---
# <a name="resource-limits-for-single-databases-using-the-dtu-based-purchasing-model"></a>Limieten voor individuele databases met behulp van het DTU-aankoopmodel 

Dit artikel bevat de gedetailleerde limieten voor de elastische pools Azure SQL Database met behulp van het DTU-aankoopmodel.

Zie voor DTU gebaseerde aankopen model limieten voor elastische pools, [resource op basis van het DTU - limieten voor elastische pools](sql-database-vcore-resource-limits-elastic-pools.md). Zie voor vCore gebaseerde limieten, [vCore gebaseerde limieten - individuele databases](sql-database-vcore-resource-limits-single-databases.md) en [vCore gebaseerde limieten - elastische pools](sql-database-vcore-resource-limits-elastic-pools.md).

## <a name="single-database-storage-sizes-and-performance-levels"></a>Individuele database: opslaggrootte en prestatieniveaus

De volgende tabellen tonen voor individuele databases, de beschikbare resources voor één database op elke prijscategorie en prestatieniveau serviceniveau. U kunt de servicelaag, het prestatieniveau en de opslagruimte voor het gebruik van een individuele database instellen de [Azure-portal](sql-database-servers-databases-manage.md#azure-portal-manage-logical-servers-and-databases), [PowerShell](sql-database-servers-databases-manage.md#powershell-manage-logical-servers-and-databases), wordt de [Azure CLI](sql-database-servers-databases-manage.md#azure-cli-manage-logical-servers-and-databases), of de [REST-API](sql-database-servers-databases-manage.md#rest-api-manage-logical-servers-and-databases).

### <a name="basic-service-tier"></a>Basisservicelaag
| **Prestatieniveau** | **Basic** |
| :--- | --: |
| Maximaal aantal DTU's | 5 |
| Opgenomen opslag (GB) | 2 |
| Maximum aantal opslagopties (GB) | 2 |
| Maximale OLTP-opslag in het geheugen (GB) |N/A |
| Maximum aantal gelijktijdige werknemers (aanvragen) | 30 |
| Maximaal aantal gelijktijdige sessies | 300 |
|||

### <a name="standard-service-tier"></a>Standaardservicelaag
| **Prestatieniveau** | **S0** | **S1** | **S2** | **S3** |
| :--- |---:| ---:|---:|---:|---:|
| Maximaal aantal DTU's | 10 | 20 | 50 | 100 |
| Opgenomen opslag (GB) | 250 | 250 | 250 | 250 |
| Maximum aantal opslagopties (GB) | 250 | 250 | 250 | 250, 500, 750, 1024 |
| Maximale OLTP-opslag in het geheugen (GB) | N/A | N/A | N/A | N/A |
| Maximum aantal gelijktijdige werknemers (aanvragen)| 60 | 90 | 120 | 200 |
| Maximaal aantal gelijktijdige sessies |600 | 900 | 1200 | 2400 |
||||||

### <a name="standard-service-tier-continued"></a>Standaard-servicelaag (vervolg)
| **Prestatieniveau** | **S4** | **S6** | **S7** | **S9** | **S12** |
| :--- |---:| ---:|---:|---:|---:|---:|
| Maximaal aantal DTU's | 200 | 400 | 800 | 1600 | 3000 |
| Opgenomen opslag (GB) | 250 | 250 | 250 | 250 | 250 |
| Maximum aantal opslagopties (GB) | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 |
| Maximale OLTP-opslag in het geheugen (GB) | N/A | N/A | N/A | N/A |N/A |
| Maximum aantal gelijktijdige werknemers (aanvragen)| 400 | 800 | 1600 | 3200 |6000 |
| Maximaal aantal gelijktijdige sessies |4800 | 9600 | 19200 | 30.000 |30.000 |
|||||||

### <a name="premium-service-tier"></a>Premium servicelaag 
| **Prestatieniveau** | **P1** | **P2** | **P4** | **P6** | **P11** | **P15** | 
| :--- |---:|---:|---:|---:|---:|---:|
| Maximaal aantal DTU's | 125 | 250 | 500 | 1000 | 1750 | 4000 |
| Opgenomen opslag (GB) | 500 | 500 | 500 | 500 | 4096 | 4096 |
| Maximum aantal opslagopties (GB) | 500, 750, 1024 | 500, 750, 1024 | 500, 750, 1024 | 500, 750, 1024 | 4096 | 4096 |
| Maximale OLTP-opslag in het geheugen (GB) | 1 | 2 | 4 | 8 | 14 | 32 |
| Maximum aantal gelijktijdige werknemers (aanvragen)| 200 | 400 | 800 | 1600 | 2400 | 6400 |
| Maximaal aantal gelijktijdige sessies | 30.000 | 30.000 | 30.000 | 30.000 | 30.000 | 30.000 |
|||||||


> [!IMPORTANT]
> Meer dan 1 TB aan opslag in de laag Premium is momenteel beschikbaar in alle regio's, behalve het volgende: West-Centraal VS, China Oost, USDoDCentral, Duitsland centraal, USDoDEast, VS Gov linksonder, Duitsland noordoosten, USGovIowa, China Noord. In andere regio’s is de maximale opslagruimte in de Premium-laag beperkt tot 1 TB. Zie [P11-P15: huidige beperkingen](#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  

## <a name="single-database-change-storage-size"></a>Individuele database: opslaggrootte wijzigen

- De DTU prijs voor één database bevat een bepaalde hoeveelheid opslag zonder extra kosten. Extra opslagruimte boven het inbegrepen bedrag kan worden ingericht voor extra kosten tot de maximale grootte bereikt, in stappen van 250 GB tot 1 TB en vervolgens in stappen van 256 GB dan 1 TB. Zie voor opgenomen opslag bedragen en limieten voor de maximale berichtgrootte [één database: opslaggrootte en prestatieniveaus](#single-database-storage-sizes-and-performance-levels).
- Extra opslag voor één database kan worden ingericht met de toename van het gebruik van de maximale grootte van de [Azure-portal](sql-database-servers-databases-manage.md#azure-portal-manage-logical-servers-and-databases), [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), wordt de [Azure CLI](/cli/azure/sql/db#az_sql_db_update), of de [REST-API](/rest/api/sql/databases/update).
- De prijs van extra opslag voor één database is de extra opslagruimte vermenigvuldigd met de eenheidsprijs extra opslagruimte van de servicetier. Zie voor meer informatie over de prijs van extra opslagruimte [prijzen SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="single-database-change-dtus"></a>Individuele database: dtu's wijzigen

Nadat u in eerste instantie een servicelaag, prestatieniveau en opslaghoeveelheid, u kunt een individuele database omhoog of omlaag schalen dynamisch op basis van het werkelijke ervaring met behulp van de [Azure-portal](sql-database-servers-databases-manage.md#azure-portal-manage-logical-servers-and-databases), [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), wordt de [Azure CLI](/cli/azure/sql/db#az_sql_db_update), of de [REST-API](/rest/api/sql/databases/update). 

De volgende video toont dynamisch wijzigen van de prestatielaag voor een verhoging van de beschikbare dtu's voor één database.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-dynamically-scale-up-or-scale-down/player]
>

Als u de servicelaag en/of het prestatieniveau van een database wijzigt, wordt er een replica van de oorspronkelijke database gemaakt op het nieuwe prestatieniveau en worden vervolgens de verbindingen overgeschakeld op de replica. Er gaan geen gegevens verloren tijdens dit proces, maar tijdens het korte moment waarop we overschakelen naar de replica, worden verbindingen met de database uitgeschakeld, zodat sommige actieve transacties kunnen worden teruggedraaid. De tijdsduur voor de overgeschakeld verschilt, maar is minder dan 30 seconden 99% van de tijd. Als er grote aantallen transacties tijdens de vlucht op het moment dat verbindingen zijn uitgeschakeld, wordt de tijdsduur voor de overgeschakeld mogelijk ook langer. 

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

## <a name="next-steps"></a>Volgende stappen

- Zie [SQL Database Veelgestelde vragen over](sql-database-faq.md) voor antwoorden op veelgestelde vragen.
- Zie voor meer informatie over de algemene Azure limieten [Azure-abonnement en Servicelimieten, quota's en beperkingen](../azure-subscription-service-limits.md).
- Zie voor meer informatie over dtu's en edtu's [dtu's en edtu's](sql-database-service-tiers.md#what-are-database-transaction-units-dtus).
- Zie voor meer informatie over de maximale grootte tempdb https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database.
