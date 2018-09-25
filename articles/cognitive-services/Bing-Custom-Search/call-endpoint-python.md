---
title: Eindpunt aanroepen met behulp van Python - Bing Custom Search - Microsoft Cognitive Services
description: Deze quickstart laat zien hoe zoekresultaten van uw exemplaar voor aangepast zoeken met behulp van Python om aan te roepen van het eindpunt van de Bing Custom Search aanvraagt.
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 05/07/2018
ms.author: v-brapel
ms.openlocfilehash: 3666e92372e9bed80e5c0c7991dcac730cebb588
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46967588"
---
# <a name="call-bing-custom-search-endpoint-python"></a>Bing Custom Search-eindpunt (Python) aanroepen

In deze Quick Start laat zien hoe zoekresultaten aanvragen bij uw exemplaar voor aangepast zoeken voor het aanroepen van het eindpunt van de Bing Custom Search met behulp van Python. 

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om deze quickstart te voltooien:

- Een exemplaar voor aangepast zoeken van kant-en-klare. Zie [maken van uw eerste exemplaar van de Bing Custom Search](quick-start.md).
- [Python](https://www.python.org/) geïnstalleerd.
- De abonnementssleutel van een. Krijgt u een abonnementssleutel wanneer u activeert de [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search), of kunt u een betaald abonnement-sleutel in uw Azure-dashboard (Zie [Cognitive Services-API-account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)).    


## <a name="run-the-code"></a>De code uitvoeren

Als u wilt uitvoeren in het volgende voorbeeld, de volgende stappen uit:

1. Maak een map voor uw code.  
  
2. Navigeer naar de map die u zojuist hebt gemaakt van een administrator-opdrachtprompt of terminal.  
  
3. Installeer de **aanvragen** python-module:  
  
    <pre>
    pip install pipenv
    pipenv install requests
    </pre>  
      
4. Maak een bestand met de naam BingCustomSearch.py in de map die u hebt gemaakt en kopieer de volgende code naar het. Vervang **uw-SUBSCRIPTION-KEY** en **uw-aangepaste-CONFIG-ID** met uw subscriptioin sleutel en configuratie-ID.  
  
    ``` Python
    import json
    import requests
    
    subscriptionKey = "YOUR-SUBSCRIPTION-KEY"
    customConfigId = "YOUR-CUSTOM-CONFIG-ID"
    searchTerm = "microsoft"
    
    url = 'https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?q=' + searchTerm + '&customconfig=' + customConfigId
    r = requests.get(url, headers={'Ocp-Apim-Subscription-Key': subscriptionKey})
    print(r.text)
    ```  
  
7. Voer de code met de volgende opdracht uit.  
  
    ```
    python BingCustomSearch.py
    ```

## <a name="next-steps"></a>Volgende stappen
- [Configureren van uw gehoste gebruikersinterface-ervaring](./hosted-ui.md)
- [Decoration markeringen gebruiken om te markeren van tekst](./hit-highlighting.md)
- [Pagina webpagina 's](./page-webpages.md)
