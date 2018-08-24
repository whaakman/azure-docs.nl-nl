---
title: bestand opnemen
description: bestand opnemen
services: functions
author: ggailey777
manager: jeconnoc
ms.service: functions
ms.topic: include
ms.date: 08/12/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 3cbe634d862682a5f6b06c2cfc77a4d3b03954f9
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "42811592"
---
De code voor alle functies in een specifieke functie-app bevindt zich in een hoofdmap (`wwwroot`) die een host-configuratiebestand als een of meer submappen bevat. Elke submap bevat de code voor een afzonderlijke functie, zoals in het volgende voorbeeld:

```
wwwroot
 | - host.json
 | - mynodefunction
 | | - function.json
 | | - index.js
 | | - node_modules
 | | | - ... packages ...
 | | - package.json
 | - mycsharpfunction
 | | - function.json
 | | - run.csx
 | - bin
 | | - mycompiledcsharp.dll
```

Het bestand host.json bevat een runtime-specifieke configuraties en bevindt zich in de hoofdmap van de functie-app. Zie voor meer informatie over de instellingen die beschikbaar zijn, de [naslaginformatie over host.json](../articles/azure-functions/functions-host-json.md).

Elke functie heeft een map met een of meer codebestanden, de configuratie van de function.json en andere afhankelijkheden. Voor een C#-klassebibliotheekproject, de gecompileerde klasse bibliotheek (.dll)-bestand is geïmplementeerd in de `bin` submap.

