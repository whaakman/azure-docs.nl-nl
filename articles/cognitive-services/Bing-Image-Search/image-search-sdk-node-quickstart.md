---
title: 'Snelstartgids: Zoeken voor afbeeldingen met behulp van de Bing afbeeldingen zoeken-SDK en Node.js'
description: In deze snelstartgids hebt u aanvragen en filteren van de installatiekopieën die zijn geretourneerd door Bing afbeeldingen zoeken, met behulp van Node.js.
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: cagronlund
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 08/28/2018
ms.author: aahi
ms.openlocfilehash: ebe6629344c076119c0bfdaee76a69df6274ca28
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45572573"
---
# <a name="quickstart-search-for-images-with-the-bing-image-search-sdk-and-nodejs"></a>Snelstartgids: Zoeken voor afbeeldingen met de Bing afbeeldingen zoeken-SDK en Node.js

Met deze Quick Start kunt u uw eerste afbeeldingen zoeken met behulp van de Bing afbeeldingen zoeken SDK, die een wrapper voor de API en bevat de dezelfde functies. Deze eenvoudige JavaScript-toepassing verzendt een zoekquery afbeelding, parseert de JSON-antwoord en Hiermee geeft u de URL van de eerste afbeelding geretourneerd.

De broncode voor dit voorbeeld is beschikbaar op [GitHub](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/imageSearch.js) met extra foutafhandeling en aantekeningen.

## <a name="prerequisites"></a>Vereisten

* De [Cognitive Services Image Search SDK voor Node.js](https://www.npmjs.com/package/azure-cognitiveservices-imagesearch)
    * Installeren met behulp van `npm install azure-cognitiveservices-imagesearch`
* De [Node.js Azure Rest](https://www.npmjs.com/package/ms-rest-azure) module
    * Installeren met behulp van `npm install ms-rest-azure`

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Maken en initialiseren van de toepassing

1. Maak een nieuwe JavaScript-bestand in uw favoriete IDE of editor, en stel de strikte regels, https en andere vereisten.

    ```javascript
    'use strict';
    https = require('https');
    const Search = require('azure-cognitiveservices-imagesearch');
    const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
    ```

2. In de belangrijkste methode voor uw project, maakt u variabelen voor de sleutel geldig abonnement, de afbeeldingsresultaten moet worden geretourneerd door Bing en een zoekterm. Instantieer vervolgens de afbeelding zoeken-client met behulp van de sleutel.

    ```javascript
    //replace this value with your valid subscription key.
    let serviceKey = "ENTER YOUR KEY HERE";

    //the search term for the request 
    let searchTerm = "canadian rockies";

    //instantiate the the image search client 
    let credentials = new CognitiveServicesCredentials(serviceKey);
    let imageSearchApiClient = new Search.ImageSearchAPIClient(credentials);

    ```

## <a name="create-an-asynchronous-helper-function"></a>Een asynchrone Help-functie maken

1. Een functie voor het aanroepen van de client asynchroon maken en het antwoord retourneren van de Bing afbeeldingen zoeken-service.  
    ```javascript
    //a helper function to perform an async call to the Bing Image Search API
    const sendQuery = async () => {
        return await imageSearchApiClient.imagesOperations.search(searchTerm);
    };
    ```
## <a name="send-a-query-and-handle-the-response"></a>Een query verzenden en verwerken van het antwoord 

1. Aanroepen van de Help-functie en verwerk de `promise` parseren van de afbeeldingsresultaten in het antwoord geretourneerd.

    Als het antwoord bevat een lijst met zoekresultaten, het eerste resultaat opslaan en afdrukken van de details, zoals een miniatuur-URL, wordt de oorspronkelijke URL, samen met het totale aantal installatiekopieën geretourneerd.  
    ```javascript
    sendQuery().then(imageResults => {
        if (imageResults == null) {
        console.log("No image results were found.");
        }
        else {
            console.log(`Total number of images returned: ${imageResults.value.length}`);
            let firstImageResult = imageResults.value[0];
            //display the details for the first image result. After running the application,
            //you can copy the resulting URLs from the console into your browser to view the image. 
            console.log(`Total number of images found: ${imageResults.value.length}`);
            console.log(`Copy these URLs to view the first image returned:`);
            console.log(`First image thumbnail url: ${firstImageResult.thumbnailUrl}`);
            console.log(`First image content url: ${firstImageResult.contentUrl}`); 
        }
      })
      .catch(err => console.error(err))
    ```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zelfstudie voor Bing afbeeldingen zoeken-app met één pagina](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/tutorial-bing-image-search-single-page-app)

## <a name="see-also"></a>Zie ook 

* [Wat is de Bing afbeeldingen zoeken?](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/overview)  
* [Probeer een online interactieve demo](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
* [Een gratis Cognitive Services-toegangssleutel ophalen](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api) 
* [Node.js-voorbeelden voor de SDK van Azure Cognitive Services](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples) 
* [Documentatie voor Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services)
* [Bing afbeeldingen zoeken-API-verwijzing](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)