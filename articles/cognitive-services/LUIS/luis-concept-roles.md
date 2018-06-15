---
title: Inzicht in hoe functies worden gebruikt in op basis van een patroon entiteiten - Azure | Microsoft Docs
description: Meer informatie over hoe een rol wordt gebruikt in een entiteit op basis van een patroon voor een subtype contextuele entiteit een naam geven.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 06/08/2018
ms.author: v-geberr;
ms.openlocfilehash: ab6100e33fb767528b87c6afde4c5ef275fc7c81
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35350052"
---
# <a name="entity-roles-in-patterns-are-contextual-subtypes"></a>Entiteit-rollen in patronen zijn contextuele subtypen
Rollen zijn met de naam, contextgevoelige subtypen van een entiteit die uitsluitend gebruikt in [patronen](luis-concept-patterns.md).

Bijvoorbeeld, in de utterance `buy a ticket from New York to London`, New York zowel Londen steden zijn, maar elk een andere betekenis heeft in de zin. Den Haag is de plaats van de oorsprong en Londen is de plaats van de bestemming. 

Rollen Geef een naam aan die verschillen:

|Entiteit|Rol|Doel|
|--|--|--|
|Locatie|oorsprong|waar het vlak van verlaat|
|Locatie|destination|waar het vliegtuig morgen|

## <a name="how-are-roles-used-in-patterns"></a>Hoe worden de rollen in patronen gebruikt?
In een patroon sjabloon utterance, worden de functies gebruikt in de utterance: 

```
buy a ticket from {Location:origin} to {Location:destination}
```

## <a name="role-syntax-in-patterns"></a>De syntaxis van de rol in patronen
De entiteit en de rol staan tussen haakjes `{}`. De entiteit en de rol worden gescheiden door een dubbele punt. 

## <a name="roles-versus-hierarchical-entities"></a>Rollen versus hiërarchische entiteiten
Hiërarchische entiteiten bieden de contextuele informatie als rollen, maar alleen voor utterances in **intents**. Daarnaast bieden de contextuele informatie als hiërarchische entiteiten, maar alleen in **patronen**.

|Contextafhankelijke learning|Gebruikt in|
|--|--|
|hiërarchische entiteiten|Intents|
|rolls|Patronen|

## <a name="next-steps"></a>Volgende stappen

* Informatie over het toevoegen [rollen](luis-how-to-add-entities.md#add-role-to-pattern-based-entity)
