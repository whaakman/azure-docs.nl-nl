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
ms.openlocfilehash: 752feca30fdca663aaf8bd88e6686781b9065682
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
Wanneer u de Sjabloonmeldingen verzendt, hoeft u alleen een set eigenschappen opgeven. In dit scenario wordt in de set eigenschappen van de gelokaliseerde versie van het huidige nieuws bevatten.

    {
        "News_English": "World News in English!",
        "News_French": "World News in French!",
        "News_Mandarin": "World News in Mandarin!"
    }



### <a name="send-notifications-using-a-c-console-app"></a>Verzenden van meldingen via een C#-consoletoepassing
Deze sectie wordt beschreven hoe u meldingen met een console-app verzendt. De code verzendt meldingen voor Windows Store en iOS-apparaten. Wijzig de `SendTemplateNotificationAsync` methode in de console-app die u eerder hebt gemaakt met de volgende code:

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

De methode SendTemplateNotificationAsync levert de gelokaliseerde stukje nieuws **alle** uw apparaten, ongeacht het platform. Uw notification hub bouwt en levert de nettolading van de juiste systeemeigen voor alle apparaten die zijn geabonneerd op een specifieke tag.

### <a name="sending-notification-with-mobile-services"></a>Verzenden van meldingen met Mobile Services
Gebruik het volgende script in uw scheduler Mobile Services:

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

