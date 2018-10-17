---
title: 'Snelstart: taal bepalen op basis van tekst - Translator Text, Ruby'
titleSuffix: Azure Cognitive Services
description: In deze snelstart bepaalt u de taal van de brontekst met behulp van de Translator Text-API met Ruby.
services: cognitive-services
author: noellelacharite
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 06/22/2018
ms.author: nolachar
ms.openlocfilehash: e2b138b5c28047487da2ca7a72169a10a125eaf5
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46121700"
---
# <a name="quickstart-identify-language-from-text-with-ruby"></a>Snelstartgids: taal bepalen op basis van tekst met Ruby

In deze snelstartgids bepaalt u de taal van de brontekst met behulp van de Translator Text-API.

## <a name="prerequisites"></a>Vereisten

U hebt [Ruby 2.4](https://www.ruby-lang.org/en/downloads/) of hoger nodig om deze code uit te voeren.

Als u de Translator Text-API wilt gebruiken, moet u ook een abonnementssleutel hebben. Zie [Hoe u zich registreert voor de Translator Text-API](translator-text-how-to-signup.md).

## <a name="detect-request"></a>Detect-aanvraag

Met de volgende code bepaalt u de taal van de brontekst met de methode [Detect](./reference/v3-0-detect.md).

1. Maak een nieuw Ruby-project in uw favoriete code-editor.
2. Voeg de onderstaande code toe.
3. Vervang de waarde `key` door een geldige toegangssleutel voor uw abonnement.
4. Voer het programma uit.

```ruby
require 'net/https'
require 'uri'
require 'cgi'
require 'json'
require 'securerandom'

# **********************************************
# *** Update or verify the following values. ***
# **********************************************

# Replace the key string value with your valid subscription key.
key = 'ENTER KEY HERE'

host = 'https://api.cognitive.microsofttranslator.com'
path = '/detect?api-version=3.0'

uri = URI (host + path)

text = 'Salve, mondo!'

content = '[{"Text" : "' + text + '"}]'

request = Net::HTTP::Post.new(uri)
request['Content-type'] = 'application/json'
request['Content-length'] = content.length
request['Ocp-Apim-Subscription-Key'] = key
request['X-ClientTraceId'] = SecureRandom.uuid
request.body = content

response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
    http.request (request)
end

result = response.body.force_encoding("utf-8")

json = JSON.pretty_generate(JSON.parse(result))
puts json
```

## <a name="detect-response"></a>Detect-antwoord

Een geslaagd antwoord wordt geretourneerd in de JSON-indeling, zoals u in het volgende voorbeeld kunt zien:

```json
[
  {
    "language": "it",
    "score": 1.0,
    "isTranslationSupported": true,
    "isTransliterationSupported": false,
    "alternatives": [
      {
        "language": "pt",
        "score": 1.0,
        "isTranslationSupported": true,
        "isTransliterationSupported": false
      },
      {
        "language": "en",
        "score": 1.0,
        "isTranslationSupported": true,
        "isTransliterationSupported": false
      }
    ]
  }
]
```

## <a name="next-steps"></a>Volgende stappen

Bekijk de voorbeeldcode voor deze snelstartgids en andere, zoals vertaling en transliteratie, evenals andere Translator Text-voorbeeldprojecten op GitHub.

> [!div class="nextstepaction"]
> [Bekijk Ruby-voorbeelden op GitHub](https://aka.ms/TranslatorGitHub?type=&language=ruby)
