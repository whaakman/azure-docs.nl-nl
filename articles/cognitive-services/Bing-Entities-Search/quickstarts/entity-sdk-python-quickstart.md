---
title: 'Snelstart: Bing Entiteiten zoeken SDK, Python'
titlesuffix: Azure Cognitive Services
description: De Bing Entiteiten zoeken-SDK-consoletoepassing instellen.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: quickstart
ms.date: 02/01/2019
ms.author: v-gedod
ms.openlocfilehash: 50620eba35be136e38d5b1f8c3083f9ddf189d9e
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55757761"
---
# <a name="quickstart-bing-entity-search-sdk-with-python"></a>Quickstart: Bing Entiteiten zoeken SDK met Python

Gebruik deze quickstart om te zoeken naar entiteiten met de Bing Entiteiten zoeken-SDK voor Python. Hoewel Bing Entiteiten zoeken een REST-API heeft die compatibel is met de meeste moderne programmeertalen, biedt de SDK een eenvoudige manier om de service te integreren in uw toepassingen. De broncode voor dit voorbeeld is te vinden op [GitHub](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/entity_search_samples.py).

## <a name="prerequisites"></a>Vereisten

* Python [2.x of 3.x](https://www.python.org/)

* De [Bing Entiteiten zoeken-SDK voor Python](https://pypi.org/project/azure-cognitiveservices-search-entitysearch/)

U kunt het best een virtuele Python-omgeving gebruiken. U kunt een virtuele omgeving installeren en initialiseren met de venv-module. U kunt virtualenv voor Python 2.7 installeren met:

```Console
python -m venv mytestenv
```

Installeer de Bing Entiteiten zoeken-SDK met:

```Console
cd mytestenv
python -m pip install azure-cognitiveservices-search-entitysearch
```

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../../includes/cognitive-services-bing-entity-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>De toepassing maken en initialiseren

1. Maak een nieuw Python-bestand in uw favoriete IDE of editor en voeg de volgende importinstructies toe. 

    ```python
    from azure.cognitiveservices.search.entitysearch import EntitySearchAPI
    from azure.cognitiveservices.search.entitysearch.models import Place, ErrorResponseException
    from msrest.authentication import CognitiveServicesCredentials
    ```

2. Maak een variabele voor uw abonnementssleutel en start een instantie van de client door er een nieuw `CognitiveServicesCredentials`-object mee te maken.
    
    ```python
    subscription_key = "YOUR-SUBSCRIPTION-KEY"
    client = EntitySearchAPI(CognitiveServicesCredentials(subscription_key))
    ```

## <a name="send-a-search-request-and-receive-a-response"></a>Een zoekopdracht verzenden en een antwoord ontvangen

1. Verzend een zoekopdracht naar Bing Entiteiten zoeken met `client.entities.search()` en een zoekquery. 
    
    ```python
    entity_data = client.entities.search(query="Gibralter")
    ```

2. Als er entiteiten worden geretourneerd, zet u `entity_data.entities.value` om in een lijst en geeft u het eerste resultaat weer.
    ```python
    if entity_data.entities.value:
    
        main_entities = [entity for entity in entity_data.entities.value
                         if entity.entity_presentation_info.entity_scenario == "DominantEntity"]
    
        if main_entities:
            print(main_entities[0].description)
    ```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een web-app met één pagina maken](../tutorial-bing-entities-search-single-page-app.md)

* [Wat is de Bing Entiteiten zoeken-API?](../overview.md )