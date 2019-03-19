---
title: Detecteren van afbeeldingstypen - Computer Vision
titleSuffix: Azure Cognitive Services
description: Concepten met betrekking tot de installatiekopie type detectie-functie van de Computer Vision-API.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 03/11/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 37cdac16a51a30bdaf1ba0266bab7fdd1f2990f0
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57895444"
---
# <a name="detecting-image-types-with-computer-vision"></a>Detecteren van afbeeldingstypen met Computer Vision

Met de [analyseren installatiekopie](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) API, Computer Vision-het type inhoud van afbeeldingen kunt analyseren die aangeeft of een afbeelding illustraties of een lijntekening is.

## <a name="detecting-clip-art"></a>Illustraties detecteren

Computer Vision analyseert een installatiekopie en classificeert de kans van de installatiekopie wordt illustraties op een schaal van 0 tot en met 3, zoals beschreven in de volgende tabel.

| Value | Betekenis |
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

Zie de [analyseren installatiekopie](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) referentiedocumentatie voor informatie over het detecteren van afbeeldingstypen.
