---
title: 'Objectdetectie: Computer Vision'
titleSuffix: Azure Cognitive Services
description: Meer informatie over concepten met betrekking tot de mogelijkheid voor het detecteren van object van de Computer Vision-API - gebruik en limieten.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 012ab849c926de332da55361c79c76c5a1311169
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60368035"
---
# <a name="detect-common-objects-in-images"></a>Algemene objecten in afbeeldingen detecteren

Objectdetectie van het is vergelijkbaar met [tagging](concept-tagging-images.md), maar de API retourneert de omsluitende vak coördinaten (in pixels) van elk gevonden object. Als een afbeelding bijvoorbeeld een hond, kat en persoon bevat, zal de detectiebewerking deze objecten vermelden, samen met hun coördinaten in de afbeelding. U kunt deze functionaliteit gebruiken voor het verwerken van de relaties tussen de objecten in een afbeelding. Ook kunt u bepalen of er meerdere exemplaren van dezelfde tag in een afbeelding zijn.

De API voor het detecteren van toepassing is tags op basis van de objecten of levensonderhoud dingen geïdentificeerd in de afbeelding. Er is momenteel geen formeel relatie tussen de labels taxonomie en de object-detectie taxonomie. Op het niveau van een concept vindt de API detecteren alleen u objecten en levensonderhoud dingen, terwijl de API-code kan ook contextuele termen, zoals 'binnenshuis', die met het omsluitende kaders kunnen niet worden vertaald.

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

## <a name="limitations"></a>Beperkingen

Het is belangrijk te weten de beperkingen van objectdetectie zodat u kunt voorkomen of de effecten van de waarde false negatieven (gemiste objecten) en beperkt details.

* Objecten worden in het algemeen niet gedetecteerd als ze klein (minder dan 5% van de afbeelding).
* Objecten in het algemeen niet worden gedetecteerd als ze nauw samen zijn gesorteerd (een stapel elementen vanaf, bijvoorbeeld).
* Objecten niet worden onderscheiden door merk of product-namen (verschillende soorten frisdrank op een rek store, bijvoorbeeld). Echter, krijgt u merk informatie van een installatiekopie met behulp van de [merknaam detectie](concept-brand-detection.md) functie.

## <a name="use-the-api"></a>De API gebruiken

De mogelijkheid voor het detecteren van object maakt deel uit van de [analyseren installatiekopie](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) API. U kunt deze API via een systeemeigen SDK of REST-aanroepen aanroepen. Opnemen `Objects` in de **visualFeatures** queryparameter. Vervolgens, wanneer u het volledige JSON-antwoord ontvangt, gewoon parseren van de tekenreeks voor de inhoud van de `"objects"` sectie.

* [Snelstart: Analyseer een afbeelding (.NET SDK)](./quickstarts-sdk/csharp-analyze-sdk.md)
* [Snelstart: Analyseer een afbeelding (REST-API)](./quickstarts/csharp-analyze.md)