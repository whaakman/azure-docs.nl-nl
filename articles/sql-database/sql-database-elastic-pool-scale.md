---
title: Schalen van resources van elastische pool - Azure SQL Database | Microsoft Docs
description: Deze pagina beschrijft vergroten/verkleinen resources voor elastische pools in Azure SQL Database.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: carlrab
ms.openlocfilehash: 0d15382f413485ccc287bac945b3c88eb748a9f6
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36313316"
---
# <a name="scale-elastic-pool-resources-in-azure-sql-database"></a>Resources van elastische pool schalen in Azure SQL Database

In dit artikel wordt beschreven hoe de berekenings- en bronnen beschikbaar voor elastische pools en gegroepeerde databases schalen in Azure SQL Database. 

## <a name="vcore-based-purchasing-model-change-elastic-pool-storage-size"></a>op basis van vCore aankoopmodel: opslaggrootte van de elastische groep wijzigen

- Opslagruimte worden ingericht tot de maximale grootte bereikt: 
 - Voor Standard-opslag vergroten of verkleinen in stappen van 10 GB
 - Voor Premium-opslag vergroten of verkleinen in stappen van 250 GB
- Opslag voor een elastische groep kan worden bevoorraad door vergroten of verkleinen van de maximale grootte.
- De prijs van opslag voor een elastische groep is de opslaghoeveelheid vermenigvuldigd met de eenheidsprijs opslag van de servicetier. Zie voor meer informatie over de prijs van extra opslagruimte [prijzen SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="vcore-based-purchasing-model-change-elastic-pool-compute-resources-vcores"></a>op basis van vCore aankoopmodel: rekenresources (vCores) voor de elastische groep wijzigen

U kunt vergroten of verkleinen van het prestatieniveau voor een elastische pool op basis van de resource die behoeften met behulp van de [Azure-portal](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqlelasticpool), wordt de [Azure CLI](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_update), of de [REST-API](/rest/api/sql/elasticpools/update).

- Wanneer schaling aanpassen vCores groep, worden de databaseverbindingen kort verwijderd. Dit is hetzelfde gedrag als deze gebeurtenis treedt op wanneer schaling dtu's aanpassen voor een individuele database (niet in een groep). Zie voor meer informatie over de duur en de gevolgen van het verbroken verbindingen voor een database tijdens bewerkingen schaling aanpassen [dtu's schaling aanpassen voor een individuele database](#single-database-change-storage-size). 
- De duur op de oorspronkelijke groep vCores kan afhankelijk van de totale hoeveelheid opslagruimte die wordt gebruikt door alle databases in de groep. In het algemeen de rescaling latentie gemiddelde 90 minuten of minder per 100 GB. Bijvoorbeeld, als de totale ruimte gebruikt door alle databases in de pool is 200 GB dan drie uur is de verwachte latentie voor schaling aanpassen van de groep of minder. In sommige gevallen binnen de categorie Standard of Basic mag de rescaling latentie onder vijf minuten ongeacht de hoeveelheid ruimte die wordt gebruikt.
- De duur voor het wijzigen van de vCores min. per database of max vCores per database is in het algemeen vijf minuten of minder.
- Wanneer toepassingen vCores downsizing, moet de gebruikte ruimte kleiner zijn dan de maximaal toegestane grootte van de doel-service tier en de pool vCores.

## <a name="dtu-based-purchasing-model-change-elastic-pool-storage-size"></a>Op basis van DTU aankoopmodel: opslaggrootte van de elastische groep wijzigen

- De prijs van de eDTU voor een elastische pool bevat een bepaalde hoeveelheid opslag zonder extra kosten. Extra opslagruimte boven het inbegrepen bedrag kan worden ingericht voor extra kosten tot de maximale grootte bereikt, in stappen van 250 GB tot 1 TB en vervolgens in stappen van 256 GB dan 1 TB. Zie voor opgenomen opslag bedragen en limieten voor de maximale berichtgrootte [elastische pool: opslaggrootte en prestatieniveaus](#elastic-pool-storage-sizes-and-performance-levels).
- Extra opslag voor een elastische pool kan worden ingericht met de toename van het gebruik van de maximale grootte van de [Azure-portal](sql-database-elastic-pool-scale.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqlelasticpool), wordt de [Azure CLI](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_update), of de [REST-API ](/rest/api/sql/elasticpools/update).
- De prijs van extra opslagruimte voor een elastische pool is de extra opslagruimte vermenigvuldigd met de eenheidsprijs extra opslagruimte van de servicetier. Zie voor meer informatie over de prijs van extra opslagruimte [prijzen SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="dtu-based-purchasing-model-change-elastic-pool-compute-resources-edtus"></a>Op basis van DTU aankoopmodel: rekenresources (edtu's) voor de elastische groep wijzigen

U kunt vergroten of verkleinen van de beschikbare bronnen voor een elastische pool op basis van de resource die behoeften met behulp van de [Azure-portal](sql-database-elastic-pool-scale.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqlelasticpool), wordt de [Azure CLI](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_update), of de [ REST-API](/rest/api/sql/elasticpools/update).

- Wanneer schaling aanpassen groepen met edtu's, worden de databaseverbindingen kort verwijderd. Dit is hetzelfde gedrag als deze gebeurtenis treedt op wanneer schaling dtu's aanpassen voor een individuele database (niet in een groep). Zie voor meer informatie over de duur en de gevolgen van het verbroken verbindingen voor een database tijdens bewerkingen schaling aanpassen [dtu's schaling aanpassen voor een individuele database](#single-database-change-storage-size). 
- De duur van groepen met edtu's oorspronkelijke kan afhankelijk van de totale hoeveelheid opslagruimte die wordt gebruikt door alle databases in de groep. In het algemeen de rescaling latentie gemiddelde 90 minuten of minder per 100 GB. Bijvoorbeeld, als de totale ruimte gebruikt door alle databases in de pool is 200 GB dan drie uur is de verwachte latentie voor schaling aanpassen van de groep of minder. In sommige gevallen binnen de categorie Standard of Basic mag de rescaling latentie onder vijf minuten ongeacht de hoeveelheid ruimte die wordt gebruikt.
- De duur voor het wijzigen van de edtu min's per database of max edtu's per database is in het algemeen vijf minuten of minder.
- Wanneer downsizing groepen met edtu's, moet de toepassingen die wordt gebruikt-ruimte kleiner zijn dan de maximaal toegestane grootte van de doel-service tier en de pool edtu's.
- Wanneer schaling groepen met edtu's aanpassen, wordt een extra opslagkosten geldt als (1) de maximale opslaggrootte van de groep wordt ondersteund door de doelgroep en (2) de maximale opslaggrootte de opgenomen opslaghoeveelheid van de doelgroep overschrijdt. Bijvoorbeeld, als een 100 eDTU Standard-pool met een maximale grootte van 100 GB aan een 50 eDTU Standard-pool wordt verkleind, is een extra opslagkosten van toepassing omdat doelgroep een maximale grootte van 100 GB ondersteunt en de opgenomen opslaghoeveelheid slechts 50 GB is. De extra opslagruimte is dus 100 – 50 GB = 50 GB. Zie voor de prijzen van extra opslagruimte [prijzen SQL Database](https://azure.microsoft.com/pricing/details/sql-database/). Als de werkelijke hoeveelheid schijfruimte minder dan de opgenomen opslaghoeveelheid is, kan klikt u vervolgens dit extra kosten verbonden worden vermeden door de maximale grootte van de database voor het bedrag opgenomen te beperken. 

## <a name="next-steps"></a>Volgende stappen

Raadpleeg voor algemene limieten, [SQL-Database op basis van vCore limieten - elastische pools](sql-database-vcore-resource-limits-elastic-pools.md) en [resource op basis van SQL Database DTU - limieten voor elastische pools](sql-database-dtu-resource-limits-elastic-pools.md).
