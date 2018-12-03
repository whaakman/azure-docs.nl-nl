---
title: 'Snelstartgids: Een zoekopdracht uitvoeren met Python - Bing Webzoekopdrachten-API'
titleSuffix: Azure Cognitive Services
description: In deze snelstartgids leert u hoe u voor het eerst de Bing Webzoekopdrachten-API aanroept met Python en een JSON-antwoord ontvangt.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: quickstart
ms.date: 8/16/2018
ms.author: aahi
ms.openlocfilehash: 0f6f3991e01e4eb6919d958002ef6230a2570dbe
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2018
ms.locfileid: "52309450"
---
# <a name="quickstart-use-python-to-call-the-bing-web-search-api"></a>Snelstartgids: Python gebruiken voor het aanroepen van de Bing Webzoekopdrachten-API  
Haal een [Cognitive Services-toegangssleutel](https://azure.microsoft.com/try/cognitive-services/) op onder **Zoeken**.  Zie ook [Prijsinformatie Cognitive Services - Bing Zoeken-API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

Gebruik deze snelstart om voor het eerst de Bing Webzoekopdrachten-API aan te roepen en binnen tien minuten een JSON-antwoord te ontvangen.  

[!INCLUDE [bing-web-search-quickstart-signup](../../../../includes/bing-web-search-quickstart-signup.md)]

Zie ook [Prijsinformatie Cognitive Services - Bing Zoeken-API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

Dit voorbeeld wordt uitgevoerd als een Jupyter-notitieblok in [MyBinder](https://mybinder.org). Klik op de badge voor het lanceren van Binder:

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingWebSearchAPI.ipynb)

## <a name="define-variables"></a>Variabelen definiëren

Vervang de waarde `subscription_key` door een geldige abonnementssleutel uit uw Azure-account.

```python
subscription_key = "YOUR_ACCESS_KEY"
assert subscription_key
```

Declareer het eindpunt voor de Bing Webzoekopdrachten-API. Als er autorisatiefouten optreden, moet u deze waarde controleren ten aanzien van het eindpunt voor zoeken met Bing in uw Azure-dashboard.

```python
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/search"
```

U kunt de zoekquery gerust aanpassen door de waarde voor `search_term` te vervangen.

```python
search_term = "Azure Cognitive Services"
```

## <a name="make-a-request"></a>Een aanvraag maken

Dit blok maakt gebruik van de `requests`-bibliotheek om de Bing Webzoekopdrachten-API aan te roepen en de resultaten als een JSON-object te retourneren. De API-sleutel wordt doorgegeven in de `headers`-woordenlijst, en de zoekterm en queryparameters worden doorgegeven in de `params`-woordenlijst. Raadpleeg de documentatie voor [Bing Webzoekopdrachten-API v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference) voor een volledige lijst met opties en parameters.

```python
import requests

headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
params  = {"q": search_term, "textDecorations":True, "textFormat":"HTML"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

## <a name="format-and-display-the-response"></a>Het antwoord opmaken en weergeven

Het object `search_results` bevat de zoekresultaten en metagegevens, zoals gerelateerde query's en pagina's. Deze code maakt gebruikt van de `IPython.display`-bibliotheek om het antwoord in uw browser op te maken en weer te geven.

```python
from IPython.display import HTML

rows = "\n".join(["""<tr>
                       <td><a href=\"{0}\">{1}</a></td>
                       <td>{2}</td>
                     </tr>""".format(v["url"],v["name"],v["snippet"]) \
                  for v in search_results["webPages"]["value"]])
HTML("<table>{0}</table>".format(rows))
```

## <a name="sample-code-on-github"></a>Voorbeeldcode in GitHub

Als u deze code lokaal wilt uitvoeren, vindt u het volledige [voorbeeld op GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/nodejs/Search/BingWebSearchv7.js).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zelfstudie voor app met één pagina voor Bing Web Search](../tutorial-bing-web-search-single-page-app.md)

[!INCLUDE [bing-web-search-quickstart-see-also](../../../../includes/bing-web-search-quickstart-see-also.md)]
