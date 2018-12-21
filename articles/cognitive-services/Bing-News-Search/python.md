---
title: 'Snelstartgids: Nieuws zoeken met Python - REST API voor Bing News Search'
titlesuffix: Azure Cognitive Services
description: Gebruik deze snelstartgids om een aanvraag naar de REST API van Bing News Search te verzenden via Python en een JSON-antwoord te ontvangen.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: quickstart
ms.date: 9/21/2017
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: 8ce8353df9a6f8354c56d9c9115645c0b7f2136a
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2018
ms.locfileid: "53251654"
---
# <a name="quickstart-perform-a-news-search-using-python-and-the-bing-news-search-rest-api"></a>Snelstartgids: Nieuws zoeken met behulp van Python en de REST API voor Bing News Search

In deze kennismaking wordt een eenvoudig voorbeeld gegeven van het aanroepen van de Bing Nieuws zoeken-API en het nabewerken van het resulterende JSON-object. Zie de [Bing Nieuws zoeken-documentatie](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference) voor meer informatie.  

U kunt dit voorbeeld uitvoeren als een Jupyter-notebook op [MyBinder](https://mybinder.org) door te klikken op de badge launch binder: 

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingNewsSearchAPI.ipynb)

## <a name="prerequisites"></a>Vereisten

U moet beschikken over een [account voor Cognitive Services-API](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met **Bing Zoeken-API’s**. De [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) is voldoende voor deze snelstartgids. U hebt de toegangssleutel nodig die is verstrekt tijdens het activeren van uw gratis proefversie.  Zie ook [Prijsinformatie Cognitive Services - Bing Zoeken-API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="running-the-walkthrough"></a>De voorbeeldtoepassing uitvoeren
Stel eerst `subscription_key` in op de API-sleutel voor de Bing API-service.


```python
subscription_key = None
assert subscription_key
```

Controleer vervolgens of het eindpunt `search_url` juist is. Op het moment van schrijven wordt er maar één eindpunt gebruikt voor Bing zoeken-API's. Als er autorisatiefouten optreden, moet u deze waarde nogmaals vergelijken met het eindpunt voor zoeken met Bing in uw Azure-dashboard.


```python
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/news/search"
```

Stel `search_term` in om te zoeken naar nieuwsartikelen over Microsoft.


```python
search_term = "Microsoft"
```

Het volgende blok maakt gebruik van de bibliotheek `requests` in Python om de Bing zoeken-API's aan te roepen en de resultaten als een JSON-object te retourneren. Merk op dat we de API-sleutel doorgeven via de woordenlijst `headers` en de zoekterm via de woordenlijst `params`. Raadpleeg de [REST-API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference)-documentatie voor de volledige lijst met opties die kunnen worden gebruikt om zoekresultaten te filteren.


```python
import requests

headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
params  = {"q": search_term, "textDecorations": True, "textFormat": "HTML"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

Het object `search_results` bevat de relevante nieuwsartikelen, samen met uitgebreide metagegevens. De volgende regel code extraheert bijvoorbeeld de beschrijvingen van de artikelen.


```python
descriptions = [article["description"] for article in search_results["value"]]
```

Deze beschrijvingen kunnen vervolgens worden weergegeven als een tabel met het zoekwoord **vet** gemarkeerd.


```python
from IPython.display import HTML
rows = "\n".join(["<tr><td>{0}</td></tr>".format(desc) for desc in descriptions])
HTML("<table>"+rows+"</table>")
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Nieuwsartikelen per pagina weergeven](paging-news.md)
> [Decoratiemarkeringen gebruiken om tekst te markeren](hit-highlighting.md)

## <a name="see-also"></a>Zie ook 

 [Nieuwsartikelen zoeken op internet](search-the-web.md)  
 [Proberen](https://azure.microsoft.com/services/cognitive-services/bing-news-search-api/)
