---
title: Detecteren van domeinspecifieke inhoudstypen - Computer Vision
titleSuffix: Azure Cognitive Services
description: Leer hoe u een installatiekopie categorisatie domein om terug te keren meer gedetailleerde informatie over een afbeelding opgeven.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 02/08/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 92859667e1dc53b9c6ca9e46a2db1c6dc335ae37
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57339008"
---
# <a name="detect-domain-specific-content"></a>Domeinspecifieke inhoud detecteren

Daarnaast categorisatie tagging en op hoog niveau, Computer Vision biedt ook ondersteuning voor verdere domeinspecifieke analyse met behulp van modellen die zijn getraind op speciale gegevens.

Er zijn twee manieren waarop u met de domeinspecifieke modellen: zelf (analyse van binnen het bereik) of als een uitbreiding op de functie voor categorisatie.

### <a name="scoped-analysis"></a>Analyse van binnen het bereik

U kunt een installatiekopie met behulp van het gekozen domeinspecifieke model door het aanroepen van analyseren de [modellen /\<model\>/het analyseren](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e200) API.

Hieronder volgt een voorbeeld van een JSON-antwoord geretourneerd door de **modellen/beroemdheden/analyseren** API voor de opgegeven installatiekopie:

![Satya Nadella standing](./images/satya.jpeg)

```json
{
  "result": {
    "celebrities": [{
      "faceRectangle": {
        "top": 391,
        "left": 318,
        "width": 184,
        "height": 184
      },
      "name": "Satya Nadella",
      "confidence": 0.99999856948852539
    }]
  },
  "requestId": "8217262a-1a90-4498-a242-68376a4b956b",
  "metadata": {
    "width": 800,
    "height": 1200,
    "format": "Jpeg"
  }
}
```

### <a name="enhanced-categorization-analysis"></a>Verbeterde categorisatie-analyse

U kunt ook domeinspecifieke modellen gebruiken om te voorzien in algemene installatiekopie analyse. U doet dit als onderdeel van [op hoog niveau categorisatie](concept-categorizing-images.md) door te geven van domeinspecifieke modellen in de *details* parameter van de [analyseren](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) API-aanroep.

In dit geval wordt de classificatie 86 categorietaxonomie eerst genoemd. Als een van de gedetecteerde categorieën beschikt over een overeenkomende domeinspecifieke-model, wordt de installatiekopie doorgegeven dat ook model en de resultaten worden toegevoegd.

De volgende JSON-antwoord bevat hoe domeinspecifieke analyse kunnen worden opgenomen als de `detail` knooppunt in een breder categorisatie-analyse.

```json
"categories":[
  {
    "name":"abstract_",
    "score":0.00390625
  },
  {
    "name":"people_",
    "score":0.83984375,
    "detail":{
      "celebrities":[
        {
          "name":"Satya Nadella",
          "faceRectangle":{
            "left":597,
            "top":162,
            "width":248,
            "height":248
          },
          "confidence":0.999028444
        }
      ],
      "landmarks":[
        {
          "name":"Forbidden City",
          "confidence":0.9978346
        }
      ]
    }
  }
]
```

## <a name="list-the-domain-specific-models"></a>Lijst met de domeinspecifieke modellen

Computer Vision ondersteunt momenteel de volgende domeinspecifieke modellen:

| Name | Description |
|------|-------------|
| beroemdheden | Herkenning van beroemdheden, ondersteund voor installatiekopieën geclassificeerd de `people_` categorie |
| Oriëntatiepunten | Oriëntatiepunten spraakherkenning, ondersteund voor installatiekopieën geclassificeerd de `outdoor_` of `building_` categorieën |

Aanroepen van de [modellen](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fd) API retourneert deze informatie, samen met de categorieën die elk model kunt toepassen:

```json
{
  "models":[
    {
      "name":"celebrities",
      "categories":[
        "people_",
        "人_",
        "pessoas_",
        "gente_"
      ]
    },
    {
      "name":"landmarks",
      "categories":[
        "outdoor_",
        "户外_",
        "屋外_",
        "aoarlivre_",
        "alairelibre_",
        "building_",
        "建筑_",
        "建物_",
        "edifício_"
      ]
    }
  ]
}
```

## <a name="next-steps"></a>Volgende stappen

Kennis met concepten over [categoriseren van beelden](concept-categorizing-images.md).
