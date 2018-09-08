---
title: Notification Hubs-bindingen voor Azure Functions
description: Over het gebruik van Azure Notification Hub-binding in Azure Functions.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: Azure functions, functies, gebeurtenisverwerking, dynamische Computing, serverloze architectuur
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 11/21/2017
ms.author: glenga
ms.openlocfilehash: 85992c0d89634c866012ba7fdaa982e46747ba0e
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44092447"
---
# <a name="notification-hubs-output-binding-for-azure-functions"></a>Notification Hubs-Uitvoerbinding voor Azure Functions

In dit artikel wordt uitgelegd hoe u pushmeldingen verzenden met behulp van [Azure Notification Hubs](../notification-hubs/notification-hubs-push-notification-overview.md) bindingen in Azure Functions. Azure Functions ondersteunt uitvoerbindingen voor Notification Hubs.

Azure Notification Hubs moeten worden geconfigureerd voor het Platform meldingen Service (PNS) u wilt gebruiken. Zie voor meer informatie over het verkrijgen van pushmeldingen in uw client-app van Notification Hubs, [aan de slag met Notification Hubs](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) en selecteer uw doelplatform voor de client in de vervolgkeuzelijst aan de bovenkant van de pagina.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Pakketten - functies 1.x

De Notification Hubs-bindingen zijn opgegeven in de [Microsoft.Azure.WebJobs.Extensions.NotificationHubs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.NotificationHubs) NuGet-pakket versie 1.x. Broncode voor het pakket is in de [azure webjobs-sdk extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/tree/v2.x/src/WebJobs.Extensions.NotificationHubs) GitHub-opslagplaats.

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="packages---functions-2x"></a>Pakketten - functies 2.x

Deze binding is niet beschikbaar in functies 2.x.

## <a name="example---template"></a>Voorbeeld - sjabloon

De meldingen die u verzendt systeemeigen meldingen kunnen worden of [Sjabloonmeldingen](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md). Systeemeigen meldingen zoals geconfigureerd in een specifieke client-platform als doel de `platform` eigenschap van de Uitvoerbinding. Een melding van de sjabloon kan worden gebruikt om u te richten op meerdere platforms.   

Zie het voorbeeld taalspecifieke:

