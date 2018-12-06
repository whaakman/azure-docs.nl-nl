---
title: 'Objectdetectie: Computer Vision'
titleSuffix: Azure Cognitive Services
description: Concepten met betrekking tot objectdetectie van met behulp van de Computer Vision-API.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 12/03/2018
ms.author: pafarley
ms.openlocfilehash: 7f0f86e783edb55bc3193df073c92c9523a90176
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52976029"
---
# <a name="object-detection"></a>Objectdetectie

Objectdetectie van het is vergelijkbaar met [tagging](concept-tagging-images.md), maar de API retourneert de omsluitende vak coördinaten (in pixels) van elk gevonden object. Bijvoorbeeld, als een afbeelding een hond, cat en een persoon bevat, de analyse-bewerking wordt een lijst die objecten, samen met hun coördinaten in de afbeelding. U kunt deze functionaliteit gebruiken voor het verwerken van de relaties tussen de objecten in een afbeelding. Ook kunt u bepalen of er meerdere exemplaren van dezelfde tag in een afbeelding zijn.

De API voor het detecteren van toepassing is tags op basis van de objecten of levensonderhoud dingen geïdentificeerd in de afbeelding. Houd er rekening mee dat op dit moment, er is geen formeel relatie tussen de taxonomie gebruikt voor labels en de taxonomie gebruikt voor objectdetectie van het. Op het niveau van een concept vindt de API detecteren alleen u objecten en levensonderhoud dingen, terwijl de API-code kan ook contextuele termen, zoals 'binnenshuis', die met het omsluitende kaders kunnen niet worden vertaald.

## <a name="object-detection-example"></a>Voorbeeld van een object-detectie

Computer Vision geretourneerd bij het detecteren van objecten in het voorbeeld ziet u de volgende JSON-antwoord.

![Een vrouw met behulp van een Microsoft Surface-apparaten in een keuken](./Images/windows-kitchen.jpg)

```json
{
   "objects":[
      {
         "rectangle":{
            "x":730,
            "y":66,
            "w":135,
            "h":85
         },
         "object":"kitchen appliance",
         "confidence":0.501
      },
      {
         "rectangle":{
            "x":523,
            "y":377,
            "w":185,
            "h":46
         },
         "object":"computer keyboard",
         "confidence":0.51
      },
      {
         "rectangle":{
            "x":471,
            "y":218,
            "w":289,
            "h":226
         },
         "object":"Laptop",
         "confidence":0.85,
         "parent":{
            "object":"computer",
            "confidence":0.851
         }
      },
      {
         "rectangle":{
            "x":654,
            "y":0,
            "w":584,
            "h":473
         },
         "object":"person",
         "confidence":0.855
      }
   ],
   "requestId":"a7fde8fd-cc18-4f5f-99d3-897dcd07b308",
   "metadata":{
      "width":1260,
      "height":473,
      "format":"Jpeg"
   }
}
```

## <a name="next-steps"></a>Volgende stappen

Kennis met concepten over [categoriseren van beelden](concept-categorizing-images.md) en [met een beschrijving van installatiekopieën](concept-describing-images.md).