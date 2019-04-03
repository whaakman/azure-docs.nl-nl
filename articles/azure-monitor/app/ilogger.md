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
ms.openlocfilehash: b8c0d84f6989e60c7abef0d423230300896663de
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58877904"
---
# <a name="applicationinsightsloggerprovider-for-net-core-ilogger-logs"></a>ApplicationInsightsLoggerProvider voor .NET Core ILogger Logboeken

Een API voor logboekregistratie die geschikt is voor verschillende soorten ingebouwde en door de derde partij logboekregistratie providers biedt ondersteuning voor ASP.NET Core. Logboekregistratie vindt plaats door het aanroepen van Log() of een variant van het op `ILogger` exemplaren. In dit artikel ziet u hoe u `ApplicationInsightsLoggerProvider` om vast te leggen `ILogger` Logboeken in- en ASP.NET Core-toepassingen. In dit artikel beschrijft ook hoe `ApplicationInsightsLoggerProvider` is geïntegreerd met andere telemetrie Application Insights.
Zie voor meer informatie meer logboekregistratie in Asp.Net Core, [in dit artikel](https://docs.microsoft.com/aspnet/core/fundamentals/logging).

## <a name="aspnet-core-applications"></a>ASP.NET Core-toepassingen

Beginnen met [Microsoft.ApplicationInsights.AspNet SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) en hoger, versie 2.7.0-beta3 `ApplicationInsightsLoggerProvider` is standaard ingeschakeld bij het inschakelen van regelmatige controle van Application Insights met behulp van de standaard-methoden - door aanroepen van `UseApplicationInsights` uitbreidingsmethode op IWebHostBuilder of `AddApplicationInsightsTelemetry` uitbreidingsmethode op IServiceCollection. `ILogger` logboeken die zijn vastgelegd door `ApplicationInsightsLoggerProvider` gelden dezelfde configuratie als geen andere verzamelde telemetriegegevens. i.e ze hebben dezelfde set `TelemetryInitializer`s, `TelemetryProcessor`s, maakt gebruik van dezelfde `TelemetryChannel`, en worden gecorreleerd en voorbeelden worden gemaakt op dezelfde manier als elke andere telemetrie.  Als u in deze versie van de SDK of hoger, wordt geen actie vereist is om vast te leggen `ILogger` Logboeken.

Standaard alleen `ILogger` logboeken van `Warning` of hoger (uit alle categorieën) worden verzonden naar Application Insights. Dit gedrag kan worden gewijzigd door Filters zijn toegepast zoals [hier](#control-logging-level). Ook extra stappen zijn vereist als `ILogger` logboeken van `Program.cs` of `Startup.cs` moeten worden vastgelegd zoals [hier](#capturing-ilogger-logs-from-startupcs-programcs-in-aspnet-core-applications).

Als u een eerdere versie van Microsoft.ApplicationInsights.AspNet SDK gebruikt, of u wilt gebruiken ApplicationInsightsLoggerProvider, zonder eventuele andere Application Insights-bewaking, volg de onderstaande stappen.

1. Installeer het nuget-pakket.

```xml
    <ItemGroup>
      <PackageReference Include="Microsoft.Extensions.Logging.ApplicationInsights" Version="2.9.1" />  
    </ItemGroup>
```

2. Wijzig `Program.cs` hieronder

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
                    // Providing an instrumentation key here is required if you are using standalone package Microsoft.Extensions.Logging.ApplicationInsights
                    // or if you want to capture logs from early in the application startup pipeline from Startup.cs or Program.cs itself.
                    builder.AddApplicationInsights("ikey");

                    // Optional: Apply filters to control what logs are sent to Application Insights.
                    // The following configures LogLevel Information or above to be sent to Application Insights for all categories.
                    builder.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>("", LogLevel.Information);
                }
            );
    }
