---
title: Node.js-Quick Start voor het voorbeeld in de Project-URL - cognitieve Microsoft-Services | Microsoft Docs
description: Aan de slag met de Preview-URL in cognitieve Microsoft-Services in Azure.
services: cognitive-services
author: mikedodaro
ms.service: cognitive-services
ms.technology: project-url-preview
ms.topic: article
ms.date: 03/16/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 195033d2740b11873baae095cec028dc8d19ce49
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345284"
---
# <a name="url-preview-node-quickstart"></a>Quick Start-URL Preview knooppunt

Het volgende knooppunt voorbeeld wordt een voorbeeld-Url voor de website van SwiftKey: https://swiftkey.com/en.

## <a name="prerequisites"></a>Vereisten

Ophalen van een toegangssleutel voor de gratis proefversie [cognitieve Services Labs](https://aka.ms/answersearchsubscription)

## <a name="code-scenario"></a>Code-scenario 

De volgende code haalt gegevens bekijken van de URL.
Deze is geïmplementeerd in de volgende stappen uit:
1. Declareer de variabelen opgeven van het eindpunt van de host en pad.
2. Geef de URL van de query voor de preview en voeg de queryparameter.  
3. Maak een handlerfunctie voor het antwoord.
4. Definieer de zoekfunctie die de aanvraag wordt gemaakt en wordt de *Ocp-Apim-Subscription-Key* header.
5. Voer de zoekfunctie. 

De volledige code voor deze demo volgt:

````
'use strict';

let https = require('https');

// Replace the subscriptionKey string value with your valid subscription key.
let subscriptionKey = 'YOUR-ACCESS-KEY'; 
let host = 'api.labs.cognitive.microsoft.com';
let path = '/urlpreview/v7.0/search';

let mkt = 'en-US';
let q = 'https://swiftkey.com/';

let params = '?q=' + encodeURI(q);

let response_handler = function (response) {
    let body = '';
    response.on('data', function (d) {
        body += d;
    });
    response.on('end', function () {
        let body_ = JSON.parse(body);
        let body__ = JSON.stringify(body_, null, '  ');
        console.log(body__);
    });
    response.on('error', function (e) {
        console.log('Error: ' + e.message);
    });
};

let Search = function () {
    let request_params = {
        method: 'GET',
        hostname: host,
        path: path + params,
        headers: {
            'Ocp-Apim-Subscription-Key': subscriptionKey,
        }
    };

    let req = https.request(request_params, response_handler);
    req.end();
}

Search();

````

## <a name="next-steps"></a>Volgende stappen
- [C#-voorbeeldcode](csharp.md)
- [Java-Quick Start](java-quickstart.md)
- [JavaScript Quick Start](javascript.md)
- [Snelstartgids voor Python](python-quickstart.md)