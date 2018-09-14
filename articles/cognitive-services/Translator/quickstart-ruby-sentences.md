---
title: Zinslengten ophalen in Translator Text met Ruby | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: In deze snelstartgids vindt u de lengte van zinnen in tekst met behulp van de Translator Text-API met Ruby in Cognitive Services.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 06/22/2018
ms.author: nolachar
ms.openlocfilehash: 3a4fc999961e06b084a0d7da42ed5576962e5722
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "43769036"
---
# <a name="quickstart-get-sentence-lengths-with-ruby"></a>Snelstartgids: zinslengten ophalen met Ruby

In deze snelstartgids vindt u de lengte van zinnen in tekst met behulp van de Translator Text-API.

## <a name="prerequisites"></a>Vereisten

U hebt [Ruby 2.4](https://www.ruby-lang.org/en/downloads/) of hoger nodig om deze code uit te voeren.

Als u de Translator Text-API wilt gebruiken, moet u ook een abonnementssleutel hebben. Zie [Hoe u zich registreert voor de Translator Text-API](translator-text-how-to-signup.md).

## <a name="breaksentence-request"></a>BreakSentence-aanvraag

Met de volgende code wordt de brontekst in zinnen verdeeld met behulp van de methode [BreakSentence](./reference/v3-0-break-sentence.md).

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
path = '/breaksentence?api-version=3.0'

uri = URI (host + path)

text = 'How are you? I am fine. What did you do today?'

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

## <a name="breaksentence-response"></a>BreakSentence-antwoord

Een geslaagd antwoord wordt geretourneerd in de JSON-indeling, zoals u in het volgende voorbeeld kunt zien:

```json
[
  {
    "detectedLanguage": {
      "language": "en",
      "score": 1.0
    },
    "sentLen": [
      13,
      11,
      22
    ]
  }
]
```

## <a name="next-steps"></a>Volgende stappen

Bekijk de voorbeeldcode voor deze snelstartgids en andere, zoals vertaling en transliteratie, evenals andere Translator Text-voorbeeldprojecten op GitHub.

> [!div class="nextstepaction"]
> [Bekijk Ruby-voorbeelden op GitHub](https://aka.ms/TranslatorGitHub?type=&language=ruby)
