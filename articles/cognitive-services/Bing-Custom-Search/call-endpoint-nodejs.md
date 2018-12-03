---
title: 'Quickstart: Eindpunt aanroepen met behulp van Node.js - Bing Custom Search'
titlesuffix: Azure Cognitive Services
description: Deze quickstart laat zien hoe u zoekresultaten kunt opvragen bij uw exemplaar voor aangepaste zoekopdrachten door met behulp van Node.js het eindpunt van Bing Custom Search aan te roepen.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: quickstart
ms.date: 05/07/2018
ms.author: aahi
ms.openlocfilehash: c0c97dd52f8fc3ff590c86f32f794beeb00f4b05
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2018
ms.locfileid: "52310249"
---
# <a name="quickstart-call-bing-custom-search-endpoint-nodejs"></a>Quickstart: Eindpunt van Bing Custom Search aanroepen (Node.js)

Deze quickstart laat zien hoe u zoekresultaten kunt opvragen bij uw exemplaar voor aangepaste zoekopdrachten door met behulp van Node.js het eindpunt van Bing Custom Search aan te roepen. 

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om deze quickstart te voltooien:

- Een exemplaar voor aangepaste zoekopdrachten dat klaar is voor gebruik. Zie [Uw eerste Bing Aangepaste zoekopdrachten-exemplaar maken](quick-start.md) voor meer informatie.
- [Node.js](https://www.nodejs.org/) geïnstalleerd.
- Een abonnementssleutel. U kunt een abonnementssleutel opvragen wanneer u uw [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search) activeert, of u kunt een sleutel voor een betaald abonnement gebruiken uit uw Azure-dashboard (zie [Snelstartgids: Een Cognitive Services-account maken in de Azure-portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)).   Zie ook [Prijsinformatie Cognitive Services - Bing Zoeken-API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="run-the-code"></a>De code uitvoeren

Volg deze stappen om het voorbeeld uit te voeren:

1. Maak een map voor uw code.  
  
2. Navigeer vanuit een opdrachtprompt of terminal naar de map die u zojuist hebt gemaakt.  
  
3. Installeer de Node-module **request**:
    <pre>
    npm install request
    </pre>  
    
4. Maak een bestand met de naam BingCustomSearch.js in de map die u hebt gemaakt en kopieer de volgende code naar het bestand. Vervang **YOUR-SUBSCRIPTION-KEY** en **YOUR-CUSTOM-CONFIG-ID** door uw abonnementssleutel en configuratie-id.  
  
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
  
6. Voer de code uit met behulp van de volgende opdracht:  
  
    ```    
    node BingCustomSearch.js
    ``` 

## <a name="next-steps"></a>Volgende stappen
- [Gehoste UI-ervaring configureren](./hosted-ui.md)
- [Decoratiemarkeringen gebruiken om tekst te markeren](./hit-highlighting.md)
- [Bladeren door webpagina's](./page-webpages.md)
