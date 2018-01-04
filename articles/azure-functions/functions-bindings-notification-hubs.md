---
title: Notification Hubs bindingen voor Azure Functions
description: Het gebruik van Azure Notification Hub-binding in Azure Functions begrijpen.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: Azure functions, functies, verwerking van gebeurtenissen, dynamische compute zonder server architectuur
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/21/2017
ms.author: glenga
ms.openlocfilehash: cbc22721bbe06fdc5a8a9026b113071e1616bcb8
ms.sourcegitcommit: 2e540e6acb953b1294d364f70aee73deaf047441
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="notification-hubs-output-binding-for-azure-functions"></a>Notification Hubs uitvoer binding voor Azure Functions

Dit artikel wordt uitgelegd hoe u pushmeldingen verzendt met behulp van [Azure Notification Hubs](../notification-hubs/notification-hubs-push-notification-overview.md) bindingen in de Azure Functions. Azure Functions ondersteunt uitvoer bindingen voor Notification Hubs.

Azure Notification Hubs moet worden geconfigureerd voor het Platform meldingen Service (PNS) u wilt gebruiken. Zie voor meer informatie over hoe u pushmeldingen in uw clientapp van Notification Hubs, [aan de slag met Notification Hubs](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) en selecteert u het doelplatform voor de client in de vervolgkeuzelijst aan de bovenkant van de pagina.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="example---template"></a>Voorbeeld - sjabloon

De meldingen die u verzendt systeemeigen meldingen kunnen worden of [Sjabloonmeldingen](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md). Systeemeigen meldingen zoals geconfigureerd in een specifieke client-platform als doel de `platform` eigenschap van de uitvoer-binding. Een melding van de sjabloon kan worden gebruikt voor meerdere platforms.   

Zie het voorbeeld taalspecifieke:

