---
title: 'Quickstart: Uitvoeren van een nieuws zoeken met Ruby en de REST-API van Bing nieuws zoeken'
titlesuffix: Azure Cognitive Services
description: Gebruik deze snelstartgids om een aanvraag naar de REST API van Bing News Search te verzenden via Ruby en een JSON-antwoord te ontvangen.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: quickstart
ms.date: 6/19/2019
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: 5db8d9055ff973c3de1b995340d77004d889aa3d
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67272725"
---
# <a name="quickstart-perform-a-news-search-using-ruby-and-the-bing-news-search-rest-api"></a>Snelstartgids: Nieuws zoeken met Ruby en de REST API van Bing News Search

Gebruik deze quickstart om voor het eerst de Bing Nieuws zoeken-API aan te roepen en een JSON-antwoord te ontvangen. Met deze eenvoudige JavaScript-toepassing wordt een zoekquery naar de API verzonden en worden de resultaten verwerkt.

Hoewel deze toepassing in Python is geschreven, is de API een RESTful-webservice die compatibel is met vrijwel elke programmeertaal. De broncode voor dit voorbeeld is beschikbaar op [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/ruby/Search/BingNewsSearchv7.rb).

## <a name="prerequisites"></a>Vereisten

* Ruby [2.4 of later](https://www.ruby-lang.org/en/downloads/)

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../includes/cognitive-services-bing-news-search-signup-requirements.md)]

Zie ook [Prijsinformatie Cognitive Services - Bing Zoeken-API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="create-and-initialize-the-application"></a>De toepassing maken en initialiseren

1. importeer de volgende pakketten in uw codebestand.

    ```ruby
    require 'net/https'
    require 'uri'
    require 'json'
    ```

2. Maak variabelen voor het API-eindpunt, de Nieuws zoeken-URL, uw abonnementssleutel en de zoekterm.

    ```ruby
    accessKey = "enter key here"
    uri  = "https://api.cognitive.microsoft.com"
    path = "/bing/v7.0/news/search"
    term = "Microsoft"
    ```

## <a name="format-and-make-an-api-request"></a>Een API-aanvraag opmaken en maken

Gebruik de variabelen uit de laatste stap om een zoek-URL voor de API-aanvraag te formatteren. Verzend vervolgens de aanvraag.

```ruby
uri = URI(uri + path + "?q=" + URI.escape(term))
request = Net::HTTP::Get.new(uri)
request['Ocp-Apim-Subscription-Key'] = accessKey
response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
   http.request(request)
end
```

## <a name="process-and-print-the-json-response"></a>Het JSON-antwoord verwerken en afdrukken

Nadat het JSON-antwoord is ontvangen, kunt u het parseren en zowel de hoofdtekst van het antwoord afdrukken, als de headers:

```ruby
puts "\nRelevant Headers:\n\n"
response.each_header do |key, value|
   # header names are coerced to lowercase
   if key.start_with?("bingapis-") or key.start_with?("x-msedge-") then
      puts key + ": " + value
   end
end
puts "\nJSON Response:\n\n"
puts JSON::pretty_generate(JSON(response.body))
```

## <a name="json-response"></a>JSON-antwoord

Een geslaagd antwoord wordt geretourneerd in de JSON-indeling, zoals u kunt zien in het volgende voorbeeld:

```json
{
   "_type": "News",
   "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/news\/search?q=Microsoft",
   "totalEstimatedMatches": 36,
   "sort": [
      {
         "name": "Best match",
         "id": "relevance",
         "isSelected": true,
         "url": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/news\/search?q=Microsoft"
      },
      {
         "name": "Most recent",
         "id": "date",
         "isSelected": false,
         "url": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/news\/search?q=Microsoft&sortby=date"
      }
   ],
   "value": [
      {
         "name": "Microsoft to open flagship London brick-and-mortar retail store",
         "url": "http:\/\/www.contoso.com\/article\/microsoft-to-open-flagshi...",
         "image": {
            "thumbnail": {
               "contentUrl": "https:\/\/www.bing.com\/th?id=ON.F9E4A49EC010417...",
               "width": 220,
               "height": 146
            }
         },
         "description": "After years of rumors about Microsoft opening a brick-and-mortar...", 
         "about": [
           {
             "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entiti...", 
             "name": "Microsoft"
           }, 
           {
             "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entit...", 
             "name": "London"
           }
         ], 
         "provider": [
           {
             "_type": "Organization", 
             "name": "Contoso"
           }
         ], 
          "datePublished": "2017-09-21T21:16:00.0000000Z", 
          "category": "ScienceAndTechnology"
      }, 

      . . .
      
      {
         "name": "Microsoft adds Availability Zones to its Azure cloud platform",
         "url": "https:\/\/contoso.com\/2017\/09\/21\/microsoft-adds-availability...",
         "image": {
            "thumbnail": {
               "contentUrl": "https:\/\/www.bing.com\/th?id=ON.0AE7595B9720...",
               "width": 700,
               "height": 466
            }
         },
         "description": "Microsoft has begun adding Availability Zones to its...",
         "about": [
            {
               "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entities\/a093e9b...",
               "name": "Microsoft"
            },
            {
               "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entities\/cf3abf7d-e379-...",
               "name": "Windows Azure"
            },
            {
               "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entities\/9cdd061c-1fae-d0...",
               "name": "Cloud"
            }
         ],
         "provider": [
            {
               "_type": "Organization",
               "name": "Contoso"
            }
         ],
         "datePublished": "2017-09-21T09:01:00.0000000Z",
         "category": "ScienceAndTechnology"
      }
   ]
}
```

 
## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een app met één pagina maken](tutorial-bing-news-search-single-page-app.md)
