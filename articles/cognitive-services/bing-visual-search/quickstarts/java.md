---
title: Java-Quickstart voor Bing visuele zoekopdrachten-API | Microsoft Docs
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Laat zien hoe u een installatiekopie uploaden naar de Bing visuele zoekopdrachten-API en weer toegang krijgen van inzicht in de afbeelding.
services: cognitive-services
author: swhite-msft
manager: rosh
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: article
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: 41e0855b126ca6e54d0a487a88fe59a0be6f72f6
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39071992"
---
# <a name="your-first-bing-visual-search-query-in-java"></a>Uw eerste Bing visuele zoekopdrachten-query in Java

Bing visuele zoekopdrachten-API retourneert informatie over een afbeelding die u opgeeft. U kunt de installatiekopie opgeven met behulp van de URL van de afbeelding, een insights token, of door een installatiekopie te uploaden. Zie voor meer informatie over deze opties [wat Bing visuele zoekopdrachten-API is?](../overview.md) In dit artikel ziet u een installatiekopie uploaden. Uploaden van een afbeelding kan nuttig zijn in mobiele scenario's waarbij u een afbeelding van een bekende oriëntatiepunt en informatie erover weer toegang krijgen. Bijvoorbeeld, kunnen de insights bevat ook algemene vragen over de oriëntatiepunt. 

Als u een lokale installatiekopie uploadt, ziet hieronder u de gegevens dat moet u in de hoofdtekst van het bericht opnemen. De gegevens moet de header Content-Disposition bevatten. De `name` parameter moet worden ingesteld op 'afbeelding' en de `filename` parameter kan worden ingesteld op een willekeurige tekenreeks. De inhoud van het formulier is het binaire bestand van de installatiekopie. De grootte van de maximale installatiekopie die u kunt uploaden is 1 MB. 

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

Dit artikel bevat een eenvoudige consoletoepassing die een Bing visuele zoekopdrachten-API-aanvraag verzendt en de JSON-zoekresultaten worden weergegeven. Terwijl deze toepassing is geschreven in Java, de API is een RESTful-Web-compatibel is met elke programmeertaal die HTTP-aanvragen te parseren van JSON. 


## <a name="prerequisites"></a>Vereisten

U moet [JDK 7 of 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) compileren en uitvoeren van deze code. U kunt een Java-IDE gebruiken als u een favoriet hebt, maar worden volstaan met een teksteditor.

In deze Quick Start kunt u een [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) abonnementssleutel of een betaald abonnement-sleutel.

## <a name="running-the-application"></a>De toepassing uitvoeren

Hieronder ziet u hoe u de installatiekopie uploaden via MultipartEntityBuilder in Java.

Om uit te voeren deze toepassing, de volgende stappen uit:

1. Downloaden en installeren de [gson bibliotheek](https://github.com/google/gson). U mag deze ook verkrijgen via Maven.
2. Maak een nieuwe Java-project in uw favoriete IDE of editor.
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
[Bing visuele zoekopdrachten-installatiekopie uploaden zelfstudie](../tutorial-visual-search-image-upload.md)
[Bing visuele zoekopdrachten-app met één pagina zelfstudie](../tutorial-bing-visual-search-single-page-app.md)  
[Bing visuele zoekopdrachten-overzicht](../overview.md)  
[Nu uitproberen](https://aka.ms/bingvisualsearchtryforfree)  
[Een gratis proefversie toegangssleutel ophalen](https://azure.microsoft.com/try/cognitive-services/?api=bing-visual-search-api)  
[Bing visuele zoekopdrachten-API-verwijzing](https://aka.ms/bingvisualsearchreferencedoc)

