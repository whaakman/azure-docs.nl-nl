---
title: Oproep- en antwoord - Node.js Quick Start voor Azure cognitieve Services, zoekopdracht Bing-Web-API | Microsoft Docs
description: Get-informatie en codevoorbeelden om u te helpen snel aan de slag met de Bing Web Search-API in Microsoft cognitieve Services in Azure.
services: cognitive-services
documentationcenter: ''
author: v-jerkin
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 9/18/2017
ms.author: v-jerkin
ms.openlocfilehash: a47dfaa48acb5b4a8ffc9b9f8da98f42e7729399
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344749"
---
# <a name="call-and-response-your-first-bing-web-search-query-for-nodejs"></a>Oproep- en -antwoord: uw eerste Bing Web query voor Node.js

De Bing Web zoeken-API biedt een vergelijkbaar met Bing.com/Search ervaring door te retourneren zoekresultaten die Bing bepaalt relevant zijn voor de query van de gebruiker. De resultaten kunnen webpagina's, afbeeldingen, video's, nieuws en entiteiten, samen met verwante zoekquery's, correcties tijdzones, eenheidconversie, vertalingen en berekeningen bevatten. Het soort u resultaten zijn gebaseerd op hun relevantie en de laag van de Bing zoeken-API's waarop u bent geabonneerd.

Dit artikel bevat een eenvoudige consoletoepassing die voert een query zoeken Bing-Web-API en de geretourneerde onbewerkte zoekresultaten in het JSON-indeling weergegeven. Terwijl deze toepassing is geschreven in JavaScript en wordt uitgevoerd onder Node.js, is de API een compatibel is met elke programmeertaal die kunt maken van HTTP-aanvragen en parseren van JSON RESTful-Web-service. 

## <a name="prerequisites"></a>Vereisten

