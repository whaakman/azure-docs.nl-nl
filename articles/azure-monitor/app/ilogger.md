---
title: .NET-traceerlogboeken in Azure Application Insights met ILogger verkennen
description: Voorbeelden van het gebruik van de provider Azure Application Insights ILogger met ASP.NET Core-en consoletoepassingen.
services: application-insights
author: cijothomas
manager: carmonm
ms.service: application-insights
ms.topic: conceptual
ms.date: 02/19/2019
ms.reviewer: mbullwin
ms.author: cithomas
ms.openlocfilehash: ca842ce46a58dafa87581b77bcbd802191f7fcd1
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/09/2019
ms.locfileid: "65511031"
---
# <a name="applicationinsightsloggerprovider-for-net-core-ilogger-logs"></a>ApplicationInsightsLoggerProvider voor .NET Core ILogger Logboeken

Een API voor logboekregistratie die geschikt is voor verschillende soorten ingebouwde en door de derde partij logboekregistratie providers biedt ondersteuning voor ASP.NET Core. Logboekregistratie vindt plaats door het aanroepen van **Log()** of een variant van op *ILogger* exemplaren. In dit artikel ziet u hoe u *ApplicationInsightsLoggerProvider* om vast te leggen ILogger Logboeken in de console en ASP.NET Core-toepassingen. In dit artikel beschrijft ook hoe ApplicationInsightsLoggerProvider kan worden geïntegreerd met andere telemetrie Application Insights.
Zie voor meer informatie, [logboekregistratie in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging).

## <a name="aspnet-core-applications"></a>ASP.NET Core-toepassingen

ApplicationInsightsLoggerProvider is standaard ingeschakeld in [Microsoft.ApplicationInsights.AspNet SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) versie 2.7.0-beta3 (en hoger) wanneer u regelmatig Application Insights-bewaking via een van de standaard inschakelen methoden:
- Door het aanroepen van de **UseApplicationInsights** uitbreidingsmethode op IWebHostBuilder 
- Door het aanroepen van de **AddApplicationInsightsTelemetry** uitbreidingsmethode op IServiceCollection

ILogger-logboeken die ApplicationInsightsLoggerProvider worden vastgelegd, gelden dezelfde configuratie als geen andere telemetriegegevens die worden verzameld. Ze hebben dezelfde set TelemetryInitializers en TelemetryProcessors, gebruikt u de dezelfde TelemetryChannel worden gecorreleerd en voorbeelden worden gemaakt op dezelfde manier als andere telemetrie. Als u versie 2.7.0-beta3 of hoger, hoeft u geen actie is vereist om vast te leggen ILogger Logboeken.

