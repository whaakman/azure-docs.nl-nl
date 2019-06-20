---
title: Snelstartgids voor informatie over het gebruik van Azure SignalR Service met ASP.NET
description: Een snelstartgids voor het gebruik van Azure SignalR Service een chatruimte maken met ASP.NET framework.
author: sffamily
ms.service: signalr
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 04/20/2019
ms.author: zhshang
ms.openlocfilehash: 2020ee02d236ca13431adb736d9f48171d33b4f3
ms.sourcegitcommit: 72f1d1210980d2f75e490f879521bc73d76a17e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/14/2019
ms.locfileid: "67147422"
---
# <a name="quickstart-create-a-chat-room-with-aspnet-and-signalr-service"></a>Quickstart: Een chatruimte maken met ASP.NET en SignalR-Service

Azure SignalR-Service is gebaseerd op [SignalR voor ASP.NET Core 2.0](https://docs.microsoft.com/aspnet/core/signalr/introduction), die is **niet** 100% compatibel is met ASP.NET SignalR. Azure SignalR-Service wordt opnieuw geïmplementeerd ASP.NET SignalR gegevensprotocol op basis van de meest recente ASP.NET Core-technologieën. Wanneer u Azure SignalR Service voor ASP.NET SignalR, sommige functies van ASP.NET SignalR niet meer worden ondersteund, bijvoorbeeld Azure SignalR wordt niet opnieuw afspelen berichten wanneer de client opnieuw verbinding maakt. De altijd Frame transport- en JSONP zijn ook niet ondersteuning. Sommige wijzigingen in de code en de juiste versie van de afhankelijke bibliotheken zijn vereist voor het werken met SignalR Service ASP.NET SignalR-toepassing maken. 

Raadpleeg de [versie verschillen doc](https://docs.microsoft.com/aspnet/core/signalr/version-differences?view=aspnetcore-2.2) voor een volledige lijst van de vergelijking van functies tussen ASP.NET SignalR en ASP.NET Core SignalR.

In deze snelstartgids leert u hoe u aan de slag met ASP.NET en Azure SignalR Service voor een vergelijkbare [chatruimte toepassing](./signalr-quickstart-dotnet-core.md).


[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]
## <a name="prerequisites"></a>Vereisten

* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/)
* [.NET 4.6.1](https://www.microsoft.com/net/download/windows)
* [ASP.NET SignalR 2.4.1](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/)

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u met uw Azure-account aan bij [Azure Portal](https://portal.azure.com/).

[!INCLUDE [Create instance](includes/signalr-quickstart-create-instance.md)]

*Serverloze* modus wordt niet ondersteund voor ASP.NET SignalR-toepassingen. Gebruik altijd *standaard* of *klassieke* voor de Azure SignalR Service-exemplaar.

U kunt ook maken met Azure-resources die worden gebruikt in deze snelstartgids met [maken van een script SignalR Service](scripts/signalr-cli-create-service.md).

## <a name="clone-the-sample-application"></a>De voorbeeldtoepassing klonen

Terwijl de service wordt geïmplementeerd, gaan we met code werken. Kloon de [voorbeeld-app vanuit GitHub](https://github.com/aspnet/AzureSignalR-samples/tree/master/aspnet-samples/ChatRoom), stel de SignalR Service-verbindingsreeks in en voer de toepassing lokaal uit.

1. Open een nieuw git-terminalvenster. Ga naar een map waarin u het voorbeeldproject wilt klonen.

1. Voer de volgende opdracht uit om de voorbeeldopslagplaats te klonen. Deze opdracht maakt een kopie van de voorbeeld-app op uw computer.

    ```bash
    git clone https://github.com/aspnet/AzureSignalR-samples.git
    ```

## <a name="configure-and-run-chat-room-web-app"></a>Configureren en uitvoeren van chatruimte web-app

1. Start Visual Studio en open de oplossing in de *aspnet-samples/ChatRoom/* map van de gekloonde opslagplaats.

1. Zoek en selecteer het exemplaar dat u hebt gemaakt in de browser waarin de Azure-portal wordt geopend.

1. Selecteer **Sleutels** om de verbindingsreeksen voor het service-exemplaar van SignalR weer te geven.

1. Selecteer en kopieer de primaire verbindingsreeks.

1. Nu de verbindingsreeks instellen in het bestand web.config.

    ```xml
    <configuration>
    <connectionStrings>
        <add name="Azure:SignalR:ConnectionString" connectionString="<Replace By Your Connection String>"/>
    </connectionStrings>
    ...
    </configuration>
    ```

1. In *Startup.cs*, in plaats van aanroepen `MapSignalR()`, moet u contact opnemen `MapAzureSignalR({your_applicationName})` en door te geven in de verbindingsreeks voor de toepassing die verbinding maken met de service in plaats van die als host fungeert SignalR zelf maken. Vervang `{YourApplicationName}` op de naam van uw toepassing. Deze naam is een unieke naam op voor deze toepassing uit uw andere toepassingen te onderscheiden. U kunt `this.GetType().FullName` als de waarde.

    ```cs
    public void Configuration(IAppBuilder app)
    {
        // Any connection or hub wire up and configuration should go here
        app.MapAzureSignalR(this.GetType().FullName);
    }
    ```

    U moet ook verwijzen naar de SDK-service voordat u deze API's. Open de **hulpprogramma's | NuGet-Pakketbeheer | Package Manager Console** en opdracht uitvoeren:

    ```powershell
    Install-Package Microsoft.Azure.SignalR.AspNet
    ```

    Alle andere blijft hetzelfde, kunt u nog steeds de hub-interface die u al bekend met bent het schrijven van bedrijfslogica gebruiken dan deze wijzigingen.

    > [!NOTE]
    > In de implementatie een eindpunt `/signalr/negotiate` wordt weergegeven voor onderhandeling met Azure SignalR Service-SDK. Een speciale onderhandeling-antwoord wordt geretourneerd wanneer clients verbinding maken met en dat clients worden omgeleid naar de service-eindpunt is gedefinieerd in de verbindingsreeks.

1. Druk op **F5** om uit te voeren van het project in de foutopsporingsmodus. Hier ziet u de toepassing lokaal wordt uitgevoerd. In plaats van een host een SignalR-runtime door de toepassing zelf, maakt deze nu verbinding met de Azure SignalR-Service.

[!INCLUDE [Cleanup](includes/signalr-quickstart-cleanup.md)]



> [!IMPORTANT]
> Het verwijderen van een resourcegroep kan niet ongedaan worden gemaakt. De resourcegroep en alle bijbehorende resources worden permanent verwijderd. Zorg ervoor dat u niet per ongeluk de verkeerde resourcegroep of resources verwijdert. Als u de resources voor het hosten van dit voorbeeld in een bestaande resourcegroep hebt gemaakt en deze groep ook resources bevat die u wilt behouden, kunt u elke resource afzonderlijk verwijderen via hun respectievelijke blade.
> 
> 

Meld u aan bij [Azure Portal](https://portal.azure.com) en klik op **Resourcegroepen**.

Typ de naam van de resourcegroep in het tekstvak **Filteren op naam...** . In de instructies voor deze snelstart is een resourcegroep met de naam *SignalRTestResources* gebruikt. Klik in de resourcegroep in de lijst met resultaten op **...** en vervolgens op **Resourcegroep verwijderen**.

   
![Verwijderen](./media/signalr-quickstart-dotnet-core/signalr-delete-resource-group.png)

Na enkele ogenblikken worden de resourcegroep en alle resources in de groep verwijderd.

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt gemaakt van een nieuwe Azure SignalR Service-resource en deze gebruikt met een ASP.NET-web-app. Hierna leert u hoe u realtime met Azure SignalR Service met ASP.NET Core-toepassingen te ontwikkelen.

> [!div class="nextstepaction"]
> [Azure SignalR Service met ASP.NET Core](./signalr-quickstart-dotnet-core.md)
