---
title: Type samengesteld entiteit
titleSuffix: Language Understanding - Azure Cognitive Services
description: Een samengestelde entiteit bestaat uit andere entiteiten, zoals vooraf gebouwde entiteiten, eenvoudige, reguliere expressies en lijst entiteiten. De afzonderlijke entiteiten vormen een hele entiteit.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 07/24/2019
ms.author: diberry
ms.openlocfilehash: 55dcc5666e63b8a87ddcb13696991fe02843fbbd
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68480152"
---
# <a name="composite-entity"></a>Samengestelde entiteit 

Een samengestelde entiteit bestaat uit andere entiteiten, zoals vooraf gebouwde entiteiten, eenvoudige, reguliere expressies en lijst entiteiten. De afzonderlijke entiteiten vormen een hele entiteit. 

**Deze entiteit is geschikt voor de volgende gegevens:**

* Aan elkaar zijn gerelateerd. 
* Ze zijn aan elkaar gerelateerd in de context van de utterance.
* Verschillende Entiteitstypen gebruiken.
* Moeten worden gegroepeerd en verwerkt door de client toepassing als een gegevens eenheid.
* Hebben een aantal uitingen voor gebruikers die machine learning nodig hebben.

![Samengestelde entiteit](./media/luis-concept-entities/composite-entity.png)

## <a name="example-json"></a>Voorbeeld van JSON

Overweeg een samengestelde entiteit van vooraf `number` gebouwde `Location::ToLocation` en met de volgende utterance:

`book 2 tickets to paris`

U ziet dat `2`, het aantal en `paris`, de ToLocation woorden tussen deze die geen deel uitmaken van een van de entiteiten hebben. De groene lijn, gebruikt in een gelabelde utterance in de [LUIS](luis-reference-regions.md) -website geeft aan dat een samengestelde entiteit.

![Samengestelde entiteit](./media/luis-concept-data-extraction/composite-entity.png)

Samengestelde entiteiten worden geretourneerd in een `compositeEntities` matrix en alle entiteiten in de samengestelde worden ook weergegeven in de `entities` matrix:

```JSON

"entities": [
    {
    "entity": "2 tickets to cairo",
    "type": "ticketInfo",
    "startIndex": 0,
    "endIndex": 17,
    "score": 0.67200166
    },
    {
    "entity": "2",
    "type": "builtin.number",
    "startIndex": 0,
    "endIndex": 0,
    "resolution": {
        "subtype": "integer",
        "value": "2"
    }
    },
    {
    "entity": "cairo",
    "type": "builtin.geographyV2",
    "startIndex": 13,
    "endIndex": 17
    }
],
"compositeEntities": [
    {
    "parentType": "ticketInfo",
    "value": "2 tickets to cairo",
    "children": [
        {
        "type": "builtin.geographyV2",
        "value": "cairo"
        },
        {
        "type": "builtin.number",
        "value": "2"
        }
    ]
    }
]
```    

|Data-object|De naam van de entiteit|Waarde|
|--|--|--|
|Vooraf gedefinieerde entiteit - nummer|"builtin.number"|"2"|
|Preconstrueerde entiteit-GeographyV2|"Location::ToLocation"|"Parijs"|

## <a name="next-steps"></a>Volgende stappen

In deze [zelf studie](luis-tutorial-composite-entity.md)voegt u een **samengestelde entiteit** toe om geëxtraheerde gegevens van verschillende typen te bundelen in één container met een entiteit. De clienttoepassing kan door de gegevens bundeling, gerelateerde gegevens in verschillende gegevenstypen eenvoudig extraheren.
