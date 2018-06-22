---
title: Schalen van één databaseresources - Azure SQL Database | Microsoft Docs
description: In dit artikel wordt beschreven hoe de berekenings- en beschikbare resources voor één database schalen in Azure SQL Database.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: carlrab
ms.openlocfilehash: 525416506a22f386de574ca02b2e919ac47b8737
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309881"
---
# <a name="scale-single-database-resources-in-azure-sql-database"></a>Resources voor één database schalen in Azure SQL Database

In dit artikel wordt beschreven hoe de berekenings- en beschikbare resources voor één database schalen in Azure SQL Database. 

## <a name="vcore-based-purchasing-model-change-storage-size"></a>op basis van vCore aankoopmodel: opslaggrootte wijzigen

- Tot de maximale grootte bereikt met stappen van 1 GB worden opslagruimte ingericht. De minimale configureerbare gegevensopslag is 5GB 
- Opslag voor één database kan worden bevoorraad door vergroten of verkleinen van het gebruik van de maximale grootte van de [Azure-portal](https://portal.azure.com), [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), de [Azure CLI](/cli/azure/sql/db#az_sql_db_update), of de [REST-API](/rest/api/sql/databases/update).
- SQL-Database wijst automatisch 30% van extra opslagruimte voor de logboekbestanden en 32GB per vCore voor TempDB, maar niet meer dan 384GB. TempDB bevindt zich op een gekoppelde SSD in alle Servicelagen.
- De prijs van opslag voor één database is de som van de gegevens opslag- en logboekbestanden opslag bedragen vermenigvuldigd met de eenheidsprijs opslag van de servicetier. De kosten van TempDB is opgenomen in de prijs vCore. Zie voor meer informatie over de prijs van extra opslagruimte [prijzen SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="vcore-based-purchasing-model-change-compute-resources"></a>op basis van vCore aankoopmodel: rekenresources wijzigen

Nadat u in eerste instantie het aantal vCores, u kunt een individuele database omhoog of omlaag schalen dynamisch op basis van het werkelijke ervaring met behulp van de [Azure-portal](sql-database-single-database-scale.md#azure-portal-manage-logical-servers-and-databases), [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), wordt de [Azure CLI](/cli/azure/sql/db#az_sql_db_update), of de [REST-API](/rest/api/sql/databases/update). 

Als u de servicelaag en/of het prestatieniveau van een database wijzigt, wordt er een replica van de oorspronkelijke database gemaakt op het nieuwe prestatieniveau en worden vervolgens de verbindingen overgeschakeld op de replica. Er gaan geen gegevens verloren tijdens dit proces, maar tijdens het korte moment waarop we overschakelen naar de replica, worden verbindingen met de database uitgeschakeld, zodat sommige actieve transacties kunnen worden teruggedraaid. De tijdsduur voor de overgeschakeld verschilt, maar wordt doorgaans onder 4 seconden is minder dan 30 seconden 99% van de tijd. Als er grote aantallen transacties tijdens de vlucht op het moment dat verbindingen zijn uitgeschakeld, wordt de tijdsduur voor de overgeschakeld mogelijk ook langer. 

De duur van het volledige proces voor omhoog schalen is afhankelijk van de grootte en de servicelaag van de database vóór en na de wijziging. Bijvoorbeeld, moet een 250 GB-database die wordt gewijzigd naar, uit of binnen een servicelaag voor algemene doeleinden uitvoeren binnen zes uur. Voor een database dezelfde grootte hebben prestatieniveaus binnen de kritieke zakelijke servicelaag wordt gewijzigd, moet de scale-up uitvoeren binnen drie uur.

> [!TIP]
> Voor het bewaken van bewerkingen in-nu wordt uitgevoerd, Zie: [bewerkingen met de REST-API van SQL beheren](/rest/api/sql/Operations/List), [beheren met CLI](/cli/azure/sql/db/op), [bewaak de bewerkingen met T-SQL](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) en deze twee PowerShell-opdrachten: [Get-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/get-azurermsqldatabaseactivity) en [Stop AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/stop-azurermsqldatabaseactivity).

* Als u een naar een hogere laag of prestaties serviceniveau upgrade, wordt de maximale grootte van de database niet verhogen tenzij u expliciet een groter formaat (maxsize) opgeeft.
* Om het downgraden van een database, moet de ruimte van de database die wordt gebruikt kleiner zijn dan de maximaal toegestane grootte van het doel prijscategorie en prestatieniveau serviceniveau. 
* Bij een upgrade van een database met [geo-replicatie](sql-database-geo-replication-portal.md) ingeschakeld, de secundaire databases upgraden naar de gewenste prestatielaag vóór de upgrade van de primaire database (algemene richtlijnen voor de beste prestaties). Bij een upgrade naar een andere, is upgraden van de secundaire database eerst vereist.
* Wanneer een database met downgraden [geo-replicatie](sql-database-geo-replication-portal.md) ingeschakeld, de primaire databases naar de gewenste prestatielaag downgraden voordat het downgraden van de secundaire database (algemene richtlijnen voor de beste prestaties). Wanneer het downgraden naar een andere editie, is de primaire database downgraden eerst vereist.
* De nieuwe eigenschappen voor de database worden pas toegepast nadat de wijzigingen zijn voltooid.

## <a name="dtu-based-purchasing-model-change-storage-size"></a>Op basis van DTU aankoopmodel: opslaggrootte wijzigen

- De DTU prijs voor één database bevat een bepaalde hoeveelheid opslag zonder extra kosten. Extra opslagruimte boven het inbegrepen bedrag kan worden ingericht voor extra kosten tot de maximale grootte bereikt, in stappen van 250 GB tot 1 TB en vervolgens in stappen van 256 GB dan 1 TB. Zie voor opgenomen opslag bedragen en limieten voor de maximale berichtgrootte [één database: opslaggrootte en prestatieniveaus](#single-database-storage-sizes-and-performance-levels).
- Extra opslag voor één database kan worden ingericht met de toename van het gebruik van de maximale grootte van de [Azure-portal](sql-database-servers-databases-manage.md#azure-portal-manage-logical-servers-and-databases), [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), wordt de [Azure CLI](/cli/azure/sql/db#az_sql_db_update), of de [REST-API](/rest/api/sql/databases/update).
- De prijs van extra opslag voor één database is de extra opslagruimte vermenigvuldigd met de eenheidsprijs extra opslagruimte van de servicetier. Zie voor meer informatie over de prijs van extra opslagruimte [prijzen SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="dtu-based-purchasing-model-change-compute-resources-dtus"></a>Op basis van DTU aankoopmodel: wijziging van rekenresources (dtu's)

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

## <a name="dtu-based-purchasing-model-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb"></a>Op basis van DTU aankoopmodel: beperkingen van P11 en P15 wanneer de maximale grootte groter is dan 1 TB

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

Raadpleeg voor algemene limieten, [SQL-Database op basis van vCore limieten - individuele databases](sql-database-vcore-resource-limits-single-databases.md) en [resource op basis van SQL Database DTU - limieten voor elastische pools](sql-database-dtu-resource-limits-single-databases.md).
