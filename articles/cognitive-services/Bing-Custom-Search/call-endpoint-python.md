---
title: Oproep-eindpunt met cognitieve Python - Bing aangepaste zoekactie - Microsoft-Services
description: Deze snelstartgids laat zien hoe zoekresultaten aanvragen van uw aangepaste zoekactie-exemplaar met behulp van Python aanroepen van het eindpunt van de aangepaste Bing-zoekactie.
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 05/07/2018
ms.author: v-brapel
ms.openlocfilehash: 889762ae0b401438f25546738268c584ddd58389
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "35345858"
---
# <a name="call-bing-custom-search-endpoint-python"></a>Aanroep Bing aangepaste zoekactie eindpunt (Python)

Deze snelstartgids laat zien hoe zoekresultaten aanvragen van uw aangepaste zoekactie-exemplaar met behulp van Python aanroepen van het eindpunt van de aangepaste Bing-zoekactie. 

## <a name="prerequisites"></a>Vereisten
U hebt het volgende nodig om deze quickstart te voltooien:

- Een exemplaar van de aangepaste zoekactie. Zie [maken van uw eerste exemplaar van Bing aangepaste zoekactie](quick-start.md).

-  [Python](https://www.python.org/) geïnstalleerd.

- Een [cognitieve Services API account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met **Bing zoeken-API's**. De [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search) voldoende is voor deze snelstartgids. U moet de toegangssleutel die is opgegeven bij het activeren van uw gratis proefversie of u kunt de sleutel van een betaald abonnement van uw Azure-dashboard. 

## <a name="run-the-code"></a>De code uitvoeren

Volg deze stappen voor het aanroepen van het eindpunt Bing aangepaste zoeken:

1. Maak een map voor uw code.
2. Navigeer naar de map die u zojuist hebt gemaakt vanaf een opdrachtprompt als administrator of terminal.
3. Installeer de **aanvragen** python-module:
    <pre>
    pip install pipenv
    pipenv install requests
    </pre>
7. Het bestand BingCustomSearch.py maken en kopieer de volgende code toe.
8. Vervang **uw ABONNEMENTSSLEUTEL** en **uw-aangepaste-CONFIG-ID** met uw sleutel en configuratie-ID (Zie stap 1).

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
9. De code met de volgende opdracht uitvoeren.
    ```
    python BingCustomSearch.py
    ```

## <a name="next-steps"></a>Volgende stappen
- [Configureer uw gehoste UI-mogelijkheden](./hosted-ui.md)
- [Decoration markeringen gebruiken om te markeren van tekst](./hit-highlighting.md)
- [Pagina webpagina 's](./page-webpages.md)