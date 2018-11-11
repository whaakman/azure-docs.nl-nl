---
title: Informatie over hoe functies worden gebruikt in entiteiten op basis van het patroon
titleSuffix: Azure Cognitive Services
description: Rollen zijn met de naam, contextuele subtypen van een entiteit die alleen in de patronen gebruikt. Bijvoorbeeld, in de utterance kopen een ticket van de New York naar Londen, New York zowel Londen steden zijn maar elk een andere betekenis heeft in de zin. New York is de plaats van de oorsprong en Londen is de plaats van bestemming.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 8a92852a2721bd391ddf7c3cf3489b820c4a1400
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2018
ms.locfileid: "51277612"
---
# <a name="entity-roles-in-patterns-are-contextual-subtypes"></a>Rollen van de entiteit in de patronen zijn contextuele subtypen
Rollen zijn met de naam, contextuele subtypen van een entiteit gebruikt alleen in [patronen](luis-concept-patterns.md).

Bijvoorbeeld, in de utterance `buy a ticket from New York to London`, New York zowel Londen zijn steden, maar elk een andere betekenis heeft in de zin. New York is de plaats van de oorsprong en Londen is de plaats van bestemming. 

Functies bieden een naam op die verschillen:

|Entiteit|Rol|Doel|
|--|--|--|
|Locatie|oorsprong|waar het vlak van verlaat|
|Locatie|destination|waar het vlak van terechtkomt|
|Vooraf gedefinieerde datetimeV2|tot|einddatum|
|Vooraf gedefinieerde datetimeV2|uit|begindatum|

## <a name="how-are-roles-used-in-patterns"></a>Hoe worden de rollen in patronen gebruikt?
Rollen worden gebruikt in een patroon van sjabloon utterance, binnen de utterance: 

|Patroon met behulp van entiteit|
|--|
|`buy a ticket from {Location:origin} to {Location:destination}`|


## <a name="role-syntax-in-patterns"></a>Syntaxis van de rol van patronen
De entiteit en de rol worden tussen haakjes, `{}`. De entiteit en de rol worden gescheiden door een dubbele punt. 


[!INCLUDE[H2 Roles versus hierarchical entities](../../../includes/cognitive-services-luis-hier-roles.md)] 

## <a name="roles-with-prebuilt-entities"></a>Rollen met vooraf gemaakte entiteiten

Rollen bieden verschillende exemplaren van de vooraf gedefinieerde entiteit binnen een utterance betekenis met vooraf gemaakte entiteiten gebruiken. 

### <a name="roles-with-datetimev2"></a>Rollen met datetimeV2

De vooraf gedefinieerde entiteit, datetimeV2, geeft een heldere van informatie over een breed scala aan verschillende in datums en tijden in uitingen. Het is raadzaam om datums en datumbereiken anders dan de vooraf gedefinieerde entiteit standaard inzicht te geven. 

## <a name="next-steps"></a>Volgende stappen

* Informatie over het toevoegen [rollen](luis-how-to-add-entities.md#add-a-role-to-pattern-based-entity)
