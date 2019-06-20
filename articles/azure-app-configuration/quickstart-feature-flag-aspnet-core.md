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
ms.openlocfilehash: bd812ad1194f88b14d88f067583ca6eee4bb0c74
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67274206"
---
# <a name="quickstart-add-feature-flags-to-an-aspnet-core-app"></a>Quickstart: Vlaggen voor functie toevoegen aan een ASP.NET Core-app

U kunt het beheer van de functie in ASP.NET Core inschakelen door verbinding te maken van uw toepassing in Azure App-configuratie. U kunt deze beheerde service kunt gebruiken voor het opslaan van alle vlaggen voor uw functie en hun status centraal beheren. In deze Quick Start laat zien hoe u App-configuratie opnemen in een ASP.NET Core web-app voor het maken van een end-to-end-implementatie van de functie voor beheer.

De .NET Core functie Management-bibliotheken Breid het framework met ondersteuning van de vlag uitgebreide functies. Deze bibliotheken zijn gebaseerd op het systeem van de configuratie van .NET Core. Ze integreren naadloos met App-configuratie door de configuratieprovider voor de .NET Core.

Een code-editor kunt u de stappen in deze Quick Start. [Visual Studio Code](https://code.visualstudio.com/) is een uitstekende optie beschikbaar is op Windows, macOS en Linux-platforms.

## <a name="prerequisites"></a>Vereisten

Als u wilt doen in deze Quick Start, installeert de [.NET Core SDK](https://dotnet.microsoft.com/download).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-app-configuration-store"></a>Maken van een configuratie van de App store

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

6. Selecteer **functie Manager** >  **+ maken** om toe te voegen van de volgende functie-vlaggen:

    | Sleutel | Status |
    |---|---|
    | Bèta | Uit |

## <a name="create-an-aspnet-core-web-app"></a>Een ASP.NET Core-web-app maken

U gebruikt de [.NET Core-opdrachtregelinterface (CLI)](https://docs.microsoft.com/dotnet/core/tools/) te maken van een nieuwe ASP.NET Core MVC-app-webproject. Het voordeel van het gebruik van de .NET Core-CLI in plaats van Visual Studio is dat de .NET Core-CLI beschikbaar voor de Windows, macOS en Linux-platforms is.

1. Maak een nieuwe map voor uw project. Voor deze Quick Start, geef deze de naam *TestFeatureFlags*.

1. Voer de volgende opdracht om een nieuwe ASP.NET Core MVC-app-webproject maken in de nieuwe map:

   ```    
   dotnet new mvc
   ```

## <a name="add-secret-manager"></a>Secret Manager toevoegen

Voeg de [Secret Manager hulpprogramma](https://docs.microsoft.com/aspnet/core/security/app-secrets) aan uw project. Het hulpprogramma Secret Manager slaat de gevoelige gegevens voor projecten buiten de projectstructuur van uw. Deze aanpak voorkomt dat er per ongeluk appgeheimen worden gedeeld in de broncode.

1. Open de *.csproj* bestand.
1. Voeg een `UserSecretsId` element zoals wordt weergegeven in het volgende voorbeeld en vervang de waarde door uw eigen, dit meestal een GUID is:

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

1. Sla het bestand op.

## <a name="connect-to-an-app-configuration-store"></a>Verbinding maken met een configuratie van de App store

1. Voeg verwijzingen toe aan de `Microsoft.Extensions.Configuration.AzureAppConfiguration` en `Microsoft.FeatureManagement` NuGet-pakketten door het uitvoeren van de volgende opdrachten:

    ```
    dotnet add package Microsoft.Extensions.Configuration.AzureAppConfiguration --version 1.0.0-preview-008920001-990

    dotnet add package Microsoft.FeatureManagement.AspNetCore --version 1.0.0-preview-009000001-1251
    ```

1. Voer de volgende opdracht om te herstellen van pakketten voor uw project:

    ```
    dotnet restore
    ```

1. Voeg een geheim met de naam **ConnectionStrings:AppConfig** toe aan Secret Manager.

    Dit geheim bevat de verbindingsreeks voor toegang tot de store van uw App-configuratie. Vervang de `<your_connection_string>` waarde in de volgende opdracht uit met de verbindingsreeks voor het opslaan van de App-configuratie.

    Deze opdracht moet worden uitgevoerd in de map met het bestand *.csproj*.

    ```
    dotnet user-secrets set ConnectionStrings:AppConfig <your_connection_string>
    ```

    U Secret Manager alleen gebruiken voor de web-app lokaal testen. Wanneer u de app implementeert [Azure App Service](https://azure.microsoft.com/services/app-service), bijvoorbeeld, u een toepassing instellen met de naam **verbindingsreeksen** in App Service in plaats van met Secret Manager voor het opslaan van de verbindingsreeks.

    U kunt toegang tot dit geheim met de App-configuratie-API. Een dubbele punt (:) werkt in de configuratienaam van de met de App-configuratie-API op alle ondersteunde platforms. Zie [configuratie omgeving](https://docs.microsoft.com/aspnet/core/fundamentals/configuration).

1. Open *Program.cs*, en voeg een verwijzing naar de configuratie van .NET Core-App-provider:

    ```csharp
    using Microsoft.Extensions.Configuration.AzureAppConfiguration;
    ```

1. Update de `CreateWebHostBuilder` methode voor het gebruik van App-configuratie door het aanroepen van de `config.AddAzureAppConfiguration()` methode.

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

1. Open *Startup.cs*, en verwijzingen naar de manager van de functie .NET Core toe te voegen:

    ```csharp
    using Microsoft.FeatureManagement.AspNetCore;
    ```

1. Update de `ConfigureServices` methode om toe te voegen functievlag ondersteuning door het aanroepen van de `services.AddFeatureManagement()` methode. Desgewenst kunt u een filter dat moet worden gebruikt met de functie vlaggen door het aanroepen van opnemen `services.AddFeatureFilter<FilterType>()`:

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddFeatureManagement();
    }
    ```

1. Voeg een *MyFeatureFlags.cs* bestand:

    ```csharp
    namespace TestFeatureFlags
    {
        public enum MyFeatureFlags
        {
            Beta
        }
    }
    ```

1. Voeg *BetaController.cs* naar de *Controllers* directory:

    ```csharp
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.FeatureManagement;
    using Microsoft.FeatureManagement.Mvc;

    namespace TestFeatureFlags.Controllers
    {
        public class BetaController: Controller
        {
            private readonly IFeatureManager _featureManager;

            public BetaController(IFeatureManagerSnapshot featureManager)
            {
                _featureManager = featureManager;
            }

            [FeatureGate(MyFeatureFlags.Beta)]
            public IActionResult Index()
            {
                return View();
            }
        }
    }
    ```

1. Open *_ViewImports.cshtml* in de *weergaven* directory, en de functie manager tag helper toe te voegen:

    ```html
    @addTagHelper *, Microsoft.FeatureManagement.AspNetCore
    ```

1. Open *_Layout.cshtml* in de *weergaven*\\*gedeelde* directory en vervang de `<nav>` streepjescode onder `<body>`  >  `<header>` door de volgende code:

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

1. Maak een *Bèta* map onder *weergaven* en toe te voegen *Index.cshtml* toe:

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

    ```
    dotnet build
    ```

1. Nadat de build is voltooid, voert u de volgende opdracht om de web-app lokaal uitvoeren:

    ```
    dotnet run
    ```

1. Open een browservenster en Ga naar `https://localhost:5001`, dit is de standaard-URL voor de web-app die lokaal wordt gehost.

    ![Quickstart voor het lokaal starten van een app](./media/quickstarts/aspnet-core-feature-flag-local-before.png)

1. Meld u aan bij [Azure Portal](https://portal.azure.com). Selecteer **alle resources**, en selecteer de configuratie van de App store-instantie die u hebt gemaakt in de Quick Start.

1. Selecteer **functie Manager**, en wijzig de status van de **Bèta** op **op**:

    | Sleutel | Status |
    |---|---|
    | Bèta | Aan |

1. Vernieuw de browserpagina om de nieuwe configuratie-instellingen te zien.

    ![Quickstart voor het lokaal starten van een app](./media/quickstarts/aspnet-core-feature-flag-local-after.png)

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Volgende stappen

In deze quickstart maakt u een nieuwe App-configuratie-archief hebt gemaakt en gebruikt voor het beheren van functies in een ASP.NET Core web-app via de [functie beheerbibliotheken](https://go.microsoft.com/fwlink/?linkid=2074664).

- Meer informatie over [management functie](./concept-feature-management.md).
- [Functie vlaggen beheren](./manage-feature-flags.md).
- [Functie vlaggen gebruiken in een ASP.NET Core-app](./use-feature-flags-dotnet-core.md).
