---
title: 'Snelstartgids: Emoties op gezichten in een afbeelding herkennen - Emotion-API, Java'
description: Hier vindt u informatie en een codevoorbeeld om snel aan de slag te gaan met de Emotion-API en Java voor Android.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: quickstart
ms.date: 05/23/2017
ms.author: anroth
ROBOTS: NOINDEX
ms.openlocfilehash: 5c845c07eff2d4627dd756a49f4b3fee2fca6a7a
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/03/2018
ms.locfileid: "48237617"
---
# <a name="quickstart-build-an-app-to-recognize-emotions-on-faces-in-an-image"></a>Snelstart: een app bouwen voor het herkennen van emoties op gezichten in een afbeelding.

> [!IMPORTANT]
> De Emotion-API wordt op 15 februari 2019 afgeschaft. De mogelijkheid voor de herkenning van emoties is nu algemeen beschikbaar als onderdeel van de [Face-API](https://docs.microsoft.com/azure/cognitive-services/face/). 

In dit artikel vindt u informatie en een codevoorbeeld om snel aan de slag te gaan met de [methode Recognize van de Emotion-API](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) in de Android-clientbibliotheek van de Emotion-API. Het voorbeeld laat zien hoe u Java kunt gebruiken voor het herkennen van de emoties die worden uitgedrukt door mensen.

## <a name="prerequisites"></a>Vereisten
* De Java for Android SDK voor de Emotion-API kunt u [hier](https://github.com/Microsoft/Cognitive-emotion-android) downloaden
* U kunt [hier](https://azure.microsoft.com/try/cognitive-services/) een gratis abonnementssleutel ophalen

## <a name="recognize-emotions-java-for-android-example-request"></a>Voorbeeldaanvraag in Java voor Android voor het herkennen van emoties

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

## <a name="recognize-emotions-sample-response"></a>Voorbeeldantwoord voor herkennen van emoties
Een geslaagde aanroep retourneert een matrix van gezichtsvermeldingen en de bijbehorende emotiescores, in aflopende volgorde gerangschikt op grootte van gezichtsrechthoek. Een leeg antwoord geeft aan dat er geen gezichten zijn gedetecteerd. Een emotievermelding bevat de volgende velden:
* faceRectangle: locatie van de rechthoek met het gezicht in de afbeelding.
* scores: emotiescores voor elk gezicht in de afbeelding.

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
