---
title: 'Snelstart: Bing Entiteiten zoeken-SDK, Node'
titleSuffix: Azure Cognitive Services
description: De Entiteiten zoeken-SDK-consoletoepassing instellen met Node.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: quickstart
ms.date: 02/12/2018
ms.author: v-gedod
ms.openlocfilehash: f94e3b5a6070da5ef9510216abd3f52a958030c5
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2018
ms.locfileid: "52311378"
---
# <a name="quickstart-bing-entity-search-sdk-with-node"></a>Snelstart: Bing Entiteiten zoeken-SDK met Node

De Bing Entiteiten zoeken-SDK bevat de functionaliteit van de REST API voor entiteitsquery's en het parseren van resultaten. 

De [broncode voor voorbeelden van de Bing Entiteiten zoeken-SDK voor C#](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/entitySearch.js) is beschikbaar op GitHub.
## <a name="application-dependencies"></a>Afhankelijkheden van de toepassing
Haal een [Cognitive Services-toegangssleutel](https://azure.microsoft.com/try/cognitive-services/) op onder **Zoeken**.  Zie ook [Prijsinformatie Cognitive Services - Bing Zoeken-API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

Stel een consoletoepassing in met de Bing Entity Search SDK:
* Voer `npm install ms-rest-azure` uit in uw ontwikkelomgeving.
* Voer `npm install azure-cognitiveservices-entitysearch` uit in uw ontwikkelomgeving.

## <a name="entity-search-client"></a>Entiteiten zoeken-client
Haal een [Cognitive Services-toegangssleutel](https://azure.microsoft.com/try/cognitive-services/) op onder *Zoeken*. Maak een exemplaar van de `CognitiveServicesCredentials`:
```
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
```
Maak vervolgens een exemplaar van de client en zoek naar resultaten:
```
const EntitySearchAPIClient = require('azure-cognitiveservices-entitysearch');

let entitySearchApiClient = new EntitySearchAPIClient(credentials);

entitySearchApiClient.entitiesOperations.search('seahawks').then((result) => {
    console.log(result.queryContext);
    console.log(result.entities.value);
    console.log(result.entities.value[0].description);
}).catch((err) => {
    throw err;
});

```
De code geeft `result.value` items weer in de console zonder tekst te parseren.  De resultaten, indien van toepassing per categorie, omvatten:
- _type: 'Thing'
- _type: 'ImageObject'

<!-- Removing until we can replace with a sanitized version.
![Entity results](media/entity-search-sdk-node-quickstart-results.png)
-->

## <a name="next-steps"></a>Volgende stappen

[Voorbeelden voor Cognitive Services Node.js SDK](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)
