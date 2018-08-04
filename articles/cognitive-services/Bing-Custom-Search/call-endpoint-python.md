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
ms.openlocfilehash: 88bf82805ba46abf79b7899e0428a83485062302
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39504964"
---
# <a name="call-bing-custom-search-endpoint-python"></a>Bing Custom Search-eindpunt (Python) aanroepen

Deze quickstart laat zien hoe zoekresultaten van uw exemplaar voor aangepast zoeken met behulp van Python om aan te roepen van het eindpunt van de Bing Custom Search aanvraagt. 

## <a name="prerequisites"></a>Vereiste onderdelen
U hebt het volgende nodig om deze quickstart te voltooien:

- Een exemplaar voor aangepast zoeken. Zie [maken van uw eerste exemplaar van de Bing Custom Search](quick-start.md).

-  [Python](https://www.python.org/) geïnstalleerd.

- Een [Cognitive Services-API-account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met **Bing zoeken-API's**. De [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search) is voldoende voor deze Quick Start. Moet u de toegangssleutel die is opgegeven wanneer u uw gratis proefversie activeren, of u de sleutel van een betaald abonnement van uw Azure-dashboard kunt. 

## <a name="run-the-code"></a>De code uitvoeren

Volg deze stappen voor het aanroepen van de Bing Custom Search-eindpunt:

1. Maak een map voor uw code.

2. Navigeer naar de map die u zojuist hebt gemaakt van een administrator-opdrachtprompt of terminal.

3. Installeer de **aanvragen** python-module:

    <pre>
    pip install pipenv
    pipenv install requests
    </pre>
    
7. Maak het bestand BingCustomSearch.py en kopieer de volgende code toe.

8. Vervang **uw-SUBSCRIPTION-KEY** en **uw-aangepaste-CONFIG-ID** door uw sleutel en -configuratie-ID (Zie stap 1).

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
9. Voer de code met de volgende opdracht uit.
    ```
    python BingCustomSearch.py
    ```

## <a name="next-steps"></a>Volgende stappen
- [Configureren van uw gehoste gebruikersinterface-ervaring](./hosted-ui.md)
- [Decoration markeringen gebruiken om te markeren van tekst](./hit-highlighting.md)
- [Pagina webpagina 's](./page-webpages.md)
