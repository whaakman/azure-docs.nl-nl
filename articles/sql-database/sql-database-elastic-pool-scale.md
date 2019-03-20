---
title: Schalen van resources voor elastische pool - Azure SQL Database | Microsoft Docs
description: Deze pagina worden beschreven vergroten/verkleinen resources voor elastische pools in Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: oslake
ms.author: moslake
ms.reviewer: carlrab
manager: craigg
ms.date: 3/14/2019
ms.openlocfilehash: d8aaf51c836a8e88c4e9b92798067167cd044e72
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58015362"
---
# <a name="scale-elastic-pool-resources-in-azure-sql-database"></a>Resources voor elastische pool schalen in Azure SQL Database

In dit artikel wordt beschreven hoe u de reken- en opslagresources die beschikbaar zijn voor elastische pools en gepoolde databases schalen in Azure SQL Database.

## <a name="change-compute-resources-vcores-or-dtus"></a>Wijziging rekenresources (vCores of dtu's)

Wanneer u hebt gekozen het aantal vCores of edtu's, u kunt een elastische pool omhoog of omlaag schalen dynamisch op basis van het feitelijke gebruik met behulp van de [Azure-portal](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](/powershell/module/az.sql/Get-AzSqlElasticPool), wordt de [Azure CLI ](/cli/azure/sql/elastic-pool#az-sql-elastic-pool-update), of de [REST-API](https://docs.microsoft.com/rest/api/sql/elasticpools/update).

### <a name="impact-of-changing-service-tier-or-rescaling-compute-size"></a>Gevolgen van de service tier of schaling aanpassen compute grootte te wijzigen

Wijzigen van de service tier of compute-grootte van een elastische pool heeft een vergelijkbaar patroon voor individuele databases en voornamelijk betrekking heeft op de service de volgende stappen uit:

1. Nieuwe compute-instantie voor de elastische pool maken  

    Een nieuwe compute-instantie voor de elastische groep is gemaakt met de aangevraagde service-laag en de grootte van de compute. Voor sommige combinaties van-servicelaag en verkleiningen compute, een replica van elke database moet worden gemaakt in de nieuwe compute-instantie die omvat het kopiëren van gegevens en kan sterke invloed uitoefenen op de totale latentie. Ongeacht de databases tijdens deze stap online blijven, en verbindingen nog steeds worden omgeleid naar de databases in de oorspronkelijke compute-instantie.

2. Schakel over routering van verbindingen met de nieuwe compute-instantie

    Bestaande verbindingen met de databases in de oorspronkelijke compute-instantie worden verwijderd. Geen nieuwe verbindingen worden tot stand gebracht met de databases in de nieuwe compute-instantie. Voor sommige combinaties van-servicelaag en verkleiningen compute, databasebestanden losgekoppeld en opnieuw gekoppeld tijdens de switch.  Ongeacht kan de switch leiden tot een korte serviceonderbreking wanneer databases niet beschikbaar in het algemeen voor minder dan 30 seconden en vaak slechts enkele seconden zijn. Als er langlopende transacties uitgevoerd wanneer verbindingen worden verwijderd, de duur van deze stap kan het langer duren om te herstellen van afgebroken transacties. [Versneld databaseherstel](sql-database-accelerated-database-recovery.md) de impact kunt beperken langlopende transacties afgebroken.

> [!IMPORTANT]
> Gegevens niet verloren tijdens een stap in de werkstroom.

### <a name="latency-of-changing-service-tier-or-rescaling-compute-size"></a>Latentie van de service tier of schaling aanpassen compute grootte te wijzigen

De latentie te wijzigen van de servicelaag of de compute-grootte van een individuele database of elastische pool oorspronkelijke met parameters als volgt:

