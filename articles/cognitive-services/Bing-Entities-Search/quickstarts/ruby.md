---
title: Ruby Quick Start voor Azure cognitieve Services, Bing entiteit Search API | Microsoft Docs
description: Get-informatie en codevoorbeelden om u te helpen snel aan de slag met de Bing entiteit zoeken-API in Microsoft cognitieve Services in Azure.
services: cognitive-services
documentationcenter: ''
author: v-jaswel
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: article
ms.date: 11/28/2017
ms.author: v-jaswel
ms.openlocfilehash: 0b47430886e70421e51438b56decc9b86d88d0fb
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345559"
---
# <a name="quickstart-for-microsoft-bing-entity-search-api-with-ruby"></a>Quick Start voor Microsoft Bing entiteit zoekopdracht API met Ruby 
<a name="HOLTop"></a>

Dit artikel laat zien hoe u de [Bing entiteit zoeken](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/search-the-web) API met Ruby.

## <a name="prerequisites"></a>Vereisten

U moet [Ruby 2.4](https://www.ruby-lang.org/en/downloads/) of later uit te voeren deze code.

U moet hebben een [cognitieve Services API account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met **Bing-API van zoekservice entiteit**. De [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/?api=bing-entity-search-api) voldoende is voor deze snelstartgids. U moet de toegangssleutel die is opgegeven bij het activeren van uw gratis proefversie of u kunt de sleutel van een betaald abonnement van uw Azure-dashboard.

## <a name="search-entities"></a>Entiteiten zoeken

Volg deze stappen voor het uitvoeren van deze toepassing.

1. Maak een nieuwe Ruby project in uw favoriete IDE.
2. Voeg de code hieronder.
3. Vervang de `key` waarde met een geldige toegangssleutel voor uw abonnement.
4. Voer het programma.

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

Een geslaagde reactie wordt geretourneerd als JSON, zoals wordt weergegeven in het volgende voorbeeld: 

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
> [Bing entiteit Search-zelfstudie](../tutorial-bing-entities-search-single-page-app.md)
> [Bing entiteit zoeken overzicht](../search-the-web.md )
> [API-referentiemateriaal](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference)
