---
title: Aan de slag met Azure Notification Hubs voor iOS-apps | Microsoft Docs
description: In deze zelfstudie leert u hoe u met Azure Notification Hubs pushmeldingen verzendt naar een iOS-toepassing.
services: notification-hubs
documentationcenter: ios
keywords: pushmelding,pushmeldingen,ios-pushmeldingen
author: jwhitedev
manager: kpiteira
editor: 
ms.assetid: b7fcd916-8db8-41a6-ae88-fc02d57cb914
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: hero-article
ms.date: 12/22/2017
ms.author: jawh
ms.openlocfilehash: 0e9e7ab196eef790b74074be319cd8122cf3ff5c
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="get-started-with-azure-notification-hubs-for-ios-apps"></a>Aan de slag met Azure Notification Hubs voor iOS-apps
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Overzicht
> [!NOTE]
> U hebt een actief Azure-account nodig om deze zelfstudie te voltooien. Als u geen account hebt, kunt u binnen een paar minuten een gratis proefaccount maken. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-ios-get-started) voor meer informatie.
> 
> 

In deze zelfstudie wordt gedemonstreerd hoe u met Azure Notification Hubs pushmeldingen verzendt naar een iOS-toepassing. U maakt een lege iOS-app die pushmeldingen ontvangt met de [Apple Push Notification Service (APNs)](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html#//apple_ref/doc/uid/TP40008194-CH8-SW1). 

Als u klaar bent, kunt u de Notification Hub gebruiken om pushmeldingen uit te zenden naar alle apparaten waarop uw app wordt uitgevoerd.

## <a name="before-you-begin"></a>Voordat u begint
[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

De volledige code voor deze zelfstudie vindt u [op GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/iOS/GetStartedNH/GetStarted). 

## <a name="prerequisites"></a>Vereisten
Voor deze zelfstudie hebt u het volgende nodig:

* [Windows Azure Messaging Framework]
* Meest recente versie van [Xcode]
* Een apparaat dat compatibel is met iOS 10 (of een hogere versie)
* [Apple Developer Program](https://developer.apple.com/programs/)-lidmaatschap
  
  > [!NOTE]
  > Vanwege configuratievereisten voor pushmeldingen moet u pushmeldingen op een fysiek iOS-apparaat (iPhone of iPad) implementeren en testen in plaats van in de iOS-simulator.
  > 
  > 

Het voltooien van deze zelfstudie is een vereiste voor alle andere Notification Hubs-zelfstudies voor iOS-apps.

[!INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

## <a name="configure-your-notification-hub-for-ios-push-notifications"></a>De Notification Hub voor iOS-pushmeldingen configureren
In deze sectie wordt u begeleid bij het maken van een nieuwe Notification Hub en het configureren van verificatie met APNS met het **.p12**-pushcertificaat dat u hebt gemaakt. Als u een Notification Hub wilt gebruiken die u al hebt gemaakt, kunt u doorgaan naar stap 5.

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">

<li>

<p>Onder <b>Notification Services</b> selecteert u <b>Apple (APNS)</b>. Selecteer <b>Certificaat</b>, klik op het bestandspictogram en selecteer het <b>.p12</b>-bestand dat u eerder hebt geëxporteerd. Zorg ervoor dat u het juiste wachtwoord opgeeft.</p>

<p>Zorg ervoor dat u de modus <b>Sandbox</b> selecteert. Dit is de juiste ontwikkelingsmodus. Gebruik <b>Productie</b> alleen als u pushmeldingen wilt verzenden naar gebruikers die uw app in de winkel hebben aangeschaft.</p>
</li>
</ol>
&emsp;&emsp;&emsp;&emsp;![APNS configureren in Azure Portal][6]

&emsp;&emsp;&emsp;&emsp;![APNS-certificering in Azure Portal configureren][7]

De Notification Hub is nu geconfigureerd om te werken met APNS en u hebt de verbindingsreeksen om uw app te registreren en pushmeldingen te verzenden.

## <a name="connect-your-ios-app-to-notification-hubs"></a>Uw iOS-app verbinden met Notification Hubs
1. Maak in Xcode een nieuw iOS-project en selecteer de sjabloon **Single View Application** (Toepassing met één weergave).
   
    ![Xcode - Toepassing voor één weergave][8]
    
2. Bij het instellen van de opties voor het nieuwe project moet u dezelfde **productnaam** en **organisatie-id** gebruiken als bij het instellen van de bundel-id in de Apple Developer-portal.
   
    ![Xcode - Projectopties][11]
    
3. Klik onder Projectnavigator op de naam van uw project, klik op het tabblad **Algemeen** en zoek naar **Ondertekening**. Zorg ervoor dat u het juiste team voor uw Apple Developer-account selecteert. XCode moet automatisch het profiel voor inrichting openen dat u eerder op basis van uw bundel-id hebt gemaakt.
   
    Als u het nieuwe profiel voor inrichting dat u hebt gemaakt in Xcode niet ziet, vernieuwt u de profielen voor uw identiteit voor ondertekening. Klik op **Xcode** in de menubalk, klik op **Preferences** (Voorkeuren), klik op het tabblad **Account** en klik op de knop **View Details** (Details weergeven), klik op uw identiteit voor ondertekening en klik vervolgens op de knop voor vernieuwen in de rechterbenedenhoek.

    ![Xcode - Profiel voor inrichting][9]

4. Selecteer het tabblad **Mogelijkheden** en zorg ervoor dat Pushmeldingen is ingeschakeld

    ![Xcode - pushmogelijkheden][12]
   
5. Download het [Windows Azure Messaging Framework] en pak het bestand uit. Klik met de rechtermuisknop op uw project in Xcode en klik op de optie **Add Files to** (Bestanden toevoegen aan) om de map **WindowsAzureMessaging.framework** aan uw Xcode-project toe te voegen. Selecteer **Options** (Opties), zorg ervoor dat **Copy items if needed** (Copy items if needed) is geselecteerd en klik op **Add** (Toevoegen).

    ![Azure SDK uitpakken][10]

6. Voeg een nieuw headerbestand toe aan uw project met de naam **HubInfo.h**. Dit bestand is bedoeld voor de constanten voor uw Notification Hub. Voeg de volgende definities toe en vervang de tijdelijke aanduidingen voor tekenreeksen door uw *hubnaam* en de *DefaultListenSharedAccessSignature* die u eerder hebt genoteerd.

    ```obj-c
        #ifndef HubInfo_h
        #define HubInfo_h
   
            #define HUBNAME @"<Enter the name of your hub>"
            #define HUBLISTENACCESS @"<Enter your DefaultListenSharedAccess connection string"
   
        #endif /* HubInfo_h */
    ```
    
7. Open uw **AppDelegate.h**-bestand en voeg de volgende instructies voor importeren toe:

    ```obj-c
        #import <WindowsAzureMessaging/WindowsAzureMessaging.h>
        #import <UserNotifications/UserNotifications.h> 
        #import "HubInfo.h"
    ```
8. Voeg in uw **AppDelegate.m-bestand** de volgende code toe in de **didFinishLaunchingWithOptions**-methode op basis van uw iOS-versie. Deze code registreert uw apparaatingang met APNS:

    ```obj-c
        UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
            UIUserNotificationTypeAlert | UIUserNotificationTypeBadge categories:nil];
   
        [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
        [[UIApplication sharedApplication] registerForRemoteNotifications];
    ```
   
9. Voeg in hetzelfde bestand de volgende methoden toe:

    ```obj-c
         - (void) application:(UIApplication *) application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *) deviceToken {
           SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:HUBLISTENACCESS
                                        notificationHubPath:HUBNAME];
   
            [hub registerNativeWithDeviceToken:deviceToken tags:nil completion:^(NSError* error) {
               if (error != nil) {
                   NSLog(@"Error registering for notifications: %@", error);
                }
                else {
                   [self MessageBox:@"Registration Status" message:@"Registered"];
              }
          }];
         }
   
        -(void)MessageBox:(NSString *) title message:(NSString *)messageText
        {
         UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }
    ```

    Deze code maakt verbinding met de Notification Hub met de verbindingsgegevens die u hebt opgegeven in HubInfo.h. Er wordt vervolgens een apparaattoken aan de Notification Hub toegekend, zodat de Notification Hub meldingen kan verzenden.

10. Voeg in hetzelfde bestand de volgende methode toe om een **UIAlert** weer te geven als de melding is ontvangen terwijl de app actief is:

    ```obj-c
            - (void)application:(UIApplication *)application didReceiveRemoteNotification: (NSDictionary *)userInfo {
               NSLog(@"%@", userInfo);
               [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
           }
    ```

11. Ontwikkel en voer de app op uw apparaat uit om te controleren of er geen fouten zijn.

## <a name="send-test-push-notifications"></a>Testpushmeldingen verzenden
U kunt ontvangst van meldingen in uw app testen met de optie *Test verzenden* in [Azure Portal]. Hiermee wordt een pushmelding naar uw apparaat verzonden.

![Azure Portal - Test verzenden][30]

[!INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]


## <a name="checking-if-your-app-can-receive-push-notifications"></a>Controleren of een app pushmeldingen kan ontvangen
Als u pushmeldingen op iOS wilt testen, moet u de app implementeren op een fysiek iOS-apparaat. U kunt geen Apple pushmeldingen verzenden via de iOS-simulator.

1. Voer de app uit en controleer of de registratie is gelukt en druk vervolgens op **OK**.
   
    ![De registratie van pushmeldingen in iOS-apps testen][33]
2. Vervolgens gaat u een pushmelding als test verzenden via [Azure Portal], zoals hierboven beschreven. 

3. De pushmelding wordt verzonden naar alle apparaten die zijn geregistreerd voor het ontvangen van meldingen van de specifieke Notification Hub.
   
    ![Het ontvangen van pushmeldingen in iOS-apps testen][35]

## <a name="next-steps"></a>Volgende stappen
In dit eenvoudige voorbeeld hebt u pushmeldingen uitgezonden naar al uw geregistreerde iOS-apparaten. Een aanbevolen volgende stap in uw leerproces is te beginnen met de zelfstudie [Azure Notification Hubs waarschuwen gebruikers voor iOS met .NET-back-end]. Deze handleiding helpt u bij het maken van een back-end voor het verzenden van pushmeldingen met tags. 

Zie de zelfstudie [Notification Hubs gebruiken om belangrijk nieuws te verzenden] als u gebruikers wilt indelen op belangengroep. 

Zie [Richtlijnen voor Notification Hubs] voor algemene informatie over Notification Hubs.

<!-- Images. -->

[6]: ./media/notification-hubs-ios-get-started/notification-hubs-apple-config.png
[7]: ./media/notification-hubs-ios-get-started/notification-hubs-apple-config-cert.png
[8]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app.png
[9]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app2.png
[10]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app3.png
[11]: ./media/notification-hubs-ios-get-started/notification-hubs-xcode-product-name.png
[12]: ./media/notification-hubs-ios-get-started/notification-hubs-enable-push.png

[30]: ./media/notification-hubs-ios-get-started/notification-hubs-test-send.png

[31]: ./media/notification-hubs-ios-get-started/notification-hubs-ios-ui.png
[32]: ./media/notification-hubs-ios-get-started/notification-hubs-storyboard-view.png
[33]: ./media/notification-hubs-ios-get-started/notification-hubs-test1.png
[35]: ./media/notification-hubs-ios-get-started/notification-hubs-test3.png



<!-- URLs. -->
[Windows Azure Messaging Framework]: http://go.microsoft.com/fwlink/?LinkID=799698&clcid=0x409
[Mobile Services iOS SDK]: http://go.microsoft.com/fwLink/?LinkID=266533
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Richtlijnen voor Notification Hubs]: http://msdn.microsoft.com/library/jj927170.aspx
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-ios-get-started-push.md
[Azure Notification Hubs waarschuwen gebruikers voor iOS met .NET-back-end]: notification-hubs-aspnet-backend-ios-apple-apns-notification.md
[Notification Hubs gebruiken om belangrijk nieuws te verzenden]: notification-hubs-ios-xplat-segmented-apns-push-notification.md

[Local and Push Notification Programming Guide]: http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW1
[Azure Portal]: https://portal.azure.com
