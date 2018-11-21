---
title: 'Snelstart: Bing Video Search SDK, Node'
titleSuffix: Azure Cognitive Services
description: De Bing Video Search SDK-consoletoepassing instellen.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: quickstart
ms.date: 02/12/2018
ms.author: rosh
ms.openlocfilehash: 9a2544fe5a3c0bac763b9aee79c36893ec56f351
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51686684"
---
# <a name="quickstart-bing-video-search-sdk-with-node"></a>Snelstart: Bing Video Search SDK met Node

De Bing Video Search SDK bevat de functionaliteit van de REST API voor videoquery's en parseerresultaten. 

De [broncode voor voorbeelden van de Bing Video Search SDK voor Node](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/videoSearch.js) is beschikbaar op Git Hub.

## <a name="application-dependencies"></a>Afhankelijkheden van de toepassing

Stel een consoletoepassing in met de Bing Video Search SDK:
* Voer `npm install ms-rest-azure` uit in uw ontwikkelomgeving.
* Voer `npm install azure-cognitiveservices-videosearch` uit in uw ontwikkelomgeving.

## <a name="video-search-client"></a>Video Search-client
Haal een [Cognitive Services-toegangssleutel](https://azure.microsoft.com/try/cognitive-services/) op onder *Zoeken*. Maak een exemplaar van de `CognitiveServicesCredentials`:
```
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
```
Instantieer vervolgens de client:
```
const VideoSearchAPIClient = require('azure-cognitiveservices-videosearch');
let client = new VideoSearchAPIClient(credentials);
```
Zoek naar resultaten.
```
client.videosOperations.search('Interstellar Trailer').then((result) => {
    console.log(result.value);
}).catch((err) => {
    throw err;
});

```

<!-- Remove until the response can be replace with a sanitized version.
The code prints `result.value` items to the console without parsing any text. The results will be:
- _type: 'VideoObjectElementType'

![Video results](media/video-search-sdk-node-results.png)
-->

## <a name="next-steps"></a>Volgende stappen

[Voorbeelden voor Cognitive Services Node.js SDK](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)
