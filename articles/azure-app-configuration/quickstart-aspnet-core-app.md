---
title: Quickstart voor Azure-app-configuratie met ASP.NET Core | Microsoft Docs
description: Een quickstart voor het gebruik van Azure-app-configuratie met ASP.NET Core-apps
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.devlang: csharp
ms.topic: quickstart
ms.tgt_pltfrm: ASP.NET Core
ms.workload: tbd
ms.date: 02/24/2019
ms.author: yegu
ms.openlocfilehash: ce19041b29d567f061dde59fbe041adf61f889a0
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56961479"
---
# <a name="quickstart-create-an-aspnet-core-app-with-azure-app-configuration"></a>Quickstart: Een ASP.NET Core-app maken met Azure-app-configuratie

Azure-app-configuratie is een beheerde configuratieservice in Azure. Hiermee kunt u eenvoudig alle toepassingsinstellingen opslaan en beheren op één plek die gescheiden is van uw code. Deze quickstart laat u zien hoe u de service kunt opnemen in een ASP.NET Core-web-app. ASP.NET Core bouwt een configuratieobject met een één-sleutelwaarde met behulp van instellingen van een of meer gegevensbronnen, ook wel *configuratieproviders* genoemd, die worden opgegeven door een toepassing. Omdat de .NET Core-client van de app-configuratie wordt geïmplementeerd als een configuratieprovider, wordt de service net als elke andere gegevensbron weergegeven.

