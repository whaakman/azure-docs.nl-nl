---
title: Java-Quick Start voor Bing Visual zoeken-API | Microsoft Docs
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Laat zien hoe u een installatiekopie uploaden naar Bing Visual zoeken-API en terughalen van inzicht in de afbeelding.
services: cognitive-services
author: swhite-msft
manager: rosh
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: article
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: 8160302faa373d69b65afe6b68a8efb44442850d
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345658"
---
# <a name="your-first-bing-visual-search-query-in-java"></a>Uw eerste Bing Visual zoekopdracht in Java

Bing Visual zoeken-API retourneert informatie over de installatiekopie die u opgeeft. Met behulp van de URL van de afbeelding, een insights token, of door een afbeelding te uploaden, kunt u de installatiekopie opgeven. Zie voor meer informatie over deze opties [wat Bing Visual zoeken-API is?](../overview.md) In dit artikel ziet u een afbeelding te uploaden. Een afbeelding te uploaden kan nuttig zijn in mobiele scenario's waarin u een afbeelding van een bekende kenmerkende en informatie over het terughalen. De insights kunnen bijvoorbeeld de algemene vragen over de kenmerkende bevatten. 

Als u een lokale installatiekopie uploadt, ziet hieronder u de gegevens dat moet u in de hoofdtekst van het bericht opnemen. De formuliergegevens moet de header Content-Disposition bevatten. De `name` parameter moet worden ingesteld op 'installatiekopie' en de `filename` parameter kan worden ingesteld op een willekeurige tekenreeks. De inhoud van het formulier is het binaire bestand van de afbeelding. De grootte van de maximale installatiekopie die u kan uploaden is 1 MB. 

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

Dit artikel bevat een eenvoudige consoletoepassing die verzendt een aanvraag Bing Visual zoeken-API en de JSON-zoekresultaten worden weergegeven. Terwijl deze toepassing is geschreven in Java, is de API een compatibel is met elke programmeertaal die kunt maken van HTTP-aanvragen en parseren van JSON RESTful-Web-service. 


## <a name="prerequisites"></a>Vereisten

U moet [JDK 7 of 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) compileren en uitvoeren van deze code. U mag een IDE voor Java gebruiken als u een favoriet hebben, maar worden volstaan met een teksteditor.

U kunt gebruiken voor deze snelstartgids een [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) abonnementssleutel of een betaald abonnement-sleutel.

## <a name="running-the-application"></a>De toepassing uitvoeren

Hieronder ziet u het uploaden van de installatiekopie via MultipartEntityBuilder in Java.

Voor het uitvoeren van deze toepassing, de volgende stappen uit:

1. Downloaden en installeren de [gson bibliotheek](https://github.com/google/gson). U kunt deze ook ophalen via Maven.
2. Maak een nieuwe Java-project in uw favoriete IDE of -editor.
3. De opgegeven code toevoegen in een bestand met de naam `VisualSearch.java`.
4. Vervang de `subscriptionKey` waarde met de abonnementssleutel van uw.
4. Vervang de `imagePath` waarde met het pad van de afbeelding te uploaden.
5. Voer het programma.


```java
package uploadimage;

import java.util.*;
import java.io.*;

/*
 * Gson: https://github.com/google/gson
 * Maven info:
 *     groupId: com.google.code.gson
 *     artifactId: gson
 *     version: 2.8.2
 *
 * Once you have compiled or downloaded gson-2.8.2.jar, assuming you have placed it in the
 * same folder as this file (BingImageSearch.java), you can compile and run this program at
 * the command line as follows.
 *
 * javac BingImageSearch.java -classpath .;gson-2.8.2.jar -encoding UTF-8
 * java -cp .;gson-2.8.2.jar BingImageSearch
 */

import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;

// http://hc.apache.org/downloads.cgi (HttpComponents Downloads) HttpClient 4.5.5

import org.apache.http.HttpEntity;
import org.apache.http.HttpResponse;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.ContentType;
import org.apache.http.entity.mime.MultipartEntityBuilder;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClientBuilder;


public class UploadImage2 {

    static String endpoint = "https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch";
    static String subscriptionKey = "<yoursubscriptionkeygoeshere";
    static String imagePath = "<pathtoyourimagetouploadgoeshere>";

    /**
     * @param args the command line arguments
     */
    public static void main(String[] args) {
        
        CloseableHttpClient httpClient = HttpClientBuilder.create().build();
        
        try {
            HttpEntity entity = MultipartEntityBuilder
                .create()
                .addBinaryBody("image", new File(imagePath))
                .build();

            HttpPost httpPost = new HttpPost(endpoint);
            httpPost.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
            httpPost.setEntity(entity);
            HttpResponse response = httpClient.execute(httpPost);

            InputStream stream = response.getEntity().getContent();
            String json = new Scanner(stream).useDelimiter("\\A").next();

            System.out.println("\nJSON Response:\n");
            System.out.println(prettify(json));
        }
        catch (IOException e)
        {
            e.printStackTrace(System.out);
            System.exit(1);
        }
        catch (Exception e) {
            e.printStackTrace(System.out);
            System.exit(1);
        }
    }
    
    // pretty-printer for JSON; uses GSON parser to parse and re-serialize
    public static String prettify(String json_text) {
        JsonParser parser = new JsonParser();
        JsonObject json = parser.parse(json_text).getAsJsonObject();
        Gson gson = new GsonBuilder().setPrettyPrinting().create();
        return gson.toJson(json);
    }
    
}
```

## <a name="next-steps"></a>Volgende stappen

[Inzichten over een afbeelding met behulp van een token inzichten verkrijgen](../use-insights-token.md)  
[Bing Visual Search één pagina app-zelfstudie](../tutorial-bing-visual-search-single-page-app.md)  
[Overzicht van Bing Visual zoeken](../overview.md)  
[Probeer deze](https://aka.ms/bingvisualsearchtryforfree)  
[Een gratis proefversie toegangssleutel ophalen](https://azure.microsoft.com/try/cognitive-services/?api=bing-visual-search-api)  
[Bing Visual Search API-verwijzingen](https://aka.ms/bingvisualsearchreferencedoc)

