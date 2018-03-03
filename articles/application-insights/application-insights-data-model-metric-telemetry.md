---
title: Azure Application Insights telemetrie gegevensmodel - metrische telemetrie | Microsoft Docs
description: Application Insights-gegevensmodel voor metrische telemetrie
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
ms.author: mbullwin
ms.openlocfilehash: 4139e3675e2202cc42b6b8d7ff7562e9c9d693bb
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2018
---
# <a name="metric-telemetry-application-insights-data-model"></a>Metrische telemetrie: Application Insights-gegevensmodel

Er zijn twee soorten metrische telemetrie ondersteund door [Application Insights](app-insights-overview.md): enkele meting en vooraf samengevoegde metrische gegevens. Meting is alleen een naam en waarde. Vooraf samengevoegde metrische gegevens geeft de minimale en maximale waarde van de metrische gegevens in de aggregatie-interval en de standaarddeviatie van deze.

Vooraf samengevoegde metrische telemetrie wordt ervan uitgegaan dat aggregatie-periode is 1 minuut.

Er zijn verschillende bekende metrische namen ondersteund door de Application Insights. Deze metrische gegevens in de tabel performanceCounters geplaatst.

Metrische gegevens voor systeem- en prestatiemeteritems:

| **.NET-naam**             | **Platform agnostisch naam** | **De naam van de REST-API** | **Beschrijving**
| ------------------------- | -------------------------- | ----------------- | ---------------- 
| `\Processor(_Total)\% Processor Time` | Werk in uitvoering... | [processorCpuPercentage](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessorCpuPercentage) | totale aantal CPU
| `\Memory\Available Bytes`                 | Werk in uitvoering... | [memoryAvailableBytes](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FmemoryAvailableBytes) | Geeft de hoeveelheid fysiek geheugen in bytes, beschikbaar is voor processen die worden uitgevoerd op de computer. Dit wordt berekend door de hoeveelheid schijfruimte op de lijsten zeroed, gratis en stand-by-geheugen op te tellen. Beschikbaar geheugen is gereed voor gebruik; Zeroed geheugen bestaat uit de pagina's van geheugen gevuld met nullen om te voorkomen dat latere processen gegevens die worden gebruikt door een eerder proces. stand-by-geheugen is geheugen dat is verwijderd uit de werkset van een proces (het fysieke geheugen) onderweg naar schijf, maar is nog steeds beschikbaar te worden teruggehaald. Zie [geheugenobject](https://msdn.microsoft.com/library/ms804008.aspx)
| `\Process(??APP_WIN32_PROC??)\% Processor Time` | Werk in uitvoering... | [processCpuPercentage](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessCpuPercentage) | CPU van het proces voor het hosten van de toepassing
| `\Process(??APP_WIN32_PROC??)\Private Bytes`      | Werk in uitvoering... | [processPrivateBytes](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessPrivateBytes) | geheugen dat wordt gebruikt door het proces voor het hosten van de toepassing
| `\Process(??APP_WIN32_PROC??)\IO Data Bytes/sec` | Werk in uitvoering... | [processIOBytesPerSecond](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessIOBytesPerSecond) | het aantal i/o-bewerkingen wordt uitgevoerd door het proces voor het hosten van de toepassing
| `\ASP.NET Applications(??APP_W3SVC_PROC??)\Requests/Sec`             | Werk in uitvoering... | [requestsPerSecond](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FrequestsPerSecond) | aantal aanvragen dat is verwerkt door toepassing 
| `\.NET CLR Exceptions(??APP_CLR_PROC??)\# of Exceps Thrown / sec`    | Werk in uitvoering... | [exceptionsPerSecond](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FexceptionsPerSecond) | aantal uitzonderingen worden veroorzaakt door toepassing
| `\ASP.NET Applications(??APP_W3SVC_PROC??)\Request Execution Time`   | Werk in uitvoering... | [requestExecutionTime](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FrequestExecutionTime) | Gemiddeld aantal aanvragen uitvoeringstijd
| `\ASP.NET Applications(??APP_W3SVC_PROC??)\Requests In Application Queue` | Werk in uitvoering... | [requestsInQueue](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FrequestsInQueue) | het aantal aanvragen dat wacht op de verwerking in een wachtrij

## <a name="name"></a>Naam

De naam van de metrische gegevens die u zou willen zien in Application Insights-portal en de gebruikersinterface. 

## <a name="value"></a>Waarde

Enkelvoudige waarde voor de meting. De som van de afzonderlijke metingen voor de aggregatie.

## <a name="count"></a>Count

Metrische gewicht van de cumulatieve metrische gegevens. Mag niet worden ingesteld voor een meting.

## <a name="min"></a>Min.

De minimumwaarde van de cumulatieve metrische gegevens. Mag niet worden ingesteld voor een meting.

## <a name="max"></a>Max.

De maximumwaarde van de cumulatieve metrische gegevens. Mag niet worden ingesteld voor een meting.

## <a name="standard-deviation"></a>Standaarddeviatie

De standaarddeviatie van de cumulatieve metrische gegevens. Mag niet worden ingesteld voor een meting.

## <a name="custom-properties"></a>Aangepaste eigenschappen

Metrische waarde met de aangepaste eigenschap `CustomPerfCounter` ingesteld op `true` aangeven dat de metriek het windows-prestatiemeteritem vertegenwoordigt. Deze metrische gegevens in de tabel performanceCounters geplaatst. Niet in customMetrics. Ook wordt de naam van deze metriek geparseerd om uit te pakken categorie, prestatiemeteritems en instantienamen.

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="next-steps"></a>Volgende stappen

- Informatie over het gebruik [Application Insights-API voor aangepaste gebeurtenissen en metrische gegevens](app-insights-api-custom-events-metrics.md#trackmetric).
- Zie [gegevensmodel](application-insights-data-model.md) voor Application Insights-typen en data model.
- Bekijk [platforms](app-insights-platforms.md) ondersteund door de Application Insights.
