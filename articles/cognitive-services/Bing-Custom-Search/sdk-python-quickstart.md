---
title: 'Quickstart: Het Bing Aangepaste zoekopdrachten-eind punt aanroepen met behulp van de python-SDK | Microsoft Docs'
titleSuffix: Azure Cognitive Services
description: Gebruik deze Quick Start om te beginnen met het aanvragen van zoek resultaten van uw Bing Aangepaste zoekopdrachten-exemplaar met behulp van de python-SDK.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: quickstart
ms.date: 07/26/2019
ms.author: aahi
ms.openlocfilehash: 7a9d0d6619878299768e1f17b4da3280ea1ec1fc
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68564620"
---
# <a name="quickstart-call-your-bing-custom-search-endpoint-using-the-python-sdk"></a>Quickstart: Het Bing Aangepaste zoekopdrachten-eind punt aanroepen met de python-SDK 

Gebruik deze Quick Start om de zoek resultaten van uw Bing Aangepaste zoekopdrachten-exemplaar te vragen met behulp van de python-SDK. Hoewel Bing Custom Search een REST-API heeft die compatibel is met de meeste programmeertalen, biedt de Bing Custom Search SDK een eenvoudige manier om de service in uw toepassingen te integreren. De bron code voor dit voor beeld is te vinden op [github](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/custom_search_samples.py) met aanvullende fout afhandeling en annotaties.

## <a name="prerequisites"></a>Vereisten

- Een Bing Custom Search-exemplaar. Zie [Quickstart: Uw eerste Bing Custom Search-exemplaar maken](quick-start.md) voor meer informatie.
- Python [2.x of 3.x](https://www.python.org/) 
- De [Bing aangepaste zoekopdrachten SDK voor python](https://pypi.org/project/azure-cognitiveservices-search-customsearch/) 

## <a name="install-the-python-sdk"></a>De python-SDK installeren

Installeer de Bing Aangepaste zoekopdrachten SDK met de volgende opdracht.

```Console
python -m pip install azure-cognitiveservices-search-customsearch
```


## <a name="create-a-new-application"></a>Een nieuwe toepassing maken

Maak een nieuw python-bestand in uw favoriete editor of IDE en voeg de volgende import bewerkingen toe.

```python
from azure.cognitiveservices.search.customsearch import CustomSearchClient
from msrest.authentication import CognitiveServicesCredentials
```

## <a name="create-a-search-client-and-send-a-request"></a>Een zoek-client maken en een aanvraag verzenden

1. Maak een variabele voor uw abonnementssleutel.

    ```python
    subscription_key = 'your-subscription-key'
    ```

2. Maak een instantie van `CustomSearchClient`met behulp `CognitiveServicesCredentials` van een object met de abonnements sleutel. 

    ```python
    client = CustomSearchClient(CognitiveServicesCredentials(subscription_key))
    ```

3. Verzend een zoek opdracht met `client.custom_instance.search()`. Voeg uw zoek term toe aan `query` de para meter en `custom_config` stel deze in op uw aangepaste configuratie-ID om uw zoek exemplaar te gebruiken. U kunt uw ID ophalen via de [Bing aangepaste zoekopdrachten Portal](https://www.customsearch.ai/)door te klikken op het tabblad **productie** .

    ```python
    web_data = client.custom_instance.search(query="xbox", custom_config="your-configuration-id")
    ```

## <a name="view-the-search-results"></a>De zoek resultaten weer geven

Als er Zoek resultaten voor webpagina's zijn gevonden, de eerste ophalen en de naam, URL en het totale aantal gevonden webpagina's afdrukken.

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
