---
title: Het gebruik van de Anomaliedetectie Finder-API met behulp van Java - Microsoft Cognitive Services | Microsoft Docs
description: Get-informatie en codevoorbeelden om u te helpen snel aan de slag met Java en de detectie van afwijkingen in Cognitive Services.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.subservice: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: kefre
ms.openlocfilehash: 7b85faf2e1803b40f3f2f0e49c5672b96e7961f5
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55224340"
---
# <a name="use-the-anomaly-finder-api-with-java"></a>De API voor Afwijkingsdetectie gebruiken met Java

[!INCLUDE [PrivatePreviewNote](../../../../../includes/cognitive-services-anomaly-finder-private-preview-note.md)]

In dit artikel bevat informatie en voorbeelden van code om u te helpen snel aan de slag met behulp van de API voor Afwijkingsdetectie taak van het resultaat van de detectie van afwijkingen voor time series-gegevens ophalen uit te voeren met behulp van Java.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="getting-anomaly-points-with-the-anomaly-detection-api-using-java"></a>Anomaliedetectie punten ophalen met de API voor Afwijkingsdetectie met behulp van Java

[!INCLUDE [DataContract](../includes/datacontract.md)]

### <a name="example-of-time-series-data"></a>Voorbeeld van time series-gegevens

Het voorbeeld van de tijd gegevenspunten van reeks is als volgt.

[!INCLUDE [Request](../includes/request.md)]

### <a name="analyze-data-and-get-anomaly-points-java-example"></a>Gegevens analyseren en anomaliedetectie punten Java-voorbeeld

De als voorbeeld wilt uitvoeren, moet u de volgende stappen uitvoeren:
1. Maak een nieuwe opdrachtregel-App.
2. Vervang de Main-klasse door de volgende code (behoud `package`-instructies).
3. Vervang de waarde `[YOUR_SUBSCRIPTION_KEY]` door de geldige abonnementssleutel.
4. Vervang de `[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]` met het voorbeeld of uw eigen gegevenspunten.
5. Download deze globale bibliotheken van de Maven-opslagplaats naar de map `lib` in uw project:
   * `org.apache.httpcomponents:httpclient:4.5.2`
6. Voer 'Main' uit.

```java

import org.apache.http.HttpEntity;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.StringEntity;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.util.EntityUtils;

public class Main {
    // **********************************************
    // *** Update or verify the following values. ***
    // **********************************************

    // Replace the subscriptionKey string value with your valid subscription key.
    public static final String subscriptionKey = "[YOUR_SUBSCRIPTION_KEY]";

    public static final String uriBase = "https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection";

    public static void main(String[] args) {
        final String content = "[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]";

        CloseableHttpClient client = HttpClients.createDefault();
        HttpPost request = new HttpPost(uriBase);

        // Request headers.
        request.setHeader("Content-Type", "application/json");
        request.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);

        try {
            StringEntity params = new StringEntity(content);
            request.setEntity(params);

            CloseableHttpResponse response = client.execute(request);
            try {
                HttpEntity respEntity = response.getEntity();
                if (respEntity != null) {
                    System.out.println("----------");
                    System.out.println(response.getStatusLine());
                    System.out.println("Response content is :\n");
                    System.out.println(EntityUtils.toString(respEntity, "utf-8"));
                    System.out.println("----------");
                }
            } catch (Exception respEx) {
                respEx.printStackTrace();
            } finally {
                response.close();
            }

        } catch (Exception ex) {
            System.err.println("Exception on Anomaly Detection: " + ex.getMessage());
            ex.printStackTrace();
        } finally {
            try {
                client.close();
            } catch (Exception e) {
                System.err.println("Exception on closing HttpClient: " + e.getMessage());
                e.printStackTrace();
            }
        }
    }
}

```

### <a name="example-response"></a>Voorbeeld van een antwoord

Een geslaagd antwoord wordt geretourneerd in JSON-indeling. Voorbeeld van een antwoord is als volgt.
[!INCLUDE [Response](../includes/response.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Java-app](../tutorials/java-tutorial.md)
