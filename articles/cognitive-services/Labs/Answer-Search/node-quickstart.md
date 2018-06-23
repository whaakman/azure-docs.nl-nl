---
title: Knooppunt Quick Start voor Microsoft cognitieve Services, Project antwoord zoeken | Microsoft Docs
description: Aan de slag met zoeken in de Project-antwoord, cognitieve Microsoft-Services in Azure.
services: cognitive-services
author: mikedodaro
ms.service: cognitive-services
ms.technology: project-answer-search
ms.topic: article
ms.date: 04/13/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 36b2709d39230aae7929164ba4c9306f57043b43
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345271"
---
# <a name="project-answer-search-node-quickstart"></a>Project antwoord zoeken knooppunt Quick Start

Het volgende voorbeeld wordt een query voor informatie over Yosemite National Park.

## <a name="prerequisites"></a>Vereisten

Ophalen van een toegangssleutel voor de gratis proefversie [cognitieve Services Labs](https://aka.ms/answersearchsubscription)

In dit voorbeeld wordt een knooppunt v8.9.4

## <a name="code-scenario"></a>Code-scenario 

De volgende code haalt antwoorden.
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
let subscriptionKey = 'YOUR-SUBSCRIPTION-KEY'; 

let host = 'api.labs.cognitive.microsoft.com';
let path = '/answerSearch/v7.0/search';

let mkt = 'en-us';
let q = 'Yosemite National Park';

let params = '?q=' + encodeURI(q) + '&mkt=en-us';

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
- [C#-voorbeeldcode](c-sharp-quickstart.md)
- [Java-Quick Start](java-quickstart.md)
- [Snelstartgids voor Python](python-quickstart.md)