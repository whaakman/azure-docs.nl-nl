---
title: 'Quickstart: Emoties herkennen op gezichten in een afbeelding - Emotion-API, JavaScript'
titlesuffix: Azure Cognitive Services
description: Bekijk informatie en codevoorbeelden om snel aan de slag te gaan met de Emotion-API en JavaScript.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.subservice: emotion-api
ms.topic: quickstart
ms.date: 05/23/2017
ms.author: anroth
ROBOTS: NOINDEX
ms.openlocfilehash: 3c6e5b0d759aeb8aa0b06e359f1f48eaf6c44520
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55208943"
---
# <a name="quickstart-build-an-app-to-recognize-emotions-on-faces-in-an-image"></a>Quickstart: Een app bouwen voor het herkennen van emoties op een gezicht in een afbeelding.

> [!IMPORTANT]
> De Emotion-API wordt op 15 februari 2019 afgeschaft. De mogelijkheid voor de herkenning van emoties is nu algemeen beschikbaar als onderdeel van de [Face-API](https://docs.microsoft.com/azure/cognitive-services/face/). 

In dit artikel vindt u informatie en codevoorbeelden om met behulp van JavaScript en de [methode Recognize van de Emotion-API](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) de emoties te herkennen die worden uitgedrukt door een of meer personen in een afbeelding.

## <a name="prerequisite"></a>Vereiste
* Haal uw gratis abonnementssleutel [hier](https://azure.microsoft.com/try/cognitive-services/) op. Als u een Azure-abonnement hebt, kunt u ook een Emotion-API-resource maken en uw abonnementssleutel en eindpunt daar ophalen.

![Emotion-API-resource maken](../Images/create-resource.png)

## <a name="recognize-emotions-javascript-example-request"></a>Voorbeeldaanvraag in JavaScript voor het herkennen van emoties

Kopieer het volgende en sla het op in een bestand zoals `test.html`. Wijzig de aanvraag `url` om de locatie te gebruiken waar u de abonnementssleutels hebt verkregen en vervang de waarde 'Ocp-Apim-Subscription-Key' door een geldige abonnementssleutel. Deze kunt u vinden in de Azure-portal, in de gedeeltes Overzicht en Sleutels van de Emotion-API-resource.

![API-eindpunt](../Images/api-url.png)

![API-abonnementssleutel](../Images/keys.png)

Wijzig de aanvraagbody naar de locatie van een afbeelding die u wilt gebruiken. Sleep het bestand naar uw browser en zet het neer om het voorbeeld uit te voeren.

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
