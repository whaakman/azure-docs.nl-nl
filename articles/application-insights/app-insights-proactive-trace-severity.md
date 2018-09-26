---
title: 'Slimme detectie: prestatievermindering in trace ernst verhouding, in Azure Application Insights | Microsoft Docs'
description: Toepassingstraceringen met Azure Application Insights voor ongewone patronen in tracetelemetrie bewaken.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 11/27/2017
ms.author: mbullwin
ms.openlocfilehash: 41e972145d2404e46b62521c7d40d997be74a7f9
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47093839"
---
# <a name="degradation-in-trace-severity-ratio-preview"></a>Degradatie traceren ernst breedteverhouding (preview)

Traceringen worden veel gebruikt in toepassingen, zoals ze het verhaal vertellen te van wat er achter de schermen gebeurt. Wanneer er iets fout gaat, bieden traceringen cruciale inzicht in de volgorde van gebeurtenissen die leiden naar de ongewenste status. Traceringen worden doorgaans ongestructureerde, maar er is één ding dat concrete invulling te geven kan worden geleerd van hen de ernst op. In van een toepassing onveranderlijke, verwachten we de verhouding tussen "tot" traceringen (*Info* en *uitgebreid*) en 'slechte' traces (*waarschuwing*, *fout*, en *kritieke*) blijft stabiel. Verondersteld wordt dat 'slechte' traceringen op gezette tijden tot op zekere hoogte een aantal oorzaken hebben gebeuren kunnen (bijvoorbeeld problemen tijdelijk afwijkende netwerkverbinding). Maar wanneer een echt probleem groeien begint, het meestal als een toename in de verhouding van 'slechte' traceringen vs "tot" traceringen manifesten. Application Insights Slimme detectie automatisch analyseert de traceringen vastgelegd door uw toepassing, en kunt u gewaarschuwd over ongebruikelijke patronen in de ernst van de tracetelemetrie van uw.

Deze functie is vereist geen speciale instellingen, dan traceerlogboekregistratie voor uw app configureren (informatie over het configureren van een trace log-listener voor [.NET](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-trace-logs) of [Java](https://docs.microsoft.com/azure/application-insights/app-insights-java-trace-logs)). Deze is actief wanneer uw app zoveel uitzonderingstelemetrie genereert.

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Wanneer kan ik dit type melding voor slimme detectie krijgen?
U kunt dit type melding krijgen als de verhouding tussen "tot" traceringen (traceringen die zijn geregistreerd met een niveau van *Info* of *uitgebreid*) en 'slechte' traces (traceringen die zijn geregistreerd met een niveau van *waarschuwing*, * Fout, of *onherstelbare fout*) is vernederen in een specifieke dag, vergeleken met een basislijn berekend voor de afgelopen zeven dagen.

## <a name="does-my-app-definitely-have-a-problem"></a>Beschikt over mijn app absoluut een probleem?
Nee, een melding betekent niet dat uw app beslist een probleem is. Hoewel een verslechtering van de in de verhouding tussen 'goede' en 'slechte' traceringen op een toepassingsprobleem wijzen kan, is het mogelijk dat deze wijziging in verhouding goedaardige. Bijvoorbeeld, de toename mogelijk vanwege een nieuwe stroom in de toepassing meer 'slechte' traceringen dan de bestaande stromen genereren).

## <a name="how-do-i-fix-it"></a>Hoe herstel ik deze?
De meldingen betreffen: diagnostische gegevens voor de ondersteuning in het proces van diagnostische gegevens:
1. **Sorteren.** De melding ziet u hoe veel bewerkingen worden beïnvloed. Hiermee kunt u een prioriteit toewijzen aan het probleem.
2. **De scope.** Het probleem invloed heeft op al het verkeer of slechts enkele bewerking? Deze gegevens kan worden opgehaald van de melding.
3. **Diagnose.** U kunt de verwante items en rapporten koppelen aan de ondersteunende informatie om u te helpen meer vaststellen van het probleem.


