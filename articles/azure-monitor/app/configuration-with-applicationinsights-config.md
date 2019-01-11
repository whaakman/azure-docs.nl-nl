---
title: ApplicationInsights.config referentie - Azure | Microsoft Docs
description: In- of uitschakelen van de standaardmodules voor het verzamelen van gegevens en prestatiemeteritems en andere parameters toevoegen.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 6e397752-c086-46e9-8648-a1196e8078c2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 09/19/2018
ms.reviewer: olegan
ms.author: mbullwin
ms.openlocfilehash: d17b1b754afc5067a885025dba83cd0fba2370d5
ms.sourcegitcommit: e7312c5653693041f3cbfda5d784f034a7a1a8f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2019
ms.locfileid: "54214569"
---
# <a name="configuring-the-application-insights-sdk-with-applicationinsightsconfig-or-xml"></a>De Application Insights-SDK configureren met ApplicationInsights.config of ApplicationInsights.xml
De Application Insights .NET SDK bestaat uit een aantal NuGet-pakketten. De [core-pakket](https://www.nuget.org/packages/Microsoft.ApplicationInsights) biedt de API voor het verzenden van telemetrie naar de Application Insights. [Extra pakketten](https://www.nuget.org/packages?q=Microsoft.ApplicationInsights) bieden telemetrie *modules* en *initializers* voor het automatisch bijhouden van telemetrie van uw toepassing en de context. Door het configuratiebestand aanpassen, kunt u inschakelen of uitschakelen telemetrie-modules en initializers, en parameters voor sommige hiervan zijn ingesteld.

Het configuratiebestand met de naam `ApplicationInsights.config` of `ApplicationInsights.xml`, afhankelijk van het type van uw toepassing. Er wordt automatisch toegevoegd aan uw project als u [in de meeste versies van de SDK installeren][start]. Dit wordt ook toegevoegd aan een web-app door [Status Monitor op een IIS-server][redfield], of wanneer u de Application Insights selecteren [-extensie voor een Azure-website of virtuele machine](azure-web-apps.md).

Er is niet een gelijkwaardige-bestand om te bepalen de [SDK in een webpagina][client].

Dit document beschrijft de secties u in de configuratie van ziet het bestand, hoe ze de onderdelen van de SDK, beheren en welke NuGet-pakketten deze onderdelen te laden.

> [!NOTE]
> ApplicationInsights.config en XML-instructies niet van toepassing op de .NET Core SDK. Voor wijzigingen in een .NET Core-toepassing gebruiken we meestal het bestand appsettings.json. Een voorbeeld hiervan vindt u de [Snapshot Debugger documentatie.](https://docs.microsoft.com/azure/application-insights/app-insights-snapshot-debugger#configure-snapshot-collection-for-aspnet-core-20-applications)

## <a name="telemetry-modules-aspnet"></a>Telemetrie-Modules (ASP.NET)
Elke module telemetrie een specifiek type gegevens verzamelt en maakt gebruik van de core API om de gegevens te verzenden. De modules worden geïnstalleerd door verschillende NuGet-pakketten, die ook de vereiste regels aan het .config-bestand toevoegen.

Er is een knooppunt in het configuratiebestand voor elke module. Als u wilt een module uitschakelen, verwijdert het knooppunt of opmerking het uit.

### <a name="dependency-tracking"></a>Bijhouden van afhankelijkheid
[Bijhouden van afhankelijkheid](../../azure-monitor/app/asp-net-dependencies.md) verzamelt telemetrie over het aanroepen van uw app voor databases en externe services en -databases maakt. Als u wilt toestaan dat deze module in een IIS-server werkt, moet u [Installeer Status Monitor][redfield]. Te gebruiken in Azure WebApps of virtuele machines, [selecteert u de Application Insights-extensie](azure-web-apps.md).

U kunt ook uw eigen afhankelijkheid bijhouden met behulp van code schrijven de [TrackDependency API](../../azure-monitor/app/api-custom-events-metrics.md#trackdependency).

* `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.DependencyCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) NuGet-pakket.

### <a name="performance-collector"></a>Prestaties verzamelen
[Systeemprestatiemeteritems verzamelt](../../azure-monitor/app/performance-counters.md) , zoals CPU, geheugen en het netwerk laden van de IIS-installaties. U kunt opgeven welke items u wilt verzamelen, met inbegrip van prestatiemeteritems die u zelf hebt gedefinieerd.

* `Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule`
* [Microsoft.ApplicationInsights.PerfCounterCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector) NuGet-pakket.

### <a name="application-insights-diagnostics-telemetry"></a>Diagnostische gegevens telemetrie van Application Insights
De `DiagnosticsTelemetryModule` fouten in de code van de Application Insights-instrumentation zelf worden gemeld. Bijvoorbeeld, als de code heeft geen toegang prestatiemeteritems tot of als een `ITelemetryInitializer` een uitzondering genereert. Met deze module wordt bijgehouden-tracetelemetrie wordt weergegeven in de [diagnostische gegevens doorzoeken][diagnostic]. Diagnostische gegevens verzendt naar dc.services.vsallin.net.

* `Microsoft.ApplicationInsights.Extensibility.Implementation.Tracing.DiagnosticsTelemetryModule`
* [Microsoft.ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet-pakket. Als u alleen dit pakket installeert, wordt het bestand ApplicationInsights.config niet automatisch gemaakt.

### <a name="developer-mode"></a>Modus voor ontwikkelaars
`DeveloperModeWithDebuggerAttachedTelemetryModule` Hiermee wordt de Application Insights `TelemetryChannel` om gegevens direct, een telemetrie-item op een tijdstip, wanneer een foutopsporingsprogramma is gekoppeld aan het toepassingsproces te verzenden. Hiermee wordt de hoeveelheid tijd tussen het moment dat wanneer uw toepassing worden bijgehouden Telemetrie en wanneer deze wordt weergegeven op de Application Insights-portal. Dit zorgt ervoor dat aanzienlijke overhead in CPU en netwerkbandbreedte.

* `Microsoft.ApplicationInsights.WindowsServer.DeveloperModeWithDebuggerAttachedTelemetryModule`
* [Application Insights Windows Server](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet-pakket

### <a name="web-request-tracking"></a>Webaanvraag bijhouden
Rapporten de [tijd en resultaat antwoordcode](../../azure-monitor/app/asp-net.md) van HTTP-aanvragen.

* `Microsoft.ApplicationInsights.Web.RequestTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet-pakket

### <a name="exception-tracking"></a>Uitzonderingen bijhouden
`ExceptionTrackingTelemetryModule` onverwerkte uitzonderingen worden bijgehouden in uw web-app. Zie [fouten en uitzonderingen][exceptions].

* `Microsoft.ApplicationInsights.Web.ExceptionTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet-pakket
* `Microsoft.ApplicationInsights.WindowsServer.UnobservedExceptionTelemetryModule` -nummers [taak uitzonderingen onopgemerkt](https://blogs.msdn.com/b/pfxteam/archive/2011/09/28/task-exception-handling-in-net-4-5.aspx).
* `Microsoft.ApplicationInsights.WindowsServer.UnhandledExceptionTelemetryModule` -niet-verwerkte uitzonderingen voor werkrollen, windows-services en consoletoepassingen worden bijgehouden.
* [Application Insights Windows Server](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet-pakket.

### <a name="eventsource-tracking"></a>Gebeurtenisbron bijhouden
`EventSourceTelemetryModule` Hiermee kunt u gebeurtenissen worden verzonden naar Application Insights als traceringen EventSource configureren. Zie voor meer informatie over het bijhouden van gebeurtenissen EventSource [EventSource gebeurtenissen](../../azure-monitor/app/asp-net-trace-logs.md#using-eventsource-events).

* `Microsoft.ApplicationInsights.EventSourceListener.EventSourceTelemetryModule`
* [Microsoft.ApplicationInsights.EventSourceListener](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EventSourceListener) 

### <a name="etw-event-tracking"></a>ETW-gebeurtenissen bijhouden
`EtwCollectorTelemetryModule` Hiermee kunt u gebeurtenissen van ETW-providers worden verzonden naar Application Insights als traceringen configureren. Zie voor meer informatie over het bijhouden van ETW-gebeurtenissen [ETW-gebeurtenissen met behulp van](../../azure-monitor/app/asp-net-trace-logs.md#using-etw-events).

* `Microsoft.ApplicationInsights.EtwCollector.EtwCollectorTelemetryModule`
* [Microsoft.ApplicationInsights.EtwCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EtwCollector) 

### <a name="microsoftapplicationinsights"></a>Microsoft.ApplicationInsights
Het pakket Microsoft.ApplicationInsights biedt de [core API](https://msdn.microsoft.com/library/mt420197.aspx) van de SDK. Gebruik deze optie de telemetrie-modules en u kunt ook [gebruiken voor het definiëren van uw eigen telemetrie](../../azure-monitor/app/api-custom-events-metrics.md).

* Er is geen vermelding in ApplicationInsights.config.
* [Microsoft.ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet-pakket. Als u alleen deze NuGet installeert, wordt er geen .config-bestand gegenereerd.

## <a name="telemetry-channel"></a>Telemetrie-kanaal
Het kanaal telemetrie beheert buffer en verzenden van telemetrie naar de Application Insights-service.

* `Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel` is de standaardkanaal voor services. Deze buffert gegevens in het geheugen.
* `Microsoft.ApplicationInsights.PersistenceChannel` vormt een alternatief voor consoletoepassingen. Dit bespaart unflushed gegevens naar permanente opslag wanneer uw app wordt afgesloten en deze wordt verzonden wanneer de app wordt opnieuw gestart.

## <a name="telemetry-initializers-aspnet"></a>Telemetrie Initializers (ASP.NET)
Telemetrie initializers instellen contexteigenschappen die worden verzonden, samen met elk telemetrie-item.

U kunt [Schrijf uw eigen initializers](../../azure-monitor/app/api-filtering-sampling.md#add-properties) contexteigenschappen in te stellen.

De standaard initializers worden alle ingesteld door de Web- of WindowsServer NuGet-pakketten:

* `AccountIdTelemetryInitializer` Hiermee stelt u de eigenschap AccountId.
* `AuthenticatedUserIdTelemetryInitializer` Hiermee stelt u de eigenschap AuthenticatedUserId zoals ingesteld door de JavaScript SDK.
* `AzureRoleEnvironmentTelemetryInitializer` updates de `RoleName` en `RoleInstance` eigenschappen van de `Device` context voor alle telemetrie-items met gegevens uit de Azure-runtime-omgeving.
* `BuildInfoConfigComponentVersionTelemetryInitializer` updates de `Version` eigenschap van de `Component` context voor alle telemetrie-items met de waarde die is geëxtraheerd uit de `BuildInfo.config` bestand gemaakt door MS Build.
* `ClientIpHeaderTelemetryInitializer` updates `Ip` eigenschap van de `Location` context van alle telemetrie-items op basis van de `X-Forwarded-For` HTTP-header van de aanvraag.
* `DeviceTelemetryInitializer` de volgende eigenschappen van de `Device` context voor alle telemetrie-items.
  * `Type` is ingesteld op 'PC'
  * `Id` is ingesteld op de domeinnaam van de computer waarop de web-App wordt uitgevoerd.
  * `OemName` is ingesteld op de waarde die is geëxtraheerd uit de `Win32_ComputerSystem.Manufacturer` veld met WMI.
  * `Model` is ingesteld op de waarde die is geëxtraheerd uit de `Win32_ComputerSystem.Model` veld met WMI.
  * `NetworkType` is ingesteld op de waarde die is geëxtraheerd uit de `NetworkInterface`.
  * `Language` is ingesteld op de naam van de `CurrentCulture`.
* `DomainNameRoleInstanceTelemetryInitializer` updates de `RoleInstance` eigenschap van de `Device` context voor alle telemetrie-items met de naam van het domein van de computer waarop de web-App wordt uitgevoerd.
* `OperationNameTelemetryInitializer` updates de `Name` eigenschap van de `RequestTelemetry` en de `Name` eigenschap van de `Operation` context van alle telemetrie-items op basis van de HTTP-methode, evenals de namen van ASP.NET MVC-controller en de actie die wordt aangeroepen om de aanvraag te verwerken.
* `OperationIdTelemetryInitializer` of `OperationCorrelationTelemetryInitializer` updates de `Operation.Id` contexteigenschap van alle telemetrie-items die zijn getraceerd tijdens de verwerking van een aanvraag met de automatisch gegenereerde `RequestTelemetry.Id`.
* `SessionTelemetryInitializer` updates de `Id` eigenschap van de `Session` context voor alle telemetrie-items met de waarde opgehaald uit de `ai_session` cookie gegenereerd door de ApplicationInsights JavaScript instrumentatie-code die wordt uitgevoerd in de browser van de gebruiker.
* `SyntheticTelemetryInitializer` of `SyntheticUserAgentTelemetryInitializer` updates de `User`, `Session`, en `Operation` contexten eigenschappen van alle telemetrie-items bijgehouden bij het verwerken van een aanvraag van een synthetische bron, zoals een beschikbaarheidsset testen of zoeken naar engine bot. Standaard [Metrics Explorer](../../azure-monitor/app/metrics-explorer.md) synthetische telemetrie niet wordt weergegeven.

    De `<Filters>` id-eigenschappen van de aanvragen instellen.
* `UserTelemetryInitializer` updates de `Id` en `AcquisitionDate` eigenschappen van `User` context voor alle telemetrie-items met waarden die zijn geëxtraheerd uit de `ai_user` cookie gegenereerd door de Application Insights JavaScript instrumentatie-code die wordt uitgevoerd in een van de gebruiker Browser.
* `WebTestTelemetryInitializer` Hiermee stelt u de gebruikers-id, de sessie-id en de eigenschappen van synthetische gegevensbron voor HTTP-die afkomstig is van aanvragen [beschikbaarheidstests](../../azure-monitor/app/monitor-web-app-availability.md).
  De `<Filters>` id-eigenschappen van de aanvragen instellen.

Voor .NET-toepassingen die worden uitgevoerd in Service Fabric, u kunt opnemen de `Microsoft.ApplicationInsights.ServiceFabric` NuGet-pakket. Dit pakket bevat een `FabricTelemetryInitializer`, welke Service Fabric-eigenschappen toevoegt aan de telemetrie-items. Zie voor meer informatie de [GitHub-pagina](https://github.com/Microsoft/ApplicationInsights-ServiceFabric/blob/master/README.md) over de eigenschappen die zijn toegevoegd door dit NuGet-pakket.

## <a name="telemetry-processors-aspnet"></a>Telemetrie-Processors (ASP.NET)
Telemetrie-processors kunnen filteren en elk telemetrie-item wijzigen voordat deze van de SDK wordt verzonden naar de portal.

U kunt [de processors van uw eigen telemetrie schrijven](../../azure-monitor/app/api-filtering-sampling.md#filtering).

#### <a name="adaptive-sampling-telemetry-processor-from-200-beta3"></a>Adaptieve steekproeven telemetrie processor (van 2.0.0-beta3)
Deze optie is standaard ingeschakeld. Als uw app veel telemetrie verzendt, verwijdert deze processor sommige van deze.

```xml

    <TelemetryProcessors>
      <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
        <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
      </Add>
    </TelemetryProcessors>

```

De parameter bevat het doel dat het algoritme probeert te bereiken. Elk exemplaar van de SDK werkt onafhankelijk van elkaar, dus als uw server een cluster met meerdere machines is, het daadwerkelijke volume van telemetrie dienovereenkomstig worden vermenigvuldigd.

[Meer informatie over steekproeven](../../azure-monitor/app/sampling.md).

#### <a name="fixed-rate-sampling-telemetry-processor-from-200-beta1"></a>Vast aantal steekproeven telemetrie processor (van 2.0.0-beta1)
Er is ook een standaard [steekproeven telemetrie processor](../../azure-monitor/app/api-filtering-sampling.md) (van 2.0.1):

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
Deze parameters van invloed op hoe de Java-SDK moet opslaan en leegmaken van de telemetriegegevens die worden verzameld.

#### <a name="maxtelemetrybuffercapacity"></a>MaxTelemetryBufferCapacity
Het aantal telemetrie-items die kunnen worden opgeslagen in de SDK in-memory-opslag. Wanneer dit aantal is bereikt, wordt de telemetrie-buffer worden leeggemaakt - dat wil zeggen, de telemetrie-items worden verzonden naar de Application Insights-server.

* Min.: 1
* Max.: 1000
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
Bepaalt hoe vaak de gegevens die zijn opgeslagen in de opslag in het geheugen moet worden leeggemaakt (verzonden naar Application Insights).

* Min.: 1
* Max.: 300
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
Bepaalt de maximale grootte in MB die is toegewezen aan de permanente opslag op de lokale schijf. Deze opslag wordt gebruikt voor persistente telemetrie-items die niet worden verzonden naar het eindpunt van de Application Insights. Wanneer de maximale grootte is bereikt, gaan nieuwe telemetrie-items verloren.

* Min.: 1
* Max.: 100
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

#### <a name="local-forwarder"></a>Lokale doorstuurserver

[Lokale doorstuurserver](opencensus-local-forwarder.md) is een agent die Application Insights verzamelt of [OpenCensus](https://opencensus.io/) telemetrie uit een groot aantal SDK's en frameworks en doorgestuurd naar Application Insights. Het is geschikt voor het uitvoeren onder Windows en Linux. Wanneer in combinatie met de Application Insights Java SDK de lokale doorstuurserver biedt volledige ondersteuning voor [Live Metrics](../../azure-monitor/app/live-stream.md) en adaptieve steekproeven.

```xml
<Channel type="com.microsoft.applicationinsights.channel.concrete.localforwarder.LocalForwarderTelemetryChannel">
<EndpointAddress><!-- put the hostname:port of your LocalForwarder instance here --></EndpointAddress>

<!-- The properties below are optional. The values shown are the defaults for each property -->

<FlushIntervalInSeconds>5</FlushIntervalInSeconds><!-- must be between [1, 500]. values outside the bound will be rounded to nearest bound -->
<MaxTelemetryBufferCapacity>500</MaxTelemetryBufferCapacity><!-- units=number of telemetry items; must be between [1, 1000] -->
</Channel>
```

Als u SpringBoot starter gebruikt, moet u het volgende toevoegen aan uw configuratiebestand (application.properties):

```yml
azure.application-insights.channel.local-forwarder.endpoint-address=<!--put the hostname:port of your LocalForwarder instance here-->
azure.application-insights.channel.local-forwarder.flush-interval-in-seconds=<!--optional-->
azure.application-insights.channel.local-forwarder.max-telemetry-buffer-capacity=<!--optional-->
```

Standaardwaarden zijn hetzelfde voor SpringBoot application.properties en applicationinsights.xml-configuratie.

## <a name="instrumentationkey"></a>InstrumentationKey
Hiermee bepaalt u de Application Insights-resource waarin uw gegevens wordt weergegeven. Doorgaans maakt u een afzonderlijke resource met een aparte sleutel voor elk van uw toepassingen.

Als u instellen van de sleutel dynamisch: bijvoorbeeld wilt als u wilt verzenden van resultaten van uw toepassing om verschillende bronnen - kunt u de sleutel van het configuratiebestand weglaten en instellen in code in plaats daarvan.

Om in te stellen de sleutel voor alle exemplaren van de TelemetryClient, stel inclusief standaardtelemetrie voor modules, de sleutel in TelemetryConfiguration.Active. Dit doen in een initialiseringsmethode, zoals global.aspx.cs in een ASP.NET-service:

```csharp

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey =
          // - for example -
          WebConfigurationManager.AppSettings["ikey"];
      //...
```

Als u alleen een specifieke set gebeurtenissen verzenden naar een andere resource wilt, kunt u de sleutel voor een specifieke TelemetryClient instellen:

```csharp

    var tc = new TelemetryClient();
    tc.Context.InstrumentationKey = "----- my key ----";
    tc.TrackEvent("myEvent");
    // ...

```

Ophalen van een nieuwe sleutel [een nieuwe resource maken in de Application Insights-portal][new].



## <a name="applicationid-provider"></a>ApplicationId Provider

_Beschikbaar in v2.6.0_

Het doel van deze provider is voor het opzoeken van een toepassings-ID op basis van een Instrumentatiesleutel. De toepassings-ID is opgenomen in RequestTelemetry en DependencyTelemetry en gebruikt om te bepalen correlatie in de Portal.

Deze optie is beschikbaar door in te stellen `TelemetryConfiguration.ApplicationIdProvider` in code of configuratie.

### <a name="interface-iapplicationidprovider"></a>-Interface: IApplicationIdProvider

```csharp
public interface IApplicationIdProvider
{
    bool TryGetApplicationId(string instrumentationKey, out string applicationId);
}
```


We bieden twee implementaties in de [Microsoft.ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights) sdk: `ApplicationInsightsApplicationIdProvider` en `DictionaryApplicationIdProvider`.

### <a name="applicationinsightsapplicationidprovider"></a>ApplicationInsightsApplicationIdProvider

Dit is een wrapper rond de API-profiel. Deze wordt aanvragen en resultaten van de cache beperken.

Deze provider wordt toegevoegd aan uw configuratiebestand tijdens de installatie van een [Microsoft.ApplicationInsights.DependencyCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) of [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/)

Deze klasse is een optionele eigenschap `ProfileQueryEndpoint`.
Dit is standaard ingesteld op `https://dc.services.visualstudio.com/api/profiles/{0}/appId`.
Als u configureren van een proxy voor deze configuratie wilt, raden wij aan via een proxy de base-adres en met ' /api/profielen/{0}/appId '. Houd er rekening mee dat '{0}' tijdens runtime per aanvraag wordt vervangen door de Instrumentatiesleutel.

#### <a name="example-configuration-via-applicationinsightsconfig"></a>Van de voorbeeldconfiguratie via ApplicationInsights.config:
```xml
<ApplicationInsights>
    ...
    <ApplicationIdProvider Type="Microsoft.ApplicationInsights.Extensibility.Implementation.ApplicationId.ApplicationInsightsApplicationIdProvider, Microsoft.ApplicationInsights">
        <ProfileQueryEndpoint>https://dc.services.visualstudio.com/api/profiles/{0}/appId</ProfileQueryEndpoint>
    </ApplicationIdProvider>
    ...
</ApplicationInsights>
```

#### <a name="example-configuration-via-code"></a>Van de voorbeeldconfiguratie via code:
```csharp
TelemetryConfiguration.Active.ApplicationIdProvider = new ApplicationInsightsApplicationIdProvider();
```

### <a name="dictionaryapplicationidprovider"></a>DictionaryApplicationIdProvider

Dit is een statische provider, die op de geconfigureerde Instrumentatiesleutel steunen / toepassings-ID-paren.

Deze klasse is een eigenschap `Defined`, waarmee wordt een Dictionary < string, string > van de Instrumentatiesleutel paren van toepassings-ID.

Deze klasse is een optionele eigenschap `Next` die kan worden gebruikt voor het configureren van een andere provider moet worden gebruikt wanneer een Instrumentatiesleutel wordt aangevraagd die niet bestaat in uw configuratie.

#### <a name="example-configuration-via-applicationinsightsconfig"></a>Van de voorbeeldconfiguratie via ApplicationInsights.config:
```xml
<ApplicationInsights>
    ...
    <ApplicationIdProvider Type="Microsoft.ApplicationInsights.Extensibility.Implementation.ApplicationId.DictionaryApplicationIdProvider, Microsoft.ApplicationInsights">
        <Defined>
            <Type key="InstrumentationKey_1" value="ApplicationId_1"/>
            <Type key="InstrumentationKey_2" value="ApplicationId_2"/>
        </Defined>
        <Next Type="Microsoft.ApplicationInsights.Extensibility.Implementation.ApplicationId.ApplicationInsightsApplicationIdProvider, Microsoft.ApplicationInsights" />
    </ApplicationIdProvider>
    ...
</ApplicationInsights>
```

#### <a name="example-configuration-via-code"></a>Van de voorbeeldconfiguratie via code:
```csharp
TelemetryConfiguration.Active.ApplicationIdProvider = new DictionaryApplicationIdProvider{
 Defined = new Dictionary<string, string>
    {
        {"InstrumentationKey_1", "ApplicationId_1"},
        {"InstrumentationKey_2", "ApplicationId_2"}
    }
};
```




## <a name="next-steps"></a>Volgende stappen
[Meer informatie over de API][api].

<!--Link references-->

[api]: ../../azure-monitor/app/api-custom-events-metrics.md
[client]: ../../azure-monitor/app/javascript.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[exceptions]: ../../azure-monitor/app/asp-net-exceptions.md
[netlogs]: ../../azure-monitor/app/asp-net-trace-logs.md
[new]: ../../azure-monitor/app/create-new-resource.md 
[redfield]: ../../azure-monitor/app/monitor-performance-live-website-now.md
[start]: ../../azure-monitor/app/app-insights-overview.md
