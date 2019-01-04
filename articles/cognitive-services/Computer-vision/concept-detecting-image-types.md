---
title: Detecteren van afbeeldingstypen - Computer Vision
titleSuffix: Azure Cognitive Services
description: Concepten met betrekking tot de installatiekopie type detectie-functie van de Computer Vision-API.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 04062d5625126712c5f14c41d610d55caf4c28b5
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2018
ms.locfileid: "53583099"
---
# <a name="detecting-image-types-with-computer-vision"></a>Detecteren van afbeeldingstypen met Computer Vision

Computer Vision kunt het inhoudstype van installatiekopieën analyseren door die aangeeft of een afbeelding illustraties, is de kans op een schaal, of een lijntekening waardering.

## <a name="detecting-clip-art"></a>Illustraties detecteren

Computer Vision analyseert een installatiekopie en classificeert de kans van de installatiekopie wordt illustraties op een schaal van 0 tot en met 3, zoals beschreven in de volgende tabel.

| Waarde | Betekenis |
|-------|---------|
| 0 | Non-clip-art (geen illustratie) |
| 1 | niet-eenduidige |
| 2 | Normaal illustraties |
| 3 | goede illustraties |

### <a name="clip-art-detection-examples"></a>Illustratie van geavanceerde detectie van voorbeelden

De volgende JSON-antwoorden ziet u wat Computer Vision geretourneerd bij het beoordelen van de kans van de voorbeeld-installatiekopieën wordt illustraties.

![Een afbeelding van een segment van kaas](./Images/cheese_clipart.png)

```json
{
    "imageType": {
        "clipArtType": 3,
        "lineDrawingType": 0
    },
    "requestId": "88c48d8c-80f3-449f-878f-6947f3b35a27",
    "metadata": {
        "height": 225,
        "width": 300,
        "format": "Jpeg"
    }
}
```

![Een blauwe huis en de voorste yard](./Images/house_yard.png)

```json
{
    "imageType": {
        "clipArtType": 0,
        "lineDrawingType": 0
    },
    "requestId": "a9c8490a-2740-4e04-923b-e8f4830d0e47",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Jpeg"
    }
}
```

## <a name="detecting-line-drawings"></a>Tekeningen detecteren

Computer Vision analyseert een installatiekopie en retourneert een Booleaanse waarde die aangeeft of de afbeelding een lijntekening is.

### <a name="line-drawing-detection-examples"></a>Voorbeelden van lijntekening detectie

Computer Vision retourneert wanneer die aangeeft of de installatiekopieën van het voorbeeld regel tekeningen zijn ziet u de volgende JSON-antwoorden.

![Een lijntekening-installatiekopie van een lion](./Images/lion_drawing.png)

```json
{
    "imageType": {
        "clipArtType": 2,
        "lineDrawingType": 1
    },
    "requestId": "6442dc22-476a-41c4-aa3d-9ceb15172f01",
    "metadata": {
        "height": 268,
        "width": 300,
        "format": "Jpeg"
    }
}
```

![Een wit bloem met een groene achtergrond](./Images/flower.png)

```json
{
    "imageType": {
        "clipArtType": 0,
        "lineDrawingType": 0
    },
    "requestId": "98437d65-1b05-4ab7-b439-7098b5dfdcbf",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Jpeg"
    }
}
```

## <a name="next-steps"></a>Volgende stappen

Kennis met concepten over [installatiekopieën taggen](concept-tagging-images.md) en [categoriseren van beelden](concept-categorizing-images.md).