Alleen *waarschuwing* of hoger ILogger Logboeken (uit alle categorieën) worden standaard verzonden naar Application Insights. Maar u kunt de [toepassen van filters voor het wijzigen van dit gedrag](#control-logging-level). Er zijn extra stappen vereist om vast te leggen ILogger logboeken van **Program.cs** of **Startup.cs**. (Zie [ILogger vastleggen van Logboeken van Startup.cs en Program.cs in ASP.NET Core-toepassingen](#capture-ilogger-logs-from-startupcs-and-programcs-in-aspnet-core-apps).)

Als u een eerdere versie van Microsoft.ApplicationInsights.AspNet SDK gebruiken of u gebruiken net ApplicationInsightsLoggerProvider wilt zonder eventuele andere Application Insights-bewaking, gebruikt u de volgende procedure:

1. Installeer het NuGet-pakket:

   ```xml
       <ItemGroup>
         <PackageReference Include="Microsoft.Extensions.Logging.ApplicationInsights" Version="2.9.1" />  
       </ItemGroup>
   ```

1. Wijzigen **Program.cs** zoals hier wordt weergegeven:

   ```csharp
   using Microsoft.AspNetCore;
   using Microsoft.AspNetCore.Hosting;
   using Microsoft.Extensions.Logging;

   public class Program
   {
       public static void Main(string[] args)
       {
           CreateWebHostBuilder(args).Build().Run();
       }

       public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
         WebHost.CreateDefaultBuilder(args)
           .UseStartup<Startup>()
         .ConfigureLogging(
               builder =>
               {
                   // Providing an instrumentation key here is required if you're using
                   // standalone package Microsoft.Extensions.Logging.ApplicationInsights
                   // or if you want to capture logs from early in the application startup
                   // pipeline from Startup.cs or Program.cs itself.
                   builder.AddApplicationInsights("ikey");

                   // Optional: Apply filters to control what logs are sent to Application Insights.
                   // The following configures LogLevel Information or above to be sent to
                   // Application Insights for all categories.
                   builder.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                                    ("", LogLevel.Information);
               }
           );
   }
   ```

De code in stap 2 wordt geconfigureerd `ApplicationInsightsLoggerProvider`. De volgende code toont een voorbeeld van de controllerklasse, dat gebruikmaakt van `ILogger` om Logboeken te verzenden. De logboeken worden vastgelegd door Application Insights.

```csharp
public class ValuesController : ControllerBase
{
    private readonly `ILogger` _logger;

    public ValuesController(ILogger<ValuesController> logger)
    {
        _logger = logger;
    }

    // GET api/values
    [HttpGet]
    public ActionResult<IEnumerable<string>> Get()
    {
        // All the following logs will be picked up by Application Insights.
        // and all of them will have ("MyKey", "MyValue") in Properties.
        using (_logger.BeginScope(new Dictionary<string, object> { { "MyKey", "MyValue" } }))
            {
                _logger.LogWarning("An example of a Warning trace..");
                _logger.LogError("An example of an Error level message");
            }
        return new string[] { "value1", "value2" };
    }
}
```

### <a name="capture-ilogger-logs-from-startupcs-and-programcs-in-aspnet-core-apps"></a>Vastleggen van Logboeken van de ILogger van Startup.cs en Program.cs in ASP.NET Core-apps

De nieuwe ApplicationInsightsLoggerProvider kunt vastleggen van Logboeken van al vroeg in de pijplijn starten van de toepassing. Hoewel ApplicationInsightsLoggerProvider automatisch in Application Insights (vanaf versie 2.7.0-beta3) is ingeschakeld, heeft deze een instrumentatiesleutel instellen tot verderop in de pijplijn geen. Dus alleen logboeken van **Controller**/ andere klassen wordt vastgelegd. Om vast te leggen van elke log beginnen met **Program.cs** en **Startup.cs** zelf, moet u expliciet inschakelen een instrumentatiesleutel voor ApplicationInsightsLoggerProvider. Bovendien *TelemetryConfiguration* is niet volledig ingesteld wanneer u zich vanaf aanmeldt **Program.cs** of **Startup.cs** zelf. Deze logboeken moet dus een minimale configuratie die gebruikmaakt van InMemoryChannel, geen steekproeven te nemen, en er is geen standaardtelemetrie voor initializers of de processors.

De volgende voorbeelden ziet u deze mogelijkheid met **Program.cs** en **Startup.cs**.

#### <a name="example-programcs"></a>Voorbeeld van Program.cs

```csharp
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Logging;

public class Program
{
    public static void Main(string[] args)
    {
        var host = CreateWebHostBuilder(args).Build();
        var logger = host.Services.GetRequiredService<ILogger<Program>>();
        // This will be picked up by AI
        logger.LogInformation("From Program. Running the host now..");
        host.Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureLogging(
        builder =>
            {
            // Providing an instrumentation key here is required if you're using
            // standalone package Microsoft.Extensions.Logging.ApplicationInsights
            // or if you want to capture logs from early in the application startup 
            // pipeline from Startup.cs or Program.cs itself.
            builder.AddApplicationInsights("ikey");

            // Adding the filter below to ensure logs of all severity from Program.cs
            // is sent to ApplicationInsights.
            builder.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                             (typeof(Program).FullName, LogLevel.Trace);

            // Adding the filter below to ensure logs of all severity from Startup.cs
            // is sent to ApplicationInsights.
            builder.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                             (typeof(Startup).FullName, LogLevel.Trace);
            }
        );
}
```

#### <a name="example-startupcs"></a>Voorbeeld Startup.cs

```csharp
public class Startup
{
    private readonly `ILogger` _logger;

    public Startup(IConfiguration configuration, ILogger<Startup> logger)
    {
        Configuration = configuration;
        _logger = logger;
    }

    public IConfiguration Configuration { get; }

    // This method gets called by the runtime. Use this method to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddApplicationInsightsTelemetry();

        // The following will be picked up by Application Insights.
        _logger.LogInformation("Logging from ConfigureServices.");
    }

    // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        if (env.IsDevelopment())
        {
            // The following will be picked up by Application Insights.
            _logger.LogInformation("Configuring for Development environment");
            app.UseDeveloperExceptionPage();
        }
        else
        {
            // The following will be picked up by Application Insights.
            _logger.LogInformation("Configuring for Production environment");
        }

        app.UseMvc();
    }
}
```

## <a name="migrate-from-the-old-applicationinsightsloggerprovider"></a>Migreren van de oude ApplicationInsightsLoggerProvider

Microsoft.ApplicationInsights.AspNet SDK-versies voordat 2.7.0-beta2 ondersteund een provider voor logboekregistratie die is nu verouderd. Deze provider is ingeschakeld via de **AddApplicationInsights()** uitbreidingsmethode van ILoggerFactory. U wordt aangeraden dat u migreren naar de nieuwe provider, die bestaat uit twee stappen:

1. Verwijder de *ILoggerFactory.AddApplicationInsights()* aanroepen vanuit de **Startup.Configure()** methode om te voorkomen dat dubbele logboekregistratie.
2. Pas geen filters gebruiken om regels in de code, omdat ze niet door de nieuwe provider worden gerespecteerd. Overloads van *ILoggerFactory.AddApplicationInsights()* duurde minimale LogLevel of filter voor de functies. Met de nieuwe provider is filteren onderdeel van het framework voor logboekregistratie zelf. Het wordt niet uitgevoerd door de Application Insights-provider. Zodat de filters die worden geleverd via *ILoggerFactory.AddApplicationInsights()* overloads moeten worden verwijderd. En filterregels moet worden opgegeven door de [bepalen het logboekregistratieniveau van](#control-logging-level) instructies. Als u *appsettings.json* om te filteren logboekregistratie, het blijven werken met de nieuwe provider omdat beide dezelfde provider alias, gebruik *ApplicationInsights*.

U kunt nog steeds de oude provider gebruiken. (Deze wordt verwijderd in een wijziging van de primaire versie 3. *xx*.) Maar het is raadzaam dat u naar de nieuwe provider voor de volgende redenen migreert:

- De voorgaande provider beschikt niet over de ondersteuning voor [melden scopes](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.2#log-scopes). In de nieuwe provider worden eigenschappen van bereik automatisch toegevoegd als aangepaste eigenschappen aan de verzamelde telemetrie.
- Logboeken kunnen nu veel lager in de toepassing opstarten pijplijn worden vastgelegd. Logboeken van de **programma** en **opstarten** klassen kunnen nu worden vastgelegd.
- Met de nieuwe provider is gefilterd op het niveau van het framework zelf. U kunt logboeken naar de Application Insights-provider filteren op dezelfde manier als voor andere providers, met inbegrip van ingebouwde providers, zoals de Console, foutopsporing, enzovoort. U kunt ook de dezelfde filters toepassen op meerdere providers.
- In de ASP.NET Core (2.0 of hoger), de aanbevolen manier om [inschakelen van logboekregistratie providers](https://github.com/aspnet/Announcements/issues/255) wordt met behulp van uitbreidingsmethoden op ILoggingBuilder in **Program.cs** zelf.

> [!Note]
> De nieuwe provider is beschikbaar voor toepassingen die zijn gericht op NETSTANDARD2.0 of hoger. Als uw toepassing is gericht op oudere versies van .NET Core, zoals .NET Core 1.1, of als het is bedoeld voor .NET Framework, echter ook doorgaan met de oude provider.

## <a name="console-application"></a>Consoletoepassing

De volgende code toont een voorbeeld-consoletoepassing die geconfigureerd voor het verzenden van ILogger traceringen naar Application Insights.

Pakketten die zijn geïnstalleerd:

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.Extensions.DependencyInjection" Version="2.1.0" />  
  <PackageReference Include="Microsoft.Extensions.Logging.ApplicationInsights" Version="2.9.1" />
  <PackageReference Include="Microsoft.Extensions.Logging.Console" Version="2.1.0" />
</ItemGroup>
```

```csharp
class Program
{
    static void Main(string[] args)
    {
        // Create the DI container.
        IServiceCollection services = new ServiceCollection();

        // Channel is explicitly configured to do flush on it later.
        var channel = new InMemoryChannel();
        services.Configure<TelemetryConfiguration>(
            (config) =>
            {
                config.TelemetryChannel = channel;
            }
        );

        // Add the logging pipelines to use. We are using Application Insights only here.
        services.AddLogging(builder =>
        {
            // Optional: Apply filters to configure LogLevel Trace or above is sent to
            // Application Insights for all categories.
            builder.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                             ("", LogLevel.Trace);
            builder.AddApplicationInsights("--YourAIKeyHere--");
        });

        // Build ServiceProvider.
        IServiceProvider serviceProvider = services.BuildServiceProvider();

        ILogger<Program> logger = serviceProvider.GetRequiredService<ILogger<Program>>();

        // Begin a new scope. This is optional.
        using (logger.BeginScope(new Dictionary<string, object> { { "Method", nameof(Main) } }))
        {
            logger.LogInformation("Logger is working"); // this will be captured by Application Insights.
        }

        // Explicitly call Flush() followed by sleep is required in Console Apps.
        // This is to ensure that even if application terminates, telemetry is sent to the back-end.
        channel.Flush();
        Thread.Sleep(1000);
    }
}
```

In dit voorbeeld wordt het zelfstandige pakket `Microsoft.Extensions.Logging.ApplicationInsights`. Deze configuratie maakt standaard gebruik van de 'minimumwaarde' TelemetryConfiguration voor het verzenden van gegevens naar Application Insights. Minimumwaarde betekent dat InMemoryChannel het kanaal dat wordt gebruikt. Er is geen TelemetryInitializers bemonsteringssysteem en niet-standaard. Dit gedrag kan worden overschreven voor een toepassing, zoals in het volgende voorbeeld wordt weergegeven.

Deze extra pakket installeren:

```xml
<PackageReference Include="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel" Version="2.9.1" />
```

De volgende sectie laat zien hoe u de standaard TelemetryConfiguration overschrijven met behulp van de **services. Configureer<TelemetryConfiguration>()** methode. In dit voorbeeld stelt u `ServerTelemetryChannel` en steekproeven. Een aangepaste ITelemetryInitializer wordt toegevoegd aan de TelemetryConfiguration.

```csharp
    // Create the DI container.
    IServiceCollection services = new ServiceCollection();
    var serverChannel = new ServerTelemetryChannel();
    services.Configure<TelemetryConfiguration>(
        (config) =>
        {
            config.TelemetryChannel = serverChannel;
            config.TelemetryInitializers.Add(new MyTelemetryInitalizer());
            config.DefaultTelemetrySink.TelemetryProcessorChainBuilder.UseSampling(5);
            serverChannel.Initialize(config);
        }
    );

    // Add the logging pipelines to use. We are adding Application Insights only.
    services.AddLogging(loggingBuilder =>
    {
        loggingBuilder.AddApplicationInsights();
    });

    ........
    ........

    // Explicitly calling Flush() followed by sleep is required in Console Apps.
    // This is to ensure that even if the application terminates, telemetry is sent to the back end.
    serverChannel.Flush();
    Thread.Sleep(1000);
```

## <a name="control-logging-level"></a>Niveau van logboekregistratie voor controle

De ASP.NET Core *ILogger* infra is een ingebouwd mechanisme om toe te passen [logboek filteren](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.2#log-filtering). Hiermee kunt u de logboeken die worden verzonden naar alle geregistreerde provider, met inbegrip van de Application Insights-provider instellen. Het filteren kan worden gedaan in de configuratie (meestal met behulp van een *appsettings.json* bestand) of in de code. Deze mogelijkheid wordt geboden door het framework zelf. Het is niet specifiek zijn voor de Application Insights-provider.

De volgende voorbeelden van filterregels voor de toepassing op ApplicationInsightsLoggerProvider.

### <a name="create-filter-rules-in-configuration-with-appsettingsjson"></a>Filterregels voor de configuratie met appsettings.json maken

De provider-alias is voor ApplicationInsightsLoggerProvider, `ApplicationInsights`. De volgende sectie van *appsettings.json* configureert u Logboeken voor *waarschuwing* en hoger uit alle categorieën en *fout* en hoger van categorieën die met beginnen ' Microsoft' worden verzonden naar `ApplicationInsightsLoggerProvider`.

```json
{
  "Logging": {
    "ApplicationInsights": {
      "LogLevel": {
        "Default": "Warning",
        "Microsoft": "Error"
      }
    },
    "LogLevel": {
      "Default": "Warning"
    }
  },
  "AllowedHosts": "*"
}
```

### <a name="create-filter-rules-in-code"></a>Filterregels voor de in code maken

Het volgende codefragment configureert u Logboeken voor *waarschuwing* en hoger uit alle categorieën en voor *fout* en hoger van categorieën die met 'Microsoft beginnen' worden verzonden naar `ApplicationInsightsLoggerProvider`. Deze configuratie is hetzelfde als in de vorige sectie in *appsettings.json*.

```csharp
    WebHost.CreateDefaultBuilder(args)
    .UseStartup<Startup>()
    .ConfigureLogging(logging =>
      logging.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                        ("", LogLevel.Warning)
             .AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                        ("Microsoft", LogLevel.Error);
```

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="what-are-the-old-and-new-versions-of-applicationinsightsloggerprovider"></a>Wat zijn de oude en nieuwe versies van ApplicationInsightsLoggerProvider?

[Microsoft.ApplicationInsights.AspNet SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) opgenomen van een ingebouwde ApplicationInsightsLoggerProvider (Microsoft.ApplicationInsights.AspNetCore.Logging.ApplicationInsightsLoggerProvider), die is ingeschakeld via  **ILoggerFactory** uitbreidingsmethoden. Deze provider is gemarkeerd als verouderd van versie 2.7.0-beta2. Dit wordt volledig worden verwijderd in de volgende wijziging voor de primaire versie. De [Microsoft.ApplicationInsights.AspNetCore 2.6.1](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) pakket zelf niet is verouderd. Het is vereist om in te schakelen bewaking van aanvragen, afhankelijkheden, enzovoort.

De voorgestelde alternatief is het zelfstandige pakket met nieuwe [Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights), die een verbeterde ApplicationInsightsLoggerProvider (bevat Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider) en uitbreidingsmethoden op ILoggerBuilder voor het inschakelen van deze.

[Microsoft.ApplicationInsights.AspNet SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) versie 2.7.0-beta3 wordt afhankelijk van het nieuwe pakket en ILogger vastleggen automatisch kunt.

### <a name="why-are-some-ilogger-logs-shown-twice-in-application-insights"></a>Waarom worden bepaalde ILogger logboeken weergegeven tweemaal in Application Insights?

Duplicatie kan optreden als u de oudere (nu verouderde) versie van ApplicationInsightsLoggerProvider ingeschakeld door het aanroepen van `AddApplicationInsights` op `ILoggerFactory`. Controleer of uw **configureren** methode heeft de volgende, en deze verwijderen:

```csharp
 public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
 {
     loggerFactory.AddApplicationInsights(app.ApplicationServices, LogLevel.Warning);
     // ..other code.
 }
```

Als u dubbele logboekregistratie ondervindt wanneer u fouten vanuit Visual Studio opspoort, stelt u `EnableDebugLogger` naar *false* in de code waarmee Application Insights, als volgt. Deze dupliceren en de oplossing is alleen relevant wanneer u foutopsporing op de toepassing.

```csharp
 public void ConfigureServices(IServiceCollection services)
 {
     ApplicationInsightsServiceOptions options = new ApplicationInsightsServiceOptions();
     options.EnableDebugLogger = false;
     services.AddApplicationInsightsTelemetry(options);
     // ..other code.
 }
```

### <a name="i-updated-to-microsoftapplicationinsightsaspnet-sdkhttpswwwnugetorgpackagesmicrosoftapplicationinsightsaspnetcore-version-270-beta3-and-logs-from-ilogger-are-captured-automatically-how-do-i-turn-off-this-feature-completely"></a>Ik bijgewerkt naar [Microsoft.ApplicationInsights.AspNet SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) versie 2.7.0-beta3 en logboeken van ILogger automatisch worden vastgelegd. Hoe ik deze functie uitschakelen volledig?

Zie de [bepalen het logboekregistratieniveau van](../../azure-monitor/app/ilogger.md#control-logging-level) sectie voor informatie over het filteren in het algemeen registreert. Gebruiken om te laten zien ApplicationInsightsLoggerProvider, `LogLevel.None`:

**In de code:**

```csharp
    builder.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                      ("", LogLevel.None);
```

**In de configuratie:**

```json
{
  "Logging": {
    "ApplicationInsights": {
      "LogLevel": {
        "Default": "None"
      }
}
```

### <a name="why-do-some-ilogger-logs-not-have-the-same-properties-as-others"></a>Waarom sommige ILogger Logboeken niet hebben dezelfde eigenschappen als anderen?

Application Insights worden vastgelegd en ILogger logboeken worden verzonden via de dezelfde TelemetryConfiguration die wordt gebruikt voor elke andere telemetrie. Maar er is een uitzondering. Standaard TelemetryConfiguration is niet volledig ingesteld wanneer u zich vanaf aanmeldt **Program.cs** of **Startup.cs**. Logboeken van deze locaties geen de standaardconfiguratie, zodat ze alle TelemetryInitializers en TelemetryProcessors wordt niet worden uitgevoerd.

### <a name="im-using-the-standalone-package-microsoftextensionsloggingapplicationinsights-and-i-want-to-log-some-additional-custom-telemetry-manually-how-should-i-do-that"></a>Ik gebruik het zelfstandige pakket Microsoft.Extensions.Logging.ApplicationInsights en ik wil aanmelden extra aangepaste telemetrie handmatig. Hoe moet ik dat doen?

Wanneer u het zelfstandige pakket, `TelemetryClient` niet naar de container DI is opgenomen, zodat u wilt maken van een nieuw exemplaar van `TelemetryClient` en gebruiken dezelfde configuratie als de logger-provider gebruikt, zoals in de volgende code wordt weergegeven. Dit zorgt ervoor dat dezelfde configuratie wordt gebruikt voor alle aangepaste telemetrie, evenals telemetrie van ILogger.

```csharp
public class MyController : ApiController
{
   // This telemtryclient can be used to track additional telemetry using TrackXXX() api.
   private readonly TelemetryClient _telemetryClient;
   private readonly ILogger _logger;

   public MyController(IOptions<TelemetryConfiguration> options, ILogger<MyController> logger)
   {
        _telemetryClient = new TelemetryClient(options.Value);
        _logger = logger;
   }  
}
```

> [!NOTE]
> Als u het pakket Microsoft.ApplicationInsights.AspNetCore naar Application Insights inschakelen, wijzigt u deze code om op te halen `TelemetryClient` rechtstreeks in de constructor. Zie voor een voorbeeld [deze Veelgestelde vragen over](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core-no-visualstudio#frequently-asked-questions).


### <a name="what-application-insights-telemetry-type-is-produced-from-ilogger-logs-or-where-can-i-see-ilogger-logs-in-application-insights"></a>Welk type van de telemetrie Application Insights wordt gemaakt van ILogger Logboeken? Of waar kan ik zien dat ILogger Logboeken in Application Insights?

ApplicationInsightsLoggerProvider legt ILogger logboeken en TraceTelemetry maakt van deze. Als een object uitzondering wordt doorgegeven aan de **Log()** methode voor ILogger, *ExceptionTelemetry* in plaats van TraceTelemetry wordt gemaakt. Deze telemetrie-items kunnen worden gevonden in dezelfde plaatsen als een andere TraceTelemetry of ExceptionTelemetry voor Application Insights, met inbegrip van de portal, analytics of lokale foutopsporing van Visual Studio.

Als u liever TraceTelemetry altijd verzenden, gebruikt u dit fragment: ```builder.AddApplicationInsights((opt) => opt.TrackExceptionsAsExceptionTelemetry = false);```

### <a name="i-dont-have-the-sdk-installed-and-i-use-the-azure-web-apps-extension-to-enable-application-insights-for-my-aspnet-core-applications-how-do-i-use-the-new-provider"></a>Ik heb de SDK is geïnstalleerd en kan ik de Azure Web Apps-extensie gebruiken voor het inschakelen van Application Insights voor mijn ASP.NET Core-toepassingen. Hoe gebruik ik de nieuwe provider? 

De extensie Application Insights in Azure Web Apps maakt gebruik van de oude provider. U kunt de filters gebruiken om regels in de *appsettings.json* -bestand voor uw toepassing. Gebruiken om te profiteren van de nieuwe provider, build-runtime-instrumentatiesleutel door middel van een NuGet-afhankelijkheid volgens de SDK. In dit artikel wordt bijgewerkt wanneer de extensie overschakelt naar de nieuwe provider gebruiken.

### <a name="im-using-the-standalone-package-microsoftextensionsloggingapplicationinsights-and-enabling-application-insights-provider-by-calling-builderaddapplicationinsightsikey-is-there-an-option-to-get-an-instrumentation-key-from-configuration"></a>Ik ben met behulp van het zelfstandige pakket Microsoft.Extensions.Logging.ApplicationInsights en Application Insights-provider in te schakelen door het aanroepen van **builder. AddApplicationInsights("ikey")**. Is er een optie om op te halen van een instrumentatiesleutel van configuratie?


Program.cs en appsettings.json als volgt wijzigen:

   ```csharp
   public class Program
   {
       public static void Main(string[] args)
       {
           CreateWebHostBuilder(args).Build().Run();
       }

       public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
           WebHost.CreateDefaultBuilder(args)
               .UseStartup<Startup>()
               .ConfigureLogging((hostingContext, logging) =>
               {
                   // hostingContext.HostingEnvironment can be used to determine environments as well.
                   var appInsightKey = hostingContext.Configuration["myikeyfromconfig"];
                   logging.AddApplicationInsights(appInsightKey);
               });
   }
   ```

   Relevante sectie uit `appsettings.json`:

   ```json
   {
     "myikeyfromconfig": "putrealikeyhere"
   }
   ```

Deze code is alleen vereist als u een zelfstandige logboekregistratie provider gebruiken. Voor regelmatige controle van Application Insights de instrumentatiesleutel wordt automatisch geladen vanuit het configuratiepad *Application Insights: Instrumentationkey*. AppSettings.JSON moet er als volgt:

   ```json
   {
     "ApplicationInsights":
       {
           "Instrumentationkey":"putrealikeyhere"
       }
   }
   ```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over:

* [Logboekregistratie in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging)
* [.NET-traceerlogboeken in Application Insights](../../azure-monitor/app/asp-net-trace-logs.md)
