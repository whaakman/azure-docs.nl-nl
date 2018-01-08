---
title: Aan de slag met Azure Notification Hubs voor Xamarin.iOS-apps | Microsoft Docs
description: In deze zelfstudie leert u hoe u met Azure Notification Hubs pushmeldingen verzendt naar een Xamarin iOS-toepassing.
services: notification-hubs
keywords: ios-pushmeldingen,pushberichten,pushmeldingen,pushbericht
documentationcenter: xamarin
author: jwhiteDev
manager: kpiteira
editor: 
ms.assetid: 4d4dfd42-c5a5-4360-9d70-7812f96924d2
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 12/22/2017
ms.author: jawh
ms.openlocfilehash: edb48cd8de9b1f7357c40cea73fa4b9a422cb8fa
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="get-started-with-azure-notification-hubs-for-xamarinios-apps"></a>Aan de slag met Azure Notification Hubs voor Xamarin.iOS-apps
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Overzicht
> [!NOTE]
> U hebt een actief Azure-account nodig om deze zelfstudie te voltooien. Als u geen account hebt, kunt u binnen een paar minuten een gratis proefaccount maken. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started) voor meer informatie.
> 
> 

In deze zelfstudie wordt gedemonstreerd hoe u met Azure Notification Hubs pushmeldingen verzendt naar een iOS-toepassing. U maakt een lege Xamarin.iOS-app die pushmeldingen ontvangt met de [Apple Push Notification Service (APNs)](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html). 

Als u klaar bent, kunt u de Notification Hub gebruiken om pushmeldingen uit te zenden naar alle apparaten waarop uw app wordt uitgevoerd. De voltooide code is beschikbaar in het [NotificationHubs-app][GitHub]-voorbeeld.

In deze zelfstudie wordt een eenvoudig scenario voor het uitzenden van pushmeldingen met Notification Hubs beschreven.

## <a name="prerequisites"></a>Vereisten
Voor deze zelfstudie hebt u het volgende nodig:

