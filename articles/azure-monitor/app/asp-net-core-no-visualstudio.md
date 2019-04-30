---
title: Azure Application Insights voor ASP.NET Core zonder Visual Studio | Microsoft Docs
description: Bewaak ASP.NET Core web-apps voor beschikbaarheid, prestaties en gebruik.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 3b722e47-38bd-4667-9ba4-65b7006c074c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 04/03/2019
ms.author: cithomas
ms.openlocfilehash: 8243523887ec9861459b2d196126237cf89bad97
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60691339"
---
# <a name="application-insights-for-aspnet-core-applications"></a>Application Insights voor ASP.NET Core-toepassingen

In dit artikel beschrijft de stappen voor het inschakelen van Application Insights voor een [ASP.NET Core](https://docs.microsoft.com/aspnet/core/?view=aspnetcore-2.2) toepassing zonder gebruik van de Visual Studio IDE. Hebt u Visual Studio IDE geïnstalleerd, klikt u vervolgens [dit](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core) document bevat de specifieke instructies Visual Studio. Application Insights wordt door de instructies in dit artikel is voltooid, start aanvragen, afhankelijkheden, uitzonderingen, prestatiemeteritems, heartbeats en logboeken verzamelen van uw ASP.NET Core-toepassing. De voorbeeldtoepassing gebruikt is een [MVC-toepassing](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/?view=aspnetcore-2.2) die gericht is op `netcoreapp2.2`, maar hier instructies zijn van toepassing op alle ASP.NET Core-toepassingen. Eventuele uitzonderingen worden genoemd om, indien van toepassing.

## <a name="supported-scenarios"></a>Ondersteunde scenario's

Application Insights-SDK (Software Development Kit) voor ASP.NET Core kunt bewaken van uw toepassingen, ongeacht waar of hoe de toepassing wordt uitgevoerd. Als uw toepassing wordt uitgevoerd en netwerkverbinding met de Application Insights-service heeft, telemetrie naar verwachting worden verzameld. Deze ondersteuning bevat, maar is niet beperkt tot een besturingssysteem (Windows, Linux, Mac), hostingmethode (in-process vs out-of-process), implementatiemethode (afhankelijk van het framework versus zelfstandig), webserver (IIS, Kestrel), platform (Azure Web Apps, Azure-VM Docker, AKS en enz.) of IDE (Visual Studio, Visual Studio Code, opdrachtregel).

## <a name="prerequisites"></a>Vereisten

- Een werkende ASP.NET Core-toepassing. Ga als volgt [dit](https://docs.microsoft.com/aspnet/core/getting-started/) handleiding voor het maken van een ASP.NET Core-toepassing, indien nodig.
- Een geldige Application Insights-instrumentatiesleutel, wat vereist is voor alle telemetrie wordt verzonden naar de Application Insights-service. Ga als volgt [deze](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource) instructies voor het maken van een nieuwe Application Insights-resource, indien nodig en een instrumentatiesleutel te verkrijgen.

## <a name="enabling-application-insights-server-side-telemetry"></a>Telemetrie Application Insights-Server-Side inschakelen

1. Installeer de Application Insights SDK voor ASP.NET Core, dit een normale NuGet-pakket is. Het is raadzaam altijd de nieuwste stabiele versie wilt gebruiken. Enkele van de functies die worden beschreven in dit artikel is mogelijk niet beschikbaar in oudere versies. Volledige release-opmerkingen voor de SDK kunnen worden gevonden [hier](https://github.com/Microsoft/ApplicationInsights-aspnetcore/releases). 

Hieronder ziet u de wijzigingen moeten worden toegevoegd aan de .csproj-bestand van uw project.

```xml
    <ItemGroup>
      <PackageReference Include="Microsoft.ApplicationInsights.AspNetCore" Version="2.6.1" />
    </ItemGroup>
```

2. Voeg `services.AddApplicationInsightsTelemetry();` naar `ConfigureServices()` methode in uw `Startup` klasse. Compleet voorbeeld hieronder.

```csharp
    // This method gets called by the runtime. Use this method to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
        // The following line enables Application Insights telemetry collection.
        services.AddApplicationInsightsTelemetry();

        // code adding other services for your application
        services.AddMvc();
    }
```

3. Instrumentatiesleutel instellen.

   Het is mogelijk instrumentatiesleutel opgeven als een argument voor `services.AddApplicationInsightsTelemetry("putinstrumentationkeyhere");`, het verdient aanbeveling om op te geven van de instrumentatiesleutel in configuratie. Hieronder ziet u hoe u een instrumentatiesleutel in `appsettings.json`. Zorg ervoor dat `appsettings.json` wordt gekopieerd naar de hoofdmap van de toepassing tijdens het publiceren.

```json
    {
      "ApplicationInsights": {
        "InstrumentationKey": "putinstrumentationkeyhere"
      },
      "Logging": {
        "LogLevel": {
          "Default": "Warning"
        }
      }
    }
```

    Alternately, the instrumentation key can also be specified in either of the following environment variables.
    APPINSIGHTS_INSTRUMENTATIONKEY
    ApplicationInsights:InstrumentationKey

    Example:
    `SET ApplicationInsights:InstrumentationKey=putinstrumentationkeyhere`

`APPINSIGHTS_INSTRUMENTATIONKEY` wordt doorgaans gebruikt om op te geven van de instrumentatiesleutel voor toepassingen die zijn geïmplementeerd in Azure Web Apps.

> [!NOTE]
> Een instrumentatiesleutel in code wins opgegeven via de omgevingsvariabele `APPINSIGHTS_INSTRUMENTATIONKEY`, die wins boven de andere opties.

4. Start uw toepassing en aanvragen toe. Telemetrie moet worden doorgestuurd naar Application Insights nu gestart. De volgende telemetrie wordt automatisch verzameld door de Application Insights-SDK.

    1. Aanvragen - binnenkomende webaanvragen naar uw toepassing.
    1. Afhankelijkheden
        1. HTTP/Https-aanroepen met `HttpClient`
        1. SQL-aanroepen met `SqlClient`
        1. Azure storage-aanroepen met [Azure Storage-Client](https://www.nuget.org/packages/WindowsAzure.Storage/)*
        1. [Event hub-Client SDK](https://www.nuget.org/packages/Microsoft.Azure.EventHubs) versie 1.1.0 of hoger
        1. [Client-SDK van service bus](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus) versie 3.0.0 en hoger

             * Azure Cosmos DB is automatisch bijgehouden alleen als u HTTP/HTTPS gebruikt. De TCP-modus wordt niet door Application Insights worden vastgelegd.


    1. [Prestatiemeteritems](https://azure.microsoft.com/documentation/articles/app-insights-web-monitor-performance/)
        1. Ondersteuning voor prestatiemeteritems in ASP.NET Core is beperkt tot de volgende
            1. SDK-versie 2.4.1 en verzamelt prestatiemeters boven als de toepassing wordt uitgevoerd in Azure Web App (Windows)
            1. SDK-versie 2.7.0-beta3 en verzamelt prestatiemeters boven als de toepassing wordt uitgevoerd in Windows, en die zijn gericht op `NETSTANDARD2.0` of hoger.
            1. Voor toepassingen die zijn gericht op het volledige .NET Framework, worden prestatiemeteritems ondersteund in alle versies van de SDK.

            Dit document wordt bijgewerkt wanneer prestaties teller ondersteuning in Linux wordt toegevoegd.
    1. [Live Metrics](https://docs.microsoft.com/azure/application-insights/app-insights-live-stream)
    1. `ILogger` Logboeken van de ernst `Warning` of hoger worden automatisch opgenomen in de SDK-versie 2.7.0-beta3 of hoger. Lees meer [hier](https://docs.microsoft.com/azure/azure-monitor/app/ilogger).

Het duurt een paar minuten telemetriegegevens om te starten in de portal weergegeven. Om snel te controleren of alles werkt, is het raadzaam te gebruiken [Live Metrics](https://docs.microsoft.com/azure/application-insights/app-insights-live-stream), terwijl het maken van aanvragen naar de toepassing die wordt uitgevoerd.

## <a name="send-ilogger-logs-to-application-insights"></a>ILogger logboeken verzenden naar Application Insights

Application Insights biedt ondersteuning voor vastleggen logboeken die worden verzonden via ILogger. Lees de volledige documentatie [hier](https://docs.microsoft.com/azure/azure-monitor/app/ilogger).

## <a name="enable-client-side-telemetry-for-web-applications"></a>Client-side-telemetrie inschakelen voor Web-Apps

De bovenstaande stappen zijn voldoende zijn om te beginnen met het verzamelen van telemetrie aan de serverzijde. Als uw toepassing onderdelen op de client heeft, volgt u onderstaande stappen om te beginnen met het verzamelen [gebruikstelemetrie](https://docs.microsoft.com/azure/azure-monitor/app/usage-overview) van daaruit.

1. Voeg in _ViewImports.cshtml, injectie:

```cshtml
    @inject Microsoft.ApplicationInsights.AspNetCore.JavaScriptSnippet JavaScriptSnippet
```

2. Voeg in _Layout.cshtml, HtmlHelper aan het einde van `<head>` sectie, maar voordat u een script. Een aangepaste JavaScript telemetrie die u wilt rapporteren in op de pagina moet worden toegevoegd nadat dit fragment:

```cshtml
    @Html.Raw(JavaScriptSnippet.FullScript)
    </head>
```

Wijzig de bestandsnamen aan de hand van uw toepassing. Namen van de hierboven hebt gebruikt, zijn van een standaardsjabloon voor MVC-toepassing.

## <a name="configuring-application-insights-sdk"></a>Application Insights-SDK configureren

Application Insights SDK voor ASP.NET Core kan worden aangepast om te wijzigen van de standaardconfiguratie. Het is mogelijk dat gebruikers van Application Insights-SDK voor ASP.NET bekend bent met het gebruik van de configuratie van `ApplicationInsights.config`, of door het wijzigen van `TelemetryConfiguration.Active`. Configuratie wordt voor ASP.NET Core, anders uitgevoerd. ASP.NET Core-SDK wordt toegevoegd aan de toepassing met behulp van ASP.NET Core ingebouwde [DependencyInjection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) mechanisme en het configureren van dezelfde dan ook gebruiken DependencyInjection. Bijna alle wijzigingen in de configuratie worden uitgevoerd in de `ConfigureServices()` -methode van uw `Startup.cs` klasse, tenzij anders vermeld. Volg de onderstaande secties voor meer informatie.

> [!NOTE]
> Het is belangrijk te weten dat de configuratie wijzigen door het wijzigen van `TelemetryConfiguration.Active` wordt niet aanbevolen in ASP.NET Core-toepassingen.

### <a name="configuring-using-applicationinsightsserviceoptions"></a>Configureren met behulp van ApplicationInsightsServiceOptions

Het is mogelijk een paar algemene instellingen wijzigen door door te geven `ApplicationInsightsServiceOptions` naar `services.AddApplicationInsightsTelemetry();`. Hieronder ziet u een voorbeeld.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        Microsoft.ApplicationInsights.AspNetCore.Extensions.ApplicationInsightsServiceOptions aiOptions
                    = new Microsoft.ApplicationInsights.AspNetCore.Extensions.ApplicationInsightsServiceOptions();
        // Disables adaptive sampling.
        aiOptions.EnableAdaptiveSampling = false;

        // Disables QuickPulse (Live Metrics stream).
        aiOptions.EnableQuickPulseMetricStream = false;
        services.AddApplicationInsightsTelemetry(aiOptions);
    }
```

De exacte lijst met configureerbare instellingen in `ApplicationInsightsServiceOptions` vindt [hier](https://github.com/Microsoft/ApplicationInsights-aspnetcore/blob/f0b8631e482d25982eb52092103b34e3ff6e5fef/src/Microsoft.ApplicationInsights.AspNetCore/Extensions/ApplicationInsightsServiceOptions.cs).

### <a name="sampling"></a>Steekproeven

Application Insights SDK voor ASP.NET Core ondersteunt zowel FixedRate en adaptieve steekproeven. Adaptieve steekproeven is standaard ingeschakeld. Ga als volgt [dit](../../azure-monitor/app/sampling.md#configuring-adaptive-sampling-for-aspnet-core-applications) document voor meer informatie over het configureren van sampling voor ASP.NET Core-toepassingen.

### <a name="adding-telemetryinitializers"></a>TelemetryInitializers toevoegen

Toevoegen van een nieuwe `TelemetryInitializer`, naar de Container DependencyInjection toevoegen, zoals hieronder wordt weergegeven. `TelemetryInitializer`s toegevoegd aan DependencyInjection container wordt opgehaald door de SDK automatisch.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddSingleton<ITelemetryInitializer, MyCustomTelemetryInitializer>();
    }
```

### <a name="removing-telemetryinitializers"></a>Removing TelemetryInitializers

Alle te verwijderen of specifieke TelemetryInitializers, die standaard aanwezig zijn, gebruikt u de volgende voorbeeldcode **nadat** aanroepen `AddApplicationInsightsTelemetry()`.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddApplicationInsightsTelemetry();

        // Remove a specific built-in TelemetryInitializer
        var tiToRemove = services.FirstOrDefault<ServiceDescriptor>
                         (t => t.ImplementationType == typeof(AspNetCoreEnvironmentTelemetryInitializer));
        if (tiToRemove != null)
        {
            services.Remove(tiToRemove);
        }

        // Remove all initializers
        // This requires importing namespace using Microsoft.Extensions.DependencyInjection.Extensions;
        services.RemoveAll(typeof(ITelemetryInitializer));
    }
```

### <a name="adding-telemetryprocessors"></a>TelemetryProcessors toevoegen

Aangepaste telemetrie processors kunnen worden toegevoegd aan de `TelemetryConfiguration` met behulp van de uitbreidingsmethode `AddApplicationInsightsTelemetryProcessor` op `IServiceCollection`. Gebruik het volgende voorbeeld.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        // ...
        services.AddApplicationInsightsTelemetry();
        services.AddApplicationInsightsTelemetryProcessor<MyFirstCustomTelemetryProcessor>();

        // If you have more processors:
        services.AddApplicationInsightsTelemetryProcessor<MySecondCustomTelemetryProcessor>();
    }
```

### <a name="configuring-or-removing-default-telemetrymodules"></a>Configureren of verwijderen van het standaard TelemetryModules

De volgende automatische verzameling modules zijn standaard ingeschakeld en zijn verantwoordelijk voor het automatisch verzamelen van telemetrie. Ze kunnen worden uitgeschakeld en geconfigureerd voor het standaardgedrag wijzigen.

1. `RequestTrackingTelemetryModule`
1. `DependencyTrackingTelemetryModule`
1. `PerformanceCollectorModule`
1. `QuickPulseTelemetryModule`
1. `AppServicesHeartbeatTelemetryModule`
1. `AzureInstanceMetadataTelemetryModule`

Het configureren van een standaard `TelemetryModule`, gebruikt u de uitbreidingsmethode `ConfigureTelemetryModule<T>` op `IServiceCollection` zoals wordt weergegeven in het onderstaande voorbeeld.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddApplicationInsightsTelemetry();

        // The following configures DependencyTrackingTelemetryModule.
        // Similarly, any other default modules can be configured.
        services.ConfigureTelemetryModule<DependencyTrackingTelemetryModule>((module, o) =>
                        {
                            module.EnableW3CHeadersInjection = true;
                        });

        // The following removes PerformanceCollectorModule to disable perf-counter collection.
        // Similarly, any other default modules can be removed.
        var performanceCounterService = services.FirstOrDefault<ServiceDescriptor>(t => t.ImplementationType == typeof(PerformanceCollectorModule));
        if (performanceCounterService != null)
        {
         services.Remove(performanceCounterService);
        }
    }
```

### <a name="configuring-telemetry-channel"></a>Telemetrie-kanaal configureren

De standaardkanaal dat wordt gebruikt is de `ServerTelemetryChannel`. Deze kan worden genegeerd door het volgende voorbeeld hieronder.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        // use the following to replace the default channel with InMemoryChannel.
        // this can also be applied to ServerTelemetryChannel as well.
        services.AddSingleton(typeof(ITelemetryChannel), new InMemoryChannel() {MaxTelemetryBufferCapacity = 19898 });

        services.AddApplicationInsightsTelemetry();
    }
```

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

*1. Ik wil meer telemetrie naast de telemetrie automatisch verzameld bijhouden. Hoe moet ik doen?*

* Een instantie van `TelemetryClient` door met behulp van Constructor-injectie, en roept u de vereiste `TrackXXX()` methode. Het is niet raadzaam maken van nieuwe `TelemetryClient` -exemplaren in ASP.NET Core-toepassing, als een singleton-instantie van `TelemetryClient` is al geregistreerd in de container DI, zodat deelt `TelemetryConfiguration` met de rest van de telemetrie. Het maken van een nieuw `TelemetryClient` exemplaar wordt alleen aanbevolen als er een afzonderlijke configuratie van de rest van de telemetrie hebben. Het volgende voorbeeld ziet hoe u kunt extra telemetrie bijhouden van een domeincontroller.

```csharp
public class HomeController : Controller
{
    private TelemetryClient telemetry;

    // use constructor injection to get TelemetryClient instance
    public HomeController(TelemetryClient telemetry)
    {
        this.telemetry = telemetry;
    }

    public IActionResult Index()
    {
        // call required TrackXXX method.
        this.telemetry.TrackEvent("HomePageRequested");
        return View();
    }
```

 Raadpleeg [Application Insights aangepaste metrische gegevens API-verwijzing](https://docs.microsoft.com/azure/azure-monitor/app/api-custom-events-metrics/) voor beschrijving van aangepaste gegevens rapporteren in Application Insights.

*2. Sommige Visual Studio-sjablonen gebruikt UseApplicationInsights() uitbreidingsmethode op IWebHostBuilder om in te schakelen van Application Insights. Is dit gebruik nog steeds geldig?*

* Application Insights inschakelen met deze methode is geldig en wordt gebruikt in Visual Studio bepalen, en in de Azure-Web-App-extensies. Het wordt echter aanbevolen om te gebruiken `services.AddApplicationInsightsTelemery()` als het overloads voor het beheren van een configuratie biedt. Zowel intern doet hetzelfde methode, dus als er is geen aangepaste configuratie worden toegepast, aanroepende is een prima.

*3. Ik ben mijn ASP.NET Core-toepassing naar Azure Web Apps implementeren. Moet ik nog steeds de Application Insights-extensie van Web-Apps inschakelen?*

* Als de SDK is geïnstalleerd tijdens het opbouwen, zoals wordt weergegeven in dit artikel, is er geen noodzaak voor het inschakelen van de Application Insights-extensie van de Web-Apps-portal. Zelfs als de uitbreiding is geïnstalleerd, deze wordt uitstel, wanneer deze SDK is al toegevoegd aan de toepassing wordt gedetecteerd. Application Insights vanuit uitbreiding inschakelt, hoeft u installeren en bijwerken van de SDK aan uw toepassing. Application Insights inschakelen aan de hand van dit artikel is echter flexibeler om redenen die hieronder.
    1. Application Insights Telemetry blijven werken
        1. Alle besturingssystemen: Windows, Linux, Mac
        1. Alle publiceren modus - zelfstandig of afhankelijk zijn van het Framework.
        1. Alle target-frameworks, met inbegrip van het volledige .NET Framework.
        1. Alle Hosting-opties - Azure-Web-APP, virtuele machines, Linux, Containers, AKS, niet-Azure.
    1. Telemetrie kan lokaal worden gezien als foutopsporing via Visual Studio.
    1. Kan bijhouden met behulp van aanvullende aangepaste telemetrie `TrackXXX()` API.
    1. Heeft volledige controle over de configuratie.

*4. Kan ik de bewaking van Application Insights met behulp van hulpprogramma's zoals Status Monitor inschakelen?*

* Nee. [Statusmonitor](https://docs.microsoft.com/azure/azure-monitor/app/monitor-performance-live-website-now) en toekomstige [IISConfigurator](https://github.com/Microsoft/ApplicationInsights-Announcements/issues/21) ondersteunt momenteel alleen ASP.NET. Het document wordt bijgewerkt wanneer ondersteuning voor ASP.NET Core-toepassing beschikbaar is.

*5. Ik heb een ASP.NET Core 2.0-toepassing? Is niet Application Insights automatisch ingeschakeld voor deze zonder dat ik iets doen?*

* `Microsoft.AspNetCore.All` 2.0 metapackage opgenomen Application Insights-SDK (versie 2.1.0), en als u de toepassing onder de foutopsporing van Visual Studio, Visual Studio application insights kunt en toont telemetrie lokaal in de IDE zelf. Telemetrie is niet verzonden naar de Application Insights-service, tenzij een instrumentatiesleutel expliciet is opgegeven. Volg de instructies in dit artikel voor het inschakelen van Application Insights, zelfs voor 2.0 wordt aangeraden apps.

*6. Kan ik mijn toepassing in Linux uitgevoerd. Zijn alle functies in Linux ook ondersteund?*

* Ja. Functies die worden ondersteund voor de SDK is dezelfde op alle platforms, met de volgende uitzonderingen:
    1. Prestatiemeteritems zijn nog niet ondersteund in niet-Windows. Dit document wordt bijgewerkt als Linux-ondersteuning is toegevoegd.
    1. Hoewel `ServerTelemetryChannel` is ingeschakeld standaard, als de toepassing wordt uitgevoerd in niet-windows, het kanaal maakt niet automatisch een map met lokale opslag om telemetrie tijdelijk als er netwerkproblemen zijn. Deze beperking zorgt ervoor dat telemetrie verloren als er tijdelijk netwerkfout of problemen met de server. De oplossing voor dit probleem is voor de gebruiker het configureren van een lokale map voor het kanaal, zoals hieronder wordt weergegeven.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        // The following will configure channel to use the given folder to temporarily
        // store telemetry items during network or application insights server issues.
        // User should ensure that the given folder already exists,
        // and that application has read/write permissions.
        services.AddSingleton(typeof(ITelemetryChannel),
                                new ServerTelemetryChannel () {StorageFolder = "/tmp/myfolder"});
        services.AddApplicationInsightsTelemetry();
    }
```
