---
title: Detecteren van domeinspecifieke inhoud - Computer Vision
titleSuffix: Azure Cognitive Services
description: Concepten met betrekking tot afbeeldingen met behulp van de Computer Vision-API te beschrijven.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.openlocfilehash: a9c71fa7e5d86cfeb4fe6fab44bbce241546ccb8
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49342550"
---
# <a name="detecting-domain-specific-content"></a>Domeinspecifieke inhoud detecteren

Daarnaast categorisatie tagging en op het hoogste niveau, Computer Vision biedt ook ondersteuning voor gespecialiseerde (of domeinspecifieke) informatie. Specifieke informatie kan worden geïmplementeerd als zelfstandige methode of met de classificatie op hoog niveau. Het fungeert als een manier om verder te verfijnen 86 categorietaxonomie door toevoeging van domeinspecifieke modellen.

Er zijn twee opties voor het gebruik van de domeinspecifieke modellen:

* Analyse van binnen het bereik  
  Alleen een gekozen model analyseren door het aanroepen van een HTTP POST-aanroep. Als u welk model weet wilt u gebruiken, geeft u de naam van het model. U alleen opvragen informatie relevant zijn voor dit model. Bijvoorbeeld, kunt u deze optie alleen gezocht naar herkenning van beroemdheden. Het antwoord bevat een lijst met mogelijkheden die overeenkomt met beroemdheden, vergezeld gaan van hun scores vertrouwen.
* Geavanceerde analyse  
  Analyseren om aanvullende informatie met betrekking tot categorieën van de 86-categorietaxonomie te leveren. Deze optie is beschikbaar voor gebruik in toepassingen waar gebruikers wilt analyse van de installatiekopie van het algemene gedetailleerde gegevens ophalen uit een of meer domeinspecifieke modellen. Wanneer deze methode wordt aangeroepen, wordt eerst de classificatie 86 categorietaxonomie genoemd. Als een van de categorieën die van bekende of overeenkomende modellen overeenkomen, volgt een tweede fase van de classificatie-aanroepen. Bijvoorbeeld, als de `details` parameter van de HTTP POST-aanroep ofwel is ingesteld op 'alle' of 'beroemdheden' bevat, wordt de methode roept de classificatie beroemdheden nadat de classificatie 86 categorie is aangeroepen. Als de afbeelding is geclassificeerd als `people_` of een subcategorie van die categorie, en vervolgens de beroemdheden-classificatie wordt aangeroepen.

## <a name="listing-domain-specific-models"></a>Domeinspecifieke modellen weergeven

U kunt de domeinspecifieke modellen dat wordt ondersteund door visie op de Computer weergeven. Computer Vision ondersteunt momenteel de volgende domeinspecifieke modellen voor het detecteren van domeinspecifieke inhoud:

| Naam | Beschrijving |
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