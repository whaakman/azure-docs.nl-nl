---
title: 'Snelstartgids: Een zoekopdracht op internet uitvoeren met C# - Bing Webzoekopdrachten-REST API'
titlesuffix: Azure Cognitive Services
description: Gebruik deze snelstartgids om een aanvraag naar de REST API van Bing News Search te verzenden via Java en een JSON-antwoord te ontvangen.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: quickstart
ms.date: 6/18/2019
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: 351bc71046df1bdd677d8c64411b6ae09dfc1ef1
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67206127"
---
# <a name="quickstart-perform-a-news-search-using-java-and-the-bing-news-search-rest-api"></a>Snelstartgids: Nieuws zoeken met Java en de REST API van Bing News Search

Gebruik deze quickstart om voor het eerst de Bing Nieuws zoeken-API aan te roepen en het JSON-antwoord te bekijken. Deze eenvoudige Java-toepassing stuurt een nieuwszoekquery naar de API en geeft het antwoord weer.

Hoewel deze toepassing in Java is geschreven, is de API een RESTful-webservice die compatibel is met vrijwel elke programmeertaal.

De broncode voor dit voorbeeld is beschikbaar [op GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/java/Search/BingNewsSearchv7.java) 

## <a name="prerequisites"></a>Vereisten

* De [Java Development Kit(JDK) 7 of 8](https://aka.ms/azure-jdks)

* De [Gson-bibliotheek](https://github.com/google/gson)


[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../includes/cognitive-services-bing-news-search-signup-requirements.md)]

Zie ook [Prijsinformatie Cognitive Services - Bing Zoeken-API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="create-and-initialize-a-project"></a>Een project maken en initialiseren

1. Maak een nieuw Java-project in uw favoriete IDE of editor en importeer de volgende bibliotheken.

    ```java
    import java.net.*;
    import java.util.*;
    import java.io.*;
    import javax.net.ssl.HttpsURLConnection;
    import com.google.gson.Gson;
    import com.google.gson.GsonBuilder;
    import com.google.gson.JsonObject;
    import com.google.gson.JsonParser;
    ```

2. Maak een nieuwe klasse, met variabelen voor het API-eindpunt, uw abonnementssleutel en zoekterm.

    ```java
    public static SearchResults SearchNews (String searchQuery) throws Exception {
        static String subscriptionKey = "enter key here";
        static String host = "https://api.cognitive.microsoft.com";
        static String path = "/bing/v7.0/news/search";
        static String searchTerm = "Microsoft";
    //...
    }
    ```

## <a name="construct-the-search-request-and-receive-a-json-response"></a>De zoekopdracht opbouwen en een JSON-antwoord ontvangen

1. Gebruik de variabelen uit de laatste stap om een zoek-URL voor de API-aanvraag te formatteren. Houd er rekening mee dat uw zoekterm URL-gecodeerd moet zijn voordat deze wordt toegevoegd aan de aanvraag.

    ```java
    public static SearchResults SearchNews (String searchQuery) throws Exception {
        // construct the search request URL (in the form of URL + query string)
        URL url = new URL(host + path + "?q=" +  URLEncoder.encode(searchQuery, "UTF-8"));
        HttpsURLConnection connection = (HttpsURLConnection)url.openConnection();
        connection.setRequestProperty("Ocp-Apim-Subscription-Key", subscriptionKey);
    }
    ```

2. Ontvang het JSON-antwoord van de Bing Nieuws zoeken-API, en bouw het resultaatobject op.

    ```java
    // receive JSON body
    InputStream stream = connection.getInputStream();
    String response = new Scanner(stream).useDelimiter("\\A").next();
    // construct result object for return
    SearchResults results = new SearchResults(new HashMap<String, String>(), response);
    ```

## <a name="process-the-json-response"></a>Het JSON-antwoord verwerken

1. Scheid de Bing-gerelateerde HTTP-headers van de JSON-hoofdtekst, sluit de stroom en retourneer het API-antwoord.
    ```java
    // extract Bing-related HTTP headers
    Map<String, List<String>> headers = connection.getHeaderFields();
    for (String header : headers.keySet()) {
        if (header == null) continue;      // may have null key
        if (header.startsWith("BingAPIs-") || header.startsWith("X-MSEdge-")) {
            results.relevantHeaders.put(header, headers.get(header).get(0));
        }
    }
    stream.close();
    return results;
    ```

2. Een methode maken om JSON te parseren en reserialiseren
    ```java
    // pretty-printer for JSON; uses GSON parser to parse and re-serialize
    public static String prettify(String json_text) {
        JsonParser parser = new JsonParser();
        JsonObject json = parser.parse(json_text).getAsJsonObject();
        Gson gson = new GsonBuilder().setPrettyPrinting().create();
        return gson.toJson(json);
    }
    ```

3. Roep de zoekmethode aan in de hoofdmethode van uw toepassing en geef de resultaten weer.
    ```csharp
   public static void main (String[] args) {
       System.out.println("Searching the Web for: " + searchTerm);
       SearchResults result = SearchNews(searchTerm);
    
       System.out.println("\nRelevant HTTP Headers:\n");
       for (String header : result.relevantHeaders.keySet())
             System.out.println(header + ": " + result.relevantHeaders.get(header));  
    System.out.println("\nJSON Response:\n");
    System.out.println(prettify(result.jsonResponse));
    }
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
> [Een web-app met één pagina maken](tutorial-bing-news-search-single-page-app.md)
