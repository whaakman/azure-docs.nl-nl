---
title: Azure Application Insights telemetrie Data Model - metrische telemetrie | Microsoft Docs
description: Application Insights-gegevensmodel voor metrische telemetrie
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
ms.openlocfilehash: f624e65508c33fa08bf1381b434847cedcf0df00
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2018
ms.locfileid: "52721099"
---
# <a name="metric-telemetry-application-insights-data-model"></a>Metrische telemetrie: Application Insights-gegevensmodel

Er zijn twee soorten metrische telemetrie wordt ondersteund door [Application Insights](app-insights-overview.md): één meting en vooraf samengevoegde metrische gegevens. Één meting is alleen een naam en waarde. Vooraf samengevoegde metrische gegevens geeft de minimale en maximale waarde van de metrische gegevens in de aggregatie-interval en de standaarddeviatie van deze.

Vooraf samengevoegde metrische telemetrie wordt ervan uitgegaan dat aggregatie-periode is één minuut.

Er zijn enkele bekende metrische namen die door Application Insights worden ondersteund. Deze metrische gegevens in de tabel performanceCounters geplaatst.

Metrische gegevens voor tellers van systeem en proces:

| **.NET-naam**             | **Naam van de Platform-agnostische** | **De naam van de REST-API** | **Beschrijving**
| ------------------------- | -------------------------- | ----------------- | ---------------- 
| `\Processor(_Total)\% Processor Time` | Werk in uitvoering... | [processorCpuPercentage](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessorCpuPercentage) | totale aantal CPU
| `\Memory\Available Bytes`                 | Werk in uitvoering... | [memoryAvailableBytes](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FmemoryAvailableBytes) | Geeft de hoeveelheid fysiek geheugen in bytes, beschikbaar voor processen die op de computer worden uitgevoerd. Dit wordt berekend door de hoeveelheid ruimte op de lijsten zeroed, gratis en stand-by-geheugen op te tellen. Beschikbaar geheugen is gereed voor gebruik; Zeroed geheugen bestaat uit pagina's van het geheugen opgevuld met nullen om te voorkomen dat hoger processen al gegevens die worden gebruikt door een vorige proces. stand-by-geheugen is geheugen dat is verwijderd uit de werkset van een proces (het fysieke geheugen) onderweg naar de schijf, maar is nog steeds beschikbaar om te worden ingetrokken. Zie [geheugenobject](https://msdn.microsoft.com/library/ms804008.aspx)
| `\Process(??APP_WIN32_PROC??)\% Processor Time` | Werk in uitvoering... | [processCpuPercentage](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessCpuPercentage) | CPU van het proces voor het hosten van de toepassing
| `\Process(??APP_WIN32_PROC??)\Private Bytes`      | Werk in uitvoering... | [processPrivateBytes](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessPrivateBytes) | geheugen dat wordt gebruikt door het proces voor het hosten van de toepassing
| `\Process(??APP_WIN32_PROC??)\IO Data Bytes/sec` | Werk in uitvoering... | [processIOBytesPerSecond](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessIOBytesPerSecond) | het aantal i/o-bewerkingen wordt uitgevoerd door proces die als host fungeert voor de toepassing
| `\ASP.NET Applications(??APP_W3SVC_PROC??)\Requests/Sec`             | Werk in uitvoering... | [requestsPerSecond](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FrequestsPerSecond) | aantal aanvragen dat is verwerkt door toepassing 
| `\.NET CLR Exceptions(??APP_CLR_PROC??)\# of Exceps Thrown / sec`    | Werk in uitvoering... | [exceptionsPerSecond](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FexceptionsPerSecond) | aantal uitzonderingen die door toepassing
| `\ASP.NET Applications(??APP_W3SVC_PROC??)\Request Execution Time`   | Werk in uitvoering... | [requestExecutionTime](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FrequestExecutionTime) | Gemiddeld aantal aanvragen uitvoeringstijd
| `\ASP.NET Applications(??APP_W3SVC_PROC??)\Requests In Application Queue` | Werk in uitvoering... | [requestsInQueue](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FrequestsInQueue) | aantal aanvragen dat wacht voor de verwerking in een wachtrij

## <a name="name"></a>Naam

De naam van de metrische gegevens die u graag zou willen zien in Application Insights-portal en gebruikersinterface. 

## <a name="value"></a>Waarde

Één waarde voor de meting. Som van de afzonderlijke metingen voor de aggregatie.

## <a name="count"></a>Count

Metrische gewicht van de samengevoegde metrische gegevens. Mag niet worden ingesteld voor een meting.

## <a name="min"></a>Min.

De minimumwaarde van de samengevoegde metrische gegevens. Mag niet worden ingesteld voor een meting.

## <a name="max"></a>Max.

Maximale waarde van de samengevoegde metrische gegevens. Mag niet worden ingesteld voor een meting.

## <a name="standard-deviation"></a>Standaarddeviatie

Standaarddeviatie van de samengevoegde metrische gegevens. Mag niet worden ingesteld voor een meting.

## <a name="custom-properties"></a>Aangepaste eigenschappen

Metrische waarde met de aangepaste eigenschap `CustomPerfCounter` ingesteld op `true` aangeven dat de metrisch gegeven staat voor de windows-prestatiemeteritem. Deze metrische gegevens in de tabel performanceCounters geplaatst. Niet in customMetrics. Ook wordt de naam van deze metriek geparseerd om uit te pakken categorie, teller en instantienamen.

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het gebruik van [Application Insights-API voor aangepaste gebeurtenissen en metrische gegevens](app-insights-api-custom-events-metrics.md#trackmetric).
- Zie [gegevensmodel](application-insights-data-model.md) voor Application Insights-typen en -gegevensmodel.
- Bekijk [platforms](app-insights-platforms.md) ondersteund door Application Insights.
