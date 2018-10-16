---
title: Kleurenschema - Computer Vision-detecteren
titleSuffix: Azure Cognitive Services
description: Concepten met betrekking tot het kleurenschema detecteren in afbeeldingen met behulp van de Computer Vision-API.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.openlocfilehash: 5d0cb6ca751c844846288e8fe26f6ae542e89831
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49339489"
---
# <a name="detecting-color-schemes"></a>Kleurenschema’s detecteren

Computer Vision extraheert kleuren uit een afbeelding. De kleuren worden vervolgens in drie verschillende contexten geanalyseerd: de dominante voorgrondkleur, de dominante achtergrondkleur en de dominante kleuren voor de installatiekopie als geheel. Ze worden in 12 dominante Accentkleuren gegroepeerd. Deze Accentkleuren zijn zwart, blauw, brown, grijs, groen, oranje, roze, paars, rood, groenblauwe, wit en geel. Computer Vision analyseert de kleuren die is geëxtraheerd uit een afbeelding om terug te keren een accentkleur dat de meest levendige kleur voor de installatiekopie voor gebruikers, door een combinatie van dominante kleuren en de verzadiging vertegenwoordigt. Afhankelijk van de kleuren in een afbeelding, kunnen de eenvoudige zwart-wit of Accentkleuren in hexadecimale kleurcodes worden geretourneerd. Computer Vision ook retourneert een Booleaanse waarde die aangeeft of een installatiekopie van een zwart- wit.

## <a name="color-scheme-detection-examples"></a>Kleur-schema-detectie-voorbeelden

Het volgende voorbeeld wordt de JSON-antwoord geretourneerd door visie op de Computer bij het detecteren van het kleurenschema van de installatiekopie van het voorbeeld. In dit geval wordt de installatiekopie van het voorbeeld is niet een zwart- wit -installatiekopie, maar de dominante kleuren van voorgrond en achtergrond zwart zijn en de dominante kleuren voor de afbeelding als een geheel zwart-wit zijn.

![Outdoor Mountain](./Images/mountain_vista.png)

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
|![Vision bloem analyseren](./Images/flower.png)| Voorgrond: zwart<br/>Achtergrond: wit<br/>Kleuren: Zwart, technische, groen|
![Vision Station van de trein analyseren](./Images/train_station.png) | Voorgrond: zwart<br/>Achtergrond: zwart<br/>Kleuren: zwart |

### <a name="accent-color-examples"></a>Accent kleur voorbeelden

 De volgende tabel beschrijft de accentkleur als een hexadecimale waarde van elke voorbeeldafbeelding geretourneerd door de Computer Vision de HTML-kleur.

| Installatiekopie | Accentkleur |
|-------|--------------|
|![Outdoor Mountain](./Images/mountain_vista.png) | #BB6D10 |
|![Vision bloem analyseren](./Images/flower.png) | #C6A205 |
|![Vision Station van de trein analyseren](./Images/train_station.png) | #474A84 |

### <a name="black--white-detection-examples"></a>Detectie van zwart- wit -voorbeelden

De volgende tabel geeft aan of de voorbeeldafbeelding zwart- wit, zoals geretourneerd door visie op de Computer.

| Installatiekopie | Zwart- wit? |
|-------|----------------|
|![Visie ontwikkelen analyseren](./Images/bw_buildings.png) | true |
|![Vision House Yard analyseren](./Images/house_yard.png) | false |

## <a name="next-steps"></a>Volgende stappen

Kennis met concepten over [detecteren afbeeldingstypen](concept-detecting-image-types.md).