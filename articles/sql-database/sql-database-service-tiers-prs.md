---
title: Azure SQL Database Premium RS service tier buiten gebruik stellen | Microsoft Docs
description: 'De servicelaag Premium RS wordt buiten gebruik gesteld en wordt de ondersteuning voor deze wordt beëindigd: Zie Opties voor de migratie.'
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
ms.date: 12/13/2018
ms.openlocfilehash: e6ce99e70d57fd8d29b0f5cb2bd1084accfc4757
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2019
ms.locfileid: "54262962"
---
# <a name="azure-sql-database-premium-rs-service-tier-preview-is-being-retired---options-for-migration"></a>Azure SQL Database Premium RS-servicelaag (preview) wordt buiten gebruik gesteld - opties voor migratie

In februari 2018 is Microsoft aangekondigd dat de servicelaag Premium RS in Azure SQL Database kan niet worden vrijgegeven voor algemene beschikbaarheid en wordt niet meer ondersteund na 31 januari 2019. Deze einde van ondersteuning deadline uitgebreid naar 30 juni 2019. Dit artikel wordt uitgelegd dat de opties voor het migreren van de servicelaag Premium RS naar een andere servicelaag. Na 30 juni 2019, zal Microsoft automatisch uw Premium RS-databases migreren naar een algemeen beschikbare service-laag die het meest overeenkomt met de prestatievereisten van uw database Premium RS.

Hier volgen de migratie bestemmingen en prijsopties kiezen die geschikt zijn voor Premium RS-klanten kunnen zijn:

- vCore-servicelaag

  De **algemeen** en **bedrijfskritiek** Servicelagen in de [op vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md). Deze twee Servicelagen zijn algemeen beschikbaar. Het vCore-aanschafmodel biedt ook de **grootschalige** servicelaag (in openbare preview) die u kunt on-demand om uw werkbelasting met automatisch schalen tot 100 TB per database aanpassen. De servicelaag grootschalige biedt i/o-prestaties die vergelijkbaar is met de Premium-servicelaag in de [DTU gebaseerde aankoopmodel](sql-database-service-tiers-dtu.md) tegen een prijs dichter bij de servicelaag Premium RS.
