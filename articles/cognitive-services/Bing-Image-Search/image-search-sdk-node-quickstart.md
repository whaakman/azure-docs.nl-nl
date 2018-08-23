---
title: 'Snelstartgids: Vragen en filteren van afbeeldingen met behulp van de SDK in Node.js'
description: In deze snelstartgids hebt u aanvragen en filteren van de installatiekopieën die zijn geretourneerd door Bing afbeeldingen zoeken, met behulp van Node.js.
titleSuffix: Azure cognitive services
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 02/12/2018
ms.author: v-gedod
ms.openlocfilehash: e88c045b220192a617e6b8caf5d8d53f70a25b5e
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2018
ms.locfileid: "41987572"
---
# <a name="quickstart-request-and-filter-images-using-the-sdk-and-nodejs"></a>Snelstartgids: Vragen en filteren van afbeeldingen met behulp van de SDK en Node.js

De Bing afbeeldingen zoeken-SDK bevat de functionaliteit van de REST-API voor de installatiekopie van query's en parseren resultaten. 

De [broncode voor Node Bing afbeeldingen zoeken-SDK-voorbeelden](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/imageSearch.js) is beschikbaar op Git Hub.

## <a name="application-dependencies"></a>Afhankelijkheden voor toepassingen

Als u een consoletoepassing met behulp van de Bing afbeeldingen zoeken-SDK instelt, worden uitgevoerd `npm install azure-cognitiveservices-imagesearch` in uw ontwikkelingsomgeving.

## <a name="image-search-client"></a>Afbeeldingen zoeken-client
Krijgen een [Cognitive Services-toegangssleutel](https://azure.microsoft.com/try/cognitive-services/) onder *zoeken*. Maak een instantie van de `CognitiveServicesCredentials`:
```
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
```
Vervolgens exemplaar maken van de client:
```
const ImageSearchAPIClient = require('azure-cognitiveservices-imagesearch');
let client = new ImageSearchAPIClient(credentials);
```
De client gebruiken om te zoeken met de querytekst van een, in dit geval 'El Capitan':
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

[Cognitive services-SDK voor Node.js-voorbeelden](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)