---
title: Pushmeldingen toevoegen aan uw Xamarin.iOS-app met Azure App Service
description: Informatie over het gebruik van Azure App Service om pushmeldingen te verzenden naar uw Xamarin.iOS-app
services: app-service\mobile
documentationcenter: xamarin
author: conceptdev
manager: crdun
editor: 
ms.assetid: 2921214a-49f8-45e1-a306-a85ce21defca
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 10/12/2016
ms.author: crdun
ms.openlocfilehash: b8d5a8d8725e2e9412cef7c377b17a77f34be27d
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="add-push-notifications-to-your-xamarinios-app"></a>Pushmeldingen toevoegen aan uw App voor Xamarin.iOS
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Overzicht
In deze zelfstudie hebt u pushmeldingen toevoegen de [Xamarin.iOS snel starten](app-service-mobile-xamarin-ios-get-started.md) project, zodat een pushmelding wordt verzonden naar het apparaat telkens wanneer een record wordt ingevoegd.

Als u het gedownloade quick start-serverproject niet gebruikt, moet u het push notification-uitbreidingspakket. Zie [werken met de .NET-back-endserver SDK voor Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) voor meer informatie.

## <a name="prerequisites"></a>Vereisten
* Voltooi de [Xamarin.iOS Quick Start](app-service-mobile-xamarin-ios-get-started.md) zelfstudie.
* Een fysiek iOS-apparaat. Pushmeldingen worden niet ondersteund door de iOS-simulator.

## <a name="register-the-app-for-push-notifications-on-apples-developer-portal"></a>De app voor pushmeldingen op Apple developer-portal te registreren
[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## <a name="configure-your-mobile-app-to-send-push-notifications"></a>Configureer uw mobiele App om pushmeldingen te verzenden
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

## <a name="update-the-server-project-to-send-push-notifications"></a>Bijwerken van het serverproject om pushmeldingen te verzenden
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a name="configure-your-xamarinios-project"></a>Configureer uw Xamarin.iOS-project
[!INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]

## <a name="add-push-notifications-to-your-app"></a>Pushmeldingen toevoegen aan uw app
1. In **QSTodoService**, de volgende eigenschap toevoegen zodat **AppDelegate** mobiele clients kunt verkrijgen:
   
            public MobileServiceClient GetClient {
            get
            {
                return client;
            }
            private set
            {
                client = value;
            }
        }
2. Voeg de volgende `using` instructie boven aan de **AppDelegate.cs** bestand.
   
        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;
3. In **AppDelegate**, overschrijven de **FinishedLaunching** gebeurtenis:
   
        public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
        {
            // registers for push for iOS8
            var settings = UIUserNotificationSettings.GetSettingsForTypes(
                UIUserNotificationType.Alert
                | UIUserNotificationType.Badge
                | UIUserNotificationType.Sound,
                new NSSet());
   
            UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
            UIApplication.SharedApplication.RegisterForRemoteNotifications();
   
            return true;
        }
4. In hetzelfde bestand, overschrijven de **RegisteredForRemoteNotifications** gebeurtenis. In deze code registreert u voor een eenvoudige sjabloon melding die door de server worden verzonden voor alle ondersteunde platforms.
   
    Zie voor meer informatie over sjablonen met Notification Hubs [sjablonen](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).

        public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
        {
            MobileServiceClient client = QSTodoService.DefaultService.GetClient;

            const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBodyAPNS}
            };

            // Register for push with your mobile app
            var push = client.GetPush();
            push.RegisterAsync(deviceToken, templates);
        }


1. Vervolgens, overschrijven de **DidReceivedRemoteNotification** gebeurtenis:
   
        public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
        {
            NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;
   
            string alert = string.Empty;
            if (aps.ContainsKey(new NSString("alert")))
                alert = (aps [new NSString("alert")] as NSString).ToString();
   
            //show alert
            if (!string.IsNullOrEmpty(alert))
            {
                UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                avAlert.Show();
            }
        }

Uw app is nu bijgewerkt ter ondersteuning van pushmeldingen.

## <a name="test"></a>Pushmeldingen testen in uw app
1. Druk op de **uitvoeren** klikken om het project bouwen en starten van de app in een compatibele iOS-apparaat en klik vervolgens op **OK** pushmeldingen accepteren.
   
   > [!NOTE]
   > U moet expliciet pushmeldingen accepteren van uw app. Deze aanvraag treedt alleen op de eerste keer dat de app wordt uitgevoerd.
   > 
   > 
2. Typ een taak in de app en klik vervolgens op het plusteken (**+**) pictogram.
3. Controleer of een melding wordt ontvangen en klik op **OK** kunnen worden verwijderd van de melding.
4. Herhaal stap 2 en onmiddellijk sluit de app en controleer of er een melding wordt weergegeven.

Deze zelfstudie hebt voltooid.

<!-- Images. -->

<!-- URLs. -->



