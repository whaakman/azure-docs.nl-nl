---
title: 'Snelstart: Gezichten in een afbeelding detecteren met de REST API en cURL'
titleSuffix: Azure Cognitive Services
description: In deze snelstart detecteert u gezichten in een afbeelding met behulp van de Face-API met cURL.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: quickstart
ms.date: 05/10/2018
ms.author: pafarley
ms.openlocfilehash: ab403ec6a9db4d1a0dc03074044eeb424e4ba875
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49953345"
---
# <a name="quickstart-detect-faces-in-an-image-using-the-rest-api-and-curl"></a>Snelstart: Gezichten in een afbeelding detecteren met de REST API en cURL

In deze snelstart detecteert u gezichten in een afbeelding met behulp van de Face-API.

## <a name="prerequisites"></a>Vereisten

U hebt een abonnementssleutel nodig om het voorbeeld uit te voeren. U kunt abonnementssleutels voor een gratis proefversie downloaden op [Cognitive Services proberen](https://azure.microsoft.com/try/cognitive-services/?api=face-api).

## <a name="detect-faces-in-an-image"></a>Gezichten in een afbeelding detecteren

Gebruik de methode [Face - Detect](https://westcentralus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) om gezichten in een afbeelding te detecteren en gezichtskenmerken te retourneren, waaronder:

* Face ID: de unieke ID die wordt gebruikt in verschillende Face-API-scenario's.
* Gezichtsrechthoek: de coördinaten die de locatie van het gezicht in de afbeelding aangeven (links, boven, breedte en hoogte).
* Oriëntatiepunten: een matrix van 27 gezichtsoriëntatiepunten die verwijzen naar de belangrijkste posities van de gezichtsonderdelen.
* Gezichtskenmerken zoals leeftijd, geslacht, glimlachintensiteit, hoofdhouding en gezichtshaar.

U kunt het voorbeeld uitvoeren aan de hand van de volgende stappen:

1. Open een opdrachtprompt.
2. Vervang `<Subscription Key>` door uw geldige abonnementssleutel.
3. Wijzig zo nodig de URL (`https://westcentralus.api.cognitive.microsoft.com/face/v1.0/detect`) in de locatie waar u de abonnementssleutels hebt verkregen.
4. Wijzig eventueel de te analyseren afbeelding (`"{\"url\":...`).
5. Plak de code in het venster.
6. Voer de opdracht uit.

### <a name="face---detect-request"></a>Face - Detect-aanvraag

> [!NOTE]
> U moet in uw REST-aanroep dezelfde locatie gebruiken waar u uw abonnementssleutels hebt verkregen. Als u bijvoorbeeld uw abonnementssleutels van 'westus' hebt verkregen, vervangt u 'westcentralus' in de onderstaande URL door 'westus'.

```shell
curl -H "Ocp-Apim-Subscription-Key: <Subscription Key>" "https://westcentralus.api.cognitive.microsoft.com/face/v1.0/detect?returnFaceId=true&returnFaceLandmarks=false&returnFaceAttributes=age,gender,headPose,smile,facialHair,glasses,emotion,hair,makeup,occlusion,accessories,blur,exposure,noise" -H "Content-Type: application/json" --data-ascii "{\"url\":\"https://upload.wikimedia.org/wikipedia/commons/c/c3/RH_Louise_Lillian_Gish.jpg\"}"
```

### <a name="face---detect-response"></a>Face - Detect-antwoord

Een geslaagd antwoord wordt geretourneerd in JSON-indeling.

```json
[
  {
    "faceId": "49d55c17-e018-4a42-ba7b-8cbbdfae7c6f",
    "faceRectangle": {
      "top": 131,
      "left": 177,
      "width": 162,
      "height": 162
    },
    "faceAttributes": {
      "smile": 0,
      "headPose": {
        "pitch": 0,
        "roll": 0.1,
        "yaw": -32.9
      },
      "gender": "female",
      "age": 22.9,
      "facialHair": {
        "moustache": 0,
        "beard": 0,
        "sideburns": 0
      },
      "glasses": "NoGlasses",
      "emotion": {
        "anger": 0,
        "contempt": 0,
        "disgust": 0,
        "fear": 0,
        "happiness": 0,
        "neutral": 0.986,
        "sadness": 0.009,
        "surprise": 0.005
      },
      "blur": {
        "blurLevel": "low",
        "value": 0.06
      },
      "exposure": {
        "exposureLevel": "goodExposure",
        "value": 0.67
      },
      "noise": {
        "noiseLevel": "low",
        "value": 0
      },
      "makeup": {
        "eyeMakeup": true,
        "lipMakeup": true
      },
      "accessories": [],
      "occlusion": {
        "foreheadOccluded": false,
        "eyeOccluded": false,
        "mouthOccluded": false
      },
      "hair": {
        "bald": 0,
        "invisible": false,
        "hairColor": [
          {
            "color": "brown",
            "confidence": 1
          },
          {
            "color": "black",
            "confidence": 0.87
          },
          {
            "color": "other",
            "confidence": 0.51
          },
          {
            "color": "blond",
            "confidence": 0.08
          },
          {
            "color": "red",
            "confidence": 0.08
          },
          {
            "color": "gray",
            "confidence": 0.02
          }
        ]
      }
    }
  }
]
```

## <a name="next-steps"></a>Volgende stappen

Verken de Face-API's die worden gebruikt om menselijke gezichten in een afbeelding te detecteren, om de gezichten af ​​te bakenen met rechthoeken en kenmerken als leeftijd en geslacht te retourneren.

> [!div class="nextstepaction"]
> [Face-API's](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
