---
title: Verwijzing in ApplicationInsights.config - Azure | Microsoft Docs
description: In- of uitschakelen van gegevens verzameling modules en prestatiemeteritems en andere parameters toevoegen.
services: application-insights
documentationcenter: 
author: OlegAnaniev-MSFT
editor: mrbullwinkle
manager: carmonm
ms.assetid: 6e397752-c086-46e9-8648-a1196e8078c2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/03/2017
ms.author: mbullwin
ms.openlocfilehash: 980b297db87c2829f3c393ae867780f263f8d87c
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="configuring-the-application-insights-sdk-with-applicationinsightsconfig-or-xml"></a>De Application Insights-SDK configureren met ApplicationInsights.config of ApplicationInsights.xml
De Application Insights .NET SDK bestaat uit een aantal NuGet-pakketten. De [core-pakket](http://www.nuget.org/packages/Microsoft.ApplicationInsights) biedt u de API voor het verzenden van telemetrie naar de Application Insights. [Extra pakketten](http://www.nuget.org/packages?q=Microsoft.ApplicationInsights) bieden telemetrie *modules* en *initializers* voor het automatisch bijhouden van telemetrie van uw toepassing en de context. Door het aanpassen van het configuratiebestand, kunt u inschakelen of uitschakelen van telemetrie-modules en initializers en parameters voor enkele ervan instellen.

Het configuratiebestand heet `ApplicationInsights.config` of `ApplicationInsights.xml`, afhankelijk van het type van uw toepassing. Wordt deze automatisch toegevoegd aan uw project wanneer u [in de meeste versies van de SDK installeren][start]. Dit wordt ook toegevoegd aan een web-app door [Status Monitor op een IIS-server][redfield], of wanneer u de Application Insights selecteren [-uitbreiding voor een Azure-website of virtuele machine](app-insights-azure-web-apps.md).

Er is geen equivalent bestand om te bepalen de [SDK in een webpagina][client].

Dit document beschrijft de secties die u in de configuratie ziet-bestand, hoe ze beheren die de onderdelen van de SDK en welke NuGet-pakketten laden van deze onderdelen.

## <a name="telemetry-modules-aspnet"></a>Telemetrie-Modules (ASP.NET)
Elke telemetrie-module een specifiek type gegevens verzamelt en de belangrijkste API gebruikt om de gegevens te verzenden. De modules worden geïnstalleerd door verschillende NuGet-pakketten, die ook de vereiste regels aan het .config-bestand toevoegen.

Er is een knooppunt in het configuratiebestand voor elke module. Verwijderen van het knooppunt als wilt uitschakelen in een module, of opmerking het uit.

### <a name="dependency-tracking"></a>Bijhouden van afhankelijkheid
[Bijhouden van afhankelijkheid](app-insights-asp-net-dependencies.md) verzamelt telemetrie over uw app voor de databases en externe services en databases maakt aanroepen. Als u wilt toestaan dat deze module werken in een IIS-server, moet u [Status Monitor installeren][redfield]. Om deze te gebruiken in Azure-web-apps of virtuele machines, [selecteert u de extensie Application Insights](app-insights-azure-web-apps.md).

U kunt ook uw eigen afhankelijkheid bijhouden met behulp van code schrijven de [TrackDependency API](app-insights-api-custom-events-metrics.md#trackdependency).

* `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.DependencyCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) NuGet-pakket.

### <a name="performance-collector"></a>Collector voor prestaties
[Verzamelt systeemprestatiemeteritems](app-insights-performance-counters.md) zoals CPU, geheugen en het netwerk laden van IIS-installaties. U kunt opgeven welke items u wilt verzamelen, met inbegrip van prestatiemeteritems die u zelf hebt ingesteld.

* `Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule`
* [Microsoft.ApplicationInsights.PerfCounterCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector) NuGet-pakket.

### <a name="application-insights-diagnostics-telemetry"></a>Diagnostische telemetrie van Application Insights
De `DiagnosticsTelemetryModule` fouten in de Application Insights instrumentation code zelf worden gemeld. Bijvoorbeeld, als de code heeft geen toegang prestatiemeteritems tot of als een `ITelemetryInitializer` er een uitzondering gegenereerd. Tracetelemetrie bijgehouden door deze module wordt weergegeven in de [diagnostische gegevens doorzoeken][diagnostic]. Diagnostische gegevens verzendt naar dc.services.vsallin.net.

* `Microsoft.ApplicationInsights.Extensibility.Implementation.Tracing.DiagnosticsTelemetryModule`
* [Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet-pakket. Als u alleen dit pakket installeert, wordt het bestand ApplicationInsights.config is niet automatisch gemaakt.

### <a name="developer-mode"></a>Modus voor ontwikkelaars
`DeveloperModeWithDebuggerAttachedTelemetryModule`zorgt ervoor dat de Application Insights `TelemetryChannel` gegevens direct één telemetrie-item op een tijdstip, wanneer een foutopsporingsprogramma is gekoppeld aan het toepassingsproces verzenden. Dit reduceert de hoeveelheid tijd tussen het moment dat wanneer uw toepassing worden bijgehouden Telemetrie en wanneer deze wordt weergegeven op de Application Insights-portal. Dit zorgt ervoor dat belangrijke overhead in CPU en netwerkbandbreedte.

* `Microsoft.ApplicationInsights.WindowsServer.DeveloperModeWithDebuggerAttachedTelemetryModule`
* [Application Insights Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet-pakket

### <a name="web-request-tracking"></a>Webaanvraag bijhouden
Rapporten de [tijd en resultaat antwoordcode](app-insights-asp-net.md) van HTTP-aanvragen.

* `Microsoft.ApplicationInsights.Web.RequestTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet-pakket

### <a name="exception-tracking"></a>Uitzonderingen bijhouden
`ExceptionTrackingTelemetryModule`houdt de niet-verwerkte uitzonderingen in uw web-app. Zie [fouten en uitzonderingen][exceptions].

* `Microsoft.ApplicationInsights.Web.ExceptionTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet-pakket
* `Microsoft.ApplicationInsights.WindowsServer.UnobservedExceptionTelemetryModule`-nummers [taak uitzonderingen onopgemerkt](http://blogs.msdn.com/b/pfxteam/archive/2011/09/28/task-exception-handling-in-net-4-5.aspx).
* `Microsoft.ApplicationInsights.WindowsServer.UnhandledExceptionTelemetryModule`-niet-verwerkte uitzonderingen voor werkrollen, windows-services en consoletoepassingen wordt bijgehouden.
* [Application Insights Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet-pakket.

### <a name="eventsource-tracking"></a>EventSource bijhouden
`EventSourceTelemetryModule`Hiermee kunt u gebeurtenissen worden verzonden naar Application Insights als traceringen EventSource configureren. Zie voor meer informatie over het bijhouden van EventSource gebeurtenissen [EventSource gebeurtenissen](app-insights-asp-net-trace-logs.md#using-eventsource-events).

* `Microsoft.ApplicationInsights.EventSourceListener.EventSourceTelemetryModule`
* [Microsoft.ApplicationInsights.EventSourceListener](http://www.nuget.org/packages/Microsoft.ApplicationInsights.EventSourceListener) 

### <a name="etw-event-tracking"></a>ETW-gebeurtenissen bijhouden
`EtwCollectorTelemetryModule`Hiermee kunt u gebeurtenissen van ETW-providers worden verzonden naar Application Insights als traceringen configureren. Zie voor meer informatie over het bijhouden van ETW-gebeurtenissen [ETW-gebeurtenissen met behulp van](app-insights-asp-net-trace-logs.md#using-etw-events).

* `Microsoft.ApplicationInsights.EtwCollector.EtwCollectorTelemetryModule`
* [Microsoft.ApplicationInsights.EtwCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.EtwCollector) 

### <a name="microsoftapplicationinsights"></a>Microsoft.ApplicationInsights
Het pakket Microsoft.ApplicationInsights biedt de [API core](https://msdn.microsoft.com/library/mt420197.aspx) van de SDK. Gebruik deze optie de telemetrie-modules en u kunt ook [gebruiken voor het definiëren van uw eigen telemetrie](app-insights-api-custom-events-metrics.md).

* Er is geen vermelding in ApplicationInsights.config.
* [Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet-pakket. Als u alleen deze NuGet installeert, wordt geen .config-bestand gegenereerd.

## <a name="telemetry-channel"></a>Telemetrie-kanaal
Het kanaal telemetrie beheert buffer en verzenden van telemetrie naar de Application Insights-service.

* `Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel`is de standaardkanaal voor services. Deze buffert gegevens in het geheugen.
* `Microsoft.ApplicationInsights.PersistenceChannel`vormt een alternatief voor consoletoepassingen. Deze kunt unflushed gegevens opslaan in de permanente opslag wanneer uw app wordt afgesloten en wordt verzonden wanneer de app opnieuw wordt gestart.

## <a name="telemetry-initializers-aspnet"></a>Telemetrie initalisatiefuncties (ASP.NET)
Telemetrie initalisatiefuncties contexteigenschappen die worden verzonden, samen met elk telemetrie-item worden ingesteld.

U kunt [schrijven van uw eigen initalisatiefuncties](app-insights-api-filtering-sampling.md#add-properties) contexteigenschappen instellen.

De standaard initalisatiefuncties zijn klaar voor gebruik door de Web- of WindowsServer NuGet-pakketten:

* `AccountIdTelemetryInitializer`de eigenschap AccountId ingesteld.
* `AuthenticatedUserIdTelemetryInitializer`de eigenschap AuthenticatedUserId ingesteld zoals ingesteld door de JavaScript SDK.
* `AzureRoleEnvironmentTelemetryInitializer`updates de `RoleName` en `RoleInstance` eigenschappen van de `Device` context voor alle telemetrie-items met gegevens uit de Azure-runtime-omgeving.
* `BuildInfoConfigComponentVersionTelemetryInitializer`updates de `Version` eigenschap van de `Component` context voor alle telemetrie-items met de waarde die is opgehaald uit de `BuildInfo.config` bestand door MS Build gemaakt.
* `ClientIpHeaderTelemetryInitializer`updates `Ip` eigenschap van de `Location` context van alle telemetrie-items op basis van de `X-Forwarded-For` HTTP-header van de aanvraag.
* `DeviceTelemetryInitializer`de volgende eigenschappen van de `Device` context voor alle telemetrie-items.
  * `Type`is ingesteld op 'PC'
  * `Id`is ingesteld op de domeinnaam van de computer waarop de webtoepassing wordt uitgevoerd.
  * `OemName`is ingesteld op de waarde die is opgehaald uit de `Win32_ComputerSystem.Manufacturer` veld met WMI.
  * `Model`is ingesteld op de waarde die is opgehaald uit de `Win32_ComputerSystem.Model` veld met WMI.
  * `NetworkType`is ingesteld op de waarde die is opgehaald uit de `NetworkInterface`.
  * `Language`is ingesteld op de naam van de `CurrentCulture`.
* `DomainNameRoleInstanceTelemetryInitializer`updates de `RoleInstance` eigenschap van de `Device` context voor alle telemetrie-items met de domeinnaam van de computer waarop de webtoepassing wordt uitgevoerd.
* `OperationNameTelemetryInitializer`updates de `Name` eigenschap van de `RequestTelemetry` en de `Name` eigenschap van de `Operation` context van alle telemetrie-items op basis van de HTTP-methode, evenals de namen van ASP.NET MVC-controller en de actie die wordt aangeroepen om de aanvraag te verwerken.
* `OperationIdTelemetryInitializer`of `OperationCorrelationTelemetryInitializer` updates de `Operation.Id` contexteigenschap van alle telemetrie-items bijgehouden tijdens de verwerking van een aanvraag met de automatisch gegenereerde `RequestTelemetry.Id`.
* `SessionTelemetryInitializer`updates de `Id` eigenschap van de `Session` context voor alle telemetrie-items met de waarde is opgehaald uit de `ai_session` cookie gegenereerd door de code die ApplicationInsights JavaScript instrumentation wordt uitgevoerd in de browser van de gebruiker.
* `SyntheticTelemetryInitializer`of `SyntheticUserAgentTelemetryInitializer` updates de `User`, `Session` en `Operation` contexten eigenschappen van alle telemetrie-items bijgehouden bij het verwerken van een aanvraag van een synthetische bron, zoals een beschikbaarheid testen of engine bot zoeken. Standaard [Metrics Explorer](app-insights-metrics-explorer.md) synthetische telemetrie niet wordt weergegeven.

    De `<Filters>` instellen van eigenschappen van de aanvragen te identificeren.
* `UserTelemetryInitializer`updates de `Id` en `AcquisitionDate` eigenschappen van `User` context voor alle telemetrie-items met waarden die worden opgehaald uit de `ai_user` cookie gegenereerd door de Application Insights JavaScript instrumentation code die wordt uitgevoerd in de browser van de gebruiker.
* `WebTestTelemetryInitializer`Hiermee stelt u de gebruikers-id, de sessie-id en de eigenschappen van synthetische gegevensbron voor HTTP-die afkomstig zijn van van aanvragen [beschikbaarheidstests](app-insights-monitor-web-app-availability.md).
  De `<Filters>` instellen van eigenschappen van de aanvragen te identificeren.

U kunt opnemen voor .NET-toepassingen die zijn uitgevoerd in de Service Fabric de `Microsoft.ApplicationInsights.ServiceFabric` NuGet-pakket. Dit pakket bevat een `FabricTelemetryInitializer`, die Service Fabric-eigenschappen toevoegt aan de telemetrie-items. Zie voor meer informatie de [GitHub-pagina](https://go.microsoft.com/fwlink/?linkid=848457) over de eigenschappen die zijn toegevoegd door dit pakket NuGet.

## <a name="telemetry-processors-aspnet"></a>Telemetrie-Processors (ASP.NET)
Telemetrie-processors kunnen filteren en wijzigen van elk telemetrie-item vlak voordat deze uit de SDK wordt verzonden naar de portal.

U kunt [de processors van uw eigen telemetrie schrijven](app-insights-api-filtering-sampling.md#filtering).

#### <a name="adaptive-sampling-telemetry-processor-from-200-beta3"></a>Adaptieve steekproeven telemetrie processor (van 2.0.0-beta3)
Deze optie is standaard ingeschakeld. Als uw app veel telemetrie verzendt, worden deze processor enkele ervan.

```xml

    <TelemetryProcessors>
      <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
        <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
      </Add>
    </TelemetryProcessors>

```

De parameter bevat het doel dat het algoritme probeert te bereiken. Elk exemplaar van de SDK werkt onafhankelijk, dus als uw server een cluster met meerdere machines, het daadwerkelijk volume dat telemetrie dienovereenkomstig wordt vermenigvuldigd.

[Meer informatie over steekproeven](app-insights-sampling.md).

#### <a name="fixed-rate-sampling-telemetry-processor-from-200-beta1"></a>Vast aantal steekproeven telemetrie processor (van 2.0.0-beta1)
Er is ook een standaard [steekproef nemen telemetrie processor](app-insights-api-filtering-sampling.md) (van 2.0.1):

```XML

    <TelemetryProcessors>
     <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.SamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">

     <!-- Set a percentage close to 100/N where N is an integer. -->
     <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
     <SamplingPercentage>10</SamplingPercentage>
     </Add>
   </TelemetryProcessors>

```



## <a name="channel-parameters-java"></a>Kanaalparameters (Java)
Deze parameters van invloed op hoe de Java SDK moet opslaan en leegmaken van de telemetriegegevens die worden verzameld.

#### <a name="maxtelemetrybuffercapacity"></a>MaxTelemetryBufferCapacity
Het aantal telemetrie-items die kunnen worden opgeslagen in de SDK geheugenopslag. Wanneer dit aantal is bereikt, wordt de telemetrie-buffer is leeggemaakt - dat wil zeggen, de telemetrie-items worden verzonden naar de Application Insights-server.

* Min: 1
* Max: 1000
* Standaard: 500

```

  <ApplicationInsights>
      ...
      <Channel>
       <MaxTelemetryBufferCapacity>100</MaxTelemetryBufferCapacity>
      </Channel>
      ...
  </ApplicationInsights>
```

#### <a name="flushintervalinseconds"></a>FlushIntervalInSeconds
Hiermee wordt bepaald hoe vaak de gegevens die zijn opgeslagen in de opslag in het geheugen moet worden leeggemaakt (verzonden naar Application Insights).

* Min: 1
* Max: 300
* Standaard: 5

```

    <ApplicationInsights>
      ...
      <Channel>
        <FlushIntervalInSeconds>100</FlushIntervalInSeconds>
      </Channel>
      ...
    </ApplicationInsights>
```

#### <a name="maxtransmissionstoragecapacityinmb"></a>MaxTransmissionStorageCapacityInMB
Bepaalt de maximale grootte in MB die is toegewezen aan de permanente opslag op de lokale schijf. Deze opslag wordt gebruikt voor persistente telemetrie-items die niet konden worden verzonden naar de Application Insights-eindpunt. Wanneer de grootte van de opslagruimte is voldaan, wordt de nieuwe telemetrie-items worden genegeerd.

* Min: 1
* Max: 100
* Standaard: 10

```

   <ApplicationInsights>
      ...
      <Channel>
        <MaxTransmissionStorageCapacityInMB>50</MaxTransmissionStorageCapacityInMB>
      </Channel>
      ...
   </ApplicationInsights>
```



## <a name="instrumentationkey"></a>InstrumentationKey
Hiermee bepaalt u de Application Insights-resource waarin uw gegevens worden weergegeven. Doorgaans maakt u een afzonderlijke resource met een afzonderlijke sleutel voor elk van uw toepassingen.

Als u instellen van de sleutel dynamisch - bijvoorbeeld wilt als u resultaten wilt verzenden vanuit uw App bij verschillende bronnen - kunt u de sleutel uit het configuratiebestand weglaten en stel deze in de code in plaats daarvan.

Om in te stellen de sleutel voor alle exemplaren van TelemetryClient, stel inclusief standaard telemetrie modules, de sleutel in TelemetryConfiguration.Active. Dit doen in een initialiseringsmethode, zoals global.aspx.cs in een ASP.NET-beheerservice:

```csharp

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey =
          // - for example -
          WebConfigurationManager.Settings["ikey"];
      //...
```

Als u alleen verzenden van een specifieke reeks gebeurtenissen naar een andere resource wilt, kunt u de sleutel instellen voor een specifieke TelemetryClient:

```csharp

    var tc = new TelemetryClient();
    tc.Context.InstrumentationKey = "----- my key ----";
    tc.TrackEvent("myEvent");
    // ...

```

Ophalen van een nieuwe sleutel [Maak een nieuwe resource in de Application Insights-portal][new].

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over de API][api].

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[netlogs]: app-insights-asp-net-trace-logs.md
[new]: app-insights-create-new-resource.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md
