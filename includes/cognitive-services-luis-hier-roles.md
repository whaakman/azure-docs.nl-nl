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
ms.date: 08/02/2018
ms.author: diberry
ms.openlocfilehash: dd99218765c873120c4117a3ec1712fe0a605e66
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/15/2018
ms.locfileid: "45637588"
---
**Vraag**: moet u een hiërarchische entiteit of een patroon met een enkele entiteit met de rol? 

**Antwoord**: dat hangt ervan af. Patronen en voorbeeld-uitingen zijn vergelijkbaar in die staan voor een gebruiker utterance en specifiek zijn voor een doel.  

Als de uitingen geen een duidelijk patroon, hiërarchische entiteiten gebruiken. 

|hiërarchische entiteiten|Eenvoudige entiteit met rollen|
|--|--|
|moeten hebben voorbeeld uitingen met onderliggende entiteiten met het label in intents|voorbeeld-uitingen als ze beschikken **rollen kunnen niet worden met het label in intents**|
|in de patronen kunt gebruiken|**moet** in patronen gebruiken|
|mogelijk moet **meer** voorbeeld uitingen in doel|mogelijk moet ** minder voorbeeld uitingen in doel|