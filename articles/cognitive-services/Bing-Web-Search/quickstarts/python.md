---
title: Oproep- en antwoord - Python Quick Start voor Azure cognitieve Services, zoekopdracht Bing-Web-API | Microsoft Docs
description: Get-informatie en codevoorbeelden om u te helpen snel aan de slag met de Bing Web Search-API in Microsoft cognitieve Services in Azure.
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 9/18/2017
ms.author: v-jerkin
ms.openlocfilehash: 8d4df9db60c7a74a5b9e53d4622528c0054b4f19
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344759"
---
# <a name="call-and-response-your-first-bing-web-search-query-in-python"></a>Oproep- en -antwoord: uw eerste Bing Web-zoekopdracht in Python

De Bing Web zoeken-API biedt een vergelijkbaar met Bing.com/Search ervaring door te retourneren zoekresultaten die Bing bepaalt relevant zijn voor de query van de gebruiker. De resultaten kunnen webpagina's, afbeeldingen, video's, nieuws en entiteiten, samen met verwante zoekquery's, correcties tijdzones, eenheidconversie, vertalingen en berekeningen bevatten. Het soort u resultaten zijn gebaseerd op hun relevantie en de laag van de Bing zoeken-API's waarop u bent geabonneerd.

Raadpleeg de [API-referentiemateriaal](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference) voor technische informatie over de API's.

U kunt in dit voorbeeld uitvoeren als een Jupyter-notebook op [MyBinder](https://mybinder.org) door te klikken op de lancering Binder badge: 

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingWebSearchAPI.ipynb)


## <a name="prerequisites"></a>Vereisten
U moet hebben een [cognitieve Services API account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met **Bing zoeken-API's**. De [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) voldoende is voor deze snelstartgids. U moet de toegangssleutel die is opgegeven bij het activeren van uw gratis proefversie of u kunt de sleutel van een betaald abonnement van uw Azure-dashboard.

## <a name="running-the-walkthrough"></a>De procedure wordt uitgevoerd

Stel `subscription_key` naar uw API-sleutel voor de Bing-API-service.


```python
subscription_key = None
assert subscription_key
```

Controleer vervolgens de `search_url` eindpunt juist is. In dit artikel wordt slechts één eindpunt voor Bing zoeken-API's gebruikt. Als u autorisatie fouten optreden, controleer dan deze waarde voor het eindpunt Bing zoeken in uw Azure-dashboard.


```python
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/search"
```

Stel `search_term` aan query Bing voor cognitieve Microsoft-Services.


```python
search_term = "Microsoft Cognitive Services"
```

De volgende blokkeren maakt gebruik van de `requests` bibliotheek in Python aanroepen uit de Bing zoeken-API's en de resultaten worden geretourneerd als een JSON-object. Zien dat wordt doorgegeven in de API-sleutel via de `headers` woordenlijst en de zoekopdracht benaming de `params` woordenlijst. Zie de volledige lijst met opties die kunnen worden gebruikt om zoekresultaten te filteren, de [REST-API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference) documentatie.


```python
import requests

headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
params  = {"q": search_term, "textDecorations":True, "textFormat":"HTML"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

De `search_results` object bevat de lijst met zoekresultaten samen met de rijke metagegevens zoals verwante query's en pagina's. De volgende regels code de eerste pagina's die door de query-indeling.


```python
from IPython.display import HTML

rows = "\n".join(["""<tr>
                       <td><a href=\"{0}\">{1}</a></td>
                       <td>{2}</td>
                     </tr>""".format(v["url"],v["name"],v["snippet"]) \
                  for v in search_results["webPages"]["value"]])
HTML("<table>{0}</table>".format(rows))
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Bing Web search één pagina app-zelfstudie](../tutorial-bing-web-search-single-page-app.md)

## <a name="see-also"></a>Zie ook 

[Overzicht van Bing webpagina's zoeken](../overview.md)  
[Probeer deze](https://azure.microsoft.com/services/cognitive-services/bing-web-search-api/)  
[Verkrijgen van een gratis proefversie toegangssleutel](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api)
[Bing Web Search API-verwijzingen](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference)
