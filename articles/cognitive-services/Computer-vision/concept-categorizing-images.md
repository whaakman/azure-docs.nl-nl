---
title: Afbeeldings categorisatie-Computer Vision
titleSuffix: Azure Cognitive Services
description: Leer concepten met betrekking tot de functie voor het categoriseren van afbeeldingen van de Computer Vision-API.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 2b9b8da550d80b027da919ba0834e43e2c83d4b4
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/10/2019
ms.locfileid: "68945314"
---
# <a name="categorize-images-by-subject-matter"></a>Afbeeldingen op onderwerp categoriseren

Naast Tags en een beschrijving Computer Vision retourneert de op taxonomie gebaseerde categorieën die zijn gedetecteerd in een installatie kopie. In tegens telling tot labels worden categorieën in een bovenliggende/onderliggende Hereditary-hiërarchie ingedeeld en zijn er minder van de tags (86, in tegens telling tot duizenden Tags). Alle categorie namen zijn in het Engels. Categorisatie kan zelf of naast het nieuwere label model worden uitgevoerd.

## <a name="the-86-category-concept"></a>Het concept van de 86 categorieën

Computer vision kan een afbeelding breed of specifiek categoriseren met behulp van de lijst met 86-categorieën in het volgende diagram. Zie [Categorietaxonomie](category-taxonomy.md) voor de volledige taxonomie in tekstindeling.

![Gegroepeerde lijsten van alle categorieën in de categorie taxonomie](./Images/analyze_categories-v2.png)

## <a name="image-categorization-examples"></a>Voor beelden van afbeeldings categorisatie

In het volgende JSON-antwoord ziet u wat Computer Vision retourneert wanneer de voorbeeld afbeelding wordt gecategoriseerd op basis van de visuele functies.

![Een vrouw aan het dak van een Apartment-gebouw](./Images/woman_roof.png)

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

In de volgende tabel ziet u een typische installatie kopie en de categorie die wordt geretourneerd door Computer Vision voor elke afbeelding.

| Image | Categorie |
|-------|----------|
| ![Vier mensen die als familie samen werken](./Images/family_photo.png) | people_group |
| ![Een puppy die in een gras veld zit](./Images/cute_dog.png) | animal_dog |
| ![Een persoon in een Zons ondergang van een Mountain Rock](./Images/mountain_vista.png) | outdoor_mountain |
| ![Een stapel van brood rollen in een tabel](./Images/bread.png) | food_bread |

## <a name="next-steps"></a>Volgende stappen

Leer concepten over het labelen van [afbeeldingen](concept-tagging-images.md) en het [beschrijven van installatie kopieën](concept-describing-images.md).
