---
title: 'Afbeeldingen: Computer Vision-tagging'
titleSuffix: Azure Cognitive Services
description: Concepten met betrekking tot afbeeldingen met behulp van de Computer Vision-API-tagging.
services: cognitive-services
author: deken
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: v-deken
ms.openlocfilehash: 159a1b9404f6ce635dcc8bb6007e52b7346ef98d
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "45982961"
---
# <a name="tagging-images"></a>Afbeeldingen taggen

Computer Vision retourneert tags op basis van meer dan 2000 herkenbare objecten, levende wezens, landschappen en acties. Wanneer tags zijn ambigu of niet algemeen bekend, biedt de API-reactie "tips" voor het verduidelijken van de betekenis van de code in de context van een bekende instelling. Tags zijn niet ingedeeld als een taxonomie en er zijn geen overnamehiërarchieën aanwezig is. Een verzameling van inhoud labels vormt de basis vormt voor een afbeelding 'description', weergegeven als een mens leesbaar taal opgemaakt in volledige zinnen. Houd er rekening mee dat op dit moment Engels is de enige ondersteunde taal voor de beschrijving bij afbeelding.

Computer Vision-algoritmen uitvoer na het uploaden van een afbeelding of een afbeeldings-URL op te geven, tags op basis van de objecten, levende wezens en acties die zijn geïdentificeerd in de afbeelding. Tags is niet beperkt tot de belangrijkste onderwerp, zoals een persoon op de voorgrond is geplaatst, maar bevat ook de instelling (binnen of buiten), meubels, hulpprogramma's, fabrieken, dieren, accessoires, gadgets enzovoort.

## <a name="image-tagging-example"></a>Voorbeeld van de installatiekopie taggen

Computer Vision geretourneerd bij het labelen van visuele kenmerken die zijn gedetecteerd in het voorbeeld ziet u de volgende JSON-antwoord.

![House_Yard](./Images/house_yard.png).

```json
{
    "tags": [
        {
            "name": "grass",
            "confidence": 0.9999995231628418
        },
        {
            "name": "outdoor",
            "confidence": 0.99992108345031738
        },
        {
            "name": "house",
            "confidence": 0.99685388803482056
        },
        {
            "name": "sky",
            "confidence": 0.99532157182693481
        },
        {
            "name": "building",
            "confidence": 0.99436837434768677
        },
        {
            "name": "tree",
            "confidence": 0.98880356550216675
        },
        {
            "name": "lawn",
            "confidence": 0.788884699344635
        },
        {
            "name": "green",
            "confidence": 0.71250593662261963
        },
        {
            "name": "residential",
            "confidence": 0.70859086513519287
        },
        {
            "name": "grassy",
            "confidence": 0.46624681353569031
        }
    ],
    "requestId": "06f39352-e445-42dc-96fb-0a1288ad9cf1",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Jpeg"
    }
}
```

## <a name="next-steps"></a>Volgende stappen

Kennis met concepten over [categoriseren van beelden](concept-categorizing-images.md) en [met een beschrijving van installatiekopieën](concept-describing-images.md).