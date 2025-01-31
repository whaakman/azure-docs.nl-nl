---
title: 'Quickstart: Gezichten in een afbeelding detecteren met de Azure REST API en cURL'
titleSuffix: Azure Cognitive Services
description: In deze snelstart gebruikt u de Azure Face REST API met cURL om gezichten in een afbeelding te detecteren.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 07/03/2019
ms.author: pafarley
ms.openlocfilehash: 68cdd147977294954051735d70307305aa5dc0cb
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67603315"
---
# <a name="quickstart-detect-faces-in-an-image-using-the-face-rest-api-and-curl"></a>Quickstart: Gezichten in een afbeelding detecteren met de Face REST API en cURL

In deze snelstart gebruikt u de Azure Face REST API met cURL om menselijke gezichten in een afbeelding te detecteren.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint. 

## <a name="prerequisites"></a>Vereisten

- Een Face-API-abonnementssleutel. U kunt een abonnementssleutel voor een gratis proefversie downloaden van [Cognitive Services proberen](https://azure.microsoft.com/try/cognitive-services/?api=face-api). Of volg de instructies in [Een Cognitive Services-account maken](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) om u te abonneren op de Face-API-service en uw sleutel op te halen.

## <a name="write-the-command"></a>De opdracht schrijven
 
U gebruikt een opdracht als volgt aanroepen van de Face-API en face kenmerkgegevens ophalen uit een afbeelding. Kopieer eerst de code in een teksteditor&mdash;u moet wijzigingen aanbrengen in bepaalde gedeelten van de opdracht voordat u deze kunt uitvoeren.

```shell
curl -H "Ocp-Apim-Subscription-Key: <Subscription Key>" "https://westcentralus.api.cognitive.microsoft.com/face/v1.0/detect?returnFaceId=true&returnFaceLandmarks=false&returnFaceAttributes=age,gender,headPose,smile,facialHair,glasses,emotion,hair,makeup,occlusion,accessories,blur,exposure,noise" -H "Content-Type: application/json" --data-ascii "{\"url\":\"https://upload.wikimedia.org/wikipedia/commons/c/c3/RH_Louise_Lillian_Gish.jpg\"}"
```

### <a name="subscription-key"></a>Abonnementssleutel
Vervang `<Subscription Key>` door een geldige Face-abonnementssleutel.

### <a name="face-endpoint-url"></a>Eindpunt-URL voor Face

De URL `https://westcentralus.api.cognitive.microsoft.com/face/v1.0/detect` geeft het Azure Face-eindpunt aan waarvoor een query moet worden uitgevoerd. Mogelijk moet u het eerste deel van deze URL zodat deze overeenkomen met de regio die overeenkomt met de abonnementssleutel van uw wijzigen. Zie de [Face-API-documenten](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) voor een lijst van alle eindpunten voor de regio.

### <a name="url-query-string"></a>Queryreeks voor URL

De queryreeks van de Face-eindpunt-URL geeft op welke gezichtskenmerken moeten worden opgehaald. U kunt deze queryreeks wijzigen afhankelijk van het beoogde gebruik.

```
?returnFaceId=true&returnFaceLandmarks=false&returnFaceAttributes=age,gender,headPose,smile,facialHair,glasses,emotion,hair,makeup,occlusion,accessories,blur,exposure,noise
```

### <a name="image-source-url"></a>Bron-URL van afbeelding
De bron-URL geeft aan welke afbeelding moet worden gebruikt als invoer. U kunt dit om te verwijzen naar een andere afbeelding die u wilt analyseren wijzigen.

```
https://upload.wikimedia.org/wikipedia/commons/c/c3/RH_Louise_Lillian_Gish.jpg
``` 

## <a name="run-the-command"></a>De opdracht uitvoeren

Nadat u de wijzigingen hebt aangebracht, opent u een opdrachtprompt en voert u de nieuwe opdracht in. De gegevens over het gezicht worden nu in het consolevenster weergegeven als JSON-gegevens. Bijvoorbeeld:

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

In deze snelstart hebt u een cURL-opdracht geschreven waarmee de Azure Face-API wordt aangeroepen om gezichten in een afbeelding te detecteren en de gezichtskenmerken te retourneren. Lees het naslagmateriaal bij de Face-API voor meer informatie.

> [!div class="nextstepaction"]
> [Face-API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
