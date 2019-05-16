---
title: 'Snelstart: Leren werken met Azure SignalR Service'
description: Een snelstart waarin u leert hoe u met de Azure SignalR-service een chatruimte maakt met ASP.NET Core MVC-apps.
author: sffamily
ms.service: signalr
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 03/01/2019
ms.author: zhshang
ms.openlocfilehash: 3dc893ea10e47e867110f674a458498a6bd24a4f
ms.sourcegitcommit: 179918af242d52664d3274370c6fdaec6c783eb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/13/2019
ms.locfileid: "65560696"
---
# <a name="quickstart-create-a-chat-room-by-using-signalr-service"></a>Quickstart: Een chatruimte maken met behulp van SignalR-Service


De Azure SignalR-service is een Azure-service waarmee ontwikkelaars eenvoudig webtoepassingen met realtime-functies kunnen bouwen. Deze service is gebaseerd op [SignalR voor ASP.NET Core 2.0](https://docs.microsoft.com/aspnet/core/signalr/introduction).

In dit artikel leest u hoe u aan de slag gaat met de Azure SignalR-service. In deze snelstartgids maakt u een toepassing voor chatten met behulp van een ASP.NET Core MVC-web-app. Deze app maakt verbinding met de resource van de Azure SignalR-service om inhoud in realtime bij te werken. U host voor de web-App lokaal en verbinding maken met meerdere browserclients. Elke client kan bijgewerkte inhoud pushen naar alle andere clients. 

U kunt elke code-editor gebruiken om de stappen in deze snelstart uit te voeren. Een optie is [Visual Studio Code](https://code.visualstudio.com/), die beschikbaar is op Windows, macOS en Linux-platforms.

De code voor deze zelfstudie is beschikbaar als download in de [GitHub-opslagplaats met AzureSignalR-voorbeelden](https://github.com/aspnet/AzureSignalR-samples/tree/master/samples/ChatRoom). Bovendien kunt u de Azure-resources die in deze Quick Start wordt gebruikt door [maken van een script SignalR Service](scripts/signalr-cli-create-service.md).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Vereisten

* Installeer de [.NET Core SDK](https://www.microsoft.com/net/download/windows).
* Download of kloon het [Azure SignalR-voorbeeld](https://github.com/aspnet/AzureSignalR-samples) in de GitHub-opslagplaats. 

## <a name="create-an-azure-signalr-resource"></a>Een Azure SignalR-resource maken

[!INCLUDE [azure-signalr-create](../../includes/signalr-create.md)]

## <a name="create-an-aspnet-core-web-app"></a>Een ASP.NET Core-web-app maken

In deze sectie maakt u de [.NET Core-opdrachtregelinterface (CLI)](https://docs.microsoft.com/dotnet/core/tools/) te maken van een ASP.NET Core MVC-web-app-project. Het voordeel van het gebruik van de .NET Core-CLI via Visual Studio is dat het is beschikbaar voor de Windows, macOS en Linux-platforms. 

1. Maak een map voor uw project. Deze snelstartgids maakt gebruik van de *E:\Testing\chattest* map.

2. Voer de volgende opdracht uit om het project te maken in de nieuwe map:

        dotnet new mvc


## <a name="add-secret-manager-to-the-project"></a>Secret Manager toevoegen aan het project

In deze sectie voegt u de [Secret Manager hulpprogramma](https://docs.microsoft.com/aspnet/core/security/app-secrets) aan uw project. Het hulpprogramma Secret Manager slaat de gevoelige gegevens voor projecten buiten de projectstructuur van uw. Deze aanpak helpt te voorkomen dat het onbedoeld delen van appgeheimen in de broncode.

1. Open het bestand *.csproj*. Voeg een element `DotNetCliToolReference` toe voor *Microsoft.Extensions.SecretManager.Tools*. Ook toevoegen een `UserSecretsId` element zoals wordt weergegeven in de volgende code voor *chattest.csproj*, en sla het bestand.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.Web">
    <PropertyGroup>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <UserSecretsId>SignalRChatRoomEx</UserSecretsId>
    </PropertyGroup>
    <ItemGroup>
        <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
    </ItemGroup>
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.0.0" />
        <DotNetCliToolReference Include="Microsoft.Extensions.SecretManager.Tools" Version="2.0.0" />
    </ItemGroup>
    </Project>    
    ```

## <a name="add-azure-signalr-to-the-web-app"></a>Azure SignalR toevoegen aan de web-app

1. Voeg een verwijzing naar de `Microsoft.Azure.SignalR` NuGet-pakket met de volgende opdracht:

        dotnet add package Microsoft.Azure.SignalR

2. Voer de volgende opdracht om te herstellen van pakketten voor uw project:

        dotnet restore

3. Voeg een geheim met de naam *Azure:SignalR:ConnectionString* toe aan Secret Manager. 

    Dit geheim bevat straks de verbindingsreeks voor toegang tot de resource van de SignalR-service. *Azure: SignalR:ConnectionString* is de standaardsleutel voor configuratie dat SignalR zoekt naar een verbinding tot stand brengen. De waarde in de volgende opdracht vervangt door de verbindingsreeks voor uw resource SignalR-Service.

    U moet deze opdracht uitvoert in dezelfde map als de *.csproj* bestand.

    ```
    dotnet user-secrets set Azure:SignalR:ConnectionString "<Your connection string>"    
    ```

    Secret Manager zal alleen worden gebruikt voor de web-app testen terwijl deze lokaal wordt gehost. In een latere zelfstudie implementeert u de chat-web-app naar Azure. Nadat de web-app is geïmplementeerd in Azure, kunt u een toepassingsinstelling gaat gebruiken in plaats van de verbindingsreeks met Secret Manager.

    Dit geheim wordt geopend met de configuratie-API. Een dubbele punt (:) werkt in de naam van de configuratie met de configuratie-API op alle ondersteunde platforms. Zie [configuratie omgeving](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/index?tabs=basicconfiguration&view=aspnetcore-2.0). 


4. Open *Startup.cs* en werk de methode `ConfigureServices` bij voor het gebruik van de Azure SignalR-service door de methode `services.AddSignalR().AddAzureSignalR()` aan te roepen:

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddMvc();
        services.AddSignalR().AddAzureSignalR();
    }
    ```

    Een parameter niet wordt door `AddAzureSignalR()`, deze code gebruikt de standaard-configuratiesleutel voor de verbindingsreeks voor SignalR-Service-resource. De sleutel van de configuratie van standaard *Azure: SignalR:ConnectionString*.

5. Verder moet u in *Startup.cs* de methode `Configure` bijwerken door de aanroep van `app.UseStaticFiles()` te vervangen door de onderstaande code. Sla het bestand vervolgens op.

    ```csharp
    app.UseFileServer();
    app.UseAzureSignalR(routes =>
    {
        routes.MapHub<Chat>("/chat");
    });
    ```            

### <a name="add-a-hub-class"></a>Een hub-klasse toevoegen

In SignalR is een hub een belangrijk onderdeel waarmee een aantal methoden die kunnen worden aangeroepen vanuit de client beschikbaar wordt gemaakt. In dit gedeelte gaat u een hub-klasse met twee methoden definiëren: 

* `Broadcast`: met deze methode verstuurt u een bericht naar alle clients.
* `Echo`: met deze methode stuurt u een bericht terug naar de aanroepende functie.

Beide methoden maken gebruik van de `Clients` interface die de ASP.NET Core SignalR-SDK bevat. Deze interface geeft u toegang tot alle verbonden clients, zodat u de inhoud aan uw clients kunt pushen.

1. Voeg in de projectmap een nieuwe map toe met de naam *Hub*. Voeg een nieuw bestand met hub-code met de naam *Chat.cs* toe aan de nieuwe map.

2. Voeg de volgende code aan *Chat.cs* voor het definiëren van uw hub-klasse en sla het bestand. 

    Werk de naamruimte voor deze klasse bij als u een andere projectnaam dan *chattest* hebt gebruikt.

    ```csharp
    using Microsoft.AspNetCore.SignalR;

    namespace chattest
    {

        public class Chat : Hub
        {
            public void BroadcastMessage(string name, string message)
            {
                Clients.All.SendAsync("broadcastMessage", name, message);
            }

            public void Echo(string name, string message)
            {
                Clients.Client(Context.ConnectionId).SendAsync("echo", name, message + " (echo from server)");
            }
        }
    }
    ```

### <a name="add-the-client-interface-for-the-web-app"></a>De clientinterface voor de web-app toevoegen

De gebruikersinterface van client voor deze app chatruimte bestaan uit HTML en JavaScript in een bestand met de naam *index.html* in de *wwwroot* directory.

Kopiëren de *index.html* -bestand, de *css* map, en de *scripts* map vanaf de *wwwroot* map van de [voorbeelden opslagplaats](https://github.com/aspnet/AzureSignalR-samples/tree/master/samples/ChatRoom/wwwroot). Plak deze in van uw project *wwwroot* map.

Dit is de belangrijkste gedragscode *index.html*: 

```javascript
var connection = new signalR.HubConnectionBuilder()
                            .withUrl('/chat')
                            .build();
bindConnectionMessage(connection);
connection.start()
    .then(function () {
        onConnected(connection);
    })
    .catch(function (error) {
        console.error(error.message);
    });
```    

De code in *index.html* aanroepen `HubConnectionBuilder.build()` om te maken van een HTTP-verbinding met de Azure SignalR-resource.

Als de verbinding tot stand is gebracht, wordt de verbinding doorgegeven aan `bindConnectionMessage`. Deze functie voegt gebeurtenis-handlers toe om binnenkomende inhoud naar de client te pushen. 

`HubConnection.start()` start de communicatie met de hub. Vervolgens `onConnected()` voegt u de knop gebeurtenis-handlers. Deze handlers gebruiken de verbinding om deze client in staat te stellen updates van inhoud naar alle verbonden clients te pushen.

## <a name="add-a-development-runtime-profile"></a>Een profiel voor een runtime-ontwikkelomgeving toevoegen

In deze sectie voegt u een runtime-ontwikkelomgeving voor ASP.NET Core. Zie voor meer informatie, [werken met meerdere omgevingen in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/environments).

1. Maak een map met de naam *eigenschappen* in uw project.

2. Voeg een nieuw bestand met de naam *launchSettings.json* naar de map met de volgende inhoud en sla het bestand.

    ```json
    {
        "profiles" : 
        {
            "ChatRoom": 
            {
                "commandName": "Project",
                "launchBrowser": true,
                "environmentVariables": 
                {
                    "ASPNETCORE_ENVIRONMENT": "Development"
                },
                "applicationUrl": "http://localhost:5000/"
            }
        }
    }
    ```


## <a name="build-and-run-the-app-locally"></a>De app lokaal compileren en uitvoeren

1. Voor het bouwen van de app met behulp van .NET Core CLI, voer de volgende opdracht in de opdrachtshell:

        dotnet build

2. Nadat de build met succes is voltooid, voert u de volgende opdracht om de web-app lokaal uitvoeren:

        dotnet run

    De app wordt lokaal worden gehost op poort 5000, zoals geconfigureerd in de ontwikkeling van runtime-profiel:

        E:\Testing\chattest>dotnet run
        Hosting environment: Development
        Content root path: E:\Testing\chattest
        Now listening on: http://localhost:5000
        Application started. Press Ctrl+C to shut down.    

3. Open twee browservensters. In elke browser, Ga naar `http://localhost:5000`. U wordt gevraagd uw naam in te voeren. Voer de naam van een klant voor clients en test berichtinhoud pushen tussen beide clients met behulp van de **verzenden** knop.

    ![Voorbeeld van een groep Azure SignalR chat](media/signalr-quickstart-dotnet-core/signalr-quickstart-complete-local.png)



## <a name="clean-up-resources"></a>Resources opschonen

Als het gaat u verder met de volgende zelfstudie, kunt u houden van de resources in deze Quick Start hebt gemaakt en opnieuw gebruiken.

Als u klaar met de voorbeeldtoepassing Quick Start bent, kunt u de Azure-resources die zijn gemaakt in deze Quick Start om kosten te voorkomen verwijderen. 

> [!IMPORTANT]
> Als u een resourcegroep niet ongedaan worden gemaakt en is voorzien van alle resources in die groep. Zorg ervoor dat u niet per ongeluk de verkeerde resourcegroep of resources verwijdert. Als u de resources voor het hosten van dit voorbeeld in een bestaande resourcegroep waarin de resources die u wilt behouden hebt gemaakt, kunt u elke resource afzonderlijk verwijderen via de blade in plaats van de resourcegroep verwijderen.
> 
> 

Meld u aan bij [Azure Portal](https://portal.azure.com) en selecteer **Resourcegroepen**.

In de **filteren op naam** tekst typt u de naam van de resourcegroep. In de instructies voor deze snelstart is een resourcegroep met de naam *SignalRTestResources* gebruikt. Op de resourcegroep in de lijst met resultaten, selecteer het weglatingsteken (**...** ) > **Resourcegroep verwijderen**.

   
![Selecties voor het verwijderen van een resourcegroep](./media/signalr-quickstart-dotnet-core/signalr-delete-resource-group.png)


U wordt gevraagd om het verwijderen van de resourcegroep te bevestigen. Voer de naam van de resourcegroep om te bevestigen en selecteer **verwijderen**.
   
Na enkele ogenblikken worden de resourcegroep en alle bijbehorende resources verwijderd.



## <a name="next-steps"></a>Volgende stappen

In deze quickstart maakt u een nieuwe Azure SignalR Service-resource gemaakt. U gebruikt deze vervolgens een ASP.NET Core web-app om updates van inhoud in realtime naar meerdere verbonden clients. Voor meer informatie over het gebruik van Azure SignalR Service gaat u naar de zelfstudie die laat zien van verificatie.

> [!div class="nextstepaction"]
> [Verificatie van Azure SignalR-service](./signalr-concept-authenticate-oauth.md)


