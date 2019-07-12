---
title: Zelfstudie voor het gebruik van dynamische configuratie van Azure App-configuratie in een .NET Core-app | Microsoft Docs
description: In deze zelfstudie leert u hoe u dynamische update van de configuratiegegevens voor .NET Core-apps
services: azure-app-configuration
documentationcenter: ''
author: abarora
manager: zhenlan
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.devlang: csharp
ms.topic: tutorial
ms.date: 07/01/2019
ms.author: abarora
ms.openlocfilehash: 1649fefda5073761d616fc48c602cab84d293ed0
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67799971"
---
# <a name="tutorial-use-dynamic-configuration-in-a-net-core-app"></a>Zelfstudie: Dynamische configuratie gebruiken in een .NET Core-app

De App-configuratie .NET Core-clientbibliotheek biedt ondersteuning voor het bijwerken van een set van configuratie-instellingen op aanvraag zonder dat een toepassing te starten. Dit kan worden geïmplementeerd met het eerste ophalen van een exemplaar van `IConfigurationRefresher` van de opties voor de configuratieprovider en vervolgens calling `Refresh` op dat exemplaar overal in uw code.

Een cache wordt gebruikt om de instellingen bijgewerkt te houden en te voorkomen dat te veel aanroepen voor het opslaan van de configuratie, voor elke instelling. Totdat de waarde in de cache van een instelling is verlopen, wordt de vernieuwingsbewerking niet de waarde bijgewerkt, zelfs wanneer de waarde in het opslaan van de configuratie is gewijzigd. De verlooptijd van de standaard voor elke aanvraag is 30 seconden, maar deze kan worden genegeerd indien nodig.

In deze zelfstudie leert hoe u dynamische configuratie-updates kunt implementeren in uw code. Dit is gebaseerd op de app die is geïntroduceerd in de QuickStart. Voordat u doorgaat, voltooien [een .NET Core-app maken met App-configuratie](./quickstart-dotnet-core-app.md) eerste.

Een code-editor kunt u de stappen in deze zelfstudie doet. [Visual Studio Code](https://code.visualstudio.com/) is een uitstekende optie die beschikbaar is op Windows, macOS en Linux-platforms.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Instellen van uw toepassing bijwerken van de configuratie met een app-configuratiearchief op aanvraag.
> * De meest recente configuratie in de controllers van uw toepassing invoeren.

## <a name="prerequisites"></a>Vereisten

Als u deze zelfstudie wilt, installeert de [.NET Core SDK](https://dotnet.microsoft.com/download).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="reload-data-from-app-configuration"></a>Gegevens opnieuw laden vanuit app-configuratie

Open *Program.cs* en bijwerken van het bestand om op te geven van de configuratie van vernieuwen in de `AddAzureAppConfiguration` methode en trigger handmatig vernieuwen met behulp van de `Refresh` methode.

```csharp
class Program
{
    private static IConfiguration _configuration = null;
    private static IConfigurationRefresher _refresher = null;

    static void Main(string[] args)
    {
        IConfigurationRefresher refresher = null;

        var builder = new ConfigurationBuilder();
        builder.AddAzureAppConfiguration(options =>
        {
            options.Connect(Environment.GetEnvironmentVariable("ConnectionString"))
                    .ConfigureRefresh(refresh =>
                    {
                        refresh.Register("TestApp:Settings:Message")
                               .SetCacheExpiration(TimeSpan.FromSeconds(10));
                    });
                    
                    _refresher = options.GetRefresher();
        });

        _configuration = builder.Build();
        PrintMessage().Wait();
    }

    private static async Task PrintMessage()
    {
        Console.WriteLine(_configuration["TestApp:Settings:Message"] ?? "Hello world!");

        // Wait for the user to press Enter
        Console.ReadLine();

        await _refresher.Refresh();
        Console.WriteLine(_configuration["TestApp:Settings:Message"] ?? "Hello world!");
    }
}
```

De `ConfigureRefresh` methode wordt gebruikt om op te geven van de instellingen die wordt gebruikt voor het bijwerken van de configuratiegegevens met het app-configuratiearchief, wanneer een vernieuwingsbewerking wordt geactiveerd. Een exemplaar van `IConfigurationRefresher` kunnen worden opgehaald door het aanroepen van `GetRefresher` methode van de opties die `AddAzureAppConfiguration` methode, en de `Refresh` methode voor dit exemplaar kan worden gebruikt voor het activeren van een vernieuwingsbewerking overal in uw code.
    
> [!NOTE]
> De verlooptijd van de standaard-cache voor een configuratie-instelling is 30 seconden, maar kan worden overschreven door het aanroepen van de `SetCacheExpiration` methode voor de initialisatiefunctie opties doorgegeven als een argument voor de `ConfigureRefresh` methode.

## <a name="build-and-run-the-app-locally"></a>De app lokaal compileren en uitvoeren

1. Stel een omgevingsvariabele met de naam **ConnectionString**, en stel deze in op de toegangssleutel voor het opslaan van de app-configuratie. Als u de Windows-opdrachtprompt, voer de volgende opdracht uit en start opnieuw op de opdrachtprompt om toe te staan van de wijziging door te voeren:

        setx ConnectionString "connection-string-of-your-app-configuration-store"

    Als u Windows PowerShell gebruikt, voert u de volgende opdracht uit:

        $Env:ConnectionString = "connection-string-of-your-app-configuration-store"

    Als u Mac OS of Linux gebruikt, voert u de volgende opdracht uit:

        export ConnectionString='connection-string-of-your-app-configuration-store'

1. Voer de volgende opdracht om de console-app te bouwen:

        dotnet build

1. Nadat de build is voltooid, voert u de volgende opdracht om de app lokaal uitvoeren:

        dotnet run

    ![Quickstart voor het lokaal starten van een app](./media/quickstarts/dotnet-core-app-run.png)

1. Meld u aan bij [Azure Portal](https://portal.azure.com). Selecteer **alle resources**, en selecteer het app-configuratie store-exemplaar dat u in de Quick Start hebt gemaakt.

1. Selecteer **configuratie Explorer**, en de waarden voor de volgende sleutels:

    | Sleutel | Waarde |
    |---|---|
    | TestApp:Settings:Message | Gegevens uit Azure-App-configuratie - bijgewerkt |

1. Druk op ENTER om te vernieuwen en de bijgewerkte waarde in het venster opdrachtprompt of PowerShell afdrukken.

    ![Quickstart voor het lokaal vernieuwen van een app](./media/quickstarts/dotnet-core-app-run-refresh.png)
    
    > [!NOTE]
    > Omdat de verlooptijd van de cache is ingesteld op 10 seconden met behulp van de `SetCacheExpiration` methode tijdens het instellen van de configuratie voor de bewerking van het vernieuwen, de waarde voor de configuratie-instelling worden alleen bijgewerkt als er ten minste 10 seconden zijn verstreken sinds de laatste vernieuwing voor de betreffende instelling.

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een beheerde Azure-service-identiteit toegevoegd om toegang tot app-configuratie te stroomlijnen en het beheer van referenties voor uw app te verbeteren. Voor meer informatie over het gebruik van App-configuratie, gaat u naar de Azure CLI-voorbeelden.

> [!div class="nextstepaction"]
> [CLI-voorbeelden](./cli-samples.md)
