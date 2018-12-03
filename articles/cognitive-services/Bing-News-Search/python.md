---
title: 'Snelstart: Bing Nieuws zoeken-API, Python'
titlesuffix: Azure Cognitive Services
description: Informatie en codevoorbeelden om snel aan de slag te gaan met de Bing Nieuws zoeken-API.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: quickstart
ms.date: 9/21/2017
ms.author: aahi
ms.openlocfilehash: 738b139cb2070f2244442311d3670757caac6541
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2018
ms.locfileid: "52308814"
---
# <a name="quickstart-for-bing-news-search-api-with-python"></a>Snelstart voor Bing Nieuws zoeken-API met Python
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
