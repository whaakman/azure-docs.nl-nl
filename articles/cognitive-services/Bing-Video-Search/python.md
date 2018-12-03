---
title: 'Snelstartgids: Bing Video Search, Python'
titlesuffix: Azure Cognitive Services
description: Informatie en codevoorbeelden ophalen om u te helpen snel aan de slag te gaan met de Bing Video’s zoeken-API.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: quickstart
ms.date: 9/21/2017
ms.author: aahi
ms.openlocfilehash: e902ae940364810e634434fb1f08408fadea9c8a
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2018
ms.locfileid: "52310545"
---
# <a name="quickstart-bing-video-search-api-with-python"></a>Snelstartgids: Bing Video's zoeken-API met Python

In deze procedure leert u hoe u de Bing Video’s zoeken-API gebruikt, die onderdeel is van Microsoft Cognitive Services in Azure. Raadpleeg de [API-referentie](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference) voor technische informatie over de API's.

U kunt dit voorbeeld uitvoeren als een Jupyter-notebook op [MyBinder](https://mybinder.org) door te klikken op de badge launch binder: 

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingVideoSearchAPI.ipynb)


## <a name="prerequisites"></a>Vereisten
U moet beschikken over een [account voor Cognitive Services-API](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met **Bing Zoeken-API’s**. De [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) is voldoende voor deze snelstartgids. U hebt de toegangssleutel nodig die wordt verstrekt bij het activeren van uw gratis proefversie of u gebruikt de sleutel van een betaald abonnement vanuit uw Azure-dashboard. Zie ook [Prijsinformatie Cognitive Services - Bing Zoeken-API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="running-the-walkthrough"></a>De voorbeeldtoepassing uitvoeren

Stel eerst `subscription_key` in op de API-sleutel voor de Bing API-service.


```python
subscription_key = None
assert subscription_key
```

Controleer vervolgens of het eindpunt `search_url` juist is. Op het moment van schrijven wordt er maar één eindpunt gebruikt voor Bing zoeken-API's. Als er autorisatiefouten optreden, moet u deze waarde nogmaals vergelijken met het eindpunt voor zoeken met Bing in uw Azure-dashboard.


```python
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/videos/search"
```

Stel `search_term` in om te zoeken naar video's van poezen


```python
search_term = "kittens"
```

Het volgende blok maakt gebruik van de bibliotheek `requests` in Python om de Bing zoeken-API's aan te roepen en de resultaten als een JSON-object te retourneren. Merk op dat we de API-sleutel doorgeven via de woordenlijst `headers` en de zoekterm via de woordenlijst `params`. Raadpleeg de [REST-API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference)-documentatie voor de volledige lijst met opties die kunnen worden gebruikt om zoekresultaten te filteren.


```python
import requests

headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
params  = {"q": search_term, "count":5, "pricing": "free", "videoLength":"short"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

Het object `search_results` bevat de relevante video's, samen met uitgebreide metagegevens. Als u een van de video's wilt weergeven, gebruikt u de eigenschap `embedHtml` van de video en plaatst u de video in een `IFrame`.


```python
from IPython.display import HTML
HTML(search_results["value"][0]["embedHtml"].replace("autoplay=1","autoplay=0"))
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Video's pagineren](paging-videos.md)
> [Miniatuurafbeeldingen vergroten/verkleinen en bijsnijden](resize-and-crop-thumbnails.md)

## <a name="see-also"></a>Zie ook 

 [Internet doorzoeken op video's](search-the-web.md) [Probeer het uit](https://azure.microsoft.com/services/cognitive-services/bing-video-search-api/)
