---
title: Snelstartgids voor Azure cognitieve Services, Bing nieuws Search API Python | Microsoft Docs
description: Get-informatie en codevoorbeelden om u te helpen snel aan de slag met de Bing nieuws zoeken-API in Microsoft cognitieve Services in Azure.
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: article
ms.date: 9/21/2017
ms.author: v-jerkin
ms.openlocfilehash: 0fde478b650513aa1527c1d47f5b453ba094506c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344731"
---
# <a name="quickstart-for-bing-news-search-api-with-python"></a>Quick Start voor Bing nieuws zoekopdracht API met behulp van Python
In dit scenario ziet u een eenvoudig voorbeeld van het aanroepen van de Search-API van Bing nieuws en na het verwerken van de resulterende JSON-object. Zie voor meer informatie [Bing nieuwe zoekopdracht documentatie](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference).  

U kunt in dit voorbeeld uitvoeren als een Jupyter-notebook op [MyBinder](https://mybinder.org) door te klikken op de lancering Binder badge: 

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingNewsSearchAPI.ipynb)

## <a name="prerequisites"></a>Vereisten

U moet hebben een [cognitieve Services API account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met **Bing zoeken-API's**. De [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) voldoende is voor deze snelstartgids. U moet de toegangssleutel die is opgegeven bij het activeren van uw gratis proefversie of u kunt de sleutel van een betaald abonnement van uw Azure-dashboard.

## <a name="running-the-walkthrough"></a>De procedure wordt uitgevoerd
Stel eerst `subscription_key` naar uw API-sleutel voor de Bing-API-service.


```python
subscription_key = None
assert subscription_key
```

Controleer vervolgens de `search_url` eindpunt juist is. In dit artikel wordt slechts één eindpunt voor Bing zoeken-API's gebruikt. Als u autorisatie fouten optreden, controleer dan deze waarde voor het eindpunt Bing zoeken in uw Azure-dashboard.


```python
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/news/search"
```

Stel `search_term` op zoek naar nieuwsartikelen over Microsoft.


```python
search_term = "Microsoft"
```

De volgende blokkeren maakt gebruik van de `requests` bibliotheek in Python aanroepen uit de Bing zoeken-API's en de resultaten worden geretourneerd als een JSON-object. Zien dat wordt doorgegeven in de API-sleutel via de `headers` woordenlijst en de zoekopdracht benaming de `params` woordenlijst. Zie de volledige lijst met opties die kunnen worden gebruikt om zoekresultaten te filteren, de [REST-API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference) documentatie.


```python
import requests

headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
params  = {"q": search_term, "textDecorations": True, "textFormat": "HTML"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

De `search_results` object bevat de nieuwe artikelen samen met de rijke metagegevens. Bijvoorbeeld haalt de volgende regel code de beschrijvingen van de artikelen.


```python
descriptions = [article["description"] for article in search_results["value"]]
```

Deze beschrijvingen kunnen vervolgens worden gerenderd als een tabel met het trefwoord gemarkeerd in **vet**.


```python
from IPython.display import HTML
rows = "\n".join(["<tr><td>{0}</td></tr>".format(desc) for desc in descriptions])
HTML("<table>"+rows+"</table>")
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Paginering nieuws](paging-news.md)
> [decoration markeringen met tekst markeren](hit-highlighting.md)

## <a name="see-also"></a>Zie ook 

 [Zoeken op het web voor nieuws](search-the-web.md)  
 [Probeer deze](https://azure.microsoft.com/services/cognitive-services/bing-news-search-api/)
