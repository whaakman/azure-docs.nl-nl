---
title: Met een beschrijving van afbeeldingen - Computer Vision
titleSuffix: Azure Cognitive Services
description: Concepten met betrekking tot afbeeldingen met behulp van de Computer Vision-API te beschrijven.
services: cognitive-services
author: deken
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: v-deken
ms.openlocfilehash: a65e3ea2fb28ca8a2250fb3e39860eb5e08c18f4
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "45981978"
---
# <a name="describing-images"></a>Afbeeldingen beschrijven

De computer Vision-algoritmen analyseren de inhoud in een afbeelding. Deze analyse vormt de basis vormt voor een 'description', weergegeven als leesbare taal in volledige zinnen. De beschrijving bevat een overzicht van wat er in de afbeelding is gevonden. De computer Vision-algoritmen genereren verschillende beschrijvingen op basis van de visuele kenmerken die zijn geïdentificeerd in de afbeelding. De beschrijving wordt geëvalueerd en een betrouwbaarheidsscore gegenereerd. Vervolgens wordt er een lijst geretourneerd die is geordend van de hoogste naar de laagste betrouwbaarheidsscore.

## <a name="image-description-example"></a>Voorbeeld van de installatiekopie van een softwarebeschrijving

De volgende JSON-antwoord wordt geïllustreerd wat Computer Vision retourneert bij de beschrijving van de voorbeeld-installatiekopie op basis van de visuele kenmerken.

![B & W gebouwen](./Images/bw_buildings.png)

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

Een voorbeeld van een bot die gebruikmaakt van deze technologie voor het genereren van afbeelding bijschriften vindt [hier](https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/intelligence-ImageCaption).  

## <a name="next-steps"></a>Volgende stappen

Kennis met concepten over [installatiekopieën taggen](concept-tagging-images.md) en [categoriseren van beelden](concept-categorizing-images.md).