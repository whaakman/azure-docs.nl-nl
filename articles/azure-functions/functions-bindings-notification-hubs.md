---
title: Azure Functions Notification Hub-binding | Microsoft Docs
description: Het gebruik van Azure Notification Hub-binding in Azure Functions begrijpen.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: Azure functions, functies, verwerking van gebeurtenissen, dynamische compute zonder server architectuur
ms.assetid: 0ff0c949-20bf-430b-8dd5-d72b7b6ee6f7
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/26/2017
ms.author: glenga
ms.openlocfilehash: 02d01d0f6e945ed54dbe766aec2a0fd7c17c510f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-functions-notification-hub-output-binding"></a>Azure Functions Notification Hub uitvoer binding
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Dit artikel wordt uitgelegd hoe u configureert en bindingen van Azure Notification Hub code in Azure Functions. 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

Uw functies kunnen verzenden van pushmeldingen in via een geconfigureerde Azure Notification Hub met een paar regels code. De Azure Notification Hub moet worden geconfigureerd voor het Platform meldingen Services (PNS) u wilt gebruiken. Zie voor meer informatie over het configureren van een Azure Notification Hub en ontwikkelen van een clienttoepassingen registreren om meldingen te ontvangen [aan de slag met Notification Hubs](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) en klik op het doelplatform voor de client aan de bovenkant.

De meldingen die u verzendt zijn systeemeigen meldingen of Sjabloonmeldingen. Systeemeigen meldingen zoals geconfigureerd in een specifieke bericht-platform als doel de `platform` eigenschap van de uitvoer-binding. Een melding van de sjabloon kan worden gebruikt voor meerdere platforms.   

## <a name="notification-hub-output-binding-properties"></a>Notification hub uitvoer bindingeigenschappen
Het bestand function.json biedt de volgende eigenschappen:


|Eigenschap  |Beschrijving  |
|---------|---------|
|**naam** | Naam van de variabele in functiecode gebruikt voor de notification hub-bericht. |
|**type** | moet worden ingesteld op `notificationHub`. |
|**tagExpression** | Code-expressies kunnen u opgeven dat meldingen worden geleverd aan een verzameling apparaten die zich hebben geregistreerd voor het ontvangen van meldingen die overeenkomen met de code-expressie.  Zie voor meer informatie [Routering en code-expressies](../notification-hubs/notification-hubs-tags-segment-push-message.md). |
|**hubName** | Naam van de notification hub-resource in de Azure-portal. |
|**verbinding** | Deze verbindingsreeks moet een **toepassingsinstelling** verbindingsreeks ingesteld op de *DefaultFullSharedAccessSignature* waarde voor uw notification hub. |
|**richting** | moet worden ingesteld op `out`. | 
|**platform** | De platform-eigenschap geeft het platform notification uw doelen melding. Standaard, als de eigenschap platform wordt weggelaten uit de uitvoer-binding kunnen Sjabloonmeldingen worden gebruikt als doel in elk platform dat is geconfigureerd op de Azure Notification Hub. Zie voor meer informatie over het gebruik van sjablonen in het algemeen verzenden cross-platform meldingen met een Azure Notification Hub [sjablonen](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md). Als de waarde, _platform_ moet een van de volgende waarden: <ul><li><code>apns</code>&mdash;Apple Push Notification Service. Zie voor meer informatie over het configureren van de notification hub voor APNS en ontvangst van de melding in een client-app [pushmeldingen verzenden naar iOS met Azure Notification Hubs](../notification-hubs/notification-hubs-ios-apple-push-notification-apns-get-started.md).</li><li><code>adm</code>&mdash;[Amazon Device Messaging](https://developer.amazon.com/device-messaging). Zie voor meer informatie over het configureren van de notification hub voor ADM en ontvangst van de melding in een Kindle-app [aan de slag met Notification Hubs voor Kindle-apps](../notification-hubs/notification-hubs-kindle-amazon-adm-push-notification.md).</li><li><code>gcm</code>&mdash;[Google Cloud Messaging](https://developers.google.com/cloud-messaging/). Firebase Cloud Messaging, dat de nieuwe versie van GCM is, wordt ook ondersteund. Zie voor meer informatie [pushmeldingen verzenden naar Android met Azure Notification Hubs](../notification-hubs/notification-hubs-android-push-notification-google-fcm-get-started.md).</li><li><code>wns</code>&mdash;[Windows Push Notification Services](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview) die gericht is op Windows-platforms. Windows Phone 8.1 en hoger wordt ook ondersteund door WNS. Zie voor meer informatie [aan de slag met Notification Hubs voor universele Windows-Platform-Apps](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).</li><li><code>mpns</code>&mdash;[Microsoft Push Notification Service](https://msdn.microsoft.com/en-us/library/windows/apps/ff402558.aspx). Dit platform biedt ondersteuning voor Windows Phone 8 en eerdere Windows Phone-platforms. Zie voor meer informatie [pushmeldingen verzenden met Azure Notification Hubs op Windows Phone](../notification-hubs/notification-hubs-windows-mobile-push-notifications-mpns.md).</li></ul> |

Voorbeeld function.json:

```json
{
  "bindings": [
    {
      "name": "notification",
      "type": "notificationHub",
      "tagExpression": "",
      "hubName": "my-notification-hub",
      "connection": "MyHubConnectionString",
      "platform": "gcm",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

## <a name="notification-hub-connection-string-setup"></a>Notification hub verbinding tekenreeks instellen
U moet de verbindingsreeks voor de hub configureren voor het gebruik van een Notification hub-uitvoer binding. U kunt selecteren van een notification hub of maak een nieuwe één rechts van de *integreren* tabblad in de functie. U kunt ook handmatig de verbindingsreeks configureren. 

De verbindingsreeks naar een notification hub configureren:

1. Navigeer naar uw notification hub in de [Azure-portal](https://portal.azure.com), kies **toegangsbeleid**, en selecteer de knop kopiëren naast de **DefaultFullSharedAccessSignature** beleid. Hiermee kopieert u de verbindingsreeks voor de *DefaultFullSharedAccessSignature* beleid naar uw notification hub. Deze verbindingsreeks bevat de functie toegang toestemming om meldingsberichten te verzenden. 
    ![Kopieer de verbindingsreeks van de notification hub](./media/functions-bindings-notification-hubs/get-notification-hub-connection.png)
1. Navigeer naar de functie-app in de Azure portal, kiest u **toepassingsinstellingen**, zoals een sleutel toevoegen `MyHubConnectionString`, plak de gekopieerde *DefaultFullSharedAccessSignature* voor uw notification hub als de waarde en klik vervolgens op **opslaan**.

Nu kunt u deze benoemde toepassingsinstelling die de notification hub-verbinding in de uitvoer-binding definieert.

## <a name="apns-native-notifications-with-c-queue-triggers"></a>Systeemeigen meldingen APNS met C# queue-triggers
Dit voorbeeld ziet u het gebruik van typen die zijn gedefinieerd de [bibliotheek van Microsoft Azure Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) een systeemeigen APNS-melding te verzenden. 

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

## <a name="gcm-native-notifications-with-c-queue-triggers"></a>Systeemeigen GCM-meldingen met C# queue-triggers
Dit voorbeeld ziet u het gebruik van typen die zijn gedefinieerd de [bibliotheek van Microsoft Azure Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) een systeemeigen GCM-melding te verzenden. 

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

## <a name="wns-native-notifications-with-c-queue-triggers"></a>WNS systeemeigen meldingen met C# queue-triggers
Dit voorbeeld ziet u het gebruik van typen die zijn gedefinieerd de [bibliotheek van Microsoft Azure Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) een systeemeigen WNS toast-melding te verzenden. 

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

## <a name="template-example-for-nodejs-timer-triggers"></a>Voorbeeld van de sjabloon voor Node.js timer triggers
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

## <a name="template-example-for-f-timer-triggers"></a>Voorbeeld van de sjabloon voor F # timer triggers
In dit voorbeeld wordt een melding verzonden op een [sjabloon registratie](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) die bevat `location` en `message`.

```fsharp
let Run(myTimer: TimerInfo, notification: byref<IDictionary<string, string>>) =
    notification = dict [("location", "Redmond"); ("message", "Hello from F#!")]
```

## <a name="template-example-using-an-out-parameter"></a>Voorbeeld van de sjabloon met een out-parameter
In dit voorbeeld wordt een melding verzonden op een [sjabloon registratie](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) die bevat een `message` aanduiding in de sjabloon.

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

## <a name="template-example-with-asynchronous-function"></a>Voorbeeld van een sjabloon met asynchrone functie
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

## <a name="template-example-using-json"></a>Voorbeeld van de sjabloon met een JSON
In dit voorbeeld wordt een melding verzonden op een [sjabloon registratie](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) die bevat een `message` aanduiding in de sjabloon met een geldig JSON-tekenreeks.

```cs
using System;

public static void Run(string myQueueItem,  out string notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    notification = "{\"message\":\"Hello from C#. Processed a queue item!\"}";
}
```

## <a name="template-example-using-notification-hubs-library-types"></a>Voorbeeld van de sjabloon met Notification Hubs bibliotheek typen
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

## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

