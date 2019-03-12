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
ms.date: 02/28/2019
ms.author: cshoe
ms.openlocfilehash: bd59a9584f6993d768a9aeb790470a1d978c78ae
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57542429"
---
# <a name="signalr-service-bindings-for-azure-functions"></a>SignalR-servicebindingen voor Azure Functions

In dit artikel wordt uitgelegd hoe u om te verifiëren en verzenden van berichten in realtime naar clients die zijn verbonden met [Azure SignalR Service](https://azure.microsoft.com/services/signalr-service/) via SignalR Service bindingen in Azure Functions. Azure Functions ondersteunt invoer- en uitvoerbindingen voor SignalR-Service.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-2x"></a>Pakketten - functies 2.x

De bindingen SignalR-Service vindt u in de [Microsoft.Azure.WebJobs.Extensions.SignalRService](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SignalRService) NuGet-pakket, versie 1.*. Broncode voor het pakket is in de [azure-functions-signalrservice-extension](https://github.com/Azure/azure-functions-signalrservice-extension) GitHub-opslagplaats.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2-manual-portal.md)]


### <a name="java-annotations"></a>Java-aantekeningen

Als u wilt de aantekeningen SignalR-Service in Java-functies gebruiken, moet u een afhankelijkheid toevoegen de *azure-functions-java-bibliotheek-signalr* artefact (versie 1.0 of hoger) aan uw pom.xml.

```xml
<dependency>
    <groupId>com.microsoft.azure.functions</groupId>
    <artifactId>azure-functions-java-library-signalr</artifactId>
    <version>1.0.0</version>
</dependency>
```

> [!NOTE]
> Gebruik de bindingen SignalR-Service in Java, zorg ervoor dat u versie 2.4.419 of hoger van Azure Functions Core Tools (hostversie 2.0.12332).

## <a name="using-signalr-service-with-azure-functions"></a>Gebruik SignalR-Service met Azure Functions

Raadpleeg voor meer informatie over het configureren en gebruiken met SignalR-Service en Azure Functions samen [Azure Functions-ontwikkeling en configuratie met Azure SignalR Service](../azure-signalr/signalr-concept-serverless-development-config.md).

## <a name="signalr-connection-info-input-binding"></a>SignalR connection-info-Invoerbinding

Voordat een client verbinding met Azure SignalR Service maken kan, moet deze de service-eindpunt-URL en een geldig toegangstoken ophalen. De *SignalRConnectionInfo* Invoerbinding produceert de SignalR-Service-eindpunt-URL en een geldig token die worden gebruikt voor verbinding met de service. Omdat het token gedurende beperkte tijd is en kan worden gebruikt om een specifieke gebruiker om een verbinding te verifiëren, moet u niet het token in de cache of delen tussen de clients. Een HTTP-trigger met behulp van deze binding kan worden gebruikt door clients om op te halen van de verbindingsgegevens.

Zie het voorbeeld taalspecifieke:

* [2.x C#](#2x-c-input-examples)
* [2.x JavaScript](#2x-javascript-input-examples)
* [2.x-Java](#2x-java-input-examples)

Zie voor meer informatie over hoe deze binding wordt gebruikt voor het maken van een 'onderhandelen over'-functie die kan worden gebruikt door een SignalR-client-SDK, de [artikel voor het ontwikkel- en configuratie van Azure Functions](../azure-signalr/signalr-concept-serverless-development-config.md) in de concepten SignalR-Service documentatie.

### <a name="2x-c-input-examples"></a>2.x C# invoer voorbeelden

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die gegevens van het SignalR-verbinding met behulp van de Invoerbinding verkrijgt en geeft dit terug via HTTP.

```cs
[FunctionName("negotiate")]
public static SignalRConnectionInfo Negotiate(
    [HttpTrigger(AuthorizationLevel.Anonymous)]HttpRequest req,
    [SignalRConnectionInfo(HubName = "chat")]SignalRConnectionInfo connectionInfo)
{
    return connectionInfo;
}
```

#### <a name="authenticated-tokens"></a>Geverifieerde tokens

Als de functie wordt geactiveerd door een geverifieerde client, kunt u een gebruiker-ID claim naar het gegenereerde token toevoegen. U kunt eenvoudig verificatie toevoegen aan een functie-app met behulp van [App Service-verificatie] (.. /App-Service/Overview-Authentication-Authorization.MD).

App Service-verificatie wordt ingesteld met de naam van HTTP-headers `x-ms-client-principal-id` en `x-ms-client-principal-name` die respectievelijk de principal-ID van client en de naam, de geverifieerde gebruiker bevatten. U kunt instellen dat de `UserId` eigenschap van de binding met de waarde van de koptekst met behulp van een [binding expressie](./functions-bindings-expressions-patterns.md): `{headers.x-ms-client-principal-id}` of `{headers.x-ms-client-principal-name}`. 

```cs
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

### <a name="2x-javascript-input-examples"></a>invoer 2.x JavaScript-voorbeelden

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
module.exports = async function (context, req, connectionInfo) {
    context.res.body = connectionInfo;
};
```

#### <a name="authenticated-tokens"></a>Geverifieerde tokens

Als de functie wordt geactiveerd door een geverifieerde client, kunt u een gebruiker-ID claim naar het gegenereerde token toevoegen. U kunt eenvoudig verificatie toevoegen aan een functie-app met behulp van [App Service-verificatie] (.. /App-Service/Overview-Authentication-Authorization.MD).

App Service-verificatie wordt ingesteld met de naam van HTTP-headers `x-ms-client-principal-id` en `x-ms-client-principal-name` die respectievelijk de principal-ID van client en de naam, de geverifieerde gebruiker bevatten. U kunt instellen dat de `userId` eigenschap van de binding met de waarde van de koptekst met behulp van een [binding expressie](./functions-bindings-expressions-patterns.md): `{headers.x-ms-client-principal-id}` of `{headers.x-ms-client-principal-name}`. 

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
module.exports = async function (context, req, connectionInfo) {
    // connectionInfo contains an access key token with a name identifier
    // claim set to the authenticated user
    context.res.body = connectionInfo;
};
```

### <a name="2x-java-input-examples"></a>invoer 2.x Java-voorbeelden

Het volgende voorbeeld wordt een [Java functie](functions-reference-java.md) die gegevens van het SignalR-verbinding met behulp van de Invoerbinding verkrijgt en geeft dit terug via HTTP.

```java
@FunctionName("negotiate")
public SignalRConnectionInfo negotiate(
        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.POST },
            authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Optional<String>> req,
        @SignalRConnectionInfoInput(
            name = "connectionInfo",
            hubName = "chat") SignalRConnectionInfo connectionInfo) {
    return connectionInfo;
}
```

#### <a name="authenticated-tokens"></a>Geverifieerde tokens

Als de functie wordt geactiveerd door een geverifieerde client, kunt u een gebruiker-ID claim naar het gegenereerde token toevoegen. U kunt eenvoudig verificatie toevoegen aan een functie-app met behulp van [App Service-verificatie] (.. /App-Service/Overview-Authentication-Authorization.MD).

App Service-verificatie wordt ingesteld met de naam van HTTP-headers `x-ms-client-principal-id` en `x-ms-client-principal-name` die respectievelijk de principal-ID van client en de naam, de geverifieerde gebruiker bevatten. U kunt instellen dat de `UserId` eigenschap van de binding met de waarde van de koptekst met behulp van een [binding expressie](./functions-bindings-expressions-patterns.md): `{headers.x-ms-client-principal-id}` of `{headers.x-ms-client-principal-name}`.

```java
@FunctionName("negotiate")
public SignalRConnectionInfo negotiate(
        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.POST },
            authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Optional<String>> req,
        @SignalRConnectionInfoInput(
            name = "connectionInfo",
            hubName = "chat",
            userId = "{headers.x-ms-client-principal-id}") SignalRConnectionInfo connectionInfo) {
    return connectionInfo;
}
```

## <a name="signalr-output-binding"></a>SignalR-Uitvoerbinding

Gebruik de *SignalR* Uitvoerbinding voor het verzenden van een of meer berichten met behulp van Azure SignalR Service. U kunt een bericht verzenden naar alle verbonden clients, of u kunt zenden alleen naar verbonden clients die zijn geverifieerd op een bepaalde gebruiker.

U kunt deze ook gebruiken voor het beheren van de groepen waartoe een gebruiker behoort.

Zie het voorbeeld taalspecifieke:

* [2.x C#](#2x-c-send-message-output-examples)
* [2.x JavaScript](#2x-javascript-send-message-output-examples)
* [2.x-Java](#2x-java-send-message-output-examples)

### <a name="2x-c-send-message-output-examples"></a>2.x C# bericht uitvoer voorbeelden verzenden

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
            // the message will only be sent to this user ID
            UserId = "userId1",
            Target = "newMessage",
            Arguments = new [] { message }
        });
}
```

#### <a name="send-to-a-group"></a>Verzenden naar een groep

U kunt een bericht verzendt alleen voor verbindingen die zijn toegevoegd aan een groep door in te stellen de `GroupName` eigenschap van het SignalR-bericht.

```cs
[FunctionName("SendMessage")]
public static Task SendMessage(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]object message,
    [SignalR(HubName = "chat")]IAsyncCollector<SignalRMessage> signalRMessages)
{
    return signalRMessages.AddAsync(
        new SignalRMessage
        {
            // the message will only be sent to this user ID
            GroupName = "myGroup",
            Target = "newMessage",
            Arguments = new [] { message }
        });
}
```

### <a name="2x-c-group-management-output-examples"></a>2.x C# groepsbeheer voorbeelden uitvoeren

SignalR-Service kunnen gebruikers worden toegevoegd aan groepen. Berichten kunnen vervolgens worden verzonden naar een groep. U kunt de `SignalRGroupAction` klasse met de `SignalR` Uitvoerbinding voor het beheren van groepslidmaatschap van een gebruiker.

#### <a name="add-user-to-a-group"></a>Gebruiker toevoegen aan een groep

Het volgende voorbeeld wordt een gebruiker toegevoegd aan een groep.

```csharp
[FunctionName("addToGroup")]
public static Task AddToGroup(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]HttpRequest req,
    string userId,
    [SignalR(HubName = "chat")]
        IAsyncCollector<SignalRGroupAction> signalRGroupActions)
{
    return signalRGroupActions.AddAsync(
        new SignalRGroupAction
        {
            UserId = userId,
            GroupName = "myGroup",
            Action = GroupAction.Add
        });
}
```

#### <a name="remove-user-from-a-group"></a>Gebruiker verwijderen uit een groep

Het volgende voorbeeld wordt een gebruiker verwijdert uit een groep.

```csharp
[FunctionName("removeFromGroup")]
public static Task RemoveFromGroup(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]HttpRequest req,
    string userId,
    [SignalR(HubName = "chat")]
        IAsyncCollector<SignalRGroupAction> signalRGroupActions)
{
    return signalRGroupActions.AddAsync(
        new SignalRGroupAction
        {
            UserId = userId,
            GroupName = "myGroup",
            Action = GroupAction.Remove
        });
}
```

### <a name="2x-javascript-send-message-output-examples"></a>2.x JavaScript verzenden bericht uitvoer voorbeelden

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
module.exports = async function (context, req) {
    context.bindings.signalRMessages = [{
        "target": "newMessage",
        "arguments": [ req.body ]
    }];
};
```

#### <a name="send-to-a-user"></a>Verzenden naar een gebruiker

U kunt een bericht verzendt alleen voor verbindingen die aan een gebruiker is geverifieerd door in te stellen de `userId` eigenschap van het SignalR-bericht.

*Function.JSON* blijft hetzelfde. Dit is de JavaScript-code:

```javascript
module.exports = async function (context, req) {
    context.bindings.signalRMessages = [{
        // message will only be sent to this user ID
        "userId": "userId1",
        "target": "newMessage",
        "arguments": [ req.body ]
    }];
};
```

#### <a name="send-to-a-group"></a>Verzenden naar een groep

U kunt een bericht verzendt alleen voor verbindingen die zijn toegevoegd aan een groep door in te stellen de `groupName` eigenschap van het SignalR-bericht.

*Function.JSON* blijft hetzelfde. Dit is de JavaScript-code:

```javascript
module.exports = async function (context, req) {
    context.bindings.signalRMessages = [{
        // message will only be sent to this group
        "groupName": "myGroup",
        "target": "newMessage",
        "arguments": [ req.body ]
    }];
};
```

### <a name="2x-javascript-group-management-output-examples"></a>2.x JavaScript-groepsbeheer voorbeelden uitvoeren

SignalR-Service kunnen gebruikers worden toegevoegd aan groepen. Berichten kunnen vervolgens worden verzonden naar een groep. U kunt de `SignalR` Uitvoerbinding voor het beheren van groepslidmaatschap van een gebruiker.

#### <a name="add-user-to-a-group"></a>Gebruiker toevoegen aan een groep

Het volgende voorbeeld wordt een gebruiker toegevoegd aan een groep.

*function.json*

```json
{
  "disabled": false,
  "bindings": [
    {
      "authLevel": "anonymous",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "res"
    },
    {
      "type": "signalR",
      "name": "signalRGroupActions",
      "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
      "hubName": "chat",
      "direction": "out"
    }
  ]
}
```

*index.js*

```javascript
module.exports = async function (context, req) {
  context.bindings.signalRGroupActions = [{
    "userId": req.query.userId,
    "groupName": "myGroup",
    "action": "add"
  }];
};
```

#### <a name="remove-user-from-a-group"></a>Gebruiker verwijderen uit een groep

Het volgende voorbeeld wordt een gebruiker verwijdert uit een groep.

*function.json*

```json
{
  "disabled": false,
  "bindings": [
    {
      "authLevel": "anonymous",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "res"
    },
    {
      "type": "signalR",
      "name": "signalRGroupActions",
      "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
      "hubName": "chat",
      "direction": "out"
    }
  ]
}
```

*index.js*

```javascript
module.exports = async function (context, req) {
  context.bindings.signalRGroupActions = [{
    "userId": req.query.userId,
    "groupName": "myGroup",
    "action": "remove"
  }];
};
```

### <a name="2x-java-send-message-output-examples"></a>2.x Java verzenden bericht uitvoer voorbeelden

#### <a name="broadcast-to-all-clients"></a>Zenden naar alle clients

Het volgende voorbeeld wordt een [Java functie](functions-reference-java.md) die verzendt een bericht weergegeven met behulp van de Uitvoerbinding voor alle verbonden clients. De `target` is de naam van de methode om te worden aangeroepen op elke client. De `arguments` eigenschap is een matrix met nul of meer objecten moeten worden doorgegeven aan de methode voor clients.

```java
@FunctionName("sendMessage")
@SignalROutput(name = "$return", hubName = "chat")
public SignalRMessage sendMessage(
        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.POST },
            authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Object> req) {

    SignalRMessage message = new SignalRMessage();
    message.target = "newMessage";
    message.arguments.add(req.getBody());
    return message;
}
```

#### <a name="send-to-a-user"></a>Verzenden naar een gebruiker

U kunt een bericht verzendt alleen voor verbindingen die aan een gebruiker is geverifieerd door in te stellen de `userId` eigenschap van het SignalR-bericht.

```java
@FunctionName("sendMessage")
@SignalROutput(name = "$return", hubName = "chat")
public SignalRMessage sendMessage(
        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.POST },
            authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Object> req) {

    SignalRMessage message = new SignalRMessage();
    message.userId = "userId1";
    message.target = "newMessage";
    message.arguments.add(req.getBody());
    return message;
}
```

#### <a name="send-to-a-group"></a>Verzenden naar een groep

U kunt een bericht verzendt alleen voor verbindingen die zijn toegevoegd aan een groep door in te stellen de `groupName` eigenschap van het SignalR-bericht.

```java
@FunctionName("sendMessage")
@SignalROutput(name = "$return", hubName = "chat")
public SignalRMessage sendMessage(
        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.POST },
            authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Object> req) {

    SignalRMessage message = new SignalRMessage();
    message.groupName = "myGroup";
    message.target = "newMessage";
    message.arguments.add(req.getBody());
    return message;
}
```

### <a name="2x-java-group-management-output-examples"></a>Java-groepsbeheer 2.x voorbeelden uitvoeren

SignalR-Service kunnen gebruikers worden toegevoegd aan groepen. Berichten kunnen vervolgens worden verzonden naar een groep. U kunt de `SignalRGroupAction` klasse met de `SignalROutput` Uitvoerbinding voor het beheren van groepslidmaatschap van een gebruiker.

#### <a name="add-user-to-a-group"></a>Gebruiker toevoegen aan een groep

Het volgende voorbeeld wordt een gebruiker toegevoegd aan een groep.

```java
@FunctionName("addToGroup")
@SignalROutput(name = "$return", hubName = "chat")
public SignalRGroupAction addToGroup(
        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.POST },
            authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Object> req,
        @BindingName("userId") String userId) {

    SignalRGroupAction groupAction = new SignalRGroupAction();
    groupAction.action = "add";
    groupAction.userId = userId;
    groupAction.groupName = "myGroup";
    return action;
}
```

#### <a name="remove-user-from-a-group"></a>Gebruiker verwijderen uit een groep

Het volgende voorbeeld wordt een gebruiker verwijdert uit een groep.

```java
@FunctionName("removeFromGroup")
@SignalROutput(name = "$return", hubName = "chat")
public SignalRGroupAction removeFromGroup(
        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.POST },
            authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Object> req,
        @BindingName("userId") String userId) {

    SignalRGroupAction groupAction = new SignalRGroupAction();
    groupAction.action = "remove";
    groupAction.userId = userId;
    groupAction.groupName = "myGroup";
    return action;
}
```

## <a name="configuration"></a>Configuratie

### <a name="signalrconnectioninfo"></a>SignalRConnectionInfo

De volgende tabel beschrijft de binding configuratie-eigenschappen die u instelt in de *function.json* bestand en de `SignalRConnectionInfo` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Description|
|---------|---------|----------------------|
|**type**|| Moet worden ingesteld op `signalRConnectionInfo`.|
|**direction**|| Moet worden ingesteld op `in`.|
|**De naam**|| Naam van de variabele in functiecode gebruikt voor de connection-info-object. |
|**hubName**|**HubName**| Deze waarde moet worden ingesteld op de naam van het SignalR-hub waarvoor de verbindingsgegevens is gegenereerd.|
|**userId**|**UserId**| Optioneel: De waarde van de gebruikers-id claim moet worden ingesteld in het toegangstoken van de sleutel. |
|**connectionStringSetting**|**connectionStringSetting**| De naam van de app-instelling met de verbindingsreeks SignalR-Service (standaard ingesteld op "AzureSignalRConnectionString") |

### <a name="signalr"></a>SignalR

De volgende tabel beschrijft de binding configuratie-eigenschappen die u instelt in de *function.json* bestand en de `SignalR` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Description|
|---------|---------|----------------------|
|**type**|| Moet worden ingesteld op `signalR`.|
|**direction**|| Moet worden ingesteld op `out`.|
|**De naam**|| Naam van de variabele in functiecode gebruikt voor de connection-info-object. |
|**hubName**|**HubName**| Deze waarde moet worden ingesteld op de naam van het SignalR-hub waarvoor de verbindingsgegevens is gegenereerd.|
|**connectionStringSetting**|**connectionStringSetting**| De naam van de app-instelling met de verbindingsreeks SignalR-Service (standaard ingesteld op "AzureSignalRConnectionString") |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over Azure functions-triggers en bindingen](functions-triggers-bindings.md)

> [!div class="nextstepaction"]
> [Azure Functions-ontwikkeling en configuratie met Azure SignalR Service](../azure-signalr/signalr-concept-serverless-development-config.md)
