---
title: 'Snelstartgids: Bing Entiteiten zoeken-API, Ruby'
titlesuffix: Azure Cognitive Services
description: Bekijk informatie en codevoorbeelden om snel aan de slag te gaan met de Bing Entiteiten zoeken-API.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: quickstart
ms.date: 11/28/2017
ms.author: aahi
ms.openlocfilehash: d0ee5aa69c0431600604c993bc8cde75a9f17c45
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2018
ms.locfileid: "52314146"
---
# <a name="quickstart-for-bing-entity-search-api-with-ruby"></a>Snelstartgids: Bing Entiteiten zoeken-API met Ruby

In dit artikel leest u hoe u de [Bing Entiteiten zoeken](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/search-the-web) -API gebruikt met Ruby.

## <a name="prerequisites"></a>Vereisten

U hebt [Ruby 2.4](https://www.ruby-lang.org/en/downloads/) of hoger nodig om deze code uit te voeren.

U moet beschikken over een [account voor de Cognitive Services-API](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met de **Bing Entiteiten zoeken-API**. De [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/?api=bing-entity-search-api) is voldoende voor deze snelstartgids. U hebt de toegangssleutel nodig die wordt verstrekt bij het activeren van uw gratis proefversie of u gebruikt de sleutel van een betaald abonnement vanuit uw Azure-dashboard.   Zie ook [Prijsinformatie Cognitive Services - Bing Zoeken-API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="search-entities"></a>Entiteiten zoeken

Volg deze stappen om deze toepassing uit te voeren.

1. Maak een nieuw Ruby-project in uw favoriete IDE.
2. Voeg de onderstaande code toe.
3. Vervang de waarde `key` door een geldige toegangssleutel voor uw abonnement.
4. Voer het programma uit.

```ruby
require 'net/https'
require 'cgi'
require 'json'

# **********************************************
# *** Update or verify the following values. ***
# **********************************************

# Replace the subscriptionKey string value with your valid subscription key.
subscriptionKey = 'ENTER KEY HERE'

host = 'https://api.cognitive.microsoft.com'
path = '/bing/v7.0/entities'

mkt = 'en-US'
query = 'italian restaurants near me'

params = '?mkt=' + mkt + '&q=' + CGI.escape(query)
uri = URI (host + path + params)

request = Net::HTTP::Get.new(uri)
request['Ocp-Apim-Subscription-Key'] = subscriptionKey

response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
    http.request (request)
end

puts JSON::pretty_generate (JSON (response.body))
```

**Antwoord**

Een geslaagd antwoord wordt geretourneerd in de JSON-indeling, zoals u kunt zien in het volgende voorbeeld: 

```json
{
  "_type": "SearchResponse",
  "queryContext": {
    "originalQuery": "italian restaurant near me",
    "askUserForLocation": true
  },
  "places": {
    "value": [
      {
        "_type": "LocalBusiness",
        "webSearchUrl": "https://www.bing.com/search?q=sinful+bakery&filters=local...",
        "name": "Liberty's Delightful Sinful Bakery & Cafe",
        "url": "https://www.contoso.com/",
        "entityPresentationInfo": {
          "entityScenario": "ListItem",
          "entityTypeHints": [
            "Place",
            "LocalBusiness"
          ]
        },
        "address": {
          "addressLocality": "Seattle",
          "addressRegion": "WA",
          "postalCode": "98112",
          "addressCountry": "US",
          "neighborhood": "Madison Park"
        },
        "telephone": "(800) 555-1212"
      },

      . . .
      {
        "_type": "Restaurant",
        "webSearchUrl": "https://www.bing.com/search?q=Pickles+and+Preserves...",
        "name": "Munson's Pickles and Preserves Farm",
        "url": "http://www.princi.com/",
        "entityPresentationInfo": {
          "entityScenario": "ListItem",
          "entityTypeHints": [
            "Place",
            "LocalBusiness",
            "Restaurant"
          ]
        },
        "address": {
          "addressLocality": "Seattle",
          "addressRegion": "WA",
          "postalCode": "98101",
          "addressCountry": "US",
          "neighborhood": "Capitol Hill"
        },
        "telephone": "(800) 555-1212"
      },
      
      . . .
    ]
  }
}
```

[Terug naar boven](#HOLTop)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zelfstudie over Bing Entiteiten zoeken](../tutorial-bing-entities-search-single-page-app.md)
> [Overzicht van Bing Entiteiten zoeken](../search-the-web.md )
> [API-handleiding](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference)
