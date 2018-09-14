---
title: 'Snelstart: Node.js gebruiken voor het aanroepen van de Bing Webzoekopdrachten-API'
description: In deze snelstart leert u hoe u voor het eerst de Bing Webzoekopdrachten-API aanroept met Node.js en een JSON-antwoord ontvangt.
services: cognitive-services
author: erhopf
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: quickstart
ms.date: 8/16/2018
ms.author: erhopf
ms.openlocfilehash: 7a46500f7cbf319c788761bccfaa92197ef67490
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "42886928"
---
# <a name="quickstart-use-nodejs-to-call-the-bing-web-search-api"></a>Snelstart: Node.js gebruiken voor het aanroepen van de Bing Webzoekopdrachten-API  

Gebruik deze snelstart om voor het eerst de Bing Webzoekopdrachten-API aan te roepen en binnen tien minuten een JSON-antwoord te ontvangen.  

[!INCLUDE [bing-web-search-quickstart-signup](../../../../includes/bing-web-search-quickstart-signup.md)]

## <a name="prerequisites"></a>Vereisten

Voordat u verdergaat met deze snelstart moet u beschikken over:

* [Node.js 6](https://nodejs.org/en/download/) of later
* Een abonnementssleutel  

## <a name="create-a-project-and-declare-required-modules"></a>Een project maken en de vereiste modules declareren

Maak een nieuw Node.js-project in uw favoriete IDE of editor. Kopieer vervolgens het codefragment hieronder in het project. In deze snelstart wordt de strict-modus gebruikt en is vereist dat gegevens worden verzonden en ontvangen via de module `https`.

```javascript
// Use strict mode.
'use strict';

// Require the https module.
let https = require('https');
```

## <a name="define-variables"></a>Variabelen definiëren

Er moet een aantal variabelen worden ingesteld voordat we verder kunnen gaan. Controleer of `host` en `path` geldig zijn en vervang de waarde `subscriptionKey` door een geldige abonnementssleutel uit uw Azure-account. U kunt de zoekquery gerust aanpassen door de waarde voor `term` te vervangen.

```javascript
// Replace with a valid subscription key.
let subscriptionKey = 'enter key here';

/*
 * Verify the endpoint URI. If you
 * encounter unexpected authorization errors, double-check this host against
 * the endpoint for your Bing Web search instance in your Azure dashboard.  
 */
let host = 'api.cognitive.microsoft.com';
let path = '/bing/v7.0/search';
let term = 'Microsoft Cognitive Services';

// Validate the subscription key.
if (subscriptionKey.length === 32) {
    bing_web_search(term);
} else {
    console.log('Invalid Bing Search API subscription key!');
    console.log('Please paste yours into the source code.');
}
```

## <a name="create-a-response-handler"></a>Een antwoordhandler maken

Maak een handler om een tekenreeks te maken van het antwoord en dit te parseren. De `response_handler` wordt aangeroepen telkens wanneer een aanvraag wordt gemaakt voor de Bing Webzoekopdrachten-API. Dit ziet u in de volgende sectie.

```javascript
let response_handler = function (response) {
    let body = '';
    response.on('data', function (d) {
        body += d;
    });
    response.on('end', function () {
        console.log('\nRelevant Headers:\n');
        for (var header in response.headers)
            // Headers are lowercased by Node.js.
            if (header.startsWith("bingapis-") || header.startsWith("x-msedge-"))
                 console.log(header + ": " + response.headers[header]);
        // Stringify and parse the response body.
        body = JSON.stringify(JSON.parse(body), null, '  ');
        console.log('\nJSON Response:\n');
        console.log(body);
    });
    response.on('error', function (e) {
        console.log('Error: ' + e.message);
    });
};
```

## <a name="make-a-request-and-print-the-response"></a>Een aanvraag maken en het antwoord weergeven

Stel de aanvraag samen en doe een aanroep naar de Bing Webzoekopdrachten-API. Nadat de aanvraag is gemaakt, wordt de functie `response_handler` aangeroepen en het antwoord afgedrukt.

```javascript
let bing_web_search = function (search) {
    console.log('Searching the Web for: ' + term);
        // Declare the method, hostname, path, and headers.
        let request_params = {
            method : 'GET',
            hostname : host,
            path : path + '?q=' + encodeURIComponent(search),
            headers : {
                'Ocp-Apim-Subscription-Key' : subscriptionKey,
            }
        };
    // Request to the Bing Web Search API.
    let req = https.request(request_params, response_handler);
    req.end();
}
```

## <a name="put-it-all-together"></a>Alles samenvoegen

De laatste stap is het uitvoeren van uw code. Als u uw code wilt vergelijken met de onze, is er [voorbeeldcode beschikbaar op GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/nodejs/Search/BingWebSearchv7.js).

## <a name="sample-response"></a>Voorbeeldantwoord

Antwoorden afkomstig van de Bing Webzoekopdrachten-API worden geretourneerd in de JSON-indeling. Dit voorbeeldantwoord is ingekort zodat één resultaat wordt weergegeven.  

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
> [Zelfstudie voor app met één pagina voor Bing Web Search](../tutorial-bing-web-search-single-page-app.md)

[!INCLUDE [bing-web-search-quickstart-see-also](../../../../includes/bing-web-search-quickstart-see-also.md)]
