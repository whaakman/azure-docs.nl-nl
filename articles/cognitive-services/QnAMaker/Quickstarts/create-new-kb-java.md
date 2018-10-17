---
title: 'Snelstart: API Java - Knowledge base maken - QnA Maker'
titlesuffix: Azure Cognitive Services
description: In deze snelstart wordt stapsgewijs uitgelegd hoe u, met behulp van een programma, een voorbeeldexemplaar van een knowledge base in QnA Maker kunt maken, dat wordt weergegeven op het Azure-dashboard van uw account voor Cognitive Services-API.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: quickstart
ms.date: 09/12/2018
ms.author: diberry
ms.openlocfilehash: e8a876d1e5416547d0eead0882b69bdf68818c5d
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2018
ms.locfileid: "48886208"
---
# <a name="create-a-new-knowledge-base-in-java"></a>Een nieuwe knowledge base maken in Java

In deze snelstart wordt stapsgewijs uitgelegd hoe u, met behulp van een programma, een voorbeeldexemplaar van een knowledge base in QnA Maker kunt maken, dat wordt weergegeven op het Azure-dashboard van uw account voor Cognitive Services-API.

[!INCLUDE [Code is available in Azure-Samples Github repo](../../../../includes/cognitive-services-qnamaker-java-repo-note.md)]

Hieronder ziet u twee voorbeelden van URL's voor veelgestelde vragen (in kb.urls van **getKB()**) die inhoud bieden. Met QnA Maker worden automatisch vragen en antwoorden opgehaald uit semi-gestructureerde inhoud, zoals veelgestelde vragen. Dit wordt nog verder uitgelegd in dit document met [gegevensbronnen](../Concepts/data-sources-supported.md). U kunt in deze snelstart ook uw eigen URL's voor veelgestelde vragen gebruiken.

## <a name="prerequisites"></a>Vereisten

U hebt [JDK 7 of 8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) nodig om deze code te compileren en uit te voeren. U kunt een Java-IDE gebruiken als u daar graag mee werkt, maar u kunt ook een teksteditor gebruiken.

