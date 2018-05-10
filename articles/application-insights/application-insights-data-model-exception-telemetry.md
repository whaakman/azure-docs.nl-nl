---
title: Azure Application Insights telemetrie gegevensmodel - Uitzonderingstelemetrie | Microsoft Docs
description: Application Insights-gegevensmodel voor uitzonderingstelemetrie
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
ms.openlocfilehash: 036670c46a0ff40dd7b20a03c90f10513395cd71
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2018
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
