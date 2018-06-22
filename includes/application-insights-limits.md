---
title: bestand opnemen
description: bestand opnemen
services: application-insights
author: mrbullwinkle
ms.service: application-insights
ms.topic: include
ms.date: 06/21/2018
ms.author: mbullwin
ms.custom: include file
ms.openlocfilehash: 90de751f416ca611f3c674232c224199ad7af717
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36310164"
---
Er gelden enkele beperkingen voor het aantal meetgegevens en gebeurtenissen per toepassing (per instrumentatiesleutel). De limieten zijn afhankelijk van de [prijscategorie](https://azure.microsoft.com/pricing/details/application-insights/) die u kiest.

| Resource | Standaardlimiet | Opmerking
| --- | --- | --- |
| Totale hoeveelheid gegevens per dag | 100 GB | U kunt gegevens beperken door een maximum in te stellen. Als u meer gegevens nodig hebt, kunt u de limiet in de portal verhogen tot 1000 GB. Voor meer dan 1000 GB capaciteit verzend een e-mail naar AIDataCap@microsoft.com.
| Beperking | Gebeurtenissen van 32 kB per seconde | De limiet wordt gemeten in een minuut.
| Bewaartijd van gegevens | 90 dagen | Deze resource is voor [Search](../articles/application-insights/app-insights-diagnostic-search.md), [Analytics](../articles/application-insights/app-insights-analytics.md) en [Metrics Explorer](../articles/application-insights/app-insights-metrics-explorer.md).
| Bewaartijd van gedetailleerde resultaten van [beschikbaarheidstests met meerdere stappen](../articles/application-insights/app-insights-monitor-web-app-availability.md#multi-step-web-tests) | 90 dagen | Deze resource biedt gedetailleerde resultaten van elke stap.
| Maximale gebeurtenisgrootte | 64 KB | 
| Naamlengte voor de eigenschappen en meetgegevens | 150 | Zie [typt u schema's](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/Schemas/Docs/).
| Lengte van de tekenreeks eigenschapswaarde | 8.192 | Zie [typt u schema's](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/Schemas/Docs/).
| Lengte van berichten voor tracering en uitzonderingen | 10 K | Zie [typt u schema's](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/Schemas/Docs/).
| Aantal [beschikbaarheidstests](../articles/application-insights/app-insights-monitor-web-app-availability.md) per app | 100 |
| [Profiler](../articles/application-insights/app-insights-profiler.md) bewaren van gegevens | Er zijn vijf dagen |
| [Profiler](../articles/application-insights/app-insights-profiler.md) gegevens verzonden per dag | 10 GB |

Zie [Over prijzen en quota voor Application Insights](../articles/application-insights/app-insights-pricing.md) voor meer informatie.