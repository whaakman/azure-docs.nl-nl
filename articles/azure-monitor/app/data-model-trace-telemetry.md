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
ms.topic: conceptual
ms.date: 04/25/2017
ms.reviewer: sergkanz
ms.author: mbullwin
ms.openlocfilehash: b777928295d37b0824287d2b9e08526eae29e0f9
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2019
ms.locfileid: "53976491"
---
# <a name="trace-telemetry-application-insights-data-model"></a>-Tracetelemetrie: Application Insights-gegevensmodel

Traceren telemetrie (in [Application Insights](../../application-insights/app-insights-overview.md)) vertegenwoordigt `printf` stijl trace-instructies die tekst gezocht. `Log4Net`, `NLog`, en andere vermeldingen in logboekbestanden op tekst gebaseerde worden vertaald naar instanties van dit type. De tracering heeft geen metingen als een uitbreidbaarheid.

## <a name="message"></a>Bericht

Traceringsbericht.

Maximumlengte: 32.768 tekens

## <a name="severity-level"></a>Ernstniveau

Traceer ernstniveau. Waarde kan zijn `Verbose`, `Information`, `Warning`, `Error`, `Critical`.

## <a name="custom-properties"></a>Aangepaste eigenschappen

[!INCLUDE [application-insights-data-model-properties](../../../includes/application-insights-data-model-properties.md)]

## <a name="next-steps"></a>Volgende stappen

- [.NET-traceerlogboeken in Application Insights verkennen](../../azure-monitor/app/asp-net-trace-logs.md).
- [Verken in Application Insights traceerlogboeken Java](../../azure-monitor/app/java-trace-logs.md).
- Zie [gegevensmodel](data-model.md) voor Application Insights-typen en -gegevensmodel.
- [Aangepaste traceersessie telemetrie schrijven](../../azure-monitor/app/api-custom-events-metrics.md#tracktrace)
- Bekijk [platforms](../../application-insights/app-insights-platforms.md) ondersteund door Application Insights.
