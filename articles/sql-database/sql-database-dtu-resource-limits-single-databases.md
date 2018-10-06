---
title: Azure SQL Database DTU gebaseerde resource beperkt individuele databases | Microsoft Docs
description: Deze pagina beschrijft enkele algemene DTU gebaseerde resourcelimieten voor individuele databases in Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: sachinpMSFT
ms.author: sachinp
ms.reviewer: carlrab
manager: craigg
ms.date: 10/04/2018
ms.openlocfilehash: 3f9d595b0a8dff8fce52769aaa12043632107ff5
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/06/2018
ms.locfileid: "48830405"
---
# <a name="resource-limits-for-single-databases-using-the-dtu-based-purchasing-model"></a>Resourcelimieten voor individuele databases met behulp van het op DTU gebaseerde aankoopmodel 

In dit artikel bevat de gedetailleerde resourcelimieten voor individuele databases van Azure SQL Database met behulp van het op DTU gebaseerde aankoopmodel.

Zie voor DTU gebaseerde aankopen model resourcelimieten voor elastische pools, [DTU gebaseerde resourcelimieten - elastische pools](sql-database-vcore-resource-limits-elastic-pools.md). Zie voor vCore gebaseerde resourcelimieten [vCore gebaseerde resourcelimieten - individuele databases](sql-database-vcore-resource-limits-single-databases.md) en [vCore gebaseerde resourcelimieten - elastische pools](sql-database-vcore-resource-limits-elastic-pools.md). Zie voor meer informatie over de verschillende modellen aankopen [aanschaffen van modellen en service-lagen](sql-database-service-tiers.md). 

> [!IMPORTANT]
> In sommige gevallen is het wellicht voor het verkleinen van een database voor het vrijmaken van ongebruikte ruimte. Zie voor meer informatie, [bestandsruimte in Azure SQL Database beheren](sql-database-file-space-management.md).

## <a name="single-database-storage-sizes-and-compute-sizes"></a>Individuele database: opslaggrootte en compute-grootten