- Prijsopties voor Dev/Test

  [Prijsopties voor ontwikkelen/testen](https://azure.microsoft.com/pricing/dev-test/) zorgt voor besparingen tot 55% ten opzichte van de tarieven inclusief licenties aan uw Visual Studio-abonnement.
- Azure Hybrid Benefit en prijzen voor gereserveerde capaciteit

  [Azure Hybrid Benefit en prijzen voor gereserveerde capaciteit](https://azure.microsoft.com/pricing/details/sql-database/) besparingen bieden tot 80% ten opzichte van de licentie-tarieven. Zie voor meer informatie over deze opties [Azure Hybrid Benefit voor SQL Server](https://azure.microsoft.com/pricing/hybrid-benefit/) en [Azure SQL Database gereserveerde capaciteit](sql-database-reserved-capacity.md).

## <a name="act-now-to-migrate-your-premium-rs-databases-to-alternative-sql-database-service-tiers"></a>Reageer nu uw Premium RS-databases migreren naar alternatieve SQL-Database-Servicelagen

Bekijk de richtlijnen in dit artikel, samen met onze prijzen en documentatie om te bepalen van de migratie van de juiste bestemmingen voor uw Premium RS-workloads.

## <a name="migrate-compute-intensive-workloads-and-save"></a>Rekenintensieve workloads migreren en opslaan

U wordt aangeraden migreren naar onze algemeen beschikbaar op vCore gebaseerde algemeen gebruik-servicelaag en opslaan meer versus met behulp van Azure Hybrid Benefit voor SQL Server en gereserveerde capaciteit aanbiedingen inclusief licenties-tarieven voor uw Premium RS rekenintensieve workloads. Als u in plaats daarvan op DTU gebaseerde inkoopoptie blijven, kunt u uw rekenintensieve Premium RS-databases migreren naar een Standard-servicelaag en nog steeds opslaan ten opzichte van de Premium RS prijzen voor algemene beschikbaarheid (als deze is overgegaan in algemene beschikbaarheid).

> [!WARNING]
> Migreren van uw workloads Premium RS naar Premium op basis van DTU-Servicelagen verhogen maandelijkse kosten versus de huidige prijzen voor Premium RS. We raden u aan de grootschalige en bedrijfskritiek lagen met Azure Hybrid Benefit en gereserveerde capaciteit voor het onderhouden van soortgelijke of lagere kosten dan Premium RS-prijzen.

### <a name="premium-rs-databases"></a>Premium RS-databases

|**Als u momenteel ingeschakeld bent...**|**Migreren naar vergelijkbare op vCore gebaseerde...**|**Migreren naar vergelijkbare op basis van DTU...**|
|---|---|---|
|Premium RS 1|Algemeen doel 1 vCore (Gen 4)|Standard 3|
|Premium RS 2|Algemeen doel 2 vCores (Gen 4)|Standard 4|
|Premium RS 4|Algemeen doel 4 vCores (Gen4)|Standaard 6|
|Premium RS 6|Algemeen doel 6 vCores (Gen4)|Standard 7|

### <a name="premium-rs-pools"></a>Premium RS-pools

|**Als u momenteel ingeschakeld bent...**|**Migreren naar vergelijkbare op vCore gebaseerde...**|**Migreren naar vergelijkbare op basis van DTU...**|
|---|---|---|
|Premium RS-pool 125 DTU|Algemeen doel 1 vCore (Gen 4)|Standard-pool 100 edtu 's|
|Premium RS-pool 250 DTU|Algemeen doel 2 vCores (Gen 4)|Standard-pool 250 edtu 's|
|Premium RS-pool 500 DTU|Algemeen doel 4 vCores (Gen4)|Standard-pool 500 edtu 's|
|Premium RS-pool 1000 DTU|Algemeen doel 8 vCores (Gen4)|Standard-pool 1000 edtu 's|

## <a name="optimize-savings-and-performance-for-your-io-intensive-workloads"></a>Besparingen en prestaties voor uw i/o-intensieve workloads optimaliseren

U wordt aangeraden de i/o-intensieve individuele databases migreren naar onze op vCore gebaseerde grootschalige-laag, momenteel in preview en uw i/o-intensieve databasepools naar onze algemeen beschikbaar bedrijfskritieke laag, voor de optimale combinatie van prestaties en kosten.  De volgende vCore gebaseerde opties worden onderhouden en verbeteren de prestaties van uw huidige kan bespaart u geld bij het in combinatie met Azure Hybrid Benefit en de gereserveerde capaciteit prijzen.

|**Als u momenteel ingeschakeld bent...**|**Migreren naar vergelijkbare op vCore gebaseerde...**|**Migreren naar vergelijkbare op basis van DTU...**|
|---|---|---|
|Premium RS 1|(Preview) Grootschalige 1 vCore (Gen 4) of -Business-kritische 1 vCore (Gen 4)|Premium 1|
|Premium RS 2|(Preview) Grootschalige 2 vCores (Gen 4) of -Business-kritische 2 vCores (Gen 4|Premium 2|
|Premium RS 4|(Preview) Zeer grootschalige 4 vCores (Gen 4) of Business-kritische 4 vCores (Gen4)|Premium 4
|Premium RS 6|(Preview) Zeer grootschalige 6 vCores (Gen 4) of kritieke zakelijke-6 vCores (Gen4)|Premium 6|

|**Als u momenteel ingeschakeld bent...**|**Migreren naar vergelijkbare op vCore gebaseerde...**|**Migreren naar vergelijkbare op basis van DTU...**|
|---|---|---|
|Premium RS-pool 125 DTU|Zakelijke kritieke 2 vCores (Gen4)|Premium-pool van 125 edtu 's|
|Premium RS-pool 250 DTU|Zakelijke kritieke 2 vCores (Gen4)|Premium-Edtu 250|
|Premium RS-pool 500 DTU|Zakelijke kritieke 4 vCores (Gen4)|Premium-pool 500 edtu 's|
|Premium RS-pool 1000 DTU|Zakelijke kritieke 8 vCores (Gen4)|Premium-pool 1000 edtu 's|

## <a name="take-advantage-of-our-new-offers"></a>Profiteer van onze nieuwe aanbiedingen

Onze service-lagen in het op vCore gebaseerde aankoopmodel komen in aanmerking voor speciale aanbiedingen die u tot 80% ten opzichte van prijs voor inbegrepen licentie kunnen opslaan. Gebruik van uw SQL Server Standard of Enterprise edition-licenties met actieve Software Assurance voor besparingen tot 55% ten opzichte van prijs voor inbegrepen licentie met de [Azure Hybrid Benefit voor SQL Server](https://azure.microsoft.com/pricing/hybrid-benefit/). U kunt het hybrid Benefit in combinatie met combineren [Azure SQL Database gereserveerde capaciteit](sql-database-reserved-capacity.md) prijzen en bespaar tot 80% wanneer u vooraf naar een of drie jaar term.  Beide voordelen vandaag nog vanuit Azure-portal te activeren.

Als u vragen hebt of problemen met betrekking tot deze wijzigen of als u nodig hebt met migratie-ondersteuning, neem dan contact op met [Microsoft](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).

## <a name="migration-from-a-premium-rs-service-tier-to-a-service-tier-in-either-the-dtu-or-the-vcore-model"></a>Migratie van een servicelaag Premium RS naar een servicelaag in de DTU of het vCore-model

### <a name="migration-of-a-database"></a>Migratie van een database

Een database migreren van een service Premium RS laag naar een servicelaag in een van de DTU of het vCore-model is vergelijkbaar met het upgraden of downgraden tussen service-lagen in de servicelaag Premium RS.

### <a name="using-database-copy-to-convert-a-premium-rs-database-to-a-dtu-based-or-vcore-based-database"></a>Met behulp van de database-exemplaar een database Premium RS te converteren naar een database op basis van DTU of op basis van vCore

U kunt een database met een grootte van de compute Premium RS kopiëren naar een database met een grootte op basis van DTU of op vCore gebaseerde compute zonder beperkingen of speciale combineren, zolang de doelgrootte berekenen biedt ondersteuning voor de maximale grootte van de brondatabase. Het database-exemplaar wordt gemaakt van een momentopname van gegevens vanaf de begintijd van de kopieerbewerking en voert geen synchronisatie van gegevens tussen de bron en het doel.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over specifieke-grootten en opties voor opslag beschikbaar voor individuele database Reken, [SQL Database vCore gebaseerde resourcelimieten voor individuele databases](sql-database-vcore-resource-limits-single-databases.md#general-purpose-service-tier-storage-sizes-and-compute-sizes)
- Zie voor meer informatie over specifieke-grootten en opties voor opslag beschikbaar voor elastische pools Reken, [SQL Database vCore gebaseerde resourcelimieten voor elastische pools](sql-database-vcore-resource-limits-elastic-pools.md#general-purpose-service-tier-storage-sizes-and-compute-sizes).
