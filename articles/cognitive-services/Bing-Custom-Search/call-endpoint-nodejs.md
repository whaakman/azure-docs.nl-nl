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
ms.openlocfilehash: 73c31c7175bd4dfcb182fb76784937c176ac7702
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46977873"
---
# <a name="call-bing-custom-search-endpoint-nodejs"></a>Aanroep van Bing Custom Search-eindpunt (Node.js)

Deze quickstart laat zien hoe zoekresultaten van uw exemplaar voor aangepast zoeken met behulp van Node.js om aan te roepen van het eindpunt van de Bing Custom Search aanvraagt. 

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om deze quickstart te voltooien:

- Een exemplaar voor aangepast zoeken van kant-en-klare. Zie [maken van uw eerste exemplaar van de Bing Custom Search](quick-start.md).
- [Node.js](https://www.nodejs.org/) geïnstalleerd.
- De abonnementssleutel van een. Krijgt u een abonnementssleutel wanneer u activeert de [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search), of kunt u een betaald abonnement-sleutel in uw Azure-dashboard (Zie [Cognitive Services-API-account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)).    

## <a name="run-the-code"></a>De code uitvoeren

Als u wilt uitvoeren in het volgende voorbeeld, de volgende stappen uit:

1. Maak een map voor uw code.  
  
2. Navigeer naar de map die u zojuist hebt gemaakt vanaf een opdrachtprompt of terminal.  
  
3. Installeer de **aanvraag** knooppunt module:
    <pre>
    npm install request
    </pre>  
    
4. Maak een bestand met de naam BingCustomSearch.js in de map die u hebt gemaakt en kopieer de volgende code naar het. Vervang **uw-SUBSCRIPTION-KEY** en **uw-aangepaste-CONFIG-ID** met uw abonnementssleutel en de configuratie-ID.  
  
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
  
6. Voer de code met de volgende opdracht uit:  
  
    ```    
    node BingCustomSearch.js
    ``` 

## <a name="next-steps"></a>Volgende stappen
- [Configureren van uw gehoste gebruikersinterface-ervaring](./hosted-ui.md)
- [Decoration markeringen gebruiken om te markeren van tekst](./hit-highlighting.md)
- [Pagina webpagina 's](./page-webpages.md)
