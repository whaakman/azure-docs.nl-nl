---
title: Zoeken op het web SDK knooppunt Quick Start | Microsoft Docs
description: Setup voor zoeken op het Web SDK-consoletoepassing.
titleSuffix: Azure cognitive services
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 02/12/2018
ms.author: v-gedod
ms.openlocfilehash: 44f7f97f6c442df3fbb1e5e08189b8db7d4b9db0
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345713"
---
# <a name="web-search-sdk-node-quickstart"></a>Web Search SDK knooppunt Quick Start

De Bing Web Search SDK bevat de functionaliteit van de REST-API voor web-query's en parseren resultaten.

De [broncode voor voorbeelden van knooppunt Bing Web Search SDK](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/webSearch.js) beschikbaar is op de Git-Hub.

## <a name="application-dependencies"></a>Afhankelijkheden voor toepassingen

Uitvoeren als u een consoletoepassing met de Bing Web Search SDK instelt, `npm install azure-cognitiveservices-websearch` in uw ontwikkelomgeving.

## <a name="web-search-client"></a>WebClient zoeken
Ophalen van een [cognitieve toegangssleutel](https://azure.microsoft.com/try/cognitive-services/) onder *Search*. Maak een instantie van de `CognitiveServicesCredentials`:
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
De code wordt afgedrukt `result.value` items naar de console zonder bij het parseren van tekst.  De resultaten, indien van toepassing per categorie, omvatten:
- _Type: 'ImageObject'
- _Type: 'NewsArticle'
- _Type: 'Webpagina'
- _Type: 'VideoObjectElementType'

<!-- Remove until this can be replaced with a sanitized version.
![Video results](media/web-search-sdk-node-results.png)
-->

## <a name="next-steps"></a>Volgende stappen

[Cognitieve services SDK voor Node.js-voorbeelden](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)
