---
title: Emotion-API Java voor Android snel aan de slag | Microsoft Docs
description: Get-informatie en een voorbeeld van code kunt u snel aan de slag met de Emotion-API met behulp van Java voor Android in cognitieve Services.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 05/23/2017
ms.author: anroth
ms.openlocfilehash: 64a5c4e761653748c4328e310f9a399fe62f9149
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344745"
---
# <a name="emotion-api-java-for-android-quick-start"></a>Emotion-API Java voor Android snel starten

> [!IMPORTANT]
> Video API Preview eindigt op 30 oktober 2017. Het nieuwe [Video indexeerfunctie API Preview](https://azure.microsoft.com/services/cognitive-services/video-indexer/) eenvoudig inzichten extraheren van video's en ervaringen van inhoud, zoals de lijst met zoekresultaten verbeteren doordat gesproken woorden, vlakken tekens en emoties. [Meer informatie](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview).

In dit artikel vindt u informatie en een voorbeeld van code kunt u snel aan de slag met de [Emotion herkennen methode](https://dev.projectoxford.ai/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) in de Emotion-API Android clientbibliotheek. Het voorbeeld laat zien hoe u Java kunt gebruiken voor het herkennen van de emoties uitgedrukt door mensen. 

## <a name="prerequisites"></a>Vereisten
* Ophalen van de Java Emotion-API voor Android SDK [hier](https://github.com/Microsoft/Cognitive-emotion-android)
* De sleutel van uw gratis abonnement ophalen [hier](https://azure.microsoft.com/try/cognitive-services/)

## <a name="recognize-emotions-java-for-android-example-request"></a>Emoties Java herkennen voor Android Voorbeeldaanvraag

```java
// // This sample uses the Apache HTTP client from HTTP Components (http://hc.apache.org/httpcomponents-client-ga/)
import java.net.URI;
import org.apache.http.HttpEntity;
import org.apache.http.HttpResponse;
import org.apache.http.client.HttpClient;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.StringEntity;
import org.apache.http.client.utils.URIBuilder;
import org.apache.http.impl.client.DefaultHttpClient;
import org.apache.http.util.EntityUtils;

public class Main
{
    public static void main(String[] args)
    {
        HttpClient httpClient = new DefaultHttpClient();

        try
        {
            // NOTE: You must use the same region in your REST call as you used to obtain your subscription keys.
            //   For example, if you obtained your subscription keys from westcentralus, replace "westus" in the 
            //   URL below with "westcentralus".
            URIBuilder uriBuilder = new URIBuilder("https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize");

            URI uri = uriBuilder.build();
            HttpPost request = new HttpPost(uri);

            // Request headers. Replace the example key below with your valid subscription key.
            request.setHeader("Content-Type", "application/json");
            request.setHeader("Ocp-Apim-Subscription-Key", "13hc77781f7e4b19b5fcdd72a8df7156");

            // Request body. Replace the example URL below with the URL of the image you want to analyze.
            StringEntity reqEntity = new StringEntity("{ \"url\": \"http://example.com/images/test.jpg\" }");
            request.setEntity(reqEntity);

            HttpResponse response = httpClient.execute(request);
            HttpEntity entity = response.getEntity();

            if (entity != null)
            {
                System.out.println(EntityUtils.toString(entity));
            }
        }
        catch (Exception e)
        {
            System.out.println(e.getMessage());
        }
    }
}
```

## <a name="recognize-emotions-sample-response"></a>Voorbeeldreactie emoties herkennen
Een geslaagde aanroep retourneert een matrix met face-vermeldingen en hun bijbehorende emotion-scores, gerangschikt op face rechthoek grootte in aflopende volgorde. Een leeg antwoord geeft aan dat er geen vlakken zijn gedetecteerd. Een emotion-vermelding bevat de volgende velden:
* faceRectangle - locatie van de rechthoek van gezicht in de installatiekopie.
* scores - Emotion scores voor elk vlak in de installatiekopie. 

```json
application/json 
[
  {
    "faceRectangle": {
      "left": 68,
      "top": 97,
      "width": 64,
      "height": 97
    },
    "scores": {
      "anger": 0.00300731952,
      "contempt": 5.14648448E-08,
      "disgust": 9.180124E-06,
      "fear": 0.0001912825,
      "happiness": 0.9875571,
      "neutral": 0.0009861537,
      "sadness": 1.889955E-05,
      "surprise": 0.008229999
    }
  }
]
