---
title: Het schrift van tekst converteren in Translator Text met Python | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: In deze snelstart converteert u tekst in één taal van het ene schrift naar het andere met de Translator Text-API met Python in Cognitive Services.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 06/21/2018
ms.author: nolachar
ms.openlocfilehash: 41fb0f72c5974a1ab034680a820dca6aa7bbdc6d
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2018
ms.locfileid: "43768996"
---
# <a name="quickstart-transliterate-text-with-python"></a>Snelstart: Tekst translitereren met Python

In deze snelstart converteert u tekst in één taal van het ene schrift naar het andere met de Translator Text-API.

## <a name="prerequisites"></a>Vereisten

U hebt [Python 3.x](https://www.python.org/downloads/) nodig om deze code uit te voeren.

Als u de Translator Text-API wilt gebruiken, moet u ook een abonnementssleutel hebben. Lees hoe u zich kunt [registreren voor de Translator Text-API](translator-text-how-to-signup.md).

## <a name="transliterate-request"></a>Transliterate-aanvraag

Het volgende converteert tekst in één taal van het ene schrift naar het andere met behulp van de methode [Transliterate](./reference/v3-0-transliterate.md).

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
path = '/transliterate?api-version=3.0'

# Transliterate text in Japanese from Japanese script (i.e. Hiragana/Katakana/Kanji) to Latin script.
params = '&language=ja&fromScript=jpan&toScript=latn';

# Transliterate "good afternoon".
text = 'こんにちは'

def transliterate (content):

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
result = transliterate (content)

# Note: We convert result, which is JSON, to and from an object so we can pretty-print it.
# We want to avoid escaping any Unicode characters that result contains. See:
# https://stackoverflow.com/questions/18337407/saving-utf-8-texts-in-json-dumps-as-utf8-not-as-u-escape-sequence
output = json.dumps(json.loads(result), indent=4, ensure_ascii=False)

print (output)
```

## <a name="transliterate-response"></a>Transliterate-antwoord

Een geslaagd antwoord wordt geretourneerd in de JSON-indeling, zoals u in het volgende voorbeeld kunt zien:

```json
[
  {
    "text": "konnnichiha",
    "script": "latn"
  }
]
```

## <a name="next-steps"></a>Volgende stappen

Bekijk de voorbeeldcode voor deze snelstart en andere resources, met inbegrip van vertaling en taalidentificatie, evenals andere Translator Text-voorbeeldprojecten op GitHub.

> [!div class="nextstepaction"]
> [Python-voorbeelden op GitHub bekijken](https://aka.ms/TranslatorGitHub?type=&language=python)
