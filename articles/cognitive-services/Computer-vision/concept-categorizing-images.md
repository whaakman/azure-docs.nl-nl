---
title: Voor het categoriseren van beelden - Computer Vision
titleSuffix: Azure Cognitive Services
description: Concepten met betrekking tot het categoriseren van afbeeldingen met behulp van de Computer Vision-API.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.openlocfilehash: 602ea8028cf89b23df692d5c2fb9b781f64bcad4
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49341681"
---
# <a name="categorizing-images"></a>Afbeeldingen categoriseren

Naast de labels en beschrijvingen, Computer Vision geeft als resultaat de taxonomie op basis van categorieën die in eerdere versies zijn gedefinieerd. Deze categorieën zijn ingedeeld als een taxonomie met bovenliggende/onderliggende erfelijke hiërarchieën. Alle categorieën zijn in het Engels. Ze kunnen worden gebruikt alleen of met onze nieuwe modellen tagging.

## <a name="the-86-category-concept"></a>Het concept 86-categorie

Op basis van een lijst met 86 concepten die worden weergegeven in het volgende diagram, kan een installatiekopie worden gecategoriseerd variërend van groot tot specifieke. Zie voor de volledige taxonomie in tekstindeling, [Categorietaxonomie](category-taxonomy.md).

![Categorieën analyseren](./Images/analyze_categories.png)

## <a name="image-categorization-examples"></a>Afbeelding categorisatie-voorbeelden

De volgende JSON-antwoord wordt geïllustreerd wat Computer Vision geretourneerd bij het categoriseren van de voorbeeld-installatiekopie op basis van de visuele kenmerken.

![Vrouw plafond](./Images/woman_roof.png)

```json
{
    "categories": [
        {
            "name": "people_",
            "score": 0.81640625
        }
    ],
    "requestId": "bae7f76a-1cc7-4479-8d29-48a694974705",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Jpeg"
    }
}
```

De volgende tabel ziet u een set typische installatiekopie en de categorie die is geretourneerd door de Computer Vision voor elke afbeelding.

| Installatiekopie | Categorie |
|-------|----------|
| ![Familie foto](./Images/family_photo.png) | people_group |
| ![Schattige hond](./Images/cute_dog.png) | animal_dog |
| ![Outdoor Mountain](./Images/mountain_vista.png) | outdoor_mountain |
| ![Vision Food brood analyseren](./Images/bread.png) | food_bread |

## <a name="next-steps"></a>Volgende stappen

Kennis met concepten over [installatiekopieën taggen](concept-tagging-images.md) en [met een beschrijving van installatiekopieën](concept-describing-images.md).