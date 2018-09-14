---
title: Tekst vertalen van Translator Text met Python | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: In deze snelstart vertaalt u tekst vanuit één taal naar een andere taal met de Translator Text-API met Python in Cognitive Services.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 06/21/2018
ms.author: nolachar
ms.openlocfilehash: 8f70ffb77e21131990d6b77a1cb13c9d5c054d06
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2018
ms.locfileid: "43768991"
---
# <a name="quickstart-translate-text-with-python"></a>Snelstart: tekst vertalen met Python

In deze snelstart vertaalt u tekst vanuit één taal naar een andere taal met de Translator Text-API.

## <a name="prerequisites"></a>Vereisten

U hebt [Python 3.x](https://www.python.org/downloads/) om deze code uit te voeren.

Als u de Translator Text-API wilt gebruiken, moet u ook een abonnementssleutel hebben. Zie [Hoe u zich registreert voor de Translator Text-API](translator-text-how-to-signup.md).

## <a name="translate-request"></a>Translate-aanvraag

Met de volgende code wordt brontekst vertaald vanuit één taal naar een andere taal met de methode [Translate](./reference/v3-0-translate.md).

1. Maak een nieuw Python-project in uw favoriete code-editor.
2. Voeg de onderstaande code toe.
3. Vervang de waarde `subscriptionKey` door een geldige toegangssleutel voor uw abonnement.
4. Voer het programma uit.

```python
# -*- coding: utf-8 -*-

import http.client, urllib.parse, uuid, json

# **********************************************
# *** Update or verify the following values. ***
# **********************************************

# Replace the subscriptionKey string value with your valid subscription key.
subscriptionKey = 'ENTER KEY HERE'

host = 'api.cognitive.microsofttranslator.com'
path = '/translate?api-version=3.0'

# Translate to German and Italian.
params = "&to=de&to=it";

text = 'Hello, world!'

def translate (content):

    headers = {
        'Ocp-Apim-Subscription-Key': subscriptionKey,
        'Content-type': 'application/json',
        'X-ClientTraceId': str(uuid.uuid4())
    }

    conn = http.client.HTTPSConnection(host)
    conn.request ("POST", path + params, content, headers)
    response = conn.getresponse ()
    return response.read ()

requestBody = [{
    'Text' : text,
}]
content = json.dumps(requestBody, ensure_ascii=False).encode('utf-8')
result = translate (content)

# Note: We convert result, which is JSON, to and from an object so we can pretty-print it.
# We want to avoid escaping any Unicode characters that result contains. See:
# https://stackoverflow.com/questions/18337407/saving-utf-8-texts-in-json-dumps-as-utf8-not-as-u-escape-sequence
output = json.dumps(json.loads(result), indent=4, ensure_ascii=False)

print (output)
```

## <a name="translate-response"></a>Translate-antwoord

Een geslaagd antwoord wordt geretourneerd in de JSON-indeling, zoals u in het volgende voorbeeld kunt zien:

```json
[
  {
    "detectedLanguage": {
      "language": "en",
      "score": 1.0
    },
    "translations": [
      {
        "text": "Hallo Welt!",
        "to": "de"
      },
      {
        "text": "Salve, mondo!",
        "to": "it"
      }
    ]
  }
]
```

## <a name="next-steps"></a>Volgende stappen

Bekijk de voorbeeldcode voor deze snelstart en andere resources, met inbegrip van transliteratie en taalidentificatie, evenals andere Translator Text-voorbeeldprojecten op GitHub.

> [!div class="nextstepaction"]
> [Python-voorbeelden op GitHub bekijken](https://aka.ms/TranslatorGitHub?type=&language=python)
