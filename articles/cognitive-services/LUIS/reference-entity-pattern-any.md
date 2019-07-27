---
title: Patroon. elk entiteits type-LUIS
titleSuffix: Azure Cognitive Services
description: Patroon. any is een tijdelijke aanduiding met variabele lengte die alleen wordt gebruikt in de sjabloon van een patroon utterance om te markeren waar de entiteit begint en eindigt.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 07/24/2019
ms.author: diberry
ms.openlocfilehash: cda6c724a36a73dc34c2bf8e7158e3e3ec92d46b
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68563221"
---
# <a name="patternany-entity"></a>Pattern.any-entiteit 

Patroon. any is een tijdelijke aanduiding met variabele lengte die alleen wordt gebruikt in de sjabloon van een patroon utterance om te markeren waar de entiteit begint en eindigt.  

Patroon. alle entiteiten moeten worden gemarkeerd in de voor beelden van [patroon](luis-how-to-model-intent-pattern.md) sjablonen, niet de intentie gebruikers voorbeelden.

**De entiteit is goed geschikt wanneer:**

* Het einde van de entiteit kan worden verward met de resterende tekst van de utterance. 

## <a name="usage"></a>Gebruik

Aan de hand van een client toepassing die zoekt naar boeken op basis van de titel, het patroon. alle extraheert de volledige titel. Een sjabloon utterance het gebruik van een patroon. elke voor deze `Was {BookTitle} written by an American this year[?]`zoek opdracht in het boek is. 

In de volgende tabel heeft elke rij twee versies van de utterance. De bovenste utterance is hoe LUIS aanvankelijk de utterance ziet. Het is niet duidelijk waar de titel van het boek begint en eindigt. De onderste utterance maakt gebruik van een patroon. een wille keurige entiteit om het begin en het einde van de entiteit te markeren. 

|Utterance met entiteit in vet|
|--|
|`Was The Man Who Mistook His Wife for a Hat and Other Clinical Tales written by an American this year?`<br><br>Was **de man die de vrouw heeft geduurd voor een hoed en andere klinische verschijnselen die zijn** geschreven met een Amerikaans dit jaar?|
|`Was Half Asleep in Frog Pajamas written by an American this year?`<br><br>Is de **halve slaap stand in kikker Pajamas** geschreven door een Amerikaans dit jaar?|
|`Was The Particular Sadness of Lemon Cake: A Novel written by an American this year?`<br><br>Was **de specifieke verdriet van citroen koek: Een roman** geschreven door een Amerikaans dit jaar?|
|`Was There's A Wocket In My Pocket! written by an American this year?`<br><br>Is **er een Wocket in mijn Pocket!** geschreven door een Amerikaans dit jaar?|
||

## <a name="example-json"></a>Voorbeeld van JSON

```JSON
{
  "query": "where is the form Understand your responsibilities as a member of the community and who needs to sign it after I read it?",
  "topScoringIntent": {
    "intent": "FindForm",
    "score": 0.999999464
  },
  "intents": [
    {
      "intent": "FindForm",
      "score": 0.999999464
    },
    {
      "intent": "GetEmployeeBenefits",
      "score": 4.883697E-06
    },
    {
      "intent": "None",
      "score": 1.02040713E-06
    },
    {
      "intent": "GetEmployeeOrgChart",
      "score": 9.278342E-07
    },
    {
      "intent": "MoveAssetsOrPeople",
      "score": 9.278342E-07
    }
  ],
  "entities": [
    {
      "entity": "understand your responsibilities as a member of the community",
      "type": "FormName",
      "startIndex": 18,
      "endIndex": 78,
      "role": ""
    }
  ]
}
```

## <a name="next-steps"></a>Volgende stappen

In deze [zelf studie](luis-tutorial-pattern-any.md)gebruikt u het **patroon. elke** entiteit voor het extra heren van gegevens uit uitingen waarbij de uitingen goed zijn opgemaakt en waarbij het einde van de gegevens gemakkelijk kan worden verward met de resterende woorden van de utterance.