|Servicelaag|Één Basic-database,</br>Standard (S0-S1)|Basisbeperkingen voor elastische groepen,</br>Standard (S2-S12) </br>Zeer grootschalige, </br>Algemeen doel individuele database of elastische pool|Premium en bedrijfskritiek individuele database of elastische pool|
|:---|:---|:---|:---|
|**Één Basic-database,</br> Standard (S0-S1)**|&bull; &nbsp;Constante wachttijd onafhankelijk van de gebruikte ruimte</br>&bull; &nbsp;Meestal minder dan vijf minuten|&bull; &nbsp;Latentie in verhouding met databaseruimte gebruikt vanwege het kopiëren van gegevens</br>&bull; &nbsp;Meestal minder dan 1 minuut per GB aan ruimte die wordt gebruikt|&bull; &nbsp;Latentie in verhouding met databaseruimte gebruikt vanwege het kopiëren van gegevens</br>&bull; &nbsp;Meestal minder dan 1 minuut per GB aan ruimte die wordt gebruikt|
|**Basisbeperkingen voor elastische groepen, </br>Standard (S2-S12) </br>zeer grootschalige, </br>algemeen gebruik één database of elastische pool**|&bull; &nbsp;Latentie in verhouding met databaseruimte gebruikt vanwege het kopiëren van gegevens</br>&bull; &nbsp;Meestal minder dan 1 minuut per GB aan ruimte die wordt gebruikt|&bull; &nbsp;Constante wachttijd onafhankelijk van de gebruikte ruimte</br>&bull; &nbsp;Meestal minder dan vijf minuten|&bull; &nbsp;Latentie in verhouding met databaseruimte gebruikt vanwege het kopiëren van gegevens</br>&bull; &nbsp;Meestal minder dan 1 minuut per GB aan ruimte die wordt gebruikt|
|**Premium en bedrijfskritiek individuele database of elastische pool**|&bull; &nbsp;Latentie in verhouding met databaseruimte gebruikt vanwege het kopiëren van gegevens</br>&bull; &nbsp;Meestal minder dan 1 minuut per GB aan ruimte die wordt gebruikt|&bull; &nbsp;Latentie in verhouding met databaseruimte gebruikt vanwege het kopiëren van gegevens</br>&bull; &nbsp;Meestal minder dan 1 minuut per GB aan ruimte die wordt gebruikt|&bull; &nbsp;Latentie in verhouding met databaseruimte gebruikt vanwege het kopiëren van gegevens</br>&bull; &nbsp;Meestal minder dan 1 minuut per GB aan ruimte die wordt gebruikt|

