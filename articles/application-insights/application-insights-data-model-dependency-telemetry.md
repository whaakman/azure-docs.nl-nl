---
title: Azure Application Insights Telemetry Data Model - Afhankelijkheidstelemetrie | Microsoft Docs
description: Application Insights-gegevensmodel voor afhankelijkheidstelemetrie
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: conceptual
ms.date: 04/17/2017
ms.reviewer: sergkanz
ms.author: mbullwin
ms.openlocfilehash: 5765be9fc88cbe38841078b5c298d3ee12269e6d
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47092550"
---
# <a name="dependency-telemetry-application-insights-data-model"></a>Afhankelijkheidstelemetrie: Application Insights-gegevensmodel

Afhankelijkheidstelemetrie (in [Application Insights](app-insights-overview.md)) vertegenwoordigt een interactie van het bewaakte onderdeel met een externe onderdeel, zoals SQL of een HTTP-eindpunt.

## <a name="name"></a>Naam

Naam van de opdracht die is gestart met deze afhankelijkheidsaanroep. De kardinaliteit van de lage waarde. Voorbeelden zijn opgeslagen procedurenaam worden opgegeven en pad van URL van de sjabloon.

## <a name="id"></a>Id

Id van een exemplaar van de aanroep van afhankelijkheid. Gebruikt voor correlatie met het verzoek telemetrie-item overeenkomt met deze afhankelijkheidsaanroep. Zie voor meer informatie, [correlatie](application-insights-correlation.md) pagina.

## <a name="data"></a>Gegevens

De opdracht is gestart door deze afhankelijkheidsaanroep. Voorbeelden zijn SQL-instructie en HTTP-URL met alle queryparameters.

## <a name="type"></a>Type

Naam van afhankelijkheid. De kardinaliteit van de lage waarde voor de logische groepering van de afhankelijkheden en de interpretatie van andere velden, zoals commandName en resultCode. Voorbeelden zijn SQL-, Azure table- en HTTP.

## <a name="target"></a>Doel

Doelsite van de afhankelijkheidsaanroep van een. Voorbeelden zijn de naam van de server, host-adres. Zie voor meer informatie, [correlatie](application-insights-correlation.md) pagina.

## <a name="duration"></a>Duur

Duur in de indeling van aanvraag: `DD.HH:MM:SS.MMMMMM`. Moet minder dan `1000` dagen.

## <a name="result-code"></a>Resultaatcode

De resultaatcode van de afhankelijkheidsaanroep van een. Voorbeelden zijn SQL-foutcode en HTTP-statuscode.

## <a name="success"></a>Geslaagd

Vermelding van geslaagde of mislukte aanroep.

## <a name="custom-properties"></a>Aangepaste eigenschappen

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Aangepaste metingen

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]


## <a name="next-steps"></a>Volgende stappen

- Instellen voor bijhouden van afhankelijkheid [.NET](app-insights-asp-net-dependencies.md).
- Instellen voor bijhouden van afhankelijkheid [Java](app-insights-java-agent.md).
- [Schrijven van aangepaste afhankelijkheidstelemetrie](app-insights-api-custom-events-metrics.md#trackdependency)
- Zie [gegevensmodel](application-insights-data-model.md) voor Application Insights-typen en -gegevensmodel.
- Bekijk [platforms](app-insights-platforms.md) ondersteund door Application Insights.
