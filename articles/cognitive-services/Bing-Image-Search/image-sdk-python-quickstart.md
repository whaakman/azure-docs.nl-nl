---
title: Afbeelding zoeken SDK Python Quick Start | Microsoft Docs
description: Instellingen voor de installatiekopie Search SDK-consoletoepassing.
titleSuffix: Azure Image Search SDK Python quickstart
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 02/14/2018
ms.author: v-gedod
ms.openlocfilehash: e30852439ad8ec2d5ddc667b75167e8b5d35be33
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345696"
---
# <a name="image-search-sdk-python-quickstart"></a>Afbeelding zoeken SDK Python Quick Start

De Bing installatiekopie Search SDK bevat de functionaliteit van de REST-API voor web-query's en parseren resultaten. 

De [broncode voor Python Bing installatiekopie Search SDK voorbeelden](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/image_search_samples.py) beschikbaar is op de Git-Hub.

## <a name="application-dependencies"></a>Afhankelijkheden voor toepassingen
Als u dit nog geen hebt, installeert u Python. De SDK is compatibel met Python 2.7 3.3, 3.4, 3.5 en 3.6.

De algemene aanbevelingen voor de ontwikkeling van een Python is met een [virtuele omgeving](https://docs.python.org/3/tutorial/venv.html). Installeren en het initialiseren van de virtuele omgeving met de [venv module](https://pypi.python.org/pypi/virtualenv). Voor Python 2.7 moet u virtualenv installeren.
```
python -m venv mytestenv
```
Bing installatiekopie zoeken SDK-afhankelijkheden installeren:
```
cd mytestenv
python -m pip install azure-cognitiveservices-search-imagesearch
```
## <a name="image-search-client"></a>Afbeelding zoeken client
Ophalen van een [cognitieve toegangssleutel](https://azure.microsoft.com/try/cognitive-services/) onder *Search*. Invoer toevoegen:
```
from azure.cognitiveservices.search.imagesearch import ImageSearchAPI
from azure.cognitiveservices.search.imagesearch.models import ImageType, ImageAspect, ImageInsightModule
from msrest.authentication import CognitiveServicesCredentials

subscription_key = "YOUR-SUBSCRIPTION-KEY"
```
Maak een instantie van de `CognitiveServicesCredentials`, en te maken van de client:
```
client = ImageSearchAPI(CognitiveServicesCredentials(subscription_key))
```
Installatiekopieën van de query (Yosemite) gefilterd voor GIF-bestanden en wide aspect zoeken. Controleer of het aantal resultaten en afdrukken insightsToken miniatuur-URL en web URL van de eerste resultaat.
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
Afbeeldingen voor (Yosemite) gefilterd voor GIF-bestanden en wide aspect zoeken.  Controleer of het aantal resultaten.  Afdrukken `insightsToken`, `thumbnail url` en `web url` van eerste resultaat.
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

Trends resultaten ophalen:
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

[Cognitieve Services Python SDK-voorbeelden](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples)


