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
ms.openlocfilehash: b5e41b1f9ee982b8ff8c86232f715d5dab705cd6
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56962159"
---
# <a name="quickstart-create-a-net-framework-app-with-azure-app-configuration"></a>Quickstart: Een .NET Framework-app maken met Azure-appconfiguratie

Azure-appconfiguratie is een beheerde configuratieservice in Azure. Hiermee kunt u eenvoudig alle toepassingsinstellingen opslaan en beheren op één plek die gescheiden is van uw code. In deze quickstart ziet u hoe u de service moet opnemen in een Windows-bureaublad console-app op basis van .NET Framework.

![Downloaden van voorbeeld voor snelstart voltooid](./media/quickstarts/dotnet-fx-app-run.png)

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze quickstart, installeert u [Visual Studio 2017](https://visualstudio.microsoft.com/vs) en [.NET Framework 4.7.1](https://dotnet.microsoft.com/download) of hoger, als u dat nog niet gedaan hebt.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-app-configuration-store"></a>Een app-configuratiearchief maken

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

## <a name="create-a-net-console-app"></a>Een .NET Core-consoletoepassing maken

1. Open Visual Studio en selecteer **Bestand** > **Nieuw** > **Project**.

2. In het dialoogvenster **Nieuw Project** selecteert u **Geïnstalleerd**, vouwt **Visual C#**   >  **Windows Desktop** uit, selecteert u  **Console-App (.NET Framework)**, tytp een **Naam** voor uw project, kiest **.NET Framework 4.7.1** of omhoog en klikt u op **OK**.

## <a name="connect-to-app-configuration-store"></a>Verbinding maken met een app-configuratiearchief

1. Klik met de rechtermuisknop op het project en selecteer **NuGet-pakketten beheren**. Zoek in het tabblad **Bladeren** de volgende NuGet-pakketten en voeg deze toe aan uw project (vink het vak **Include prerelease** aan als u ze niet vinden).
    ```
    Microsoft.Configuration.ConfigurationBuilders.AzureAppConfiguration 1.0.0 preview or later
    Microsoft.Configuration.ConfigurationBuilders.Environment 2.0.0 preview or later
    ```

2. Update het *App.config*-bestand van uw project als volgt:

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

   Houd er rekening mee, omdat we de verbindingsreeks van het opslaan van de app-configuratie lezen uit de omgevingsvariabele `ConnectionString`, dat het belangrijk is om de configuratiebuilder `Environment` toe te voegen voor `MyConfigStore` in de eigenschap `configBuilders` van sectie `appSettings`.

3. Open *Program.cs* en werk de methode `Main` bij voor het gebruik van de app-configuratie door methode `ConfigurationManager` aan te roepen.

    ```csharp
    static void Main(string[] args)
    {
        string message = ConfigurationManager.AppSettings["TestApp:Settings:Message"];

        Console.WriteLine(message);
    }
    ```

## <a name="build-and-run-the-app-locally"></a>De app lokaal compileren en uitvoeren

1. Stel een omgevingsvariabele met de naam **ConnectionString** in en stel deze in op de toegangssleutel van uw app-configuratiearchief. Als u Windows Command Prompt gebruikt, moet u de volgende opdracht uitvoeren:

        setx ConnectionString "connection-string-of-your-app-configuration-store"

    Als u Windows PowerShell gebruikt, moet u de volgende opdracht uitvoeren:

        $Env:ConnectionString = "connection-string-of-your-app-configuration-store"

2. Herstart Visual Studio zodat de wijziging wordt doorgevoerd en druk vervolgens opnieuw op **Ctrl + F5** op het toetsenbord om de console-app te compileren en uit te voeren.

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Volgende stappen

In deze quickstart hebt u een nieuw app-configuratiearchief gemaakt en dit gebruikt met een .NET Core console-app. Als u meer wilt weten over het gebruik van de app-configuratie, gaat u verder met de volgende zelfstudie, waarin verificatie wordt behandeld.

> [!div class="nextstepaction"]
> [Beheerde identiteiten voor integratie met Azure-resources](./integrate-azure-managed-service-identity.md)
