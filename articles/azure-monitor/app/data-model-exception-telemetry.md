---
title: Azure Application Insights telemetrie Data Model - Uitzonderingstelemetrie | Microsoft Docs
description: Application Insights-gegevensmodel voor uitzonderingstelemetrie
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
ms.openlocfilehash: 5800bbc71158da42bc0baf21a4219be3ce54b22b
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2019
ms.locfileid: "54000515"
---
# <a name="exception-telemetry-application-insights-data-model"></a>Uitzonderingstelemetrie: Application Insights-gegevensmodel

In [Application Insights](../../application-insights/app-insights-overview.md), een exemplaar van de uitzondering vertegenwoordigt een verwerkt of niet-verwerkte uitzondering die is opgetreden tijdens het uitvoeren van de bewaakte toepassing.

## <a name="problem-id"></a>Probleem-id

Id van waar de uitzondering is opgetreden in de code. Gebruikt voor het groeperen van uitzonderingen. Meestal een combinatie van uitzonderingstype en een functie van de aanroepstack.

Maximumlengte: 1024 tekens

## <a name="severity-level"></a>Ernstniveau

Traceer ernstniveau. Waarde kan zijn `Verbose`, `Information`, `Warning`, `Error`, `Critical`.

## <a name="exception-details"></a>Details van uitzondering

(Kan worden uitgebreid)

## <a name="custom-properties"></a>Aangepaste eigenschappen

[!INCLUDE [application-insights-data-model-properties](../../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Aangepaste metingen

[!INCLUDE [application-insights-data-model-measurements](../../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Volgende stappen

- Zie [gegevensmodel](data-model.md) voor Application Insights-typen en -gegevensmodel.
- Meer informatie over het [diagnose-uitzonderingen in uw web-apps met Application Insights](../../azure-monitor/app/asp-net-exceptions.md).
- Bekijk [platforms](../../azure-monitor/app/platforms.md) ondersteund door Application Insights.
