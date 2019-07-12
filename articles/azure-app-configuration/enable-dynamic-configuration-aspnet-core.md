---
title: Zelfstudie voor het gebruik van dynamische configuratie van Azure-app-configuratie in een ASP.NET Core-app | Microsoft Docs
description: In deze zelfstudie leert u hoe u de configuratiegegevens voor ASP.NET Core-apps dynamisch bijwerkt
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.devlang: csharp
ms.topic: tutorial
ms.date: 02/24/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: 78c64786f523aa424e8a9816e42db70e2a2997c2
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67798454"
---
# <a name="tutorial-use-dynamic-configuration-in-an-aspnet-core-app"></a>Zelfstudie: Dynamische configuratie in een ASP.NET Core-app gebruiken

ASP.NET Core is een pluggable configuratiesysteem die configuratiegegevens kan worden gelezen uit een groot aantal bronnen. Wijzigingen op elk gewenst moment kan verwerken zonder dat een toepassing te starten. ASP.NET Core biedt ondersteuning voor de binding van configuratie-instellingen voor sterk getypeerde .NET-klassen. Het ze in uw code met behulp van de verschillende injects `IOptions<T>` patronen. Een van deze patronen specifiek `IOptionsSnapshot<T>`, automatisch opnieuw laden van de configuratie van de toepassing wanneer de onderliggende gegevens worden gewijzigd. U kunt `IOptionsSnapshot<T>` in domeincontrollers in uw toepassing invoeren voor toegang tot de meest recente configuratiegegevens die zijn opgeslagen in Azure-app-configuratie.

U kunt ook de App-configuratie ASP.NET Core-clientbibliotheek instellen om te vernieuwen van een set van configuratie-instellingen dynamisch met behulp van een middleware. Als de web-app blijft om aanvragen te ontvangen, blijven de configuratie-instellingen worden bijgewerkt met het opslaan van de configuratie.

Een cache wordt gebruikt om de instellingen bijgewerkt te houden en te voorkomen dat te veel aanroepen voor het opslaan van de configuratie, voor elke instelling. Totdat de waarde in de cache van een instelling is verlopen, wordt de vernieuwingsbewerking niet de waarde bijgewerkt, zelfs wanneer de waarde in het opslaan van de configuratie is gewijzigd. De verlooptijd van de standaard voor elke aanvraag is 30 seconden, maar deze kan worden genegeerd indien nodig.

In deze zelfstudie leert hoe u dynamische configuratie-updates kunt implementeren in uw code. Dit is gebaseerd op de web-app die is geïntroduceerd in de quickstarts. Voordat u doorgaat, voltooien [maken van een ASP.NET Core-app met App-configuratie](./quickstart-aspnet-core-app.md) eerste.

