---
title: Snelstartgids voor Azure cognitieve Services, Bing Video Search API Python | Microsoft Docs
description: Get-informatie en codevoorbeelden om u te helpen snel aan de slag met de Bing Video zoeken-API in Microsoft cognitieve Services in Azure.
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: article
ms.date: 9/21/2017
ms.author: v-jerkin
ms.openlocfilehash: ce4356f05e69540bc3bc3241e2ec1751ff7a7276
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344727"
---
# <a name="quickstart-for-bing-video-search-api-with-python"></a>Quick Start voor Bing Video zoeken-API met behulp van Python

In dit scenario ziet u hoe de Bing Video zoeken-API gebruiken, onderdeel van Microsoft cognitieve Services in Azure. Raadpleeg de [API-referentiemateriaal](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference) voor technische informatie over de API's.

U kunt in dit voorbeeld uitvoeren als een Jupyter-notebook op [MyBinder](https://mybinder.org) door te klikken op de lancering Binder badge: 

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingVideoSearchAPI.ipynb)


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
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/videos/search"
```

Stel `search_term` op zoek naar video's van katjes


```python
search_term = "kittens"
```

De volgende blokkeren maakt gebruik van de `requests` bibliotheek in Python aanroepen uit de Bing zoeken-API's en de resultaten worden geretourneerd als een JSON-object. Zien dat wordt doorgegeven in de API-sleutel via de `headers` woordenlijst en de zoekopdracht benaming de `params` woordenlijst. Zie de volledige lijst met opties die kunnen worden gebruikt om zoekresultaten te filteren, de [REST-API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference) documentatie.


```python
import requests

headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
params  = {"q": search_term, "count":5, "pricing": "free", "videoLength":"short"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

De `search_results` object bevat de relevante video's samen met de rijke metagegevens. Weergave een van de video's, gebruikt u de `embedHtml` eigenschap en plaats deze in een `IFrame`.


```python
from IPython.display import HTML
HTML(search_results["value"][0]["embedHtml"].replace("autoplay=1","autoplay=0"))
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Paginering video's](paging-videos.md)
> [miniatuurafbeeldingen bijsnijden en Resizing](resize-and-crop-thumbnails.md)

## <a name="see-also"></a>Zie ook 

 [Zoeken op het web voor video's](search-the-web.md) [te proberen](https://azure.microsoft.com/services/cognitive-services/bing-video-search-api/)
