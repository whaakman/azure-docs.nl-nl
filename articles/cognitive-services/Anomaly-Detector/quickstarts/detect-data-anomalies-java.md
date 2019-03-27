---
title: 'Quickstart: Detecteer afwijkingen in uw time series-gegevens met behulp van de REST-API voor Afwijkingsdetectie Detector en Java | Microsoft Docs'
description: Gebruik de API van de detectie van afwijkingen voor het detecteren van afwijkingen in de reeks als een batch of voor streaming-gegevens.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: article
ms.date: 03/26/2019
ms.author: aahi
ms.openlocfilehash: 2a6d356bcf65cdb59e84b34cf2a7478de796e61a
ms.sourcegitcommit: fbfe56f6069cba027b749076926317b254df65e5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58473164"
---
# <a name="quickstart-detect-anomalies-in-your-time-series-data-using-the-anomaly-detector-rest-api-and-java"></a>Quickstart: Detecteer afwijkingen in uw time series-gegevens met behulp van de REST-API voor Afwijkingsdetectie Detector en Java

Gebruik deze Quick Start om te starten met behulp van twee modi voor detectie van afwijkingen Detector-API's voor het detecteren van afwijkingen in uw time series-gegevens. Deze Java-toepassing twee API-aanvragen met JSON-indeling time series-gegevens verzendt en ontvangt de antwoorden.

| API-aanvraag                                        | Toepassingsuitvoer                                                                                                                         |
|----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| Detecteer afwijkingen als batch                        | De JSON-antwoord met de anomaliedetectie-status (en andere gegevens) voor elk gegevenspunt in de time series-gegevens en de posities van alle gedetecteerde afwijkingen. |
| De status van afwijkingen van de meest recente gegevenspunt detecteren | De JSON-antwoord met de anomaliedetectie-status (en andere gegevens) voor de meest recente gegevenspunt in de time series-gegevens.                                                                                                                                         |

 Hoewel deze toepassing in Java is geschreven, is de API een RESTful-webservice die compatibel is met vrijwel elke programmeertaal.

## <a name="prerequisites"></a>Vereisten

