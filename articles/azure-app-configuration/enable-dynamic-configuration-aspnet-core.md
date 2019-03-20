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
ms.devlang: na
ms.topic: tutorial
ms.date: 02/24/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: d5e1e5989f9902d9ab8dcc3e6c2b9d2a71f12df9
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58224376"
---
# <a name="tutorial-use-dynamic-configuration-in-an-aspnet-core-app"></a>Zelfstudie: Dynamische configuratie in een ASP.NET Core-app gebruiken

ASP.NET Core is een pluggable configuratiesysteem die configuratiegegevens kan worden gelezen uit een groot aantal bronnen. Wijzigingen op elk gewenst moment kan verwerken zonder dat een toepassing te starten. ASP.NET Core biedt ondersteuning voor de binding van configuratie-instellingen voor sterk getypeerde .NET-klassen. Het ze in uw code met behulp van de verschillende injects `IOptions<T>` patronen. Een van deze patronen specifiek `IOptionsSnapshot<T>`, automatisch opnieuw laden van de configuratie van de toepassing wanneer de onderliggende gegevens worden gewijzigd. 

U kunt `IOptionsSnapshot<T>` in domeincontrollers in uw toepassing invoeren voor toegang tot de meest recente configuratiegegevens die zijn opgeslagen in Azure-app-configuratie. U kunt er ook voor instellen van de App-configuratie ASP.NET Core-clientbibliotheek continu bewaken en eventuele wijzigingen in een app-configuratiearchief ophalen. U definieert de periodiek interval voor de polling.

In deze zelfstudie leert hoe u dynamische configuratie-updates kunt implementeren in uw code. Dit is gebaseerd op de web-app die is geïntroduceerd in de quickstarts. Voordat u doorgaat, voltooien [maken van een ASP.NET Core-app met App-configuratie](./quickstart-aspnet-core-app.md) eerste.

Een code-editor kunt u de stappen in deze Quick Start. [Visual Studio Code](https://code.visualstudio.com/) is een uitstekende optie die beschikbaar is op Windows, macOS en Linux-platforms.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Instellen van uw toepassing om bij te werken van de configuratie ervan in reactie op wijzigingen in een configuratie van apps.
> * De meest recente configuratie in de controllers van uw toepassing invoeren.

## <a name="prerequisites"></a>Vereisten

Als u wilt doen in deze Quick Start, installeert de [.NET Core SDK](https://dotnet.microsoft.com/download).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="reload-data-from-app-configuration"></a>Gegevens opnieuw laden vanuit app-configuratie

1. Open Program.cs en werk de `CreateWebHostBuilder` methode door toe te voegen de `config.AddAzureAppConfiguration()` methode.

    ```csharp
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                var settings = config.Build();
                config.AddAzureAppConfiguration(o => o.Connect(settings["ConnectionStrings:AppConfig"])
                    .Watch("TestApp:Settings:BackgroundColor", TimeSpan.FromSeconds(1))
                    .Watch("TestApp:Settings:FontColor", TimeSpan.FromSeconds(1))
                    .Watch("TestApp:Settings:Message", TimeSpan.FromSeconds(1)));
            })
            .UseStartup<Startup>();
    ```

    De tweede parameter in de `.Watch` methode is het polling-interval waarmee de client-bibliotheek voor ASP.NET-query's opslaan van de configuratie van een app. De clientbibliotheek controleert of het specifieke configuratie-instellingen om te bekijken of eventuele wijzigingen zijn opgetreden.

2. Toevoegen van een bestand Settings.cs die definieert en implementeert een nieuwe `Settings` klasse.

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

3. Open Startup.cs en werk de `ConfigureServices` methode voor het binden van configuratiegegevens naar het `Settings` klasse.

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

## <a name="use-the-latest-configuration-data"></a>De meest recente configuratiegegevens gebruiken

1. Open HomeController.cs in de map domeincontrollers. Update de `HomeController` klasse voor het ontvangen van `Settings` via afhankelijkheidsinjectie uit en controleer het gebruik van de waarden ervan.

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

2. Index.cshtml in de weergaven openen > Home-directory en vervang de inhoud ervan door het volgende script:

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

4. Meld u aan bij [Azure Portal](https://aka.ms/azconfig/portal). Selecteer **alle resources**, en selecteer het app-configuratie store-exemplaar dat u in de Quick Start hebt gemaakt.

5. Selecteer **sleutel/waarde-Explorer**, en de waarden voor de volgende sleutels:

    | Sleutel | Value |
    |---|---|
    | TestAppSettings:BackgroundColor | blue |
    | TestAppSettings:FontColor | lightGray |
    | TestAppSettings:Message | Gegevens uit Azure-app-configuratie - nu met live updates! |

6. Vernieuw de browserpagina om de nieuwe configuratie-instellingen te zien.

    ![Quickstart voor het lokaal vernieuwen van een app](./media/quickstarts/aspnet-core-app-launch-local-after.png)

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een beheerde Azure-service-identiteit toegevoegd om toegang tot app-configuratie te stroomlijnen en het beheer van referenties voor uw app te verbeteren. Voor meer informatie over het gebruik van App-configuratie, gaat u naar de Azure CLI-voorbeelden.

> [!div class="nextstepaction"]
> [CLI-voorbeelden](./cli-samples.md)
