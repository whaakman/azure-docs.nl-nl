---
title: bestand opnemen
description: bestand opnemen
services: application-insights
author: mrbullwinkle
ms.service: application-insights
ms.topic: include
ms.date: 08/06/2019
ms.author: mbullwin
ms.custom: include file
ms.openlocfilehash: 7bf9c338315f5013be815625b06d633567a801a0
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68912342"
---
Er zijn enkele limieten voor het aantal metrische gegevens en gebeurtenissen per toepassing, dat wil zeggen, per instrumentatie sleutel. De limieten zijn afhankelijk van de [prijscategorie](https://azure.microsoft.com/pricing/details/application-insights/) die u kiest.

| Resource | Standaardlimiet | Opmerking
| --- | --- | --- |
| Totale hoeveelheid gegevens per dag | 100 GB | U kunt gegevens beperken door een maximum in te stellen. Als u meer gegevens nodig hebt, kunt u de limiet in de portal verhogen tot 1.000 GB. Voor capaciteit groter dan 1.000 GB stuurt u een e-mail AIDataCap@microsoft.comnaar.
| Beperken | 32.000 gebeurtenissen per seconde | De limiet wordt gemeten in een minuut.
| Bewaartijd van gegevens | 90 dagen | Deze resource is voor [Search](../articles/azure-monitor/app/diagnostic-search.md), [Analytics](../articles/azure-monitor/app/analytics.md) en [Metrics Explorer](../articles/azure-monitor/app/metrics-explorer.md).
| Bewaartijd van gedetailleerde resultaten van [beschikbaarheidstests met meerdere stappen](../articles/azure-monitor/app/availability-multistep.md) | 90 dagen | Deze resource biedt gedetailleerde resultaten van elke stap.
| Maximale grootte van het telemetrie-item | 64 kB |
| Maximum aantal telemetriegegevens per batch | 64 K |
| Naamlengte voor de eigenschappen en meetgegevens | 150 | Zie [schema typen](https://github.com/microsoft/ApplicationInsights-Home/tree/master/EndpointSpecs/Schemas/Bond).
| Lengte van de tekenreeks eigenschapswaarde | 8\.192  | Zie [schema typen](https://github.com/microsoft/ApplicationInsights-Home/tree/master/EndpointSpecs/Schemas/Bond).
| Lengte van berichten voor tracering en uitzonderingen | 32.768  | Zie [schema typen](https://github.com/microsoft/ApplicationInsights-Home/tree/master/EndpointSpecs/Schemas/Bond).
| Aantal [beschikbaarheidstests](../articles/azure-monitor/app/monitor-web-app-availability.md) per app | 100 |
| [Profiler](../articles/azure-monitor/app/profiler.md) gegevens retentie | 5 dagen |
| Gegevens van [Profiler](../articles/azure-monitor/app/profiler.md) per dag verzonden | 10 GB |

Zie [Over prijzen en quota voor Application Insights](../articles/azure-monitor/app/pricing.md) voor meer informatie.