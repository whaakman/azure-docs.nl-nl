---
title: Systeemvariabelen in Azure Data Factory | Microsoft Docs
description: "Dit artikel wordt beschreven systeemvariabelen die worden ondersteund door Azure Data Factory. U kunt deze variabelen gebruiken in expressies voor bij het definiëren van Data Factory-entiteiten."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: shlo
ms.openlocfilehash: bdf1754226852145e9bf5597256339549f253071
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="system-variables-supported-by-azure-data-factory"></a>Systeemvariabelen die worden ondersteund door Azure Data Factory
Dit artikel wordt beschreven systeemvariabelen die worden ondersteund door Azure Data Factory. U kunt deze variabelen gebruiken in expressies voor bij het definiëren van Data Factory-entiteiten. 

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [functies en variabelen in de Data Factory versie 1](v1/data-factory-functions-variables.md).


## <a name="pipeline-scope"></a>Pipeline-bereik:

| Naam variabele | Beschrijving |
| --- | --- |
| @pipeline().DataFactory |Naam van de gegevensfactory de pijplijn uitvoeren wordt binnen uitgevoerd | 
| @pipeline().Pipeline |Naam van de pijplijn |
| @pipeline().RunId | ID van de specifieke pijplijn uitvoeren | 
| @pipeline().TriggerType | Type van de trigger die de pijplijn (handmatige, Scheduler) aangeroepen | 
| @pipeline().TriggerId| ID van de trigger die de pijplijn roept |
| @pipeline().TriggerName| Naam van de trigger die de pijplijn roept |
| @pipeline().TriggerTime| Tijd wanneer de trigger die de pijplijn wordt aangeroepen. De trigger is de werkelijke gestarte tijd, niet de geplande tijd. Bijvoorbeeld: `13:20:08.0149599Z` wordt geretourneerd in plaats van`13:20:00.00Z` |

## <a name="trigger-scope"></a>Scope activeren:

| Naam variabele | Beschrijving |
| --- | --- |
| trigger().scheduledTime |De tijd wanneer de trigger is gepland voor het aanroepen van de pijplijn worden uitgevoerd. Bijvoorbeeld: deze variabele voor een trigger die elke 5 min wordt geactiveerd, zou retourneren `2017-06-01T22:20:00Z`, `2017-06-01T22:25:00Z`, `2017-06-01T22:29:00Z` respectievelijk.|
| trigger().startTime |Tijd wanneer de trigger **daadwerkelijk** geactiveerd om aan te roepen de pijplijn worden uitgevoerd. Bijvoorbeeld voor een trigger die elke 5 min wordt geactiveerd, deze variabele kan retourneren ongeveer het volgende `2017-06-01T22:20:00.4061448Z`, `2017-06-01T22:25:00.7958577Z`, `2017-06-01T22:29:00.9935483Z` respectievelijk.|

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over hoe deze variabelen worden gebruikt in expressies voor [expressietaal & functies](control-flow-expression-language-functions.md). 
