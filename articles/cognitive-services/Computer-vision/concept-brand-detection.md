---
title: Merk-detectie - Computer Vision
titleSuffix: Azure Cognitive Services
description: Concepten met betrekking tot merk/het logo detectie met behulp van de Computer Vision-API.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 01/26/2019
ms.author: pafarley
ms.openlocfilehash: abeca204296bcb3933013f2b7434b8c558f62e50
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58496584"
---
# <a name="brand-detection"></a>Merk-detectie

Merk-detectie is een speciale modus van [object detectie](concept-object-detection.md) die gebruikmaakt van een database van duizenden globale logo's voor het identificeren van commerciële merken in afbeeldingen en video. U kunt deze functie bijvoorbeeld gebruiken om te ontdekken welke merken het populairst zijn op sociale media of het meest voorkomen in productplaatsing in de media.

De Computer Vision-service detecteert of er nog merk logo's in een bepaalde afbeelding. Als dit het geval is, wordt de merknaam, een betrouwbaarheidsscore en de coördinaten van een vak rond het logo.

De ingebouwde logo-database bevat informatie over populaire merken in de consumer electronics, kleding en meer. Als u merkt dat het merk die u zoekt niet wordt herkend door de Computer Vision-service, u mogelijk een betere dienstverlening maken en trainen van uw eigen logo detector met behulp van de [Custom Vision](https://docs.microsoft.com/azure/cognitive-services/Custom-Vision-Service/) service.

## <a name="brand-detection-example"></a>Merk-detectie-voorbeeld

De volgende JSON-antwoorden illustreren Computer Vision geretourneerd bij het detecteren van merken in de voorbeeld-installatiekopieën.

![Een grijze sweatshirt met een Microsoft-label en het logo erop](./Images/gray-shirt-logo.jpg)

```json
{
   "brands":[
      {
         "name":"Microsoft",
         "confidence":0.706,
         "rectangle":{
            "x":470,
            "y":862,
            "w":338,
            "h":327
         }
      }
   ],
   "requestId":"5fda6b40-3f60-4584-bf23-911a0042aa13",
   "metadata":{
      "width":2286,
      "height":1715,
      "format":"Jpeg"
   }
}
```
In sommige gevallen kan haalt het merk detector zowel de installatiekopie van het logo en de naam van de gestileerde merk als twee afzonderlijke logo's.

![Een rood t-shirt met een Microsoft-label en het logo erop](./Images/red-shirt-logo.jpg)

```json
{
   "brands":[
      {
         "name":"Microsoft",
         "confidence":0.657,
         "rectangle":{
            "x":436,
            "y":473,
            "w":568,
            "h":267
         }
      },
      {
         "name":"Microsoft",
         "confidence":0.85,
         "rectangle":{
            "x":101,
            "y":561,
            "w":273,
            "h":263
         }
      }
   ],
   "requestId":"10dcd2d6-0cf6-4a5e-9733-dc2e4b08ac8d",
   "metadata":{
      "width":1286,
      "height":1715,
      "format":"Jpeg"
   }
}
```

## <a name="use-the-api"></a>De API gebruiken

De mogelijkheid voor het detecteren van merk maakt deel uit van de [analyseren installatiekopie](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) API. U kunt deze API via een systeemeigen SDK of REST-aanroepen aanroepen. Opnemen `Brands` in de **visualFeatures** queryparameter. Vervolgens, wanneer u het volledige JSON-antwoord ontvangt, gewoon parseren van de tekenreeks voor de inhoud van de `"brands"` sectie.

* [Snelstart: Analyseer een afbeelding (.NET SDK)](./quickstarts-sdk/csharp-analyze-sdk.md)
* [Snelstart: Analyseer een afbeelding (REST-API)](./quickstarts/csharp-analyze.md)
