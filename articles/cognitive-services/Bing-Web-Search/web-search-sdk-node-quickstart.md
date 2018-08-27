---
title: 'Snelstartgids: Gebruik van de Bing webzoekopdrachten SDK voor Node.js'
description: Instellingen voor zoeken op het Web SDK-consoletoepassing.
titleSuffix: Azure cognitive services
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 08/16/2018
ms.author: v-gedod, erhopf
ms.openlocfilehash: e25c295fc0fc144110325d3c494a513ea35aeb05
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "42888587"
---
# <a name="quickstart-use-the-bing-web-search-sdk-for-nodejs"></a>Snelstartgids: Gebruik van de Bing webzoekopdrachten SDK voor Node.js

De Bing Web Search SDK bevat de functionaliteit van de REST-API voor web-query's en parseren resultaten.

De [broncode voor knooppunt Bing Web Search SDK-voorbeelden](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/webSearch.js) is beschikbaar op GitHub.

## <a name="application-dependencies"></a>Afhankelijkheden voor toepassingen

Als u een consoletoepassing met behulp van de Bing Web Search SDK instelt, worden uitgevoerd `npm install azure-cognitiveservices-websearch` in uw ontwikkelingsomgeving.

## <a name="web-search-client"></a>WebClient zoeken
Krijgen een [Cognitive Services-abonnementssleutel](https://azure.microsoft.com/try/cognitive-services/) onder *zoeken*. Maak een instantie van de `CognitiveServicesCredentials`:
```
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
```
Vervolgens exemplaar maken van de client:
```
const WebSearchAPIClient = require('azure-cognitiveservices-websearch');
let webSearchApiClient = new WebSearchAPIClient(credentials);
```
Zoeken naar resultaten:
```
webSearchApiClient.web.search('seahawks').then((result) => {
    let properties = ["images", "webPages", "news", "videos"];
    for (let i = 0; i < properties.length; i++) {
        if (result[properties[i]]) {
            console.log(result[properties[i]].value);
        } else {
            console.log(`No ${properties[i]} data`);
        }
    }
}).catch((err) => {
    throw err;
})

```
De code worden afgedrukt `result.value` items naar de console zonder het parseren van tekst.  De resultaten, indien van toepassing per categorie, omvat:
- _Type: 'ImageObject'
- _Type: 'NewsArticle'
- _Type: 'Webpagina'
- _Type: 'VideoObjectElementType'

<!-- Remove until this can be replaced with a sanitized version.
![Video results](media/web-search-sdk-node-results.png)
-->

## <a name="next-steps"></a>Volgende stappen

[Cognitive services-SDK voor Node.js-voorbeelden](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)
