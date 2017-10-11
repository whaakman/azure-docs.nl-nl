---
title: Azure Mobile Engagement iOS SDK overzicht | Microsoft Docs
description: Meest recente updates en procedures voor iOS SDK voor Azure Mobile Engagement
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 3a03bbd6-bcf8-436c-9775-5a8188629252
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 07/17/2017
ms.author: piyushjo
ms.openlocfilehash: 6acd343782a3ee07750e27ec3022ff81cedfadee
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="ios-sdk-for-azure-mobile-engagement"></a>iOS-SDK voor Azure Mobile Engagement
Begin hier voor alle informatie over het integreren van Azure Mobile Engagement in een iOS-App. Als u u het eerst proberen wilt, controleert u of u doen onze [15 minuten zelfstudie](mobile-engagement-ios-get-started.md).

Klik om te zien de [SDK-inhoud](mobile-engagement-ios-sdk-content.md)

## <a name="integration-procedures"></a>Integratie procedures
1. Begin hier: [Mobile Engagement integreren in uw iOS-app](mobile-engagement-ios-integrate-engagement.md)
2. Voor meldingen: [Reach (meldingen) integreren in uw iOS-app](mobile-engagement-ios-integrate-engagement-reach.md)
3. Tag plan implementatie: [hoe u de geavanceerde Mobile Engagement tags API in uw iOS-app](mobile-engagement-ios-use-engagement-api.md)

## <a name="release-notes"></a>Releaseopmerkingen
### <a name="410-07172017"></a>4.1.0 (07/17/2017)
* Vaste badges uitgeschakeld op de achtergrond.
* Vaste waarschuwingen op 9 over API's niet aangeroepen in hoofdwachtrij XCode.
* Een geheugenlek vast Reach polls.
* Ondersteuning voor iOS verwijderd 6.X. Vanaf deze versie van het implementatiedoel van uw toepassing moet ten minste iOS 7.

Zie voor een eerdere versie de [voltooien release-opmerkingen](mobile-engagement-ios-release-notes.md)

## <a name="upgrade-procedures"></a>Upgradeprocedures
Als u hebt al een oudere versie van Engagement geïntegreerd in uw toepassing, hebt u de volgende punten overwegen bij het upgraden van de SDK.

Wellicht hebt u verschillende procedures volgen als u verschillende versies van de SDK van de volledige Zie gemist [Procedures Upgrade](mobile-engagement-ios-upgrade-procedure.md).

Voor elke nieuwe versie van de SDK moet u eerst vervangen (verwijderen en opnieuw te importeren in xcode) de mappen EngagementSDK en EngagementReach.

### <a name="from-300-to-400"></a>Van 3.0.0 naar 4.0.0
### <a name="xcode-8"></a>XCode 8
XCode 8 is verplicht vanaf versie 4.0.0 van de SDK.

> [!NOTE]
> Als u echt afhankelijk van XCode 7 zijn en u kunt de [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh). Er is een bekend probleem in de vorige versie van de reach-module tijdens het uitvoeren van op iOS-10-apparaten: Er zijn geen kennisgevingen systeem waarop actie is ondernomen. Om op te lossen dit er voor het implementeren van de afgeschafte API `application:didReceiveRemoteNotification:` in uw app delegeren als volgt:
>
>

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [!IMPORTANT]
> **Deze tijdelijke oplossing wordt niet aanbevolen** zoals dit gedrag in een toekomstige (zelfs secundaire) iOS-versie-upgrade wijzigen kunt omdat deze API voor iOS is afgeschaft. U moet zo snel mogelijk overschakelen naar XCode 8.
>
>

#### <a name="usernotifications-framework"></a>UserNotifications framework
U wilt toevoegen de `UserNotifications` framework in uw fasen bouwen.

uw project openen in de Projectverkenner en selecteert u het juiste doel. Open vervolgens de **'Buildfasen'** tabblad en in de **'Link Binary With Libraries'** menu framework toevoegen `UserNotifications.framework` -de koppeling als instellen`Optional`

