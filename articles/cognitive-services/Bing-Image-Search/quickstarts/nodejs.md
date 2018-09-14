---
title: "Snelstartgids: Verzenden zoekquery's met de Bing afbeeldingen zoeken-API met behulp van Node.js"
titleSuffix: Azure Cognitive Services
description: Gebruik deze Quick Start om te zoeken en -installatiekopieën zoeken op Internet met behulp van de Bing webzoekopdrachten-API.
services: cognitive-services
documentationcenter: ''
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 8/20/2018
ms.author: aahi
ms.openlocfilehash: 43f0cfec6aa2d4263b6a627736c2a432b2943145
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45576643"
---
# <a name="quickstart-send-search-queries-using-the-bing-image-search-rest-api-and-nodejs"></a>Snelstartgids: Verzenden zoekquery's met behulp van de Bing afbeeldingen zoeken-REST-API en Node.js

Gebruik deze Quick Start voor het maken van de eerste aanroep van de Bing afbeeldingen zoeken-API en een JSON-antwoord ontvangen. Deze eenvoudige JavaScript-toepassing een zoekquery verzendt naar de API en de onbewerkte resultaten worden weergegeven.

Terwijl deze toepassing is geschreven in JavaScript en wordt uitgevoerd in Node.js, de API is een RESTful-Web-service compatibel is met de meeste moderne programmeertalen.

De broncode voor dit voorbeeld is beschikbaar [op GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/nodejs/Search/BingImageSearchv7Quickstart.js) met extra foutafhandeling en code-aantekeningen.

## <a name="prerequisites"></a>Vereisten

* De nieuwste versie van [Node.js](https://nodejs.org/en/download/).

* De [aanvraag van de JavaScript-bibliotheek](https://github.com/request/request)
[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Maken en initialiseren van de toepassing

1. Maak een nieuwe JavaScript-bestand in uw favoriete IDE of editor, en stel de vereisten voor strikte regels en https.

    ```javascript
    'use strict';
    let https = require('https');
    ```

2. Variabelen voor de API-eindpunt, zoekpad afbeeldings-API, uw abonnementssleutel maken en zoekterm.
    ```javascript
    let subscriptionKey = 'enter key here';
    let host = 'api.cognitive.microsoft.com';
    let path = '/bing/v7.0/images/search';
    let term = 'tropical ocean';
    ```

## <a name="construct-the-search-request-and-query"></a>De zoekopdracht en een query bouwen.

1. Gebruik de variabelen in de vorige stap om een URL zoeken voor de API-aanvraag. Houd er rekening mee dat uw zoekterm URL-gecodeerd moet zijn voordat het wordt verzonden naar de API.

    ```javascript
    let request_params = {
        method : 'GET',
        hostname : host,
        path : path + '?q=' + encodeURIComponent(search),
        headers : {
        'Ocp-Apim-Subscription-Key' : subscriptionKey,
        }
    };
    ```

2. De aanvraag-bibliotheek voor het verzenden van uw query aan de API gebruiken. `response_handler` in de volgende sectie worden gedefinieerd.
    ```javascript
    let req = https.request(request_params, response_handler);
    req.end();
    ```

## <a name="handle-and-parse-the-response"></a>Verwerken en parseren van het antwoord

1. een functie met de naam definiëren `response_handler` die een HTTP-aanroep duurt `response`, als een parameter. in deze functie kunt u de volgende stappen uitvoeren:
    
    1. Definieer een variabele bevatten de hoofdtekst van het JSON-antwoord.  
        ```javascript
        let response_handler = function (response) {
            let body = '';
        };
        ```

    2. De hoofdtekst van het antwoord Store wanneer de **gegevens** vlag wordt aangeroepen 
        ```javascript
        response.on('data', function (d) {
            body += d;
        });
        ```

    3. Wanneer een **end** vlag is heeft, de JSON kan worden verwerkt en URL voor de installatiekopie kan worden afgedrukt, samen met het totale aantal geretourneerde installatiekopieën.
    
        ```javascript
        response.on('end', function () {
            let firstImageResult = imageResults.value[0];
            console.log(`Image result count: ${imageResults.value.length}`);
            console.log(`First image thumbnail url: ${firstImageResult.thumbnailUrl}`);
            console.log(`First image web search url: ${firstImageResult.webSearchUrl}`);
         });
        ```

## <a name="json-response"></a>JSON-antwoord

Antwoorden van de Bing afbeeldingen zoeken-API worden geretourneerd als JSON. Dit voorbeeldantwoord is ingekort zodat één resultaat wordt weergegeven.

```json
{
"_type":"Images",
"instrumentation":{
    "_type":"ResponseInstrumentation"
},
"readLink":"images\/search?q=tropical ocean",
"webSearchUrl":"https:\/\/www.bing.com\/images\/search?q=tropical ocean&FORM=OIIARP",
"totalEstimatedMatches":842,
"nextOffset":47,
"value":[
    {
        "webSearchUrl":"https:\/\/www.bing.com\/images\/search?view=detailv2&FORM=OIIRPO&q=tropical+ocean&id=8607ACDACB243BDEA7E1EF78127DA931E680E3A5&simid=608027248313960152",
        "name":"My Life in the Ocean | The greatest WordPress.com site in ...",
        "thumbnailUrl":"https:\/\/tse3.mm.bing.net\/th?id=OIP.fmwSKKmKpmZtJiBDps1kLAHaEo&pid=Api",
        "datePublished":"2017-11-03T08:51:00.0000000Z",
        "contentUrl":"https:\/\/mylifeintheocean.files.wordpress.com\/2012\/11\/tropical-ocean-wallpaper-1920x12003.jpg",
        "hostPageUrl":"https:\/\/mylifeintheocean.wordpress.com\/",
        "contentSize":"897388 B",
        "encodingFormat":"jpeg",
        "hostPageDisplayUrl":"https:\/\/mylifeintheocean.wordpress.com",
        "width":1920,
        "height":1200,
        "thumbnail":{
        "width":474,
        "height":296
        },
        "imageInsightsToken":"ccid_fmwSKKmK*mid_8607ACDACB243BDEA7E1EF78127DA931E680E3A5*simid_608027248313960152*thid_OIP.fmwSKKmKpmZtJiBDps1kLAHaEo",
        "insightsMetadata":{
        "recipeSourcesCount":0,
        "bestRepresentativeQuery":{
            "text":"Tropical Beaches Desktop Wallpaper",
            "displayText":"Tropical Beaches Desktop Wallpaper",
            "webSearchUrl":"https:\/\/www.bing.com\/images\/search?q=Tropical+Beaches+Desktop+Wallpaper&id=8607ACDACB243BDEA7E1EF78127DA931E680E3A5&FORM=IDBQDM"
        },
        "pagesIncludingCount":115,
        "availableSizesCount":44
        },
        "imageId":"8607ACDACB243BDEA7E1EF78127DA931E680E3A5",
        "accentColor":"0050B2"
    }
}
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zelfstudie voor Bing afbeeldingen zoeken-app met één pagina](../tutorial-bing-image-search-single-page-app.md)

## <a name="see-also"></a>Zie ook 

* [Wat is de Bing afbeeldingen zoeken?](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/overview)  
* [Probeer een online interactieve demo](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
* [Een gratis Cognitive Services-toegangssleutel ophalen](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api)  
* [Documentatie voor Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services)
* [Bing afbeeldingen zoeken-API-verwijzing](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)