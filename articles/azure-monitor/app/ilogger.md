---
title: .NET-traceerlogboeken in Azure Application Insights met ILogger verkennen
description: Voorbeelden van het gebruik van de implementatie van Azure Application Insights ILogger met ASP.NET Core-en consoletoepassingen.
services: application-insights
author: cijothomas
manager: carmonm
ms.service: application-insights
ms.topic: conceptual
ms.date: 02/19/2019
ms.reviewer: mbullwin
ms.author: cithomas
ms.openlocfilehash: 4c385d2af0d9e4e213cd690b3c30d8719588220d
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58399502"
---
# <a name="ilogger"></a>ILogger

Een API voor logboekregistratie die geschikt is voor tal van ingebouwde en door de derde partij logboekregistratie providers biedt ondersteuning voor ASP.NET Core. In dit artikel laat zien hoe voor het afhandelen van aanmelding bij de implementatie van de Application Insights ILogger in- en ASP.NET Core-toepassingen. Zie voor meer informatie over ILogger op basis van logboekregistratie, [in dit artikel](https://docs.microsoft.com/aspnet/core/fundamentals/logging).

## <a name="console-application"></a>Consoletoepassing

Hieronder ziet u een voorbeeld-consoletoepassing die is geconfigureerd voor het verzenden van ILogger traceringen naar Application Insights.

Pakketten die zijn geïnstalleerd:

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.Extensions.DependencyInjection" Version="2.1.0" />
  <PackageReference Include="Microsoft.Extensions.Logging" Version="2.1.0" />
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
        // Optional: Apply filters to configure LogLevel Trace or above is sent to ApplicationInsights for all
        // categories.
        loggingBuilder.AddFilter<ApplicationInsightsLoggerProvider>("", LogLevel.Trace);
            loggingBuilder.AddApplicationInsights("--YourAIKeyHere--");                
        });

        // Build ServiceProvider.
        IServiceProvider serviceProvider = services.BuildServiceProvider();

        ILogger<Program> logger = serviceProvider.GetRequiredService<ILogger<Program>>();

        // Begin a new scope. This is optional. Epecially in case of AspNetCore request info is already
        // present in scope.
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

## <a name="aspnet-core-application"></a>ASP.NET Core-toepassing

Hieronder ziet u een voorbeeld van een ASP.NET Core-toepassing die is geconfigureerd voor het verzenden van ILogger traceringen naar application insights. In dit voorbeeld kan worden uitgevoerd voor het verzenden van ILogger traceringen van Program.cs, Startup.cs of een andere Contoller/toepassingslogica.

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var host = BuildWebHost(args);
        var logger = host.Services.GetRequiredService<ILogger<Program>>();
        logger.LogInformation("From Program. Running the host now.."); // This will be picked up by AI
        host.Run();
    }

    public static IWebHost BuildWebHost(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureLogging(logging =>
        {
            logging.AddApplicationInsights("ikeyhere");

            // Optional: Apply filters to configure LogLevel Trace or above is sent to
            // ApplicationInsights for all categories.
            logging.AddFilter<ApplicationInsightsLoggerProvider>("", LogLevel.Trace);

            // Additional filtering For category starting in "Microsoft",
            // only Warning or above will be sent to Application Insights.
            logging.AddFilter<ApplicationInsightsLoggerProvider>("Microsoft", LogLevel.Warning);
        })
        .Build();
}
```

```csharp
public class Startup
{
    private readonly ILogger _logger;

    public Startup(IConfiguration configuration, ILogger<Startup> logger)
    {
        Configuration = configuration;
        _logger = logger;
    }

    public IConfiguration Configuration { get; }

    // This method gets called by the runtime. Use this method to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);

        // The following will be picked up by Application Insights.
        _logger.LogInformation("From ConfigureServices. Services.AddMVC invoked"); 
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

```csharp
public class ValuesController : ControllerBase
{
    private readonly ILogger _logger;

    public ValuesController(ILogger<ValuesController> logger)
    {
        _logger = logger;
    }

    // GET api/values
    [HttpGet]
    public ActionResult<IEnumerable<string>> Get()
    {
        // All the following logs will be picked up by Application Insights.
        // and all have ("MyKey", "MyValue") in Properties.
        using (_logger.BeginScope(new Dictionary<string, object> { { "MyKey", "MyValue" } }))
            {
            _logger.LogInformation("An example of a Information trace..");
            _logger.LogWarning("An example of a Warning trace..");
            _logger.LogTrace("An example of a Trace level message");
            }
        return new string[] { "value1", "value2" };
    }
}
```

In beide bovenstaande voorbeelden, het zelfstandige pakket wordt Microsoft.Extensions.Logging.ApplicationInsights gebruikt. Deze configuratie maakt standaard gebruik van de minimumwaarde `TelemetryConfiguration` voor het verzenden van gegevens naar Application Insights. Minimumwaarde betekent dat het kanaal dat wordt gebruikt is `InMemoryChannel`, geen steekproeven en geen standaard TelemetryInitializers. Dit gedrag kan worden overschreven voor een toepassing, zoals wordt weergegeven in het volgende voorbeeld.

Deze extra pakket installeren:

```xml
<PackageReference Include="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel" Version="2.9.1" />
```

De volgende sectie wordt beschreven hoe het standaard `TelemetryConfiguration`. In dit voorbeeld configureert u `ServerTelemetryChannel`, bemonsteringssysteem, en een aangepaste `ITelemetryInitializer`.

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

