---
title: Voor het categoriseren van beelden - Computer Vision
titleSuffix: Azure Cognitive Services
description: Meer informatie over concepten met betrekking tot de installatiekopie categorisatie-functie van de Computer Vision-API.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: a85fb65ad971bfc70bdab0689e99921e616b56ac
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/15/2019
ms.locfileid: "56308856"
---
# <a name="categorize-images"></a>Categoriseren van beelden

Naast de labels en een beschrijving, Computer Vision geeft als resultaat de taxonomie op basis van categorieën gedetecteerd in een afbeelding. In tegenstelling tot tags, categorieën zijn ingedeeld in een erfelijke hiërarchie bovenliggend/onderliggend en er zijn minder van deze (in plaats van duizenden tags 86). Alle namen worden in het Engels. Categorisatie kan worden gedaan door zelf of naast het nieuwere model van de labels.

## <a name="the-86-category-concept"></a>Het concept van de 86 categorieën

Computer vision kunt categoriseren een installatiekopie van een breed of specifiek, met behulp van de lijst met 86 categorieën in het volgende diagram. Zie [Categorietaxonomie](category-taxonomy.md) voor de volledige taxonomie in tekstindeling.

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
