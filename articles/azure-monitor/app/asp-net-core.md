---
title: Azure Application Insights voor ASP.NET Core-toepassingen | Microsoft Docs
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
ms.date: 05/22/2019
ms.author: mbullwin
ms.openlocfilehash: 7fe5a4f5a5d1d254918f1b4f997acfb9cf67a75b
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67272438"
---
# <a name="application-insights-for-aspnet-core-applications"></a>Application Insights voor ASP.NET Core-toepassingen

In dit artikel wordt beschreven hoe u Application Insights voor een [ASP.NET Core](https://docs.microsoft.com/aspnet/core) toepassing. Wanneer u de instructies in dit artikel hebt voltooid, verzamelt Application Insights aanvragen, afhankelijkheden, uitzonderingen, prestatiemeteritems, heartbeats en logboeken van uw ASP.NET Core-toepassing. 

Het voorbeeld we hier gebruiken is een [MVC-toepassing](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app) die is gericht op `netcoreapp2.2`. U kunt deze instructies toepassen op alle ASP.NET Core-toepassingen.

## <a name="supported-scenarios"></a>Ondersteunde scenario's

De [Application Insights SDK voor ASP.NET Core](https://nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) kunt bewaken van uw toepassingen, ongeacht waar of hoe ze worden uitgevoerd. Als uw toepassing wordt uitgevoerd en netwerkverbinding met Azure heeft, kan telemetriegegevens worden verzameld. Application Insights-controle wordt ondersteund overal .NET Core wordt ondersteund. Ondersteuning omvat:
* **Besturingssysteem**: Windows, Linux of Mac.
* **Methode hosting**: In proces of buiten het proces. 
* **Implementatiemethode**: Framework afhankelijke of zelfstandig.
* **Webserver**: IIS (Internet informatieserver) of Kestrel. 
* **Hostingplatform**: De functie Web Apps van Azure App Service, Azure-VM, Docker, Azure Kubernetes Service (AKS), enzovoort.
* **IDE**: Visual Studio, Visual Studio Code of vanaf de opdrachtregel.

## <a name="prerequisites"></a>Vereisten

- Een werkende ASP.NET Core-toepassing. Als u maken van een ASP.NET Core-toepassing wilt, volgt u deze [zelfstudie voor ASP.NET Core](https://docs.microsoft.com/aspnet/core/getting-started/).
- Een geldige Application Insights-instrumentatiesleutel. Deze sleutel is vereist voor alle telemetrie wordt verzonden naar Application Insights. Als u wilt maken van een nieuwe Application Insights-resource om op te halen van een sleutel, Zie instrumentation [maken van een Application Insights-resource](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource).

## <a name="enable-application-insights-server-side-telemetry-visual-studio"></a>Application Insights-serverzijde telemetrie (Visual Studio) inschakelen

1. Open het project in Visual Studio.

    > [!TIP]
    > Als u wilt, kunt u instellen van broncodebeheer voor uw project zodat u de wijzigingen die Application Insights kunt bijhouden. Als broncodebeheer, schakelt **bestand** > **Add to Source Control**.

2. Selecteer **Project** > **Application Insights Telemetry toevoegen**.

3. Selecteer **aan de slag**. Tekst van deze selectie kan variëren, afhankelijk van uw versie van Visual Studio. Sommige oudere versies gebruiken een **gratis starten** knop in plaats daarvan.

4. Selecteer uw abonnement. Selecteer vervolgens **Resource** > **registreren**.

5. Controleer na het toevoegen van Application Insights aan uw project, om te bevestigen dat u de nieuwste stabiele versie van de SDK. Ga naar **Project** > **NuGet-pakketten beheren** > **Microsoft.ApplicationInsights.AspNetCore**. Als u wilt, kiest u **Update**.

     ![Schermopname die laat zien waar het pakket Application Insights voor update selecteren](./media/asp-net-core/update-nuget-package.png)

6. Als u de optionele tip gevolgd en uw project toegevoegd aan broncodebeheer, gaat u naar **weergave** > **Team Explorer** > **wijzigingen**. Selecteer vervolgens elk bestand om een diff-weergave van de wijzigingen van Application Insights telemetry te zien.

## <a name="enable-application-insights-server-side-telemetry-no-visual-studio"></a>Application Insights-serverzijde telemetrie (geen Visual Studio) inschakelen

1. Installeer de [Application Insights SDK NuGet-pakket voor ASP.NET Core](https://nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore). U wordt aangeraden dat u altijd de nieuwste stabiele versie gebruiken. Opmerkingen bij de volledige release vinden voor de SDK op de [GitHub-opslagplaats van open-source](https://github.com/Microsoft/ApplicationInsights-aspnetcore/releases).

    Het volgende codevoorbeeld ziet u de wijzigingen moeten worden toegevoegd aan het project `.csproj` bestand.

    ```xml
        <ItemGroup>
          <PackageReference Include="Microsoft.ApplicationInsights.AspNetCore" Version="2.7.0" />
        </ItemGroup>
    ```

2. Voeg `services.AddApplicationInsightsTelemetry();` naar de `ConfigureServices()` methode in uw `Startup` klasse, zoals in dit voorbeeld:

    ```csharp
        // This method gets called by the runtime. Use this method to add services to the container.
        public void ConfigureServices(IServiceCollection services)
        {
            // The following line enables Application Insights telemetry collection.
            services.AddApplicationInsightsTelemetry();
    
            // This code adds other services for your application.
            services.AddMvc();
        }
    ```

3. Stel de instrumentatiesleutel.

    Hoewel u kunt de instrumentatiesleutel opgeven als een argument voor `AddApplicationInsightsTelemetry`, het is raadzaam dat u de instrumentatiesleutel in de configuratie opgeeft. Het volgende codevoorbeeld laat zien hoe u een instrumentatiesleutel in `appsettings.json`. Zorg ervoor dat `appsettings.json` wordt gekopieerd naar de hoofdmap van de toepassing tijdens het publiceren.

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

    U kunt ook de instrumentatiesleutel in een van de volgende omgevingsvariabelen opgeven:

    * `APPINSIGHTS_INSTRUMENTATIONKEY`

    * `ApplicationInsights:InstrumentationKey`

    Bijvoorbeeld:

    * `SET ApplicationInsights:InstrumentationKey=putinstrumentationkeyhere`

    * `SET APPINSIGHTS_INSTRUMENTATIONKEY=putinstrumentationkeyhere`

    Normaal gesproken `APPINSIGHTS_INSTRUMENTATIONKEY` Hiermee geeft u de instrumentatiesleutel voor toepassingen die zijn geïmplementeerd in Web Apps.

    > [!NOTE]
    > Een instrumentatiesleutel in code wins opgegeven via de omgevingsvariabele `APPINSIGHTS_INSTRUMENTATIONKEY`, die wins boven de andere opties.

## <a name="run-your-application"></a>Uw toepassing uitvoeren

Start uw toepassing en aanvragen toe. Telemetrie moet nu stromen naar Application Insights. De Application Insights SDK verzamelt automatisch de volgende telemetrie.

|Aanvragen/afhankelijkheden |Details|
|---------------|-------|
|Aanvragen | Binnenkomende webaanvragen naar uw toepassing. |
|HTTP of HTTPS | Aanroepen met `HttpClient`. |
|SQL | Aanroepen met `SqlClient`. |
|[Azure Storage](https://www.nuget.org/packages/WindowsAzure.Storage/) | Aanroepen met de Azure Storage-client. |
|[EventHubs-client-SDK](https://www.nuget.org/packages/Microsoft.Azure.EventHubs) | Versie 1.1.0 of hoger. |
|[ServiceBus client SDK](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus)| Versie 3.0.0 en hoger. |
|Azure Cosmos DB | Automatisch bijgehouden alleen als u HTTP/HTTPS gebruikt. Application Insights niet TCP-modus voor vastleggen. |

### <a name="performance-counters"></a>Prestatiemeteritems

Ondersteuning voor [prestatiemeteritems](https://azure.microsoft.com/documentation/articles/app-insights-web-monitor-performance/) in ASP.NET Core is beperkt:

   * SDK-versies 2.4.1 en later verzamelen van prestatiemeteritems als de toepassing wordt uitgevoerd in Web-Apps (Windows).
   * SDK-versies 2.7.0-beta3 en hoger verzamelen prestaties items als de toepassing wordt uitgevoerd in Windows en doelen `NETSTANDARD2.0` of hoger.
   * Voor toepassingen die zijn gericht op het .NET Framework, ondersteuning voor alle versies van de SDK van prestatiemeteritems.
 
In dit artikel wordt bijgewerkt wanneer prestaties teller ondersteuning in Linux wordt toegevoegd.

### <a name="ilogger-logs"></a>ILogger Logboeken

[Logboeken ILogger](https://docs.microsoft.com/azure/azure-monitor/app/ilogger) van ernst `Warning` of hoger worden automatisch opgenomen in de SDK-versies 2.7.0-beta3 en hoger.

### <a name="live-metrics"></a>Live metrics Stream

Het duurt een paar minuten voordat telemetrie wordt weergegeven in de portal. Als u wilt snel controleren of alles werkt, is het raadzaam te gebruiken [Live Metrics](https://docs.microsoft.com/azure/application-insights/app-insights-live-stream) wanneer u aanvragen versturen naar de toepassing die wordt uitgevoerd.

## <a name="enable-client-side-telemetry-for-web-applications"></a>Telemetrie aan de clientzijde voor webtoepassingen inschakelen

De voorgaande stappen zijn voldoende zijn om te beginnen met het verzamelen van telemetrie aan de serverzijde van de server. Als uw toepassing heeft de clientonderdelen, volgt u de volgende stappen om te beginnen met het verzamelen [gebruikstelemetrie](https://docs.microsoft.com/azure/azure-monitor/app/usage-overview).

1. In `_ViewImports.cshtml`, injectie toevoegen:

    ```cshtml
        @inject Microsoft.ApplicationInsights.AspNetCore.JavaScriptSnippet JavaScriptSnippet
    ```

2. In `_Layout.cshtml`, invoegen `HtmlHelper` aan het einde van de `<head>` sectie, maar voordat u een script. Als u wilt voor het rapporteren van eventuele aangepaste JavaScript telemetrie van de pagina, kunt u deze na dit fragment invoeren:

    ```cshtml
        @Html.Raw(JavaScriptSnippet.FullScript)
        </head>
    ```

De `.cshtml` bestandsnamen waarnaar wordt verwezen eerder afkomstig zijn van een standaardsjabloon voor de MVC-toepassing. Uiteindelijk, als u goed inschakelen bewaking aan clientzijde voor uw toepassing wilt, het JavaScript-fragment moet worden weergegeven de `<head>` sectie van elke pagina van uw toepassing die u wilt bewaken. Kunt u dit doel voor deze toepassingssjabloon uitvoeren door toe te voegen aan de JavaScript-fragment `_Layout.cshtml`. 

Als uw project bevat geen `_Layout.cshtml`, u kunt nog steeds toevoegen [bewaking aan clientzijde](https://docs.microsoft.com/azure/azure-monitor/app/website-monitoring). U kunt dit doen door het JavaScript-fragment toe te voegen aan een gelijkwaardig-bestand dat bepaalt de `<head>` aan alle pagina's in uw app. Of u kunt het codefragment toevoegen aan meerdere pagina's, maar deze oplossing is moeilijk te onderhouden en wordt in het algemeen niet aanbevolen.

## <a name="configure-the-application-insights-sdk"></a>De Application Insights-SDK configureren

U kunt de Application Insights-SDK voor ASP.NET Core de standaardconfiguratie te wijzigen. Het is mogelijk dat gebruikers van de Application Insights-SDK ASP.NET bekend bent met het wijzigen van de configuratie met behulp van `ApplicationInsights.config` of door het wijzigen van `TelemetryConfiguration.Active`. U configuratie anders voor ASP.NET Core. De ASP.NET Core-SDK toevoegen aan de toepassing en deze configureren met behulp van ASP.NET Core ingebouwde [afhankelijkheidsinjectie](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection). Bijna alle configuratiewijzigingen in de `ConfigureServices()` -methode van uw `Startup.cs` klasse, tenzij u anders bent omgeleid. De volgende secties bieden meer informatie.

> [!NOTE]
> In ASP.NET Core-toepassingen, wijzigen van de configuratie door het wijzigen van `TelemetryConfiguration.Active` wordt niet aanbevolen.

### <a name="using-applicationinsightsserviceoptions"></a>Met behulp van ApplicationInsightsServiceOptions

U kunt een paar algemene instellingen wijzigen door te geven `ApplicationInsightsServiceOptions` naar `AddApplicationInsightsTelemetry`, zoals in dit voorbeeld:

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

Zie voor meer informatie de [configureerbare instellingen in `ApplicationInsightsServiceOptions` ](https://github.com/microsoft/ApplicationInsights-aspnetcore/blob/develop/src/Microsoft.ApplicationInsights.AspNetCore/Extensions/ApplicationInsightsServiceOptions.cs).

### <a name="sampling"></a>Steekproeven

De Application Insights-SDK voor ASP.NET Core ondersteunt zowel vast tarief als adaptieve steekproeven. Adaptieve steekproeven is standaard ingeschakeld. 

Zie voor meer informatie, [adaptieve steekproeven voor ASP.NET Core-toepassingen configureren](../../azure-monitor/app/sampling.md#configuring-adaptive-sampling-for-aspnet-core-applications).

### <a name="adding-telemetryinitializers"></a>TelemetryInitializers toevoegen

Gebruik [telemetrie initializers](https://docs.microsoft.com/azure/azure-monitor/app/api-filtering-sampling#add-properties-itelemetryinitializer) indien u wenst te definiëren van de algemene eigenschappen die worden verzonden met alle telemetrie.

Voeg een nieuwe `TelemetryInitializer` naar de `DependencyInjection` container zoals wordt weergegeven in de volgende code. De SDK kiest automatisch een `TelemetryInitializer` die wordt toegevoegd aan de `DependencyInjection` container.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddSingleton<ITelemetryInitializer, MyCustomTelemetryInitializer>();
    }
```

### <a name="removing-telemetryinitializers"></a>Removing TelemetryInitializers

Telemetrie initializers zijn standaard aanwezig. Alle te verwijderen of specifieke telemetrie initializers, gebruikt u de volgende voorbeeldcode *nadat* u aanroepen `AddApplicationInsightsTelemetry()`.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddApplicationInsightsTelemetry();

        // Remove a specific built-in telemetry initializer
        var tiToRemove = services.FirstOrDefault<ServiceDescriptor>
                         (t => t.ImplementationType == typeof(AspNetCoreEnvironmentTelemetryInitializer));
        if (tiToRemove != null)
        {
            services.Remove(tiToRemove);
        }

        // Remove all initializers
        // This requires importing namespace by using Microsoft.Extensions.DependencyInjection.Extensions;
        services.RemoveAll(typeof(ITelemetryInitializer));
    }
```

### <a name="adding-telemetry-processors"></a>Telemetrie-processors toe te voegen

U kunt aangepaste telemetrie processors aan toevoegen `TelemetryConfiguration` met behulp van de uitbreidingsmethode `AddApplicationInsightsTelemetryProcessor` op `IServiceCollection`. U telemetrie processors in [geavanceerde scenario's filteren](https://docs.microsoft.com/azure/azure-monitor/app/api-filtering-sampling#filtering-itelemetryprocessor) om toe te staan voor een meer rechtstreekse controle over wat is opgenomen of uitgesloten van de telemetrie die u naar de Application Insights-service verzenden. Gebruik het volgende voorbeeld.

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

Application Insights maakt gebruik van modules telemetrie [automatisch verzamelen nuttige informatie](https://docs.microsoft.com/azure/azure-monitor/app/auto-collect-dependencies) over specifieke werkbelastingen zonder extra configuratie.

De volgende automatische-verzameling-modules zijn standaard ingeschakeld. Deze modules zijn verantwoordelijk voor het automatisch verzamelen van telemetrie. U kunt uitschakelen of configureren zodat ze hun standaardgedrag aanpassen.

* `RequestTrackingTelemetryModule`
* `DependencyTrackingTelemetryModule`
* `PerformanceCollectorModule`
* `QuickPulseTelemetryModule`
* `AppServicesHeartbeatTelemetryModule`
* `AzureInstanceMetadataTelemetryModule`

Het configureren van een standaard `TelemetryModule`, gebruikt u de uitbreidingsmethode `ConfigureTelemetryModule<T>` op `IServiceCollection`, zoals wordt weergegeven in het volgende voorbeeld.

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

### <a name="configuring-a-telemetry-channel"></a>Configureren van een telemetrie-kanaal

Het standaardkanaal is `ServerTelemetryChannel`. U kunt deze negeren als het volgende voorbeeld wordt.

```csharp
using Microsoft.ApplicationInsights.Channel;

    public void ConfigureServices(IServiceCollection services)
    {
        // Use the following to replace the default channel with InMemoryChannel.
        // This can also be applied to ServerTelemetryChannel.
        services.AddSingleton(typeof(ITelemetryChannel), new InMemoryChannel() {MaxTelemetryBufferCapacity = 19898 });

        services.AddApplicationInsightsTelemetry();
    }
```

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="how-can-i-track-telemetry-thats-not-automatically-collected"></a>Hoe kan ik de telemetrie die niet automatisch worden verzameld bijhouden?

Een exemplaar van `TelemetryClient` door met behulp van constructor-injectie, en roept u de vereiste `TrackXXX()` methode. U kunt beter geen maken van nieuwe `TelemetryClient` -exemplaren in een ASP.NET Core-toepassing. Een singleton-instantie van `TelemetryClient` is al geregistreerd in de `DependencyInjection` container, zodat deelt `TelemetryConfiguration` met de rest van de telemetrie. Het maken van een nieuw `TelemetryClient` exemplaar wordt alleen aanbevolen als een configuratie die is gescheiden van de rest van de telemetrie. 

Het volgende voorbeeld ziet hoe u kunt extra telemetrie bijhouden van een domeincontroller.

```csharp
using Microsoft.ApplicationInsights;

public class HomeController : Controller
{
    private TelemetryClient telemetry;

    // Use constructor injection to get a TelemetryClient instance.
    public HomeController(TelemetryClient telemetry)
    {
        this.telemetry = telemetry;
    }

    public IActionResult Index()
    {
        // Call the required TrackXXX method.
        this.telemetry.TrackEvent("HomePageRequested");
        return View();
    }
```

Zie voor meer informatie over aangepaste gegevensrapportage in Application Insights, [Application Insights aangepaste metrische gegevens API-verwijzing](https://docs.microsoft.com/azure/azure-monitor/app/api-custom-events-metrics/).

### <a name="some-visual-studio-templates-used-the-useapplicationinsights-extension-method-on-iwebhostbuilder-to-enable-application-insights-is-this-usage-still-valid"></a>Sommige Visual Studio-sjablonen gebruikt de uitbreidingsmethode UseApplicationInsights() op IWebHostBuilder om in te schakelen van Application Insights. Is dit gebruik nog steeds geldig?

Ja, inschakelen van Application Insights met deze methode is ongeldig. Deze techniek wordt gebruikt in Visual Studio-onboarding en in de Web-Apps-extensies. We raden u echter aan met behulp van `services.AddApplicationInsightsTelemetry()` omdat deze overloads voor het beheren van een configuratie biedt. Beide methoden doen hetzelfde intern, dus als u niet dat aangepaste configuratie toe te passen wilt, kunt u een van beide methoden aanroepen.

### <a name="im-deploying-my-aspnet-core-application-to-web-apps-should-i-still-enable-the-application-insights-extension-from-web-apps"></a>Ik ben mijn toepassing ASP.NET Core Web-Apps implementeren. Moet ik nog steeds de Application Insights-extensie van Web-Apps inschakelen?

Als de SDK is geïnstalleerd tijdens het opbouwen, zoals wordt weergegeven in dit artikel, moet u niet de extensie Application Insights vanuit de portal voor App Service inschakelen. Zelfs als de extensie is geïnstalleerd, wordt een back-up uitschakelen wanneer wordt gedetecteerd dat de SDK al is toegevoegd aan de toepassing. Als u Application Insights via de extensie inschakelen, hoeft u niet te installeren en bijwerken van de SDK. Maar als u Application Insights met de instructies in dit artikel inschakelen, hebt u omdat meer flexibiliteit:

   * Application Insights telemetry blijven werken:
       * Alle besturingssystemen, waaronder Windows, Linux en Mac.
       * Alle publiceren modi, met inbegrip van zelfstandige of framework afhankelijke.
       * Alle target-frameworks, met inbegrip van het volledige .NET Framework.
       * Alle hostingopties, waaronder Web-Apps, virtuele machines, Linux, containers, Azure Kubernetes Service en niet-Azure die als host fungeert.
   * U kunt telemetrie lokaal zien wanneer u fouten vanuit Visual Studio opspoort.
   * U kunt aanvullende aangepaste telemetrie volgen met behulp van de `TrackXXX()` API.
   * Hebt u volledige controle over de configuratie.

### <a name="can-i-enable-application-insights-monitoring-by-using-tools-like-status-monitor"></a>Kan ik de bewaking van Application Insights met behulp van hulpprogramma's zoals Status Monitor inschakelen?

Nee. [Statusmonitor](https://docs.microsoft.com/azure/azure-monitor/app/monitor-performance-live-website-now) en [Status Monitor v2](https://docs.microsoft.com/azure/azure-monitor/app/status-monitor-v2-overview) momenteel ondersteuning voor ASP.NET 4.x alleen.

### <a name="is-application-insights-automatically-enabled-for-my-aspnet-core-20-application"></a>Application Insights automatisch ingeschakeld voor mijn ASP.NET Core 2.0-toepassing?

De `Microsoft.AspNetCore.All` 2.0 metapackage opgenomen Application Insights-SDK (versie 2.1.0). Als u de toepassing onder de foutopsporingsfunctie uitvoert, wordt Visual Studio Application Insights kunt en toont telemetrie lokaal in de IDE zelf. Telemetrie is niet verzonden naar de Application Insights-service, tenzij een instrumentatiesleutel is opgegeven. Volg de instructies in dit artikel voor het inschakelen van Application Insights, zelfs voor 2.0 wordt aangeraden apps.

### <a name="if-i-run-my-application-in-linux-are-all-features-supported"></a>Als ik mijn toepassing in Linux wordt uitgevoerd, worden alle functies dan ondersteund?

Ja. Functies die worden ondersteund voor de SDK is hetzelfde als in alle platforms, met de volgende uitzonderingen:

* Prestatiemeteritems worden alleen ondersteund in Windows.
* Hoewel `ServerTelemetryChannel` is ingeschakeld als de toepassing wordt uitgevoerd in Linux of MacOS, het kanaal niet automatisch maakt standaard een map met lokale opslag om telemetrie tijdelijk als er netwerkproblemen zijn. Telemetrie is vanwege deze beperking, gaan verloren als er tijdelijke problemen met netwerk of de server zijn. Configureer een lokale map voor het kanaal om dit probleem omzeilen:

```csharp
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;

    public void ConfigureServices(IServiceCollection services)
    {
        // The following will configure the channel to use the given folder to temporarily
        // store telemetry items during network or Application Insights server issues.
        // User should ensure that the given folder already exists
        // and that the application has read/write permissions.
        services.AddSingleton(typeof(ITelemetryChannel),
                                new ServerTelemetryChannel () {StorageFolder = "/tmp/myfolder"});
        services.AddApplicationInsightsTelemetry();
    }
```

## <a name="open-source-sdk"></a>Open-source-SDK

[Lees- en dragen bij aan de code](https://github.com/Microsoft/ApplicationInsights-aspnetcore#recent-updates).

## <a name="video"></a>Video

- Bekijk deze externe stapsgewijze video voor [Application Insights configureren met .NET Core en Visual Studio](https://www.youtube.com/watch?v=NoS9UhcR4gA&t) helemaal.
- Bekijk deze externe stapsgewijze video voor [Application Insights configureren met .NET Core en Visual Studio Code](https://youtu.be/ygGt84GDync) helemaal.

## <a name="next-steps"></a>Volgende stappen

* [Verken gebruikersstromen](../../azure-monitor/app/usage-flows.md) om te begrijpen hoe gebruikers navigeren via uw app.
* [Configureren van een verzameling momentopname](https://docs.microsoft.com/azure/application-insights/app-insights-snapshot-debugger) om te zien van de status van de broncode en variabelen op dit moment wordt er een uitzondering gegenereerd.
* [Gebruik de API](../../azure-monitor/app/api-custom-events-metrics.md) voor het verzenden van uw eigen gebeurtenissen en metrische gegevens voor een gedetailleerde weergave van de prestaties en het gebruik van uw app.
* Gebruik [beschikbaarheidstests](../../azure-monitor/app/monitor-web-app-availability.md) om te controleren of uw app continu van over de hele wereld.
