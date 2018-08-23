---
title: 'Snelstartgids: Vragen en filteren van afbeeldingen met behulp van de SDK in Python'
description: In deze snelstartgids hebt u aanvragen en filteren van de installatiekopieën die zijn geretourneerd door Bing afbeeldingen zoeken, met behulp van Python.
titleSuffix: Azure Image Search SDK Python quickstart
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 02/14/2018
ms.author: v-gedod
ms.openlocfilehash: 4729f103bb9b50d4ff039907db8eb677f3dc290a
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2018
ms.locfileid: "41987957"
---
 # <a name="quickstart-request-and-filter-images-using-the-sdk-and-python"></a>Snelstartgids: Vragen en filteren van afbeeldingen met behulp van de SDK en Python

De Bing afbeeldingen zoeken-SDK bevat de functionaliteit van de REST-API voor web-query's en parseren resultaten. 

De [broncode voor Python Bing afbeeldingen zoeken-SDK-voorbeelden](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/image_search_samples.py) is beschikbaar op Git Hub.

## <a name="application-dependencies"></a>Afhankelijkheden voor toepassingen
Als u er nog geen hebt, installeert u Python. De SDK is compatibel met Python 2.7, 3.3, 3.4, 3.5 en 3.6.

De algemene aanbevelingen voor het ontwikkelen van Python is het gebruik van een [virtuele omgeving](https://docs.python.org/3/tutorial/venv.html). Installeren en het initialiseren van de virtuele omgeving met de [venv module](https://pypi.python.org/pypi/virtualenv). U moet voor Python 2.7 virtualenv installeren.
```
python -m venv mytestenv
```
Bing afbeeldingen zoeken-SDK-afhankelijkheden installeren:
```
cd mytestenv
python -m pip install azure-cognitiveservices-search-imagesearch
```
## <a name="image-search-client"></a>Afbeeldingen zoeken-client
Krijgen een [Cognitive Services-toegangssleutel](https://azure.microsoft.com/try/cognitive-services/) onder *zoeken*. Invoer toevoegen:
```
from azure.cognitiveservices.search.imagesearch import ImageSearchAPI
from azure.cognitiveservices.search.imagesearch.models import ImageType, ImageAspect, ImageInsightModule
from msrest.authentication import CognitiveServicesCredentials

subscription_key = "YOUR-SUBSCRIPTION-KEY"
```
Maak een instantie van de `CognitiveServicesCredentials`, en het instantiëren van de client:
```
client = ImageSearchAPI(CognitiveServicesCredentials(subscription_key))
```
Zoeken naar afbeeldingen op query (Yosemite), gefilterd op GIF-animaties en breed aspect. Controleer of het aantal resultaten en afdrukken insightsToken, miniatuur-URL en web-URL van de eerste resultaat.
```
image_results = client.images.search(
        query="Yosemite",
        image_type=ImageType.animated_gif, # Could be the str "AnimatedGif"
        aspect=ImageAspect.wide # Could be the str "Wide"
    )
    print("\r\nSearch images for \"Yosemite\" results that are animated gifs and wide aspect")

    if image_results.value:
        first_image_result = image_results.value[0]
        print("Image result count: {}".format(len(image_results.value)))
        print("First image insights token: {}".format(first_image_result.image_insights_token))
        print("First image thumbnail url: {}".format(first_image_result.thumbnail_url))
        print("First image web search url: {}".format(first_image_result.web_search_url))
    else:
        print("Couldn't find image results!")

```
Zoeken naar afbeeldingen voor (Yosemite), gefilterd op GIF-animaties en breed aspect.  Controleer of het aantal resultaten.  Afdrukken `insightsToken`, `thumbnail url` en `web url` van eerste resultaat.
```
image_results = client.images.search(
    query="Yosemite",
    image_type=ImageType.animated_gif, # Could be the str "AnimatedGif"
    aspect=ImageAspect.wide # Could be the str "Wide"
)
print("\r\nSearch images for \"Yosemite\" results that are animated gifs and wide aspect")

if image_results.value:
    first_image_result = image_results.value[0]
    print("Image result count: {}".format(len(image_results.value)))
    print("First image insights token: {}".format(first_image_result.image_insights_token))
    print("First image thumbnail url: {}".format(first_image_result.thumbnail_url))
    print("First image web search url: {}".format(first_image_result.web_search_url))
else:
    print("Couldn't find image results!")

```

Trending resultaten ophalen:
```
trending_result = client.images.trending()
print("\r\nSearch trending images")

# Categorires
if trending_result.categories:
    first_category = trending_result.categories[0]
    print("Category count: {}".format(len(trending_result.categories)))
    print("First category title: {}".format(first_category.title))
    if first_category.tiles:
        first_tile = first_category.tiles[0]
        print("Subcategory tile count: {}".format(len(first_category.tiles)))
        print("First tile text: {}".format(first_tile.query.text))
        print("First tile url: {}".format(first_tile.query.web_search_url))
    else:
        print("Couldn't find subcategory tiles!")
    else:
        print("Couldn't find categories!")

```

## <a name="next-steps"></a>Volgende stappen

[Cognitive Services Python SDK-voorbeelden](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples)


