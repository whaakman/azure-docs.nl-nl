---
title: Snapshot Debugger voor .NET-Apps inschakelen in azure Service Fabric, Cloud service en Virtual Machines | Microsoft Docs
description: Snapshot Debugger voor .NET-apps in azure Service Fabric, Cloud service en Virtual Machines inschakelen
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
ms.author: bfung
ms.openlocfilehash: 5a6cf763ae16b55806df2acaf2e03fd8c13d1e76
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2019
ms.locfileid: "68359285"
---
# <a name="enable-snapshot-debugger-for-net-apps-in-azure-service-fabric-cloud-service-and-virtual-machines"></a>Snapshot Debugger voor .NET-apps in azure Service Fabric, Cloud service en Virtual Machines inschakelen

Als uw kern toepassing voor ASP.NET of ASP.NET wordt uitgevoerd in Azure App Service, wordt het ten zeerste aanbevolen Snapshot Debugger in te [scha kelen via de pagina Application Insights Portal](snapshot-debugger-appservice.md?toc=/azure/azure-monitor/toc.json). Als uw toepassing echter een aangepaste Snapshot Debugger configuratie of een preview-versie van .NET core vereist, moet deze instructie ***worden gevolgd naast*** de instructies voor [het inschakelen van de Application Insights Portal pagina](snapshot-debugger-appservice.md?toc=/azure/azure-monitor/toc.json).

Als uw toepassing wordt uitgevoerd in azure Service Fabric, Cloud service, Virtual Machines of on-premises machines, moet u de volgende instructies gebruiken. 
    
## <a name="configure-snapshot-collection-for-aspnet-applications"></a>Momentopname verzamelen voor ASP.NET-toepassingen configureren

1. [Application Insights inschakelen in uw web-app](../../azure-monitor/app/asp-net.md), als u dit nog niet hebt gedaan.

2. Bevatten de [Microsoft.ApplicationInsights.SnapshotCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet-pakket in uw app.

3. Aangepaste configuratie van Snapshot Debugger toegevoegd aan [ApplicationInsights. config](../../azure-monitor/app/configuration-with-applicationinsights-config.md), indien nodig. De standaard configuratie van Snapshot Debugger is grotendeels leeg en alle instellingen zijn optioneel. Hier volgt een voor beeld van een configuratie equivalent met de standaard configuratie:

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


## <a name="configure-snapshot-collection-for-applications-using-aspnet-core-20-or-above"></a>Momentopname verzameling configureren voor toepassingen die gebruikmaken van ASP.NET Core 2,0 of hoger

1. [Application Insights inschakelen in uw ASP.NET Core web-app](../../azure-monitor/app/asp-net-core.md), als u dit nog niet hebt gedaan.

    > [!NOTE]
    > Ervoor dat uw toepassing verwijst naar versie 2.1.1 of hoger, van het pakket Microsoft.ApplicationInsights.AspNetCore zijn.

2. Bevatten de [Microsoft.ApplicationInsights.SnapshotCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet-pakket in uw app.

3. Wijzigen van uw toepassing `Startup` klasse die u wilt toevoegen en configureren van de Snapshot Collector telemetrie processor.
    1. Als [micro soft. ApplicationInsights. SnapshotCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet-pakket versie 1.3.5 of hoger wordt gebruikt, voegt u de volgende using `Startup.cs`-instructies toe aan.

       ```csharp
            using Microsoft.ApplicationInsights.SnapshotCollector;
       ```

       Voeg het volgende toe aan het einde van de ConfigureServices-methode `Startup` in de `Startup.cs`-klasse in.

       ```csharp
            services.AddSnapshotCollector((configuration) => Configuration.Bind(nameof(SnapshotCollectorConfiguration), configuration));
       ```
    2. Als [micro soft. ApplicationInsights. SnapshotCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet-pakket versie 1.3.4 of hieronder wordt gebruikt, voegt u de volgende using `Startup.cs`-instructies toe aan.

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

4. Als dat nodig is, kunt u de Snapshot Debugger configuratie aanpassen door een sectie SnapshotCollectorConfiguration toe te voegen aan appSettings. json. Alle instellingen in de Snapshot Debugger configuratie zijn optioneel. Hier volgt een voor beeld van een configuratie equivalent met de standaard configuratie:

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

- Verkeer genereren voor uw toepassing die een uitzonde ring kan veroorzaken. Wacht vervolgens 10 tot 15 minuten totdat moment opnamen naar het Application Insights-exemplaar worden verzonden.
- Zie [moment opnamen](snapshot-debugger.md?toc=/azure/azure-monitor/toc.json#view-snapshots-in-the-portal) in de Azure Portal.
- Zie [Snapshot debugger Troubleshooting](snapshot-debugger-troubleshoot.md?toc=/azure/azure-monitor/toc.json)(Engelstalig) voor hulp bij het oplossen van problemen met snapshot debugger.
