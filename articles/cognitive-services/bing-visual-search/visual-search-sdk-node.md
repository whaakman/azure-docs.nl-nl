---
title: 'Quickstart: Afbeeldingsinzichten krijgen met behulp van de Bing Visual Search SDK voor Node.js'
titleSuffix: Azure Cognitive Services
description: Upload een afbeelding met behulp van de Bing Visual Search SDK en krijg inzicht in de afbeelding.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 06/18/2018
ms.author: aahi
ms.openlocfilehash: 6f7740ebd09a1fde564613c38ab12513ca51fece
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67204900"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-sdk-for-nodejs"></a>Quickstart: Afbeeldingsinzichten krijgen met behulp van de Bing Visual Search SDK voor Node.js

In deze snelstart leert u hoe u met de Node.js-SDK afbeeldingsinzichten krijgt uit de Bing Visual Search-service. Hoewel Bing Visual Search een REST API heeft die compatibel is met de meeste programmeertalen, biedt de SDK een eenvoudige manier om de service in uw toepassingen te integreren. De broncode voor dit voorbeeld is te vinden op [GitHub](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/visualSearch.js). 

## <a name="prerequisites"></a>Vereisten
* [Node.js](https://www.nodejs.org/)
* De Bing Visual Search SDK voor Node.js
    * U stelt een consoletoepassing in met behulp van de Bing Visual Search SDK met de volgende opdrachten:
        1. `npm install ms-rest-azure`
        2. `npm install azure-cognitiveservices-search-visualSearch`.


[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

<a name="client"></a>

## <a name="create-and-initialize-the-application"></a>De toepassing maken en initialiseren

1. Maak een nieuw JavaScript-bestand in uw favoriete IDE of editor en voeg de volgende vereisten toe. Maak vervolgens variabelen voor de abonnementssleutel , de aangepaste configuratie-id en het bestandspad naar de afbeelding die u wilt uploaden. 

    ```javascript
    const os = require("os");
    const async = require('async');
    const fs = require('fs');
    const Search = require('azure-cognitiveservices-visualsearch');
    const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
    
    let keyVar = 'YOUR-VISUAL-SEARCH-ACCESS-KEY';
    let credentials = new CognitiveServicesCredentials(keyVar);
    let filePath = "../Data/image.jpg";
    ```

2. Maak een instantie van de client.

    ```javascript
    let visualSearchClient = new Search.VisualSearchClient(credentials);
    ```

## <a name="search-for-images"></a>Zoeken naar afbeeldingen

1. Gebruik `fs.createReadStream()` om uw afbeeldingsbestand in te lezen en variabelen te maken voor uw zoekaanvraag en -resultaten. Gebruik vervolgens de client om te zoeken naar afbeeldingen.

    ```javascript
    let fileStream = fs.createReadStream(filePath);
    let visualSearchRequest = JSON.stringify({});
    let visualSearchResults;
    try {
        visualSearchResults = await visualSearchClient.images.visualSearch({
            image: fileStream,
            knowledgeRequest: visualSearchRequest
        });
        console.log("Search visual search request with binary of image");
    } catch (err) {
        console.log("Encountered exception. " + err.message);
    }
    ```

2. De resultaten van de vorige query parseren:

    ```javascript
    // Visual Search results
    if (visualSearchResults.image.imageInsightsToken) {
        console.log(`Uploaded image insights token: ${visualSearchResults.image.imageInsightsToken}`);
    }
    else {
        console.log("Couldn't find image insights token!");
    }
    
    // List of tags
    if (visualSearchResults.tags.length > 0) {
        let firstTagResult = visualSearchResults.tags[0];
        console.log(`Visual search tag count: ${visualSearchResults.tags.length}`);
    
        // List of actions in first tag
        if (firstTagResult.actions.length > 0) {
            let firstActionResult = firstTagResult.actions[0];
            console.log(`First tag action count: ${firstTagResult.actions.length}`);
            console.log(`First tag action type: ${firstActionResult.actionType}`);
        }
        else {
            console.log("Couldn't find tag actions!");
        }
    
    }
    else {
        console.log("Couldn't find image tags!");
    }
    
    ```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een web-app van één pagina maken](tutorial-bing-visual-search-single-page-app.md).
