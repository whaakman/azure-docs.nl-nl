---
title: 'Quickstart: afbeeldingsinzichten krijgen met behulp van de Bing Visual Search REST API en Python'
titleSuffix: Azure Cognitive Services
description: Leer hoe u een afbeelding uploadt naar de Bing Visual Search-API en inzichten in de afbeelding verkrijgt.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 4/02/2019
ms.author: scottwhi
ms.openlocfilehash: 7ec37b4c3bdeb924b3e35dbcb5d07a478611f631
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2019
ms.locfileid: "59047123"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-rest-api-and-python"></a>Quickstart: afbeeldingsinzichten krijgen met behulp van de Bing Visual Search REST API en Python

Met deze Quick Start kunt u uw eerste aanroep naar de Bing visuele zoekopdrachten-API en de resultaten te bekijken. Deze Python-toepassing wordt een installatiekopie geüpload naar de API en de informatie die wordt weergegeven. Hoewel deze toepassing is geschreven in Python, de API is een RESTful-Web-compatibel is met de meeste moderne programmeertalen.

Wanneer u een lokale installatiekopie uploadt, de gegevens moet bevatten de `Content-Disposition` header. U moet instellen de `name` parameter 'afbeelding', en u kunt instellen de `filename` parameter een tekenreeks. De inhoud van het formulier omvat de binaire gegevens van de installatiekopie. De grootte van de maximale installatiekopie die u kunt uploaden is 1 MB.

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

## <a name="prerequisites"></a>Vereisten

* [Python 3.x](https://www.python.org/)

[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="initialize-the-application"></a>De toepassing initialiseren

1. Maak een nieuwe Python-bestand in uw favoriete IDE of editor, en voeg de volgende `import` instructie:

    ```python
    import requests, json
    ```

2. Variabelen voor de abonnementssleutel van uw, eindpunt en het pad naar de installatiekopie die u uploadt maken:

    ```python

    BASE_URI = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch'
    SUBSCRIPTION_KEY = 'your-subscription-key'
    imagePath = 'your-image-path'
    ```

3. Maak een dictionary-object voor het opslaan van de header-informatie van uw aanvraag. Uw abonnementssleutel binden aan de tekenreeks `Ocp-Apim-Subscription-Key`, zoals hieronder weergegeven:

    ```python
    HEADERS = {'Ocp-Apim-Subscription-Key': SUBSCRIPTION_KEY}
    ```

4. Maak een andere woordenlijst om uw installatiekopie, die wordt geopend en wordt geüpload wanneer u de aanvraag verzendt:

    ```python
    file = {'image' : ('myfile', open(imagePath, 'rb'))}
    ```

## <a name="parse-the-json-response"></a>Het JSON-antwoord parseren

1. Maken van een methode met de naam `print_json()` in de API-reactie en afdrukken van de JSON:

    ```python
    def print_json(obj):
        """Print the object as json"""
        print(json.dumps(obj, sort_keys=True, indent=2, separators=(',', ': ')))
    ```

## <a name="send-the-request"></a>De aanvraag verzenden

1. Gebruik `requests.post()` om een aanvraag naar de Bing Visual Search-API te verzenden. Neem hierin de tekenreeks voor uw eindpunt, de header en gegevens over het bestand op. Afdrukken `response.json()` met `print_json()`:

    ```python
    try:
        response = requests.post(BASE_URI, headers=HEADERS, files=file)
        response.raise_for_status()
        print_json(response.json())
    
    except Exception as ex:
        raise ex
    ```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een visuele zoekopdrachten één pagina web-app maken](../tutorial-bing-visual-search-single-page-app.md)
