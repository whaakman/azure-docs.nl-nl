---
title: bestand opnemen
description: bestand opnemen
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: luis
ms.topic: include
ms.custom: include file
ms.date: 08/16/2018
ms.author: diberry
ms.openlocfilehash: 419f15901b665b43b850922f77bd32d7aac8d3a2
ms.sourcegitcommit: ebb460ed4f1331feb56052ea84509c2d5e9bd65c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "42920613"
---
Het voorbeeldbestand uitingen **utterances.json**, een specifieke indeling volgt. 

De `text` veld bevat de tekst van de voorbeeld-utterance. De `intentName` veld moet overeenkomen met de naam van een bestaande kunt u lezen wat in de LUIS-app. Het veld `entityLabels` is vereist. Als u niet dat alle entiteiten een label wilt, geeft u een lege matrix.

Als de matrix entityLabels niet leeg is, wordt is de `startCharIndex` en `endCharIndex` wilt markeren van de entiteit waarnaar in de `entityName` veld. De index is nul, wat betekent dat 6 in het bovenste voorbeeld verwijst naar de "S" van Seattle en niet de spatie vóór de hoofdstad S. Als u begin of einde van het label op een spatie in de tekst, mislukt de API-aanroep naar de utterances toevoegen.

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