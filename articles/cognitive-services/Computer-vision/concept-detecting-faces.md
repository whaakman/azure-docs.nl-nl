---
title: Detecteren van gezichten - Computer Vision
titleSuffix: Azure Cognitive Services
description: Concepten met betrekking tot het detecteren van gezichten wordt uitgevoerd met de Computer Vision-API.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.openlocfilehash: bf358d1e8f60f989ced8db966bbf0a5179fab25b
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49342310"
---
# <a name="detecting-faces"></a>Gezichten detecteren

Computer Vision detecteert menselijke gezichten op een afbeelding en genereert de leeftijd, geslacht en rechthoek voor elk gezicht gedetecteerd. Computer Vision biedt een subset van de functionaliteit die beschikbaar is in [Face](/azure/cognitive-services/face/) en u kunt de Face-service gebruiken voor gedetailleerdere analyse, zoals gezichtsherkenning en houdingsdetectie.  

## <a name="face-detection-examples"></a>Face detection-voorbeelden

Het eerste voorbeeld ziet u de JSON-antwoord geretourneerd door de Computer Vision voor een afbeelding met een enkele menselijke gezichten.

![Vision vrouw plafond Face analyseren](./Images/woman_roof_face.png)

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

Het tweede voorbeeld ziet u de JSON-antwoord geretourneerd voor een afbeelding met meerdere menselijke gezichten.

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

Kennis met concepten over [domeinspecifieke inhoud detecteren](concept-detecting-domain-content.md).