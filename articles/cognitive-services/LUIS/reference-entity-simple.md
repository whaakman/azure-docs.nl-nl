---
title: Type eenvoudig entiteit
titleSuffix: Language Understanding - Azure Cognitive Services
description: Een eenvoudige entiteit is een generieke entiteit die één concept beschrijft en die wordt geleerd van de door de computer geleerde context. Omdat eenvoudige entiteiten doorgaans namen zijn zoals bedrijfs namen, product namen of andere categorieën namen, voegt u een woordgroepen lijst toe wanneer u een eenvoudige entiteit gebruikt om het signaal van de gebruikte namen te verhogen.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 07/24/2019
ms.author: diberry
ms.openlocfilehash: 3f03b33f685bb5c7c9ba8f2267b8556c5dadade4
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68480218"
---
# <a name="simple-entity"></a>Eenvoudige entiteit 

Een eenvoudige entiteit is een generieke entiteit die één concept beschrijft en die wordt geleerd van de door de computer geleerde context. Omdat eenvoudige entiteiten doorgaans namen zijn zoals bedrijfs namen, product namen of andere categorieën namen, voegt u een woordgroepen [lijst](luis-concept-feature.md) toe wanneer u een eenvoudige entiteit gebruikt om het signaal van de gebruikte namen te verhogen. 

**De entiteit is goed geschikt wanneer:**

* De gegevens zijn niet consistent opgemaakt, maar aangeven hetzelfde. 

![eenvoudige entiteit](./media/luis-concept-entities/simple-entity.png)

## <a name="example-json"></a>Voorbeeld van JSON

`Bob Jones wants 3 meatball pho`

In de vorige utterance `Bob Jones` wordt aangeduid als een eenvoudige `Customer` entiteit.

De gegevens die worden geretourneerd van het eindpunt bevat de naam van de entiteit, de gedetecteerde tekst van de utterance, de locatie van de gedetecteerde tekst en de score is:

```JSON
"entities": [
  {
  "entity": "bob jones",
  "type": "Customer",
  "startIndex": 0,
  "endIndex": 8,
  "score": 0.473899543
  }
]
```

|Data-object|De naam van de entiteit|Waarde|
|--|--|--|
|Eenvoudige entiteit|`Customer`|`bob jones`|

## <a name="next-steps"></a>Volgende stappen

In deze [zelf studie](luis-quickstart-primary-and-secondary-data.md)extraheert u door de computer geleerde gegevens van de naam van de werk taak uit een utterance met behulp van de **eenvoudige entiteit**. Als u de nauw keurigheid van de extractie wilt verg Roten, voegt u een [woordgroepen lijst](luis-concept-feature.md) toe met termen die specifiek zijn voor de eenvoudige entiteit.