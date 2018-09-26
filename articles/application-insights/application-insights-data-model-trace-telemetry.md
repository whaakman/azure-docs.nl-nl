---
title: Azure Application Insights telemetrie Data Model - Tracetelemetrie | Microsoft Docs
description: Application Insights-gegevensmodel voor tracetelemetrie
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: conceptual
ms.date: 04/25/2017
ms.reviewer: sergkanz
ms.author: mbullwin
ms.openlocfilehash: 79d012ddb454842e53fe296d9b618438ee7b3e91
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47093822"
---
# <a name="trace-telemetry-application-insights-data-model"></a>Telemetrie traceren: Application Insights-gegevensmodel

Traceren telemetrie (in [Application Insights](app-insights-overview.md)) vertegenwoordigt `printf` stijl trace-instructies die tekst gezocht. `Log4Net`, `NLog`, en andere vermeldingen in logboekbestanden op tekst gebaseerde worden vertaald naar instanties van dit type. De tracering heeft geen metingen als een uitbreidbaarheid.

## <a name="message"></a>Bericht

Traceringsbericht.

Maximale lengte: 32.768 tekens

## <a name="severity-level"></a>Ernstniveau

Traceer ernstniveau. Waarde kan zijn `Verbose`, `Information`, `Warning`, `Error`, `Critical`.

## <a name="custom-properties"></a>Aangepaste eigenschappen

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="next-steps"></a>Volgende stappen

- [.NET-traceerlogboeken in Application Insights verkennen](app-insights-asp-net-trace-logs.md).
- [Verken in Application Insights traceerlogboeken Java](app-insights-java-trace-logs.md).
- Zie [gegevensmodel](application-insights-data-model.md) voor Application Insights-typen en -gegevensmodel.
- [Aangepaste traceersessie telemetrie schrijven](app-insights-api-custom-events-metrics.md#tracktrace)
- Bekijk [platforms](app-insights-platforms.md) ondersteund door Application Insights.
