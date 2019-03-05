---
title: 'Quickstart: Spellingcontrole met de Bing Spellingcontrole-REST API en Python'
titlesuffix: Azure Cognitive Services
description: Aan de slag met de Bing Spellingcontrole-REST API om de spelling en grammatica te controleren.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: quickstart
ms.date: 02/20/2019
ms.author: aahi
ms.openlocfilehash: e95006c6448bf1179d33bcd00c16d6e4246db148
ms.sourcegitcommit: 24906eb0a6621dfa470cb052a800c4d4fae02787
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56887315"
---
# <a name="quickstart-check-spelling-with-the-bing-spell-check-rest-api-and-python"></a>Quickstart: Spellingcontrole met de Bing Spellingcontrole-REST API en Python

Gebruik deze quickstart om uw eerste aanroep naar de Bing Spellingcontrole REST API te maken. Deze eenvoudige Python-toepassing verzendt een aanvraag naar de API en retourneert een lijst met voorgestelde correcties. Hoewel deze toepassing in Python is geschreven, is de API een RESTful-webservice die compatibel is met vrijwel elke programmeertaal. De broncode voor deze toepassing is beschikbaar op [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/python/Search/BingEntitySearchv7.py)

## <a name="prerequisites"></a>Vereisten

* Python [3.x](https://www.python.org)

[!INCLUDE [cognitive-services-bing-spell-check-signup-requirements](../../../../includes/cognitive-services-bing-spell-check-signup-requirements.md)]


## <a name="initialize-the-application"></a>De toepassing initialiseren

1. Maak een nieuw Python-bestand in uw favoriete IDE of editor en voeg de volgende importinstructie toe.

   ```python
   import requests
   import json
   ```

2. Maak variabelen voor de tekst waarvan u de spelling wilt controleren, maak de abonnementssleutel en het Bing Spellingcontrole-eindpunt.

    ```python
    api_key = "enter-your-key-here"
    example_text = "Hollo, wrld" # the text to be spell-checked
    endpoint = "https://api.cognitive.microsoft.com/bing/v7.0/SpellCheck"
    ```

## <a name="create-the-parameters-for-the-request"></a>Parameters voor de aanvraag maken

1. Een nieuwe woordenlijst maken met `text` als sleutel en uw tekst als waarde.

    ```python
    data = {'text': example_text}
    ```

2. Voeg de parameters voor uw aanvraag toe. Stel de `mkt`-parameter in voor uw markt en `mode` voor `proof`. 

    ```python
    params = {
        'mkt':'en-us',
        'mode':'proof'
        }
    ```

3. Voeg een `Content-Type`-header toe en voeg uw abonnementssleutel toe aan de `Ocp-Apim-Subscription-Key`-header.

    ```python
    headers = {
        'Content-Type': 'application/x-www-form-urlencoded',
        'Ocp-Apim-Subscription-Key': api_key,
        }
    ```

## <a name="send-the-request-and-read-the-response"></a>De aanvraag verzenden en het antwoord lezen

1. Verzend de POST-aanvraag met de aanvraagbibliotheek.

    ```python
    response = requests.post(endpoint, headers=headers, params=params, data=data)
    ```

2. Haal het JSON-antwoord op en druk het af.
    
    ```python
    json_response = response.json()
    print(json.dumps(json_response, indent=4))
    ```

## <a name="example-json-response"></a>Voorbeeld van JSON-antwoord

Een geslaagd antwoord wordt geretourneerd in de JSON-indeling, zoals u kunt zien in het volgende voorbeeld: 

```json
{
   "_type": "SpellCheck",
   "flaggedTokens": [
      {
         "offset": 0,
         "token": "Hollo",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "Hello",
               "score": 0.9115257530801
            },
            {
               "suggestion": "Hollow",
               "score": 0.858039839213461
            },
            {
               "suggestion": "Hallo",
               "score": 0.597385084464481
            }
         ]
      },
      {
         "offset": 7,
         "token": "wrld",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "world",
               "score": 0.9115257530801
            }
         ]
      }
   ]
}
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een web-app met één pagina maken](../tutorials/spellcheck.md)

- [Wat is de Bing Spellingcontrole-API?](../overview.md)
- [Referentie voor de Bing Spellingcontrole-API v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference)
