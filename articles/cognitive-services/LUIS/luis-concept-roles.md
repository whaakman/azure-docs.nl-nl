---
title: Rollen voor entiteiten
titleSuffix: Azure Cognitive Services
description: Rollen zijn met de naam, contextuele subtypen van een entiteit die alleen in de patronen gebruikt. Bijvoorbeeld, in de utterance `buy a ticket from New York to London`, New York zowel Londen zijn steden, maar elk een andere betekenis heeft in de zin. New York is de plaats van de oorsprong en Londen is de plaats van bestemming.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 04/01/2019
ms.author: diberry
ms.openlocfilehash: 318e71b68bbabeeef34c75a412f9fdd5b6db754a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65073013"
---
# <a name="entity-roles-for-contextual-subtypes"></a>Entiteit rollen voor contextuele subtypen

Rollen kunnen entiteiten zijn met de naam subtypen. Een rol kan worden gebruikt met een vooraf gedefinieerde of aangepaste entiteit, en worden gebruikt in zowel voorbeeld uitingen en patronen. 

<a name="example-role-for-entities"></a>
<a name="roles-with-prebuilt-entities"></a>

## <a name="machine-learned-entity-example-of-roles"></a>Voorbeeld van de entiteit machine geleerd van rollen

In de utterance "kopen van een ticket van de **New York** naar **Londen**, New York zowel Londen zijn steden, maar elk een andere betekenis heeft in de zin. New York is de plaats van de oorsprong en Londen is de plaats van bestemming. 

```
buy a ticket from New York to London
```

Functies bieden een naam op die verschillen:

|Entiteitstype|Naam van de entiteit|Rol|Doel|
|--|--|--|--|
|Eenvoudig|Locatie|Oorsprong|waar het vlak van verlaat|
|Eenvoudig|Locatie|destination|waar het vlak van terechtkomt|

## <a name="non-machine-learned-entity-example-of-roles"></a>Voorbeeld van de entiteit niet machine geleerd van rollen

In de utterance 'Plannen de vergadering uit 8 tot en met 9', zowel de getallen geven een tijd, maar telkens wanneer een andere betekenis heeft in de utterance. Rollen bieden de naam van de verschillen. 

```
Schedule the meeting from 8 to 9
```

|Entiteitstype|Rolnaam|Value|
|--|--|--|
|Vooraf gedefinieerde datetimeV2|StartTime|8|
|Vooraf gedefinieerde datetimeV2|Eindtijd|9|

## <a name="are-multiple-entities-in-an-utterance-the-same-thing-as-roles"></a>Meerdere entiteiten in een utterance hetzelfde als rollen zijn? 

Meerdere entiteiten kunnen bestaan in een utterance en zonder gebruik van rollen kunnen worden geëxtraheerd. Als de context van de zin geeft aan met dat de versie van de entiteit heeft een waarde, wordt een rol moet worden gebruikt. 

### <a name="dont-use-roles-for-duplicates-without-meaning"></a>Gebruik geen functies voor duplicaten zonder betekenis

Als de utterance een lijst met locaties bevat, `I want to travel to Seattle, Cairo, and London.`, dit is een lijst waarin elk item een extra betekenis niet hebben. 

### <a name="use-roles-if-duplicates-indicate-meaning"></a>Rollen gebruiken als de duplicaten aangeven betekenis

Als de utterance een lijst met locaties met betekenis bevat, `I want to travel from Seattle, with a layover in Londen, landing in Cairo.`, deze betekenis van de oorsprong, layover en de bestemming moet worden vastgelegd met de rol.

### <a name="roles-can-indicate-order"></a>Rollen kunnen duiden op volgorde

Als de utterance gewijzigd om aan te geven die u extraheren wilt, `I want to first start with Seattle, second London, then third Cairo`, kunt u in een aantal manieren uitpakken. U kunt de tokens die wijzen op de rol labelen `first start with`, `second`, `third`. U kunt ook de vooraf gedefinieerde entiteit **rangtelwoord** en de **GeographyV2** vooraf gemaakte entiteiten in een samengestelde entiteit om vast te leggen van het idee van de volgorde en plaats. 

## <a name="how-are-roles-used-in-example-utterances"></a>Hoe worden de rollen in voorbeeld-uitingen gebruikt?

Wanneer een entiteit een rol is, en de entiteit is gemarkeerd als in een voorbeeld-utterance, hebt u de keuze van alleen de entiteit selecteren of de entiteit en de rol selecteren. 

Het volgende voorbeeld-uitingen entiteiten en functies gebruiken:

|Token weergeven|Entiteit weergeven|
|--|--|
|Ik ben interessante meer te weten over **Seattle**|Ik wil graag meer weten over {Location}|
|Kopen van een ticket van Seattle naar New York|Kopen van een ticket bij {locatie: oorsprong} naar {locatie: Destination}|

## <a name="how-are-roles-used-in-patterns"></a>Hoe worden de rollen in patronen gebruikt?
Rollen worden gebruikt in een patroon van sjabloon utterance, binnen de utterance: 

|Patroon met behulp van entiteit|
|--|
|`buy a ticket from {Location:origin} to {Location:destination}`|


## <a name="role-syntax-in-patterns"></a>Syntaxis van de rol van patronen
De entiteit en de rol worden tussen haakjes, `{}`. De entiteit en de rol worden gescheiden door een dubbele punt. 

## <a name="entity-roles-versus-collaborator-roles"></a>Entiteit functies ten opzichte van de samenwerker rollen

Entiteit rollen toepassen op het gegevensmodel van de LUIS-app. [De samenwerker](luis-concept-collaborator.md) rollen toepassen op niveaus van het ontwerpen van toegang. 

[!INCLUDE [Entity roles in batch testing - currently not supported](../../../includes/cognitive-services-luis-roles-not-supported-in-batch-testing.md)]

## <a name="next-steps"></a>Volgende stappen

* Gebruik een [praktische zelfstudie](tutorial-entity-roles.md) gebruik van entiteit met niet-machine-geleerde entiteiten
* Informatie over het toevoegen [rollen](luis-how-to-add-entities.md#add-a-role-to-pattern-based-entity)