* [C# script - parameter](#c-script-template-example---out-parameter)
* [C# script - asynchrone](#c-script-template-example---asynchronous)
* [C# script - JSON](#c-script-template-example---json)
* [C# script - bibliotheek typen](#c-script-template-example---library-types)
* [F#](#f-template-example)
* [JavaScript](#javascript-template-example)

### <a name="c-script-template-example---out-parameter"></a>C# script sjabloon voorbeeld - uitvoerparameter

In dit voorbeeld wordt een melding verzonden op een [sjabloon registratie](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) die bevat een `message` tijdelijke aanduiding in de sjabloon.

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

### <a name="c-script-template-example---asynchronous"></a>C# script sjabloon voorbeeld - asynchrone

Als u van asynchrone code gebruikmaakt, zijn out-parameters niet toegestaan. In dit geval gebruiken `IAsyncCollector` te retourneren van de melding van uw sjabloon. De volgende code is een asynchrone voorbeeld van de bovenstaande code. 

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

### <a name="c-script-template-example---json"></a>C# script sjabloon voorbeeld - JSON

In dit voorbeeld wordt een melding verzonden op een [sjabloon registratie](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) die bevat een `message` tijdelijke aanduiding in de sjabloon met gebruik van een geldige JSON-tekenreeks.

```cs
using System;

public static void Run(string myQueueItem,  out string notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    notification = "{\"message\":\"Hello from C#. Processed a queue item!\"}";
}
```

### <a name="c-script-template-example---library-types"></a>C# script sjabloon voorbeeld - bibliotheek typen

Dit voorbeeld ziet u het gebruik van typen die zijn gedefinieerd de [bibliotheek van Microsoft Azure Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/). 

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

### <a name="f-template-example"></a>F # sjabloon voorbeeld

In dit voorbeeld wordt een melding verzonden op een [sjabloon registratie](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) die bevat `location` en `message`.

```fsharp
let Run(myTimer: TimerInfo, notification: byref<IDictionary<string, string>>) =
    notification = dict [("location", "Redmond"); ("message", "Hello from F#!")]
```

### <a name="javascript-template-example"></a>Voorbeeld van JavaScript-sjabloon

In dit voorbeeld wordt een melding verzonden op een [sjabloon registratie](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) die bevat `location` en `message`.

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

Dit C# scriptvoorbeeld ziet u het gebruik van typen die zijn gedefinieerd de [bibliotheek van Microsoft Azure Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) een systeemeigen WNS toast-melding te verzenden. 

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

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruiken de [NotificationHub](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions.NotificationHubs/NotificationHubAttribute.cs) kenmerk, die is gedefinieerd in NuGet-pakket [Microsoft.Azure.WebJobs.Extensions.NotificationHubs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.NotificationHubs).

Constructorparameters en de eigenschappen van het kenmerk worden beschreven in de [configuratie](#configuration) sectie.

## <a name="configuration"></a>Configuratie

De volgende tabel beschrijft de binding-configuratie-eigenschappen die u instelt in de *function.json* bestand en de `NotificationHub` kenmerk:

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**type** |N.v.t.| Moet worden ingesteld op 'notificationHub'. |
|**richting** |N.v.t.| Moet worden ingesteld op 'out'. | 
|**naam** |N.v.t.| Naam van de variabele in functiecode gebruikt voor de notification hub-bericht. |
|**tagExpression** |**TagExpression** | Code-expressies kunnen u opgeven dat meldingen worden geleverd aan een verzameling apparaten die zijn geregistreerd voor het ontvangen van meldingen die overeenkomen met de code-expressie.  Zie voor meer informatie [Routering en code-expressies](../notification-hubs/notification-hubs-tags-segment-push-message.md). |
|**hubName** | **HubName** | Naam van de notification hub-resource in de Azure-portal. |
|**verbinding** | **ConnectionStringSetting** | De naam van een app-instelling die een Notification Hubs-verbindingsreeks bevat.  De verbindingsreeks moet worden ingesteld op de *DefaultFullSharedAccessSignature* waarde voor uw notification hub. Zie [verbinding tekenreeks instellen](#connection-string-setup) verderop in dit artikel.|
|**platform** | **Platform** | De platform-eigenschap geeft het clientplatform uw doelen melding. Standaard, als de eigenschap platform wordt weggelaten uit de uitvoer-binding kunnen Sjabloonmeldingen worden gebruikt als doel in elk platform dat is geconfigureerd op de Azure Notification Hub. Zie voor meer informatie over het gebruik van sjablonen in het algemeen verzenden cross-platform meldingen met een Azure Notification Hub [sjablonen](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md). Als de waarde, **platform** moet een van de volgende waarden: <ul><li><code>apns</code>&mdash;Apple Push Notification Service. Zie voor meer informatie over het configureren van de notification hub voor APNS en ontvangst van de melding in een client-app [pushmeldingen verzenden naar iOS met Azure Notification Hubs](../notification-hubs/notification-hubs-ios-apple-push-notification-apns-get-started.md).</li><li><code>adm</code>&mdash;[Amazon Device Messaging](https://developer.amazon.com/device-messaging). Zie voor meer informatie over het configureren van de notification hub voor ADM en ontvangst van de melding in een Kindle-app [aan de slag met Notification Hubs voor Kindle-apps](../notification-hubs/notification-hubs-kindle-amazon-adm-push-notification.md).</li><li><code>gcm</code>&mdash;[Google Cloud Messaging](https://developers.google.com/cloud-messaging/). Firebase Cloud Messaging, dat de nieuwe versie van GCM is, wordt ook ondersteund. Zie voor meer informatie [pushmeldingen verzenden naar Android met Azure Notification Hubs](../notification-hubs/notification-hubs-android-push-notification-google-fcm-get-started.md).</li><li><code>wns</code>&mdash;[Windows Push Notification Services](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview) die gericht is op Windows-platforms. Windows Phone 8.1 en hoger wordt ook ondersteund door WNS. Zie voor meer informatie [aan de slag met Notification Hubs voor universele Windows-Platform-Apps](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).</li><li><code>mpns</code>&mdash;[Microsoft Push Notification Service](https://msdn.microsoft.com/library/windows/apps/ff402558.aspx). Dit platform biedt ondersteuning voor Windows Phone 8 en eerdere Windows Phone-platforms. Zie voor meer informatie [pushmeldingen verzenden met Azure Notification Hubs op Windows Phone](../notification-hubs/notification-hubs-windows-mobile-push-notifications-mpns.md).</li></ul> |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

### <a name="functionjson-file-example"></a>Voorbeeld van een Function.JSON

Hier volgt een voorbeeld van een binding met Notification Hubs in een *function.json* bestand.

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

### <a name="connection-string-setup"></a>Verbinding tekenreeks instellen

U moet de verbindingsreeks voor de hub configureren voor het gebruik van een notification hub-uitvoer binding. U kunt selecteren van een notification hub of maak een nieuwe één rechts van de *integreren* tabblad in de Azure-portal. U kunt ook handmatig de verbindingsreeks configureren. 

De verbindingsreeks naar een notification hub configureren:

1. Navigeer naar uw notification hub in de [Azure-portal](https://portal.azure.com), kies **toegangsbeleid**, en selecteer de knop kopiëren naast de **DefaultFullSharedAccessSignature** beleid. Hiermee kopieert u de verbindingsreeks voor de *DefaultFullSharedAccessSignature* beleid naar uw notification hub. Deze verbindingsreeks kunt de functie meldingen verzenden naar de hub.
    ![Kopieer de verbindingsreeks van de notification hub](./media/functions-bindings-notification-hubs/get-notification-hub-connection.png)
1. Navigeer naar de functie-app in de Azure portal, kiest u **toepassingsinstellingen**, zoals een sleutel toevoegen **MyHubConnectionString**, plak de gekopieerde *DefaultFullSharedAccessSignature* voor uw notification hub als de waarde en klik vervolgens op **opslaan**.

De naam van de toepassingsinstelling van deze is wat komt in de instelling voor uitvoer de binding in *function.json* of het .NET-kenmerk. Zie de [configuratiesectie](#configuration) eerder in dit artikel.

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over Azure functions triggers en bindingen](functions-triggers-bindings.md)

