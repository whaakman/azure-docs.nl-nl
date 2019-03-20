---
title: 'Quickstart: Woorden opzoeken in een tweetalige woordenlijst, Python - Translator Text-API'
titleSuffix: Azure Cognitive Services
description: In deze snelstart leert u hoe u alternatieve vertalingen en gebruiksvoorbeelden voor een bepaalde tekst kunt vinden met behulp van Python en de Translator Text-REST API.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 02/21/2019
ms.author: erhopf
ms.openlocfilehash: 55cb9564205c99abc868413ebf43e575999198ed
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58183693"
---
# <a name="quickstart-look-up-words-with-bilingual-dictionary-using-python"></a>Quickstart: Woorden opzoeken in een tweetalige woordenlijst met Python

In deze snelstart leert u hoe u alternatieve vertalingen en gebruiksvoorbeelden voor een bepaalde tekst kunt vinden met behulp van Python en de Translator Text-REST API.

Voor deze snelstart is een [Azure Cognitive Services-account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met een Translator Text-resource vereist. Als u geen account hebt, kunt u de [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/) gebruiken om een abonnementssleutel op te halen.

## <a name="prerequisites"></a>Vereisten

Voor deze snelstart zijn de volgende zaken vereist:

* Python 2.7.x of 3.x
* Een Azure-abonnementssleutel voor Translator Text

## <a name="create-a-project-and-import-required-modules"></a>Een project maken en de vereiste modules importeren

Maak een nieuwe Python-project met behulp van uw favoriete IDE of editor, of maak een nieuwe map op uw bureaublad. Dit codefragment kopiëren naar de projectmap in een bestand met de naam `dictionary-lookup.py`.

```python
# -*- coding: utf-8 -*-
import os, requests, uuid, json
```

> [!NOTE]
> Als u deze modules nog niet hebt gebruikt, moet u ze installeren voordat u het programma uitvoert. Voer voor het installeren van deze pakketten voert u `pip install requests uuid` uit.

Met de eerste opmerking laat u de Python-vertaler weten dat UTF-8-codering moet worden gebruikt. De vereiste modules worden dan geïmporteerd voor het lezen van uw abonnementssleutel uit een omgevingsvariabele, voor het opstellen van de HTTP-aanvraag, voor het maken van een unieke id en voor het verwerken van het JSON-antwoord dat door de Translator Text-API wordt geretourneerd.

## <a name="set-the-subscription-key-base-url-and-path"></a>De abonnementssleutel, de basis-URL en het pad instellen

In dit voorbeeld laat u de Translator Text-abonnementssleutel ophalen uit de omgevingsvariabele `TRANSLATOR_TEXT_KEY`. Als u niet bekend bent met omgevingsvariabelen, kunt u `subscriptionKey` als tekenreeks instellen en een opmerking plaatsen in de voorwaardelijke instructie.

Kopieer deze code naar uw project:

```python
# Checks to see if the Translator Text subscription key is available
# as an environment variable. If you are setting your subscription key as a
# string, then comment these lines out.
if 'TRANSLATOR_TEXT_KEY' in os.environ:
    subscriptionKey = os.environ['TRANSLATOR_TEXT_KEY']
else:
    print('Environment variable for TRANSLATOR_TEXT_KEY is not set.')
    exit()
# If you want to set your subscription key as a string, uncomment the line
# below and add your subscription key. Then, be sure to delete your "os" import.
# subscriptionKey = 'put_your_key_here'
```

Het globaal eindpunt voor de Translator Text is ingesteld als de `base_url`. Met `path` wordt de `dictionary/lookup`-route ingesteld en wordt bepaald dat we versie 3 van de API willen gebruiken.

De `params` worden gebruikt voor het instellen van de bron- en uitvoertalen. In dit voorbeeld wordt Engels en Spaans gebruikt: `en` en `es`.

>[!NOTE]
> Meer informatie over eindpunten, routes en aanvraagparameters vindt u in [Translator Text-API 3.0: Opzoeken in woordenlijst](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-dictionary-lookup).

```python
base_url = 'https://api.cognitive.microsofttranslator.com'
path = '/dictionary/lookup?api-version=3.0'
params = '&from=en&to=es';
constructed_url = base_url + path + params
```

## <a name="add-headers"></a>Headers toevoegen

U kunt aanvragen het eenvoudigst verifiëren door uw abonnementssleutel op te geven als `Ocp-Apim-Subscription-Key`-header. Dat doen we in dit voorbeeld dan ook. Als alternatief kunt u in plaats van uw abonnementssleutel een toegangstoken gebruiken en het toegangstoken opgeven als `Authorization`-header voor het valideren van uw aanvraag. Zie [Verificatie](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication) voor meer informatie.

Kopieer dit codefragment naar uw project:

```python
headers = {
    'Ocp-Apim-Subscription-Key': subscriptionKey,
    'Content-type': 'application/json',
    'X-ClientTraceId': str(uuid.uuid4())
}
```

## <a name="create-a-request-to-find-alternate-translations"></a>Een aanvraag maken om alternatieve vertalingen te zoeken

Definieer de tekenreeks (of tekenreeksen) die u wilt laten vertalen:

```python
# You can pass more than one object in body.
body = [{
    'text': 'Elephants'
}]
```

Vervolgens maakt u een POST-aanvraag met de `requests`-module. Hier zijn drie argumenten voor nodig: de samengevoegde URL, de aanvraagheaders en de aanvraagbody:

```python
request = requests.post(constructed_url, headers=headers, json=body)
response = request.json()
```

## <a name="print-the-response"></a>Het antwoord weergeven

De laatste stap is het weergeven van de antwoorden. Met dit codefragment worden de resultaten verfraaid: de sleutels worden gesorteerd, er wordt gebruikgemaakt van inspringing en er worden item- en sleutelscheidingstekens opgegeven.

```python
print(json.dumps(response, sort_keys=True, indent=4, ensure_ascii=False, separators=(',', ': ')))
```

## <a name="put-it-all-together"></a>Alles samenvoegen

Dat was het. U hebt een eenvoudig programma gemaakt dat we de Translator Text-API zullen noemen. Er is een JSON-antwoord geretourneerd. Het is nu tijd om uw programma uit te voeren:

```console
python alt-translations.py
```

Als u uw code graag wilt vergelijken met de onze, kunt u het volledige voorbeeld vinden op [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Python).

## <a name="sample-response"></a>Voorbeeldantwoord

```json
[
    {
        "displaySource": "elephants",
        "normalizedSource": "elephants",
        "translations": [
            {
                "backTranslations": [
                    {
                        "displayText": "elephants",
                        "frequencyCount": 1207,
                        "normalizedText": "elephants",
                        "numExamples": 5
                    }
                ],
                "confidence": 1.0,
                "displayTarget": "elefantes",
                "normalizedTarget": "elefantes",
                "posTag": "NOUN",
                "prefixWord": ""
            }
        ]
    }
]
```

## <a name="clean-up-resources"></a>Resources opschonen

Als u uw abonnementssleutel hebt vastgelegd in het programma, verwijdert u deze sleutel wanneer u klaar bent met de snelstart.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Python-voorbeelden op GitHub bekijken](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Python)

## <a name="see-also"></a>Zie ook

Meer informatie over het gebruik van de Translator Text-API voor:

* [Tekst vertalen](quickstart-python-translate.md)
* [Tekst transcriberen](quickstart-python-transliterate.md)
* [Een taal identificeren op basis van de invoer](quickstart-python-detect.md)
* [Een lijst ophalen van ondersteunde talen](quickstart-python-languages.md)
* [De zinlengte in invoer bepalen](quickstart-python-sentences.md)
