---
title: 'Snelstart: Een Visual Search-query maken, Java - Bing Visual Search'
titleSuffix: Azure Cognitive Services
description: Een afbeelding uploaden naar Bing Visual Search-API en inzichten over de afbeelding terugkrijgen.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: quickstart
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: f54914b846c6a001a9fb10d938a038e390abf6bf
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50416440"
---
# <a name="quickstart-your-first-bing-visual-search-query-in-java"></a>Snelstart: Uw eerste Bing Visual Search-query in Java

Bing Visual Search-API retourneert informatie over een afbeelding die u opgeeft. U kunt de afbeelding opgeven door de URL van de afbeelding te gebruiken, een inzichttoken of door een afbeelding te uploaden. Zie [Wat is Bing Visual Search-API?](../overview.md) voor informatie over deze opties. Dit artikel demonstreert het uploaden van een afbeelding. Het uploaden van een afbeelding kan handig zijn in mobiele scenario's waarbij u een foto neemt van een bekend oriëntatiepunt en er informatie over terugkrijgt. De inzichten kunnen bijvoorbeeld trivia bevatten over het oriëntatiepunt. 

Als u een lokale afbeelding uploadt, toont het volgende de formuliergegevens die u in de POST moet opnemen. De formuliergegevens moeten de header Content-Disposition bevatten. De parameter `name` moet worden ingesteld op "image" en de parameter `filename` kan op een willekeurige tekenreeks worden ingesteld. De inhoud van het formulier is het binaire bestand van de afbeelding. De maximale afbeeldingsgrootte die u kunt uploaden is 1 MB. 

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

Dit artikel bevat een eenvoudige consoletoepassing die een Bing Visual Search-API-aanvraag verzendt en de JSON-zoekresultaten weergeeft. Hoewel deze toepassing in Java is geschreven, is de API een RESTful-webservice die compatibel is met elke programmeertaal die HTTP-aanvragen kan doen en JSON kan parseren. 


## <a name="prerequisites"></a>Vereisten

U hebt [JDK 7 of 8](https://aka.ms/azure-jdks) nodig om deze code te compileren en uit te voeren. U kunt een Java-IDE gebruiken als u daar graag mee werkt, maar een teksteditor volstaat.

In deze snelstartgids kunt u een code van een [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) of van een betaald abonnement gebruiken.

## <a name="running-the-application"></a>De toepassing uitvoeren

Hieronder ziet u hoe u de afbeelding uploadt met behulp van MultipartEntityBuilder in Java.

Volg deze stappen voor het uitvoeren van deze toepassing:

1. Download of installeer de [gson-bibliotheek](https://github.com/google/gson). U kunt deze ook verkrijgen via Maven.
2. Maak een nieuw Java-project in uw favoriete IDE of editor.
3. Voeg de geleverde code toe aan een bestand met de naam `VisualSearch.java`.
4. Vervang de waarde `subscriptionKey` door uw abonnementssleutel.
4. Vervang de waarde van `imagePath` door het pad van de te uploaden afbeelding.
5. Voer het programma uit.


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

[Inzichten krijgen over een afbeelding met behulp van een inzichttoken](../use-insights-token.md)  
[Zelfstudie: Afbeelding uploaden naar Bing Visual Search](../tutorial-visual-search-image-upload.md)
[Zelfstudie: Bing Visual Search-app met één pagina](../tutorial-bing-visual-search-single-page-app.md)  
[Overzicht van Bing Visual Search ](../overview.md)  
[Proberen](https://aka.ms/bingvisualsearchtryforfree)  
[Een toegangscode voor een gratis proefversie aanvragen](https://azure.microsoft.com/try/cognitive-services/?api=bing-visual-search-api)  
[Naslaginformatie over Bing Visual Search-API](https://aka.ms/bingvisualsearchreferencedoc)