#### <a name="application-push-capability"></a>Toepassing push mogelijkheid
XCode 8 opnieuw kunnen instellen voor uw app push mogelijkheid, Controleer of deze klopt de `capability` tabblad van het geselecteerde doel.

#### <a name="add-the-new-ios-10-notification-registration-code"></a>De nieuwe registratiecode voor iOS 10 melding toevoegen
De oudere codefragment registreren van de app kan meldingen werkt nog maar afgeschaft API's gebruikt bij het uitvoeren op iOS 10.

Importeer de `User Notification` framework:

        #import <UserNotifications/UserNotifications.h>

In uw toepassingsgemachtigde `application:didFinishLaunchingWithOptions` methode vervangen:

        if ([application respondsToSelector:@selector(registerUserNotificationSettings:)]) {
            [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert) categories:nil]];
            [application registerForRemoteNotifications];
        }
        else {

            [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
        }

door:

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

#### <a name="resolve-unusernotificationcenter-delegate-conflicts"></a>UNUserNotificationCenter gemachtigde conflicten oplossen

*Als uw toepassing noch een van de bibliotheken van de derde partij implementeert een `UNUserNotificationCenterDelegate` en vervolgens kunt u dit gedeelte overslaan.*

Een `UNUserNotificationCenter` gemachtigde wordt gebruikt door de SDK voor het bewaken van de levenscyclus van de Engagement-meldingen op apparaten waarop iOS 10 of hoger. De SDK heeft een eigen implementatie van de `UNUserNotificationCenterDelegate` protocol, maar er mag slechts één `UNUserNotificationCenter` delegeren per toepassing. Geen andere gedelegeerde toegevoegd aan de `UNUserNotificationCenter` object met de Engagement een conflict veroorzaken. Als de SDK de of alle andere leveranciers gemachtigde detecteert wordt deze niet zijn eigen implementatie gebruiken om een waarschuwingsbericht geeft u de conflicten op te lossen. U moet de Engagement-logica toevoegen aan uw eigen gemachtigde om de conflicten oplossen.

Er zijn twee manieren om dit te bereiken.

Voorstel 1, door de gemachtigde doorsturen aanroepen naar de SDK:

    #import <UIKit/UIKit.h>
    #import "EngagementAgent.h"
    #import <UserNotifications/UserNotifications.h>


    @interface MyAppDelegate : NSObject <UIApplicationDelegate, UNUserNotificationCenterDelegate>
    @end

    @implementation MyAppDelegate

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterWillPresentNotification:notification withCompletionHandler:completionHandler]
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterDidReceiveNotificationResponse:response withCompletionHandler:completionHandler]
    }
    @end

Of voorstel 2, door het overnemen van de `AEUserNotificationHandler` klasse

    #import "AEUserNotificationHandler.h"
    #import "EngagementAgent.h"

    @interface CustomUserNotificationHandler :AEUserNotificationHandler
    @end

    @implementation CustomUserNotificationHandler

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center willPresentNotification:notification withCompletionHandler:completionHandler];
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse: UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center didReceiveNotificationResponse:response withCompletionHandler:completionHandler];
    }

    @end

> [!NOTE]
> U kunt bepalen of een melding afkomstig van Engagement of niet door het doorgeven van is de `userInfo` woordenlijst met de Agent `isEngagementPushPayload:` klasse-methode.

Zorg ervoor dat de `UNUserNotificationCenter` gemachtigde van het object is ingesteld op uw gemachtigde binnen ofwel de `application:willFinishLaunchingWithOptions:` of de `application:didFinishLaunchingWithOptions:` methode van de toepassingsgemachtigde van uw.
Bijvoorbeeld, als u de bovenstaande voorstel 1 geïmplementeerd:

      - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
        // Any other code

        [UNUserNotificationCenter currentNotificationCenter].delegate = self;
        return YES;
      }
