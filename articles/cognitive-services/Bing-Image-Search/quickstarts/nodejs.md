---
title: "Snelstartgids: Verzenden zoekquery's met de REST-API voor de Bing afbeeldingen zoeken-API met behulp van Node.js"
description: In deze Quick Start stuurt u zoekopdrachten naar de Bing webzoekopdrachten-API om een lijst met relevante afbeeldingen met behulp van Node.js.
services: cognitive-services
documentationcenter: ''
author: v-jerkin
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 9/21/2017
ms.author: v-jerkin
ms.openlocfilehash: 975275bea61a5903c06da394b762b1aceb18023f
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2018
ms.locfileid: "41987653"
---
# <a name="quickstart-send-search-queries-using-the-rest-api-and-nodejs"></a>Snelstartgids: Verzenden zoekquery's met behulp van de REST-API en Node.js

De Bing afbeeldingen zoeken-API biedt een ervaring die vergelijkbaar is met Bing.com/Images doordat u kunt een zoekquery voor de gebruiker naar Bing te verzenden en een lijst met relevante afbeeldingen teruggaan.

Dit artikel bevat een eenvoudige consoletoepassing die een Bing afbeeldingen zoeken-API-query en de zoekresultaten geretourneerde onbewerkte, die zich in de JSON-indeling worden weergegeven. Hoewel deze toepassing is geschreven in JavaScript en wordt uitgevoerd onder Node.js, de API is een RESTful-Web-compatibel is met elke programmeertaal die HTTP-aanvragen te parseren van JSON. 

## <a name="prerequisites"></a>Vereisten