U moet [Node.js 6](https://nodejs.org/en/download/) deze code uit te voeren.

U moet hebben een [cognitieve Services API account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met **Bing zoeken-API's**. De [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) voldoende is voor deze snelstartgids. U moet de toegangssleutel die is opgegeven bij het activeren van uw gratis proefversie of u kunt de sleutel van een betaald abonnement van uw Azure-dashboard.

## <a name="running-the-application"></a>De toepassing uitvoeren

Volg deze stappen voor het uitvoeren van deze toepassing.

1. Maak een nieuwe Node.js-project in uw favoriete IDE of -editor.
2. Voeg de opgegeven code.
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
// the endpoint for your Bing Web search instance in your Azure dashboard.
let host = 'api.cognitive.microsoft.com';
let path = '/bing/v7.0/search';

let term = 'Microsoft Cognitive Services';

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

let bing_web_search = function (search) {
  console.log('Searching the Web for: ' + term);
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
    bing_web_search(term);
} else {
    console.log('Invalid Bing Search API subscription key!');
    console.log('Please paste yours into the source code.');
}

```

## <a name="json-response"></a>JSON-antwoord

Er volgt een voorbeeldantwoord. Als u wilt beperken de lengte van de JSON, alleen een enkelvoudig resultaat wordt verkregen wordt weergegeven en andere onderdelen van het antwoord is afgekapt. 

```json
{
  "_type": "SearchResponse",
  "queryContext": {
    "originalQuery": "Microsoft Cognitive Services"
  },
  "webPages": {
    "webSearchUrl": "https://www.bing.com/search?q=Microsoft+cognitive+services",
    "totalEstimatedMatches": 22300000,
    "value": [
      {
        "id": "https://api.cognitive.microsoft.com/api/v7/#WebPages.0",
        "name": "Microsoft Cognitive Services",
        "url": "https://www.microsoft.com/cognitive-services",
        "displayUrl": "https://www.microsoft.com/cognitive-services",
        "snippet": "Knock down barriers between you and your ideas. Enable natural and contextual interaction with tools that augment users' experiences via the power of machine-based AI. Plug them in and bring your ideas to life.",
        "deepLinks": [
          {
            "name": "Face API",
            "url": "https://azure.microsoft.com/services/cognitive-services/face/",
            "snippet": "Add facial recognition to your applications to detect, identify, and verify faces using a Face API from Microsoft Azure. ... Cognitive Services; Face API;"
          },
          {
            "name": "Text Analytics",
            "url": "https://azure.microsoft.com/services/cognitive-services/text-analytics/",
            "snippet": "Cognitive Services; Text Analytics API; Text Analytics API . Detect sentiment, ... you agree that Microsoft may store it and use it to improve Microsoft services, ..."
          },
          {
            "name": "Computer Vision API",
            "url": "https://azure.microsoft.com/services/cognitive-services/computer-vision/",
            "snippet": "Extract the data you need from images using optical character recognition and image analytics with Computer Vision APIs from Microsoft Azure."
          },
          {
            "name": "Emotion",
            "url": "https://www.microsoft.com/cognitive-services/en-us/emotion-api",
            "snippet": "Cognitive Services Emotion API - microsoft.com"
          },
          {
            "name": "Bing Speech API",
            "url": "https://azure.microsoft.com/services/cognitive-services/speech/",
            "snippet": "Add speech recognition to your applications, including text to speech, with a speech API from Microsoft Azure. ... Cognitive Services; Bing Speech API;"
          },
          {
            "name": "Get Started for Free",
            "url": "https://azure.microsoft.com/services/cognitive-services/",
            "snippet": "Add vision, speech, language, and knowledge capabilities to your applications using intelligence APIs and SDKs from Cognitive Services."
          }
        ]
      }
    ]
  },
  "relatedSearches": {
    "id": "https://api.cognitive.microsoft.com/api/v7/#RelatedSearches",
    "value": [
      {
        "text": "microsoft bot framework",
        "displayText": "microsoft bot framework",
        "webSearchUrl": "https://www.bing.com/search?q=microsoft+bot+framework"
      },
      {
        "text": "microsoft cognitive services youtube",
        "displayText": "microsoft cognitive services youtube",
        "webSearchUrl": "https://www.bing.com/search?q=microsoft+cognitive+services+youtube"
      },
      {
        "text": "microsoft cognitive services search api",
        "displayText": "microsoft cognitive services search api",
        "webSearchUrl": "https://www.bing.com/search?q=microsoft+cognitive+services+search+api"
      },
      {
        "text": "microsoft cognitive services news",
        "displayText": "microsoft cognitive services news",
        "webSearchUrl": "https://www.bing.com/search?q=microsoft+cognitive+services+news"
      },
      {
        "text": "ms cognitive service",
        "displayText": "ms cognitive service",
        "webSearchUrl": "https://www.bing.com/search?q=ms+cognitive+service"
      },
      {
        "text": "microsoft cognitive services text analytics",
        "displayText": "microsoft cognitive services text analytics",
        "webSearchUrl": "https://www.bing.com/search?q=microsoft+cognitive+services+text+analytics"
      },
      {
        "text": "microsoft cognitive services toolkit",
        "displayText": "microsoft cognitive services toolkit",
        "webSearchUrl": "https://www.bing.com/search?q=microsoft+cognitive+services+toolkit"
      },
      {
        "text": "microsoft cognitive services api",
        "displayText": "microsoft cognitive services api",
        "webSearchUrl": "https://www.bing.com/search?q=microsoft+cognitive+services+api"
      }
    ]
  },
  "rankingResponse": {
    "mainline": {
      "items": [
        {
          "answerType": "WebPages",
          "resultIndex": 0,
          "value": {
            "id": "https://api.cognitive.microsoft.com/api/v7/#WebPages.0"
          }
        }
      ]
    },
    "sidebar": {
      "items": [
        {
          "answerType": "RelatedSearches",
          "value": {
            "id": "https://api.cognitive.microsoft.com/api/v7/#RelatedSearches"
          }
        }
      ]
    }
  }
}
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Bing Web search één pagina app-zelfstudie](../tutorial-bing-web-search-single-page-app.md)

## <a name="see-also"></a>Zie ook 

[Overzicht van Bing webpagina's zoeken](../overview.md)  
[Probeer deze](https://azure.microsoft.com/services/cognitive-services/bing-web-search-api/)  
[Een gratis proefversie toegangssleutel ophalen](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api)  
[Bing Web Search API-verwijzingen](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference)
