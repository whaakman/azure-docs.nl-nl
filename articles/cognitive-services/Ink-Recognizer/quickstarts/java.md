---
title: 'Quickstart: Herkent digitale inkt met Ink herkenning REST-API en Java'
description: De Ink-herkenning-API gebruiken om te beginnen met het herkennen van digitale inkt lijnen.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: ink-recognizer
ms.topic: article
ms.date: 05/02/2019
ms.author: aahi
ms.openlocfilehash: 6237253922544dc47bb11aec4dd58139f99eb0da
ms.sourcegitcommit: 17411cbf03c3fa3602e624e641099196769d718b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2019
ms.locfileid: "65518614"
---
# <a name="quickstart-recognize-digital-ink-with-the-ink-recognizer-rest-api-and-java"></a>Quickstart: Herkent digitale inkt met Ink herkenning REST-API en Java

Gebruik deze Quick Start om te beginnen met behulp van de Ink-API voor herkenning van digitale inkt lijnen. Deze Java-toepassing een API-aanvraag met JSON-indeling ink lijn gegevens verzendt en het antwoord wordt opgehaald.

Hoewel deze toepassing in Java is geschreven, is de API een RESTful-webservice die compatibel is met vrijwel elke programmeertaal.

Doorgaans wilt u de API aanroepen vanuit een digitale bij app. In deze snelstartgids verzendt ink lijn gegevens voor het volgende voorbeeld van handgeschreven vanuit een JSON-bestand.

![een afbeelding van handgeschreven tekst](../media/handwriting-sample.jpg)

