---
title: Oproep- en antwoord - Python Quick Start voor Azure cognitieve Services, Bing installatiekopie zoeken-API | Microsoft Docs
description: Get-informatie en codevoorbeelden kunt u snel aan de slag met Bing installatiekopie Search API in Microsoft cognitieve Services in Azure.
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 9/21/2017
ms.author: v-jerkin
ms.openlocfilehash: 3b5d6a961ce4bcde8aaf73f1fbd30689a6c2c2d1
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344729"
---
# <a name="call-and-response-your-first-bing-image-search-query-in-python"></a>Oproep- en -antwoord: uw eerste zoekopdracht van Bing installatiekopie in Python

De Bing-API van zoekservice afbeelding biedt een vergelijkbaar met Bing.com/Images ervaring doordat u een zoekopdracht gebruiker verzenden naar Bing en ophalen van een lijst met relevante installatiekopieën.

In dit scenario ziet u een eenvoudig voorbeeld van het aanroepen van de Search-API van Bing installatiekopie en de resulterende JSON-object achteraf worden verwerkt. Zie voor meer informatie [zoeken naar Bing-afbeelding documentatie](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference).

U kunt in dit voorbeeld uitvoeren als een Jupyter-notebook op [MyBinder](https://mybinder.org) door te klikken op de lancering Binder badge: 

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingImageSearchAPI.ipynb)

## <a name="prerequisites"></a>Vereisten

U moet hebben een [cognitieve Services API account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met **Bing zoeken-API's**. De [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) voldoende is voor deze snelstartgids. U moet de toegangssleutel die is opgegeven bij het activeren van uw gratis proefversie of u kunt de sleutel van een betaald abonnement van uw Azure-dashboard.

## <a name="running-the-walkthrough"></a>De procedure wordt uitgevoerd
Om door te gaan met deze stapsgewijze Kennismaking ingesteld `subscription_key` naar uw API-sleutel voor de Bing-API-service.


```python
subscription_key = None
assert subscription_key
```

Controleer vervolgens de `search_url` eindpunt juist is. In dit artikel wordt slechts één eindpunt voor Bing zoeken-API's gebruikt. Als u autorisatie fouten optreden, controleer dan deze waarde voor het eindpunt Bing zoeken in uw Azure-dashboard.


```python
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/images/search"
```

Stel `search_term` om te zoeken voor afbeeldingen van Puppy's.


```python
search_term = "puppies"
```

De volgende blokkeren maakt gebruik van de `requests` bibliotheek in Python aanroepen uit de Bing zoeken-API's en de resultaten worden geretourneerd als een JSON-object. Zien dat wordt doorgegeven in de API-sleutel via de `headers` woordenlijst en de zoekopdracht benaming de `params` woordenlijst. Zie de volledige lijst met opties die kunnen worden gebruikt om zoekresultaten te filteren, de [REST-API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference) documentatie.


```python
import requests

headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
params  = {"q": search_term, "license": "public", "imageType": "photo"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

De `search_results` object bevat de werkelijke installatiekopieën samen met de rijke metagegevens zoals verwante items. De volgende coderegel kan bijvoorbeeld de miniatuur-URL's voor de eerste 16 resultaten extraheren.


```python
thumbnail_urls = [img["thumbnailUrl"] for img in search_results["value"][:16]]
```

Vervolgens kunnen we gebruiken de `PIL` bibliotheek voor het downloaden van de miniatuurafbeeldingen en de `matplotlib` bibliotheek om ze in een raster $4 \times 4$ weer te geven.


```python
%matplotlib inline
import matplotlib.pyplot as plt
from PIL import Image
from io import BytesIO

f, axes = plt.subplots(4, 4)
for i in range(4):
    for j in range(4):
        image_data = requests.get(thumbnail_urls[i+4*j])
        image_data.raise_for_status()
        image = Image.open(BytesIO(image_data.content))        
        axes[i][j].imshow(image)
        axes[i][j].axis("off")
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zoeken naar Bing-afbeelding één pagina app-zelfstudie](../tutorial-bing-image-search-single-page-app.md)

## <a name="see-also"></a>Zie ook 

[Zoeken naar afbeelding van Bing-overzicht](../overview.md)  
[Probeer deze](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
[Een gratis proefversie toegangssleutel ophalen](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api)  
[Bing installatiekopie Search API-verwijzingen](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)
