---
title: 'Quickstart: Bing Spellingcontrole-API, Ruby'
titlesuffix: Azure Cognitive Services
description: Haal informatie en codevoorbeelden op om u te helpen snel aan de slag te gaan met de Bing Spellingcontrole-API.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: quickstart
ms.date: 09/14/2017
ms.author: aahi
ms.openlocfilehash: 3371ed7689ad089559a5874fb89f14962194194d
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55203216"
---
# <a name="quickstart-for-bing-spell-check-api-with-ruby"></a>Quickstart voor Bing Spellingcontrole-API met Ruby 

In dit artikel leest u hoe u de [Bing Spellingcontrole-API](https://azure.microsoft.com/services/cognitive-services/spell-check/) kunt gebruiken in combinatie met Ruby. De Spellingcontrole-API retourneert een lijst met woorden die niet worden herkend, samen met voorgestelde vervangingen. Normaal gesproken zou u tekst naar deze API sturen en vervolgens de voorgestelde vervangingen in de tekst doorvoeren of deze weergeven aan de gebruikers van uw toepassing, zodat ze zelf kunnen beslissen of de vervangingen moeten worden doorgevoerd. Dit artikel laat zien hoe een aanvraag met de tekst "Hollo, wrld!" wordt verzonden. De voorgestelde vervangingen zijn "Hello" en "world".

## <a name="prerequisites"></a>Vereisten

U hebt [Ruby 2.4](https://www.ruby-lang.org/en/downloads/) of hoger nodig om deze code uit te voeren.

U moet een [Cognitive Services-API-account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met **Bing Spellingcontrole-API voor Bing versie 7** hebben. De [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/#lang) is voldoende voor deze quickstart. U hebt de toegangssleutel nodig die wordt verstrekt bij het activeren van uw gratis proefversie of u gebruikt de sleutel van een betaald abonnement vanuit uw Azure-dashboard. Zie ook [Prijsinformatie Cognitive Services - Bing Zoeken-API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="get-spell-check-results"></a>Resultaten van spellingcontrole ophalen

1. Maak een nieuw Ruby-project in uw favoriete IDE.
2. Voeg de onderstaande code toe.
3. Vervang de waarde `key` door een geldige toegangssleutel voor uw abonnement.
4. Voer het programma uit.

```ruby
require 'net/http'
require 'uri'
require 'json'

uri = 'https://api.cognitive.microsoft.com'
path = '/bing/v7.0/spellcheck?'
params = 'mkt=en-us&mode=proof'

uri = URI(uri + path + params)
uri.query = URI.encode_www_form({
    # Request parameters
    'text' => 'Hollo, wrld!'
})

# NOTE: Replace this example key with a valid subscription key.
key = 'ENTER KEY HERE'

# The headers in the following example 
# are optional but should be considered as required:
#
# X-MSEdge-ClientIP: 999.999.999.999  
# X-Search-Location: lat: +90.0000000000000;long: 00.0000000000000;re:100.000000000000
# X-MSEdge-ClientID: <Client ID from Previous Response Goes Here>
#
# See below for more information.

request = Net::HTTP::Post.new(uri)
request['Content-Type'] = "application/x-www-form-urlencoded"

request['Ocp-Apim-Subscription-Key'] = key

response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
    http.request(request)
end

result = JSON.pretty_generate(JSON.parse(response.body))
puts result
```

**Antwoord**

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
> [Zelfstudie over Bing Spellingcontrole](../tutorials/spellcheck.md)

## <a name="see-also"></a>Zie ook

- [Overzicht van Bing Spellingcontrole](../proof-text.md)
- [Referentie voor de Bing Spellingcontrole-API v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference)