* [C#-script - uit-parameter](#c-script-template-example---out-parameter)
* [C#-script - asynchrone](#c-script-template-example---asynchronous)
* [C#-script - JSON](#c-script-template-example---json)
* [C#-script - bibliotheek typen](#c-script-template-example---library-types)
* [F#](#f-template-example)
* [JavaScript](#javascript-template-example)

### <a name="c-script-template-example---out-parameter"></a>C#-script-voorbeeldsjabloon - uit-parameter

In dit voorbeeld wordt een melding verzonden voor een [sjabloon registratie](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) die bevat een `message` tijdelijke aanduiding in de sjabloon.

```cs
using System;
using System.Threading.Tasks;
using System.Collections.Generic;

public static void Run(string myQueueItem,  out IDictionary<string, string> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    notification = GetTemplateProperties(myQueueItem);
}

private static IDictionary<string, string> GetTemplateProperties(string message)
{
    Dictionary<string, string> templateProperties = new Dictionary<string, string>();
    templateProperties["message"] = message;
    return templateProperties;
}
```

### <a name="c-script-template-example---asynchronous"></a>C#-sjabloon-voorbeeldscript - asynchrone

Als u van asynchrone code gebruikmaakt, zijn om de parameters niet toegestaan. In dit geval gebruiken `IAsyncCollector` om terug te keren van de kennisgeving van uw sjabloon. De volgende code is een asynchrone voorbeeld van de bovenstaande code. 

```cs
using System;
using System.Threading.Tasks;
using System.Collections.Generic;

public static async Task Run(string myQueueItem, IAsyncCollector<IDictionary<string,string>> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    log.Info($"Sending Template Notification to Notification Hub");
    await notification.AddAsync(GetTemplateProperties(myQueueItem));    
}

private static IDictionary<string, string> GetTemplateProperties(string message)
{
    Dictionary<string, string> templateProperties = new Dictionary<string, string>();
    templateProperties["user"] = "A new user wants to be added : " + message;
    return templateProperties;
}
```

### <a name="c-script-template-example---json"></a>Voorbeeldsjabloon C# - script: JSON

In dit voorbeeld wordt een melding verzonden voor een [sjabloon registratie](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) die bevat een `message` tijdelijke aanduiding in de sjabloon met behulp van een geldige JSON-tekenreeks.

```cs
using System;

public static void Run(string myQueueItem,  out string notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    notification = "{\"message\":\"Hello from C#. Processed a queue item!\"}";
}
```

### <a name="c-script-template-example---library-types"></a>Voorbeeldsjabloon C# - script: bibliotheek typen

In dit voorbeeld ziet u hoe u typen die zijn gedefinieerd de [bibliotheek van Microsoft Azure Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/). 

```cs
#r "Microsoft.Azure.NotificationHubs"

using System;
using System.Threading.Tasks;
using Microsoft.Azure.NotificationHubs;

public static void Run(string myQueueItem,  out Notification notification, TraceWriter log)
{
   log.Info($"C# Queue trigger function processed: {myQueueItem}");
   notification = GetTemplateNotification(myQueueItem);
}

private static TemplateNotification GetTemplateNotification(string message)
{
    Dictionary<string, string> templateProperties = new Dictionary<string, string>();
    templateProperties["message"] = message;
    return new TemplateNotification(templateProperties);
}
```

### <a name="f-template-example"></a>F #-voorbeeldsjabloon

In dit voorbeeld wordt een melding verzonden voor een [sjabloon registratie](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) die bevat `location` en `message`.

```fsharp
let Run(myTimer: TimerInfo, notification: byref<IDictionary<string, string>>) =
    notification = dict [("location", "Redmond"); ("message", "Hello from F#!")]
```

### <a name="javascript-template-example"></a>Voorbeeld van de JavaScript-sjabloon

In dit voorbeeld wordt een melding verzonden voor een [sjabloon registratie](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) die bevat `location` en `message`.

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();

    if(myTimer.isPastDue)
    {
        context.log('Node.js is running late!');
    }
    context.log('Node.js timer trigger function ran!', timeStamp);  
    context.bindings.notification = {
        location: "Redmond",
        message: "Hello from Node!"
    };
    context.done();
};
```

## <a name="example---apns-native"></a>Voorbeeld: systeemeigen APNS

In dit voorbeeld C# script laat zien hoe een systeemeigen APNS-melding te verzenden. 

```cs
#r "Microsoft.Azure.NotificationHubs"
#r "Newtonsoft.Json"

using System;
using Microsoft.Azure.NotificationHubs;
using Newtonsoft.Json;

public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a new user to be processed in the form of a JSON string with 
    // a "name" value.
    //
    // The JSON format for a native APNS notification is ...
    // { "aps": { "alert": "notification message" }}  

    log.Info($"Sending APNS notification of a new user");    
    dynamic user = JsonConvert.DeserializeObject(myQueueItem);    
    string apnsNotificationPayload = "{\"aps\": {\"alert\": \"A new user wants to be added (" + 
                                        user.name + ")\" }}";
    log.Info($"{apnsNotificationPayload}");
    await notification.AddAsync(new AppleNotification(apnsNotificationPayload));        
}
```

## <a name="example---gcm-native"></a>Voorbeeld: systeemeigen GCM

In dit voorbeeld C# script laat zien hoe een systeemeigen GCM-melding te verzenden. 

```cs
#r "Microsoft.Azure.NotificationHubs"
#r "Newtonsoft.Json"

using System;
using Microsoft.Azure.NotificationHubs;
using Newtonsoft.Json;

public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a new user to be processed in the form of a JSON string with 
    // a "name" value.
    //
    // The JSON format for a native GCM notification is ...
    // { "data": { "message": "notification message" }}  

    log.Info($"Sending GCM notification of a new user");    
    dynamic user = JsonConvert.DeserializeObject(myQueueItem);    
    string gcmNotificationPayload = "{\"data\": {\"message\": \"A new user wants to be added (" + 
                                        user.name + ")\" }}";
    log.Info($"{gcmNotificationPayload}");
    await notification.AddAsync(new GcmNotification(gcmNotificationPayload));        
}
```

## <a name="example---wns-native"></a>Voorbeeld: systeemeigen WNS

In dit voorbeeld C# script ziet u hoe u typen die zijn gedefinieerd de [bibliotheek van Microsoft Azure Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) voor het verzenden van een systeemeigen pop-upmelding met WNS. 

```cs
#r "Microsoft.Azure.NotificationHubs"
#r "Newtonsoft.Json"

using System;
using Microsoft.Azure.NotificationHubs;
using Newtonsoft.Json;

public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a new user to be processed in the form of a JSON string with 
    // a "name" value.
    //
    // The XML format for a native WNS toast notification is ...
    // <?xml version="1.0" encoding="utf-8"?>
    // <toast>
    //      <visual>
    //     <binding template="ToastText01">
    //       <text id="1">notification message</text>
    //     </binding>
    //   </visual>
    // </toast>

    log.Info($"Sending WNS toast notification of a new user");    
    dynamic user = JsonConvert.DeserializeObject(myQueueItem);    
    string wnsNotificationPayload = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                                    "<toast><visual><binding template=\"ToastText01\">" +
                                        "<text id=\"1\">" + 
                                            "A new user wants to be added (" + user.name + ")" + 
                                        "</text>" +
                                    "</binding></visual></toast>";

    log.Info($"{wnsNotificationPayload}");
    await notification.AddAsync(new WindowsNotification(wnsNotificationPayload));        
}
```

## <a name="attributes"></a>Kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruikt u de [NotificationHub](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions.NotificationHubs/NotificationHubAttribute.cs) kenmerk.

De parameters van constructor en eigenschappen van het kenmerk worden beschreven in de [configuratie](#configuration) sectie.

## <a name="configuration"></a>Configuratie

De volgende tabel beschrijft de binding configuratie-eigenschappen die u instelt in de *function.json* bestand en de `NotificationHub` kenmerk:

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**type** |N.v.t.| Moet worden ingesteld op 'notificationHub'. |
|**direction** |N.v.t.| Moet worden ingesteld op 'out'. | 
|**De naam** |N.v.t.| Naam van de variabele in functiecode gebruikt voor de notification hub-bericht. |
|**tagExpression** |**TagExpression** | Code-expressies kunnen u opgeven dat meldingen worden geleverd aan een set met apparaten die zijn geregistreerd voor het ontvangen van meldingen die overeenkomen met de tagexpressie.  Zie voor meer informatie, [Routering en tag expressies](../notification-hubs/notification-hubs-tags-segment-push-message.md). |
|**HubName** | **HubName** | De naam van de resource voor notification hub in Azure portal. |
|**verbinding** | **connectionStringSetting** | De naam van een app-instelling met een verbindingsreeks Notification Hubs.  De verbindingsreeks moet worden ingesteld op de *DefaultFullSharedAccessSignature* waarde voor de notification hub. Zie [verbindingsinstellingen voor tekenreeks](#connection-string-setup) verderop in dit artikel.|
|**platform** | **Platform** | De eigenschap platform geeft het clientplatform uw doelen melding. Standaard, als de eigenschap platform wordt weggelaten uit de Uitvoerbinding kunnen Sjabloonmeldingen worden gebruikt om u te richten op elk platform dat is geconfigureerd op de Azure Notification Hub. Zie voor meer informatie over het gebruik van sjablonen in het algemeen voor het verzenden van cross-platform kunt verzenden met een Azure Notification Hub [sjablonen](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md). Als de waarde, **platform** moet een van de volgende waarden: <ul><li><code>apns</code>&mdash;Apple Push Notification Service. Zie voor meer informatie over het configureren van de notification hub voor APN's en de melding ontvangen in een client-app [pushmeldingen verzenden naar iOS met Azure Notification Hubs](../notification-hubs/notification-hubs-ios-apple-push-notification-apns-get-started.md).</li><li><code>adm</code>&mdash;[Amazon Device Messaging](https://developer.amazon.com/device-messaging). Zie voor meer informatie over het configureren van de notification hub voor ADM en ontvangst van de melding in een app Kindle [aan de slag met Notification Hubs voor Kindle-apps](../notification-hubs/notification-hubs-kindle-amazon-adm-push-notification.md).</li><li><code>gcm</code>&mdash;[Google Cloud Messaging](https://developers.google.com/cloud-messaging/). Firebase Cloud Messaging, dat de nieuwe versie van GCM is, wordt ook ondersteund. Zie voor meer informatie, [pushmeldingen verzenden naar Android met Azure Notification Hubs](../notification-hubs/notification-hubs-android-push-notification-google-fcm-get-started.md).</li><li><code>wns</code>&mdash;[Windows Push Notification Services](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview) die gericht is op Windows-platforms. Windows Phone 8.1 en hoger wordt ook ondersteund door WNS. Zie voor meer informatie, [aan de slag met Notification Hubs voor Windows Universal Platform-Apps](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).</li><li><code>mpns</code>&mdash;[Microsoft Push Notification Service](https://msdn.microsoft.com/library/windows/apps/ff402558.aspx). Dit platform biedt ondersteuning voor Windows Phone 8 en eerdere Windows Phone-platforms. Zie voor meer informatie, [pushmeldingen verzenden met Azure Notification Hubs op Windows Phone](../notification-hubs/notification-hubs-windows-mobile-push-notifications-mpns.md).</li></ul> |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

### <a name="functionjson-file-example"></a>Voorbeeld van een Function.JSON

Hier volgt een voorbeeld van een binding Notification Hubs in een *function.json* bestand.

```json
{
  "bindings": [
    {
      "type": "notificationHub",
      "direction": "out",
      "name": "notification",
      "tagExpression": "",
      "hubName": "my-notification-hub",
      "connection": "MyHubConnectionString",
      "platform": "gcm"
    }
  ],
  "disabled": false
}
```

### <a name="connection-string-setup"></a>Verbindingsinstellingen voor tekenreeks

Voor het gebruik van een notification hub-Uitvoerbinding, moet u de verbindingsreeks voor de hub configureren. U kunt een notification hub selecteren of maken van een nieuwe één rechtstreeks vanuit de *integreren* tabblad in de Azure-portal. U kunt ook handmatig de verbindingsreeks configureren. 

De verbindingsreeks voor een notification hub configureren:

1. Navigeer naar de notification hub in de [Azure-portal](https://portal.azure.com), kiest u **toegangsbeleid**, en selecteer de knop kopiëren naast de **DefaultFullSharedAccessSignature** beleid. Hiermee kopieert u de verbindingsreeks voor de *DefaultFullSharedAccessSignature* beleid naar uw notification hub. Deze verbindingsreeks kunt uw functie meldingen verzenden naar de hub.
    ![Kopieer de notification hub-verbindingsreeks](./media/functions-bindings-notification-hubs/get-notification-hub-connection.png)
1. Navigeer naar uw functie-app in Azure portal, kiest u **toepassingsinstellingen**, zoals een sleutel toevoegen **MyHubConnectionString**, plak de gekopieerde *DefaultFullSharedAccessSignature* voor uw notification hub als de waarde en klik vervolgens op **opslaan**.

De naam van deze toepassingsinstelling is wat komt in de instelling voor uitvoer de binding in *function.json* of het .NET-kenmerk. Zie de [configuratiesectie](#configuration) eerder in dit artikel.

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="exceptions-and-return-codes"></a>Uitzonderingen en retourcodes

| Binding | Referentie |
|---|---|
| Meldingshub | [Operations Guide](https://docs.microsoft.com/rest/api/notificationhubs/) |

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over Azure functions-triggers en bindingen](functions-triggers-bindings.md)

