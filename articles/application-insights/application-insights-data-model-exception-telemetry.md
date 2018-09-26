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
ms.devlang: multiple
ms.topic: conceptual
ms.date: 04/25/2017
ms.reviewer: sergkanz
ms.author: mbullwin
ms.openlocfilehash: dc5480b90ef6b5520f47c51f0c105202d7071089
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47093590"
---
# <a name="exception-telemetry-application-insights-data-model"></a>Uitzonderingstelemetrie: Application Insights-gegevensmodel

In [Application Insights](app-insights-overview.md), een exemplaar van de uitzondering vertegenwoordigt een verwerkt of niet-verwerkte uitzondering die is opgetreden tijdens het uitvoeren van de bewaakte toepassing.

## <a name="problem-id"></a>Probleem-id

Id van waar de uitzondering is opgetreden in de code. Gebruikt voor het groeperen van uitzonderingen. Meestal een combinatie van uitzonderingstype en een functie van de aanroepstack.

Maximale lengte: 1024 tekens

## <a name="severity-level"></a>Ernstniveau

Traceer ernstniveau. Waarde kan zijn `Verbose`, `Information`, `Warning`, `Error`, `Critical`.

## <a name="exception-details"></a>Details van uitzondering

(Kan worden uitgebreid)

## <a name="custom-properties"></a>Aangepaste eigenschappen

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Aangepaste metingen

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Volgende stappen

- Zie [gegevensmodel](application-insights-data-model.md) voor Application Insights-typen en -gegevensmodel.
- Meer informatie over het [diagnose-uitzonderingen in uw web-apps met Application Insights](app-insights-asp-net-exceptions.md).
- Bekijk [platforms](app-insights-platforms.md) ondersteund door Application Insights.