U kunt elke code-editor gebruiken om de stappen in deze snelstart uit te voeren. [Visual Studio Code](https://code.visualstudio.com/) is een uitstekende optie die beschikbaar is op de Windows-, macOS- en Linux-platforms.

## <a name="prerequisites"></a>Vereisten

Om deze quickstart te kunnen voltooien, moet u de [.NET Core-SDK](https://dotnet.microsoft.com/download) installeren.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-app-configuration-store"></a>Een app-configuratiearchief maken

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

## <a name="create-an-aspnet-core-web-app"></a>Een ASP.NET Core-web-app maken

U gebruikt de [opdrachtregelinterface (CLI) van .NET Core](https://docs.microsoft.com/dotnet/core/tools/) om een nieuw web-app-project van ASP.NET Core MVC te maken. Het voordeel van de CLI van .NET Core ten opzichte van Visual Studio is dat de interface beschikbaar is voor Windows, macOS en Linux.

1. Maak een nieuwe map voor uw project. Voor deze quickstart noemen we deze *TestAppConfig*.

2. Voer in de nieuwe map de volgende opdracht uit om een nieuw web-app-project van ASP.NET Core MVC te maken:

        dotnet new mvc

## <a name="add-secret-manager"></a>Secret Manager toevoegen

Voeg het [hulpprogramma Secret Manager](https://docs.microsoft.com/aspnet/core/security/app-secrets) toe aan uw project. Dit hulpprogramma slaat gevoelige gegevens voor ontwikkeltaken op buiten de projectstructuur. Deze aanpak voorkomt dat er per ongeluk appgeheimen worden gedeeld in de broncode.

- Open het bestand *.csproj*. Voeg een `UserSecretsId`-element toe, zoals hieronder wordt weergegeven, en vervang de waarde ervan door uw eigen waarde. Meestal is dit een GUID. Sla het bestand op.

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

## <a name="connect-to-app-configuration-store"></a>Verbinding maken met een app-configuratiearchief

1. Voeg een verwijzing naar het NuGet-pakket `Microsoft.Extensions.Configuration.AzureAppConfiguration` door de volgende opdracht uit te voeren:

        dotnet add package Microsoft.Extensions.Configuration.AzureAppConfiguration

2. Voer de volgende opdracht uit om pakketten voor uw project te herstellen.

        dotnet restore

3. Voeg een geheim met de naam *ConnectionStrings:AppConfig* toe aan Secret Manager.

    Dit geheim bevat straks de verbindingsreeks voor toegang tot uw app-configuratiearchief. Vervang de waarde in de onderstaande opdracht door de verbindingsreeks voor uw app-configuratiearchief.

    Deze opdracht moet worden uitgevoerd in de map met het bestand *.csproj*.

        dotnet user-secrets set ConnectionStrings:AppConfig "Endpoint=<your_endpoint>;Id=<your_id>;Secret=<your_secret>"

    Secret Manager wordt alleen gebruikt om de web-app lokaal te testen. Wanneer de app is geïmplementeerd (bijvoorbeeld naar [Azure App Service](https://azure.microsoft.com/services/app-service/web)), gaat u een toepassingsinstelling (bijvoorbeeld **Verbindingsreeksen** in App Service) gebruiken in plaats van de verbindingsreeks op te slaan met Secret Manager.

    Dit geheim is toegankelijk met de configuratie-API. Een dubbele punt (:) werkt in de configuratienaam met de configuratie-API op alle ondersteunde platforms, zie [Configuratie per omgeving](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/index?tabs=basicconfiguration&view=aspnetcore-2.0#configuration-by-environment).

4. Open *Program.cs* en werk de methode `CreateWebHostBuilder` bij voor het gebruik van app-configuratie door de methode `config.AddAzureAppConfiguration()` aan te roepen.

    ```csharp
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                var settings = config.Build();
                config.AddAzureAppConfiguration(settings["ConnectionStrings:AppConfig"]);
            })
            .UseStartup<Startup>();
    ```

5. Open *Index.cshtml* in de directory *Views* > *Home* en vervang de inhoud ervan door de volgende code:

    ```html
    @using Microsoft.Extensions.Configuration
    @inject IConfiguration Configuration

    <!DOCTYPE html>
    <html lang="en">
    <style>
        body {
            background-color: @Configuration["TestApp:Settings:BackgroundColor"]
        }
        h1 {
            color: @Configuration["TestApp:Settings:FontColor"];
            font-size: @Configuration["TestApp:Settings:FontSize"];
        }
    </style>
    <head>
        <title>Index View</title>
    </head>
    <body>
        <h1>@Configuration["TestApp:Settings:Message"]</h1>
    </body>
    </html>
    ```

6. Open *_Layout.cshtml* in de directory *Views* > *Shared* en vervang de inhoud ervan door de volgende code:

    ```html
    <!DOCTYPE html>
    <html>
    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>@ViewData["Title"] - hello_world</title>

        <link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.css" />
        <link rel="stylesheet" href="~/css/site.css" />
    </head>
    <body>
        <div class="container body-content">
            @RenderBody()
        </div>

        <script src="~/lib/jquery/dist/jquery.js"></script>
        <script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
        <script src="~/js/site.js" asp-append-version="true"></script>

        @RenderSection("Scripts", required: false)
    </body>
    </html>
    ```

## <a name="build-and-run-the-app-locally"></a>De app lokaal compileren en uitvoeren

1. Compileer de app met behulp van de .NET Core CLI door de volgende opdracht uit te voeren in de opdrachtshell:

        dotnet build

2. Als het compileren is voltooid, voert u de volgende opdracht uit om de web-app lokaal uit te voeren:

        dotnet run

3. Open een browservenster en navigeer naar `http://localhost:5000`. Dit is de standaard-URL voor de web-app die lokaal wordt gehost.

    ![Quickstart voor het lokaal starten van een app](./media/quickstarts/aspnet-core-app-launch-local.png)

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Volgende stappen

In deze quickstart hebt u een nieuw app-configuratiearchief gemaakt en dat gebruikt met een ASP.NET Core-web-app. Als u meer wilt weten over het gebruik van de app-configuratie, gaat u verder met de volgende zelfstudie, waarin verificatie wordt behandeld.

> [!div class="nextstepaction"]
> [Beheerde identiteiten voor integratie met Azure-resources](./integrate-azure-managed-service-identity.md)
