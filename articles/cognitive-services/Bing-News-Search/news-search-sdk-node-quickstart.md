---
title: 'Snelstart: Bing Nieuws zoeken-SDK, Node'
titleSuffix: Azure Cognitive Services
description: De Bing Nieuws zoeken-SDK-consoletoepassing instellen
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: quickstart
ms.date: 02/12/2018
ms.author: v-gedod
ms.openlocfilehash: 075ce771b6fa92663f172734aabf57b089b294a9
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2018
ms.locfileid: "52316936"
---
# <a name="quickstart-bing-news-search-sdk-with-node"></a>Snelstart: Bing Nieuws zoeken-SDK met Node

De Bing Nieuws zoeken-SDK bevat de functionaliteit van de REST API voor nieuwsaanvragen en parseerresultaten. 

De [-broncode voor voorbeelden van de Bing Nieuws zoeken-SDK voor Node](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/newsSearch.js) is beschikbaar op Git Hub.

## <a name="application-dependencies"></a>Afhankelijkheden van de toepassing
Haal een [Cognitive Services-toegangssleutel](https://azure.microsoft.com/try/cognitive-services/) op onder **Zoeken**.  Zie ook [Prijsinformatie Cognitive Services - Bing Zoeken-API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/). 

Stel een consoletoepassing in met de Bing News Search SDK:
* Voer `npm install ms-rest-azure` uit in uw ontwikkelomgeving.
* Voer `npm install azure-cognitiveservices-newssearch` uit in uw ontwikkelomgeving.

## <a name="news-search-client"></a>Nieuws zoeken-client
Haal een [Cognitive Services-toegangssleutel](https://azure.microsoft.com/try/cognitive-services/) op onder *Zoeken*. Maak een exemplaar van de `CognitiveServicesCredentials`:
```
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
```
Instantieer vervolgens de client:
```
const NewsSearchAPIClient = require('azure-cognitiveservices-newssearch');
let client = new NewsSearchAPIClient(credentials);
```
Gebruik de client om te zoeken met een querytekst, in dit geval 'Winter Olympics':
```
client.newsOperations.search('Winter Olympics').then((result) => {
    console.log(result.value);
}).catch((err) => {
    throw err;
});

```
De code geeft `result.value` items weer in de console zonder tekst te parseren. De resultaten, indien van toepassing per categorie, omvatten:
- _type: 'NewsArticle'
- _type: 'WebPage'
- _type: 'VideoObject'
- _type: 'ImageObject'

<!-- Remove until we can replace with santized version
![News results](media/node-sdk-quickstart-results.png)
-->

## <a name="next-steps"></a>Volgende stappen

[Voorbeelden voor Cognitive Services Node.js SDK](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)
