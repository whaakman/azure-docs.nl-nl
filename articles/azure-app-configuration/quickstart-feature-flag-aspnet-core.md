---
title: Snelstartgids voor het toevoegen van de functie vlaggen aan ASP.NET Core | Microsoft Docs
description: Een snelstartgids voor functie vlaggen aan ASP.NET Core-apps toe te voegen en deze beheren in Azure App-configuratie
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: maiye
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.devlang: csharp
ms.topic: quickstart
ms.tgt_pltfrm: ASP.NET Core
ms.workload: tbd
ms.date: 04/19/2019
ms.author: yegu
ms.openlocfilehash: 95f702b1d85dc8fe22b1800df3f7b0ebc987bee5
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65412383"
---
# <a name="quickstart-add-feature-flags-to-an-aspnet-core-app"></a>Quickstart: Vlaggen voor functie toevoegen aan een ASP.NET Core-app

Beheer van de functie in ASP.NET Core kan worden ingeschakeld door uw toepassing in Azure App-configuratie verbinding te maken. U kunt deze beheerde service kunt gebruiken voor het opslaan van alle vlaggen voor uw functie en hun status centraal beheren. In deze Quick Start laat zien hoe de service opnemen in een ASP.NET Core web-app om een end-to-end implementatie van het beheer van de functie te maken.

De .NET Core functie Management-bibliotheken Breid het framework met ondersteuning van de vlag uitgebreide functies. Ze zijn gebaseerd op het systeem van de configuratie van .NET Core. Ze integreren naadloos met App-configuratie door de configuratieprovider voor de .NET Core.

