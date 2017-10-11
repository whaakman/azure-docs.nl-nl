---
title: Azure Application Insights telemetrie gegevensmodel - Uitzonderingstelemetrie | Microsoft Docs
description: Application Insights-gegevensmodel voor uitzonderingstelemetrie
services: application-insights
documentationcenter: .net
author: SergeyKanzhelev
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 04/25/2017
ms.author: bwren
ms.openlocfilehash: 6b220b0cb6719bac606f599d657d08ab847c7590
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="exception-telemetry-application-insights-data-model"></a>Uitzonderingstelemetrie: Application Insights-gegevensmodel

In [Application Insights](app-insights-overview.md), een exemplaar van de uitzondering vertegenwoordigt een verwerkt of niet-verwerkte uitzondering die is opgetreden tijdens het uitvoeren van de bewaakte toepassing.

## <a name="problem-id"></a>Probleem-Id

Id van waar de uitzondering is opgetreden in de code. Gebruikt voor het groeperen van uitzonderingen. Doorgaans is dit een combinatie van uitzonderingstype en een functie uit de aanroepstack.

Maximale lengte: 1024 tekens

## <a name="severity-level"></a>Ernstniveau

Ernstniveau traceren. Waarde kan zijn `Verbose`, `Information`, `Warning`, `Error`, `Critical`.

## <a name="exception-details"></a>Details van uitzondering

(Om te worden uitgebreid)

## <a name="custom-properties"></a>Aangepaste eigenschappen

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Aangepaste metingen

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Volgende stappen

- Zie [gegevensmodel](application-insights-data-model.md) voor Application Insights-typen en data model.
- Meer informatie over hoe [onderzoeken uitzonderingen in uw web-apps met Application Insights](app-insights-asp-net-exceptions.md).
- Bekijk [platforms](app-insights-platforms.md) ondersteund door de Application Insights.
