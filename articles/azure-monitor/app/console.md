---
title: Azure Application Insights voor consoletoepassingen | Microsoft Docs
description: Bewaak web-apps voor beschikbaarheid, prestaties en gebruik.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 3b722e47-38bd-4667-9ba4-65b7006c074c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 08/28/2018
ms.reviewer: lmolkova
ms.author: mbullwin
ms.openlocfilehash: 57c0b930982f3a8b82ef7c077bfcdae6dfa23aac
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54077110"
---
# <a name="application-insights-for-net-console-applications"></a>Application Insights voor .NET-consoletoepassingen
[Application Insights](../../application-insights/app-insights-overview.md) kunt u uw web-App voor beschikbaarheid, prestaties en gebruik te bewaken.

U moet een abonnement met [Microsoft Azure](https://azure.com). Aanmelden met een Microsoft-account, die u mogelijk voor Windows, Xbox Live of andere Microsoft-cloudservices. Uw team mogelijk een organisatie-abonnement op Azure: vraag de eigenaar u toevoegen aan met uw Microsoft-account.

## <a name="getting-started"></a>Aan de slag

* Maak in de [Azure Portal](https://portal.azure.com) [een Application Insights-resource](../../azure-monitor/app/create-new-resource.md ). Kies voor het toepassingstype, **algemene**.
* Kopieer de instrumentatiesleutel. Zoek de sleutel in de **Essentials** vervolgkeuzelijst van de nieuwe resource die u hebt gemaakt. 
* Installeer de meest recente [Microsoft.ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights) pakket.
* Stel de instrumentatiesleutel in uw code voor het bijhouden van alle telemetrie (of een set APPINSIGHTS_INSTRUMENTATIONKEY omgevingsvariabele). Hierna zou het mogelijk handmatig telemetrie bijhouden en deze wordt weergegeven op de Azure-portal

```csharp
TelemetryConfiguration.Active.InstrumentationKey = " *your key* ";
var telemetryClient = new TelemetryClient();
telemetryClient.TrackTrace("Hello World!");
```

* Installeer de meest recente versie van [Microsoft.ApplicationInsights.DependencyCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) package - automatisch worden bijgehouden HTTP-, SQL of enige andere externe afhankelijkheidsaanroepen.

U kunt initialiseren en Application Insights configureren vanuit de code of met behulp van `ApplicationInsights.config` bestand. Zorg ervoor dat de initialisatie gebeurt zo vroeg mogelijk. 

> [!NOTE]
> Instructies die verwijzen naar **ApplicationInsights.config** zijn alleen van toepassing op apps die zijn gericht op het .NET Framework en niet van toepassing op .NET Core-toepassingen.

### <a name="using-config-file"></a>Met behulp van configuratiebestand

Standaard Application Insights-SDK zoekt `ApplicationInsights.config` bestand in de werkmap wanneer `TelemetryConfiguration` wordt gemaakt

```csharp
TelemetryConfiguration config = TelemetryConfiguration.Active; // Reads ApplicationInsights.config file if present
```

U kunt ook opgeven pad naar het configuratiebestand.

```csharp
using System.IO;
TelemetryConfiguration configuration = TelemetryConfiguration.CreateFromConfiguration(File.ReadAllText("C:\\ApplicationInsights.config"));
var telemetryClient = new TelemetryClient(configuration);
```

Zie voor meer informatie, [verwijzing naar een bestand configuratie](configuration-with-applicationinsights-config.md).

Krijgt u mogelijk een compleet voorbeeld van het configuratiebestand dat door het installeren van de meest recente versie van [Microsoft.ApplicationInsights.WindowsServer](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer) pakket. Hier volgt de **minimale** configuratie voor de verzameling van afhankelijkheid die gelijk is aan het codevoorbeeld.

```XML
<?xml version="1.0" encoding="utf-8"?>
<ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings">
  <InstrumentationKey>Your Key</InstrumentationKey>
  <TelemetryInitializers>
    <Add Type="Microsoft.ApplicationInsights.DependencyCollector.HttpDependenciesParsingTelemetryInitializer, Microsoft.AI.DependencyCollector"/>
  </TelemetryInitializers>
  <TelemetryModules>
    <Add Type="Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule, Microsoft.AI.DependencyCollector">
      <ExcludeComponentCorrelationHttpHeadersOnDomains>
        <Add>core.windows.net</Add>
        <Add>core.chinacloudapi.cn</Add>
        <Add>core.cloudapi.de</Add>
        <Add>core.usgovcloudapi.net</Add>
        <Add>localhost</Add>
        <Add>127.0.0.1</Add>
      </ExcludeComponentCorrelationHttpHeadersOnDomains>
      <IncludeDiagnosticSourceActivities>
        <Add>Microsoft.Azure.ServiceBus</Add>
        <Add>Microsoft.Azure.EventHubs</Add>
      </IncludeDiagnosticSourceActivities>
    </Add>
  </TelemetryModules>
  <TelemetryChannel Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel, Microsoft.AI.ServerTelemetryChannel"/>
</ApplicationInsights>

```

### <a name="configuring-telemetry-collection-from-code"></a>Verzamelen van telemetriegegevens van code configureren

* Tijdens het opstarten van de toepassing maken en configureren van `DependencyTrackingTelemetryModule` -exemplaar, deze moet singleton zijn en moeten worden bewaard voor de levensduur van de toepassing.

```csharp
var module = new DependencyTrackingTelemetryModule();

// prevent Correlation Id to be sent to certain endpoints. You may add other domains as needed.
module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.windows.net");
//...

// enable known dependency tracking, note that in future versions, we will extend this list. 
// please check default settings in https://github.com/Microsoft/ApplicationInsights-dotnet-server/blob/develop/Src/DependencyCollector/NuGet/ApplicationInsights.config.install.xdt#L20
module.IncludeDiagnosticSourceActivities.Add("Microsoft.Azure.ServiceBus");
module.IncludeDiagnosticSourceActivities.Add("Microsoft.Azure.EventHubs");
//....

// initialize the module
module.Initialize(configuration);
```

* Algemene telemetrie initializers toevoegen

```csharp
// stamps telemetry with correlation identifiers
TelemetryConfiguration.Active.TelemetryInitializers.Add(new OperationCorrelationTelemetryInitializer());

// ensures proper DependencyTelemetry.Type is set for Azure RESTful API calls
TelemetryConfiguration.Active.TelemetryInitializers.Add(new HttpDependenciesParsingTelemetryInitializer());
```

* Voor .NET Framework-Windows-app, kunt u ook installeren en initialiseren van prestatiemeteritem collector module, zoals wordt beschreven [hier](https://apmtips.com/blog/2017/02/13/enable-application-insights-live-metrics-from-code/)

#### <a name="full-example"></a>Compleet voorbeeld

```csharp
using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.DependencyCollector;
using Microsoft.ApplicationInsights.Extensibility;
using System.Net.Http;
using System.Threading.Tasks;

namespace ConsoleApp
{
    class Program
    {
        static void Main(string[] args)
        {
            TelemetryConfiguration configuration = TelemetryConfiguration.Active;

            configuration.InstrumentationKey = "removed";
            configuration.TelemetryInitializers.Add(new OperationCorrelationTelemetryInitializer());
            configuration.TelemetryInitializers.Add(new HttpDependenciesParsingTelemetryInitializer());

            var telemetryClient = new TelemetryClient();
            using (InitializeDependencyTracking(configuration))
            {
                // run app...

                telemetryClient.TrackTrace("Hello World!");

                using (var httpClient = new HttpClient())
                {
                    // Http dependency is automatically tracked!
                    httpClient.GetAsync("https://microsoft.com").Wait();
                }

            }

            // before exit, flush the remaining data
            telemetryClient.Flush();

            // flush is not blocking so wait a bit
            Task.Delay(5000).Wait();

        }

        static DependencyTrackingTelemetryModule InitializeDependencyTracking(TelemetryConfiguration configuration)
        {
            var module = new DependencyTrackingTelemetryModule();

            // prevent Correlation Id to be sent to certain endpoints. You may add other domains as needed.
            module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.windows.net");
            module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.chinacloudapi.cn");
            module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.cloudapi.de");
            module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.usgovcloudapi.net");
            module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("localhost");
            module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("127.0.0.1");

            // enable known dependency tracking, note that in future versions, we will extend this list. 
            // please check default settings in https://github.com/Microsoft/ApplicationInsights-dotnet-server/blob/develop/Src/DependencyCollector/NuGet/ApplicationInsights.config.install.xdt#L20
            module.IncludeDiagnosticSourceActivities.Add("Microsoft.Azure.ServiceBus");
            module.IncludeDiagnosticSourceActivities.Add("Microsoft.Azure.EventHubs");

            // initialize the module
            module.Initialize(configuration);

            return module;
        }
    }
}

```

## <a name="next-steps"></a>Volgende stappen
* [Afhankelijkheden controleren](../../azure-monitor/app/asp-net-dependencies.md) om te zien als REST, SQL of andere externe resources zorgen voor vertraging.
* [Gebruik de API](../../azure-monitor/app/api-custom-events-metrics.md) voor het verzenden van uw eigen gebeurtenissen en metrische gegevens voor een gedetailleerdere weergave van de prestaties en het gebruik van uw app.
