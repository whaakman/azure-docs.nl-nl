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
ms.date: 08/29/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 0793f572e043248af409e65cca4fd854f1371900
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55880872"
---
# <a name="detect-color-schemes-in-images"></a>Kleurenschema's detecteren in afbeeldingen

Computer Vision extraheert kleuren uit een afbeelding. De kleuren worden vervolgens in drie verschillende contexten geanalyseerd: de dominante voorgrondkleur, de dominante achtergrondkleur en de dominante kleuren voor de installatiekopie als geheel. Ze worden in 12 dominante Accentkleuren gegroepeerd. Deze accentkleuren zijn zwart, blauw, bruin, grijs, groen, oranje, roze, paars, rood, groenblauw, wit en geel. Computer Vision analyseert de kleuren die is geëxtraheerd uit een afbeelding om terug te keren een accentkleur dat de meest levendige kleur voor de installatiekopie voor gebruikers, door een combinatie van dominante kleuren en de verzadiging vertegenwoordigt. Afhankelijk van de kleuren in een afbeelding kunnen ofwel gewoon zwart-wit ofwel accentkleuren worden geretourneerd in hexadecimale kleurcodes. Computer Vision ook retourneert een Booleaanse waarde die aangeeft of een installatiekopie van een zwart- wit.

## <a name="color-scheme-detection-examples"></a>Kleur-schema-detectie-voorbeelden

Het volgende voorbeeld wordt de JSON-antwoord geretourneerd door visie op de Computer bij het detecteren van het kleurenschema van de installatiekopie van het voorbeeld. In dit geval wordt de installatiekopie van het voorbeeld is niet een zwart- wit -installatiekopie, maar de dominante kleuren van voorgrond en achtergrond zwart zijn en de dominante kleuren voor de afbeelding als een geheel zwart-wit zijn.

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

De volgende tabel beschrijft de dominante voorgrond, achtergrond en kleuren voor elke voorbeeldafbeelding geretourneerd door de Computer Vision afbeelding.

| Installatiekopie | Dominante kleuren |
|-------|-----------------|
|![Een wit bloem met een groene achtergrond](./Images/flower.png)| Voorgrond: Zwart<br/>Achtergrond: Wit<br/>Kleuren: Zwart, wit-groen|
![Een trein een station wilt doorlopen](./Images/train_station.png) | Voorgrond: Zwart<br/>Achtergrond: Zwart<br/>Kleuren: Zwart |

### <a name="accent-color-examples"></a>Accent kleur voorbeelden

 De volgende tabel beschrijft de accentkleur als een hexadecimale waarde van elke voorbeeldafbeelding geretourneerd door de Computer Vision de HTML-kleur.

| Installatiekopie | Accentkleur |
|-------|--------------|
|![Een persoon die permanent op een rock mountain zonsondergang](./Images/mountain_vista.png) | #BB6D10 |
|![Een wit bloem met een groene achtergrond](./Images/flower.png) | #C6A205 |
|![Een trein een station wilt doorlopen](./Images/train_station.png) | #474A84 |

### <a name="black--white-detection-examples"></a>Detectie van zwart- wit -voorbeelden

De volgende tabel geeft aan of de voorbeeldafbeelding zwart- wit, zoals geretourneerd door visie op de Computer.

| Installatiekopie | Zwart- wit? |
|-------|----------------|
|![Een zwart-wit beeld van de gebouwen in Manhattan](./Images/bw_buildings.png) | true |
|![Een blauwe huis en de voorste yard](./Images/house_yard.png) | false |

## <a name="next-steps"></a>Volgende stappen

Kennis met concepten over [detecteren afbeeldingstypen](concept-detecting-image-types.md).
