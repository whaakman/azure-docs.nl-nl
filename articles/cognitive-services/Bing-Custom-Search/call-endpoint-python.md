---
title: 'Quickstart: Eindpunt aanroepen met behulp van Python - Bing Custom Search'
titlesuffix: Azure Cognitive Services
description: Deze quickstart laat zien hoe u zoekresultaten opvraagt bij uw exemplaar voor aangepaste zoekopdrachten door met behulp van Python het eindpunt van Bing Custom Search aan te roepen.
services: cognitive-services
author: brapel
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: quickstart
ms.date: 05/07/2018
ms.author: v-brapel
ms.openlocfilehash: d16b7354b0ef727ba3c670b97105e1bd6f99034e
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48815457"
---
# <a name="quickstart-call-bing-custom-search-endpoint-python"></a>Quickstart: Eindpunt van Bing Custom Search aanroepen (Python)

Deze quickstart laat zien hoe u zoekresultaten opvraagt bij uw exemplaar voor aangepaste zoekopdrachten door met behulp van Python het eindpunt van Bing Custom Search aan te roepen. 

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om deze quickstart te voltooien:

- Een exemplaar voor aangepaste zoekopdrachten dat klaar is voor gebruik. Zie [Uw eerste Bing Aangepaste zoekopdrachten-exemplaar maken](quick-start.md) voor meer informatie.
- [Python](https://www.python.org/) geïnstalleerd.
- Een abonnementssleutel. U kunt een abonnementssleutel opvragen wanneer u uw [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search) activeert, of u kunt een sleutel voor een betaald abonnement gebruiken uit uw Azure-dashboard (zie [Snelstartgids: Een Cognitive Services-account maken in de Azure-portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)).    


## <a name="run-the-code"></a>De code uitvoeren

Volg deze stappen om het voorbeeld uit te voeren:

1. Maak een map voor uw code.  
  
2. Navigeer vanuit een opdrachtprompt met beheerdersbevoegdheden of een terminal naar de map die u zojuist hebt gemaakt.  
  
3. Installeer de Python-module **requests**:  
  
    <pre>
    pip install pipenv
    pipenv install requests
    </pre>  
      
4. Maak een bestand met de naam BingCustomSearch.py in de map die u hebt gemaakt en kopieer de volgende code naar het bestand. Vervang **YOUR-SUBSCRIPTION-KEY** en **YOUR-CUSTOM-CONFIG-ID** door uw abonnementssleutel en configuratie-id.  
  
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
  
7. Voer de code uit met behulp van de volgende opdracht.  
  
    ```
    python BingCustomSearch.py
    ```

## <a name="next-steps"></a>Volgende stappen
- [Gehoste UI-ervaring configureren](./hosted-ui.md)
- [Decoratiemarkeringen gebruiken om tekst te markeren](./hit-highlighting.md)
- [Bladeren door webpagina's](./page-webpages.md)
