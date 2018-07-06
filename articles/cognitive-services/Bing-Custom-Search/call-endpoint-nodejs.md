---
title: Eindpunt aanroepen met behulp van Node.js - Bing Custom Search - Microsoft Cognitive Services
description: Deze quickstart laat zien hoe zoekresultaten van uw exemplaar voor aangepast zoeken met behulp van Node.js om aan te roepen van het eindpunt van de Bing Custom Search aanvraagt.
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 05/07/2018
ms.author: v-brapel
ms.openlocfilehash: 5d9391cc486dc868a1a291ccc7095291cddd3e4c
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37858456"
---
# <a name="call-bing-custom-search-endpoint-nodejs"></a>Aanroep van Bing Custom Search-eindpunt (Node.js)

Deze quickstart laat zien hoe zoekresultaten van uw exemplaar voor aangepast zoeken met behulp van Node.js om aan te roepen van het eindpunt van de Bing Custom Search aanvraagt. 

## <a name="prerequisites"></a>Vereisten
U hebt het volgende nodig om deze quickstart te voltooien:

- Een exemplaar voor aangepast zoeken. Zie [maken van uw eerste exemplaar van de Bing Custom Search](quick-start.md).

- [Node.js](https://www.nodejs.org/) geïnstalleerd.

-  [Account voor cognitive Services API](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met **Bing zoeken-API's**. De [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search) is voldoende voor deze Quick Start. Moet u de toegangssleutel die is opgegeven wanneer u uw gratis proefversie activeren, of u de sleutel van een betaald abonnement van uw Azure-dashboard kunt.

## <a name="run-the-code"></a>De code uitvoeren

Volg deze stappen voor het aanroepen van de Bing Custom Search-eindpunt:

1. Maak een map voor uw code.

2. Navigeer naar de map die u zojuist hebt gemaakt vanaf een opdrachtprompt of terminal.

3. Installeer de **aanvraag** knooppunt module:
    <pre>
    npm install request
    </pre>
    
4. Maak het bestand BingCustomSearch.js en kopieer de volgende code toe.

5. Vervang **uw-SUBSCRIPTION-KEY** en **uw-aangepaste-CONFIG-ID** door uw sleutel en -configuratie-ID (Zie stap 1).

    ``` javascript
    var request = require("request");
    
    var subscriptionKey = 'YOUR-SUBSCRIPTION-KEY';
    var customConfigId = 'YOUR-CUSTOM-CONFIG-ID';
    var searchTerm = 'microsoft';
    
    var options = {
        url: 'https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?' + 
          'q=' + searchTerm + 
          '&customconfig=' + customConfigId,
        headers: {
            'Ocp-Apim-Subscription-Key' : subscriptionKey
        }
    }
    
    request(options, function(error, response, body){
        var searchResponse = JSON.parse(body);
        for(var i = 0; i < searchResponse.webPages.value.length; ++i){
            var webPage = searchResponse.webPages.value[i];
            console.log('name: ' + webPage.name);
            console.log('url: ' + webPage.url);
            console.log('displayUrl: ' + webPage.displayUrl);
            console.log('snippet: ' + webPage.snippet);
            console.log('dateLastCrawled: ' + webPage.dateLastCrawled);
            console.log();
        }
    })
    ```
6. Voer de code met de volgende opdracht uit.
    ```    
    node BingCustomSearch.js
   ``` 

## <a name="next-steps"></a>Volgende stappen
- [Configureren van uw gehoste gebruikersinterface-ervaring](./hosted-ui.md)
- [Decoration markeringen gebruiken om te markeren van tekst](./hit-highlighting.md)
- [Pagina webpagina 's](./page-webpages.md)