U moet [Node.js 6](https://nodejs.org/en/download/) deze code uit te voeren.

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="running-the-application"></a>De toepassing uitvoeren

Volg deze stappen om uit te voeren deze toepassing.

1. Maak een nieuwe Node.js-project in uw favoriete IDE of editor.
2. De opgegeven code toevoegen.
3. Vervang de `subscriptionKey` waarde met een geldige toegangssleutel voor uw abonnement.
4. Voer het programma.

```javascript
'use strict';

let https = require('https');

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace the subscriptionKey string value with your valid subscription key.
let subscriptionKey = 'enter key here';

// Verify the endpoint URI.  At this writing, only one endpoint is used for Bing
// search APIs.  In the future, regional endpoints may be available.  If you
// encounter unexpected authorization errors, double-check this host against
// the endpoint for your Bing Search instance in your Azure dashboard.
let host = 'api.cognitive.microsoft.com';
let path = '/bing/v7.0/images/search';

let term = 'puppies';

let response_handler = function (response) {
    let body = '';
    response.on('data', function (d) {
        body += d;
    });
    response.on('end', function () {
        console.log('\nRelevant Headers:\n');
        for (var header in response.headers)
            // header keys are lower-cased by Node.js
            if (header.startsWith("bingapis-") || header.startsWith("x-msedge-"))
                 console.log(header + ": " + response.headers[header]);
        body = JSON.stringify(JSON.parse(body), null, '  ');
        console.log('\nJSON Response:\n');
        console.log(body);
    });
    response.on('error', function (e) {
        console.log('Error: ' + e.message);
    });
};

let bing_image_search = function (search) {
  console.log('Searching images for: ' + term);
  let request_params = {
        method : 'GET',
        hostname : host,
        path : path + '?q=' + encodeURIComponent(search),
        headers : {
            'Ocp-Apim-Subscription-Key' : subscriptionKey,
        }
    };

    let req = https.request(request_params, response_handler);
    req.end();
}

if (subscriptionKey.length === 32) {
    bing_image_search(term);
} else {
    console.log('Invalid Bing Search API subscription key!');
    console.log('Please paste yours into the source code.');
}
```

## <a name="json-response"></a>JSON-antwoord

Er volgt een voorbeeldantwoord. Als u wilt beperken de lengte van de JSON, slechts één resultaat wordt weergegeven en andere onderdelen van het antwoord is afgekapt. 

```json
{
  "_type": "Images",
  "instrumentation": {},
  "readLink": "https://api.cognitive.microsoft.com/api/v7/images/search?q=puppies",
  "webSearchUrl": "https://www.bing.com/images/search?q=puppies&FORM=OIIARP",
  "totalEstimatedMatches": 955,
  "nextOffset": 1,
  "value": [
    {
      "webSearchUrl": "https://www.bing.com/images/search?view=detailv...",
      "name": "So cute - Puppies Wallpaper",
      "thumbnailUrl": "https://tse3.mm.bing.net/th?id=OIP.jHrihoDNkXGS1t...",
      "datePublished": "2014-02-01T21:55:00.0000000Z",
      "contentUrl": "http://images4.contoso.com/image/photos/14700000/So-cute-puppies...",
      "hostPageUrl": "http://www.contoso.com/clubs/puppies/images/14749028/...",
      "contentSize": "394455 B",
      "encodingFormat": "jpeg",
      "hostPageDisplayUrl": "www.contoso.com/clubs/puppies/images/14749...",
      "width": 1600,
      "height": 1200,
      "thumbnail": {
        "width": 300,
        "height": 225
      },
      "imageInsightsToken": "ccid_jHrihoDN*mid_F68CC526226E163FD1EA659747AD...",
      "insightsMetadata": {
        "recipeSourcesCount": 0
      },
      "imageId": "F68CC526226E163FD1EA659747ADCB8F9FA36",
      "accentColor": "8D613E"
    }
  ],
  "queryExpansions": [
    {
      "text": "Shih Tzu Puppies",
      "displayText": "Shih Tzu",
      "webSearchUrl": "https://www.bing.com/images/search?q=Shih+Tzu+Puppies...",
      "searchLink": "https://api.cognitive.microsoft.com/api/v7/images/search?q=Shih...",
      "thumbnail": {
        "thumbnailUrl": "https://tse2.mm.bing.net/th?q=Shih+Tzu+Puppies&pid=Api..."
      }
    }
  ],
  "pivotSuggestions": [
    {
      "pivot": "puppies",
      "suggestions": [
        {
          "text": "Dog",
          "displayText": "Dog",
          "webSearchUrl": "https://www.bing.com/images/search?q=Dog&tq=%7b%22pq%...",
          "searchLink": "https://api.cognitive.microsoft.com/api/v7/images/search?q=Dog...",
          "thumbnail": {
            "thumbnailUrl": "https://tse1.mm.bing.net/th?q=Dog&pid=Api&mkt=en-US..."
          }
        }
      ]
    }
  ],
  "similarTerms": [
    {
      "text": "cute",
      "displayText": "cute",
      "webSearchUrl": "https://www.bing.com/images/search?q=cute&FORM=...",
      "thumbnail": {
        "url": "https://tse2.mm.bing.net/th?q=cute&pid=Api&mkt=en-US..."
      }
    }
  ],
  "relatedSearches": [
    {
      "text": "Cute Puppies",
      "displayText": "Cute Puppies",
      "webSearchUrl": "https://www.bing.com/images/search?q=Cute+Puppies",
      "searchLink": "https://api.cognitive.microsoft.com/api/v7/images/sear...",
      "thumbnail": {
        "thumbnailUrl": "https://tse4.mm.bing.net/th?q=Cute+Puppies&pid=..."
      }
    }
  ]
}
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zelfstudie voor Bing afbeeldingen zoeken-app met één pagina](../tutorial-bing-image-search-single-page-app.md)

## <a name="see-also"></a>Zie ook 

[Overzicht van de Bing afbeeldingen zoeken](../overview.md)  
[Nu uitproberen](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
[Een gratis proefversie toegangssleutel ophalen](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api)  
[Bing afbeeldingen zoeken-API-verwijzing](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)
