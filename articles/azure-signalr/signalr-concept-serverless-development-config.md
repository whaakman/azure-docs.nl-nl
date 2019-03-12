---
title: Ontwikkelen en toepassingen voor Azure Functions SignalR-Service configureren
description: Meer informatie over het ontwikkelen en serverloze realtime toepassingen met behulp van Azure Functions en Azure SignalR Service configureren
author: anthonychu
ms.service: signalr
ms.topic: conceptual
ms.date: 03/01/2019
ms.author: antchu
ms.openlocfilehash: 9b68b9d0bbac984c29759cf4b7b026a559a9d819
ms.sourcegitcommit: dd1a9f38c69954f15ff5c166e456fda37ae1cdf2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57569145"
---
# <a name="azure-functions-development-and-configuration-with-azure-signalr-service"></a>Azure Functions-ontwikkeling en configuratie met Azure SignalR Service

Azure Functions-toepassingen kunnen gebruikmaken van de [bindingen voor Azure SignalR Service](../azure-functions/functions-bindings-signalr-service.md) waarmee realtime functies kunt toevoegen. Client-SDK's beschikbaar in verschillende talen clienttoepassingen gebruiken voor het verbinding maken met Azure SignalR Service en ontvangen van berichten in realtime.

In dit artikel beschrijft de concepten voor het ontwikkelen en configureren van een Azure-functie-app die is geïntegreerd met SignalR-Service.

## <a name="signalr-service-configuration"></a>Configuratie van de SignalR-Service

Azure SignalR-Service kunnen in verschillende modi worden geconfigureerd. Gebruikt in combinatie met Azure Functions, de service moet worden geconfigureerd *zonder* modus.

Zoek in de Azure-portal, de *instellingen* pagina van uw resource SignalR-Service. Stelt de *-servicemodus* naar *zonder server*.

![SignalR-servicemodus](media/signalr-concept-azure-functions/signalr-service-mode.png)

## <a name="azure-functions-development"></a>Azure Functions-ontwikkeling

Een realtime toepassing zonder server die zijn gebouwd met Azure Functions en Azure SignalR Service doorgaans vereist twee Azure Functions:

* Een 'onderhandelen over'-functie die de client worden aangeroepen om op te halen van een geldige SignalR Service-toegangstoken en service-eindpunt-URL
* Een of meer functies die berichten verzenden of groepslidmaatschap beheren

### <a name="negotiate-function"></a>functie onderhandelen

Een clienttoepassing moet een geldige toegangstoken verbinding maken met Azure SignalR Service. Een toegangstoken mag anonieme of geverifieerde in een bepaalde gebruiker-ID. Serverloze toepassingen met SignalR-Service vereisen dat een HTTP-eindpunt met de naam 'onderhandelen' om een token en andere informatie, zoals de SignalR-Service-eindpunt-URL te verkrijgen.

Gebruik een HTTP geactiveerde functie van Azure en de *SignalRConnectionInfo* Invoerbinding voor het genereren van het verbindingsobject informatie. De functie moet een HTTP-route die eindigen op `/negotiate`.