U moet beschikken over een [account voor Cognitive Services-API](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met **QnA Maker** als resource. U hebt een betaalde abonnementssleutel van uw nieuwe API-account nodig op het [Azure-dashboard](https://portal.azure.com/#create/Microsoft.CognitiveServices). Beide sleutels werken voor deze snelstart.

![Sleutel voor de service Azure-dashboard](../media/sub-key.png)

## <a name="create-knowledge-base"></a>Een knowledge base maken

Met de volgende code wordt een nieuwe knowledge base gemaakt met behulp van de methode [Create](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff).

1. Maak een nieuw Java-project in uw favoriete IDE.
1. Voeg de [Google GSON-bibliotheek](https://github.com/google/gson) toe aan uw Java-project. U kunt dit doen door het .jar-bestand handmatig te [maken](https://stackoverflow.com/questions/5258159/how-to-make-an-executable-jar-file) en te importeren, of door een afhankelijkheid toe te voegen aan uw favoriete hulpprogramma voor projectbeheer, bijvoorbeeld Maven.
1. Kopieer en plak de onderstaande code.
1. Vervang de waarde `subscriptionKey` door de geldige abonnementssleutel.
1. Voer het programma uit.

```java
import java.io.*;
import java.lang.reflect.Type;
import java.net.*;
import java.util.*;
import javax.net.ssl.HttpsURLConnection;

/**
 * Gson: https://github.com/google/gson
 * Maven info:
 *    <dependency>
 *      <groupId>com.google.code.gson</groupId>
 *      <artifactId>gson</artifactId>
 *      <version>2.8.5</version>
 *    </dependency>
 */
import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.google.gson.JsonElement;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;
import com.google.gson.reflect.TypeToken;

/** NOTE: To compile and run this code without an IDE:
 * 1. Save this file as CreateKB.java
 * 2. Run *:
 *    javac CreateKB.java -cp .;<GSON> -encoding UTF-8
 * 3. Run *:
 *    java -cp .;<GSON> CreateKB
 * *replace <GSON> with the name of the current Google GSON library .jar file,
 * for example: gson-2.8.5.jar
*/

public class CreateKB {

    // **********************************************
    // *** Update or verify the following values. ***
    // **********************************************

    // Replace this with a valid subscription key.
    static String subscriptionKey = "ADD YOUR SUBSCRIPTION KEY HERE";

    // Components used to create HTTP request URIs for QnA Maker operations.
    static String host = "https://westus.api.cognitive.microsoft.com";
    static String service = "/qnamaker/v4.0";
    static String method = "/knowledgebases/create";

    // Serializing classes into JSON for our request to the server.
    // For the JSON request schema, see <a href="https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff">QnA Maker V4.0</a>
    public static class KB {
        String name;
        Question[] qnaList;
        String[] urls;
        File[] files;
    }

    public static class Question {
        Integer id;
        String answer;
        String source;
        String[] questions;
        Metadata[] metadata;
    }

    public static class Metadata {
        String name;
        String value;
    }

    public static class File {
        String fileName;
        String fileUri;
    }

    // This class has the HTTP response headers and body that is returned
    // by the HTTP request.
    public static class Response {
        Map<String, List<String>> Headers;
        String Response;

        /**
         * Constructor that specifies header and body response
         * @param headers List of headers
         * @param response Response returned from your HTTP request
         */
        public Response(Map<String, List<String>> headers, String response) {
            this.Headers = headers;
            this.Response = response;
        }
    }

    /**
     * Formats and indents JSON for display.
     * @param json_text The JSON string to format and indent.
     * @return The formatted, indented JSON string.
     */
    public static String PrettyPrint (String json_text) {
        JsonParser parser = new JsonParser();
        JsonElement json = parser.parse(json_text);
        Gson gson = new GsonBuilder().setPrettyPrinting().create();
        return gson.toJson(json);
    }

    /**
     * Sends an HTTP GET request.
     * @param url The URL of the HTTP request.
     * @return The object that represents the HTTP response.
     */
    public static Response Get (URL url) throws Exception{
        HttpsURLConnection connection = (HttpsURLConnection) url.openConnection();
            connection.setRequestMethod("GET");
            connection.setRequestProperty("Ocp-Apim-Subscription-Key", subscriptionKey);
            connection.setDoOutput(true);
        StringBuilder response = new StringBuilder ();
        BufferedReader in = new BufferedReader(new InputStreamReader(connection.getInputStream(), "UTF-8"));

        String line;
        while ((line = in.readLine()) != null) {
            response.append(line);
        }
        in.close();
        return new Response (connection.getHeaderFields(), response.toString());
    }

    /**
     * Builds and sends an HTTP POST request.
     * @param url The URL of the HTTP request.
     * @param content The contents of your POST.
     * @return The object that represents the HTTP response.
     */
    public static Response Post (URL url, String content) throws Exception{
        HttpsURLConnection connection = (HttpsURLConnection) url.openConnection();
        connection.setRequestMethod("POST");
        connection.setRequestProperty("Content-Type", "application/json");
        connection.setRequestProperty("Content-Length", content.length() + "");
        connection.setRequestProperty("Ocp-Apim-Subscription-Key", subscriptionKey);
        connection.setDoOutput(true);

        DataOutputStream wr = new DataOutputStream(connection.getOutputStream());
        byte[] encoded_content = content.getBytes("UTF-8");
        wr.write(encoded_content, 0, encoded_content.length);
        wr.flush();
        wr.close();

        StringBuilder response = new StringBuilder ();
        BufferedReader in = new BufferedReader(new InputStreamReader(connection.getInputStream(), "UTF-8"));
        String line;
        while ((line = in.readLine()) != null) {
            response.append(line);
        }
        in.close();
        return new Response (connection.getHeaderFields(), response.toString());
    }

    /**
      * Sends the request to create the knowledge base.
      * @param kb Your knowledge base object.
      * @return Sends POST request.
      * @throws Exception If POST request fails.
      */
    public static Response CreateKB (KB kb) throws Exception {
        URL url = new URL (host + service + method);
        System.out.println ("Calling " + url.toString() + ".");
        String content = new Gson().toJson(kb);
        return Post(url, content);
    }

    /**
     * Checks the status of the request to create the knowledge base.
     * @param operation The operation ID.
     * @return The specific URL being called.
     * @throws Exception If the GET request fails.
     */
    public static Response GetStatus (String operation) throws Exception {
        URL url = new URL (host + service + operation);
        System.out.println ("Calling " + url.toString() + ".");
        return Get(url);
    }

    /**
     * Sends a sample request to create a knowledge base. To understand
     * this 'kb' object, refer to the <a href="https://docs.microsoft.com/en-us/azure/cognitive-services/QnAMaker/concepts/knowledge-base">Knowledge base</a> concept page.
     * @return A new knowledge base.
     */
    public static KB GetKB () {
        KB kb = new KB ();
        kb.name = "Example Knowledge Base";

        Question q = new Question();
        q.id = 0;
        q.answer = "You can use our REST APIs to manage your Knowledge Base. See here for details: https://westus.dev.cognitive.microsoft.com/docs/services/58994a073d9e04097c7ba6fe/operations/58994a073d9e041ad42d9baa";
        q.source = "Custom Editorial";
        q.questions = new String[]{"How do I programmatically update my Knowledge Base?"};

        Metadata md = new Metadata();
        md.name = "category";
        md.value = "api";
        q.metadata = new Metadata[]{md};

        kb.qnaList = new Question[]{q};
        kb.urls = new String[]{"https://docs.microsoft.com/en-in/azure/cognitive-services/qnamaker/faqs",     "https://docs.microsoft.com/en-us/bot-framework/resources-bot-framework-faq"};

        return kb;
    }

    public static void main(String[] args) {
        try {
            // Send the request to create the knowledge base.
            Response response = CreateKB (GetKB ());
            String operation = response.Headers.get("Location").get(0);
            System.out.println (PrettyPrint (response.Response));
            // Loop until the request is completed.
            Boolean done = false;
            while (!done) {
                // Check on the status of the request.
                response = GetStatus (operation);
                System.out.println (PrettyPrint (response.Response));
                Type type = new TypeToken<Map<String, String>>(){}.getType();
                Map<String, String> fields = new Gson().fromJson(response.Response, type);
                String state = fields.get ("operationState");
                // If the request is still running, the server tells us how
                // long to wait before checking the status again.
                if (state.equals("Running") || state.equals("NotStarted")) {
                    String wait = response.Headers.get ("Retry-After").get(0);
                    System.out.println ("Waiting " + wait + " seconds...");
                    Thread.sleep (Integer.parseInt(wait) * 1000);
                }
                else {
                    done = true;
                }
            }
        } catch (Exception e) {
            System.out.println (e.getCause().getMessage());
        }
    }
}
```

## <a name="understanding-what-qna-maker-returns"></a>Inzicht krijgen in de inhoud die wordt geretourneerd met QnA Maker

Een geslaagd antwoord wordt geretourneerd in de JSON-indeling, zoals u in het volgende voorbeeld kunt zien. Uw resultaten kunnen iets verschillen. Als met de definitieve aanroep de status Geslaagd wordt geretourneerd, is de knowledge base gemaakt. Raadpleeg de sectie [Get Operation Details](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/operations_getoperationdetails) (Bewerkingsdetails ophalen) van de QnA Maker-API.

```json
Calling https://westus.api.cognitive.microsoft.com/qnamaker/v4.0/knowledgebases/create.
{
  "operationState": "NotStarted",
  "createdTimestamp": "2018-04-13T01:52:30Z",
  "lastActionTimestamp": "2018-04-13T01:52:30Z",
  "userId": "2280ef5917bb4ebfa1aae41fb1cebb4a",
  "operationId": "e88b5b23-e9ab-47fe-87dd-3affc2fb10f3"
}
Calling https://westus.api.cognitive.microsoft.com/qnamaker/v4.0/operations/d9d40918-01bd-49f4-88b4-129fbc434c94.
{
  "operationState": "Running",
  "createdTimestamp": "2018-04-13T01:52:30Z",
  "lastActionTimestamp": "2018-04-13T01:52:30Z",
  "userId": "2280ef5917bb4ebfa1aae41fb1cebb4a",
  "operationId": "e88b5b23-e9ab-47fe-87dd-3affc2fb10f3"
}
Waiting 30 seconds...
Calling https://westus.api.cognitive.microsoft.com/qnamaker/v4.0/operations/d9d40918-01bd-49f4-88b4-129fbc434c94.
{
  "operationState": "Succeeded",
  "createdTimestamp": "2018-04-13T01:52:30Z",
  "lastActionTimestamp": "2018-04-13T01:52:46Z",
  "resourceLocation": "/knowledgebases/b0288f33-27b9-4258-a304-8b9f63427dad",
  "userId": "2280ef5917bb4ebfa1aae41fb1cebb4a",
  "operationId": "e88b5b23-e9ab-47fe-87dd-3affc2fb10f3"
}
Press any key to continue.
```
   
## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Naslaginformatie over REST API voor QnA Maker (V4)](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)