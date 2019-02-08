---
title: Met een beschrijving van afbeeldingen - Computer Vision
titleSuffix: Azure Cognitive Services
description: Concepten met betrekking tot de functie voor de beschrijving van afbeelding van de Computer Vision-API.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 7919a84ffe948c9b6a8f68fc1372f1976c09bc79
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55864278"
---
# <a name="describe-images-with-human-readable-language"></a>Installatiekopieën met leesbare taal beschrijven

De computer Vision-algoritmen analyseren de inhoud in een afbeelding. Deze analyse vormt de basis voor een 'beschrijving‘ die wordt weergegeven als voor mensen leesbare taal opgemaakt in volledige zinnen. De beschrijving bevat een overzicht van wat er in de afbeelding is gevonden. De computer Vision-algoritmen genereren verschillende beschrijvingen op basis van de visuele kenmerken die zijn geïdentificeerd in de afbeelding. De beschrijving wordt geëvalueerd en een betrouwbaarheidsscore gegenereerd. Vervolgens wordt er een lijst geretourneerd die is geordend van de hoogste naar de laagste betrouwbaarheidsscore.

## <a name="image-description-example"></a>Voorbeeld van de installatiekopie van een softwarebeschrijving

De volgende JSON-antwoord wordt geïllustreerd wat Computer Vision retourneert bij de beschrijving van de voorbeeld-installatiekopie op basis van de visuele kenmerken.

![Een zwart-wit beeld van de gebouwen in Manhattan](./Images/bw_buildings.png)

```json
{
    "description": {
        "tags": ["outdoor", "building", "photo", "city", "white", "black", "large", "sitting", "old", "water", "skyscraper", "many", "boat", "river", "group", "street", "people", "field", "tall", "bird", "standing"],
        "captions": [
            {
                "text": "a black and white photo of a city",
                "confidence": 0.95301952483304808
            },
            {
                "text": "a black and white photo of a large city",
                "confidence": 0.94085190563213816
            },
            {
                "text": "a large white building in a city",
                "confidence": 0.93108362931954824
            }
        ]
    },
    "requestId": "b20bfc83-fb25-4b8d-a3f8-b2a1f084b159",
    "metadata": {
        "height": 300,
        "width": 239,
        "format": "Jpeg"
    }
}
```

## <a name="next-steps"></a>Volgende stappen

Kennis met concepten over [installatiekopieën taggen](concept-tagging-images.md) en [categoriseren van beelden](concept-categorizing-images.md).