Een code-editor kunt u de stappen in deze Quick Start. [Visual Studio Code](https://code.visualstudio.com/) is een uitstekende optie beschikbaar is op Windows, macOS en Linux-platforms.

## <a name="prerequisites"></a>Vereisten

Als u wilt doen in deze Quick Start, installeert de [.NET Core SDK](https://dotnet.microsoft.com/download).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-app-configuration-store"></a>Een app-configuratiearchief maken

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

6. Selecteer **functie Manager** > **+ maken** om toe te voegen van de volgende functie-vlaggen:

    | Sleutel | Status |
    |---|---|
    | Bèta | Uit |

## <a name="create-an-aspnet-core-web-app"></a>Een ASP.NET Core-web-app maken

U gebruikt de [.NET Core-opdrachtregelinterface (CLI)](https://docs.microsoft.com/dotnet/core/tools/) te maken van een nieuwe ASP.NET Core MVC-app-webproject. Het voordeel van het gebruik van de .NET Core-CLI via Visual Studio is dat het is beschikbaar voor de Windows, macOS en Linux-platforms.

1. Maak een nieuwe map voor uw project. Voor deze Quick Start, geef deze de naam *TestFeatureFlags*.

2. Voer de volgende opdracht om een nieuwe ASP.NET Core MVC-app-webproject maken in de nieuwe map:

        dotnet new mvc

## <a name="add-secret-manager"></a>Secret Manager toevoegen

Voeg de [Secret Manager hulpprogramma](https://docs.microsoft.com/aspnet/core/security/app-secrets) aan uw project. Dit hulpprogramma slaat gevoelige gegevens voor ontwikkeltaken op buiten de projectstructuur. Deze aanpak voorkomt dat er per ongeluk appgeheimen worden gedeeld in de broncode.

- Open de *.csproj* bestand. Voeg een `UserSecretsId` element zoals hieronder, en vervang de waarde door uw eigen, dit meestal een GUID is. Sla het bestand op.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.Web">

    <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <UserSecretsId>79a3edd0-2092-40a2-a04d-dcb46d5ca9ed</UserSecretsId>
    </PropertyGroup>

    <ItemGroup>
        <PackageReference Include="Microsoft.AspNetCore.App" />
        <PackageReference Include="Microsoft.AspNetCore.Razor.Design" Version="2.1.2" PrivateAssets="All" />
    </ItemGroup>

    </Project>
    ```

## <a name="connect-to-an-app-configuration-store"></a>Verbinding maken met een app-configuratiearchief

1. Voeg verwijzingen toe aan de `Microsoft.Extensions.Configuration.AzureAppConfiguration` en `Microsoft.FeatureManagement` NuGet-pakketten door het uitvoeren van de volgende opdrachten:

        dotnet add package Microsoft.Extensions.Configuration.AzureAppConfiguration --version 1.0.0-preview-008520001

        dotnet add package Microsoft.FeatureManagement.AspNetCore --version 1.0.0-preview-008560001-910

2. Voer de volgende opdracht om te herstellen van pakketten voor uw project:

        dotnet restore

3. Voeg een geheim met de naam *ConnectionStrings:AppConfig* toe aan Secret Manager.

    Dit geheim bevat de verbindingsreeks voor toegang tot de store van uw app-configuratie. De waarde in de volgende opdracht vervangt door de verbindingsreeks voor het opslaan van de app-configuratie.

    Deze opdracht moet worden uitgevoerd in de map met het bestand *.csproj*.

        dotnet user-secrets set ConnectionStrings:AppConfig <your_connection_string>

    Secret Manager wordt alleen gebruikt om de web-app lokaal testen. Wanneer de app wordt geïmplementeerd op [Azure App Service](https://azure.microsoft.com/services/app-service/web), bijvoorbeeld, u een toepassingsinstelling **verbindingsreeksen** in App Service in plaats van met Secret Manager voor het opslaan van de verbindingsreeks.

    Dit geheim wordt geopend met de configuratie-API. Een dubbele punt (:) werkt in de configuratienaam van de met de configuratie-API op alle ondersteunde platforms. Zie [configuratie omgeving](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/index?tabs=basicconfiguration&view=aspnetcore-2.0).

4. Open *Program.cs*, en voeg een verwijzing naar de configuratie van .NET Core-App-provider.

    ```csharp
    using Microsoft.Extensions.Configuration.AzureAppConfiguration;
    ```

5. Update de `CreateWebHostBuilder` methode voor het gebruik van App-configuratie door het aanroepen van de `config.AddAzureAppConfiguration()` methode.

    ```csharp
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                var settings = config.Build();
                config.AddAzureAppConfiguration(options => {
                    options.Connect(settings["ConnectionStrings:AppConfig"])
                           .UseFeatureFlags();
                });
            })
            .UseStartup<Startup>();
    ```

6. Open *Startup.cs*, en voeg verwijzingen naar de manager van de functie .NET Core.

    ```csharp
    using Microsoft.FeatureManagement.AspNetCore;
    ```

7. Update de `ConfigureServices` methode om toe te voegen functievlag ondersteuning door het aanroepen van de `services.AddFeatureManagement()` methode en eventueel een filter dat moet worden gebruikt met de functie vlaggen door het aanroepen van `services.AddFeatureFilter<FilterType>()`:

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddFeatureManagement();
    }
    ```

8. Voeg een *MyFeatureFlags.cs* bestand.

    ```csharp
    namespace TestFeatureFlags
    {
        public enum MyFeatureFlags
        {
            Beta
        }
    }
    ```

9. Voeg *BetaController.cs* naar de map domeincontrollers:

    ```csharp
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.FeatureManagement.AspNetCore;

    namespace TestFeatureFlags.Controllers
    {
        public class BetaController: Controller
        {
            private readonly IFeatureManager _featureManager;

            public BetaController(IFeatureManagerSnapshot featureManager)
            {
                _featureManager = featureManager;
            }

            [Feature(MyFeatureFlags.Beta)]
            public IActionResult Index()
            {
                return View();
            }
        }
    }
    ```

10. Open *_ViewImports.cshtml* in de map weergaven en de functie manager tag helper toe te voegen:

    ```html
    @addTagHelper *, Microsoft.FeatureManagement.AspNetCore
    ```

11. Open *_Layout.cshtml* in de weergaven > gedeelde map en vervangen, om de `<nav>` balk onder `<body>`  >  `<header>` door de volgende code:

    ```html
    <nav class="navbar navbar-expand-sm navbar-toggleable-sm navbar-light bg-white border-bottom box-shadow mb-3">
        <div class="container">
            <a class="navbar-brand" asp-area="" asp-controller="Home" asp-action="Index">TestFeatureFlags</a>
            <button class="navbar-toggler" type="button" data-toggle="collapse" data-target=".navbar-collapse" aria-controls="navbarSupportedContent"
            aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
            </button>
            <div class="navbar-collapse collapse d-sm-inline-flex flex-sm-row-reverse">
                <ul class="navbar-nav flex-grow-1">
                    <li class="nav-item">
                        <a class="nav-link text-dark" asp-area="" asp-controller="Home" asp-action="Index">Home</a>
                    </li>
                    <feature name="Beta">
                    <li class="nav-item">
                        <a class="nav-link text-dark" asp-area="" asp-controller="Beta" asp-action="Index">Beta</a>
                    </li>
                    </feature>
                    <li class="nav-item">
                        <a class="nav-link text-dark" asp-area="" asp-controller="Home" asp-action="Privacy">Privacy</a>
                    </li>
                </ul>
            </div>
        </div>
    </nav>
    ```

12. Maak een map bèta onder weergaven en voeg *Index.cshtml* toe:

    ```html
    @{
        ViewData["Title"] = "Beta Home Page";
    }

    <h1>
        This is the beta website.
    </h1>
    ```

## <a name="build-and-run-the-app-locally"></a>De app lokaal compileren en uitvoeren

1. Voor het bouwen van de app met behulp van .NET Core CLI, voer de volgende opdracht in de opdrachtshell:

        dotnet build

2. Nadat de build is voltooid, voert u de volgende opdracht om de web-app lokaal uitvoeren:

        dotnet run

3. Open een browservenster en Ga naar `https://localhost:5001`, dit is de standaard-URL voor de web-app die lokaal wordt gehost.

    ![Quickstart voor het lokaal starten van een app](./media/quickstarts/aspnet-core-feature-flag-local-before.png)

4. Meld u aan bij [Azure Portal](https://aka.ms/azconfig/portal). Selecteer **alle resources**, en selecteer het app-configuratie store-exemplaar dat u in de Quick Start hebt gemaakt.

5. Selecteer **functie Manager**, en wijzig de waarde van *Bèta* naar *op*:

    | Sleutel | Status |
    |---|---|
    | Bèta | Aan |

6. Vernieuw de browserpagina om de nieuwe configuratie-instellingen te zien.

    ![Quickstart voor het lokaal starten van een app](./media/quickstarts/aspnet-core-feature-flag-local-after.png)

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Volgende stappen

In deze quickstart maakt u een nieuwe app-configuratie-archief hebt gemaakt en gebruikt voor het beheren van functies in een ASP.NET Core web-app via de [functie beheerbibliotheken](https://go.microsoft.com/fwlink/?linkid=2074664).

* Meer informatie over [functie management](./concept-feature-management.md)
* [Functievlaggen beheren](./manage-feature-flags.md)
* [Functie vlaggen gebruiken in een ASP.NET Core-app](./use-feature-flags-dotnet-core.md)