```

De bovenstaande code configureert `ApplicationInsightsLoggerProvider`. Hieronder ziet u een voorbeeld van de controllerklasse, dat gebruikmaakt van `ILogger` voor het verzenden van Logboeken, die worden vastgelegd door Application Insights.

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

### <a name="capturing-ilogger-logs-from-startupcs-programcs-in-aspnet-core-applications"></a>Vastleggen ILogger logboeken van Startup.cs, Program.cs in Asp.Net Core-toepassingen

Met de nieuwe ApplicationInsightsLoggerProvider is het mogelijk om vast te leggen van de logboeken van al vroeg in de toepassing opstarten-pijplijn. Zelfs al ApplicationInsightsLoggerProvider is automatisch ingeschakeld Application Insights (van 2.7.0-beta3 en hoger), nog geen instrumentation belangrijke setup pas later in de pijplijn, dus alleen logboeken van de domeincontroller / andere klassen wordt vastgelegd. Voor het vastleggen van alle logboeken die beginnen met `Program.cs` en `Startup.cs` zelf, een moet ApplicationInsightsLoggerProvider expliciet inschakelen met een instrumentatiesleutel. Het is ook belangrijk om te weten dat `TelemetryConfiguration` is niet volledig ingesteld wanneer registreren iets uit `Program.cs` of `Startup.cs` zelf, zodat deze logboeken wordt een bare minimale configuratie, waardoor InMemoryChannel, geen steekproeven en geen standaardtelemetrie gebruikt gebruiken initializers of processors.

Hieronder ziet u voorbeelden van `Program.cs` en `Startup.cs` met deze mogelijkheid.

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
                    // providing an instrumentation key here is required if you are using standalone package Microsoft.Extensions.Logging.ApplicationInsights
                    // or if you want to capture logs from early in the application startup pipeline from Startup.cs or Program.cs itself.
                    builder.AddApplicationInsights("ikey");

                    // Adding the filter below to ensure logs of all severity from Program.cs is sent to ApplicationInsights.
                    // Replace YourAppName with the namespace of your application's Program.cs
                    builder.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>("YourAppName.Program", LogLevel.Trace);
                    // Adding the filter below to ensure logs of all severity from Startup.cs is sent to ApplicationInsights.
                    // Replace YourAppName with the namespace of your application's Startup.cs
                    builder.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>("YourAppName.Startup", LogLevel.Trace);
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

## <a name="migrating-from-old-applicationinsightsloggerprovider"></a>Migreren van oude ApplicationInsightsLoggerProvider

Microsoft.ApplicationInsights.AspNet SDK-versies vóór 2.7.0-beta2, ondersteund een provider voor logboekregistratie die is nu verouderd. Deze provider is ingeschakeld met `AddApplicationInsights()` uitbreidingsmethode van `ILoggerFactory`. Deze provider is nu verouderd, en gebruikers te migreren naar de nieuwe provider worden voorgesteld. Migratie bestaat uit twee stappen.

1. Verwijderen van ILoggerFactory.AddApplicationInsights() aanroep van `Startup.Configure()` methode om te voorkomen dat dubbele logboekregistratie.
2. Pas geen filters gebruiken om regels in de code als ze niet door de nieuwe provider worden gerespecteerd. Overloads van ILoggerFactory.AddApplicationInsights() duurde minimale LogLevel of filter voor de functies. Met de nieuwe provider filteren maakt deel uit van het framework voor logboekregistratie zelf, en niet door Application Insights-provider wordt uitgevoerd. Dus geen filters geleverd via `ILoggerFactory.AddApplicationInsights()` overloads moeten worden verwijderd en filterregels moet worden opgegeven na [deze](#control-logging-level) instructies. Als u `appsettings.json` om te filteren logboekregistratie, blijven deze werken met de nieuwe provider omdat beide dezelfde Provider Alias - **ApplicationInsights**.

Terwijl de oude provider kan nog steeds worden gebruikt (dit is nu verouderd en wordt alleen in de primaire versie wijzigen 3.xx verwijderd), migreren naar nieuwere provider wordt ten zeerste aangeraden de volgende oorzaken hebben.

1. Vorige provider ontbreken ondersteuning van [Scopes](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.2#log-scopes). In de nieuwe provider worden eigenschappen van bereik automatisch toegevoegd als aangepaste eigenschappen aan de verzamelde telemetrie.
2. Logboeken kunnen nu veel lager in de toepassing opstarten pijplijn worden vastgelegd. i.e Logboeken van klassen programma en opstarten kunnen nu worden vastgelegd.
3. Met de nieuwe provider, wordt het filteren uitgevoerd op het niveau van het framework zelf. Het filteren van logboeken met Application Insights-provider kan worden uitgevoerd in exact dezelfde manier als voor andere providers, met inbegrip van ingebouwde providers, zoals de Console, foutopsporing, enzovoort. Het is ook mogelijk dezelfde filters toepassen op meerdere providers.
4. De [aanbevolen](https://github.com/aspnet/Announcements/issues/255) manier in Asp.Net Core (2.0 of hoger) om in te schakelen logboekregistratie providers is met behulp van uitbreidingsmethoden op ILoggingBuilder in `Program.cs` zelf.

> [!Note]
De nieuwe Provider is beschikbaar voor toepassingen die zijn gericht op `NETSTANDARD2.0` of hoger. Als uw toepassing is die gericht is op oudere versies van .NET Core, zoals .NET Core 1.1 of als die gericht is op .NET Framework, echter ook doorgaan met de oude provider.

## <a name="console-application"></a>Consoletoepassing

De volgende code toont een voorbeeld-consoletoepassing die is geconfigureerd voor het verzenden van `ILogger` traceringen naar Application Insights.

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
        // Create DI container.
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
        services.AddLogging(loggingBuilder =>
        {
            // Optional: Apply filters to configure LogLevel Trace or above is sent to Application Insights for all
            // categories.
            loggingBuilder.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                                                                                    ("", LogLevel.Trace);
            loggingBuilder.AddApplicationInsights("--YourAIKeyHere--");
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

In het bovenstaande voorbeeld wordt het zelfstandige pakket `Microsoft.Extensions.Logging.ApplicationInsights` wordt gebruikt. Deze configuratie maakt standaard gebruik van de minimumwaarde `TelemetryConfiguration` voor het verzenden van gegevens naar Application Insights. Minimumwaarde betekent dat het kanaal dat wordt gebruikt is `InMemoryChannel`, geen steekproeven en geen standaard TelemetryInitializers. Dit gedrag kan worden overschreven voor een toepassing, zoals wordt weergegeven in het volgende voorbeeld.

Deze extra pakket installeren:

```xml
<PackageReference Include="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel" Version="2.9.1" />
```

De volgende sectie wordt beschreven hoe het standaard `TelemetryConfiguration` met behulp van `services.Configure<TelemetryConfiguration>()` methode. In dit voorbeeld stelt u `ServerTelemetryChannel`sampling en voegt u een aangepaste `ITelemetryInitializer` naar de `TelemetryConfiguration`.

```csharp
    // Create DI container.
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

    // Explicitly call Flush() followed by sleep is required in Console Apps.
    // This is to ensure that even if application terminates, telemetry is sent to the back-end.
    serverChannel.Flush();
    Thread.Sleep(1000);
