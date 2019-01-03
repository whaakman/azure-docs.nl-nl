---
title: Detecteren van domeinspecifieke inhoudstypen - Computer Vision
titleSuffix: Azure Cognitive Services
description: Leer hoe u een installatiekopie categorisatie domein om terug te keren meer gedetailleerde informatie over een afbeelding opgeven.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 50942634bd50974453c242d1980db9fc589bd47e
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2018
ms.locfileid: "53579206"
---
# <a name="detecting-domain-specific-content"></a>Domeinspecifieke inhoud detecteren

Daarnaast categorisatie tagging en op het hoogste niveau, Computer Vision biedt ook ondersteuning voor gespecialiseerde (of domeinspecifieke) informatie. Gespecialiseerde informatie kan worden geïmplementeerd als zelfstandige methode of met de categorisatie op hoog niveau. Dit fungeert als een manier om de taxonomie van de 86 categorieën verder te verfijnen door toevoeging van domeinspecifieke modellen.

Er zijn twee opties voor het gebruik van de domeinspecifieke modellen:

* Analyse van binnen het bereik  
  Alleen een gekozen model analyseren door het aanroepen van een HTTP POST-aanroep. Als u welk model weet wilt u gebruiken, geeft u de naam van het model. U alleen opvragen informatie relevant zijn voor dit model. U kunt deze optie bijvoorbeeld gebruiken als u alleen herkenning van beroemdheden wilt. Het antwoord bevat een lijst met mogelijke overeenkomsten met beroemdheden, samen met de betrouwbaarheidsscores.
* Geavanceerde analyse  
  Gebruik analyse om aanvullende informatie voor de categorieën van de 86-categorieëntaxonomie op te geven. Deze optie is beschikbaar voor toepassingen waarbij gebruikers een algemene afbeeldingsanalyse willen ophalen naast details uit één of meer domeinspecifieke modellen. Wanneer deze methode wordt aangeroepen, wordt eerst de classificatie van de 86-categorieëntaxonomie aangeroepen. Als een van de categorieën die van bekende of overeenkomende modellen overeenkomen, volgt een tweede fase van de classificatie-aanroepen. Bijvoorbeeld, als de `details` parameter van de HTTP POST-aanroep ofwel is ingesteld op 'alle' of 'beroemdheden' bevat, wordt de methode roept de classificatie beroemdheden nadat de classificatie 86 categorie is aangeroepen. Als de afbeelding is geclassificeerd als `people_` of een subcategorie van die categorie, en vervolgens de beroemdheden-classificatie wordt aangeroepen.

## <a name="listing-domain-specific-models"></a>Domeinspecifieke modellen weergeven

U kunt de domeinspecifieke modellen dat wordt ondersteund door visie op de Computer weergeven. Computer Vision ondersteunt momenteel de volgende domeinspecifieke modellen voor het detecteren van domeinspecifieke inhoud:

| Name | Description |
|------|-------------|
| beroemdheden | Herkenning van beroemdheden, ondersteund voor installatiekopieën geclassificeerd de `people_` categorie |
| Oriëntatiepunten | Oriëntatiepunten spraakherkenning, ondersteund voor installatiekopieën geclassificeerd de `outdoor_` of `building_` categorieën |

### <a name="domain-model-list-example"></a>Voorbeeld van een domein model

De volgende JSON-antwoord bevat de domeinspecifieke modellen dat wordt ondersteund door visie op de Computer.

```json
{
    "models": [
        {
            "name": "celebrities",
            "categories": ["people_", "人_", "pessoas_", "gente_"]
        },
        {
            "name": "landmarks",
            "categories": ["outdoor_", "户外_", "屋外_", "aoarlivre_", "alairelibre_",
                "building_", "建筑_", "建物_", "edifício_"]
        }
    ]
}
```

## <a name="next-steps"></a>Volgende stappen

Kennis met concepten over [categoriseren van beelden](concept-categorizing-images.md).