---
title: Snapshot Debugger voor .NET-apps in Azure Service Fabric-Service in de Cloud en virtuele Machines inschakelen | Microsoft Docs
description: Snapshot Debugger voor .NET-apps in Azure Service Fabric-Service in de Cloud en virtuele Machines inschakelen
services: application-insights
documentationcenter: ''
author: brahmnes
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: mbullwin
ms.date: 03/07/2019
ms.author: brahmnes
ms.openlocfilehash: 4041bee71a41cee06243d53de128bcceecda5618
ms.sourcegitcommit: 4133f375862fdbdec07b70de047d70c66ac29d50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/15/2019
ms.locfileid: "58001823"
---
# <a name="enable-snapshot-debugger-for-net-apps-in-azure-service-fabric-cloud-service-and-virtual-machines"></a>Snapshot Debugger voor .NET-apps in Azure Service Fabric-Service in de Cloud en virtuele Machines inschakelen

Als uw ASP.NET- of ASP.NET core-toepassing wordt uitgevoerd in Azure App Service, kunnen ook de onderstaande instructies worden gebruikt. Als uw toepassing een aangepaste Snapshot Debugger-configuratie vereist, het raadzaam om te [Snapshot Debugger inschakelen via de Application Insights-portalpagina](snapshot-debugger-appservice.md?toc=/azure/azure-monitor/toc.json). Als uw toepassing wordt uitgevoerd in Azure Service Fabric, Cloudservice, virtuele Machines of on-premises machines, moeten de volgende instructies worden gebruikt. 
    
## <a name="configure-snapshot-collection-for-aspnet-applications"></a>Momentopname verzamelen voor ASP.NET-toepassingen configureren

1. [Application Insights inschakelen in uw web-app](../../azure-monitor/app/asp-net.md), als u dit nog niet hebt gedaan.

