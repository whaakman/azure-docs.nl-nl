---
title: Gezichten detecteren
titleSuffix: Computer Vision - Cognitive Services - Azure
description: Concepten met betrekking tot het detecteren van gezichten wordt uitgevoerd met Computer Vision in Azure Cognitive Services.
services: cognitive-services
author: deken
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: article
ms.date: 08/29/2018
ms.author: v-deken
ms.openlocfilehash: abf0dc0ea80685130f6566ae962d2bed124e0924
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "44725360"
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