Voor individuele databases in de volgende tabellen tonen de beschikbare resources voor een individuele database op elke servicelaag en compute grootte. U kunt de servicelaag, rekencapaciteit en hoeveelheid opslagruimte voor het gebruik van een individuele database instellen de [Azure-portal](sql-database-single-databases-manage.md#azure-portal-manage-logical-servers-and-databases), [PowerShell](sql-database-single-databases-manage.md#powershell-manage-logical-servers-and-databases), wordt de [Azure CLI](sql-database-single-databases-manage.md#azure-cli-manage-logical-servers-and-databases), of de [ REST-API](sql-database-single-databases-manage.md#rest-api-manage-logical-servers-and-databases).

### <a name="basic-service-tier"></a>Basisservicelaag
| **COMPUTE-grootte** | **Basic** |
| :--- | --: |
| Maximaal aantal DTU's | 5 |
| Inbegrepen opslag (GB) | 2 |
| Maximale opslagopties (GB) | 2 |
| Maximale OLTP-opslag in het geheugen (GB) |N.v.t. |
| Maximaal aantal gelijktijdige werknemers (aanvragen) | 30 |
| Maximaal aantal gelijktijdige sessies | 300 |
|||

### <a name="standard-service-tier"></a>Standaardservicelaag
| **COMPUTE-grootte** | **S0** | **S1** | **S2** | **S3** |
| :--- |---:| ---:|---:|---:|
| Maximaal aantal DTU's | 10 | 20 | 50 | 100 |
| Inbegrepen opslag (GB) | 250 | 250 | 250 | 250 |
| Maximale opslagopties (GB) | 250 | 250 | 250 | 250, 500, 750, 1024 |
| Maximale OLTP-opslag in het geheugen (GB) | N.v.t. | N.v.t. | N.v.t. | N.v.t. |
| Maximaal aantal gelijktijdige werknemers (aanvragen)| 60 | 90 | 120 | 200 |
| Maximaal aantal gelijktijdige sessies |600 | 900 | 1200 | 2400 |
||||||

### <a name="standard-service-tier-continued"></a>Standard-servicelaag (vervolg)
| **COMPUTE-grootte** | **S4** | **S6** | **S7** | **S9** | **S12** |
| :--- |---:| ---:|---:|---:|---:|
| Maximaal aantal DTU's | 200 | 400 | 800 | 1600 | 3000 |
| Inbegrepen opslag (GB) | 250 | 250 | 250 | 250 | 250 |
| Maximale opslagopties (GB) | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 |
| Maximale OLTP-opslag in het geheugen (GB) | N.v.t. | N.v.t. | N.v.t. | N.v.t. |N.v.t. |
| Maximaal aantal gelijktijdige werknemers (aanvragen)| 400 | 800 | 1600 | 3200 |6000 |
| Maximaal aantal gelijktijdige sessies |4800 | 9600 | 19200 | 30000 |30000 |
|||||||

### <a name="premium-service-tier"></a>Premium servicelaag 
| **COMPUTE-grootte** | **P1** | **P2** | **P4** | **P6** | **P11** | **P15** | 
| :--- |---:|---:|---:|---:|---:|---:|
| Maximaal aantal DTU's | 125 | 250 | 500 | 1000 | 1750 | 4000 |
| Inbegrepen opslag (GB) | 500 | 500 | 500 | 500 | 4096 | 4096 |
| Maximale opslagopties (GB) | 500, 750, 1024 | 500, 750, 1024 | 500, 750, 1024 | 500, 750, 1024 | 4096 | 4096 |
| Maximale OLTP-opslag in het geheugen (GB) | 1 | 2 | 4 | 8 | 14 | 32 |
| Maximaal aantal gelijktijdige werknemers (aanvragen)| 200 | 400 | 800 | 1600 | 2400 | 6400 |
| Maximaal aantal gelijktijdige sessies | 30000 | 30000 | 30000 | 30000 | 30000 | 30000 |
|||||||


> [!IMPORTANT]
> Meer dan 1 TB aan opslag in de Premium-laag is momenteel beschikbaar in alle regio's behalve het volgende: West-Centraal VS, China-Oost, USDoDCentral, Duitsland-centraal, USDoDEast, VS (overheid)-zuidwesten, Duitsland-Noordoost, USGovIowa, China-Noord. In andere regio’s is de maximale opslagruimte in de Premium-laag beperkt tot 1 TB. Zie [P11-P15: huidige beperkingen](#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  

## <a name="single-database-change-storage-size"></a>Individuele database: opslaggrootte wijzigen

- De prijs voor DTU voor een individuele database bevat een bepaalde hoeveelheid opslagruimte zonder extra kosten. Extra opslagruimte bovenop de inbegrepen hoeveelheid worden ingezet er gelden aanvullende kosten tot de maximale grootte is bereikt in stappen van 250 GB tot 1 TB, en klik vervolgens in stappen van 256 GB dan 1 TB. Zie voor de hoeveelheid inbegrepen opslag en limieten voor de maximale berichtgrootte [individuele database: opslaggrootte en compute-grootten](#single-database-storage-sizes-and-compute-sizes).
- Extra opslag voor een individuele database kan worden ingericht met de toename van het gebruik van de maximale grootte van de [Azure-portal](sql-database-single-databases-manage.md#azure-portal-manage-logical-servers-and-databases), [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), wordt de [Azure CLI](/cli/azure/sql/db#az-sql-db-update), of de [REST-API](/rest/api/sql/databases/update).
- De prijs voor extra opslagruimte voor een individuele database is de hoeveelheid extra opslagruimte vermenigvuldigd met de prijs voor extra opslagruimte per eenheid van de servicelaag. Zie voor meer informatie over de prijs van extra opslagruimte [prijzen van SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="single-database-change-dtus"></a>Individuele database: dtu's wijzigen

Wanneer u hebt gekozen een servicelaag, rekencapaciteit en hoeveelheid opslagruimte, u kunt een individuele database omhoog of omlaag schalen dynamisch op basis van het feitelijke gebruik met behulp van de [Azure-portal](sql-database-single-databases-manage.md#azure-portal-manage-logical-servers-and-databases), [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), [ PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), wordt de [Azure CLI](/cli/azure/sql/db#az-sql-db-update), of de [REST-API](/rest/api/sql/databases/update). 

De volgende video ziet u dynamisch wijzigen van de service-laag en het berekenen van de grootte verhogen beschikbaar dtu's voor één database.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-dynamically-scale-up-or-scale-down/player]
>

Wijzigen van de service tier en/of grootte van een database maakt u een replica van de oorspronkelijke database op de nieuwe compute-grootte en vervolgens de verbindingen overgeschakeld naar de replica. Er gaan geen gegevens verloren tijdens dit proces, maar tijdens het korte moment waarop we overschakelen naar de replica, worden verbindingen met de database uitgeschakeld, zodat sommige actieve transacties kunnen worden teruggedraaid. De tijdsduur voor de switch-meer dan verschilt, maar is minder dan 30 seconden 99% van de tijd. Als er veel transacties actief zijn op het moment dat de verbindingen zijn uitgeschakeld, wordt de tijdsduur voor de switch-meer dan mogelijk ook langer. 

De duur van het volledige proces voor omhoog schalen is afhankelijk van de grootte en de servicelaag van de database vóór en na de wijziging. Een 250 GB-database die wordt gewijzigd naar, vanuit of binnen een serviceniveau Standard, zou bijvoorbeeld binnen zes uur voltooid. Voor een database de dezelfde grootte die van compute-grootten in de Premium-servicelaag, zou de scale-up binnen drie uur voltooid.

> [!TIP]
> Zie voor het bewaken van bewerkingen in het wachtrijbericht: [beheren met behulp van de REST-API voor SQL operations](/rest/api/sql/Operations/List), [bewerkingen met behulp van CLI beheren](/cli/azure/sql/db/op), [bewaken van bewerkingen met behulp van T-SQL](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) en deze twee PowerShell-opdrachten: [Get-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/get-azurermsqldatabaseactivity) en [Stop AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/stop-azurermsqldatabaseactivity).

* Als u een upgrade naar een hogere servicelaag uitvoert of grootte van compute, wordt de maximale grootte van de database niet verhogen, tenzij u expliciet een groter formaat (maxsize) opgeeft.
* Als u wilt downgraden van een database, moet de database die wordt gebruikt-ruimte kleiner zijn dan de maximaal toegestane grootte van de gewenste servicelaag en compute-grootte. 
* Wanneer de Downgrade uitvoert vanaf **Premium** naar de **Standard** laag, een extra opslag in rekening gebracht als zowel (1) de maximale grootte van de database wordt ondersteund in de doelgrootte berekenen en (2) de maximumgrootte is groter dan de inbegrepen de hoeveelheid opslagruimte van de doel-compute-grootte. Bijvoorbeeld, als een P1-database met een maximale grootte van 500 GB is verkleind in S3, is een extra opslagkosten van toepassing omdat S3 biedt ondersteuning voor een maximale grootte van 500 GB en de overeengekomen opslaghoeveelheid slechts 250 GB is. De hoeveelheid extra opslagruimte is dus 500 GB – 250 GB = 250 GB. Zie voor informatie over prijzen van extra opslagruimte [prijzen van SQL Database](https://azure.microsoft.com/pricing/details/sql-database/). Als de werkelijke hoeveelheid ruimte die wordt gebruikt kleiner dan de hoeveelheid inbegrepen opslag is en vervolgens deze extra kosten kan worden vermeden door de maximale grootte van de database voor de hoeveelheid die inclusief te beperken. 
* Bij het upgraden van een database met [geo-replicatie](sql-database-geo-replication-portal.md) is ingeschakeld, de secundaire databases upgraden naar de gewenste servicelaag en grootte compute vóór de upgrade van de primaire database (algemene richtlijnen voor de beste prestaties). Bij een upgrade naar een andere, is upgrade van de secundaire database eerst vereist.
* Wanneer een database met downgraden [geo-replicatie](sql-database-geo-replication-portal.md) is ingeschakeld, de primaire databases aan de gewenste servicelaag downgraden en grootte compute voordat het downgraden van de secundaire database (algemene richtlijnen voor de beste prestaties). Wanneer de Downgrade uitvoert naar een andere editie, is downgraden van de primaire database eerst vereist.
* De mogelijkheden om de service te herstellen verschillen voor de verschillende servicelagen. Als u een downgrade uitvoert op de **Basic** laag, wordt er een lagere bewaarperiode voor back-up - Zie [back-ups van Azure SQL Database](sql-database-automated-backups.md).
* De nieuwe eigenschappen voor de database worden pas toegepast nadat de wijzigingen zijn voltooid.

## <a name="single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb"></a>Individuele database: beperkingen van P11 en P15 wanneer de maximale grootte van meer dan 1 TB

Een maximale grootte die groter zijn dan 1 TB voor P11 en P15-database wordt ondersteund in de volgende regio's: Australië-Oost, Australië-Zuidoost, Brazilië-Zuid, Canada-centraal, Canada-Oost, VS-midden, Frankrijk-centraal, Duitsland-centraal, Japan-Oost, Japan-West, Korea-centraal, Noord-centraal VS, Noord-Europa, Zuid-centraal VS, Zuidoost-Azië, UK-Zuid, UK-West, VS-oost2, VS-West, VS (overheid) Virginia, en West-Europa. De volgende overwegingen en beperkingen van toepassing op P11 en P15-databases met een maximale grootte die groter zijn dan 1 TB:

- Als u ervoor kiest een maximale grootte die groter zijn dan 1 TB bij het maken van een database (met behulp van een waarde van 4 TB of 4096 GB), wordt de opdracht create mislukt met een fout als de database is ingericht in een niet-ondersteunde regio.
- Voor bestaande P11 en P15-databases zich bevinden in een van de ondersteunde regio's, kunt u de maximale opslag voor de dan 1 TB in stappen van 256 GB verhogen tot 4 TB. Als u wilt zien als een groter formaat wordt ondersteund in uw regio, gebruikt u de [DATABASEPROPERTYEX](/sql/t-sql/functions/databasepropertyex-transact-sql) functie of de grootte van de database in Azure portal controleren. Upgraden van een bestaande P11 of P15 kan database alleen worden uitgevoerd door een principal-aanmelding op serverniveau of leden van de databaserol dbmanager. 
- Als een upgrade bewerking wordt uitgevoerd in een ondersteunde regio wordt de configuratie direct bijgewerkt. De database blijft online tijdens het upgradeproces. U niet de volledige hoeveelheid opslag dan 1 TB opslag echter gebruiken totdat de werkelijke databasebestanden zijn bijgewerkt naar de nieuwe maximale grootte. De lengte van de tijd die nodig is afhankelijk van de grootte van de database een upgrade wordt uitgevoerd. 
- Bij het maken of bijwerken van een P11 of P15-database, kunt u alleen kiezen tussen de maximale grootte van 1 TB en 4 TB in stappen van 256 GB. Bij het maken van een P11/P15, is de standaardoptie voor de opslag van 1 TB is vooraf geselecteerd. Voor databases die zich in een van de ondersteunde regio's, kunt u het maximum opslag tot een maximum van 4 TB voor een database met nieuwe of bestaande verhogen. Voor alle andere regio's, kan niet de maximale grootte worden verhoogd dan 1 TB. De prijs verandert niet wanneer u 4 TB inbegrepen opslag selecteren.
- Worden als de maximale grootte van een database is ingesteld op meer dan 1 TB, klikt u vervolgens deze kan niet gewijzigd tot 1 TB, zelfs als de werkelijke opslag gebruikt lager dan 1 TB is. Daarom kan downgraden van een P11 of P15 met een maximale grootte die groter zijn dan 1 TB aan een 1 TB P11 of P15 TB 1 of lager compute grootte, zoals P1-P6). Deze beperking geldt ook voor het terugzetten en de kopie-scenario's met inbegrip van punt-in-time, geo-restore, lange-termijn-back-up-retentie en database-exemplaar. Zodra een database is geconfigureerd met een maximale grootte die groter zijn dan 1 TB, moeten alle bewerkingen voor het herstellen van deze database worden uitgevoerd in een P11/P15 met een maximale grootte die groter zijn dan 1 TB.
- Voor scenario's met actieve geo-replicatie:
   - Instellen van een relatie geo-replicatie: als de primaire database P11 of P15 is, de secondary(ies) moet ook worden P11 of P15; lagere compute-grootten worden geweigerd als secundaire replica's, omdat ze zijn niet geschikt voor ondersteuning van meer dan 1 TB.
   - Upgrade van de primaire database in een relatie geo-replicatie: de maximale grootte wijzigen naar meer dan 1 TB op een primaire database wordt dezelfde wijziging op de secundaire database geactiveerd. Beide upgrades moeten zijn gelukt is om de wijziging op de primaire pas van kracht zijn. Er gelden beperkingen van de regio voor de optie meer dan 1 TB. Als de secundaire server in een regio die geen ondersteuning biedt voor meer dan 1 TB, wordt de primaire niet bijgewerkt.
- Met behulp van de Import/Export-service voor het laden van P11/P15-databases met meer dan 1 TB, wordt niet ondersteund. Gebruik SqlPackage.exe naar [importeren](sql-database-import.md) en [exporteren](sql-database-export.md) gegevens.

## <a name="next-steps"></a>Volgende stappen

- Zie [Veelgestelde vragen over SQL-Database](sql-database-faq.md) voor antwoorden op veelgestelde vragen.
- Zie [overzicht van resource beperkt op een logische server](sql-database-resource-limits-logical-server.md) voor informatie over de beperkingen op het niveau van de server en -abonnement.
- Zie voor meer informatie over algemene Azure-limieten [Azure-abonnement en Servicelimieten, quotums en beperkingen](../azure-subscription-service-limits.md).
- Zie voor meer informatie over dtu's en edtu's [dtu's en edtu's](sql-database-service-tiers.md#dtu-based-purchasing-model).
- Zie voor meer informatie over de maximale grootte tempdb https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database.
