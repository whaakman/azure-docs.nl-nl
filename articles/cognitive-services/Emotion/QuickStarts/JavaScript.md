---
title: Snel starten van emotion-API JavaScript | Microsoft Docs
description: Get-informatie en codevoorbeelden kunt u snel aan de slag met de Emotion-API met JavaScript in cognitieve Services.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 05/23/2017
ms.author: anroth
ms.openlocfilehash: 2578b0212f9b4a6483402074d7c9eff73e51ca6b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344747"
---
# <a name="emotion-api-javascript-quick-start"></a>Emotion-API JavaScript snel starten

> [!IMPORTANT]
> Video API Preview eindigt op 30 oktober 2017. Het nieuwe [Video indexeerfunctie API Preview](https://azure.microsoft.com/services/cognitive-services/video-indexer/) eenvoudig inzichten extraheren van video's en ervaringen van inhoud, zoals de lijst met zoekresultaten verbeteren doordat gesproken woorden, vlakken tekens en emoties. [Meer informatie](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview).

In dit artikel vindt u informatie en codevoorbeelden kunt u snel aan de slag met de [Emotion-API herkennen methode](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) met JavaScript voor het herkennen van de emoties uitgedrukt in een of meer personen in een afbeelding.

## <a name="prerequisite"></a>Vereiste
* Sleutel voor gratis abonnement ophalen [hier](https://azure.microsoft.com/try/cognitive-services/), of als u een Azure-abonnement hebben een Emotion-API-resource maken en uw Abonnementssleutel en het eindpunt er ophalen.

![Emotion-API-Resource maken](../Images/create-resource.png)

## <a name="recognize-emotions-javascript-example-request"></a>Herkent emoties JavaScript Voorbeeldaanvraag

Kopieer de volgende en sla deze op een bestand, zoals `test.html`. Wijzig de aanvraag `url` gebruiken de locatie waar u de sleutels van uw abonnement hebt verkregen en vervang de waarde 'Ocp-Apim-Subscription-Key' door uw sleutel geldig abonnement. Deze kunnen worden gevonden in de Azure portal in de secties overzicht en de sleutels van uw resource Emotion-API, respectievelijk. 

![API-eindpunt](../Images/api-url.png)

![Abonnement-API-sleutel](../Images/keys.png)

en wijzig de hoofdtekst van de aanvraag naar de locatie van een installatiekopie die u wilt gebruiken. Het voorbeeld slepen en neerzetten het bestand in uw browser uitvoeren.

```html
<!DOCTYPE html>
<html>
<head>
    <title>JSSample</title>
    <script src="http://ajax.googleapis.com/ajax/libs/jquery/1.9.0/jquery.min.js"></script>
</head>

<h2>Face Rectangle</h2>
<ul id="faceRectangle">
<!-- Will populate list with response content -->
</ul>

<h2>Emotions</h2>
<ul id="scores">
<!-- Will populate list with response content -->
</ul>

<body>

<script type="text/javascript">
    $(function() {
        // No query string parameters for this API call.
        var params = { };
      
        $.ajax({
            // NOTE: You must use the same location in your REST call as you used to obtain your subscription keys.
            //   For example, if you obtained your subscription keys from westcentralus, replace "westus" in the 
            //   URL below with "westcentralus".
            url: "https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize?" + $.param(params),
            beforeSend: function(xhrObj){
                // Request headers, also supports "application/octet-stream"
                xhrObj.setRequestHeader("Content-Type","application/json");

                // NOTE: Replace the "Ocp-Apim-Subscription-Key" value with a valid subscription key.
                xhrObj.setRequestHeader("Ocp-Apim-Subscription-Key","<your subscription key>");
            },
            type: "POST",
            // Request body
            data: '{"url": "<your image url>"}',
        }).done(function(data) {
            // Get face rectangle dimensions
            var faceRectangle = data[0].faceRectangle;
            var faceRectangleList = $('#faceRectangle');

            // Append to DOM
            for (var prop in faceRectangle) {
                faceRectangleList.append("<li> " + prop + ": " + faceRectangle[prop] + "</li>");
            }
            
            // Get emotion confidence scores
            var scores = data[0].scores;
            var scoresList = $('#scores');

            // Append to DOM
            for(var prop in scores) {
                scoresList.append("<li> " + prop + ": " + scores[prop] + "</li>")
            }
        }).fail(function(err) {
            alert("Error: " + JSON.stringify(err));
        });
    });
</script>
</body>
</html>
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
