---
title: Systeemvariabelen in Azure Data Factory | Microsoft Docs
description: Dit artikel beschrijft de systeemvariabelen ondersteund door Azure Data Factory. U kunt deze variabelen gebruiken in expressies voor bij het definiëren van Data Factory-entiteiten.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 06/12/2018
ms.author: shlo
ms.openlocfilehash: 9a4d5acfe16a2fdbb3b631cb8baf6cb8e90a7d58
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54016284"
---
# <a name="system-variables-supported-by-azure-data-factory"></a>Systeemvariabelen ondersteund door Azure Data Factory
Dit artikel beschrijft de systeemvariabelen ondersteund door Azure Data Factory. U kunt deze variabelen gebruiken in expressies voor bij het definiëren van Data Factory-entiteiten.

## <a name="pipeline-scope"></a>Bereik van de pijplijn
Deze systeemvariabelen kunnen overal worden verwezen in de pijplijn-JSON.

| De naam van variabele | Description |
| --- | --- |
| @pipeline().DataFactory |Naam van de data factory de pijplijnuitvoering wordt uitgevoerd binnen |
| @pipeline().Pipeline |Naam van de pijplijn |
| @pipeline().RunId | ID van de specifieke pijplijnuitvoering |
| @pipeline().TriggerType | Type van de trigger die de pijplijn (handmatige, Scheduler) aangeroepen |
| @pipeline().TriggerId| ID van de trigger die de pijplijn aanroept |
| @pipeline().TriggerName| Naam van de trigger die de pijplijn aanroept |
| @pipeline().TriggerTime| Tijdstip waarop de trigger die de pijplijn wordt aangeroepen. De trigger is de daadwerkelijke geactiveerde tijd, niet op het geplande tijdstip. Bijvoorbeeld, `13:20:08.0149599Z` wordt geretourneerd in plaats van `13:20:00.00Z` |

## <a name="schedule-trigger-scope"></a>Bereik van de Trigger plannen
Deze systeemvariabelen kunnen overal worden verwezen in de trigger JSON als de trigger van het type is: "ScheduleTrigger."

| De naam van variabele | Description |
| --- | --- |
| @trigger.scheduledTime) |Tijd wanneer de trigger is gepland voor het aanroepen van de pijplijn-run. Bijvoorbeeld, voor een trigger die elke 5 minuten wordt geactiveerd, deze variabele retourneerde `2017-06-01T22:20:00Z`, `2017-06-01T22:25:00Z`, `2017-06-01T22:29:00Z` respectievelijk.|
| @trigger.startTime) |Tijdstip waarop de trigger **daadwerkelijk** geactiveerd voor het aanroepen van de pijplijn-run. Bijvoorbeeld voor een trigger die elke 5 minuten wordt geactiveerd, deze variabele kan retourneren er ongeveer als volgt `2017-06-01T22:20:00.4061448Z`, `2017-06-01T22:25:00.7958577Z`, `2017-06-01T22:29:00.9935483Z` respectievelijk.|

## <a name="tumbling-window-trigger-scope"></a>Tumblingvenstertrigger venster Trigger bereik
Deze systeemvariabelen kunnen overal worden verwezen in de trigger JSON als de trigger van het type is: "TumblingWindowTrigger."

| De naam van variabele | Description |
| --- | --- |
| @trigger(). outputs.windowStartTime |Begin van het venster wanneer de trigger is gepland om aan te roepen van de pijplijnuitvoering. Als tumblingvenstertriggers een frequentie van 'uur heeft' zou dit de tijd aan het begin van het uur.|
| @trigger(). outputs.windowEndTime |Einde van het venster wanneer de trigger is gepland om aan te roepen van de pijplijn-run. Als tumblingvenstertriggers een frequentie van 'uur heeft' zou dit de tijd aan het einde van het uur.|
## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over hoe deze variabelen worden gebruikt in expressies voor [expressietaal & functies](control-flow-expression-language-functions.md).
