---
title: 'Quickstart: Afbeeldingsinzichten krijgen met behulp van de Bing Visual Search REST-API en Java'
titleSuffix: Azure Cognitive Services
description: Leer hoe u een afbeelding uploadt naar de Bing Visual Search-API en inzichten in de afbeelding verkrijgt.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 4/02/2019
ms.author: scottwhi
ms.openlocfilehash: 2fe4e9dad0b198fe54e06ce07100d231f1f7d157
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2019
ms.locfileid: "59046441"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-rest-api-and-java"></a>Quickstart: Afbeeldingsinzichten krijgen met behulp van de Bing Visual Search REST-API en Java

Met deze Quick Start kunt u uw eerste aanroep naar de Bing visuele zoekopdrachten-API en de resultaten te bekijken. Deze Java-toepassing wordt een installatiekopie geüpload naar de API en de informatie die wordt weergegeven. Hoewel deze toepassing is geschreven in Java, de API is een RESTful-Web-compatibel is met de meeste moderne programmeertalen.

Wanneer u een lokale installatiekopie uploadt, de gegevens moet bevatten de `Content-Disposition` header. U moet instellen de `name` parameter 'afbeelding', en u kunt instellen de `filename` parameter een tekenreeks. De inhoud van het formulier omvat de binaire gegevens van de installatiekopie. De grootte van de maximale installatiekopie die u kunt uploaden is 1 MB.

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

## <a name="prerequisites"></a>Vereisten

* De [Java Development Kit (JDK) 7 of 8](https://aka.ms/azure-jdks)
* De [Gson Java-bibliotheek](https://github.com/google/gson)
* [Apache HttpComponents](https://hc.apache.org/downloads.cgi)

[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="create-and-initialize-a-project"></a>Een project maken en initialiseren

1. Maak een nieuwe Java-project in uw favoriete IDE of editor en importeer de volgende bibliotheken:

    ```java
    import java.util.*;
    import java.io.*;
    import com.google.gson.Gson;
    import com.google.gson.GsonBuilder;
    import com.google.gson.JsonObject;
    import com.google.gson.JsonParser;
    
    // HttpClient libraries
    
    import org.apache.http.HttpEntity;
    import org.apache.http.HttpResponse;
    import org.apache.http.client.methods.HttpPost;
    import org.apache.http.entity.ContentType;
    import org.apache.http.entity.mime.MultipartEntityBuilder;
    import org.apache.http.impl.client.CloseableHttpClient;
    import org.apache.http.impl.client.HttpClientBuilder;
    ```

2. Variabelen voor uw API-eindpunt, de abonnementssleutel en het pad naar uw installatiekopie maken:

    ```java
    static String endpoint = "https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch";
    static String subscriptionKey = "your-key-here";
    static String imagePath = "path-to-your-image";
    ```

## <a name="create-the-json-parser"></a>De JSON-parser maken

Een methode voor het maken van de JSON-antwoord van de API meer maken met behulp van leesbare `JsonParser`:

    ```java
    public static String prettify(String json_text) {
            JsonParser parser = new JsonParser();
            JsonObject json = parser.parse(json_text).getAsJsonObject();
            Gson gson = new GsonBuilder().setPrettyPrinting().create();
            return gson.toJson(json);
        }
    ```

## <a name="construct-the-search-request-and-query"></a>De zoekopdracht en query compileren

1. In de belangrijkste methode van uw toepassing, maakt u een HTTP-client met behulp van `HttpClientBuilder.create().build();`:

    ```java
    CloseableHttpClient httpClient = HttpClientBuilder.create().build();
    ```

2. Maak een `HttpEntity` uw installatiekopie uploaden naar de API-object:

    ```java
    HttpEntity entity = MultipartEntityBuilder
        .create()
        .addBinaryBody("image", new File(imagePath))
        .build();
    ```

3. Maak een `httpPost` object met het eindpunt toe en stel de koptekst van uw abonnementssleutel te gebruiken:

    ```java
    HttpPost httpPost = new HttpPost(endpoint);
    httpPost.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
    httpPost.setEntity(entity);
    ```

## <a name="receive-and-process-the-json-response"></a>Het JSON-antwoord ontvangen en verwerken

1. Gebruik de `HttpClient.execute()` methode voor het verzenden van een aanvraag naar de API en opslaan van het antwoord in een `InputStream` object:
    
    ```java
    HttpResponse response = httpClient.execute(httpPost);
    InputStream stream = response.getEntity().getContent();
    ```

2. De JSON-tekenreeks Store en het antwoord afdrukken:

```java
String json = new Scanner(stream).useDelimiter("\\A").next();
System.out.println("\nJSON Response:\n");
System.out.println(prettify(json));
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een visuele zoekopdrachten één pagina web-app bouwen](../tutorial-bing-visual-search-single-page-app.md)
