---
title: Java-Quick Start voor het voorbeeld in de Project-URL - cognitieve Microsoft-Services | Microsoft Docs
description: Het voorbeeldscript aan de slag met behulp van de evaluatieversie van de URL-Project in Microsoft cognitieve Services in Azure.
services: cognitive-services
author: mikedodaro
ms.service: cognitive-services
ms.technology: project-url-preview
ms.topic: article
ms.date: 04/24/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 2de74f48882605bfcf05f65723ba5d8993587f51
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345290"
---
# <a name="url-preview-java-quickstart"></a>Quick Start-URL Preview Java

Het volgende Java-voorbeeld wordt een voorbeeld-Url voor de website van SwiftKey: https://swiftkey.com/en.

## <a name="prerequisites"></a>Vereisten

Ophalen van een toegangssleutel voor de gratis proefversie [cognitieve Services Labs](https://aka.ms/answersearchsubscription)

## <a name="request"></a>Aanvragen 

De volgende code maakt een `WebRequest`, stelt u de koptekst van de toegang en voegt u een queryreeks voor 'https://swiftkey.com/en'.  Vervolgens stuurt de aanvraag en wijst het antwoord op een tekenreeks op voor de JSON-tekst bevatten.

````
    // construct URL of search request (endpoint + query string)

    static String host = "https://api.labs.cognitive.microsoft.com";
    static String path = "/urlpreview/v7.0/search";

    static String searchTerm = "https://swiftkey.com/en";

    URL url = new URL(host + path + "?q=" +  URLEncoder.encode(searchQuery, "UTF-8") + &mkt=en-us");
    HttpsURLConnection connection = (HttpsURLConnection)url.openConnection();
    connection.setRequestProperty("Ocp-Apim-Subscription-Key", subscriptionKey);

    // receive JSON body
    InputStream stream = connection.getInputStream();
    String response = new Scanner(stream).useDelimiter("\\A").next();

    // construct result object for return
    SearchResults results = new SearchResults(new HashMap<String, String>(), response);
````

## <a name="complete-code"></a>Volledige code

De Bing antwoord zoeken-API retourneert resultaten van de zoekmachine Bing.
1. Downloaden en installeren van de bibliotheek gson.
2. Maak een nieuwe Java-project in uw favoriete IDE of -editor.
3. Voeg de code hieronder.
4. De waarde subscriptionKey vervangen door een geldig toegangssleutel voor uw abonnement.
5. Voer het programma.

````
package UrlPreviewpkg;

import java.net.*;
import java.util.*;
import java.io.*;
import javax.net.ssl.HttpsURLConnection;

import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;

/*
 * Gson: https://github.com/google/gson
 * Maven info:
 *     groupId: com.google.code.gson
 *     artifactId: gson
 *     version: 2.8.1
 *
 * When you have compiled or downloaded gson-2.8.1.jar and have placed it in the
 * same folder as this file (BingImageSearch.java), you can compile and run this program at
 * the command line as follows.
 *
 * javac UrlPreviewcls.java -classpath .;gson-2.8.1.jar -encoding UTF-8
 * java -cp .;gson-2.8.1.jar UrlPreview
 */

public class UrlPreview 
{
    // Replace the subscriptionKey string value with your valid subscription key.
    static String subscriptionKey = "YOUR-ACCESS-KEY";

    static String host = "https://api.labs.cognitive.microsoft.com";
    static String path = "/urlpreview/v7.0/search";

    static String searchTerm = "https://swiftkey.com/en";

    public static SearchResults SearchImages (String searchQuery) throws Exception {
        // construct URL of search request (endpoint + query string)
        URL url = new URL(host + path + "?q=" +  URLEncoder.encode(searchQuery, "UTF-8"));
        HttpsURLConnection connection = (HttpsURLConnection)url.openConnection();
        connection.setRequestProperty("Ocp-Apim-Subscription-Key", subscriptionKey);

        // receive JSON body
        InputStream stream = connection.getInputStream();
        String response = new Scanner(stream).useDelimiter("\\A").next();

        // construct result object for return
        SearchResults results = new SearchResults(new HashMap<String, String>(), response);

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
    }

    // pretty-printer for JSON; uses GSON parser to parse and re-serialize
    public static String prettify(String json_text) {
        JsonParser parser = new JsonParser();
        JsonObject json = parser.parse(json_text).getAsJsonObject();
        Gson gson = new GsonBuilder().setPrettyPrinting().create();
        return gson.toJson(json);
    }

    public static void main (String[] args) {
        if (subscriptionKey.length() != 32) {
            System.out.println("Invalid Bing Search API subscription key!");
            System.out.println("Please paste yours into the source code.");
            System.exit(1);
        }

        try {
            System.out.println("Searching the Web for: " + searchTerm);

            SearchResults result = SearchImages(searchTerm);

            System.out.println("\nRelevant HTTP Headers:\n");
            for (String header : result.relevantHeaders.keySet())
                System.out.println(header + ": " + result.relevantHeaders.get(header));

            System.out.println("\nJSON Response:\n");
            System.out.println(prettify(result.jsonResponse));
        }
        catch (Exception e) {
            e.printStackTrace(System.out);
            System.exit(1);
        }
    }
}
//Container class for search results encapsulates relevant headers and JSON data
class SearchResults{
 HashMap<String, String> relevantHeaders;
 String jsonResponse;
 SearchResults(HashMap<String, String> headers, String json) {
     relevantHeaders = headers;
     jsonResponse = json;
 }
}

````

## <a name="next-steps"></a>Volgende stappen
- [Quick Start C#](csharp.md)
- [JavaScript Quick Start](javascript.md)
- [Knooppunt Quick Start](node-quickstart.md)
- [Snelstartgids voor PYthon](python-quickstart.md)