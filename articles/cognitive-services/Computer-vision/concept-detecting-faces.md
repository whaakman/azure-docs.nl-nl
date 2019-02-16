---
title: Detecteren van gezichten - Computer Vision
titleSuffix: Azure Cognitive Services
description: Concepten met betrekking tot de face detection-functie van de Computer Vision-API.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 1056b8be113d56342aea8f83d5325737f7ecb93b
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/15/2019
ms.locfileid: "56308448"
---
# <a name="face-detection-with-computer-vision"></a>Gezichtsdetectie met Computer Vision

Computer Vision kunt Detecteer menselijke gezichten binnen een installatiekopie en de leeftijd, geslacht en rechthoek voor elke gedetecteerde gezicht genereren. 

> [!NOTE]
> Deze functie wordt ook aangeboden door de Azure [Face](/azure/cognitive-services/face/) service. Zie dit alternatief voor meer gedetailleerde analyse, met inbegrip van gezichts-id te maken krijgt en detectie met zich meebrengen. 

## <a name="face-detection-examples"></a>Face detection-voorbeelden

Het volgende voorbeeld ziet u de JSON-antwoord geretourneerd door de Computer Vision voor een afbeelding met een enkele menselijke gezichten.

![Vision-analyse Vrouw op dak-gezicht](./Images/woman_roof_face.png)

```json
{
    "faces": [
        {
            "age": 23,
            "gender": "Female",
            "faceRectangle": {
                "top": 45,
                "left": 194,
                "width": 44,
                "height": 44
            }
        }
    ],
    "requestId": "8439ba87-de65-441b-a0f1-c85913157ecd",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Png"
    }
}
```

Het volgende voorbeeld ziet u de JSON-antwoord geretourneerd voor een afbeelding met meerdere menselijke gezichten.

![Vision familie foto Face analyseren](./Images/family_photo_face.png)

```json
{
    "faces": [
        {
            "age": 11,
            "gender": "Male",
            "faceRectangle": {
                "top": 62,
                "left": 22,
                "width": 45,
                "height": 45
            }
        },
        {
            "age": 11,
            "gender": "Female",
            "faceRectangle": {
                "top": 127,
                "left": 240,
                "width": 42,
                "height": 42
            }
        },
        {
            "age": 37,
            "gender": "Female",
            "faceRectangle": {
                "top": 55,
                "left": 200,
                "width": 41,
                "height": 41
            }
        },
        {
            "age": 41,
            "gender": "Male",
            "faceRectangle": {
                "top": 45,
                "left": 103,
                "width": 39,
                "height": 39
            }
        }
    ],
    "requestId": "3a383cbe-1a05-4104-9ce7-1b5cf352b239",
    "metadata": {
        "height": 230,
        "width": 300,
        "format": "Png"
    }
}
```

## <a name="next-steps"></a>Volgende stappen

Zie de [analyseren installatiekopie](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) verwijzen naar de documentatie voor meer informatie over het gebruik van de face detection-functie.
