---
title: 'Snelstartgids: Zoeken voor afbeeldingen met behulp van de Bing afbeeldingen zoeken-SDK en Python'
titleSuffix: Azure Cognitive Services
description: Gebruik deze Quick Start om te zoeken en -installatiekopieën zoeken op Internet met behulp van de Bing afbeeldingen zoeken-SDK en Python.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 08/28/2018
ms.author: aahi
ms.openlocfilehash: 4a24f1e4e051b627034f1d4664e94e0f47c43014
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45578290"
---
# <a name="quickstart-search-for-images-with-the-bing-image-search-sdk-and-python"></a>Snelstartgids: Zoeken voor afbeeldingen met de Bing afbeeldingen zoeken-SDK en Python

Met deze Quick Start kunt u uw eerste afbeeldingen zoeken met behulp van de Bing afbeeldingen zoeken SDK, die een wrapper voor de API en bevat de dezelfde functies. Deze eenvoudige Python-toepassing verzendt een zoekquery afbeelding, parseert de JSON-antwoord en Hiermee geeft u de URL van de eerste afbeelding geretourneerd.

De broncode voor dit voorbeeld is beschikbaar [op Github](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/image-search-quickstart.py) met extra foutafhandeling en aantekeningen.

## <a name="prerequisites"></a>Vereisten 

* [Python 2.7 of 3.4](https://www.python.org/) en hoger.

* De [installatiekopieën van Azure Search SDK](https://pypi.org/project/azure-cognitiveservices-search-imagesearch/) voor Python
    * Installeren met behulp van `pip install azure-cognitiveservices-search-imagesearch`

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Maken en initialiseren van de toepassing

1. Maak een nieuwe Python-script in uw favoriete IDE of editor, en de volgende producten:

    ```python
    from azure.cognitiveservices.search.imagesearch import ImageSearchAPI
    from msrest.authentication import CognitiveServicesCredentials
    ```

2. Variabelen maken voor uw abonnementstermijn van sleutel en zoeken.

    ```python
    subscription_key = "Enter your key here"
    search_term = "canadian rockies"
    ```

## <a name="create-the-image-search-client"></a>De afbeelding zoeken-client maken

3. Maak een instantie van `CognitiveServicesCredentials`, en deze gebruiken voor het starten van de client:

    ```python
    client = ImageSearchAPI(CognitiveServicesCredentials(subscription_key))
    ```
4. Zoekquery's verzenden naar de Bing afbeeldingen zoeken-API:
    ```python
    image_results = client.images.search(query=search_term)
    ```
## <a name="process-and-view-the-results"></a>Verwerken en de resultaten bekijken

Parseren van de afbeeldingsresultaten in het antwoord geretourneerd.


Als het antwoord bevat een lijst met zoekresultaten, het eerste resultaat opslaan en afdrukken van de details, zoals een miniatuur-URL, wordt de oorspronkelijke URL, samen met het totale aantal installatiekopieën geretourneerd.  

```python
if image_results.value:
    first_image_result = image_results.value[0]
    print("Total number of images returned: {}".format(len(image_results.value)))
    print("First image thumbnail url: {}".format(first_image_result.thumbnail_url))
    print("First image content url: {}".format(first_image_result.content_url))
else:
    print("No image results returned!")
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zelfstudie voor Bing afbeeldingen zoeken-app met één pagina](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/tutorial-bing-image-search-single-page-app)

## <a name="see-also"></a>Zie ook 

* [Wat is de Bing afbeeldingen zoeken?](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/overview)  
* [Probeer een online interactieve demo](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
* [Een gratis Cognitive Services-toegangssleutel ophalen](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api)
* [Python-voorbeelden voor de SDK van Azure Cognitive Services](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples)  
* [Documentatie voor Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services)
* [Bing afbeeldingen zoeken-API-verwijzing](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)