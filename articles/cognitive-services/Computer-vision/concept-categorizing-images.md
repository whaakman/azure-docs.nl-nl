---
title: Voor het categoriseren van beelden - Computer Vision
titleSuffix: Azure Cognitive Services
description: Meer informatie over concepten met betrekking tot de installatiekopie categorisatie-functie van de Computer Vision-API.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: f182110d150583ee1c241c23a2e1924d9f3e3bd4
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55158040"
---
# <a name="image-categorization-with-computer-vision"></a>Afbeelding categorisatie met Computer Vision

Naast de labels en beschrijvingen, Computer Vision geeft als resultaat de taxonomie op basis van categorieën die in eerdere versies zijn gedefinieerd. Deze categorieën zijn ingedeeld als taxonomie met bovenliggende/onderliggende overdraagbare hiërarchieën. Alle categorieën zijn in het Engels. Ze kunnen worden gebruikt alleen of met onze nieuwe modellen tagging.

## <a name="the-86-category-concept"></a>Het concept van de 86 categorieën

Op basis van een lijst met 86 concepten die worden weergegeven in het volgende diagram, kan een installatiekopie worden gecategoriseerd variërend van groot tot specifieke. Zie [Categorietaxonomie](category-taxonomy.md) voor de volledige taxonomie in tekstindeling.

![gegroepeerde lijsten van de categorieën in de categorietaxonomie](./Images/analyze_categories-v2.png)

## <a name="image-categorization-examples"></a>Afbeelding categorisatie-voorbeelden

De volgende JSON-antwoord wordt geïllustreerd wat Computer Vision geretourneerd bij het categoriseren van de voorbeeld-installatiekopie op basis van de visuele kenmerken.

![Een vrouw die op het plafond van een gebouw apartment](./Images/woman_roof.png)

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
| ![Vier personen zich voordeed samen als een familie](./Images/family_photo.png) | people_group |
| ![Een puppy zit in een grassy veld](./Images/cute_dog.png) | animal_dog |
| ![Een persoon die permanent op een rock mountain zonsondergang](./Images/mountain_vista.png) | outdoor_mountain |
| ![Een stapel van brood rollen in een tabel](./Images/bread.png) | food_bread |

## <a name="next-steps"></a>Volgende stappen

Kennis met concepten over [installatiekopieën taggen](concept-tagging-images.md) en [met een beschrijving van installatiekopieën](concept-describing-images.md).