* Meest recente versie van [Xcode][Install Xcode]
* Een apparaat dat compatibel is met iOS 10 (of een hogere versie)
* [Apple Developer Program](https://developer.apple.com/programs/)-lidmaatschap
* [Visual Studio voor Mac]
  
  > [!NOTE]
  > Vanwege configuratievereisten voor iOS-pushmeldingen moet u de voorbeeldtoepassing op een fysiek iOS-apparaat (iPhone of iPad) implementeren en testen in plaats van in de simulator.
  > 
  > 

Het voltooien van deze zelfstudie is een vereiste voor alle andere Notification Hubs-zelfstudies voor Xamarin.iOS-apps.

[!INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

## <a name="configure-your-notification-hub-for-ios-push-notifications"></a>De Notification Hub voor iOS-pushmeldingen configureren
In deze sectie wordt u begeleid bij het maken van een nieuwe Notification Hub en het configureren van verificatie met APNS met het **.p12**-pushcertificaat dat u hebt gemaakt. Als u een Notification Hub wilt gebruiken die u al hebt gemaakt, kunt u doorgaan naar stap 5.

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">

<li>

<p>Klik op de knop <b>Notification Services</b> en selecteer <b>Apple (APNS)</b>. Selecteer <b>Certificaat</b>, klik op het bestandspictogram en selecteer het <b>.p12</b>-bestand dat u eerder hebt geëxporteerd. Zorg ervoor dat u het juiste wachtwoord opgeeft.</p>

<p>Zorg ervoor dat u de modus <b>Sandbox</b> selecteert. Dit is de juiste ontwikkelingsmodus. Gebruik <b>Productie</b> alleen als u pushmeldingen wilt verzenden naar gebruikers die uw app in de winkel hebben aangeschaft.</p>
</li>
</ol>

&emsp;&emsp;&emsp;&emsp;![APNS configureren in Azure Portal][6]

&emsp;&emsp;&emsp;&emsp;![APNS-certificering in Azure Portal configureren][7]

De Notification Hub is nu geconfigureerd om te werken met APNs en u hebt de verbindingsreeksen om uw app te registreren en pushmeldingen te verzenden.

## <a name="connect-your-app-to-the-notification-hub"></a>Uw app verbinden met de Notification Hub
#### <a name="create-a-new-project"></a>Een nieuw project maken
1. Maak in Visual Studio een nieuw iOS-project, selecteer de sjabloon **Single View App** (Toepassing met één weergave) en klik op **Volgende**.
   
     ![Visual Studio - Toepassingstype selecteren][31]

2. Voer de app-naam en organisatie-id in, klik op **Volgende** en vervolgens op **Maken**

3. In de oplossingsweergave dubbelklikt u op *Into.plist* en zorgt u ervoor dat bij **Identiteit** de bundel-id overeenkomt met de id die werd gebruikt bij het maken van uw inrichtingsprofiel. Controleer onder **Ondertekening** of uw Developer-account is geselecteerd bij **Team**, of 'Ondertekening automatisch beheren' is geselecteerd en uw certificaat voor ondertekening en het profiel voor inrichting automatisch zijn geselecteerd.

    ![Visual Studio - iOS-app configureren][32]

4. Voeg het Azure Messaging-pakket toe. Klik in de oplossingsweergave met de rechtermuisknop op het project en selecteer **Toevoegen** > **NuGet-pakketten toevoegen**. Zoek naar **Xamarin.Azure.NotificationHubs.iOS** en voeg het pakket toe aan uw project.

5. Voeg een nieuw bestand toe aan de klasse, geef deze de naam **Constants.cs**, voeg de volgende variabelen toe en vervang de tijdelijke aanduidingen voor tekenreeksen door uw *hubnaam* en de *DefaultListenSharedAccessSignature* die u eerder hebt genoteerd.
   
    ```csharp
        // Azure app-specific connection string and hub path
        public const string ConnectionString = "<Azure connection string>";
        public const string NotificationHubPath = "<Azure hub path>";
    ```

6. Voeg in **AppDelegate.cs** de volgende instructie toe:
   
    ```csharp
        using WindowsAzure.Messaging;
    ```

7. Declareer een exemplaar van **SBNotificationHub**:
   
    ```csharp
        private SBNotificationHub Hub { get; set; }
    ```

8. Werk in **AppDelegate.cs** het item **FinishedLaunching()** zo bij dat het overeenkomt met het volgende:
   
    ```csharp
        public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
        {
            if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
                var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                       UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound,
                       new NSSet ());
   
                UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
                UIApplication.SharedApplication.RegisterForRemoteNotifications ();
            } else {
                UIRemoteNotificationType notificationTypes = UIRemoteNotificationType.Alert | UIRemoteNotificationType.Badge | UIRemoteNotificationType.Sound;
                UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (notificationTypes);
            }
   
            return true;
        }
    ```

9. Overschrijf de **RegisteredForRemoteNotifications()**-methode in **AppDelegate.cs**:
   
    ```csharp
        public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
        {
            Hub = new SBNotificationHub(Constants.ConnectionString, Constants.NotificationHubPath);
   
            Hub.UnregisterAllAsync (deviceToken, (error) => {
                if (error != null)
                {
                    Console.WriteLine("Error calling Unregister: {0}", error.ToString());
                    return;
                }
   
                NSSet tags = null; // create tags if you want
                Hub.RegisterNativeAsync(deviceToken, tags, (errorCallback) => {
                    if (errorCallback != null)
                        Console.WriteLine("RegisterNativeAsync error: " + errorCallback.ToString());
                });
            });
        }
    ```

10. Overschrijf de **ReceivedRemoteNotification()**-methode in **AppDelegate.cs**:
   
    ```csharp
        public override void ReceivedRemoteNotification(UIApplication application, NSDictionary userInfo)
        {
            ProcessNotification(userInfo, false);
        }
    ```

11. Maak de volgende **ProcessNotification()**-methode in **AppDelegate.cs**:
   
    ```csharp
        void ProcessNotification(NSDictionary options, bool fromFinishedLaunching)
        {
            // Check to see if the dictionary has the aps key.  This is the notification payload you would have sent
            if (null != options && options.ContainsKey(new NSString("aps")))
            {
                //Get the aps dictionary
                NSDictionary aps = options.ObjectForKey(new NSString("aps")) as NSDictionary;
   
                string alert = string.Empty;
   
                //Extract the alert text
                // NOTE: If you're using the simple alert by just specifying
                // "  aps:{alert:"alert msg here"}  ", this will work fine.
                // But if you're using a complex alert with Localization keys, etc.,
                // your "alert" object from the aps dictionary will be another NSDictionary.
                // Basically the JSON gets dumped right into a NSDictionary,
                // so keep that in mind.
                if (aps.ContainsKey(new NSString("alert")))
                    alert = (aps [new NSString("alert")] as NSString).ToString();
   
                //If this came from the ReceivedRemoteNotification while the app was running,
                // we of course need to manually process things like the sound, badge, and alert.
                if (!fromFinishedLaunching)
                {
                    //Manually show an alert
                    if (!string.IsNullOrEmpty(alert))
                    {
                        UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                        avAlert.Show();
                    }
                }
            }
        }
    ```
   > [!NOTE]
   > U kunt ervoor kiezen **FailedToRegisterForRemoteNotifications()** te overschrijven om situaties zoals het ontbreken van een netwerkverbinding af te handelen. Dit is vooral belangrijk wanneer de gebruiker de toepassing in de offlinemodus start (bijvoorbeeld in een vliegtuig) en u scenario's voor het verzenden van pushberichten wilt afhandelen die specifiek zijn voor uw app.
  

12. Voer de app uit op uw apparaat.

## <a name="sending-test-push-notifications"></a>Testpushmeldingen verzenden
U kunt ontvangst van meldingen in uw app testen met de optie *Test verzenden* in [Azure Portal]. Hiermee wordt een pushmelding als test naar uw apparaat verzonden.

![Azure Portal - Test verzenden][30]

Pushmeldingen worden gewoonlijk in een back-endservice zoals Mobile Apps of ASP.NET verzonden met een compatibele bibliotheek. U kunt de REST API ook rechtstreeks gebruiken om meldingsberichten te verzenden als er geen bibliotheek beschikbaar is voor uw back-end.

Als volgende stap wordt aanbevolen de zelfstudie [Use Notification Hubs to push notifications to users](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) (Notification Hubs gebruiken om pushmeldingen naar gebruikers te verzenden) door te nemen voor informatie over het verzenden van meldingen vanuit een ASP.NET-back-end. Voor het verzenden van meldingen kunt u echter de volgende methoden gebruiken:

Hier volgt een lijst met andere zelfstudies die u kunt bekijken voor het verzenden van meldingen:
* REST-interface: u kunt pushmeldingen op elk back-endplatform ondersteunen met behulp van de [REST-interface](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).
* **Microsoft Azure Notification Hubs .NET SDK**: in NuGet Package Manager voor Visual Studio voert u [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) uit.
* Node.js: [Notification Hubs gebruiken vanuit Node.js](notification-hubs-nodejs-push-notification-tutorial.md).
* Java/PHP**: zie 'Notification Hubs gebruiken vanuit Java/PHP' voor een voorbeeld van hoe u pushmeldingen verzendt met de REST API's ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).

## <a name="next-steps"></a>Volgende stappen
In dit eenvoudige voorbeeld hebt u pushmeldingen uitgezonden naar al uw iOS-apparaten. Als u zich op specifieke gebruikers wilt richten, raadpleegt u de zelfstudie [Notification Hubs gebruiken om pushmeldingen naar gebruikers te verzenden]. Als u gebruikers wilt indelen op belangengroepen, raadpleegt u [Notification Hubs gebruiken om belangrijk nieuws te verzenden]. Lees meer over het gebruik van Notification Hubs in [Richtlijnen voor Notification Hubs] en in [Notification Hubs-procedure voor iOS].

<!-- Images. -->
[6]: ./media/notification-hubs-ios-get-started/notification-hubs-apple-config.png
[7]: ./media/notification-hubs-ios-get-started/notification-hubs-apple-config-cert.png
[213]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-console-app.png

[215]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler1.png
[216]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler2.png

[30]: ./media/notification-hubs-ios-get-started/notification-hubs-test-send.png
[31]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-ios-app.png
[32]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-app-settings.png



<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Windows Azure Messaging Framework]: http://go.microsoft.com/fwlink/?LinkID=799698&clcid=0x409
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-xamarin-ios
[Richtlijnen voor Notification Hubs]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs-procedure voor iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456
[Visual Studio voor Mac]: https://www.visualstudio.com/vs/visual-studio-mac/
[Notification Hubs gebruiken om pushmeldingen naar gebruikers te verzenden]: /manage/services/notification-hubs/notify-users-aspnet
[Notification Hubs gebruiken om belangrijk nieuws te verzenden]: /manage/services/notification-hubs/breaking-news-dotnet

[Local and Push Notification Programming Guide]:https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/HandlingRemoteNotifications.html#//apple_ref/doc/uid/TP40008194-CH6-SW1
[Apple Push Notification Service]: http://go.microsoft.com/fwlink/p/?LinkId=272584

[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
[GitHub]: https://github.com/xamarin/mobile-samples/tree/master/Azure/NotificationHubs
[Azure-portal]: https://portal.azure.com
