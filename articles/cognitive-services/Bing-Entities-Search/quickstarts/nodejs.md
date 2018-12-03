---
title: 'Snelstartgids: Bing Entiteiten zoeken-API, Node.js'
titlesuffix: Azure Cognitive Services
description: Bekijk informatie en codevoorbeelden om snel aan de slag te gaan met de Bing Entiteiten zoeken-API.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: quickstart
ms.date: 11/28/2017
ms.author: aahi
ms.openlocfilehash: 80af035d3d3f4c530e417669c2fa9acad39eb355
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2018
ms.locfileid: "52315984"
---
# <a name="quickstart-for-bing-entity-search-api-with-nodejs"></a>Snelstartgids voor Bing Entiteiten zoeken-API met Node.js

In dit artikel leest u hoe u de [Bing Entiteiten zoeken](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/search-the-web) -API gebruikt met Node.JS.

## <a name="prerequisites"></a>Vereisten

U hebt [Node.js 6](https://nodejs.org/en/download/) nodig om deze code uit te voeren.

U moet beschikken over een [account voor de Cognitive Services-API](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met de **Bing Entiteiten zoeken-API**. De [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/?api=bing-entity-search-api) is voldoende voor deze snelstartgids. U hebt de toegangssleutel nodig die wordt verstrekt bij het activeren van uw gratis proefversie of u gebruikt de sleutel van een betaald abonnement vanuit uw Azure-dashboard.  Zie ook [Prijsinformatie Cognitive Services - Bing Zoeken-API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="search-entities"></a>Entiteiten zoeken

Volg deze stappen om deze toepassing uit te voeren.

1. Maak een nieuw Node.js-project in uw favoriete IDE.
2. Voeg de onderstaande code toe.
3. Vervang de waarde `key` door een geldige toegangssleutel voor uw abonnement.
4. Voer het programma uit.

```nodejs
'use strict';

let https = require ('https');

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace the subscriptionKey string value with your valid subscription key.
let subscriptionKey = 'ENTER KEY HERE';

let host = 'api.cognitive.microsoft.com';
let path = '/bing/v7.0/entities';

let mkt = 'en-US';
let q = 'italian restaurant near me';

let params = '?mkt=' + mkt + '&q=' + encodeURI(q);

let response_handler = function (response) {
    let body = '';
    response.on ('data', function (d) {
        body += d;
    });
    response.on ('end', function () {
        let body_ = JSON.parse (body);
        let body__ = JSON.stringify (body_, null, '  ');
        console.log (body__);
    });
    response.on ('error', function (e) {
        console.log ('Error: ' + e.message);
    });
};

let Search = function () {
    let request_params = {
        method : 'GET',
        hostname : host,
        path : path + params,
        headers : {
            'Ocp-Apim-Subscription-Key' : subscriptionKey,
        }
    };

    let req = https.request (request_params, response_handler);
    req.end ();
}

Search ();
```

**Antwoord**

Een geslaagd antwoord wordt geretourneerd in de JSON-indeling, zoals u kunt zien in het volgende voorbeeld: 

```json
{
  "_type": "SearchResponse",
  "queryContext": {
    "originalQuery": "italian restaurant near me",
    "askUserForLocation": true
  },
  "places": {
    "value": [
      {
        "_type": "LocalBusiness",
        "webSearchUrl": "https://www.bing.com/search?q=sinful+bakery&filters=local...",
        "name": "Liberty's Delightful Sinful Bakery & Cafe",
        "url": "https://www.contoso.com/",
        "entityPresentationInfo": {
          "entityScenario": "ListItem",
          "entityTypeHints": [
            "Place",
            "LocalBusiness"
          ]
        },
        "address": {
          "addressLocality": "Seattle",
          "addressRegion": "WA",
          "postalCode": "98112",
          "addressCountry": "US",
          "neighborhood": "Madison Park"
        },
        "telephone": "(800) 555-1212"
      },

      . . .
      {
        "_type": "Restaurant",
        "webSearchUrl": "https://www.bing.com/search?q=Pickles+and+Preserves...",
        "name": "Munson's Pickles and Preserves Farm",
        "url": "http://www.princi.com/",
        "entityPresentationInfo": {
          "entityScenario": "ListItem",
          "entityTypeHints": [
            "Place",
            "LocalBusiness",
            "Restaurant"
          ]
        },
        "address": {
          "addressLocality": "Seattle",
          "addressRegion": "WA",
          "postalCode": "98101",
          "addressCountry": "US",
          "neighborhood": "Capitol Hill"
        },
        "telephone": "(800) 555-1212"
      },
      
      . . .
    ]
  }
}
```

[Terug naar boven](#HOLTop)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zelfstudie over Bing Entiteiten zoeken](../tutorial-bing-entities-search-single-page-app.md)
> [Overzicht van Bing Entiteiten zoeken](../search-the-web.md )
> [API-handleiding](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference)