    // Add the logging pipelines to use. We are adding ApplicationInsights only.
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

Terwijl de bovenstaande methode kan worden gebruikt in een ASP.NET Core-toepassing, is een algemene aanpak combineren reguliere toepassingsbewaking (aanvragen, afhankelijkheden, enzovoort) in met ILogger vastleggen, zoals hieronder wordt weergegeven.

Deze extra pakket installeren:

```xml
<PackageReference Include="Microsoft.ApplicationInsights.AspNetCore" Version="2.6.1" />
```

Het volgende toevoegen aan de `ConfigureServices` methode. Deze code kunt reguliere toepassingsbewaking met standaardconfiguratie (ServerTelemetryChannel, LiveMetrics, aanvraag/afhankelijkheden, correlatie enz.)

```csharp
services.AddApplicationInsightsTelemetry("ikeyhere");
```

In dit voorbeeld wordt de configuratie die wordt gebruikt door `ApplicationInsightsLoggerProvider` is hetzelfde als die worden gebruikt door regelmatig toepassingsbewaking. Daarom beide `ILogger` traceringen en andere telemetrie (aanvragen, afhankelijkheden, enzovoort) wordt dezelfde set uitgevoerd `TelemetryInitializers`, `TelemetryProcessors`, en `TelemetryChannel`. Ze zullen worden gecorreleerd en steekproef/niet voorbeelden worden gemaakt op dezelfde manier.

Er is echter een uitzondering op dit gedrag. De standaardwaarde `TelemetryConfiguration` is niet volledig ingesteld wanneer registreren iets uit `Program.cs` of `Startup.cs` zelf, zodat deze logboeken niet de standaardconfiguratie heeft. Elke andere logboek (bijvoorbeeld: logboeken van domeincontrollers, modellen, enzovoort) zou echter de configuratie te delen.

## <a name="control-logging-level"></a>Niveau van logboekregistratie voor controle

Naast de logboeken op code zoals in de bovenstaande voorbeelden filteren, het is ook mogelijk om te bepalen het niveau van logboekregistratie van Application Insights worden vastgelegd, door het wijzigen van de `appsettings.json`. De [ASP.NET logboekregistratie fundamentals documentatie](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.2#log-filtering) laat zien hoe u om dit te bereiken. Speciaal voor Application Insights, is de naam van de provider-alias `ApplicationInsights`, zoals weergegeven in het onderstaande voorbeeld configureren `ApplicationInsights` om vast te leggen alleen logboeken van `Warning` en hoger uit alle categorieën.

```json
{
  "Logging": {
    "ApplicationInsights": {
      "LogLevel": {
        "Default": "Warning"
      }
    },
    "LogLevel": {
      "Default": "Warning"
    }
  },
  "AllowedHosts": "*"
}
```

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

*Ik zie dat de logboeken van bepaalde ILogger tweemaal in Application Insights worden weergegeven?*

* Dit is mogelijk hebt u de oudere (nu verouderde) versie van `ApplicationInsightsLoggerProvider` ingeschakeld door het aanroepen van `AddApplicationInsights` op `ILoggerFactory`. Controleer of uw `Configure` methode heeft de volgende en verwijderen.

   ```csharp
    public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
    {
        loggerFactory.AddApplicationInsights(app.ApplicationServices, LogLevel.Warning);
        // ..other code.
    }
   ```

* Als u dubbele logboekregistratie ondervindt wanneer foutopsporing via Visual Studio, en wijzig vervolgens de code die wordt gebruikt om in te schakelen van Application Insights als volgt door in te stellen `EnableDebugLogger` moet false zijn. Dit is alleen relevant wanneer foutopsporing voor de toepassing.

   ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        ApplicationInsightsServiceOptions options = new ApplicationInsightsServiceOptions();
        options.EnableDebugLogger = false;
        services.AddApplicationInsightsTelemetry(options);
        // ..other code.
    }
   ```



## <a name="next-steps"></a>Volgende stappen

Meer informatie over:

- [ILogger op basis van logboekregistratie](https://docs.microsoft.com/aspnet/core/fundamentals/logging)
- [.NET-traceerlogboeken](../../azure-monitor/app/asp-net-trace-logs.md)
