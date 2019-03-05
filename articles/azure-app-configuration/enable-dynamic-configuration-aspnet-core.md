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
ms.openlocfilehash: 44ae922b182874eef378d4868fb278c3c76252db
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56884411"
---
# <a name="tutorial-use-dynamic-configuration-in-an-aspnet-core-app"></a>Zelfstudie: Dynamische configuratie in een ASP.NET Core-app gebruiken

ASP.NET Core heeft een pluggable configuratiesysteem dat configuratiegegevens uit diverse bronnen kan lezen, en ook wijzigingen op elk gewenst moment kan verwerken zonder dat een toepassing opnieuw moet worden opgestart. Het biedt ondersteuning voor binding van configuratie-instellingen voor sterk getypeerde .NET-klassen en voeren deze in uw code in met behulp van de verschillende `IOptions<T>`-patronen. Een van deze patronen, met name `IOptionsSnapshot<T>`, zorgt voor het automatische opnieuw laden van de configuratie van de toepassing wanneer de onderliggende gegevens wijzigen. U kunt `IOptionsSnapshot<T>` in domeincontrollers in uw toepassing invoeren voor toegang tot de meest recente configuratiegegevens die zijn opgeslagen in Azure-app-configuratie. Bovendien kunt u de app-configuratie ASP.NET Core-clientbibliotheek continu bewaken en eventuele wijzigingen in een app-configuratie ophalen via polling op basis van een door u gedefinieerd periodiek interval.

In deze zelfstudie leert hoe u dynamische configuratie-updates kunt implementeren in uw code. Dit is gebaseerd op de web-app die is geïntroduceerd in de quickstarts. Volg eerst [Een ASP.NET Core-app maken met app-configuratie](./quickstart-aspnet-core-app.md) voordat u verder gaat.

U kunt elke code-editor gebruiken om de stappen in deze snelstart uit te voeren. [Visual Studio Code](https://code.visualstudio.com/) is een uitstekende optie die beschikbaar is op de Windows-, macOS- en Linux-platforms.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Uw toepassing instellen voor het bijwerken van de configuratie als reactie op wijzigingen in een app-configuratiearchief
> * De meest recente configuratie in de controllers van uw toepassing invoeren

## <a name="prerequisites"></a>Vereisten

Om deze quickstart te kunnen voltooien, moet u de [.NET Core-SDK](https://dotnet.microsoft.com/download) installeren.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="reload-data-from-app-configuration"></a>Gegevens opnieuw laden vanuit app-configuratie

1. Open *Program.cs* en werk de methode `CreateWebHostBuilder` bij door de methode `config.AddAzureAppConfiguration()` toe te voegen.

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

    De tweede parameter in de methode `.Watch` is het pollinginterval waarmee de ASP.NET-clientbibliotheek in een app-configuratiearchief zoekt om te zien of er een wijziging voor de specifieke configuratie-instelling is.

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

3. Open *Startup.cs* en werk de methode `ConfigureServices` voor het binden van configuratiegegevens aan de klasse `Settings` bij.

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

1. Open *HomeController.cs* in de directory *Controllers*, werk de klasse `HomeController` bij voor het ontvangen van `Settings` via afhankelijkheidsinjectie en gebruik de waarden ervan.

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

2. Open *Index.cshtml* in de directory *Views* > *Home* en vervang de inhoud ervan door het volgende:

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

1. Compileer de app met behulp van de .NET Core CLI door de volgende opdracht uit te voeren in de opdrachtshell:

        dotnet build

2. Als het compileren is voltooid, voert u de volgende opdracht uit om de web-app lokaal uit te voeren:

        dotnet run

3. Open een browservenster en navigeer naar `http://localhost:5000`. Dit is de standaard-URL voor de web-app die lokaal wordt gehost.

    ![Quickstart voor het lokaal starten van een app](./media/quickstarts/aspnet-core-app-launch-local-before.png)

4. Meld u aan bij de [Microsoft Azure-portal](https://aka.ms/azconfig/portal), klik op **Alle resources** en het exemplaar van het app-configuratiearchief dat u in de quickstart hebt gemaakt.

5. Klik op **Sleutel-waardeverkenner** en werk de waarden van de volgende sleutels bij:

    | Sleutel | Waarde |
    |---|---|
    | TestAppSettings:BackgroundColor | blue |
    | TestAppSettings:FontColor | lightGray |
    | TestAppSettings:Message | Gegevens uit Azure-app-configuratie - nu met live updates! |

6. Vernieuw de browserpagina om de nieuwe configuratie-instellingen te zien.

    ![Quickstart voor het lokaal vernieuwen van een app](./media/quickstarts/aspnet-core-app-launch-local-after.png)

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een beheerde Azure-service-identiteit toegevoegd om toegang tot app-configuratie te stroomlijnen en het beheer van referenties voor uw app te verbeteren. Voor meer informatie over het gebruik van app-configuratie, gaat u verder naar de Azure CLI-voorbeelden.

> [!div class="nextstepaction"]
> [CLI-voorbeelden](./cli-samples.md)
