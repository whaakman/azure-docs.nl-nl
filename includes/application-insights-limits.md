---
title: bestand opnemen
description: bestand opnemen
services: application-insights
author: mrbullwinkle
ms.service: application-insights
ms.topic: include
ms.date: 06/20/2019
ms.author: mbullwin
ms.custom: include file
ms.openlocfilehash: 708421feca879ade26a1488541fa7fbf8a601358
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67303834"
---
Gelden enkele beperkingen voor het aantal meetgegevens en gebeurtenissen per toepassing, dat wil zeggen, er per instrumentatiesleutel. De limieten zijn afhankelijk van de [prijscategorie](https://azure.microsoft.com/pricing/details/application-insights/) die u kiest.

| Resource | Standaardlimiet | Opmerking
| --- | --- | --- |
| Totale hoeveelheid gegevens per dag | 100 GB | U kunt gegevens beperken door een maximum in te stellen. Als u meer gegevens nodig hebt, kunt u de limiet in de portal verhogen tot 1000 GB. E-mailbericht verzendt voor groter is dan 1000 GB capaciteit AIDataCap@microsoft.com.
| Beperking | 32.000 gebeurtenissen/seconde | De limiet wordt gemeten in een minuut.
| Bewaartijd van gegevens | 90 dagen | Deze resource is voor [Search](../articles/azure-monitor/app/diagnostic-search.md), [Analytics](../articles/azure-monitor/app/analytics.md) en [Metrics Explorer](../articles/azure-monitor/app/metrics-explorer.md).
| Bewaartijd van gedetailleerde resultaten van [beschikbaarheidstests met meerdere stappen](../articles/azure-monitor/app/availability-multistep.md) | 90 dagen | Deze resource biedt gedetailleerde resultaten van elke stap.
| Maximale gebeurtenisgrootte | 64,000 |
| Naamlengte voor de eigenschappen en meetgegevens | 150 | Zie [typt u schema's](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/Schemas/Docs/).
| Lengte van de tekenreeks eigenschapswaarde | 8\.192 | Zie [typt u schema's](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/Schemas/Docs/).
| Lengte van berichten voor tracering en uitzonderingen | 32,768  | Zie [typt u schema's](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/Schemas/Docs/).
| Aantal [beschikbaarheidstests](../articles/azure-monitor/app/monitor-web-app-availability.md) per app | 100 |
| [Profiler](../articles/azure-monitor/app/profiler.md) bewaren van gegevens | 5 dagen |
| [Profiler](../articles/azure-monitor/app/profiler.md) gegevens die per dag worden verzonden | 10 GB |

Zie [Over prijzen en quota voor Application Insights](../articles/azure-monitor/app/pricing.md) voor meer informatie.