2. Bevatten de [Microsoft.ApplicationInsights.SnapshotCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet-pakket in uw app.

3. Indien nodig, de configuratie van de Snapshot Debugger toegevoegd aan aangepast [ApplicationInsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md). De standaardconfiguratie voor Snapshot Debugger grotendeels leeg is en alle instellingen zijn optioneel. Hier volgt een voorbeeld van een configuratie gelijk is aan de standaard-configuratie:

    ```xml
    <TelemetryProcessors>
        <Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">
        <!-- The default is true, but you can disable Snapshot Debugging by setting it to false -->
        <IsEnabled>true</IsEnabled>
        <!-- Snapshot Debugging is usually disabled in developer mode, but you can enable it by setting this to true. -->
        <!-- DeveloperMode is a property on the active TelemetryChannel. -->
        <IsEnabledInDeveloperMode>false</IsEnabledInDeveloperMode>
        <!-- How many times we need to see an exception before we ask for snapshots. -->
        <ThresholdForSnapshotting>1</ThresholdForSnapshotting>
        <!-- The maximum number of examples we create for a single problem. -->
        <MaximumSnapshotsRequired>3</MaximumSnapshotsRequired>
        <!-- The maximum number of problems that we can be tracking at any time. -->
        <MaximumCollectionPlanSize>50</MaximumCollectionPlanSize>
        <!-- How often we reconnect to the stamp. The default value is 15 minutes.-->
        <ReconnectInterval>00:15:00</ReconnectInterval>
        <!-- How often to reset problem counters. -->
        <ProblemCounterResetInterval>1.00:00:00</ProblemCounterResetInterval>
        <!-- The maximum number of snapshots allowed in ten minutes.The default value is 1. -->
        <SnapshotsPerTenMinutesLimit>3</SnapshotsPerTenMinutesLimit>
        <!-- The maximum number of snapshots allowed per day. -->
        <SnapshotsPerDayLimit>30</SnapshotsPerDayLimit>
        <!-- Whether or not to collect snapshot in low IO priority thread. The default value is true. -->
        <SnapshotInLowPriorityThread>true</SnapshotInLowPriorityThread>
        <!-- Agree to send anonymous data to Microsoft to make this product better. -->
        <ProvideAnonymousTelemetry>true</ProvideAnonymousTelemetry>
        <!-- The limit on the number of failed requests to request snapshots before the telemetry processor is disabled. -->
        <FailedRequestLimit>3</FailedRequestLimit>
        </Add>
    </TelemetryProcessors>
    ```

4. Momentopnamen worden verzameld alleen op uitzonderingen die worden gerapporteerd aan Application Insights. In sommige gevallen (bijvoorbeeld oudere versies van het .NET-platform), moet u wellicht te [uitzondering verzamelen configureren](../../azure-monitor/app/asp-net-exceptions.md#exceptions) om te zien welke uitzonderingen met momentopnamen in de portal.


## <a name="configure-snapshot-collection-for-aspnet-core-20-applications"></a>Momentopname verzamelen voor ASP.NET Core 2.0-toepassingen configureren

1. [Application Insights inschakelen in uw ASP.NET Core web-app](../../azure-monitor/app/asp-net-core.md), als u dit nog niet hebt gedaan.

    > [!NOTE]
    > Ervoor dat uw toepassing verwijst naar versie 2.1.1 of hoger, van het pakket Microsoft.ApplicationInsights.AspNetCore zijn.

2. Bevatten de [Microsoft.ApplicationInsights.SnapshotCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet-pakket in uw app.

3. Wijzigen van uw toepassing `Startup` klasse die u wilt toevoegen en configureren van de Snapshot Collector telemetrie processor.

    Voeg de volgende using-instructies toe aan `Startup.cs`

   ```csharp
   using Microsoft.ApplicationInsights.SnapshotCollector;
   using Microsoft.Extensions.Options;
   using Microsoft.ApplicationInsights.AspNetCore;
   using Microsoft.ApplicationInsights.Extensibility;
   ```

   Voeg de volgende `SnapshotCollectorTelemetryProcessorFactory` klasse aan `Startup` klasse.

   ```csharp
   class Startup
   {
       private class SnapshotCollectorTelemetryProcessorFactory : ITelemetryProcessorFactory
       {
           private readonly IServiceProvider _serviceProvider;

           public SnapshotCollectorTelemetryProcessorFactory(IServiceProvider serviceProvider) =>
               _serviceProvider = serviceProvider;

           public ITelemetryProcessor Create(ITelemetryProcessor next)
           {
               var snapshotConfigurationOptions = _serviceProvider.GetService<IOptions<SnapshotCollectorConfiguration>>();
               return new SnapshotCollectorTelemetryProcessor(next, configuration: snapshotConfigurationOptions.Value);
           }
       }
       ...
    ```
    Voeg de `SnapshotCollectorConfiguration` en `SnapshotCollectorTelemetryProcessorFactory` services aan de pijplijn starten:

    ```csharp
       // This method gets called by the runtime. Use this method to add services to the container.
       public void ConfigureServices(IServiceCollection services)
       {
           // Configure SnapshotCollector from application settings
           services.Configure<SnapshotCollectorConfiguration>(Configuration.GetSection(nameof(SnapshotCollectorConfiguration)));

           // Add SnapshotCollector telemetry processor.
           services.AddSingleton<ITelemetryProcessorFactory>(sp => new SnapshotCollectorTelemetryProcessorFactory(sp));

           // TODO: Add other services your application needs here.
       }
   }
   ```

4. Indien nodig, moet u de configuratie van de Snapshot Debugger aangepast door het toevoegen van een sectie SnapshotCollectorConfiguration aan appsettings.json. Alle instellingen in de configuratie van de Snapshot Debugger zijn optioneel. Hier volgt een voorbeeld van een configuratie gelijk is aan de standaard-configuratie:

   ```json
   {
     "SnapshotCollectorConfiguration": {
       "IsEnabledInDeveloperMode": false,
       "ThresholdForSnapshotting": 1,
       "MaximumSnapshotsRequired": 3,
       "MaximumCollectionPlanSize": 50,
       "ReconnectInterval": "00:15:00",
       "ProblemCounterResetInterval":"1.00:00:00",
       "SnapshotsPerTenMinutesLimit": 1,
       "SnapshotsPerDayLimit": 30,
       "SnapshotInLowPriorityThread": true,
       "ProvideAnonymousTelemetry": true,
       "FailedRequestLimit": 3
     }
   }
   ```

## <a name="configure-snapshot-collection-for-other-net-applications"></a>Momentopname verzamelen voor andere .NET-toepassingen configureren

1. Als uw toepassing is niet al zijn geïnstrumenteerd met Application Insights, aan de slag met [Application Insights inschakelen en het instellen van de instrumentatiesleutel](../../azure-monitor/app/windows-desktop.md).

2. Voeg de [Microsoft.ApplicationInsights.SnapshotCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet-pakket in uw app.

3. Momentopnamen worden verzameld alleen op uitzonderingen die worden gerapporteerd aan Application Insights. Mogelijk moet u uw code meldt deze wijzigen. De code van afhandelingsservice voor uitzondering, is afhankelijk van de structuur van uw toepassing, maar een voorbeeld is lager dan:
    ```csharp
   TelemetryClient _telemetryClient = new TelemetryClient();

   void ExampleRequest()
   {
        try
        {
            // TODO: Handle the request.
        }
        catch (Exception ex)
        {
            // Report the exception to Application Insights.
            _telemetryClient.TrackException(ex);

            // TODO: Rethrow the exception if desired.
        }
   }
    ```

## <a name="next-steps"></a>Volgende stappen

- Genereert verkeer naar uw toepassing die een uitzondering kunt activeren. Wacht 10 tot 15 minuten voor momentopnamen worden verzonden naar de Application Insights-exemplaar.
- Zie [momentopnamen](snapshot-debugger.md?toc=/azure/azure-monitor/toc.json) in Azure portal.
- Zie voor hulp bij het oplossen van problemen van Profiler, [Snapshot Debugger probleemoplossing](snapshot-debugger-troubleshoot.md?toc=/azure/azure-monitor/toc.json).