```

## <a name="control-logging-level"></a>Niveau van logboekregistratie voor controle

De Asp.Net Core `ILogger` infra heeft ingebouwde mechanisme om toe te passen [filteren](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.2#log-filtering) van Logboeken, waarmee gebruikers voor het beheren van de logboeken die zijn verzonden naar alle geregistreerde providers, met inbegrip van Application Insights-provider. Deze filtering kan worden gedaan in de configuratie (meestal met behulp van `appsettings.json` bestand) of in de code. Deze functie wordt geleverd door het framework zelf, en is niet specifiek voor Application Insights-provider.

Voorbeelden van het toepassen van de filterregels op ApplicationInsightsLoggerProvider hieronder.

### <a name="create-filter-rules-in-configuration-with-appsettingsjson"></a>Filterregels voor de configuratie met appsettings.json maken

De provider-alias is voor ApplicationInsightsLoggerProvider, `ApplicationInsights`. De hieronder weergegeven sectie in `appsettings.json` configureert Logboeken `Warning` en hoger uit alle categorieën `Error` en hoger van categorieën die beginnen met 'Microsoft' worden verzonden naar `ApplicationInsightsLoggerProvider`.

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

De onderstaande code codefragment configureert Logboeken `Warning` en hoger uit alle categorieën `Error` en hoger van categorieën die beginnen met 'Microsoft' worden verzonden naar `ApplicationInsightsLoggerProvider`. Deze configuratie is hetzelfde als de configuratie van de bovenstaande doen in `appsettings.json`.

```csharp
    WebHost.CreateDefaultBuilder(args)
    .UseStartup<Startup>()
    .ConfigureLogging(logging =>
        logging.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>("", LogLevel.Warning)
               .AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>("Microsoft", LogLevel.Error);
```

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

*1. Wat is de oude en nieuwe ApplicationInsightsLoggerProvider?*

* [Microsoft.ApplicationInsights.AspNet SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) geleverd met een ingebouwde ApplicationInsightsLoggerProvider (Microsoft.ApplicationInsights.AspNetCore.Logging.ApplicationInsightsLoggerProvider), die is ingeschakeld met behulp van ILoggerFactory uitbreidingsmethoden. Deze provider is gemarkeerd als verouderd van 2.7.0-beta2 en hoger en volledig wordt verwijderd in de volgende wijziging voor de primaire versie. [Dit](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) pakket zelf niet verouderd en is vereist voor het inschakelen van bewaking van aanvragen, afhankelijkheden, enzovoort.

* De voorgestelde alternatief is het zelfstandige pakket met nieuwe [Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights), met een verbeterde ApplicationInsightsLoggerProvider ( Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider), en -extensies methoden op ILoggerBuilder voor het inschakelen van deze.

* [Microsoft.ApplicationInsights.AspNet SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) versie 2.7.0-beta3 en hoger duurt een afhankelijkheid op de bovenstaande pakket, en kunt `ILogger` automatisch vastleggen.

*2. Ik krijg sommige `ILogger` logboeken worden weergegeven in Application Insights tweemaal?*

* Deze duplicatie is mogelijk hebt u de oudere (nu verouderde) versie van `ApplicationInsightsLoggerProvider` ingeschakeld door het aanroepen van `AddApplicationInsights` op `ILoggerFactory`. Controleer of uw `Configure` methode heeft de volgende en verwijderen.

   ```csharp
    public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
    {
        loggerFactory.AddApplicationInsights(app.ApplicationServices, LogLevel.Warning);
        // ..other code.
    }
   ```

* Als u dubbele logboekregistratie ondervindt wanneer foutopsporing via Visual Studio, en wijzig vervolgens de code die wordt gebruikt om in te schakelen van Application Insights als volgt door in te stellen `EnableDebugLogger` moet false zijn. Dit duplicatie probleem en de oplossing is alleen relevant zijn bij het opsporen van fouten in de toepassing.

   ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        ApplicationInsightsServiceOptions options = new ApplicationInsightsServiceOptions();
        options.EnableDebugLogger = false;
        services.AddApplicationInsightsTelemetry(options);
        // ..other code.
    }
   ```

*3. Ik bijgewerkt naar [Microsoft.ApplicationInsights.AspNet SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) versie 2.7.0-beta3 en ik ben nu zien die zich aanmeldt vanaf `ILogger` automatisch worden vastgelegd. Hoe kan ik deze functie uitschakelen volledig?*

* Zie [dit](../../azure-monitor/app/ilogger.md#control-logging-level) sectie om te weten hoe om te filteren in het algemeen registreert. Als u wilt laten zien ApplicationInsightsLoggerProvider gebruiken `LogLevel.None` voor.

  In de code

    ```csharp
        builder.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>("", LogLevel.None);
    ```

  In de configuratie

    ```json
    {
      "Logging": {
        "ApplicationInsights": {
          "LogLevel": {
            "Default": "None"
          }
    }
    ```

*4. Ik krijg sommige `ILogger` logboeken worden niet dezelfde eigenschappen als anderen hebben?*

* Application Insights-opnamen en verzendt `ILogger` registreert met behulp van dezelfde `TelemetryConfiguration` gebruikt voor elke andere telemetrie. Er is een uitzondering op deze regel. De standaardwaarde `TelemetryConfiguration` is niet volledig ingesteld wanneer registreren iets uit `Program.cs` of `Startup.cs` zelf en dus Logboeken vanaf deze locaties niet de standaard-configuratie heeft en kan daarom niet wordt uitgevoerd alle de `TelemetryInitializer`s en `TelemetryProcessor`s.

*5. Welk type van de telemetrie Application Insights wordt gemaakt van `ILogger` Logboeken? of waar kan ik zien `ILogger` Logboeken in Application Insights?*

* ApplicationInsightsLoggerProvider vastgelegd `ILogger` registreert en maakt `TraceTelemetry` uit. Als een object uitzondering wordt doorgegeven aan de methode Log() ILogger, klikt u vervolgens in plaats van `TraceTelemetry`, een `ExceptionTelemetry` wordt gemaakt. Deze telemetrie-items kunnen u vinden in dezelfde plaatsen als elk ander `TraceTelemetry` of `ExceptionTelemetry` voor Application Insights, met inbegrip van de portal, analytics of lokale foutopsporing van Visual Studio.
Als u liever altijd verzenden `TraceTelemetry`, gebruikt u het fragment ```builder.AddApplicationInsights((opt) => opt.TrackExceptionsAsExceptionTelemetry = false);```.

*5. Ik heb SDK is geïnstalleerd, en ik Azure Web-Appuitbreiding voor Application Insights inschakelen voor mijn Asp.Net Core-toepassingen gebruiken. Hoe gebruik ik de nieuwe provider?*

* Application Insights-extensie in Azure Web-App maakt gebruik van de oude provider. Regels filteren kan worden gewijzigd `appsettings.json` voor uw toepassing. Als u wilt profiteren van de nieuwe provider, gebruikt u build-runtime-instrumentatiesleutel door middel van nuget afhankelijkheid volgens de SDK. Dit document wordt bijgewerkt wanneer extensie overschakelt naar de nieuwe provider gebruiken.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over:

* [Logboekregistratie in Asp.Net Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging)
* [.NET-traceerlogboeken in Application Insights](../../azure-monitor/app/asp-net-trace-logs.md)