> [!NOTE]
>
> - In het geval van de servicelaag wijzigen of schaling compute voor een elastische pool aanpassen, moet de som van de ruimte die wordt gebruikt voor alle databases in de pool worden gebruikt voor het berekenen van de schatting.
> - In het geval van een database verplaatsen naar/van een elastische pool, alleen de ruimte die wordt gebruikt door de database heeft gevolgen voor de latentie, niet de ruimte die wordt gebruikt door de elastische pool.
>
> [!TIP]
> Als u wilt bewaken in voortgang van bewerkingen, Zie: [Beheren met behulp van de REST-API voor SQL operations](https://docs.microsoft.com/rest/api/sql/operations/list), [bewerkingen met behulp van CLI beheren](/cli/azure/sql/db/op), [bewaken van bewerkingen met behulp van T-SQL](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) en deze twee PowerShell-opdrachten: [Get-AzSqlDatabaseActivity](/powershell/module/az.sql/get-azsqldatabaseactivity) en [Stop-AzSqlDatabaseActivity](/powershell/module/az.sql/stop-azsqldatabaseactivity).

### <a name="additional-considerations-when-changing-service-tier-or-rescaling-compute-size"></a>Aanvullende overwegingen bij het wijzigen van grootte van laag of schaling aanpassen compute-service

- Bij het verkleinen alvorens te vCores of edtu's voor een elastische pool, zijn de toepassingen die wordt gebruikt-ruimte moet kleiner zijn dan de maximaal toegestane grootte van de doel-service-laag en de groep edtu's.
- Wanneer schaling aanpassen vCores of edtu's voor een elastische pool, is een extra opslagkosten van toepassing als (1) de maximale opslaggrootte van de pool wordt ondersteund door de doelgroep en (2) de maximale opslaggrootte groter is dan de hoeveelheid inbegrepen opslag van de doelgroep. Bijvoorbeeld, als een 100 eDTU Standard-pool met een maximale grootte van 100 GB aan een 50 eDTU Standard-pool wordt verkleind, is een extra opslagkosten van toepassing omdat doelgroep een maximale grootte van 100 GB ondersteunt en de overeengekomen opslaghoeveelheid alleen 50 GB wordt. De hoeveelheid extra opslagruimte is dus 100 GB-50 GB = 50 GB. Zie voor informatie over prijzen van extra opslagruimte [prijzen van SQL Database](https://azure.microsoft.com/pricing/details/sql-database/). Als de werkelijke hoeveelheid ruimte die wordt gebruikt kleiner dan de hoeveelheid inbegrepen opslag is en vervolgens deze extra kosten kan worden vermeden door de maximale grootte van de database voor de hoeveelheid die inclusief te beperken.

### <a name="billing-during-rescaling"></a>Facturering tijdens schaling aanpassen

U worden in rekening gebracht voor elk uur bestaat in een database met behulp van de hoogste servicelaag + compute-grootte die tijdens dat uur, ongeacht het gebruik of of de database minder dan een uur actief is toegepast. Als u een individuele database maken en deze vijf minuten later verwijdert weerspiegelt uw factuur een post voor één database-uur.

## <a name="change-elastic-pool-storage-size"></a>Elastische pool opslaggrootte wijzigen

> [!IMPORTANT]
> In sommige gevallen is het wellicht voor het verkleinen van een database voor het vrijmaken van ongebruikte ruimte. Zie voor meer informatie, [bestandsruimte in Azure SQL Database beheren](sql-database-file-space-management.md).

### <a name="vcore-based-purchasing-model"></a>Op vCore gebaseerd aanschafmodel

- Opslag kan worden ingericht tot de maximale grootte is bereikt:

  - Voor opslag in de standard-of algemene service-lagen, vergroten of verkleinen in stappen van 10 GB
  - Voor opslag in de premium- of bedrijfskritiek Servicelagen, vergroten of verkleinen in stappen van 250 GB
- Opslag voor een elastische pool kan worden bevoorraad door vergroten of verkleinen van de maximale grootte.
- De prijs van opslag voor een elastische pool is de hoeveelheid opslagruimte, vermenigvuldigd met de prijs per eenheid opslag van de servicelaag. Zie voor meer informatie over de prijs van extra opslagruimte [prijzen van SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).

> [!IMPORTANT]
> In sommige gevallen is het wellicht voor het verkleinen van een database voor het vrijmaken van ongebruikte ruimte. Zie voor meer informatie, [bestandsruimte in Azure SQL Database beheren](sql-database-file-space-management.md).

### <a name="dtu-based-purchasing-model"></a>DTU gebaseerde aankoopmodel

- De prijs eDTU voor een elastische pool is inclusief een bepaalde hoeveelheid opslagruimte zonder extra kosten. Extra opslagruimte bovenop de inbegrepen hoeveelheid worden ingezet er gelden aanvullende kosten tot de maximale grootte is bereikt in stappen van 250 GB tot 1 TB, en klik vervolgens in stappen van 256 GB dan 1 TB. Zie voor de hoeveelheid inbegrepen opslag en limieten voor de maximale berichtgrootte [elastische pool: opslaggrootte en compute-grootten](sql-database-dtu-resource-limits-elastic-pools.md#elastic-pool-storage-sizes-and-compute-sizes).
- Extra opslag voor een elastische pool kan worden ingericht met de toename van het gebruik van de maximale grootte van de [Azure-portal](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](/powershell/module/az.sql/Get-AzSqlElasticPool), wordt de [Azure CLI](/cli/azure/sql/elastic-pool#az-sql-elastic-pool-update), of de [REST-API ](https://docs.microsoft.com/rest/api/sql/elasticpools/update).
- De prijs voor extra opslagruimte voor een elastische pool is de hoeveelheid extra opslagruimte vermenigvuldigd met de prijs voor extra opslagruimte per eenheid van de servicelaag. Zie voor meer informatie over de prijs van extra opslagruimte [prijzen van SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).

> [!IMPORTANT]
> In sommige gevallen is het wellicht voor het verkleinen van een database voor het vrijmaken van ongebruikte ruimte. Zie voor meer informatie, [bestandsruimte in Azure SQL Database beheren](sql-database-file-space-management.md).

## <a name="next-steps"></a>Volgende stappen

Raadpleeg voor algemene resourcelimieten, [SQL Database vCore gebaseerde resourcelimieten - elastische pools](sql-database-vcore-resource-limits-elastic-pools.md) en [SQL Database DTU gebaseerde resourcelimieten - elastische pools](sql-database-dtu-resource-limits-elastic-pools.md).
