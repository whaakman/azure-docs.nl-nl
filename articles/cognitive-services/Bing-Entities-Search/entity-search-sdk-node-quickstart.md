---
title: Entiteit zoeken SDK knooppunt Quick Start | Microsoft Docs
description: Setup voor de entiteit zoeken SDK-consoletoepassing.
titleSuffix: Azure cognitive services
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: article
ms.date: 02/12/2018
ms.author: v-gedod
ms.openlocfilehash: 2904ecfed33334458f9b6a9ca2500cd0bfef13bc
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "35345859"
---
# <a name="entity-search-sdk-node-quickstart"></a>Entiteit zoeken SDK knooppunt Quick Start

De Bing entiteit Search SDK bevat de functionaliteit van de REST-API voor entiteit query's en parseren resultaten. 

De [broncode voor voorbeelden van C# Bing entiteit Search SDK](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/entitySearch.js) beschikbaar is op de Git-Hub.
## <a name="application-dependencies"></a>Afhankelijkheden voor toepassingen

Uitvoeren als u een consoletoepassing met de Bing entiteit Search SDK instelt, `npm install azure-cognitiveservices-entitysearch` in uw ontwikkelomgeving.

## <a name="entity-search-client"></a>Entiteit zoeken client
Ophalen van een [cognitieve toegangssleutel](https://azure.microsoft.com/try/cognitive-services/) onder *Search*. Maak een instantie van de `CognitiveServicesCredentials`:
```
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
```
Vervolgens exemplaar maken van de client en zoekt u resultaten:
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
De code wordt afgedrukt `result.value` items naar de console zonder bij het parseren van tekst.  De resultaten, indien van toepassing per categorie, omvatten:
- _Type: 'Wat is'
- _Type: 'ImageObject'

<!-- Removing until we can replace with a sanitized version.
![Entity results](media/entity-search-sdk-node-quickstart-results.png)
-->

## <a name="next-steps"></a>Volgende stappen

[Cognitieve services SDK voor Node.js-voorbeelden](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)