Zie voor meer informatie over het maken van de functie onderhandelen over de [ *SignalRConnectionInfo* invoer binding verwijzing](../azure-functions/functions-bindings-signalr-service.md#signalr-connection-info-input-binding).

Raadpleeg voor meer informatie over het maken van een geverifieerde token, [App Service-verificatie met behulp van](#using-app-service-authentication).

### <a name="sending-messages-and-managing-group-membership"></a>Verzenden van berichten en groepslidmaatschap beheren

Gebruik de *SignalR* uitvoer binding berichten te verzenden naar clients die zijn verbonden met Azure SignalR Service. U kunt berichten uitzenden naar alle clients of u kunt ze verzenden naar een subset van clients die worden geverifieerd met een specifieke gebruikers-ID of die zijn toegevoegd aan een specifieke groep.

Gebruikers kunnen worden toegevoegd aan een of meer groepen. U kunt ook de *SignalR* -Uitvoerbinding toevoegen of verwijderen van gebruikers naar/van groepen.

Zie voor meer informatie de [ *SignalR* uitvoer binding verwijzing](../azure-functions/functions-bindings-signalr-service.md#signalr-output-binding).

### <a name="signalr-hubs"></a>SignalR-Hubs

SignalR heeft een concept van 'hubs'. Elke clientverbinding en elk bericht dat wordt verzonden vanuit Azure Functions is afgestemd op een specifieke hub. U kunt hubs gebruiken als een manier voor het scheiden van uw verbindingen en berichten in logische naamruimten.

## <a name="client-development"></a>Clientontwikkeling

SignalR-clienttoepassingen kunnen gebruikmaken van de client-SignalR SDK in een van verschillende talen eenvoudig verbinding maken met en berichten ontvangen van Azure SignalR-Service.

### <a name="configuring-a-client-connection"></a>Een clientverbinding configureren

Voor verbinding met SignalR-Service, moet een client een geslaagde verbinding-onderhandeling die uit deze stappen bestaat uitvoeren:

1. Een aanvraag naar de *onderhandelen over* HTTP-eindpunt hierboven wordt beschreven om geldige verbindingsinformatie te verkrijgen
1. Verbinding maken met SignalR-Service met behulp van de service-eindpunt-URL en het toegangstoken verkregen via de *onderhandelen over* eindpunt

SignalR client-SDK's bevatten de logica die nodig zijn om uit te voeren van de handshake onderhandeling. Doorgeven van het onderhandelen over eindpunt-URL, min de `negotiate` segment van de SDK `HubConnectionBuilder`. Hier volgt een voorbeeld in JavaScript:

```javascript
const connection = new signalR.HubConnectionBuilder()
  .withUrl('https://my-signalr-function-app.azurewebsites.net/api')
  .build()
```

Volgens de conventies wordt de SDK wordt automatisch toegevoegd `/negotiate` naar de URL en gebruikt deze om te beginnen met het onderhandelen over.

> [!NOTE]
> Als u de JavaScript/TypeScript-SDK in een browser gebruikt, moet u [cross-origin resource sharing (CORS) inschakelen](#enabling-cors) op uw functie-App.

Raadpleeg de documentatie voor de taal voor meer informatie over het gebruik van de SignalR-client-SDK:

* [.NET Standard](https://docs.microsoft.com/aspnet/core/signalr/dotnet-client)
* [JavaScript](https://docs.microsoft.com/aspnet/core/signalr/javascript-client)
* [Java](https://docs.microsoft.com/aspnet/core/signalr/java-client)

### <a name="sending-messages-from-a-client-to-the-service"></a>Verzenden van berichten van een client naar de service

Hoewel de SDK SignalR clienttoepassingen in staat stelt om aan te roepen back-endlogica in een SignalR-hub, is deze functionaliteit nog niet ondersteund wanneer u SignalR-Service met Azure Functions gebruiken. Gebruik HTTP-aanvragen voor het aanroepen van Azure Functions.

## <a name="azure-functions-configuration"></a>Azure Functions-configuratie

Azure functie-apps die kunnen worden geïntegreerd met Azure SignalR Service kunnen worden geïmplementeerd, zoals een typische Azure-functie-app, met behulp van technieken zoals [continu implementatie](../azure-functions/functions-continuous-deployment.md), [zip-implementatie](../azure-functions/deployment-zip-push.md), en [uitvoeren vanaf pakket](../azure-functions/run-functions-from-deployment-package.md).

Er zijn echter een aantal speciale overwegingen voor apps die gebruikmaken van de bindingen SignalR-Service. Als de client wordt uitgevoerd in een browser, moet CORS zijn ingeschakeld. En als de app is verificatie vereist, kunt u het eindpunt onderhandelen over integreren met App Service-verificatie.

### <a name="enabling-cors"></a>CORS inschakelen

De JavaScript/TypeScript-client maakt HTTP-aanvragen voor de functie onderhandelen over om te onderhandelen over de verbinding initiëren. Wanneer de clienttoepassing wordt gehost op een ander domein dan de Azure-functie-app, cross-origin resource sharing (CORS) moet zijn ingeschakeld op de functie-app of de browser de aanvragen worden geblokkeerd.

#### <a name="localhost"></a>Localhost

Bij het uitvoeren van de functie-app op uw lokale computer, kunt u toevoegen een `Host` gedeelte *local.settings.json* CORS inschakelen. In de `Host` sectie, voegt u twee eigenschappen:

* `CORS` -Geef de basis-URL die de oorsprong van de clienttoepassing
* `CORSCredentials` -instellen op `true` naar 'withCredentials'-aanvragen toestaan

Voorbeeld:

```json
{
  "IsEncrypted": false,
  "Values": {
    // values
  },
  "Host": {
    "CORS": "http://localhost:8080",
    "CORSCredentials": true
  }
}
```

#### <a name="azure"></a>Azure

Om in te schakelen CORS voor een Azure-functie-app, gaat u naar het scherm van de configuratie van CORS onder de *platformfuncties* tabblad van de functie-app in Azure portal.

CORS met Access-Control-toestaan-referenties moet zijn ingeschakeld voor de SignalR-client voor het aanroepen van de functie onderhandelen over. Schakel het selectievakje in te schakelen.

In de *oorsprongen toegestaan* sectie, voegt u een item met de oorsprong basis-URL van uw webtoepassing.

![CORS configureren](media/signalr-concept-serverless-development-config/cors-settings.png)

### <a name="using-app-service-authentication"></a>Met behulp van App Service-verificatie

Azure Functions heeft ingebouwde verificatie, ondersteuning van populaire providers, zoals Facebook, Twitter, Google, Microsoft-Account en Azure Active Directory. Deze functie kan worden geïntegreerd met de *SignalRConnectionInfo* binding te maken van verbindingen met Azure SignalR Service die zijn geverifieerd op een gebruikers-ID. Uw toepassing kunt verzenden van berichten met de *SignalR* Uitvoerbinding die bedoeld zijn voor die gebruiker-ID.

In de Azure-portal in uw functie-app *platformfuncties* tabblad, open de *verificatie/autorisatie* instellingenvenster. Volg de documentatie voor [App Service-verificatie](../app-service/overview-authentication-authorization.md) het configureren van verificatie met behulp van een id-provider van uw keuze.

Wanneer geconfigureerd, geverifieerde HTTP-aanvragen worden opgenomen `x-ms-client-principal-name` en `x-ms-client-principal-id` headers die respectievelijk gebruikersnaam van de identiteit van de geverifieerde en gebruikers-ID bevat.

U kunt deze headers in uw *SignalRConnectionInfo* bindingsconfiguratie om geverifieerde verbindingen te maken. Hier volgt een voorbeeld C# onderhandelen over de functie die gebruikmaakt van de `x-ms-client-principal-id` header.

```csharp
[FunctionName("negotiate")]
public static SignalRConnectionInfo Negotiate(
    [HttpTrigger(AuthorizationLevel.Anonymous)]HttpRequest req,
    [SignalRConnectionInfo
        (HubName = "chat", UserId = "{headers.x-ms-client-principal-id}")]
        SignalRConnectionInfo connectionInfo)
{
    // connectionInfo contains an access key token with a name identifier claim set to the authenticated user
    return connectionInfo;
}
```

U kunt voor die gebruiker vervolgens berichten verzenden door in te stellen de `UserId` eigenschap van een SignalR-bericht.

```csharp
[FunctionName("SendMessage")]
public static Task SendMessage(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]object message,
    [SignalR(HubName = "chat")]IAsyncCollector<SignalRMessage> signalRMessages)
{
    return signalRMessages.AddAsync(
        new SignalRMessage
        {
            // the message will only be sent to these user IDs
            UserId = "userId1",
            Target = "newMessage",
            Arguments = new [] { message }
        });
}
```

Zie voor informatie over andere talen, de [bindingen voor Azure SignalR Service](../azure-functions/functions-bindings-signalr-service.md) voor Azure Functions naar verwijzen.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd over het ontwikkelen en configureren van serverloze SignalR Service-toepassingen met behulp van Azure Functions. Probeer te maken van een toepassing zelf met behulp van een van deze quick starts voor een of zelfstudies op de [SignalR Service-overzichtspagina](index.yml).