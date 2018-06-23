---
title: Zoeken naar afbeelding SDK knooppunt Quick Start | Microsoft Docs
description: Setup voor het zoeken naar afbeelding SDK-consoletoepassing.
titleSuffix: Azure cognitive services
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 02/12/2018
ms.author: v-gedod
ms.openlocfilehash: e4c8303e39accbb7caec15c0ef47d701971ce632
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345698"
---
# <a name="image-search-sdk-node-quickstart"></a>Afbeelding zoeken SDK knooppunt Quick Start

De Bing installatiekopie Search SDK bevat de functionaliteit van de REST-API voor de installatiekopie van query's en parseren resultaten. 

De [broncode voor voorbeelden van knooppunt Bing installatiekopie Search SDK](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/imageSearch.js) beschikbaar is op de Git-Hub.

## <a name="application-dependencies"></a>Afhankelijkheden voor toepassingen

Uitvoeren als u een consoletoepassing met de Bing installatiekopie Search SDK instelt, `npm install azure-cognitiveservices-imagesearch` in uw ontwikkelomgeving.

## <a name="image-search-client"></a>Afbeelding zoeken client
Ophalen van een [cognitieve toegangssleutel](https://azure.microsoft.com/try/cognitive-services/) onder *Search*. Maak een instantie van de `CognitiveServicesCredentials`:
```
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
```
Vervolgens exemplaar maken van de client:
```
const ImageSearchAPIClient = require('azure-cognitiveservices-imagesearch');
let client = new ImageSearchAPIClient(credentials);
```
De client gebruiken om te zoeken met een querytekst, in dit geval 'El Capitan':
```
client.imagesOperations.search('El Capitan', function (err, result, request, response) {
    if (err) throw err;
    console.log(result.value);
});

```
<!-- Need to sanitize result
The code prints `result.value` items to the console without parsing any text. The results will be:
- _type: 'ImageObjectElementType'

![Imageresults](media/node-sdk-quickstart-image-results.png)
-->

## <a name="next-steps"></a>Volgende stappen

[Cognitieve services SDK voor Node.js-voorbeelden](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)