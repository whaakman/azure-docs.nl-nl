---
title: bestand opnemen
description: bestand opnemen
services: notification-hubs
author: spelluru
ms.service: notification-hubs
ms.topic: include
ms.date: 04/02/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 527567ee3f3a939c7358fb6a62271cbe38e16974
ms.sourcegitcommit: ebb460ed4f1331feb56052ea84509c2d5e9bd65c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "42915464"
---
Wanneer u de Sjabloonmeldingen verzendt, hoeft u alleen voor een set eigenschappen. In dit scenario bevatten de set eigenschappen van de gelokaliseerde versie van het huidige nieuws.

```json
{
    "News_English": "World News in English!",
    "News_French": "World News in French!",
    "News_Mandarin": "World News in Mandarin!"
}
```

### <a name="send-notifications-using-a-c-console-app"></a>Verzenden van meldingen met behulp van een C#-consoletoepassing

Deze sectie wordt beschreven hoe u meldingen met behulp van een console-app verzendt. De code verzendt meldingen naar Windows Store- en iOS-apparaten. Wijzig de methode `SendTemplateNotificationAsync` in de console-app die u eerder hebt gemaakt met de volgende code:

```csharp
private static async void SendTemplateNotificationAsync()
{
    // Define the notification hub.
    NotificationHubClient hub = 
        NotificationHubClient.CreateClientFromConnectionString(
            "<connection string with full access>", "<hub name>");

    // Sending the notification as a template notification. All template registrations that contain 
    // "messageParam" or "News_<local selected>" and the proper tags will receive the notifications. 
    // This includes APNS, GCM, WNS, and MPNS template registrations.
    Dictionary<string, string> templateParams = new Dictionary<string, string>();

    // Create an array of breaking news categories.
    var categories = new string[] { "World", "Politics", "Business", "Technology", "Science", "Sports"};
    var locales = new string[] { "English", "French", "Mandarin" };

    foreach (var category in categories)
    {
        templateParams["messageParam"] = "Breaking " + category + " News!";

        // Sending localized News for each tag too...
        foreach( var locale in locales)
        {
            string key = "News_" + locale;

            // Your real localized news content would go here.
            templateParams[key] = "Breaking " + category + " News in " + locale + "!";
        }

        await hub.SendTemplateNotificationAsync(templateParams, category);
    }
}
```

De methode SendTemplateNotificationAsync levert de gelokaliseerde stukje nieuws **alle** uw apparaten, ongeacht het platform. Uw notification hub bouwt en levert de juiste systeemeigen nettolading op alle apparaten die zijn geabonneerd op een specifieke tag.

### <a name="sending-notification-with-mobile-services"></a>Mededeling met Mobile Services

Gebruik het volgende script in uw Mobile Services scheduler:

```csharp
var azure = require('azure');
var notificationHubService = azure.createNotificationHubService('<hub name>', '<connection string with full access>');
var notification = {
        "News_English": "World News in English!",
        "News_French": "World News in French!",
        "News_Mandarin", "World News in Mandarin!"
}
notificationHubService.send('World', notification, function(error) {
    if (!error) {
        console.warn("Notification successful");
    }
});
```
