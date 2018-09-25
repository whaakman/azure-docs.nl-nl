---
title: Schalen van resources voor elastische pool - Azure SQL Database | Microsoft Docs
description: Deze pagina worden beschreven vergroten/verkleinen resources voor elastische pools in Azure SQL Database.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 09/20/2018
ms.author: carlrab
ms.openlocfilehash: b0dce3a21a2d254a593db88246097b255f74a024
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47033087"
---
# <a name="scale-elastic-pool-resources-in-azure-sql-database"></a>Resources voor elastische pool schalen in Azure SQL Database

In dit artikel wordt beschreven hoe u de reken- en opslagresources die beschikbaar zijn voor elastische pools en gepoolde databases schalen in Azure SQL Database. 

## <a name="vcore-based-purchasing-model-change-elastic-pool-storage-size"></a>op vCore gebaseerde aankoopmodel: opslaggrootte van de elastische pool wijzigen

- Opslag kan worden ingericht tot de maximale grootte is bereikt: 
 - Voor Standard-opslag, vergroten of verkleinen in stappen van 10 GB
 - Voor Premium-opslag, vergroten of verkleinen in stappen van 250 GB
- Opslag voor een elastische pool kan worden bevoorraad door vergroten of verkleinen van de maximale grootte.
- De prijs van opslag voor een elastische pool is de hoeveelheid opslagruimte, vermenigvuldigd met de prijs per eenheid opslag van de servicelaag. Zie voor meer informatie over de prijs van extra opslagruimte [prijzen van SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).

> [!IMPORTANT]
> In sommige gevallen is het wellicht voor het verkleinen van een database voor het vrijmaken van ongebruikte ruimte. Zie voor meer informatie, [bestandsruimte in Azure SQL Database beheren](sql-database-file-space-management.md).

## <a name="vcore-based-purchasing-model-change-elastic-pool-compute-resources-vcores"></a>op vCore gebaseerde aankoopmodel: rekenresources (vCores) voor de elastische pool wijzigen

