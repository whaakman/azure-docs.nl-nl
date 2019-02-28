---
title: Integreren met een continue integratie en levering pijplijn met behulp van Azure App-configuratie | Microsoft Docs
description: Meer informatie over het genereren van een configuratiebestand met gegevens in Azure App-configuratie tijdens de continue integratie en levering
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 02/24/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: 7b2e919bc46810e8478956675ffeb1cb0542da85
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56957365"
---
# <a name="integrate-with-a-cicd-pipeline"></a>Integreren met een CI/CD-pijplijn

Ter verbetering van de flexibiliteit van uw toepassing meer veiligheid tegen de externe kans niet meer kunnen bereiken van Azure App-configuratie, moet u de huidige configuratiegegevens verpakken in een bestand dat is geïmplementeerd met de toepassing en lokaal geladen tijdens het opstarten . Deze aanpak zorgt ervoor dat uw toepassing standaardwaarden voor de instelling ten minste hebben. Deze waarden worden overschreven door nieuwe wijzigingen in een configuratie van apps wanneer deze beschikbaar is.

Met behulp van de [ **exporteren** ](./howto-import-export-data.md#export-data) functie van Azure App-configuratie, kunt u het proces van het ophalen van de huidige van configuratiegegevens als een enkel bestand automatiseren. Vervolgens kunt u dit bestand insluiten in een stap build of implementatie in uw continue integratie en continue implementatiepijplijn.

Het volgende voorbeeld ziet u hoe u met de configuratie van de App gegevens als een build-stap voor de web-app die is geïntroduceerd in de QuickStart. Volledige [maken van een ASP.NET Core-app met App-configuratie](./quickstart-aspnet-core-app.md) eerste voordat u doorgaat.

U kunt elke code-editor gebruiken om de stappen in deze snelstart uit te voeren. [Visual Studio Code](https://code.visualstudio.com/) is een uitstekende optie die beschikbaar is op de Windows-, macOS- en Linux-platforms.

## <a name="prerequisites"></a>Vereisten

Als u lokaal hebt gemaakt, downloadt en installeert [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) als u dat nog niet gedaan hebt.

Als u een cloud gebouwd wilt, met Azure DevOps bijvoorbeeld, controleert u of [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) in uw build-systeem is geïnstalleerd.

## <a name="export-app-configuration-store"></a>Opslaan van de app-configuratie exporteren

1. Open uw *.csproj* -bestand en voeg de volgende:

    ```xml
    <Target Name="Export file" AfterTargets="Build">
        <Message Text="Export the configurations to a temp file. " />
        <Exec WorkingDirectory="$(MSBuildProjectDirectory)" Condition="$(ConnectionString) != ''" Command="az appconfig kv export -f $(OutDir)\azureappconfig.json --format json --separator : --connection-string $(ConnectionString)" />
    </Target>
    ```

    De *ConnectionString* die zijn gekoppeld met de configuratie van de app store moet worden toegevoegd als een omgevingsvariabele.

2. Open *Program.cs* en werk de `CreateWebHostBuilder` methode voor het gebruik van het geëxporteerde json-bestand door het aanroepen van de `config.AddJsonFile()` methode.

    ```csharp
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                var directory = System.IO.Path.GetDirectoryName(Assembly.GetExecutingAssembly().Location);
                var settings = config.Build();

                config.AddJsonFile(Path.Combine(directory, "azureappconfig.json"));
                config.AddAzureAppConfiguration(settings["ConnectionStrings:AppConfig"]);
            })
            .UseStartup<Startup>();
    ```

## <a name="build-and-run-the-app-locally"></a>Bouwen en de app lokaal uitvoeren

1. Stel een omgevingsvariabele met de naam **ConnectionString** en stel deze in op de toegangssleutel voor het opslaan van de app-configuratie. Als u van Windows-opdrachtprompt gebruikmaakt, voer de volgende opdracht uit en start opnieuw op de opdrachtprompt om toe te staan van de wijziging door te voeren:

        setx ConnectionString "connection-string-of-your-app-configuration-store"

    Als u Windows PowerShell gebruikt, moet u de volgende opdracht uitvoeren:

        $Env:ConnectionString = "connection-string-of-your-app-configuration-store"

    Als u Mac OS of Linux, moet u de volgende opdracht uitvoeren:

        export ConnectionString='connection-string-of-your-app-configuration-store'

2. Compileer de app met behulp van de .NET Core CLI door de volgende opdracht uit te voeren in de opdrachtshell:

        dotnet build

3. Als het compileren is voltooid, voert u de volgende opdracht uit om de web-app lokaal uit te voeren:

        dotnet run

4. Open een browservenster en navigeer naar `http://localhost:5000`, dit is de standaard-URL voor de web-app die lokaal wordt gehost.

    ![Snelstartgids voor het starten van de app lokaal](./media/quickstarts/aspnet-core-app-launch-local.png)

## <a name="next-steps"></a>Volgende stappen

* [Beheerde identiteiten voor de integratie van Azure-Resources](./integrate-azure-managed-service-identity.md)
