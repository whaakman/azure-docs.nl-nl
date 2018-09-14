---
title: "Snelstartgids: Verzenden zoekquery's met de Bing afbeeldingen zoeken-API en Java"
titleSuffix: Azure Cognitive Services
description: Gebruik deze Quick Start om te zoeken en -installatiekopieën zoeken op Internet met behulp van de Bing webzoekopdrachten-API.
services: cognitive-services
documentationcenter: ''
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 8/20/2018
ms.author: aahi
ms.openlocfilehash: 519667af255e3c1b39d336e7a399ba215803daa5
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45577476"
---
# <a name="quickstart-send-search-queries-using-the-bing-image-search-rest-api-and-java"></a>Snelstartgids: Verzenden zoekquery's met behulp van de Bing afbeeldingen zoeken-REST-API en Java

Gebruik deze Quick Start voor het maken van de eerste aanroep van de Bing afbeeldingen zoeken-API en een JSON-antwoord ontvangen. Deze eenvoudige Java-toepassing een zoekquery verzendt naar de API en de onbewerkte resultaten worden weergegeven.

Terwijl deze toepassing is geschreven in Java, de API is een RESTful-Web-compatibel is met de meeste moderne programmeertalen.

De broncode voor dit voorbeeld is beschikbaar [op GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/java/Search/BingImageSearchv7Quickstart.java) met extra foutafhandeling en code-aantekeningen.

## <a name="prerequisites"></a>Vereisten

* De [Java Development Kit(JDK) 7 of 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 

* De [Gson-bibliotheek](https://github.com/google/gson)

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="create-and-initialize-a-project"></a>Maken en een project initialiseren

1. Maak een nieuwe Java-project in uw favoriete IDE of editor en importeer de volgende bibliotheken.

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

2. Variabelen voor de API-eindpunt, uw abonnementssleutel maken en zoekterm.

    ```java
    static String subscriptionKey = "enter key here";
    static String host = "https://api.cognitive.microsoft.com";
    static String path = "/bing/v7.0/images/search";
    static String searchTerm = "tropical ocean";
    ```

## <a name="construct-the-search-request-and-query"></a>De zoekopdracht en een query maken

1. Gebruik de variabelen in de vorige stap om een URL zoeken voor de API-aanvraag. Houd er rekening mee dat de zoekterm URL gecodeerd moet voordat het wordt toegevoegd aan de aanvraag.

    ```java
    // construct the search request URL (in the form of endpoint + query string)
    URL url = new URL(host + path + "?q=" +  URLEncoder.encode(searchQuery, "UTF-8"));
    HttpsURLConnection connection = (HttpsURLConnection)url.openConnection();
    connection.setRequestProperty("Ocp-Apim-Subscription-Key", subscriptionKey);
    ```

## <a name="receive-and-process-the-json-response"></a>Ontvangen en verwerken van het JSON-antwoord

1. De JSON-antwoord ontvangen van de Bing afbeeldingen zoeken-API, en bouw het resultaatobject.

    ```java
    // receive JSON body
    InputStream stream = connection.getInputStream();
    String response = new Scanner(stream).useDelimiter("\\A").next();
    // construct result object for return
    SearchResults results = new SearchResults(new HashMap<String, String>(), response);
    ```
2. De Bing-gerelateerde HTTP-headers te scheiden van de JSON-hoofdtekst
    ```java
    // extract Bing-related HTTP headers
    Map<String, List<String>> headers = connection.getHeaderFields();
    for (String header : headers.keySet()) {
        if (header == null) continue;      // may have null key
        if (header.startsWith("BingAPIs-") || header.startsWith("X-MSEdge-")) {
            results.relevantHeaders.put(header, headers.get(header).get(0));
        }
    }
    ```

3. Sluit de stroom en parseren van het antwoord. Ophalen van het totale aantal geretourneerde zoekresultaten en de miniatuur url naar de eerste afbeelding-resultaat. 

    ```java
    stream.close();
    JsonParser parser = new JsonParser();
    JsonObject json = parser.parse(result.jsonResponse).getAsJsonObject();
    //get the first image result from the JSON object, along with the total 
    //number of images returned by the Bing Image Search API. 
    String total = json.get("totalEstimatedMatches").getAsString();
    JsonArray results = json.getAsJsonArray("value");
    JsonObject first_result = (JsonObject)results.get(0);
    String resultURL = first_result.get("thumbnailUrl").getAsString();
    ```
4. Vergeet niet uw abonnementssleutel verwijderen uit de toepassingscode.

## <a name="json-response"></a>JSON-antwoord

Antwoorden van de Bing afbeeldingen zoeken-API worden geretourneerd als JSON. Dit voorbeeldantwoord is ingekort zodat één resultaat wordt weergegeven.

```json
{
"_type":"Images",
"instrumentation":{
    "_type":"ResponseInstrumentation"
},
"readLink":"images\/search?q=tropical ocean",
"webSearchUrl":"https:\/\/www.bing.com\/images\/search?q=tropical ocean&FORM=OIIARP",
"totalEstimatedMatches":842,
"nextOffset":47,
"value":[
    {
        "webSearchUrl":"https:\/\/www.bing.com\/images\/search?view=detailv2&FORM=OIIRPO&q=tropical+ocean&id=8607ACDACB243BDEA7E1EF78127DA931E680E3A5&simid=608027248313960152",
        "name":"My Life in the Ocean | The greatest WordPress.com site in ...",
        "thumbnailUrl":"https:\/\/tse3.mm.bing.net\/th?id=OIP.fmwSKKmKpmZtJiBDps1kLAHaEo&pid=Api",
        "datePublished":"2017-11-03T08:51:00.0000000Z",
        "contentUrl":"https:\/\/mylifeintheocean.files.wordpress.com\/2012\/11\/tropical-ocean-wallpaper-1920x12003.jpg",
        "hostPageUrl":"https:\/\/mylifeintheocean.wordpress.com\/",
        "contentSize":"897388 B",
        "encodingFormat":"jpeg",
        "hostPageDisplayUrl":"https:\/\/mylifeintheocean.wordpress.com",
        "width":1920,
        "height":1200,
        "thumbnail":{
        "width":474,
        "height":296
        },
        "imageInsightsToken":"ccid_fmwSKKmK*mid_8607ACDACB243BDEA7E1EF78127DA931E680E3A5*simid_608027248313960152*thid_OIP.fmwSKKmKpmZtJiBDps1kLAHaEo",
        "insightsMetadata":{
        "recipeSourcesCount":0,
        "bestRepresentativeQuery":{
            "text":"Tropical Beaches Desktop Wallpaper",
            "displayText":"Tropical Beaches Desktop Wallpaper",
            "webSearchUrl":"https:\/\/www.bing.com\/images\/search?q=Tropical+Beaches+Desktop+Wallpaper&id=8607ACDACB243BDEA7E1EF78127DA931E680E3A5&FORM=IDBQDM"
        },
        "pagesIncludingCount":115,
        "availableSizesCount":44
        },
        "imageId":"8607ACDACB243BDEA7E1EF78127DA931E680E3A5",
        "accentColor":"0050B2"
    }
}
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zelfstudie voor Bing afbeeldingen zoeken-app met één pagina](../tutorial-bing-image-search-single-page-app.md)

## <a name="see-also"></a>Zie ook 

* [Wat is de Bing afbeeldingen zoeken?](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/overview)  
* [Probeer een online interactieve demo](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
* [Een gratis Cognitive Services-toegangssleutel ophalen](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api)  
* [Documentatie voor Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services)
* [Bing afbeeldingen zoeken-API-verwijzing](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)