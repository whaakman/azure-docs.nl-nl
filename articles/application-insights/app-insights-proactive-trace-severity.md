---
title: Detectie - verslechtering van de tracering ernst verhouding in Azure Application Insights van smartcard | Microsoft Docs
description: Toepassingstraceringen met Azure Application Insights voor ongewone patronen in tracetelemetrie bewaken.
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/27/2017
ms.author: mbullwin
ms.openlocfilehash: 29ed195dadb7230df425d6d981a0a482e09ee79f
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/09/2017
---
# <a name="degradation-in-trace-severity-ratio-preview"></a>Verslechtering van de tracering ernst verhouding (preview)

Traceringen worden veel gebruikt in toepassingen, zoals ze helpen bij het verhaal van wat er achter de schermen gebeurt. Bij problemen, traceringen cruciaal inzicht verschaffen in de volgorde van gebeurtenissen, wat leidt tot de ongewenste status. Traceringen worden doorgaans ongestructureerde, maar er is één ding dat concrete invulling te geven kan worden geleerd van hen hun ernst. In de actieve status van een toepassing, verwachten we de verhouding tussen 'goede' traceringen (*Info* en *uitgebreid*) en 'slechte' traceringen (*waarschuwing*, *fout* en *Kritiek*) stabiel blijven. Verondersteld wordt dat 'slechte' traceringen op regelmatige basis tot op zekere hoogte een aantal oorzaken hebben gebeuren kunnen (bijvoorbeeld problemen tijdelijk afwijkende netwerkverbinding). Maar wanneer een echte probleem groeiende begint, er meestal als een toename van de verhouding 'slechte' traceringen vs 'goede' traceringen manifesten. Application Insights Slimme detectie automatisch analyseert de traceringen vastgelegd door uw toepassing en kunt u gewaarschuwd over ongewone patronen in de ernst van uw tracetelemetrie.

Dit onderdeel vereist geen speciale instellingen, dan traceerlogboekregistratie voor uw app configureren (informatie over het configureren van een logboek traceringslistener voor [.NET](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-trace-logs) of [Java](https://docs.microsoft.com/azure/application-insights/app-insights-java-trace-logs)). Het is actief wanneer uw app genoeg uitzonderingstelemetrie genereert.

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Wanneer dit type Slimme detectie melding krijgt?
Kunt u dit type melding krijgen als de verhouding tussen 'goede' traceringen (traceringen die zijn geregistreerd met een niveau van *Info* of *uitgebreid*) en 'slechte' traceringen (traceringen die zijn geregistreerd met een niveau van *waarschuwing*, * Fout, of *onherstelbare fout*) in een specifieke dag, vergeleken met een basislijn berekend gedurende de afgelopen zeven dagen is beïnvloeden.

## <a name="does-my-app-definitely-have-a-problem"></a>Beschikt over mijn app zeker een probleem?
Nee, een melding betekent dat uw app zeker een probleem heeft niet. Hoewel een verslechtering van de verhouding tussen 'goede' en 'slechte' traceringen kan duiden op een toepassingsprobleem, kan deze wijziging in verhouding onschadelijk zijn. Bijvoorbeeld, de toename mogelijk vanwege een nieuwe werkstroom in de toepassing meer 'slechte' traceringen dan bestaande stromen tekensetcodering).

## <a name="how-do-i-fix-it"></a>Hoe kan ik oplossen?
Diagnostische gegevens voor de ondersteuning van het proces van diagnostische gegevens bevatten, de meldingen:
1. **Selectie.** De melding ziet u hoe veel bewerkingen worden beïnvloed. Hiermee kunt u een prioriteit toewijzen aan het probleem.
2. **De scope.** Het probleem invloed op alle verkeer, of een enkele bewerking? Deze informatie kan worden verkregen van de melding.
3. **Diagnose.** U kunt de verwante items en rapporten koppelen aan de ondersteunende informatie om u te helpen verdere vaststellen van het probleem.


