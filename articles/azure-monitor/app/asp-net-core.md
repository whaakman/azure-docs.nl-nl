---
title: Azure Application Insights voor ASP.NET Core | Microsoft Docs
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
ms.date: 05/21/2019
ms.author: mbullwin
ms.openlocfilehash: 8522b1d0a8f8466870966d3f11ce66f7bf15672b
ms.sourcegitcommit: db3fe303b251c92e94072b160e546cec15361c2c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/22/2019
ms.locfileid: "66016372"
---
# <a name="application-insights-for-aspnet-core-applications"></a>Application Insights voor ASP.NET Core-toepassingen

In dit artikel wordt beschreven hoe u Application Insights voor een [ASP.NET Core](https://docs.microsoft.com/aspnet/core) toepassing. Wanneer u de instructies in dit artikel hebt voltooid, kan Application Insights wordt gestart aanvragen, afhankelijkheden, uitzonderingen, prestatiemeteritems, heartbeats en logboeken verzamelen van uw ASP.NET Core-toepassing. De voorbeeldtoepassing is een [MVC-toepassing](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app) die gericht is op `netcoreapp2.2`, maar deze instructies zijn van toepassing op alle ASP.NET Core-toepassingen.

## <a name="supported-scenarios"></a>Ondersteunde scenario's

De [Application Insights-SDK (Software Development Kit) voor ASP.NET Core](https://nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) kunt bewaken van uw toepassingen, ongeacht waar of hoe de toepassing wordt uitgevoerd. Als uw toepassing wordt uitgevoerd en netwerkverbinding met Azure heeft, kan telemetriegegevens worden verzameld. Dit betekent dat overal die.NET Core wordt ondersteund door die Application Insights-bewaking wordt ondersteund. Deze ondersteuning omvat een besturingssysteem (Windows, Linux, Mac), hostingmethode (in-process vs out-of-process), implementatiemethode (afhankelijk van het framework versus zelfstandig), webserver (IIS, Kestrel), host-platform (Azure Web Apps Azure-VM, Docker, Azure Kubernetes Service (AKS), enzovoort.) of IDE (Visual Studio, Visual Studio Code, opdrachtregel).

## <a name="prerequisites"></a>Vereisten

- Een werkende ASP.NET Core-toepassing. Ga als volgt de [ASP.NET Core aan de slag](https://docs.microsoft.com/aspnet/core/getting-started/) te maken van een ASP.NET Core-toepassing, indien nodig.
- Een geldige Application Insights-instrumentatiesleutel, wat vereist is voor alle telemetrie wordt verzonden naar de Application Insights-service. Ga als volgt de [instructies voor een resource maken](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource) te maken van een nieuwe Application Insights-resource, indien nodig en een instrumentatiesleutel te verkrijgen.

## <a name="enable-application-insights-server-side-telemetry-visual-studio"></a>Application Insights-serverzijde telemetrie (Visual Studio) inschakelen

1. Open het project in Visual Studio.

    > [!TIP]
    > Tijdens het verplichte stap, kan het handig zijn voor het instellen van broncodebeheer voor uw project, zodat u alle wijzigingen die door de Application Insights kunt bijhouden. Om in te schakelen de bron besturingselement selecteren **bestand** > **Add to Source Control**.

2. Selecteer **Project** > **Application Insights Telemetry toevoegen**.

3. Selecteer **aan de slag**. (Afhankelijk van uw versie van Visual Studio, de tekst kan verschillen. Sommige oudere versies hebben een **gratis starten** knop in plaats daarvan.)

4. Selecteer uw abonnement en selecteer vervolgens **Resource** > **registreren**.

5. Controleer na het toevoegen van Application Insights aan uw project, om te bevestigen dat u van de nieuwste stabiele versie van de SDK gebruikmaakt. Ga naar **Project** > **NuGet-pakketten beheren** > **Microsoft.ApplicationInsights.AspNetCore** > desgewenst kiezen **Update**.

     ![Schermafbeelding van NuGet-pakket scherm beheren met Application Insights-pakket geselecteerd voor bijwerken](./media/asp-net-core/update-nuget-package.png)

6. Als u de optionele tip gevolgd en uw project toegevoegd aan broncodebeheer u kunt vervolgens gaat u naar **weergave** > **Team Explorer** > **wijzigingen** en Selecteer elk afzonderlijk bestand voor een diff-weergave van de wijzigingen van het Application Insights telemetry toevoegen.

## <a name="enable-application-insights-server-side-telemetry-without-visual-studio"></a>Application Insights-serverzijde telemetrie (zonder Visual Studio) inschakelen

1. Installeer de [Application Insights SDK NuGet-pakket voor ASP.NET Core](https://nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore). We raden je aan het gebruik altijd de nieuwste stabiele versie. Volledige release-opmerkingen voor de SDK kunnen u vinden op de [open source-GitHub-opslagplaats](https://github.com/Microsoft/ApplicationInsights-aspnetcore/releases).

    Het volgende codefragment bevat de wijzigingen moeten worden toegevoegd aan het project `.csproj` bestand.

    ```xml
        <ItemGroup>
          <PackageReference Include="Microsoft.ApplicationInsights.AspNetCore" Version="2.6.1" />
        </ItemGroup>
    ```

2. Voeg `services.AddApplicationInsightsTelemetry();` naar de `ConfigureServices()` methode in uw `Startup` klasse. Compleet voorbeeld hieronder.

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

3. Stel de instrumentatiesleutel.

    Het is mogelijk de instrumentatiesleutel opgeven als een argument voor `AddApplicationInsightsTelemetry`, het is raadzaam dat u de instrumentatiesleutel in de configuratie opgeeft. Hieronder ziet u hoe u een instrumentatiesleutel in `appsettings.json`. Zorg ervoor dat `appsettings.json` wordt gekopieerd naar de hoofdmap van de toepassing tijdens het publiceren.

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

    U kunt ook kan de instrumentatiesleutel ook worden opgegeven in een van de volgende omgevingsvariabelen.

    APPINSIGHTS_INSTRUMENTATIONKEY

    ApplicationInsights:InstrumentationKey

    Voorbeeld:

    `SET ApplicationInsights:InstrumentationKey=putinstrumentationkeyhere`

    `SET APPINSIGHTS_INSTRUMENTATIONKEY=putinstrumentationkeyhere`

    `APPINSIGHTS_INSTRUMENTATIONKEY` wordt doorgaans gebruikt om op te geven van de instrumentatiesleutel voor toepassingen die zijn geïmplementeerd in Azure Web Apps.

    > [!NOTE]
    > Een instrumentatiesleutel in code wins opgegeven via de omgevingsvariabele `APPINSIGHTS_INSTRUMENTATIONKEY`, die wins boven de andere opties.

## <a name="run-your-application"></a>Uw toepassing uitvoeren

 Start uw toepassing en aanvragen toe. Telemetrie moet worden doorgestuurd naar Application Insights nu gestart. De volgende telemetrie wordt automatisch verzameld door de Application Insights-SDK.

|Aanvragen/afhankelijkheden |Details|
|---------------|-------|
|Aanvragen | Binnenkomende webaanvragen naar uw toepassing. |
|HTTP/Https | Aanroepen met `HttpClient`. |
|SQL | Aanroepen met `SqlClient`. |
|[Azure Storage](https://www.nuget.org/packages/WindowsAzure.Storage/) | Aanroepen met Azure Storage-Client. |
|[EventHub Client SDK](https://www.nuget.org/packages/Microsoft.Azure.EventHubs) | Versie 1.1.0 of hoger. |
|[ServiceBus Client SDK](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus)| Versie 3.0.0 en hoger. |
|Azure Cosmos DB | Alleen automatisch bijgehouden als HTTP/HTTPS wordt gebruikt. De TCP-modus wordt niet door Application Insights worden vastgelegd. |

### <a name="performance-counters"></a>Prestatiemeteritems

Ondersteuning voor [prestatiemeteritems](https://azure.microsoft.com/documentation/articles/app-insights-web-monitor-performance/) is beperkt tot het volgende in ASP.NET Core

   * SDK-versie 2.4.1 en verzamelt prestatiemeters boven als de toepassing wordt uitgevoerd in Azure Web App (Windows)
   * SDK-versie 2.7.0-beta3 en verzamelt prestatiemeters boven als de toepassing wordt uitgevoerd in Windows, en die zijn gericht op `NETSTANDARD2.0` of hoger.
   * Voor toepassingen die zijn gericht op het .NET Framework, worden prestatiemeteritems ondersteund in alle versies van de SDK.
   * In dit artikel wordt bijgewerkt wanneer prestaties teller ondersteuning in Linux wordt toegevoegd.

### <a name="ilogger-logs"></a>ILogger Logboeken

[Logboeken ILogger](https://docs.microsoft.com/azure/azure-monitor/app/ilogger) van ernst `Warning` of hoger worden automatisch opgenomen in de SDK-versie 2.7.0-beta3 of hoger.

### <a name="live-metrics"></a>Live Metrics

Het duurt een paar minuten telemetriegegevens om te beginnen met het weergegeven in de portal. Om snel te controleren of alles werkt, is het raadzaam te gebruiken [Live Metrics](https://docs.microsoft.com/azure/application-insights/app-insights-live-stream), terwijl het maken van aanvragen naar de toepassing die wordt uitgevoerd.

## <a name="enable-client-side-telemetry-for-web-applications"></a>Client-side-telemetrie inschakelen voor Web-Apps

De bovenstaande stappen zijn voldoende zijn om te beginnen met het verzamelen van telemetrie aan de serverzijde van de server. Als uw toepassing heeft de clientonderdelen, volgt u onderstaande stappen om te beginnen met het verzamelen [gebruikstelemetrie](https://docs.microsoft.com/azure/azure-monitor/app/usage-overview) van daaruit.

1. In `_ViewImports.cshtml`, injectie toevoegen:

    ```cshtml
        @inject Microsoft.ApplicationInsights.AspNetCore.JavaScriptSnippet JavaScriptSnippet
    ```

2. In `_Layout.cshtml`, HtmlHelper invoegen aan het einde van `<head>` sectie, maar voordat u een script. Een aangepaste JavaScript telemetrie die u wilt rapporteren in op de pagina moet worden toegevoegd nadat dit fragment:

    ```cshtml
        @Html.Raw(JavaScriptSnippet.FullScript)
        </head>
    ```

De `.cshtml` hierboven vermelde bestandsnamen zijn van een standaardsjabloon voor de MVC-toepassing. Om in te schakelen naar behoren client-side monitoring voor uw toepassing moet u uiteindelijk de JavaScript-fragment aanwezig zijn in de `<head>` sectie van elke pagina van uw toepassing die u wilt bewaken. Voor deze sjabloon voor toepassingen toe te voegen aan de Javascript-fragment `_Layout.cshtml` wordt effectief dit doel te bereiken. Als uw project heeft geen deze specifiek bestand kunt u nog steeds toevoegen [bewaking aan clientzijde](https://docs.microsoft.com/azure/azure-monitor/app/website-monitoring). Zou moeten aan toe te voegen JavaScript naar een equivalente-bestand die bepaalt de `<head>` van alle pagina's in uw app, of u kunt ook u het codefragment meerdere afzonderlijke pagina's als dit kunnen moeilijk zijn zou te onderhouden en is doorgaans niet kan toevoegen aanbevolen.

## <a name="configuring-application-insights-sdk"></a>Application Insights-SDK configureren

Application Insights SDK voor ASP.NET Core kan worden aangepast om te wijzigen van de standaardconfiguratie. Het is mogelijk dat gebruikers van Application Insights-SDK voor ASP.NET bekend bent met het gebruik van de configuratie van `ApplicationInsights.config`, of door het wijzigen van `TelemetryConfiguration.Active`. Configuratie wordt voor ASP.NET Core, anders uitgevoerd. De ASP.NET Core-SDK wordt toegevoegd aan de toepassing en geconfigureerd met behulp van ASP.NET Core ingebouwde [afhankelijkheidsinjectie](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection). Bijna alle wijzigingen in de configuratie worden uitgevoerd in de `ConfigureServices()` -methode van uw `Startup.cs` klasse, tenzij anders vermeld. Volg de onderstaande secties voor meer informatie.

> [!NOTE]
>  Configuratie van de wijzigen door het wijzigen van `TelemetryConfiguration.Active` wordt niet aanbevolen in ASP.NET Core-toepassingen.

### <a name="configuring-using-applicationinsightsserviceoptions"></a>Configureren met behulp van ApplicationInsightsServiceOptions

Het is mogelijk een paar algemene instellingen wijzigen door door te geven `ApplicationInsightsServiceOptions` naar `AddApplicationInsightsTelemetry`. Hieronder ziet u een voorbeeld.

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

De exacte lijst met configureerbare instellingen in `ApplicationInsightsServiceOptions` vindt [hier](https://github.com/microsoft/ApplicationInsights-aspnetcore/blob/develop/src/Microsoft.ApplicationInsights.AspNetCore/Extensions/ApplicationInsightsServiceOptions.cs).

### <a name="sampling"></a>Steekproeven

Application Insights SDK voor ASP.NET Core ondersteunt zowel FixedRate en adaptieve steekproeven. Adaptieve steekproeven is standaard ingeschakeld. Volg onze [richtlijnen voor adaptieve steekproeven](../../azure-monitor/app/sampling.md#configuring-adaptive-sampling-for-aspnet-core-applications), voor informatie over het configureren van sampling voor ASP.NET Core-toepassingen.

### <a name="adding-telemetryinitializers"></a>TelemetryInitializers toevoegen

[Telemetrie initializers](https://docs.microsoft.com/azure/azure-monitor/app/api-filtering-sampling#add-properties-itelemetryinitializer) worden gebruikt wanneer u wilt definiëren van de algemene eigenschappen die worden verzonden met alle telemetrie.

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

Aangepaste telemetrie processors kunnen worden toegevoegd aan de `TelemetryConfiguration` met behulp van de uitbreidingsmethode `AddApplicationInsightsTelemetryProcessor` op `IServiceCollection`. Telemetrie-processors worden gebruikt in [geavanceerde scenario's filteren](https://docs.microsoft.com/azure/azure-monitor/app/api-filtering-sampling#filtering-itelemetryprocessor) om toe te staan voor een meer rechtstreekse controle over wat is opgenomen of uitgesloten van de telemetrie die u naar de Application Insights-service verzenden. Gebruik het volgende voorbeeld.

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

Application Insights telemetrie-modules gebruikt als een manier om [nuttige informatie verzamelen van automatische](https://docs.microsoft.com/azure/azure-monitor/app/auto-collect-dependencies) over specifieke werkbelastingen zonder extra configuratie.

De volgende automatische verzameling modules zijn standaard ingeschakeld en zijn verantwoordelijk voor het automatisch verzamelen van telemetrie. Ze kunnen worden uitgeschakeld en geconfigureerd voor het standaardgedrag wijzigen.

* `RequestTrackingTelemetryModule`
* `DependencyTrackingTelemetryModule`
* `PerformanceCollectorModule`
* `QuickPulseTelemetryModule`
* `AppServicesHeartbeatTelemetryModule`
* `AzureInstanceMetadataTelemetryModule`

Het configureren van een standaard `TelemetryModule`, gebruikt u de uitbreidingsmethode `ConfigureTelemetryModule<T>` op `IServiceCollection` zoals wordt weergegeven in het onderstaande voorbeeld.

```csharp
using Microsoft.ApplicationInsights.DependencyCollector;
using Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector;

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
using Microsoft.ApplicationInsights.Channel;

    public void ConfigureServices(IServiceCollection services)
    {
        // use the following to replace the default channel with InMemoryChannel.
        // this can also be applied to ServerTelemetryChannel as well.
        services.AddSingleton(typeof(ITelemetryChannel), new InMemoryChannel() {MaxTelemetryBufferCapacity = 19898 });

        services.AddApplicationInsightsTelemetry();
    }
```

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="i-want-to-track-additional-telemetry-other-than-the-auto-collected-telemetry-how-do-i-do-it"></a>Ik wil om bij te houden van extra telemetrie dan de telemetrie automatisch verzameld. Hoe moet ik doen?

Een instantie van `TelemetryClient` door met behulp van Constructor-injectie, en roept u de vereiste `TrackXXX()` methode. Het is niet raadzaam maken van nieuwe `TelemetryClient` -exemplaren in ASP.NET Core-toepassing, als een singleton-instantie van `TelemetryClient` is al geregistreerd in de container DI, zodat deelt `TelemetryConfiguration` met de rest van de telemetrie. Het maken van een nieuw `TelemetryClient` exemplaar wordt alleen aanbevolen als er een afzonderlijke configuratie van de rest van de telemetrie hebben. Het volgende voorbeeld ziet hoe u kunt extra telemetrie bijhouden van een domeincontroller.

```csharp
using Microsoft.ApplicationInsights;

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

 Raadpleeg [Application Insights aangepaste metrische gegevens API-verwijzing](https://docs.microsoft.com/azure/azure-monitor/app/api-custom-events-metrics/) voor een beschrijving van aangepaste gegevens rapporteren in Application Insights.

### <a name="some-visual-studio-templates-used-useapplicationinsights-extension-method-on-iwebhostbuilder-to-enable-application-insights-is-this-usage-still-valid"></a>Sommige Visual Studio-sjablonen gebruikt UseApplicationInsights() uitbreidingsmethode op IWebHostBuilder om in te schakelen van Application Insights. Is dit gebruik nog steeds geldig?

Application Insights inschakelen met deze methode is geldig en wordt gebruikt in Visual Studio bepalen, en in de Azure-Web-App-extensies. Het wordt echter aanbevolen om te gebruiken `services.AddApplicationInsightsTelemetry()` als het overloads voor het beheren van een configuratie biedt. Beide methoden intern hetzelfde doen, dus als er is geen aangepaste configuratie worden toegepast, aanroepende is een prima.

### <a name="i-am-deploying-my-aspnet-core-application-to-azure-web-apps-should-i-still-enable-the-application-insights-extension-from-web-apps"></a>Ik ben mijn ASP.NET Core-toepassing naar Azure Web Apps implementeren. Moet ik nog steeds de Application Insights-extensie van Web-Apps inschakelen?

Als de SDK is geïnstalleerd tijdens het opbouwen, zoals wordt weergegeven in dit artikel, is er geen noodzaak voor het inschakelen van de Application Insights-extensie van de App Service-portal. Zelfs als de extensie is geïnstalleerd, wordt een back-up uitschakelen wanneer wordt gedetecteerd dat de SDK al is toegevoegd aan de toepassing. Application Insights vanuit de uitbreiding inschakelt, hoeft u installeren en bijwerken van de SDK. Application Insights inschakelen aan de hand van dit artikel is echter flexibeler om redenen die hieronder.
   * Application Insights Telemetry blijven werken:
       * Alle besturingssystemen: Windows, Linux, Mac
       * Alle modi - publiceren zelfstandig of afhankelijk zijn van het Framework.
       * Alle target-frameworks, met inbegrip van het volledige .NET Framework.
       * Alle Hosting-opties - Azure-Web-APP, virtuele machines, Linux, Containers, AKS, niet-Azure.
   * Telemetrie kan lokaal worden gezien als foutopsporing via Visual Studio.
   * Kan bijhouden met behulp van aanvullende aangepaste telemetrie `TrackXXX()` API.
   * Hebt u volledige controle over de configuratie.

### <a name="can-i-enable-application-insights-monitoring-using-tools-like-status-monitor"></a>Kan ik de bewaking van Application Insights met behulp van hulpprogramma's zoals Status Monitor inschakelen?

Nee. [Statusmonitor](https://docs.microsoft.com/azure/azure-monitor/app/monitor-performance-live-website-now) en toekomstige [Status Monitor v2](https://docs.microsoft.com/azure/azure-monitor/app/status-monitor-v2-overview) biedt momenteel ondersteuning voor ASP.NET 4.x alleen.

### <a name="i-have-an-aspnet-core-20-application-isnt-application-insights-automatically-enabled-without-me-doing-anything"></a>Ik heb een ASP.NET Core 2.0-toepassing. Is niet Application Insights automatisch ingeschakeld zonder dat ik iets doen?

`Microsoft.AspNetCore.All` 2.0 metapackage opgenomen Application Insights-SDK (versie 2.1.0), en als u de toepassing onder de foutopsporing van Visual Studio, Visual Studio Application Insights kunt en toont telemetrie lokaal in de IDE zelf. Telemetrie is niet verzonden naar de Application Insights-service, tenzij een instrumentatiesleutel expliciet is opgegeven. Volg de instructies in dit artikel voor het inschakelen van Application Insights, zelfs voor 2.0 wordt aangeraden apps.

### <a name="i-run-my-application-in-linux-are-all-features-supported-in-linux-as-well"></a>Kan ik mijn toepassing in Linux uitgevoerd. Zijn alle functies in Linux ook ondersteund?

* Ja. Functies die worden ondersteund voor de SDK is dezelfde op alle platforms, met de volgende uitzonderingen:

    * Prestatiemeteritems zijn nog niet ondersteund in niet-Windows.
    * Hoewel `ServerTelemetryChannel` is ingeschakeld standaard, als de toepassing wordt uitgevoerd in Linux of MacOS, het kanaal maakt niet automatisch een map met lokale opslag om telemetrie tijdelijk als er netwerkproblemen zijn. Deze beperking zorgt ervoor dat telemetrie verloren als er tijdelijk netwerkfout of problemen met de server. De oplossing voor dit probleem is voor de gebruiker het configureren van een lokale map voor het kanaal, zoals hieronder wordt weergegeven.

```csharp
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;

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

## <a name="open-source-sdk"></a>Open-source-SDK
[Lees- en dragen bij aan de code](https://github.com/Microsoft/ApplicationInsights-aspnetcore#recent-updates).

## <a name="video"></a>Video

- Externe stapsgewijze video over [Application Insights configureren met .NET Core en Visual Studio](https://www.youtube.com/watch?v=NoS9UhcR4gA&t) helemaal.

## <a name="next-steps"></a>Volgende stappen
* [Verken Gebruikersstromen](../../azure-monitor/app/usage-flows.md) om te begrijpen hoe gebruikers navigeren via uw app.
* [Momentopname verzamelen configureren](https://docs.microsoft.com/azure/application-insights/app-insights-snapshot-debugger) om te zien van de status van de broncode en variabelen op dit moment wordt er een uitzondering gegenereerd.
* [Gebruik de API](../../azure-monitor/app/api-custom-events-metrics.md) voor het verzenden van uw eigen gebeurtenissen en metrische gegevens voor een gedetailleerdere weergave van de prestaties en het gebruik van uw app.
* Gebruik [beschikbaarheidstests](../../azure-monitor/app/monitor-web-app-availability.md) om te controleren of uw app continu van over de hele wereld.
