---
title: Aan de slag met Azure Mobile Engagement voor iOS in Objective C | Microsoft Docs
description: Informatie over het gebruik van Azure Mobile Engagement met analyses en pushmeldingen voor iOS-apps.
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 117b5742-522b-41de-98c5-f432da2d98f8
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: hero-article
ms.date: 07/17/2017
ms.author: piyushjo
ms.openlocfilehash: 913a60df4ce7c431b1c260135785972aac00c69d
ms.sourcegitcommit: 234c397676d8d7ba3b5ab9fe4cb6724b60cb7d25
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-ios-apps-in-objective-c"></a>Aan de slag met Azure Mobile Engagement voor iOS-apps in Objective C
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In dit onderwerp leest u hoe u Azure Mobile Engagement gebruikt om inzicht te krijgen in het gebruik van uw apps, en om pushmeldingen te verzenden aan gesegmenteerde gebruikers van een iOS-toepassing.
In deze zelfstudie maakt u een lege iOS-app die basisgegevens verzamelt en pushmeldingen ontvangt via Apple Push Notification System (APNS).

Voor deze zelfstudie hebt u het volgende nodig:

* XCode 8, die u vanuit de Mac App Store kunt installeren.
* De [Mobile Engagement iOS SDK].

Het voltooien van deze zelfstudie is een vereiste voor alle andere Mobile Engagement-zelfstudies voor iOS-apps.

> [!NOTE]
> U hebt een actief Azure-account nodig om deze zelfstudie te voltooien. Als u geen account hebt, kunt u binnen een paar minuten een account voor de gratis proefversie maken. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-get-started) voor meer informatie.
>
>

## <a id="setup-azme"></a>Mobile Engagement instellen voor uw iOS-app
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a id="connecting-app"></a>Uw app verbinden met de back-end van Mobile Engagement
Deze zelfstudie toont een ‘basisintegratie’, de minimale set die vereist is voor het verzamelen van gegevens en verzenden van een pushmelding. De volledige integratiedocumentatie is te vinden in de [Mobile Engagement iOS SDK-integratie](mobile-engagement-ios-sdk-overview.md).

We gaan een eenvoudige app maken met XCode ter illustratie van de integratie.

