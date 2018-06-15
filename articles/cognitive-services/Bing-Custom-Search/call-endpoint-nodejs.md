---
title: Oproep-eindpunt met cognitieve Node.js - Bing aangepaste zoekactie - Microsoft-Services
description: Deze snelstartgids laat zien hoe zoekresultaten aanvragen bij uw aangepaste zoekactie-exemplaar met behulp van Node.js aan te roepen van het eindpunt van de aangepaste Bing-zoekactie.
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 05/07/2018
ms.author: v-brapel
ms.openlocfilehash: 48fc234e15ce3b9172d766f6fae11b51a017ce70
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "35345857"
---
# <a name="call-bing-custom-search-endpoint-nodejs"></a>Aanroep Bing aangepaste zoekactie eindpunt (Node.js)

Deze snelstartgids laat zien hoe zoekresultaten aanvragen bij uw aangepaste zoekactie-exemplaar met behulp van Node.js aan te roepen van het eindpunt van de aangepaste Bing-zoekactie. 

## <a name="prerequisites"></a>Vereisten
U hebt het volgende nodig om deze quickstart te voltooien:

- Een exemplaar van de aangepaste zoekactie. Zie [maken van uw eerste exemplaar van Bing aangepaste zoekactie](quick-start.md).

- [Node.js](https://www.nodejs.org/) geïnstalleerd.

-  [Cognitieve Services API account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met **Bing zoeken-API's**. De [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search) voldoende is voor deze snelstartgids. U moet de toegangssleutel die is opgegeven bij het activeren van uw gratis proefversie of u kunt de sleutel van een betaald abonnement van uw Azure-dashboard.

## <a name="run-the-code"></a>De code uitvoeren

Volg deze stappen voor het aanroepen van het eindpunt Bing aangepaste zoeken:

1. Maak een map voor uw code.
2. Navigeer naar de map die u zojuist hebt gemaakt vanaf een opdrachtprompt of terminal.
3. Installeer de **aanvraag** knooppunt module:
    <pre>
    npm install request
    </pre>
4. Het bestand BingCustomSearch.js maken en kopieer de volgende code toe.
5. Vervang **uw ABONNEMENTSSLEUTEL** en **uw-aangepaste-CONFIG-ID** met uw sleutel en configuratie-ID (Zie stap 1).

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
6. De code met de volgende opdracht uitvoeren.
    ```    
    node BingCustomSearch.js
   ``` 

## <a name="next-steps"></a>Volgende stappen
- [Configureer uw gehoste UI-mogelijkheden](./hosted-ui.md)
- [Decoration markeringen gebruiken om te markeren van tekst](./hit-highlighting.md)
- [Pagina webpagina 's](./page-webpages.md)