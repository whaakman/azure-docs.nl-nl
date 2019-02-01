---
title: bestand opnemen
description: bestand opnemen
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.subservice: luis
ms.topic: include
ms.custom: include file
ms.date: 10/23/2018
ms.author: diberry
ms.openlocfilehash: dee31fca841ea309128681637cc41fa0fb1e7aea
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55480467"
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
