---
title: Azure Application Insights telemetrie gegevensmodel - Tracetelemetrie | Microsoft Docs
description: Application Insights-gegevensmodel voor tracetelemetrie
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 04/25/2017
ms.author: mbullwin; sergkanz
ms.openlocfilehash: d93ed9f292b6c05d0a3fb3202567f4024f62e35e
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2018
---
# <a name="trace-telemetry-application-insights-data-model"></a>Telemetrie traceren: Application Insights-gegevensmodel

Telemetrie traceren (in [Application Insights](app-insights-overview.md)) vertegenwoordigt `printf` stijl trace-instructies die tekst doorzocht. `Log4Net`, `NLog`, en andere vermeldingen in logboekbestanden op basis van tekst worden vertaald naar exemplaren van dit type. De tracering heeft geen metingen als een uitbreidbaarheid.

## <a name="message"></a>Bericht

Trace-bericht.

Maximale lengte: 32768 tekens

## <a name="severity-level"></a>Ernstniveau

Ernstniveau traceren. Waarde kan zijn `Verbose`, `Information`, `Warning`, `Error`, `Critical`.

## <a name="custom-properties"></a>Aangepaste eigenschappen

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="next-steps"></a>Volgende stappen

- [.NET-traceerlogboeken in Application Insights verkennen](app-insights-asp-net-trace-logs.md).
- [Java in Application Insights traceerlogboeken verkennen](app-insights-java-trace-logs.md).
- Zie [gegevensmodel](application-insights-data-model.md) voor Application Insights-typen en data model.
- [Aangepaste traceersessie telemetrie schrijven](app-insights-api-custom-events-metrics.md#tracktrace)
- Bekijk [platforms](app-insights-platforms.md) ondersteund door de Application Insights.
