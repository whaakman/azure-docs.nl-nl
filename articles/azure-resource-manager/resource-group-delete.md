---
title: Azure Resource Manager-resourcegroep voor verwijderen
description: Hierin wordt beschreven hoe Azure Resource Manager de verwijdering van resources orders wanneer een resourcegroep wordt verwijderd.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2018
ms.author: tomfitz
ms.openlocfilehash: 8b0711cab07584aa84ab437a2a4efb5aab92f3d1
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2018
ms.locfileid: "52319356"
---
# <a name="azure-resource-manager-resource-group-deletion"></a>Azure Resource Manager resource verwijderen

Dit artikel wordt beschreven hoe Azure Resource Manager de verwijdering van resources orders wanneer u een resourcegroep verwijderen.

## <a name="determine-order-of-deletion"></a>Bepaalt de volgorde van verwijdering

Wanneer u een resourcegroep verwijdert, Resource Manager de bepaalt de volgorde om resources te verwijderen. Hierbij de volgende volgorde:

1. Alle resources in de onderliggende (geneste) worden verwijderd.

2. Resources die andere resources beheren, worden vervolgens verwijderd. Een resource kan hebben de `managedBy` eigenschap is ingesteld om aan te geven dat een andere resource deze beheert. Als deze eigenschap is ingesteld, worden de resource die wordt beheerd van de andere resource wordt verwijderd voordat de andere resources.

3. De resterende resources zijn verwijderd na de vorige twee categorieën.

## <a name="resource-deletion"></a>Resource verwijderen

Nadat de volgorde is bepaald, problemen met Resource Manager een verwijderbewerking voor elke resource. Wacht voor eventuele afhankelijkheden om te voltooien voordat u doorgaat.

Voor synchrone bewerkingen kan zijn de verwachte reactie van geslaagde-codes:

* 200
* 204
* 404

Voor asynchrone bewerkingen is de verwachte reactie van geslaagde 202. Resource Manager houdt de location-header of de header van de azure-asynchrone bewerking om te bepalen van de status van de asynchrone bewerking.
  
### <a name="errors"></a>Fouten

Wanneer een delete-bewerking wordt een fout geretourneerd, probeert de aanroep voor het verwijderen door Resource Manager opnieuw. Nieuwe pogingen worden uitgevoerd voor de 5xx, 429 en statuscodes 408. Standaard is de periode voor opnieuw proberen 15 minuten.

## <a name="after-deletion"></a>Na verwijdering

Resource Manager geeft een GET-aanroep op elke bron die deze probeert te verwijderen. 404 wordt het antwoord van deze aanroep GET verwacht. Bij het Resource Manager haalt een 404-fout, partitielezer de verwijdering is voltooid. Resource Manager wordt de resource uit de cache verwijderd.

Als de GET-aanroep voor de resource een 200 of 201 retourneert, stelt Resource Manager de resource.

### <a name="errors"></a>Fouten

Als de GET-bewerking een fout retourneert, pogingen Resource Manager de GET voor de volgende foutcode:

* Minder dan 100
* 408
* 429
* Groter is dan 500

Resource Manager een failover voor andere foutcodes, het verwijderen van de resource.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over concepten van Resource Manager, [overzicht van Azure Resource Manager](resource-group-overview.md).
* Als u de bewerkingen voor een resourceprovider, Zie [REST API van Azure](/rest/api/).
