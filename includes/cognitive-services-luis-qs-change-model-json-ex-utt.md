---
title: bestand opnemen
description: bestand opnemen
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.subservice: luis
ms.topic: include
ms.custom: include file
ms.date: 08/16/2018
ms.author: diberry
ms.openlocfilehash: 2317e0b8bfe01f94989412db7c0c4560b2ca728f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60710245"
---
Het voorbeeldbestand met utterances, **utterances.json**, volgt een specifieke indeling. 

Het veld `text` bevat de tekst van de voorbeeldutterance. Het veld `intentName` moet overeenkomen met de naam van een bestaande intentie in de LUIS-app. Het veld `entityLabels` is vereist. Als u niet alle entiteiten een label wilt geven, geeft u een lege matrix op.

Als de entityLabels-matrix niet leeg is, moeten `startCharIndex` en `endCharIndex` de entiteit markeren waarnaar wordt verwezen in het veld `entityName`. De index is nul, wat betekent dat 6 in het bovenste voorbeeld verwijst naar de "S" van Seattle en niet naar de spatie vóór de hoofdletter S. Als u het label begint of eindigt bij een spatie in de tekst, mislukt de API-aanroep om de utterances toe te voegen.

```JSON
[
  {
    "text": "go to Seattle today",
    "intentName": "BookFlight",
    "entityLabels": [
      {
        "entityName": "Location::LocationTo",
        "startCharIndex": 6,
        "endCharIndex": 12
      }
    ]
  },
  {
    "text": "purple dogs are difficult to work with",
    "intentName": "BookFlight",
    "entityLabels": []
  }
]
```
