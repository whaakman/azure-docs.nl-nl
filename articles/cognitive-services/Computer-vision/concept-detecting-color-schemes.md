---
title: Kleurenschema - Computer Vision-detecteren
titleSuffix: Azure Cognitive Services
description: Concepten met betrekking tot het kleurenschema detecteren in afbeeldingen met behulp van de Computer Vision-API.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 02/08/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 6b25da9b2569b0185d41684c45a22a3eb3377511
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/15/2019
ms.locfileid: "56313072"
---
# <a name="detect-color-schemes-in-images"></a>Kleurenschema's detecteren in afbeeldingen

Computer Vision analyseert de kleuren in een afbeelding voor drie verschillende kenmerken: de dominante voorgrondkleur, de dominante achtergrondkleur en de set dominante kleuren voor de installatiekopie als geheel. Kleuren deel uitmaken van de set geretourneerd: zwart, blauw, brown, grijs, groen, oranje, roze, paars, rood, groenblauwe, wit en geel. 

Computer Vision extraheert ook een accentkleur, die de meeste levendige kleur in de afbeelding, op basis van een combinatie van dominante kleuren en de verzadiging vertegenwoordigt. De accentkleur geretourneerd als een HTML-code voor hexadecimale kleur. 

Computer Vision retourneert ook een Booleaanse waarde die aangeeft of een afbeelding in zwart-wit.

## <a name="color-scheme-detection-examples"></a>Kleur-schema-detectie-voorbeelden

Het volgende voorbeeld wordt de JSON-antwoord geretourneerd door visie op de Computer bij het detecteren van het kleurenschema van de installatiekopie van het voorbeeld. In dit geval wordt de installatiekopie van het voorbeeld is niet een zwart-wit-installatiekopie, maar de dominante voorgrond en achtergrondkleuren zijn zwart en de dominante kleuren voor de afbeelding als een geheel zwart-wit zijn.

![Berglandschap](./Images/mountain_vista.png)

```json
{
    "color": {
        "dominantColorForeground": "Black",
        "dominantColorBackground": "Black",
        "dominantColors": ["Black", "White"],
        "accentColor": "BB6D10",
        "isBwImg": false
    },
    "requestId": "0dc394bf-db50-4871-bdcc-13707d9405ea",
    "metadata": {
        "height": 202,
        "width": 300,
        "format": "Jpeg"
    }
}
```

### <a name="dominant-color-examples"></a>Voorbeelden van de overheersende kleur

De volgende tabel ziet u de geretourneerde voorgrond, achtergrond en de kleuren van de afbeelding van elke voorbeeldafbeelding.

| Installatiekopie | Dominante kleuren |
|-------|-----------------|
|![Een wit bloem met een groene achtergrond](./Images/flower.png)| Voorgrond: Zwart<br/>Achtergrond: Wit<br/>Kleuren: Zwart, wit-groen|
![Een trein een station wilt doorlopen](./Images/train_station.png) | Voorgrond: Zwart<br/>Achtergrond: Zwart<br/>Kleuren: Zwart |

### <a name="accent-color-examples"></a>Accent kleur voorbeelden

 De volgende tabel ziet de accentkleur van de geretourneerde als een hexadecimale waarde van elke voorbeeldafbeelding de HTML-kleur.

| Installatiekopie | Accentkleur |
|-------|--------------|
|![Een persoon die permanent op een rock mountain zonsondergang](./Images/mountain_vista.png) | #BB6D10 |
|![Een wit bloem met een groene achtergrond](./Images/flower.png) | #C6A205 |
|![Een trein een station wilt doorlopen](./Images/train_station.png) | #474A84 |

### <a name="black--white-detection-examples"></a>Detectie van zwart- wit -voorbeelden

De volgende tabel ziet de Computer Vision-zwart-wit-evaluatie in de voorbeeld-installatiekopieën.

| Installatiekopie | Zwart- wit? |
|-------|----------------|
|![Een zwart-wit beeld van de gebouwen in Manhattan](./Images/bw_buildings.png) | true |
|![Een blauwe huis en de voorste yard](./Images/house_yard.png) | false |

## <a name="next-steps"></a>Volgende stappen

Kennis met concepten over [detecteren afbeeldingstypen](concept-detecting-image-types.md).