Een code-editor kunt u de stappen in deze zelfstudie doet. [Visual Studio Code](https://code.visualstudio.com/) is een uitstekende optie die beschikbaar is op Windows, macOS en Linux-platforms.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Instellen van uw toepassing om bij te werken van de configuratie ervan in reactie op wijzigingen in een configuratie van apps.
> * De meest recente configuratie in de controllers van uw toepassing invoeren.

## <a name="prerequisites"></a>Vereisten

Als u deze zelfstudie wilt, installeert de [.NET Core SDK](https://dotnet.microsoft.com/download).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="reload-data-from-app-configuration"></a>Gegevens opnieuw laden vanuit app-configuratie

1. Open *Program.cs*, en werk de `CreateWebHostBuilder` methode om toe te voegen de `config.AddAzureAppConfiguration()` methode.

    ```csharp
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                var settings = config.Build();

                config.AddAzureAppConfiguration(options =>
                {
                    options.Connect(settings["ConnectionStrings:AppConfig"])
                           .ConfigureRefresh(refresh =>
                           {
                               refresh.Register("TestApp:Settings:BackgroundColor")
                                      .Register("TestApp:Settings:FontColor")
                                      .Register("TestApp:Settings:Message")
                           });
                }
            })
            .UseStartup<Startup>();
    ```

    De `ConfigureRefresh` methode wordt gebruikt om op te geven van de instellingen die wordt gebruikt voor het bijwerken van de configuratiegegevens met het app-configuratiearchief, wanneer een vernieuwingsbewerking wordt geactiveerd. Wilt u daadwerkelijk een vernieuwingsbewerking activeert, moet een middleware vernieuwen worden geconfigureerd voor de toepassing de om configuratiegegevens te vernieuwen wanneer een wijziging optreedt.

2. Voeg een *Settings.cs*-bestand toe dat een nieuwe `Settings`-klasse definieert en implementeert.

    ```csharp
    namespace TestAppConfig
    {
        public class Settings
        {
            public string BackgroundColor { get; set; }
            public long FontSize { get; set; }
            public string FontColor { get; set; }
            public string Message { get; set; }
        }
    }
    ```

3. Open *Startup.cs*, en werk de `ConfigureServices` methode voor het binden van configuratiegegevens naar het `Settings` klasse.

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.Configure<Settings>(Configuration.GetSection("TestApp:Settings"));

        services.Configure<CookiePolicyOptions>(options =>
        {
            options.CheckConsentNeeded = context => true;
            options.MinimumSameSitePolicy = SameSiteMode.None;
        });

        services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
    }
    ```

4. Update de `Configure` methode om toe te voegen een middleware zodat de configuratie-instellingen die zijn geregistreerd voor vernieuwen om te worden bijgewerkt terwijl de ASP.NET Core web-app blijft om aanvragen te ontvangen.

    ```csharp
    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        app.UseAzureAppConfiguration();
        app.UseMvc();
    }
    ```
    
    De middleware maakt gebruik van de configuratie vernieuwen is opgegeven in de `AddAzureAppConfiguration` methode in `Program.cs` te vernieuwen voor elke aanvraag die wordt ontvangen door de ASP.NET Core web-app. Voor elke aanvraag een vernieuwingsbewerking wordt geactiveerd en de clientbibliotheek wordt gecontroleerd of de waarde in de cache voor de geregistreerde configuratie-instellingen zijn verlopen. De waarden voor de instellingen worden bijgewerkt met het opslaan van de app-configuratie voor de cache waarden die zijn verlopen, en de resterende waarden ongewijzigd blijven.
    
    > [!NOTE]
    > De verlooptijd van de standaard-cache voor een configuratie-instelling is 30 seconden, maar kan worden overschreven door het aanroepen van de `SetCacheExpiration` methode voor de initialisatiefunctie opties doorgegeven als een argument voor de `ConfigureRefresh` methode.

## <a name="use-the-latest-configuration-data"></a>De meest recente configuratiegegevens gebruiken

1. Open *HomeController.cs* in de map van de Controllers, en voeg een verwijzing naar de `Microsoft.Extensions.Options` pakket.

    ```csharp
    using Microsoft.Extensions.Options;
    ```

2. Update de `HomeController` klasse voor het ontvangen van `Settings` via afhankelijkheidsinjectie uit en controleer het gebruik van de waarden ervan.

    ```csharp
    public class HomeController : Controller
    {
        private readonly Settings _settings;
        public HomeController(IOptionsSnapshot<Settings> settings)
        {
            _settings = settings.Value;
        }

        public IActionResult Index()
        {
            ViewData["BackgroundColor"] = _settings.BackgroundColor;
            ViewData["FontSize"] = _settings.FontSize;
            ViewData["FontColor"] = _settings.FontColor;
            ViewData["Message"] = _settings.Message;

            return View();
        }
    }
    ```

3. Open *Index.cshtml* in de weergaven > Home-directory en vervang de inhoud ervan door het volgende script:

    ```html
    <!DOCTYPE html>
    <html lang="en">
    <style>
        body {
            background-color: @ViewData["BackgroundColor"]
        }
        h1 {
            color: @ViewData["FontColor"];
            font-size: @ViewData["FontSize"];
        }
    </style>
    <head>
        <title>Index View</title>
    </head>
    <body>
        <h1>@ViewData["Message"]</h1>
    </body>
    </html>
    ```

## <a name="build-and-run-the-app-locally"></a>De app lokaal compileren en uitvoeren

1. Voor het bouwen van de app met behulp van .NET Core CLI, voer de volgende opdracht in de opdrachtshell:

        dotnet build

2. Nadat de build is voltooid, voert u de volgende opdracht om de web-app lokaal uitvoeren:

        dotnet run

3. Open een browservenster en Ga naar `http://localhost:5000`, dit is de standaard-URL voor de web-app die lokaal wordt gehost.

    ![Quickstart voor het lokaal starten van een app](./media/quickstarts/aspnet-core-app-launch-local-before.png)

4. Meld u aan bij [Azure Portal](https://portal.azure.com). Selecteer **alle resources**, en selecteer het app-configuratie store-exemplaar dat u in de Quick Start hebt gemaakt.

5. Selecteer **configuratie Explorer**, en de waarden voor de volgende sleutels:

    | Sleutel | Value |
    |---|---|
    | TestAppSettings:BackgroundColor | groen |
    | TestAppSettings:FontColor | lightGray |
    | TestAppSettings:Message | Gegevens uit Azure-app-configuratie - nu met live updates! |

6. Vernieuw de browserpagina om de nieuwe configuratie-instellingen te zien. Meer dan een vernieuwing van de browserpagina mogelijk zijn vereist voor de wijzigingen worden weergegeven.

    ![Quickstart voor het lokaal vernieuwen van een app](./media/quickstarts/aspnet-core-app-launch-local-after.png)
    
    > [!NOTE]
    > Omdat de configuratie-instellingen zijn opgeslagen in de cache met een standaard-verlooptijd van 30 seconden, eventuele wijzigingen aangebracht in de instellingen in het app-configuratiearchief zou alleen worden doorgevoerd in de web-app wanneer de cache is verlopen.

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een beheerde Azure-service-identiteit toegevoegd om toegang tot app-configuratie te stroomlijnen en het beheer van referenties voor uw app te verbeteren. Voor meer informatie over het gebruik van App-configuratie, gaat u naar de Azure CLI-voorbeelden.

> [!div class="nextstepaction"]
> [CLI-voorbeelden](./cli-samples.md)
