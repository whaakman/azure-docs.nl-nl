---
title: Azure Functions SignalR-Service-bindingen
description: Over het gebruik van SignalR Service bindingen met Azure Functions.
services: functions
documentationcenter: na
author: craigshoemaker
manager: jeconnoc
editor: ''
tags: ''
keywords: Azure functions, functies, gebeurtenisverwerking, dynamische Computing, serverloze architectuur
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/23/2018
ms.author: cshoe
ms.openlocfilehash: 74092f57b3531a037aee71d433c33dddf8c2b694
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "53001799"
---
# <a name="signalr-service-bindings-for-azure-functions"></a>SignalR-servicebindingen voor Azure Functions

In dit artikel wordt uitgelegd hoe u om te verifiëren en verzenden van berichten in realtime naar clients die zijn verbonden met [Azure SignalR Service](https://azure.microsoft.com/services/signalr-service/) via SignalR Service bindingen in Azure Functions. Azure Functions ondersteunt invoer- en uitvoerbindingen voor SignalR-Service.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-2x"></a>Pakketten - functies 2.x

De bindingen SignalR-Service vindt u in de [Microsoft.Azure.WebJobs.Extensions.SignalRService](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SignalRService) NuGet-pakket, versie 1.0.0-preview1-*. Broncode voor het pakket is in de [azure-functions-signalrservice-extension](https://github.com/Azure/azure-functions-signalrservice-extension) GitHub-opslagplaats.

> [!NOTE]
> Azure SignalR-Service is algemeen beschikbaar. SignalR-Service-bindingen voor Azure Functions zijn echter momenteel in preview.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2-manual-portal.md)]

## <a name="signalr-connection-info-input-binding"></a>SignalR connection-info-Invoerbinding

Voordat een client verbinding met Azure SignalR Service maken kan, moet deze de service-eindpunt-URL en een geldig toegangstoken ophalen. De *SignalRConnectionInfo* Invoerbinding produceert de SignalR-Service-eindpunt-URL en een geldig token die worden gebruikt voor verbinding met de service. Omdat het token gedurende beperkte tijd is en kan worden gebruikt om een specifieke gebruiker om een verbinding te verifiëren, moet u niet het token in de cache of delen tussen de clients. Een HTTP-trigger met behulp van deze binding kan worden gebruikt door clients om op te halen van de verbindingsgegevens.

Zie het voorbeeld taalspecifieke:

* [2.x C#](#2x-c-input-example)
* [2.x JavaScript](#2x-javascript-input-example)

### <a name="2x-c-input-example"></a>invoer 2.x C#-voorbeeld

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die gegevens van het SignalR-verbinding met behulp van de Invoerbinding verkrijgt en geeft dit terug via HTTP.

```cs
[FunctionName("GetSignalRInfo")]
public static SignalRConnectionInfo GetSignalRInfo(
    [HttpTrigger(AuthorizationLevel.Anonymous)]HttpRequest req, 
    [SignalRConnectionInfo(HubName = "chat")]SignalRConnectionInfo connectionInfo)
{
    return connectionInfo;
}
```

#### <a name="authenticated-tokens"></a>Geverifieerde tokens

Als de functie wordt geactiveerd door een geverifieerde client, kunt u een gebruiker-ID claim naar het gegenereerde token toevoegen. U kunt eenvoudig verificatie toevoegen aan een functie-app met behulp van [App Service-verificatie] (.. /App-service/App-service-Authentication-Overview.MD).

App Service-verificatie wordt ingesteld met de naam van HTTP-headers `x-ms-client-principal-id` en `x-ms-client-principal-name` die respectievelijk de principal-ID van client en de naam, de geverifieerde gebruiker bevatten. U kunt instellen dat de `UserId` eigenschap van de binding met de waarde van de koptekst met behulp van een [binding expressie](functions-triggers-bindings.md#binding-expressions-and-patterns): `{headers.x-ms-client-principal-id}` of `{headers.x-ms-client-principal-name}`. 

```cs
[FunctionName("GetSignalRInfo")]
public static SignalRConnectionInfo GetSignalRInfo(
    [HttpTrigger(AuthorizationLevel.Anonymous)]HttpRequest req, 
    [SignalRConnectionInfo
        (HubName = "chat", UserId = "{headers.x-ms-client-principal-id}")]
        SignalRConnectionInfo connectionInfo)
{
    // connectionInfo contains an access key token with a name identifier claim set to the authenticated user
    return connectionInfo;
}
```

### <a name="2x-javascript-input-example"></a>Voorbeeld van invoer de 2.x JavaScript

Het volgende voorbeeld ziet u een SignalR connection-info Invoerbinding in een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding om terug te keren de verbindingsinformatie bevatten.

Hier volgt binding-gegevens de *function.json* bestand:

Voorbeeld van de function.json:

```json
{
    "type": "signalRConnectionInfo",
    "name": "connectionInfo",
    "hubName": "chat",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "direction": "in"
}
```

Dit is de JavaScript-code:

```javascript
module.exports = function (context, req, connectionInfo) {
    context.res = { body: connectionInfo };
    context.done();
};
```

#### <a name="authenticated-tokens"></a>Geverifieerde tokens

Als de functie wordt geactiveerd door een geverifieerde client, kunt u een gebruiker-ID claim naar het gegenereerde token toevoegen. U kunt eenvoudig verificatie toevoegen aan een functie-app met behulp van [App Service-verificatie] (.. /App-service/App-service-Authentication-Overview.MD).

App Service-verificatie wordt ingesteld met de naam van HTTP-headers `x-ms-client-principal-id` en `x-ms-client-principal-name` die respectievelijk de principal-ID van client en de naam, de geverifieerde gebruiker bevatten. U kunt instellen dat de `userId` eigenschap van de binding met de waarde van de koptekst met behulp van een [binding expressie](functions-triggers-bindings.md#binding-expressions-and-patterns): `{headers.x-ms-client-principal-id}` of `{headers.x-ms-client-principal-name}`. 

Voorbeeld van de function.json:

```json
{
    "type": "signalRConnectionInfo",
    "name": "connectionInfo",
    "hubName": "chat",
    "userId": "{headers.x-ms-client-principal-id}",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "direction": "in"
}
```

Dit is de JavaScript-code:

```javascript
module.exports = function (context, req, connectionInfo) {
    // connectionInfo contains an access key token with a name identifier 
    // claim set to the authenticated user
    context.res = { body: connectionInfo };
    context.done();
};
```

## <a name="signalr-output-binding"></a>SignalR-Uitvoerbinding

Gebruik de *SignalR* Uitvoerbinding voor het verzenden van een of meer berichten met behulp van Azure SignalR Service. U kunt een bericht verzenden naar alle verbonden clients, of u kunt zenden alleen naar verbonden clients die zijn geverifieerd op een bepaalde gebruiker.

Zie het voorbeeld taalspecifieke:

* [2.x C#](#2x-c-output-example)
* [2.x JavaScript](#2x-javascript-output-example)

### <a name="2x-c-output-example"></a>Voorbeeld van de 2.x C#-uitvoer

#### <a name="broadcast-to-all-clients"></a>Zenden naar alle clients

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die verzendt een bericht weergegeven met behulp van de Uitvoerbinding voor alle verbonden clients. De `Target` is de naam van de methode om te worden aangeroepen op elke client. De `Arguments` eigenschap is een matrix met nul of meer objecten moeten worden doorgegeven aan de methode voor clients.

```cs
[FunctionName("SendMessage")]
public static Task SendMessage(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]object message, 
    [SignalR(HubName = "chat")]IAsyncCollector<SignalRMessage> signalRMessages)
{
    return signalRMessages.AddAsync(
        new SignalRMessage 
        {
            Target = "newMessage", 
            Arguments = new [] { message } 
        });
}
```

#### <a name="send-to-a-user"></a>Verzenden naar een gebruiker

U kunt een bericht verzendt alleen voor verbindingen die aan een gebruiker is geverifieerd door in te stellen de `UserId` eigenschap van het SignalR-bericht.

```cs
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

### <a name="2x-javascript-output-example"></a>Voorbeeld van de JavaScript-uitvoer 2.x

#### <a name="broadcast-to-all-clients"></a>Zenden naar alle clients

Het volgende voorbeeld ziet u een SignalR-Uitvoerbinding een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding voor het verzenden van een bericht met Azure SignalR Service. Stel de Uitvoerbinding naar een matrix van een of meer SignalR-berichten. Een SignalR-bericht bestaat uit een `target` eigenschap met de naam van de methode om aan te roepen op elke client, en een `arguments` eigenschap die is een matrix met objecten worden doorgegeven aan de clientmethode als argumenten.

Hier volgt binding-gegevens de *function.json* bestand:

Voorbeeld van de function.json:

```json
{
  "type": "signalR",
  "name": "signalRMessages",
  "hubName": "<hub_name>",
  "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
  "direction": "out"
}
```

Dit is de JavaScript-code:

```javascript
module.exports = function (context, req) {
    context.bindings.signalRMessages = [{
        "target": "newMessage",
        "arguments": [ req.body ]
    }];
    context.done();
};
```

#### <a name="send-to-a-user"></a>Verzenden naar een gebruiker

U kunt een bericht verzendt alleen voor verbindingen die aan een gebruiker is geverifieerd door in te stellen de `userId` eigenschap van het SignalR-bericht.

*Function.JSON* blijft hetzelfde. Dit is de JavaScript-code:

```javascript
module.exports = function (context, req) {
    context.bindings.signalRMessages = [{
        // message will only be sent to these user IDs
        "userId": "userId1",
        "target": "newMessage",
        "arguments": [ req.body ]
    }];
    context.done();
};
```

## <a name="configuration"></a>Configuratie

### <a name="signalrconnectioninfo"></a>SignalRConnectionInfo

De volgende tabel beschrijft de binding configuratie-eigenschappen die u instelt in de *function.json* bestand en de `SignalRConnectionInfo` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Description|
|---------|---------|----------------------|
|**type**|| Moet worden ingesteld op `signalRConnectionInfo`.|
|**direction**|| Moet worden ingesteld op `in`.|
|**De naam**|| Naam van de variabele in functiecode gebruikt voor de connection-info-object. |
|**HubName**|**HubName**| Deze waarde moet worden ingesteld op de naam van het SignalR-hub waarvoor de verbindingsgegevens is gegenereerd.|
|**Gebruikers-id**|**Gebruikers-id**| Optioneel: De waarde van de gebruikers-id claim moet worden ingesteld in het toegangstoken van de sleutel. |
|**connectionStringSetting**|**connectionStringSetting**| De naam van de app-instelling met de verbindingsreeks SignalR-Service (standaard ingesteld op "AzureSignalRConnectionString") |

### <a name="signalr"></a>SignalR

De volgende tabel beschrijft de binding configuratie-eigenschappen die u instelt in de *function.json* bestand en de `SignalR` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Description|
|---------|---------|----------------------|
|**type**|| Moet worden ingesteld op `signalR`.|
|**direction**|| Moet worden ingesteld op `out`.|
|**De naam**|| Naam van de variabele in functiecode gebruikt voor de connection-info-object. |
|**HubName**|**HubName**| Deze waarde moet worden ingesteld op de naam van het SignalR-hub waarvoor de verbindingsgegevens is gegenereerd.|
|**connectionStringSetting**|**connectionStringSetting**| De naam van de app-instelling met de verbindingsreeks SignalR-Service (standaard ingesteld op "AzureSignalRConnectionString") |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over Azure functions-triggers en bindingen](functions-triggers-bindings.md)