De broncode voor deze Quick Start kunt u vinden op [GitHub](https://go.microsoft.com/fwlink/?linkid=2089904).

## <a name="prerequisites"></a>Vereisten

- De [Java&trade; ontwikkeling Kit(JDK) 7](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) of hoger.

- Deze bibliotheken importeren vanuit de opslagplaats met Maven
    - [JSON-code in Java](https://mvnrepository.com/artifact/org.json/json) pakket
    - [Apache HttpClient](https://mvnrepository.com/artifact/org.apache.httpcomponents/httpclient) pakket

- De voorbeeld ink lijn gegevens voor deze Quick Start kunt u vinden op [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/java/InkRecognition/quickstart/example-ink-strokes.json).

[!INCLUDE [cognitive-services-ink-recognizer-signup-requirements](../../../../includes/cognitive-services-ink-recognizer-signup-requirements.md)]

## <a name="create-a-new-application"></a>Een nieuwe toepassing maken

1. Maak een nieuw Java-project in uw favoriete IDE of editor en importeer de volgende bibliotheken.

    ```java
    import org.apache.http.HttpEntity;
    import org.apache.http.client.methods.CloseableHttpResponse;
    import org.apache.http.client.methods.HttpPost;
    import org.apache.http.entity.StringEntity;
    import org.apache.http.impl.client.CloseableHttpClient;
    import org.apache.http.impl.client.HttpClients;
    import org.apache.http.util.EntityUtils;
    import java.io.IOException;
    import java.nio.file.Files;
    import java.nio.file.Paths;
    ```

2. Variabelen voor de abonnementssleutel van uw en uw eindpunt maken. Hieronder ziet u de URI die u voor inktherkenning van Windows gebruiken kunt. Deze zullen worden toegevoegd aan uw service-eindpunt later voor het maken van de API-aanvraag-URL.

    ```java
    // Replace the subscriptionKey string value with your valid subscription key.
    static final String subscriptionKey = "YOUR_SUBSCRIPTION_KEY";
    // Replace the dataPath string with a path to the JSON formatted ink stroke data file.
    static final String dataPath = "PATH_TO_INK_STROKE_DATA";
    
    static final String endpoint = "https://api.cognitive.microsoft.com";
    static final String inkRecognitionUrl = "/inkrecognizer/v1.0-preview/recognize";
    ```

## <a name="create-a-function-to-send-requests"></a>Maak een functie voor het verzenden van aanvragen

1. Maak een nieuwe functie met de naam `sendRequest()` waarmee de variabelen die eerder is gemaakt. Voer vervolgens de volgende stappen uit.

2. Maak een `CloseableHttpClient` -object dat aanvragen naar de API verzenden. Verzenden van de aanvraag voor een `HttpPut` request-object door een combinatie van uw eindpunt en de URL van de Ink-herkenning.

3. Gebruik van de aanvraag `setHeader()` functie om in te stellen de `Content-Type` koptekst `application/json`, en uw abonnementssleutel toevoegen aan de `Ocp-Apim-Subscription-Key` header.

4. Gebruik van de aanvraag `setEntity()` functie waarmee u kunt de gegevens worden verzonden.   

5. Gebruik van de client `execute()` functie voor het verzenden van de aanvraag en op te slaan naar een `CloseableHttpResponse` object. 

6. Maak een `HttpEntity` object voor het opslaan van de inhoud van de reactie. Bekijk de inhoud met `getEntity()`. Als het antwoord niet leeg is, retourneert deze.
    
    ```java
    static String sendRequest(String apiAddress, String endpoint, String subscriptionKey, String requestData) {
        try (CloseableHttpClient client = HttpClients.createDefault()) {
            HttpPut request = new HttpPut(endpoint + apiAddress);
            // Request headers.
            request.setHeader("Content-Type", "application/json");
            request.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
            request.setEntity(new StringEntity(requestData));
            try (CloseableHttpResponse response = client.execute(request)) {
                HttpEntity respEntity = response.getEntity();
                if (respEntity != null) {
                    return EntityUtils.toString(respEntity, "utf-8");
                }
            } catch (Exception respEx) {
                respEx.printStackTrace();
            }
        } catch (IOException ex) {
            System.err.println("Exception recognizing ink: " + ex.getMessage());
            ex.printStackTrace();
        }
        return null;
    }
    ```

## <a name="send-an-ink-recognition-request"></a>Een aanvraag van de spraakherkenning ink verzenden

Maken van een methode met de naam `recognizeInk()` voor het herkennen van uw gegevens van de lijn ink. Roep de `sendRequest()` methode hierboven hebt gemaakt met uw eindpunt, de url, de abonnementssleutel en de json-gegevens. Het resultaat en af te drukken naar de console.

```java
static void recognizeInk(String requestData) {
    System.out.println("Sending an ink recognition request");
    String result = sendRequest(inkRecognitionUrl, endpoint, subscriptionKey, requestData);
    System.out.println(result);
}
```

## <a name="load-your-digital-ink-data-and-send-the-request"></a>Uw digitale Inktgegevens laden en de aanvraag verzenden

1. Lees in de JSON-bestand met de gegevens die worden toegevoegd aan de aanvragen in de belangrijkste methode van uw toepassing.

2. Roep de ink erkenning-functie die eerder is gemaakt.
    
    ```java
    public static void main(String[] args) throws Exception {
        String requestData = new String(Files.readAllBytes(Paths.get(dataPath)), "UTF-8");
        recognizeInk(requestData);
    }
    ```

## <a name="run-the-application-and-view-the-response"></a>Voer de toepassing en de reactie weergeven

Voer de toepassing uit. Een geslaagde respons wordt geretourneerd in JSON-indeling. U kunt ook het JSON-antwoord vinden op [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/java/InkRecognition/quickstart/example-response.json).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Naslaginformatie over REST API](https://go.microsoft.com/fwlink/?linkid=2089907)


Als u wilt zien hoe de Ink-API in een digitale bij app werkt, bekijk de volgende voorbeeldtoepassingen op GitHub:
* [C# en Universal Windows Platform (UWP)](https://go.microsoft.com/fwlink/?linkid=2089803)  
* [C# en Windows Presentation Foundation (WPF)](https://go.microsoft.com/fwlink/?linkid=2089804)
* [Webbrowser-app (Javascript)](https://go.microsoft.com/fwlink/?linkid=2089908)       
* [Mobiele app (Java en Android)](https://go.microsoft.com/fwlink/?linkid=2089906)
* [Mobiele app (Swift en iOS)](https://go.microsoft.com/fwlink/?linkid=2089805)
