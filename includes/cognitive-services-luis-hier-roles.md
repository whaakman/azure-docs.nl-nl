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
ms.date: 10/23/2018
ms.author: diberry
ms.openlocfilehash: 0ad3b360611a12b95568e9a20f13a57c93b2e603
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50086118"
---
## <a name="roles-versus-hierarchical-entities"></a>Functies ten opzichte van hiërarchische entiteiten

Moet u een hiërarchische entiteit of een patroon gebruiken met een enkele entiteit met de rol? 

Dat hangt ervan af. Patronen en voorbeeld-uitingen zijn vergelijkbaar in die staan voor een gebruiker utterance en specifiek zijn voor een doel.  

Als de uitingen geen een duidelijk patroon, hiërarchische entiteiten gebruiken. 

|hiërarchische entiteiten|Eenvoudige entiteit met rollen|
|--|--|
|Beschikbaar in de voorbeeld-uitingen en patronen van doel|Alleen beschikbaar in de patronen|
|Moeten hebben voorbeeld uitingen in bedoeling met onderliggende entiteiten met het label|Rollen kunnen niet worden met het label in de voorbeeld-uitingen in doel|
|Mogelijk moet **meer** voorbeeld uitingen in de bedoeling om op te halen van entiteit|Hoeft **minder** voorbeeld uitingen in doel|
