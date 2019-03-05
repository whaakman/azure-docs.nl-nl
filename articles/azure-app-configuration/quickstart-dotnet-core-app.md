---
title: Quickstart voor Azure App Configuration met .NET Core | Microsoft Docs
description: Een quickstart voor het gebruik van Azure App Configuration met .NET Core-apps
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.devlang: csharp
ms.topic: quickstart
ms.tgt_pltfrm: .NET Core
ms.workload: tbd
ms.date: 02/24/2019
ms.author: yegu
ms.openlocfilehash: cd4115aaeec15d14d48dcb71cbdc75212c6dc2db
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56960668"
---
# <a name="quickstart-create-an-net-core-app-with-app-configuration"></a>Quickstart: Een .NET Core-app maken met App Configuration

Azure App Configuration is een beheerde configuratieservice in Azure. Hiermee kunt u eenvoudig alle toepassingsinstellingen opslaan en beheren op één plek die gescheiden is van uw code. Deze quickstart laat u zien hoe u de service kunt opnemen in een .NET Core-console-app.

U kunt elke code-editor gebruiken om de stappen in deze snelstart uit te voeren. [Visual Studio Code](https://code.visualstudio.com/) is een uitstekende optie die beschikbaar is op de Windows-, macOS- en Linux-platforms.

## <a name="prerequisites"></a>Vereisten

Om deze quickstart te kunnen voltooien, moet u de [.NET Core-SDK](https://dotnet.microsoft.com/download) installeren.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-app-configuration-store"></a>Een app-configuratiearchief maken

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

## <a name="create-a-net-core-console-app"></a>Een .NET Core-console-app maken

U gebruikt de [opdrachtregelinterface (CLI) van .NET Core](https://docs.microsoft.com/dotnet/core/tools/) om een nieuw web-app-project van .NET Core Console te maken. Het voordeel van de CLI van .NET Core ten opzichte van Visual Studio is dat de interface beschikbaar is voor Windows, macOS en Linux.

1. Maak een nieuwe map voor uw project.

2. Voer in de nieuwe map de volgende opdracht uit om een nieuw web-app-project van ASP.NET Core MVC te maken:

        dotnet new console

## <a name="connect-to-app-configuration-store"></a>Verbinding maken met een app-configuratiearchief

1. Voeg een verwijzing naar het NuGet-pakket `Microsoft.Extensions.Configuration.AzureAppConfiguration` door de volgende opdracht uit te voeren:

        dotnet add package Microsoft.Extensions.Configuration.AzureAppConfiguration

2. Voer de volgende opdracht uit om pakketten voor uw project te herstellen.

        dotnet restore

3. Open *Program.cs* en werk de `Main`-methode bij voor het gebruik van App Configuration door de `builder.AddAzureAppConfiguration()`-methode aan te roepen.

    ```csharp
    static void Main(string[] args)
    {
        var builder = new ConfigurationBuilder();
        builder.AddAzureAppConfiguration(Environment.GetEnvironmentVariable("ConnectionString"));

        var config = builder.Build();
        Console.WriteLine(config["TestApp:Settings:Message"] ?? "Hello world!");
    }
    ```

## <a name="build-and-run-the-app-locally"></a>De app lokaal bouwen en uitvoeren

1. Stel een omgevingsvariabele met de naam **ConnectionString** in en stel deze in op de toegangssleutel van uw app-configuratiearchief. Als u de Windows-opdrachtprompt gebruikt, voert u de volgende opdracht uit en start u de opdrachtprompt opnieuw op om de wijziging door te voeren:

        setx ConnectionString "connection-string-of-your-app-configuration-store"

    Als u Windows PowerShell gebruikt, moet u de volgende opdracht uitvoeren:

        $Env:ConnectionString = "connection-string-of-your-app-configuration-store"

    Als u macOS of Linux gebruikt, moet u de volgende opdracht uitvoeren:

        export ConnectionString='connection-string-of-your-app-configuration-store'

2. Voer het volgende commando uit om de console-app te bouwen:

        dotnet build

3. Als het bouwen is voltooid, voert u de volgende opdracht uit om de app lokaal uit te voeren:

        dotnet run

    ![Quickstart app uitvoeren](./media/quickstarts/dotnet-core-app-run.png)

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Volgende stappen

In deze quickstart hebt u een nieuwe app-configuratiearchief gemaakt en deze gebruikt met een .NET Core console-app. Als u meer wilt weten over het gebruik van de app-configuratie, gaat u verder met de volgende zelfstudie, waarin verificatie wordt behandeld.

> [!div class="nextstepaction"]
> [Beheerde identiteiten voor integratie met Azure-resources](./integrate-azure-managed-service-identity.md)
