---
title: Video zoeken SDK knooppunt Quick Start | Microsoft Docs
description: Instellingen voor de consoletoepassing Video Search SDK.
titleSuffix: Azure cognitive services
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: article
ms.date: 02/12/2018
ms.author: v-gedod
ms.openlocfilehash: 5718c750288e0a5605db3296d2911cca5e03375c
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "35345863"
---
# <a name="video-search-sdk-node-quickstart"></a>Video zoeken SDK knooppunt Quick Start

De Bing Video Search SDK bevat de functionaliteit van de REST-API voor video's en parseren resultaten. 

De [broncode voor voorbeelden van knooppunt Bing Video Search SDK](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/videoSearch.js) beschikbaar is op de Git-Hub.

## <a name="application-dependencies"></a>Afhankelijkheden voor toepassingen

Uitvoeren als u een consoletoepassing met de Bing Video Search SDK instelt, `npm install azure-cognitiveservices-videosearch` in uw ontwikkelomgeving.

## <a name="video-search-client"></a>Video zoeken client
Ophalen van een [cognitieve toegangssleutel](https://azure.microsoft.com/try/cognitive-services/) onder *Search*. Maak een instantie van de `CognitiveServicesCredentials`:
```
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
```
Vervolgens exemplaar maken van de client:
```
const VideoSearchAPIClient = require('azure-cognitiveservices-videosearch');
let client = new VideoSearchAPIClient(credentials);
```
Zoeken naar resultaten.
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

[Cognitieve services SDK voor Node.js-voorbeelden](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)