U kunt vergroten of verkleinen van de compute-grootte aan een elastische pool op basis van de resource moet met behulp van de [Azure-portal](sql-database-elastic-pool-scale.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqlelasticpool), wordt de [Azure CLI](/cli/azure/sql/elastic-pool#az-sql-elastic-pool-update), of de [ REST-API](/rest/api/sql/elasticpools/update).

- Wanneer schaling aanpassen pool vCores, databaseverbindingen kort verwijderd. Dit is hetzelfde gedrag net als treedt op wanneer schaling dtu's aanpassen voor een individuele database (niet in een groep). Zie voor meer informatie over de duur en de impact van verbroken verbindingen voor een database tijdens de bewerkingen voor opnieuw schalen, [dtu's schaling aanpassen voor een individuele database](#single-database-change-storage-size). 
- De duur op de oorspronkelijke groep vCores kan afhankelijk van de totale hoeveelheid opslagruimte die wordt gebruikt door alle databases in de groep. In het algemeen de opnieuw schalen latentie gemiddelde 90 minuten of minder per 100 GB. Bijvoorbeeld, als de totale ruimte wordt gebruikt door alle databases in de groep is 200 GB en de verwachte latentie voor schaling aanpassen van de groep is 3 uur of minder. In sommige gevallen binnen de Standard- of Basic-laag mag de opnieuw schalen latentie van minder dan vijf minuten, ongeacht de hoeveelheid ruimte die wordt gebruikt.
- In het algemeen is de duur voor het wijzigen van de min-vCores per database of het maximum aantal vCores per database is vijf minuten of minder.
- Wanneer verkleinen alvorens te pool vCores, is de groep gebruikt ruimte moet kleiner zijn dan de maximaal toegestane grootte van de doel-service-laag en groep vCores.

## <a name="dtu-based-purchasing-model-change-elastic-pool-storage-size"></a>DTU gebaseerde aankoopmodel: opslaggrootte van de elastische pool wijzigen

- De prijs eDTU voor een elastische pool is inclusief een bepaalde hoeveelheid opslagruimte zonder extra kosten. Extra opslagruimte bovenop de inbegrepen hoeveelheid worden ingezet er gelden aanvullende kosten tot de maximale grootte is bereikt in stappen van 250 GB tot 1 TB, en klik vervolgens in stappen van 256 GB dan 1 TB. Zie voor de hoeveelheid inbegrepen opslag en limieten voor de maximale berichtgrootte [elastische pool: opslaggrootte en compute-grootten](sql-database-dtu-resource-limits-elastic-pools.md#elastic-pool-storage-sizes-and-compute-sizes).
- Extra opslag voor een elastische pool kan worden ingericht met de toename van het gebruik van de maximale grootte van de [Azure-portal](sql-database-elastic-pool-scale.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqlelasticpool), wordt de [Azure CLI](/cli/azure/sql/elastic-pool#az-sql-elastic-pool-update), of de [REST-API ](/rest/api/sql/elasticpools/update).
- De prijs voor extra opslagruimte voor een elastische pool is de hoeveelheid extra opslagruimte vermenigvuldigd met de prijs voor extra opslagruimte per eenheid van de servicelaag. Zie voor meer informatie over de prijs van extra opslagruimte [prijzen van SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).

> [!IMPORTANT]
> In sommige gevallen is het wellicht voor het verkleinen van een database voor het vrijmaken van ongebruikte ruimte. Zie voor meer informatie, [bestandsruimte in Azure SQL Database beheren](sql-database-file-space-management.md).

## <a name="dtu-based-purchasing-model-change-elastic-pool-compute-resources-edtus"></a>DTU gebaseerde aankoopmodel: rekenresources (edtu's) voor de elastische pool wijzigen

U kunt vergroten of verkleinen van de beschikbare resources voor een elastische pool op basis van de resource moet met behulp van de [Azure-portal](sql-database-elastic-pool-scale.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqlelasticpool), wordt de [Azure CLI](/cli/azure/sql/elastic-pool#az-sql-elastic-pool-update), of de [ REST-API](/rest/api/sql/elasticpools/update).

- Wanneer schaling met het aanpassen van de edtu's, zijn kort databaseverbindingen verwijderd. Dit is hetzelfde gedrag net als treedt op wanneer schaling dtu's aanpassen voor een individuele database (niet in een groep). Zie voor meer informatie over de duur en de impact van verbroken verbindingen voor een database tijdens de bewerkingen voor opnieuw schalen, [dtu's schaling aanpassen voor een individuele database](#single-database-change-storage-size). 
- De duur van oorspronkelijke Edtu kan afhankelijk van de totale hoeveelheid opslagruimte die wordt gebruikt door alle databases in de groep. In het algemeen de opnieuw schalen latentie gemiddelde 90 minuten of minder per 100 GB. Bijvoorbeeld, als de totale ruimte wordt gebruikt door alle databases in de groep is 200 GB en de verwachte latentie voor schaling aanpassen van de groep is 3 uur of minder. In sommige gevallen binnen de Standard- of Basic-laag mag de opnieuw schalen latentie van minder dan vijf minuten, ongeacht de hoeveelheid ruimte die wordt gebruikt.
- In het algemeen is de duur voor het wijzigen van het minimaal aantal edtu's per database of maximumaantal edtu's per database is vijf minuten of minder.
- Wanneer verkleinen alvorens te edtu's, zijn de toepassingen die wordt gebruikt-ruimte moet kleiner zijn dan de maximaal toegestane grootte van de doel-service-laag en de groep edtu's.
- Wanneer schaling met het aanpassen van de edtu's, is een extra opslagkosten van toepassing als (1) de maximale opslaggrootte van de pool wordt ondersteund door de doelgroep en (2) de maximale opslaggrootte groter is dan de hoeveelheid inbegrepen opslag van de doelgroep. Bijvoorbeeld, als een 100 eDTU Standard-pool met een maximale grootte van 100 GB aan een 50 eDTU Standard-pool wordt verkleind, is een extra opslagkosten van toepassing omdat doelgroep een maximale grootte van 100 GB ondersteunt en de overeengekomen opslaghoeveelheid alleen 50 GB wordt. De hoeveelheid extra opslagruimte is dus 100 GB-50 GB = 50 GB. Zie voor informatie over prijzen van extra opslagruimte [prijzen van SQL Database](https://azure.microsoft.com/pricing/details/sql-database/). Als de werkelijke hoeveelheid ruimte die wordt gebruikt kleiner dan de hoeveelheid inbegrepen opslag is en vervolgens deze extra kosten kan worden vermeden door de maximale grootte van de database voor de hoeveelheid die inclusief te beperken. 

## <a name="next-steps"></a>Volgende stappen

Raadpleeg voor algemene resourcelimieten, [SQL Database vCore gebaseerde resourcelimieten - elastische pools](sql-database-vcore-resource-limits-elastic-pools.md) en [SQL Database DTU gebaseerde resourcelimieten - elastische pools](sql-database-dtu-resource-limits-elastic-pools.md).
