---
title: 'Snelstart: Afbeeldingen zoeken met Python - Bing Afbeeldingen zoeken-API'
description: Gebruik deze snelstart om voor het eerst de Bing Afbeeldingen zoeken-API aan te roepen en een JSON-antwoord te ontvangen. Deze eenvoudige Python-toepassing stuurt een zoekquery naar de API en toont de onbewerkte resultaten.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: quickstart
ms.date: 8/20/2018
ms.author: aahi
ms.openlocfilehash: 44cc556e68234fb9957c01fa9f04861293e96e6a
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46298163"
---
# <a name="quickstart-send-search-queries-using-the-rest-api-and-python"></a>Snel starten: zoekquery's verzenden met behulp van de REST-API en Python

Gebruik deze snelstart om voor het eerst de Bing Afbeeldingen zoeken-API aan te roepen en een JSON-antwoord te ontvangen. Deze eenvoudige Python-toepassing stuurt een zoekquery naar de API en toont de onbewerkte resultaten.

Hoewel deze toepassing in Python is geschreven, is de API een RESTful-webservice die compatibel is met vrijwel elke programmeertaal.

U kunt dit voorbeeld uitvoeren als een Jupyter-notebook op [MyBinder](https://mybinder.org) door te klikken op de badge launch binder:

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingImageSearchAPI.ipynb)


Daarnaast is de broncode voor dit voorbeeld beschikbaar op [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/python/Search/BingImageSearchv7.py).

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="running-the-quickstart"></a>De snelstart uitvoeren

Als u aan de slag wilt gaan, stelt u `subscription_key` in op een geldige abonnementssleutel voor de Bing API-service.

```python
subscription_key = None
assert subscription_key
```

Controleer vervolgens of het eindpunt `search_url` juist is. Op het moment van schrijven wordt er maar één eindpunt gebruikt voor Bing zoeken-API's. Als er autorisatiefouten optreden, moet u deze waarde nogmaals vergelijken met het eindpunt voor zoeken met Bing in uw Azure-dashboard.


```python
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/images/search"
```

Stel `search_term` in op het zoeken naar afbeeldingen van puppy's.


```python
search_term = "puppies"
```

Het volgende blok maakt gebruik van de bibliotheek `requests` in Python om de Bing zoeken-API's aan te roepen en de resultaten als een JSON-object te retourneren. Merk op dat we de API-sleutel doorgeven via de woordenlijst `headers` en de zoekterm via de woordenlijst `params`. Raadpleeg de [REST-API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)-documentatie voor de volledige lijst met opties die kunnen worden gebruikt om zoekresultaten te filteren.


```python
import requests

headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
params  = {"q": search_term, "license": "public", "imageType": "photo"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

Het `search_results`-object bevat de daadwerkelijke afbeeldingen, naast uitgebreide metagegevens zoals gerelateerde items. Met de volgende regel met code kunt u bijvoorbeeld de miniatuur-URL’s voor de eerste 16 resultaten extraheren.


```python
thumbnail_urls = [img["thumbnailUrl"] for img in search_results["value"][:16]]
```

Vervolgens gebruikt u de bibliotheek `PIL` om de miniatuurafbeeldingen te downloaden; de bibliotheek `matplotlib` rendert ze vervolgens in een $4 \times 4$-raster.


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
plt.show()
```

## <a name="sample-json-response"></a>Voorbeeld van een JSON-antwoord

Antwoorden afkomstig van de Bing Afbeeldingen zoeken-API worden geretourneerd in de JSON-indeling. Dit voorbeeldantwoord is ingekort zodat één resultaat wordt weergegeven.

```json
{
"_type":"Images",
"instrumentation":{
    "_type":"ResponseInstrumentation"
},
"readLink":"images\/search?q=tropical ocean",
"webSearchUrl":"https:\/\/www.bing.com\/images\/search?q=tropical ocean&FORM=OIIARP",
"totalEstimatedMatches":842,
"nextOffset":47,
"value":[
    {
        "webSearchUrl":"https:\/\/www.bing.com\/images\/search?view=detailv2&FORM=OIIRPO&q=tropical+ocean&id=8607ACDACB243BDEA7E1EF78127DA931E680E3A5&simid=608027248313960152",
        "name":"My Life in the Ocean | The greatest WordPress.com site in ...",
        "thumbnailUrl":"https:\/\/tse3.mm.bing.net\/th?id=OIP.fmwSKKmKpmZtJiBDps1kLAHaEo&pid=Api",
        "datePublished":"2017-11-03T08:51:00.0000000Z",
        "contentUrl":"https:\/\/mylifeintheocean.files.wordpress.com\/2012\/11\/tropical-ocean-wallpaper-1920x12003.jpg",
        "hostPageUrl":"https:\/\/mylifeintheocean.wordpress.com\/",
        "contentSize":"897388 B",
        "encodingFormat":"jpeg",
        "hostPageDisplayUrl":"https:\/\/mylifeintheocean.wordpress.com",
        "width":1920,
        "height":1200,
        "thumbnail":{
        "width":474,
        "height":296
        },
        "imageInsightsToken":"ccid_fmwSKKmK*mid_8607ACDACB243BDEA7E1EF78127DA931E680E3A5*simid_608027248313960152*thid_OIP.fmwSKKmKpmZtJiBDps1kLAHaEo",
        "insightsMetadata":{
        "recipeSourcesCount":0,
        "bestRepresentativeQuery":{
            "text":"Tropical Beaches Desktop Wallpaper",
            "displayText":"Tropical Beaches Desktop Wallpaper",
            "webSearchUrl":"https:\/\/www.bing.com\/images\/search?q=Tropical+Beaches+Desktop+Wallpaper&id=8607ACDACB243BDEA7E1EF78127DA931E680E3A5&FORM=IDBQDM"
        },
        "pagesIncludingCount":115,
        "availableSizesCount":44
        },
        "imageId":"8607ACDACB243BDEA7E1EF78127DA931E680E3A5",
        "accentColor":"0050B2"
    }
}
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zelfstudie voor app met één pagina voor Bing Image Search](../tutorial-bing-image-search-single-page-app.md)

## <a name="see-also"></a>Zie ook

* [Wat is Bing Image Search?](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/overview)  
* [Online interactieve demo proberen](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
* [Gratis Cognitive Services-toegangssleutel ophalen](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api)  
* [Documentatie van Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services)
* [Naslag voor Bing Afbeeldingen zoeken-API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)
