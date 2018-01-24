---
title: Azure Application Insights voor consoletoepassingen | Microsoft Docs
description: Bewaken van webtoepassingen voor beschikbaarheid, prestaties en het gebruik.
services: application-insights
documentationcenter: .net
author: lmolkova
manager: bfung
ms.assetid: 3b722e47-38bd-4667-9ba4-65b7006c074c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 12/18/2017
ms.author: lmolkova
ms.openlocfilehash: 57f5670eec36ff2c4332da592dd2a3eef73fdefc
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="application-insights-for-net-console-applications"></a>Application Insights voor .NET-console toepassingen
[Application Insights](app-insights-overview.md) kunt u uw webtoepassing voor beschikbaarheid, prestaties en gebruik te bewaken.

U moet een abonnement met [Microsoft Azure](http://azure.com). Aanmelden met een Microsoft-account, moet u voor Windows, Xbox Live of andere Microsoft-cloudservices wellicht. Uw team wellicht een organisatie-abonnement op Azure: vraag de eigenaar u toevoegen met behulp van uw Microsoft-account.

## <a name="getting-started"></a>Aan de slag

* Maak in de [Azure Portal](https://portal.azure.com) [een Application Insights-resource](app-insights-create-new-resource.md). Kies ASP.NET-app als het toepassingstype.
* Kopieer de instrumentatiesleutel. De sleutel vinden in de Essentials vervolgkeuzelijst van de nieuwe resource die u hebt gemaakt. 
* Installeer de meest recente [Microsoft.ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights) pakket.
* Stel de instrumentatiesleutel hiertoe in uw code voordat alle telemetrie bijhouden (of set APPINSIGHTS_INSTRUMENTATIONKEY omgevingsvariabele). Daarna moet u kunnen handmatig telemetrie volgen en deze wordt weergegeven op de Azure-portal

```csharp
TelemetryConfiguration.Active.InstrumentationKey = " *your key* ";
var telemetryClient = new TelemetryClient();
telemetryClient.TrackTrace("Hello World!");
```

* Installeer de nieuwste versie van [Microsoft.ApplicationInsights.DependencyCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) package - deze automatisch worden bijgehouden HTTP, SQL of sommige andere externe afhankelijkheidsaanroepen.

U kunt initialiseren en Application Insights configureren vanuit de code of met behulp van `ApplicationInsights.config` bestand. Zorg ervoor dat de initialisatie gebeurt zo spoedig mogelijk.

### <a name="using-config-file"></a>Met behulp van configuratiebestand

Standaard Application Insights-SDK zoekt `ApplicationInsights.config` bestand in de werkmap wanneer `TelemetryConfiguration` wordt gemaakt

```csharp
TelemetryConfiguration config = TelemetryConfiguration.Active; // Read ApplicationInsights.config file if present
```

U kunt ook opgeven pad naar het configuratiebestand.

```csharp
TelemetryConfiguration configuration = TelemetryConfiguration.CreateFromConfiguration("ApplicationInsights.config");
```

Zie voor meer informatie [configuratie bestandsverwijzing](app-insights-configuration-with-applicationinsights-config.md).

Mogelijk dat u een compleet voorbeeld van het configuratiebestand door het installeren van de meest recente versie van [Microsoft.ApplicationInsights.WindowsServer](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer) pakket. Hier volgt de **minimale** configuratie voor de verzameling van afhankelijkheid die gelijk is aan het codevoorbeeld.

```XML
<?xml version="1.0" encoding="utf-8"?>
<ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings">
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

* Tijdens het opstarten van de toepassing maken en configureren `DependencyTrackingTelemetryModule` instantie - deze moet singleton zijn en moeten worden bewaard voor de levensduur van de toepassing.

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

* Algemene telemetrie initalisatiefuncties toevoegen

```csharp
// stamps telemetry with correlation identifiers
TelemetryConfiguration.Active.TelemetryInitializers.Add(new OperationCorrelationTelemetryInitializer());

// ensures proper DependencyTelemetry.Type is set for Azure RESTful API calls
TelemetryConfiguration.Active.TelemetryInitializers.Add(new HttpDependenciesParsingTelemetryInitializer());
```

* Voor .NET Framework Windows-app, kunt u ook installeren en initialiseren van prestatiemeteritem collector module, zoals wordt beschreven [hier](http://apmtips.com/blog/2017/02/13/enable-application-insights-live-metrics-from-code/)

#### <a name="full-example"></a>Compleet voorbeeld

```csharp
static void Main(string[] args)
{
    TelemetryConfiguration configuration = TelemetryConfiguration.Active;

    configuration.InstrumentationKey = "removed";
    configuration.TelemetryInitializers.Add(new OperationCorrelationTelemetryInitializer());
    configuration.TelemetryInitializers.Add(new HttpDependenciesParsingTelemetryInitializer());

    var telemetryClient = new TelemetryClient();
    using (IntitializeDependencyTracking(configuration))
    {
        telemetryClient.TrackTrace("Hello World!");

        using (var httpClient = new HttpClient())
        {
            // Http dependency is automatically tracked!
            httpClient.GetAsync("https://microsoft.com").Wait();
        }
    }

    // run app...

    // when application stops or you are done with dependency tracking, do not forget to dispose the module
    dependencyTrackingModule.Dispose();

    telemetryClient.Flush();
}

static DependencyTrackingTelemetryModule IntitializeDependencyTracking(TelemetryConfiguration configuration)
{
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
```

## <a name="next-steps"></a>Volgende stappen
* [Afhankelijkheden bewaken](app-insights-asp-net-dependencies.md) om te zien als REST, SQL of andere externe bronnen u vertragen.
* [Gebruik de API](app-insights-api-custom-events-metrics.md) voor het verzenden van uw eigen gebeurtenissen en metrische gegevens voor een meer gedetailleerde weergave van de prestaties en het gebruik van uw app.
