---
title: Quickstart voor Azure-appconfiguratie met .NET Framework | Microsoft Docs
description: Een quickstart voor het gebruik van Azure-app-configuratie met .NET Framework-apps
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.devlang: csharp
ms.topic: quickstart
ms.tgt_pltfrm: .NET
ms.workload: tbd
ms.date: 02/24/2019
ms.author: yegu
ms.openlocfilehash: 551b884f032eaba3f052fcb7571ba907038152ff
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58226841"
---
# <a name="quickstart-create-a-net-framework-app-with-azure-app-configuration"></a>Quickstart: Een .NET Framework-app maken met Azure-appconfiguratie

Azure-appconfiguratie is een beheerde configuratieservice in Azure. U kunt deze eenvoudig opslaan en beheren van alle instellingen van de toepassing op één plek dat gescheiden van uw code. In deze quickstart ziet u hoe u de service moet opnemen in een Windows-bureaublad console-app op basis van .NET Framework.

![Snelstartgids voltooid lokale](./media/quickstarts/dotnet-fx-app-run.png)

## <a name="prerequisites"></a>Vereisten

Als u wilt doen in deze Quick Start, installeert u [Visual Studio 2017](https://visualstudio.microsoft.com/vs) en [.NET Framework 4.7.1](https://dotnet.microsoft.com/download) of hoger als u dat nog niet gedaan hebt.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-app-configuration-store"></a>Een app-configuratiearchief maken

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

## <a name="create-a-net-console-app"></a>Een .NET Core-consoletoepassing maken

1. Start Visual Studio en selecteer **bestand** > **nieuw** > **Project**.

2. In **nieuw Project**, selecteer **geïnstalleerde** > **Visual C#**   >  **Windows Desktop**. Selecteer **Console-App (.NET Framework)**, en voer een naam voor uw project. Selecteer **.NET Framework 4.7.1** of omhoog en selecteer **OK**.

## <a name="connect-to-an-app-configuration-store"></a>Verbinding maken met een app-configuratiearchief

1. Met de rechtermuisknop op uw project en selecteer **NuGet-pakketten beheren**. Op de **Bladeren** tabblad, zoeken en de volgende NuGet-pakketten toevoegen aan uw project. Als u niet kunt vinden, selecteert u de **Include prerelease** selectievakje.
    ```
    Microsoft.Configuration.ConfigurationBuilders.AzureAppConfiguration 1.0.0 preview or later
    Microsoft.Configuration.ConfigurationBuilders.Environment 2.0.0 preview or later
    ```

2. Update de *App.config* -bestand van uw project als volgt te werk:

    ```xml
    <configSections>
        <section name="configBuilders" type="System.Configuration.ConfigurationBuildersSection, System.Configuration, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" restartOnExternalChanges="false" requirePermission="false" />
    </configSections>

    <configBuilders>
        <builders>
            <add name="MyConfigStore" mode="Greedy" connectionString="${ConnectionString}" type="Microsoft.Configuration.ConfigurationBuilders.AzureAppConfigurationBuilder, Microsoft.Configuration.ConfigurationBuilders.AzureAppConfiguration" />
            <add name="Environment" mode="Greedy" type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder, Microsoft.Configuration.ConfigurationBuilders.Environment" />
        </builders>
    </configBuilders>

    <appSettings configBuilders="Environment,MyConfigStore">
        <add key="AppName" value="Console App Demo" />
        <add key="ConnectionString" value ="Set via an environment variable - for example, dev, test, staging, or production connection string." />
    </appSettings>
    ```

   De verbindingsreeks van het opslaan van de app-configuratie van de omgevingsvariabele wordt gelezen `ConnectionString`. Toevoegen de `Environment` builder configuratie voordat de `MyConfigStore` in de `configBuilders` eigenschap van de `appSettings` sectie.

3. Open *Program.cs*, en werk de `Main` methode voor het gebruik van App-configuratie door het aanroepen van `ConfigurationManager`.

    ```csharp
    static void Main(string[] args)
    {
        string message = ConfigurationManager.AppSettings["TestApp:Settings:Message"];

        Console.WriteLine(message);
    }
    ```

## <a name="build-and-run-the-app-locally"></a>De app lokaal compileren en uitvoeren

1. Stel een omgevingsvariabele met de naam **ConnectionString** in en stel deze in op de toegangssleutel van uw app-configuratiearchief. Als u de opdrachtprompt van Windows gebruikt, voert u de volgende opdracht uit:

        setx ConnectionString "connection-string-of-your-app-configuration-store"

    Als u Windows PowerShell gebruikt, voert u de volgende opdracht uit:

        $Env:ConnectionString = "connection-string-of-your-app-configuration-store"

2. Start opnieuw op Visual Studio, zodat de wijziging door te voeren. Druk op Ctrl + F5 om te bouwen en uitvoeren van de console-app.

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt gemaakt van een nieuwe app-configuratiearchief en deze gebruikt met een .NET Framework-consoletoepassing. Doorgaan naar de volgende zelfstudie waarin wordt gedemonstreerd verificatie voor meer informatie over het gebruik van App-configuratie.

> [!div class="nextstepaction"]
> [Beheerde identiteiten voor de integratie van Azure-resources](./integrate-azure-managed-service-identity.md)
