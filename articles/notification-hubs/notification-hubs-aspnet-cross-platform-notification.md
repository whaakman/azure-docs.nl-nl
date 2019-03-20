---
title: Platformonafhankelijke meldingen verzenden naar gebruikers met Azure Notification Hubs (ASP.NET)
description: Informatie over het gebruik van Notification Hubs-sjablonen te verzenden, in een enkele aanvraag, een platformonafhankelijke melding die gericht is op alle platforms.
services: notification-hubs
documentationcenter: ''
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: 11d2131b-f683-47fd-a691-4cdfc696f62b
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: multiple
ms.topic: article
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: 0f92b49c9d77029a9624782b49eb23f7083c49aa
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57863090"
---
# <a name="send-cross-platform-notifications-to-users-with-notification-hubs"></a>Platformonafhankelijke meldingen verzenden naar gebruikers met Notification Hubs

In een vorige zelfstudie [meldingen verzenden naar gebruikers met Notification Hubs], hebt u geleerd hoe u pushmeldingen verzenden naar alle apparaten die zijn geregistreerd bij een geverifieerde gebruiker. In deze zelfstudie zijn meerdere aanvragen vereist een melding verzenden naar elke ondersteunde client-platform. Azure Notification Hubs biedt ondersteuning voor sjablonen waarmee u kunt opgeven hoe een specifiek apparaat wil om meldingen te ontvangen. Deze methode vereenvoudigt platformonafhankelijke meldingen verzenden.

In dit artikel laat zien hoe u kunt profiteren van sjablonen te verzenden, in een enkele aanvraag, een platformonafhankelijke melding die gericht is op alle platforms. Zie voor meer informatie over sjablonen, [overzicht van Azure Notification Hubs][Templates].

> [!IMPORTANT]
> Projecten voor Windows Phone 8.1 en oudere versies worden niet ondersteund in Visual Studio 2017. Zie [Geschikte platforms voor Visual Studio 2017 en compatibiliteit](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs) voor meer informatie.

> [!NOTE]
> Met Notification Hubs, kan een apparaat meerdere sjablonen met dezelfde tag registreren. In dit geval wordt een binnenkomend bericht die gericht is op de resultaten van de tag in meerdere meldingen aan het apparaat, één voor elke sjabloon geleverd. Dit proces kunt u hetzelfde bericht in meerdere visual meldingen, zoals beide als een badge- en als een pop-upmelding in een Windows Store-app weergeven.

## <a name="send-cross-platform-notifications-using-templates"></a>Verzenden van platformonafhankelijke meldingen met behulp van sjablonen

Cross-platform om meldingen te verzenden met behulp van sjablonen, het volgende doen:

1. Vouw in Solution Explorer in Visual Studio, de **Controllers** map en open vervolgens het bestand RegisterController.cs.

2. Zoek het blok van code in de `Put` methode maakt u een nieuwe registratie en vervang de `switch` inhoud met de volgende code:

    ```csharp
    switch (deviceUpdate.Platform)
    {
        case "mpns":
            var toastTemplate = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                "<wp:Notification xmlns:wp=\"WPNotification\">" +
                    "<wp:Toast>" +
                        "<wp:Text1>$(message)</wp:Text1>" +
                    "</wp:Toast> " +
                "</wp:Notification>";
            registration = new MpnsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
            break;
        case "wns":
            toastTemplate = @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(message)</text></binding></visual></toast>";
            registration = new WindowsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
            break;
        case "apns":
            var alertTemplate = "{\"aps\":{\"alert\":\"$(message)\"}}";
            registration = new AppleTemplateRegistrationDescription(deviceUpdate.Handle, alertTemplate);
            break;
        case "fcm":
            var messageTemplate = "{\"data\":{\"message\":\"$(message)\"}}";
            registration = new FcmTemplateRegistrationDescription(deviceUpdate.Handle, messageTemplate);
            break;
        default:
            throw new HttpResponseException(HttpStatusCode.BadRequest);
    }
    ```

    Deze code roept de platform-specifieke methode voor het maken van de registratie van een sjabloon in plaats van de registratie van een systeemeigen. Omdat sjabloonregistraties afgeleid van systeemeigen registraties, hoeft u niet te wijzigen van bestaande registraties.

3. In de `Notifications` -controller, vervangen de `sendNotification` methode met de volgende code:

    ```csharp
    public async Task<HttpResponseMessage> Post()
    {
        var user = HttpContext.Current.User.Identity.Name;
        var userTag = "username:" + user;

        var notification = new Dictionary<string, string> { { "message", "Hello, " + user } };
        await Notifications.Instance.Hub.SendTemplateNotificationAsync(notification, userTag);

        return Request.CreateResponse(HttpStatusCode.OK);
    }
    ```

    Deze code verzendt een melding naar alle platforms op hetzelfde moment, zonder dat u ondervindt om op te geven van een systeemeigen nettolading. Notification Hubs bouwt en levert de juiste nettolading aan elk apparaat met de opgegeven *tag* waarde, zoals opgegeven in de geregistreerde sjablonen.

4. Uw back-end WebApi project publiceren.

5. Voer de client-app opnieuw uit en controleer vervolgens of dat de registratie is voltooid.

6. (Optioneel) De client-app implementeren op een tweede apparaat en voer vervolgens de app.
    Een melding wordt weergegeven op elk apparaat.

## <a name="next-steps"></a>Volgende stappen

Nu dat u deze zelfstudie hebt voltooid, vindt u meer informatie over Notification Hubs en sjablonen in de volgende onderwerpen:

* [Use Notification Hubs to send breaking news]: Demonstrates another scenario for using templates.
* [Overzicht van Azure Notification Hubs][Templates]: Bevat meer gedetailleerde informatie over sjablonen.

<!-- Anchors. -->

<!-- Images. -->

<!-- URLs. -->
[Push to users ASP.NET]: notification-hubs-aspnet-backend-ios-apple-apns-notification.md
[Push to users Mobile Services]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Visual Studio 2012 Express for Windows 8]: https://go.microsoft.com/fwlink/?LinkId=257546

[Use Notification Hubs to send breaking news]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[Azure Notification Hubs]: https://go.microsoft.com/fwlink/p/?LinkId=314257
[Meldingen verzenden naar gebruikers met Notification Hubs]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Templates]: https://go.microsoft.com/fwlink/p/?LinkId=317339
[Notification Hub How to for Windows Store]: https://msdn.microsoft.com/library/windowsazure/jj927172.aspx
