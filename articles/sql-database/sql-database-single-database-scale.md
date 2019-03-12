---
title: Schalen van één database-resources - Azure SQL Database | Microsoft Docs
description: In dit artikel wordt beschreven hoe u de reken- en opslagresources die beschikbaar zijn voor een individuele database schalen in Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: juliemsft
ms.author: jrasnick
ms.reviewer: carlrab
manager: craigg
ms.date: 03/06/2019
ms.openlocfilehash: d6de6e2752c16d95c81f47b9d14d53e0233a4ed6
ms.sourcegitcommit: dd1a9f38c69954f15ff5c166e456fda37ae1cdf2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57570796"
---
# <a name="scale-single-database-resources-in-azure-sql-database"></a>Schalen van één database-resources in Azure SQL Database

In dit artikel wordt beschreven hoe u de reken- en opslagresources die beschikbaar zijn voor een individuele database schalen in Azure SQL Database.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="change-compute-resources-vcores-or-dtus"></a>Wijziging rekenresources (vCores of dtu's)

Wanneer u hebt gekozen het aantal vCores of dtu's, u kunt een individuele database omhoog of omlaag schalen dynamisch op basis van het feitelijke gebruik met behulp van de [Azure-portal](sql-database-single-databases-manage.md#manage-an-existing-sql-database-server), [Transact-SQL](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current#examples-1), [ PowerShell](/powershell/module/az.sql/set-azsqldatabase), wordt de [Azure CLI](/cli/azure/sql/db#az-sql-db-update), of de [REST-API](https://docs.microsoft.com/rest/api/sql/databases/update).

De volgende video ziet u dynamisch wijzigen van de service-laag en het berekenen van de grootte verhogen beschikbaar dtu's voor één database.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-dynamically-scale-up-or-scale-down/player]
>

### <a name="impact-of-changing-service-tier-or-rescaling-compute-size"></a>Gevolgen van de service tier of schaling aanpassen compute grootte te wijzigen

Wijzigen van de service tier of compute-grootte van een individuele database voornamelijk betrekking heeft op de service de volgende stappen uit:

1. Nieuwe compute-instantie voor de database maken  

    Een nieuwe compute-instantie voor de database wordt gemaakt met de aangevraagde service-laag en de rekencapaciteit. Voor sommige combinaties van-servicelaag en verkleiningen compute, een replica van de database moet worden gemaakt in de nieuwe compute-instantie die omvat het kopiëren van gegevens en kan sterke invloed uitoefenen op de totale latentie. Ongeacht de database online blijven tijdens deze stap, en verbindingen nog steeds worden omgeleid naar de database in de oorspronkelijke compute-instantie.

2. Schakel over routering van verbindingen met de nieuwe compute-instantie

    Bestaande verbindingen met de database in de oorspronkelijke compute-instantie worden verwijderd. Geen nieuwe verbindingen worden tot stand gebracht met de database in de nieuwe compute-instantie. Voor sommige combinaties van-servicelaag en verkleiningen compute, databasebestanden losgekoppeld en opnieuw gekoppeld tijdens de switch.  Ongeacht kan de switch leiden tot een korte serviceonderbreking wanneer de database niet beschikbaar in het algemeen voor minder dan 30 seconden en vaak slechts enkele seconden is. Als er langlopende transacties uitgevoerd wanneer verbindingen worden verwijderd, de duur van deze stap kan het langer duren om te herstellen van afgebroken transacties. [Versneld databaseherstel](sql-database-accelerated-database-recovery.md) de impact kunt beperken langlopende transacties afgebroken.

> [!IMPORTANT]
> Gegevens niet verloren tijdens een stap in de werkstroom.

### <a name="latency-of-changing-service-tier-or-rescaling-compute-size"></a>Latentie van de service tier of schaling aanpassen compute grootte te wijzigen

De latentie te wijzigen van de servicelaag of de compute-grootte van een individuele database of elastische pool oorspronkelijke met parameters als volgt:

|Servicelaag|Één Basic-database,</br>Standard (S0-S1)|Basisbeperkingen voor elastische groepen,</br>Standard (S2-S12) </br>Zeer grootschalige, </br>Algemeen doel individuele database of elastische pool|Premium en bedrijfskritiek individuele database of elastische pool|
|:---|:---|:---|:---|
|**Één Basic-database,</br> Standard (S0-S1)**|&bull; &nbsp;Constante wachttijd onafhankelijk van de gebruikte ruimte</br>&bull; &nbsp;Meestal minder dan vijf minuten|&bull; &nbsp;Latentie in verhouding met databaseruimte gebruikt vanwege het kopiëren van gegevens</br>&bull; &nbsp;Meestal minder dan 1 minuut per GB aan ruimte die wordt gebruikt|&bull; &nbsp;Latentie in verhouding met databaseruimte gebruikt vanwege het kopiëren van gegevens</br>&bull; &nbsp;Meestal minder dan 1 minuut per GB aan ruimte die wordt gebruikt|
|**Basisbeperkingen voor elastische groepen, </br>Standard (S2-S12) </br>zeer grootschalige, </br>algemeen gebruik één database of elastische pool**|&bull; &nbsp;Latentie in verhouding met databaseruimte gebruikt vanwege het kopiëren van gegevens</br>&bull; &nbsp;Meestal minder dan 1 minuut per GB aan ruimte die wordt gebruikt|&bull; &nbsp;Constante wachttijd onafhankelijk van de gebruikte ruimte</br>&bull; &nbsp;Meestal minder dan vijf minuten|&bull; &nbsp;Latentie in verhouding met databaseruimte gebruikt vanwege het kopiëren van gegevens</br>&bull; &nbsp;Meestal minder dan 1 minuut per GB aan ruimte die wordt gebruikt|
|**Premium en bedrijfskritiek individuele database of elastische pool**|&bull; &nbsp;Latentie in verhouding met databaseruimte gebruikt vanwege het kopiëren van gegevens</br>&bull; &nbsp;Meestal minder dan 1 minuut per GB aan ruimte die wordt gebruikt|&bull; &nbsp;Latentie in verhouding met databaseruimte gebruikt vanwege het kopiëren van gegevens</br>&bull; &nbsp;Meestal minder dan 1 minuut per GB aan ruimte die wordt gebruikt|&bull; &nbsp;Latentie in verhouding met databaseruimte gebruikt vanwege het kopiëren van gegevens</br>&bull; &nbsp;Meestal minder dan 1 minuut per GB aan ruimte die wordt gebruikt|

> [!TIP]
> Als u wilt bewaken in voortgang van bewerkingen, Zie: [Beheren met behulp van de REST-API voor SQL operations](https://docs.microsoft.com/rest/api/sql/operations/list), [bewerkingen met behulp van CLI beheren](/cli/azure/sql/db/op), [bewaken van bewerkingen met behulp van T-SQL](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) en deze twee PowerShell-opdrachten: [Get-AzSqlDatabaseActivity](/powershell/module/az.sql/get-azsqldatabaseactivity) en [Stop-AzSqlDatabaseActivity](/powershell/module/az.sql/stop-azsqldatabaseactivity).

### <a name="additional-considerations-when-changing-service-tier-or-rescaling-compute-size"></a>Aanvullende overwegingen bij het wijzigen van grootte van laag of schaling aanpassen compute-service

- Als u een upgrade naar een hogere servicelaag uitvoert of grootte van compute, wordt de maximale grootte van de database niet verhogen, tenzij u expliciet een groter formaat (maxsize) opgeeft.
- Als u wilt downgraden van een database, moet de database die wordt gebruikt-ruimte kleiner zijn dan de maximaal toegestane grootte van de gewenste servicelaag en compute-grootte.
- Wanneer de Downgrade uitvoert vanaf **Premium** naar de **Standard** laag, een extra opslag in rekening gebracht als zowel (1) de maximale grootte van de database wordt ondersteund in de doelgrootte berekenen en (2) de maximumgrootte is groter dan de inbegrepen de hoeveelheid opslagruimte van de doel-compute-grootte. Bijvoorbeeld, als een P1-database met een maximale grootte van 500 GB is verkleind in S3, is een extra opslagkosten van toepassing omdat S3 biedt ondersteuning voor een maximale grootte van 500 GB en de overeengekomen opslaghoeveelheid slechts 250 GB is. De hoeveelheid extra opslagruimte is dus 500 GB – 250 GB = 250 GB. Zie voor informatie over prijzen van extra opslagruimte [prijzen van SQL Database](https://azure.microsoft.com/pricing/details/sql-database/). Als de werkelijke hoeveelheid ruimte die wordt gebruikt kleiner dan de hoeveelheid inbegrepen opslag is en vervolgens deze extra kosten kan worden vermeden door de maximale grootte van de database voor de hoeveelheid die inclusief te beperken.
- Bij het upgraden van een database met [geo-replicatie](sql-database-geo-replication-portal.md) is ingeschakeld, de secundaire databases upgraden naar de gewenste servicelaag en grootte compute vóór de upgrade van de primaire database (algemene richtlijnen voor de beste prestaties). Bij een upgrade naar een andere, is upgrade van de secundaire database eerst vereist.
- Wanneer een database met downgraden [geo-replicatie](sql-database-geo-replication-portal.md) is ingeschakeld, de primaire databases aan de gewenste servicelaag downgraden en grootte compute voordat het downgraden van de secundaire database (algemene richtlijnen voor de beste prestaties). Wanneer de Downgrade uitvoert naar een andere editie, is downgraden van de primaire database eerst vereist.
- De mogelijkheden om de service te herstellen verschillen voor de verschillende servicelagen. Als u een downgrade uitvoert op de **Basic** laag, wordt er een lagere bewaarperiode voor back-up. Zie [back-ups van Azure SQL Database](sql-database-automated-backups.md).
- De nieuwe eigenschappen voor de database worden pas toegepast nadat de wijzigingen zijn voltooid.

### <a name="billing-during-rescaling"></a>Facturering tijdens schaling aanpassen

U worden in rekening gebracht voor elk uur bestaat in een database met behulp van de hoogste servicelaag + compute-grootte die tijdens dat uur, ongeacht het gebruik of of de database minder dan een uur actief is toegepast. Als u een individuele database maken en deze vijf minuten later verwijdert weerspiegelt uw factuur een post voor één database-uur.

## <a name="change-storage-size"></a>Opslaggrootte wijzigen

### <a name="vcore-based-purchasing-model"></a>Op vCore gebaseerd aanschafmodel

- Opslag kan worden ingericht tot de maximale grootte is bereikt met behulp van de stappen van 1 GB. De minimale configureerbare gegevensopslag is 5 GB
- Opslag voor een individuele database kan worden bevoorraad door vergroten of verkleinen van de maximale grootte met behulp van de [Azure-portal](https://portal.azure.com), [Transact-SQL](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current#examples-1), [PowerShell](/powershell/module/az.sql/set-azsqldatabase), de [Azure CLI](/cli/azure/sql/db#az-sql-db-update), of de [REST-API](https://docs.microsoft.com/rest/api/sql/databases/update).
- SQL-Database wijst automatisch 30% van de extra opslag voor de logboekbestanden en 32GB per vCore voor TempDB, maar niet meer dan 384GB. TempDB bevindt zich op een gekoppelde SSD in alle service-lagen.
- De prijs van opslag voor een individuele database is de som van de gegevens opslag- en logboekbestanden opslag bedragen vermenigvuldigd met de prijs per eenheid opslag van de servicelaag. De kosten van TempDB is opgenomen in de vCore-prijs. Zie voor meer informatie over de prijs van extra opslagruimte [prijzen van SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).

> [!IMPORTANT]
> In sommige gevallen is het wellicht voor het verkleinen van een database voor het vrijmaken van ongebruikte ruimte. Zie voor meer informatie, [bestandsruimte in Azure SQL Database beheren](sql-database-file-space-management.md).

### <a name="dtu-based-purchasing-model"></a>DTU gebaseerde aankoopmodel

- De prijs voor DTU voor een individuele database bevat een bepaalde hoeveelheid opslagruimte zonder extra kosten. Extra opslagruimte bovenop de inbegrepen hoeveelheid worden ingezet er gelden aanvullende kosten tot de maximale grootte is bereikt in stappen van 250 GB tot 1 TB, en klik vervolgens in stappen van 256 GB dan 1 TB. Zie voor de hoeveelheid inbegrepen opslag en limieten voor de maximale berichtgrootte [individuele database: Opslaggrootte en compute-grootten](sql-database-dtu-resource-limits-single-databases.md#single-database-storage-sizes-and-compute-sizes).
- Extra opslag voor een individuele database kan worden ingericht door de maximale grootte van de Azure Portal gebruikt, [Transact-SQL](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current#examples-1), [PowerShell](/powershell/module/az.sql/set-azsqldatabase), wordt de [Azure CLI](/cli/azure/sql/db#az-sql-db-update), of de [ REST-API](https://docs.microsoft.com/rest/api/sql/databases/update).
- De prijs voor extra opslagruimte voor een individuele database is de hoeveelheid extra opslagruimte vermenigvuldigd met de prijs voor extra opslagruimte per eenheid van de servicelaag. Zie voor meer informatie over de prijs van extra opslagruimte [prijzen van SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).

> [!IMPORTANT]
> In sommige gevallen is het wellicht voor het verkleinen van een database voor het vrijmaken van ongebruikte ruimte. Zie voor meer informatie, [bestandsruimte in Azure SQL Database beheren](sql-database-file-space-management.md).

## <a name="dtu-based-purchasing-model-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb"></a>DTU gebaseerde aankoopmodel: Beperkingen van P11 en P15 wanneer de maximale grootte van meer dan 1 TB

Voor de Premium-laag is er meer dan 1 TB aan opslagruimte beschikbaar in alle regio's, met uitzondering van: China - oost, China - noord, Duitsland - centraal, Duitsland - noordoost, US - west-centraal, US - DoD-regio's en US Government - centraal. In deze regio’s is de maximale opslagruimte in de Premium-laag beperkt tot 1 TB. Raadpleeg [P11-P15 huidige beperkingen](sql-database-single-database-scale.md#dtu-based-purchasing-model-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb) voor meer informatie. De volgende overwegingen en beperkingen van toepassing op P11 en P15-databases met een maximale grootte die groter zijn dan 1 TB:

- Als u ervoor kiest een maximale grootte die groter zijn dan 1 TB bij het maken van een database (met behulp van een waarde van 4 TB of 4096 GB), wordt de opdracht create mislukt met een fout als de database is ingericht in een niet-ondersteunde regio.
- Voor bestaande P11 en P15-databases zich bevinden in een van de ondersteunde regio's, kunt u de maximale opslag voor de dan 1 TB in stappen van 256 GB verhogen tot 4 TB. Als u wilt zien als een groter formaat wordt ondersteund in uw regio, gebruikt u de [DATABASEPROPERTYEX](/sql/t-sql/functions/databasepropertyex-transact-sql) functie of de grootte van de database in Azure portal controleren. Upgraden van een bestaande P11 of P15 kan database alleen worden uitgevoerd door een principal-aanmelding op serverniveau of leden van de databaserol dbmanager.
- Als een upgrade bewerking wordt uitgevoerd in een ondersteunde regio wordt de configuratie direct bijgewerkt. De database blijft online tijdens het upgradeproces. U niet de volledige hoeveelheid opslag dan 1 TB opslag echter gebruiken totdat de werkelijke databasebestanden zijn bijgewerkt naar de nieuwe maximale grootte. De lengte van de tijd die nodig is afhankelijk van de grootte van de database een upgrade wordt uitgevoerd.
- Bij het maken of bijwerken van een P11 of P15-database, kunt u alleen kiezen tussen de maximale grootte van 1 TB en 4 TB in stappen van 256 GB. Bij het maken van een P11/P15, is de standaardoptie voor de opslag van 1 TB is vooraf geselecteerd. Voor databases die zich in een van de ondersteunde regio's, kunt u het maximum opslag tot een maximum van 4 TB voor een database met nieuwe of bestaande verhogen. Voor alle andere regio's, kan niet de maximale grootte worden verhoogd dan 1 TB. De prijs verandert niet wanneer u 4 TB inbegrepen opslag selecteren.
- Worden als de maximale grootte van een database is ingesteld op meer dan 1 TB, klikt u vervolgens deze kan niet gewijzigd tot 1 TB, zelfs als de werkelijke opslag gebruikt lager dan 1 TB is. Daarom kan downgraden van een P11 of P15 met een maximale grootte die groter zijn dan 1 TB aan een 1 TB P11 of P15 TB 1 of lager compute grootte, zoals P1-P6). Deze beperking geldt ook voor het terugzetten en de kopie-scenario's met inbegrip van punt-in-time, geo-restore, lange-termijn-back-up-retentie en database-exemplaar. Zodra een database is geconfigureerd met een maximale grootte die groter zijn dan 1 TB, moeten alle bewerkingen voor het herstellen van deze database worden uitgevoerd in een P11/P15 met een maximale grootte die groter zijn dan 1 TB.
- Voor scenario's met actieve geo-replicatie:
  - Instellen van een relatie geo-replicatie: Als de primaire database P11 of P15 is, moet de secondary(ies) ook worden P11 of P15; lagere compute grootte worden geweigerd als secundaire replica's, omdat ze zijn niet geschikt voor ondersteuning van meer dan 1 TB.
  - Upgrade van de primaire database in een relatie geo-replicatie: Wijzigen van de maximale grootte in meer dan 1 TB op een primaire database, wordt dezelfde wijziging op de secundaire database geactiveerd. Beide upgrades moeten zijn gelukt is om de wijziging op de primaire pas van kracht zijn. Er gelden beperkingen van de regio voor de optie meer dan 1 TB. Als de secundaire server in een regio die geen ondersteuning biedt voor meer dan 1 TB, wordt de primaire niet bijgewerkt.
- Met behulp van de Import/Export-service voor het laden van P11/P15-databases met meer dan 1 TB, wordt niet ondersteund. Gebruik SqlPackage.exe naar [importeren](sql-database-import.md) en [exporteren](sql-database-export.md) gegevens.

## <a name="next-steps"></a>Volgende stappen

Raadpleeg voor algemene resourcelimieten, [SQL Database vCore gebaseerde resourcelimieten - individuele databases](sql-database-vcore-resource-limits-single-databases.md) en [SQL Database DTU gebaseerde resourcelimieten - elastische pools](sql-database-dtu-resource-limits-single-databases.md).
