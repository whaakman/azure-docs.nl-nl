---
title: Cross-platform meldingen verzenden naar gebruikers met Azure Notification Hubs (ASP.NET)
description: Informatie over het gebruik van Notification Hubs-sjablonen te verzenden, in een afzonderlijke aanvraag, een melding platform networkdirect die gericht is op alle platforms.
services: notification-hubs
documentationcenter: 
author: ysxu
manager: erikre
editor: 
ms.assetid: 11d2131b-f683-47fd-a691-4cdfc696f62b
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: multiple
ms.topic: article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: 3c6dde338cb154f0cbe02642e4ff0f81d070aa25
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="send-cross-platform-notifications-to-users-with-notification-hubs"></a>Cross-platform meldingen verzenden naar gebruikers met Notification Hubs
In een vorige zelfstudie [meldingen verzenden naar gebruikers met Notification Hubs], hebt u geleerd hoe u pushmeldingen naar alle apparaten die zijn geregistreerd voor een specifieke geverifieerde gebruiker. In deze zelfstudie zijn meerdere aanvragen vereist een melding te verzenden naar elke ondersteund clientplatform. Azure Notification Hubs ondersteunt sjablonen, waarmee u kunt opgeven hoe een specifiek apparaat wil ontvangen van meldingen. Deze methode vereenvoudigt het verzenden van meldingen op meerdere platforms. 

In dit artikel laat zien hoe om te profiteren van sjablonen te verzenden, in een afzonderlijke aanvraag, een melding platform networkdirect die gericht is op alle platforms. Zie voor meer informatie over sjablonen, [overzicht van Azure Notification Hubs][Templates].

> [!IMPORTANT]
> Projecten van Windows Phone 8.1 en lager worden niet ondersteund in Visual Studio 2017. Zie [Geschikte platforms voor Visual Studio 2017 en compatibiliteit](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs) voor meer informatie.

> [!NOTE]
> Een apparaat kan meerdere sjablonen met dezelfde tag registreren met Notification Hubs. In dit geval wordt een binnenkomend bericht die gericht is op de resultaten van de tag in meerdere meldingen aan het apparaat, één voor elke sjabloon geleverd. Dit proces kunt u hetzelfde bericht in meerdere visual meldingen, zoals beide als een badge en als een pop-upmelding in Windows Store-Apps weergegeven.
> 
> 

Cross-platform om meldingen te verzenden met behulp van sjablonen, het volgende doen:

1. Vouw in Solution Explorer in Visual Studio de **domeincontrollers** map en open vervolgens het bestand RegisterController.cs.

2. Zoek het codeblok in de **plaatsen** methode die een nieuwe registratie maakt en vervang de `switch` inhoud met de volgende code:
   
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
            case "gcm":
                var messageTemplate = "{\"data\":{\"message\":\"$(message)\"}}";
                registration = new GcmTemplateRegistrationDescription(deviceUpdate.Handle, messageTemplate);
                break;
            default:
                throw new HttpResponseException(HttpStatusCode.BadRequest);
        }
   
    Deze code roept de platform-specifieke methode voor het maken van de registratie van een sjabloon in plaats van de registratie van een systeemeigen. Omdat de sjabloon registraties afgeleid van systeemeigen registraties, moet u geen bestaande rapporten wijzigen.

3. In de **meldingen** -controller, vervang de **sendNotification** methode met de volgende code:
   
        public async Task<HttpResponseMessage> Post()
        {
            var user = HttpContext.Current.User.Identity.Name;
            var userTag = "username:" + user;
   
            var notification = new Dictionary<string, string> { { "message", "Hello, " + user } };
            await Notifications.Instance.Hub.SendTemplateNotificationAsync(notification, userTag);
   
            return Request.CreateResponse(HttpStatusCode.OK);
        }
   
    Deze code verzendt een melding op alle platforms op hetzelfde moment, zonder dat u moet een systeemeigen nettolading opgeven. Notification Hubs bouwt en levert de nettolading van de juiste voor elk apparaat met de opgegeven *tag* waarde, zoals opgegeven in de geregistreerde sjablonen.

4. Uw WebApi-back-end-project opnieuw te publiceren.

5. Voer de clientapp opnieuw en controleer vervolgens of de registratie is geslaagd.

6. (Optioneel) De clientapp implementeren op een tweede apparaat en voer vervolgens de app.
    Houd er rekening mee dat er een melding wordt weergegeven op elk apparaat.

## <a name="next-steps"></a>Volgende stappen
Nu dat u deze zelfstudie hebt voltooid, meer informatie over Notification Hubs en sjablonen in de volgende onderwerpen:

* [Use Notification Hubs to send breaking news]: Demonstrates another scenario for using templates.
* [Overzicht van Azure Notification Hubs][Templates]: bevat gedetailleerde informatie over sjablonen.

<!-- Anchors. -->

<!-- Images. -->




<!-- URLs. -->
[Push to users ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Push to users Mobile Services]: /manage/services/notification-hubs/notify-users/
[Visual Studio 2012 Express for Windows 8]: http://go.microsoft.com/fwlink/?LinkId=257546

[Use Notification Hubs to send breaking news]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[Azure Notification Hubs]: http://go.microsoft.com/fwlink/p/?LinkId=314257
[meldingen verzenden naar gebruikers met Notification Hubs]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Templates]: http://go.microsoft.com/fwlink/p/?LinkId=317339
[Notification Hub How to for Windows Store]: http://msdn.microsoft.com/library/windowsazure/jj927172.aspx
