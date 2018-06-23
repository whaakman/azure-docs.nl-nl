---
title: Nieuws zoeken SDK knooppunt Quick Start | Microsoft Docs
description: De consoletoepassing nieuws Search SDK instellen
titleSuffix: Azure cognitive services
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: article
ms.date: 02/12/2018
ms.author: v-gedod
ms.openlocfilehash: 4ae99aa100b697a0dd75863c6f0c3c556dfa3d21
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345708"
---
# <a name="news-search-sdk-node-quickstart"></a>Nieuws zoeken SDK knooppunt Quick Start

De Bing nieuws Search SDK bevat de functionaliteit van de REST-API voor nieuws query's en parseren resultaten. 

De [broncode voor voorbeelden van knooppunt Bing nieuws Search SDK](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/newsSearch.js) beschikbaar is op de Git-Hub.

## <a name="application-dependencies"></a>Afhankelijkheden voor toepassingen

Uitvoeren als u een consoletoepassing met Bing nieuws Search SDK instelt, `npm install azure-cognitiveservices-newssearch` in uw ontwikkelomgeving.

## <a name="news-search-client"></a>Nieuws zoeken client
Ophalen van een [cognitieve toegangssleutel](https://azure.microsoft.com/try/cognitive-services/) onder *Search*. Maak een instantie van de `CognitiveServicesCredentials`:
```
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
```
Vervolgens exemplaar maken van de client:
```
const NewsSearchAPIClient = require('azure-cognitiveservices-newssearch');
let client = new NewsSearchAPIClient(credentials);
```
De client gebruiken om te zoeken met een querytekst, in dit geval 'Winter Olympische':
```
client.newsOperations.search('Winter Olympics').then((result) => {
    console.log(result.value);
}).catch((err) => {
    throw err;
});

```
De code wordt afgedrukt `result.value` items naar de console zonder bij het parseren van tekst. De resultaten, indien van toepassing per categorie, omvatten:
- _Type: 'NewsArticle'
- _Type: 'Webpagina'
- _Type: 'VideoObject'
- _Type: 'ImageObject'

<!-- Remove until we can replace with santized version
![News results](media/node-sdk-quickstart-results.png)
-->

## <a name="next-steps"></a>Volgende stappen

[Cognitieve services SDK voor Node.js-voorbeelden](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)
