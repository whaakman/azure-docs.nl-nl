---
title: Systeemvariabelen in Azure Data Factory | Microsoft Docs
description: Dit artikel wordt beschreven systeemvariabelen die worden ondersteund door Azure Data Factory. U kunt deze variabelen gebruiken in expressies voor bij het definiëren van Data Factory-entiteiten.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/12/2018
ms.author: shlo
ms.openlocfilehash: 43ea8703bdbfc23511c831a5f91c9461948cc254
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37058960"
---
# <a name="system-variables-supported-by-azure-data-factory"></a>Systeemvariabelen die worden ondersteund door Azure Data Factory
Dit artikel wordt beschreven systeemvariabelen die worden ondersteund door Azure Data Factory. U kunt deze variabelen gebruiken in expressies voor bij het definiëren van Data Factory-entiteiten.

## <a name="pipeline-scope"></a>Pipeline-bereik
Deze systeemvariabelen kunnen overal in de JSON-pijplijn worden verwezen.

| Naam variabele | Beschrijving |
| --- | --- |
| @pipeline().DataFactory |Naam van de gegevensfactory de pijplijn uitvoeren wordt binnen uitgevoerd |
| @pipeline().Pipeline |Naam van de pijplijn |
| @pipeline().RunId | ID van de specifieke pijplijn uitvoeren |
| @pipeline().TriggerType | Type van de trigger die de pijplijn (handmatige, Scheduler) aangeroepen |
| @pipeline().TriggerId| ID van de trigger die de pijplijn roept |
| @pipeline().TriggerName| Naam van de trigger die de pijplijn roept |
| @pipeline().TriggerTime| Tijd wanneer de trigger die de pijplijn wordt aangeroepen. De trigger is de werkelijke gestarte tijd, niet de geplande tijd. Bijvoorbeeld: `13:20:08.0149599Z` wordt geretourneerd in plaats van `13:20:00.00Z` |

## <a name="schedule-trigger-scope"></a>Trigger bereik plannen
Deze systeemvariabelen kunnen een willekeurige plaats in de JSON van de trigger worden verwezen als de trigger van het type is: "ScheduleTrigger."

| Naam variabele | Beschrijving |
| --- | --- |
| @trigger.scheduledTime) |De tijd wanneer de trigger is gepland voor het aanroepen van de pijplijn worden uitgevoerd. Bijvoorbeeld: deze variabele voor een trigger die elke 5 min wordt geactiveerd, zou retourneren `2017-06-01T22:20:00Z`, `2017-06-01T22:25:00Z`, `2017-06-01T22:29:00Z` respectievelijk.|
| @trigger.startTime) |Tijd wanneer de trigger **daadwerkelijk** geactiveerd om aan te roepen de pijplijn worden uitgevoerd. Bijvoorbeeld voor een trigger die elke 5 min wordt geactiveerd, deze variabele kan retourneren ongeveer het volgende `2017-06-01T22:20:00.4061448Z`, `2017-06-01T22:25:00.7958577Z`, `2017-06-01T22:29:00.9935483Z` respectievelijk.|

## <a name="tumbling-window-trigger-scope"></a>Daling venster Trigger bereik
Deze systeemvariabelen kunnen een willekeurige plaats in de JSON van de trigger worden verwezen als de trigger van het type is: "TumblingWindowTrigger."

| Naam variabele | Beschrijving |
| --- | --- |
| @trigger(). outputs.windowStartTime |Starten van het venster als de trigger is gepland om aan te roepen de pijplijn uitvoeren. Als de daling venster trigger een frequentie van 'per uur heeft' zou dit de tijd aan het begin van het uur.|
| @trigger(). outputs.windowEndTime |Einde van het venster als de trigger is gepland om aan te roepen de pijplijn worden uitgevoerd. Als de daling venster trigger een frequentie van 'per uur heeft' zou dit de tijd aan het einde van het uur.|
## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over hoe deze variabelen worden gebruikt in expressies voor [expressietaal & functies](control-flow-expression-language-functions.md).
