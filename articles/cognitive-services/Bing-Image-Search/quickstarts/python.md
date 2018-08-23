---
title: "Snelstartgids: Verzenden zoekquery's met de REST-API voor de Bing afbeeldingen zoeken-API in Python"
description: In deze Quick Start stuurt u zoekopdrachten naar de Bing webzoekopdrachten-API om een lijst met relevante afbeeldingen met behulp van Python.
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 9/21/2017
ms.author: v-jerkin
ms.openlocfilehash: bc527ba39b580935f113f56aa63f7bdd283ba304
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2018
ms.locfileid: "41988790"
---
# <a name="quickstart-send-search-queries-using-the-rest-api-and-python"></a>Snelstartgids: Verzenden zoekquery's met behulp van de REST-API en Python

De Bing afbeeldingen zoeken-API biedt een ervaring die vergelijkbaar is met Bing.com/Images doordat u kunt een zoekquery voor de gebruiker naar Bing te verzenden en een lijst met relevante afbeeldingen teruggaan.

Dit scenario ziet u een eenvoudig voorbeeld van de aanroep naar de Bing afbeeldingen zoeken-API en nabewerking het resulterende JSON-object. Zie voor meer informatie, [Bing afbeeldingen zoeken-documentatie](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference).

In dit voorbeeld kunt u uitvoeren als een Jupyter-notebook op [MyBinder](https://mybinder.org) door te klikken op de lancering Binder badge: 

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingImageSearchAPI.ipynb)

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="running-the-walkthrough"></a>De procedures uitvoeren
Om door te gaan met deze stapsgewijze Kennismaking instelt `subscription_key` tot uw API-sleutel voor de Bing API-service.


```python
subscription_key = None
assert subscription_key
```

Vervolgens controleren of de `search_url` eindpunt juist is. In dit artikel is geschreven, wordt slechts één eindpunt gebruikt voor Bing zoeken-API's. Als u autorisatiefouten optreden, Controleer u deze waarde op basis van de Bing zoeken-eindpunten in uw Azure-dashboard.


```python
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/images/search"
```

Stel `search_term` om te zoeken voor afbeeldingen van Puppy's.


```python
search_term = "puppies"
```

De volgende blokkeren maakt gebruik van de `requests` bibliotheek in Python aanroepen uit naar de Bing zoeken-API's en de resultaten worden geretourneerd als JSON-object. Zien dat we in de API-sleutel via doorgeven de `headers` woordenlijst en de zoekopdracht termijn de `params` woordenlijst. Zie de volledige lijst met opties die kunnen worden gebruikt om zoekresultaten te filteren, de [REST-API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference) documentatie.


```python
import requests

headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
params  = {"q": search_term, "license": "public", "imageType": "photo"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

De `search_results` -object bevat de werkelijke hoeveelheid installatiekopieën en uitgebreide metagegevens zoals verwante items. De volgende coderegel kunt bijvoorbeeld de miniatuur-URL's voor de eerste 16 resultaten ophalen.


```python
thumbnail_urls = [img["thumbnailUrl"] for img in search_results["value"][:16]]
```

Vervolgens, gebruiken we de `PIL` bibliotheek voor het downloaden van de miniaturen en de `matplotlib` bibliotheek om ze in een raster $4 \times 4$ weer te geven.


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
> [Zelfstudie voor Bing afbeeldingen zoeken-app met één pagina](../tutorial-bing-image-search-single-page-app.md)

## <a name="see-also"></a>Zie ook 

[Overzicht van de Bing afbeeldingen zoeken](../overview.md)  
[Nu uitproberen](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
[Een gratis proefversie toegangssleutel ophalen](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api)  
[Bing afbeeldingen zoeken-API-verwijzing](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)