- De [Java&trade; ontwikkeling Kit(JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) of hoger.

- Deze bibliotheken importeren vanuit de opslagplaats met Maven
    - [JSON-code in Java](https://mvnrepository.com/artifact/org.json/json) pakket
    - [Apache HttpClient](https://mvnrepository.com/artifact/org.apache.httpcomponents/httpclient) pakket

- Een JSON-bestand met time series-gegevens verwijst. De voorbeeldgegevens voor deze Quick Start kunt u vinden op [GitHub](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/request-data.json).

[!INCLUDE [cognitive-services-anomaly-detector-data-requirements](../../../../includes/cognitive-services-anomaly-detector-data-requirements.md)]

[!INCLUDE [cognitive-services-anomaly-detector-signup-requirements](../../../../includes/cognitive-services-anomaly-detector-signup-requirements.md)]

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
    import org.json.JSONArray;
    import org.json.JSONObject;
    import java.io.IOException;
    import java.nio.file.Files;
    import java.nio.file.Paths;
    ```

2. Variabelen voor de abonnementssleutel van uw en uw eindpunt maken. Hieronder vindt u de URI's die u voor detectie van afwijkingen gebruiken kunt. Deze worden toegevoegd aan uw service-eindpunt later het maken van de API aanvraag-URL's.

    |Detectiemethode  |URI  |
    |---------|---------|
    |Detectie van batch    | `/anomalydetector/v1.0/timeseries/entire/detect`        |
    |Detectie op de meest recente gegevenspunt     | `/anomalydetector/v1.0/timeseries/last/detect`        |

    ```java
    // Replace the subscriptionKey string value with your valid subscription key.
    static final String subscriptionKey = "[YOUR_SUBSCRIPTION_KEY]";
    //replace the endpoint URL with the correct one for your subscription. Your endpoint can be found in the Azure portal. 
    //For example: https://westus2.api.cognitive.microsoft.com
    static final String endpoint = "[YOUR_ENDPOINT_URL]";
    // Replace the dataPath string with a path to the JSON formatted time series data.
    static final String dataPath = "[PATH_TO_TIME_SERIES_DATA]";
    static final String latestPointDetectionUrl = "/anomalydetector/v1.0/timeseries/last/detect";
    static final String batchDetectionUrl = "/anomalydetector/v1.0/timeseries/entire/detect";
    ```

3. In de JSON-gegevensbestand lezen

    ```java
    String requestData = new String(Files.readAllBytes(Paths.get(dataPath)), "UTF-8");
    ```

## <a name="create-a-function-to-send-requests"></a>Maak een functie voor het verzenden van aanvragen

1. Maak een nieuwe functie met de naam `sendRequest()` waarmee de variabelen die eerder is gemaakt. Voer vervolgens de volgende stappen uit.

2. Maak een `CloseableHttpClient` -object dat aanvragen naar de API verzenden. Verzenden van de aanvraag voor een `HttpPost` request-object door een combinatie van uw eindpunt en een URL van de detectie van afwijkingen.

3. Gebruik van de aanvraag `setHeader()` functie om in te stellen de `Content-Type` koptekst naar `application/json`, en uw abonnementssleutel toevoegen aan de `Ocp-Apim-Subscription-Key` header.

4. Gebruik van de aanvraag `setEntity()` functie waarmee u kunt de gegevens worden verzonden.   

5. Gebruik van de client `execute()` functie voor het verzenden van de aanvraag en op te slaan naar een `CloseableHttpResponse` object. 

6. Maak een `HttpEntity` object voor het opslaan van de inhoud van de reactie. Bekijk de inhoud met `getEntity()`. Als het antwoord niet leeg is, retourneert deze.

```java
static String sendRequest(String apiAddress, String endpoint, String subscriptionKey, String requestData) {
    try (CloseableHttpClient client = HttpClients.createDefault()) {
        HttpPost request = new HttpPost(endpoint + apiAddress);
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
        System.err.println("Exception on Anomaly Detector: " + ex.getMessage());
        ex.printStackTrace();
    }
    return null;
}
```

## <a name="detect-anomalies-as-a-batch"></a>Detecteer afwijkingen als batch

1. Maken van een methode met de naam `detectAnomaliesBatch()` voor het detecteren van afwijkingen in de gegevens op als een batch. Roep de `sendRequest()` methode hierboven hebt gemaakt met uw eindpunt, de url, de abonnementssleutel en de json-gegevens. Het resultaat en af te drukken naar de console.

2. De posities van afwijkingen vinden in de gegevensset. Van het antwoord `isAnomaly` veld bevat een Booleaanse waarde die betrekking hebben op of een bepaald gegevenspunt een afwijking is. Ophalen van de JSON-matrix en doorlopen en het afdrukken van de index van een `true` waarden. Deze waarden overeenkomen met de index van afwijkende gegevenspunten, als deze zijn gevonden.

    
    ```java
    static void detectAnomaliesBatch(String requestData) {
        System.out.println("Detecting anomalies as a batch");
        String result = sendRequest(batchDetectionUrl, endpoint, subscriptionKey, requestData);
        if (result != null) {
            System.out.println(result);
            JSONObject jsonObj = new JSONObject(result);
            JSONArray jsonArray = jsonObj.getJSONArray("isAnomaly");
            System.out.println("Anomalies found in the following data positions:");
            for (int i = 0; i < jsonArray.length(); ++i) {
                if (jsonArray.getBoolean(i))
                    System.out.print(i + ", ");
            }
            System.out.println();
        }
    }
    ```

## <a name="detect-the-anomaly-status-of-the-latest-data-point"></a>De status van afwijkingen van de meest recente gegevenspunt detecteren

* Maken van een methode met de naam `detectAnomaliesLatest()` voor het detecteren van de status van afwijkingen van de laatste gegevenspunt in de gegevensset. Roep de `sendRequest()` methode hierboven hebt gemaakt met uw eindpunt, de url, de abonnementssleutel en de json-gegevens. Het resultaat en af te drukken naar de console.

```java
static void detectAnomaliesLatest(String requestData) {
    System.out.println("Determining if latest data point is an anomaly");
    String result = sendRequest(latestPointDetectionUrl, endpoint, subscriptionKey, requestData);
    System.out.println(result);
}
```

## <a name="load-your-time-series-data-and-send-the-request"></a>Uw time series-gegevens laden en de aanvraag verzenden

1. Lees in de JSON-bestand met de gegevens die worden toegevoegd aan de aanvragen in de belangrijkste methode van uw toepassing.

2. Aanroepen van de twee anomaliedetectie-detectie-functies die eerder is gemaakt.
    
    ```java
    public static void main(String[] args) throws Exception {
        String requestData = new String(Files.readAllBytes(Paths.get(dataPath)), "UTF-8");
        detectAnomaliesBatch(requestData);
        detectAnomaliesLatest(requestData);
    }
    ```

### <a name="example-response"></a>Voorbeeld van een antwoord

Een geslaagde respons wordt geretourneerd in JSON-indeling. Klik op de onderstaande koppelingen voor het weergeven van het JSON-antwoord op GitHub:
* [Voorbeeld van een antwoord voor de detectie van batch](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/batch-response.json)
* [Voorbeeld van de laatste punt detectie antwoord](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/latest-point-response.json)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Naslaginformatie over REST API](https://westus2.dev.cognitive.microsoft.com/docs/services/AnomalyDetector/operations/post-timeseries-entire-detect)