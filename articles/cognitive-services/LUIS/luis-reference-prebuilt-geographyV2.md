---
title: Geografie V2 vooraf gemaakte entiteiten
titleSuffix: Azure Cognitive Services
description: In dit artikel bevat geographyV2 vooraf gedefinieerde entiteitgegevens in Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cjgronlund
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/24/2018
ms.author: diberry
ms.openlocfilehash: 07bbb8e17a9f75bd878c384b5b8d90798d043814
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53086215"
---
# <a name="geographyv2-entity"></a>GeographyV2-entiteit
De vooraf gedefinieerde geographyV2 entiteit detecteert plaatsen. Omdat deze entiteit wordt al getraind, hoeft u niet om toe te voegen van de voorbeeld-uitingen GeographyV2 die aan de toepassing intents. GeographyV2 entiteit wordt ondersteund in het Engels [cultuur](luis-reference-prebuilt-entities.md).

## <a name="subtypes"></a>Subtypen
De geografische locaties zijn subtypen:

|Subtype|Doel|
|--|--|
|`poi`|nuttige plaats|
|`city`|naam van plaats|
|`countryRegion`|naam van het land of regio|
|`continent`|naam van continent|
|`state`|naam van staat of provincie|


## <a name="resolution-for-geographyv2-entity"></a>Oplossing voor GeographyV2 entiteit
Het volgende voorbeeld ziet u de resolutie van de **builtin.geographyV2** entiteit.

```json
{
    "query": "Carol is visiting the sphinx in gizah egypt in africa before heading to texas",
    "topScoringIntent": {
        "intent": "None",
        "score": 0.8008023
    },
    "intents": [
        {
            "intent": "None",
            "score": 0.8008023
        }
    ],
    "entities": [
        {
            "entity": "the sphinx",
            "type": "builtin.geographyV2.poi",
            "startIndex": 18,
            "endIndex": 27
        },
        {
            "entity": "gizah",
            "type": "builtin.geographyV2.city",
            "startIndex": 32,
            "endIndex": 36
        },
        {
            "entity": "egypt",
            "type": "builtin.geographyV2.countryRegion",
            "startIndex": 38,
            "endIndex": 42
        },
        {
            "entity": "africa",
            "type": "builtin.geographyV2.continent",
            "startIndex": 47,
            "endIndex": 52
        },
        {
            "entity": "texas",
            "type": "builtin.geographyV2.state",
            "startIndex": 72,
            "endIndex": 76
        },
        {
            "entity": "carol",
            "type": "builtin.personName",
            "startIndex": 0,
            "endIndex": 4
        }
    ]
} 
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de [e](luis-reference-prebuilt-email.md), [getal](luis-reference-prebuilt-number.md), en [rangtelwoord](luis-reference-prebuilt-ordinal.md) entiteiten. 