### <a name="create-a-new-ios-project"></a>Een nieuw iOS-project maken
[!INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

### <a name="connect-your-app-to-the-mobile-engagement-backend"></a>Uw app verbinden met de back-end van Mobile Engagement
1. Download de [Mobile Engagement iOS SDK].
2. Pak het .tar.gz-bestand uit in een map op uw computer.
3. Klik met de rechtermuisknop op het project en selecteer **Add file to**.

    ![][1]

4. Navigeer naar de map waarin u de SDK hebt uitgepakt, selecteer de map `EngagementSDK`, klik op **Opties** in de hoek linksonder en zorg ervoor dat het selectievakje **Items kopiëren indien nodig** en het selectievakje voor uw doel zijn ingeschakeld. Druk vervolgens **OK**.

    ![][2]

5. Open het tabblad **Build Phases** en voeg in het menu **Link Binary With Libraries** de frameworks toe, zoals hieronder wordt weergegeven:

    ![][3]

6. Ga terug naar de Azure-portal op de pagina **Verbindingsgegevens** van de app en kopieer de verbindingsreeks.

    ![](../../includes/media/mobile-engagement-create-app-in-portal-new/app-connection-info.png)
7. Voeg de volgende regel code toe aan het bestand **AppDelegate.m**.

    ```obj-c
    #import "EngagementAgent.h"
    ```
8. Plak nu de verbindingsreeks in de gemachtigde `didFinishLaunchingWithOptions`.

    ```obj-c
    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
    {
            [...]   
            [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
            [...]
    }
    ```

9. `setTestLogEnabled` is een optionele instructie waarmee de SDK-logboeken worden ingeschakeld, zodat u problemen kunt identificeren.

## <a id="monitor"></a>Realtime-bewaking inschakelen
U dient ten minste één scherm (activiteit) naar de back-end van Mobile Engagement te sturen om te beginnen met het verzenden van gegevens en ervoor te zorgen dat de gebruikers actief zijn.

1. Open het bestand **ViewController.h** en importeer **EngagementViewController.h**:

    ```obj-c
    #import "EngagementViewController.h"
    ```

2. Vervang nu de bovenliggende klasse van de **ViewController**-interface door `EngagementViewController`:
 
    ```obj-c
   @interface ViewController : EngagementViewController
   ```

## <a id="monitor"></a>App verbinden met realtime-bewaking
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a id="integrate-push"></a>Pushmeldingen en in-app-berichten inschakelen
Met Mobile Engagement kunt u communiceren met uw gebruikers en ze bereiken met pushmeldingen en in-app-berichten in de context van campagnes. Deze module heet REACH in de Mobile Engagement-portal.
In de volgende secties stelt u de app in om die te ontvangen.

### <a name="enable-your-app-to-receive-silent-push-notifications"></a>Ontvangen van achtergrond-pushmeldingen inschakelen voor de app
[!INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

### <a name="add-the-reach-library-to-your-project"></a>De Reach-bibliotheek toevoegen aan uw project
1. Klik met de rechtermuisknop op het project.
2. Selecteer **Add file to**.
3. Navigeer naar de map waar u de SDK hebt uitgepakt.
4. Selecteer de map `EngagementReach`.
5. Klik op **Add**.

### <a name="modify-your-application-delegate"></a>Uw toepassingsgemachtigde wijzigen
1. Ga terug naar het bestand **AppDeletegate.m** en importeer de Reach-module.

    ```obj-c
    #import "AEReachModule.h"
    #import <UserNotifications/UserNotifications.h>
    ```

2. Binnen de methode `application:didFinishLaunchingWithOptions` maakt u een Reach-module en geeft die door aan uw bestaande initialisatieregel voor Engagement:

    ```obj-c
    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
        AEReachModule * reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
        [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
        [...]
        return YES;
    }
    ```

### <a name="enable-your-app-to-receive-apns-push-notifications"></a>Ontvangen van APNS-pushmeldingen inschakelen voor de app
1. Voeg de volgende regel toe aan de methode `application:didFinishLaunchingWithOptions`:

    ```obj-c
    if (NSFoundationVersionNumber >= NSFoundationVersionNumber_iOS_8_0)
    {
        if (NSFoundationVersionNumber > NSFoundationVersionNumber_iOS_9_x_Max)
        {
            [UNUserNotificationCenter.currentNotificationCenter requestAuthorizationWithOptions:(UNAuthorizationOptionBadge | UNAuthorizationOptionSound | UNAuthorizationOptionAlert) completionHandler:^(BOOL granted, NSError * _Nullable error) {}];
        }else
        {
            [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert)   categories:nil]];
        }
        [application registerForRemoteNotifications];
    }
    else
    {
        [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
    }
    ```
2. Voeg de methode `application:didRegisterForRemoteNotificationsWithDeviceToken` als volgt toe:

    ```obj-c
    - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
    {
            [[EngagementAgent shared] registerDeviceToken:deviceToken];
        NSLog(@"Registered Token: %@", deviceToken);
    }
    ```

3. Voeg de methode `didFailToRegisterForRemoteNotificationsWithError` als volgt toe:

    ```obj-c
    - (void)application:(UIApplication*)application didFailToRegisterForRemoteNotificationsWithError:(NSError*)error
    {
        NSLog(@"Failed to get token, error: %@", error);
    }
    ```

4. Voeg de methode `didReceiveRemoteNotification:fetchCompletionHandler` als volgt toe:

    ```obj-c
    - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
    }
    ```
    
[!INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
[Mobile Engagement iOS SDK]: http://aka.ms/qk2rnj

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png
