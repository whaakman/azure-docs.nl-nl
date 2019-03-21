---
title: 'Quickstart: Aanroepen van uw Bing Custom Search-eindpunt met behulp van de Python-SDK | Microsoft Docs'
titleSuffix: Azure Cognitive Services
description: De Bing aangepaste zoekopdrachten SDK voor Python gebruiken om aangepaste zoekresultaten.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: quickstart
ms.date: 03/05/2019
ms.author: aahi
ms.openlocfilehash: c4c5059bc57ea33357145f6b119456dc6c5bdb7b
ms.sourcegitcommit: dd1a9f38c69954f15ff5c166e456fda37ae1cdf2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57571809"
---
# <a name="quickstart-call-your-bing-custom-search-endpoint-using-the-python-sdk"></a>Quickstart: Aanroepen van uw Bing Custom Search-eindpunt met behulp van de Python-SDK 

Gebruik deze Quick Start om te beginnen met aanvragen van zoekresultaten van uw Bing Custom Search-exemplaar met behulp van de Python-SDK. Hoewel Bing Custom Search een REST-API heeft die compatibel is met de meeste programmeertalen, biedt de Bing Custom Search SDK een eenvoudige manier om de service in uw toepassingen te integreren. De broncode voor dit voorbeeld kunt u vinden op [GitHub](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/custom_search_samples.py) met extra foutafhandeling en aantekeningen.

## <a name="prerequisites"></a>Vereisten

- Een Bing Custom Search-exemplaar. Zie [Quickstart: Uw eerste Bing Custom Search-exemplaar maken](quick-start.md) voor meer informatie.
- Python [2.x of 3.x](https://www.python.org/) 
- De [Bing Custom Search SDK voor Python](https://pypi.org/project/azure-cognitiveservices-search-customsearch/) 

## <a name="install-the-python-sdk"></a>De Python-SDK installeren

De Bing Custom Search SDK installeren met de volgende opdracht.

```Console
python -m pip install azure-cognitiveservices-search-customsearch
```


## <a name="create-a-new-application"></a>Een nieuwe toepassing maken

Maak een nieuwe Python-bestand in uw favoriete editor of IDE, en voeg de volgende import toe.

```python
from azure.cognitiveservices.search.customsearch import CustomSearchClient
from msrest.authentication import CognitiveServicesCredentials
```

## <a name="create-a-search-client-and-send-a-request"></a>Maak een search-client en een aanvraag verzenden

1. Maak een variabele voor uw abonnementssleutel.

    ```python
    subscription_key = 'your-subscription-key'
    ```

2. Maak een instantie van `CustomSearchClient`, met een `CognitiveServicesCredentials` object met de abonnementssleutel. 

    ```python
    client = CustomSearchClient(CognitiveServicesCredentials(subscription_key))
    ```

3. Verzenden van een zoekopdracht met `client.custom_instance.search()`. Toevoegen van de zoekterm die de `query` parameter en stel `custom_config` naar uw aangepaste configuratie-ID om uw search-exemplaar te gebruiken. Krijgt u de ID van de [Bing Custom Search-portal](https://www.customsearch.ai/), door te klikken op de **productie** tabblad.

    ```python
    web_data = client.custom_instance.search(query="xbox", custom_config="your-configuration-id")
    ```

## <a name="view-the-search-results"></a>De lijst met zoekresultaten weergeven

Als alle zoekresultaten voor webpagina's zijn gevonden, het eerste certificaat ophalen en afdrukken van de naam, de URL en de totale webpagina's gevonden.

```python
if web_data.web_pages.value:
    first_web_result = web_data.web_pages.value[0]
    print("Web Pages result count: {}".format(len(web_data.web_pages.value)))
    print("First Web Page name: {}".format(first_web_result.name))
    print("First Web Page url: {}".format(first_web_result.url))
else:
    print("Didn't see any web data..")
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een web-app voor aangepaste zoekopdrachten bouwen](./tutorials/custom-search-web-page.md)
