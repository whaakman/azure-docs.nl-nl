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
ms.openlocfilehash: 7062d98d40c15f4e9e873038fc12fc1b104c996d
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2018
ms.locfileid: "52333476"
---
# <a name="categorizing-images"></a>Afbeeldingen categoriseren

Naast de labels en beschrijvingen, Computer Vision geeft als resultaat de taxonomie op basis van categorieën die in eerdere versies zijn gedefinieerd. Deze categorieën zijn ingedeeld als taxonomie met bovenliggende/onderliggende overdraagbare hiërarchieën. Alle categorieën zijn in het Engels. Ze kunnen worden gebruikt alleen of met onze nieuwe modellen tagging.

## <a name="the-86-category-concept"></a>Het concept van de 86 categorieën

Op basis van een lijst met 86 concepten die worden weergegeven in het volgende diagram, kan een installatiekopie worden gecategoriseerd variërend van groot tot specifieke. Zie [Categorietaxonomie](category-taxonomy.md) voor de volledige taxonomie in tekstindeling.

![gegroepeerde lijsten van de categorieën in de categorietaxonomie](./Images/analyze_categories-v2.png)

## <a name="image-categorization-examples"></a>Afbeelding categorisatie-voorbeelden

De volgende JSON-antwoord wordt geïllustreerd wat Computer Vision geretourneerd bij het categoriseren van de voorbeeld-installatiekopie op basis van de visuele kenmerken.

![Vrouw op dak](./Images/woman_roof.png)

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
| ![Familiefoto](./Images/family_photo.png) | people_group |
| ![Leuke hond](./Images/cute_dog.png) | animal_dog |
| ![Berglandschap](./Images/mountain_vista.png) | outdoor_mountain |
| ![Vision-analyse Voedsel en brood](./Images/bread.png) | food_bread |

## <a name="next-steps"></a>Volgende stappen

Kennis met concepten over [installatiekopieën taggen](concept-tagging-images.md) en [met een beschrijving van installatiekopieën](concept-describing-images.md).