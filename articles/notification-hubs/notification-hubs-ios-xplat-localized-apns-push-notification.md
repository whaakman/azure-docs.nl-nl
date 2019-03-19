---
title: Gelokaliseerde pushmeldingen naar iOS-apparaten met Azure Notification Hubs | Microsoft Docs
description: Informatie over het gebruik van gelokaliseerde pushmeldingen naar iOS-apparaten met behulp van Azure Notification Hubs.
services: notification-hubs
documentationcenter: ios
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: 484914b5-e081-4a05-a84a-798bbd89d428
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: 527e9979b624970dd55b4300fe63c27386640ac4
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57886599"
---
# <a name="tutorial-push-localized-notifications-to-ios-devices-using-azure-notification-hubs"></a>Zelfstudie: Gelokaliseerde pushmeldingen naar iOS-apparaten met Azure Notification Hubs

> [!div class="op_single_selector"]
> * [Windows Store C#](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
> * [iOS](notification-hubs-ios-xplat-localized-apns-push-notification.md)

Deze zelfstudie leert u hoe u de [sjablonen](notification-hubs-templates-cross-platform-push-messages.md) functie van Azure Notification Hubs om belangrijke nieuwsfeiten gelokaliseerde taal en het apparaat. In deze zelfstudie begint u met de iOS-app gemaakt in [Notification Hubs gebruiken om belangrijk nieuws te verzenden]. Als u klaar bent, kunt u zich registreren voor categorieën waarin u geïnteresseerd bent, Geef een taal waarin u om de meldingen te ontvangen en alleen pushmeldingen ontvangen voor de geselecteerde categorieën in die taal.

Er zijn twee onderdelen voor dit scenario:

* iOS-app kan de client-apparaten om op te geven van een taal en om u te abonneren op verschillende belangrijke nieuwscategorieën;
* De back-end zendt de meldingen, met behulp van de **tag** en **sjabloon** functies van Azure Notification Hubs.

In deze zelfstudie voert u de volgende stappen uit:

> [!div class="checklist"]
> * De gebruikersinterface van de app bijwerken
> * De iOS-app bouwen
> * Voor gelokaliseerde Sjabloonmeldingen verzenden vanuit .NET-consoletoepassing
> * Voor gelokaliseerde Sjabloonmeldingen verzenden vanuit het apparaat

## <a name="overview"></a>Overzicht

In [Notification Hubs gebruiken om belangrijk nieuws te verzenden], u een app die gebruikt gebouwd **tags** om u te abonneren op meldingen voor verschillende nieuwscategorieën. Veel apps, gericht op meerdere markten, en lokalisatie vereist. Dit betekent dat de inhoud van de meldingen zelf moeten worden gelokaliseerd en die worden geleverd aan de juiste set met apparaten. Deze zelfstudie leert u hoe u de **sjabloon** functie van Notification Hubs eenvoudig leveren gelokaliseerde laatste nieuws te verzenden.

> [!NOTE]
> Eén manier om gelokaliseerde meldingen te verzenden is het maken van meerdere versies van elk label. Bijvoorbeeld, ter ondersteuning van Engels, Frans en Mandarijn, moet u drie verschillende codes voor wereldnieuws dat is: "world_en", "world_fr" en 'world_ch'. Vervolgens moet u een gelokaliseerde versie van de wereldnieuws dat is verzonden naar elk van deze tags. In dit onderwerp, kunt u sjablonen gebruiken om te voorkomen dat de verspreiding van tags en de vereiste om meerdere berichten te verzenden.

Sjablonen zijn op een hoog niveau een manier om op te geven hoe een specifiek apparaat ontvangt een melding. De sjabloon bepaalt de exacte indeling van de payload door te verwijzen naar eigenschappen die deel uitmaken van het bericht dat is verzonden door uw back-endapp. In het geval is verzendt u een landinstelling bericht met alle ondersteunde talen:

```json
{
    "News_English": "...",
    "News_French": "...",
    "News_Mandarin": "..."
}
```

Vervolgens zorgt u ervoor dat apparaten registreren met een sjabloon die naar de juiste eigenschap verwijst. Bijvoorbeeld, een iOS-app die wil registreren voor de Franse nieuws registreert met behulp van de volgende syntaxis:

```json
{
    aps: {
        alert: "$(News_French)"
    }
}
```

Zie voor meer informatie over sjablonen [sjablonen](notification-hubs-templates-cross-platform-push-messages.md) artikel.

## <a name="prerequisites"></a>Vereisten

* Voltooi de [Pushmeldingen verzenden naar specifieke iOS-apparaten](notification-hubs-ios-xplat-segmented-apns-push-notification.md) zelfstudie en hebben de code die beschikbaar is, omdat deze zelfstudie is gebaseerd rechtstreeks op die code.
* Visual Studio 2017 is optioneel.

## <a name="update-the-app-user-interface"></a>De gebruikersinterface van de app bijwerken

In deze sectie maakt u het belangrijke nieuws-app die u hebt gemaakt in het onderwerp wijzigen [Notification Hubs gebruiken om belangrijk nieuws te verzenden] gelokaliseerd met behulp van sjablonen het laatste nieuws te verzenden.

In uw `MainStoryboard_iPhone.storyboard`, Voeg een gesegmenteerde besturingselement met de drie talen: Engels, Frans en Mandarijn.

![Het maken van het iOS-UI-storyboard][13]

Controleer of een IBOutlet in uw ViewController.h toevoegen, zoals wordt weergegeven in de volgende afbeelding:

![Verkooppunten voor de switches maken][14]

## <a name="build-the-ios-app"></a>De iOS-app bouwen

1. In uw `Notification.h` toevoegen de `retrieveLocale` methode, en wijzig de store en abonneren methoden, zoals wordt weergegeven in de volgende code:

    ```objc
    - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet*) categories completion: (void (^)(NSError* error))completion;

    - (void) subscribeWithLocale:(int) locale categories:(NSSet*) categories completion:(void (^)(NSError *))completion;

    - (NSSet*) retrieveCategories;

    - (int) retrieveLocale;
    ```
    In uw `Notification.m`, wijzigen de `storeCategoriesAndSubscribe` methode door toe te voegen de `locale` parameter en op te slaan in de standaardinstellingen voor gebruiker:

    ```objc
    - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion {
        NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
        [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];
        [defaults setInteger:locale forKey:@"BreakingNewsLocale"];
        [defaults synchronize];

        [self subscribeWithLocale: locale categories:categories completion:completion];
    }
    ```

    Wijzig vervolgens de *abonneren* methode om op te nemen van de landinstellingen:

    ```objc
    - (void) subscribeWithLocale: (int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion{
        SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:@"<connection string>" notificationHubPath:@"<hub name>"];

        NSString* localeString;
        switch (locale) {
            case 0:
                localeString = @"English";
                break;
            case 1:
                localeString = @"French";
                break;
            case 2:
                localeString = @"Mandarin";
                break;
        }

        NSString* template = [NSString stringWithFormat:@"{\"aps\":{\"alert\":\"$(News_%@)\"},\"inAppMessage\":\"$(News_%@)\"}", localeString, localeString];

        [hub registerTemplateWithDeviceToken:self.deviceToken name:@"localizednewsTemplate" jsonBodyTemplate:template expiryTemplate:@"0" tags:categories completion:completion];
    }
    ```

    Gebruik van de methode `registerTemplateWithDeviceToken`, in plaats van `registerNativeWithDeviceToken`. Wanneer u zich voor een sjabloon registreren, hebt u de json-sjabloon en ook een naam op voor de sjabloon opgeven (zoals de app verschillende sjablonen registreren wilt mogelijk). Zorg ervoor dat u het registreren van uw categorieën als labels, als u wilt ervoor zorgen dat de meldingen voor deze nieuws.

    Voeg de methode om op te halen van de landinstellingen van de standaardinstellingen voor gebruiker:

    ```objc
    - (int) retrieveLocale {
        NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];

        int locale = [defaults integerForKey:@"BreakingNewsLocale"];

        return locale < 0?0:locale;
    }
    ```

2. Nu dat u hebt gewijzigd de `Notifications` klasse, die u hebt om ervoor te zorgen dat de `ViewController` maakt gebruik van de nieuwe `UISegmentControl`. Voeg de volgende regel in de `viewDidLoad` methode om ervoor te zorgen om weer te geven van de landinstelling die momenteel is geselecteerd:

    ```objc
    self.Locale.selectedSegmentIndex = [notifications retrieveLocale];
    ```

    Klik op uw `subscribe` methode wijzigen van de aanroep van de `storeCategoriesAndSubscribe` naar de volgende code:

    ```objc
    [notifications storeCategoriesAndSubscribeWithLocale: self.Locale.selectedSegmentIndex categories:[NSSet setWithArray:categories] completion: ^(NSError* error) {
        if (!error) {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
                                    @"Subscribed!" delegate:nil cancelButtonTitle:
                                    @"OK" otherButtonTitles:nil, nil];
            [alert show];
        } else {
            NSLog(@"Error subscribing: %@", error);
        }
    }];
    ```

3. Tot slot moet u bijwerken de `didRegisterForRemoteNotificationsWithDeviceToken` methode in uw AppDelegate.m, zodat u uw registratie correct vernieuwen kunt wanneer uw app wordt gestart. Wijzigen van de aanroep van de `subscribe` methode van meldingen door de volgende code:

    ```obj-c
    NSSet* categories = [self.notifications retrieveCategories];
    int locale = [self.notifications retrieveLocale];
    [self.notifications subscribeWithLocale: locale categories:categories completion:^(NSError* error) {
        if (error != nil) {
            NSLog(@"Error registering for notifications: %@", error);
        }
    }];
    ```

## <a name="optional-send-localized-template-notifications-from-net-console-app"></a>(optioneel) Voor gelokaliseerde Sjabloonmeldingen verzenden vanuit .NET-consoletoepassing

[!INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]

## <a name="optional-send-localized-template-notifications-from-the-device"></a>(optioneel) Voor gelokaliseerde Sjabloonmeldingen verzenden vanuit het apparaat

Als u geen toegang tot Visual Studio hebt, of alleen wilt testen van de voor gelokaliseerde Sjabloonmeldingen verzenden rechtstreeks vanuit de app op het apparaat. U kunt de gelokaliseerde Sjabloonparameters aan toevoegen de `SendNotificationRESTAPI` methode die u hebt gedefinieerd in de vorige zelfstudie.

```objc
- (void)SendNotificationRESTAPI:(NSString*)categoryTag
{
    NSURLSession* session = [NSURLSession sessionWithConfiguration:[NSURLSessionConfiguration
                                defaultSessionConfiguration] delegate:nil delegateQueue:nil];

    NSString *json;

    // Construct the messages REST endpoint
    NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                        HUBNAME, API_VERSION]];

    // Generated the token to be used in the authorization header.
    NSString* authorizationToken = [self generateSasToken:[url absoluteString]];

    //Create the request to add the template notification message to the hub
    NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
    [request setHTTPMethod:@"POST"];

    // Add the category as a tag
    [request setValue:categoryTag forHTTPHeaderField:@"ServiceBusNotification-Tags"];

    // Template notification
    json = [NSString stringWithFormat:@"{\"messageParam\":\"Breaking %@ News : %@\","
            \"News_English\":\"Breaking %@ News in English : %@\","
            \"News_French\":\"Breaking %@ News in French : %@\","
            \"News_Mandarin\":\"Breaking %@ News in Mandarin : %@\","
            categoryTag, self.notificationMessage.text,
            categoryTag, self.notificationMessage.text,  // insert English localized news here
            categoryTag, self.notificationMessage.text,  // insert French localized news here
            categoryTag, self.notificationMessage.text]; // insert Mandarin localized news here

    // Signify template notification format
    [request setValue:@"template" forHTTPHeaderField:@"ServiceBusNotification-Format"];

    // JSON Content-Type
    [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];

    //Authenticate the notification message POST request with the SaS token
    [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];

    //Add the notification message body
    [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];

    // Send the REST request
    NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
                completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
        {
        NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
            if (error || httpResponse.statusCode != 200)
            {
                NSLog(@"\nError status: %d\nError: %@", httpResponse.statusCode, error);
            }
            if (data != NULL)
            {
                //xmlParser = [[NSXMLParser alloc] initWithData:data];
                //[xmlParser setDelegate:self];
                //[xmlParser parse];
            }
        }];

    [dataTask resume];
}
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie, kunt u gelokaliseerde meldingen verzonden naar iOS-apparaten. Als u wilt weten hoe u pushmeldingen verzenden naar specifieke gebruikers van iOS-apps, Ga naar de volgende zelfstudie:

> [!div class="nextstepaction"]
>[Pushmeldingen verzenden naar specifieke gebruikers](notification-hubs-aspnet-backend-ios-apple-apns-notification.md)

<!-- Images. -->
[13]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized1.png
[14]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized2.png

<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: https://msdn.microsoft.com/library/jj927168.aspx
[Notification Hubs gebruiken om belangrijk nieuws te verzenden]: notification-hubs-ios-xplat-segmented-apns-push-notification.md
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs: ASP.NET]: notification-hubs-aspnet-backend-ios-apple-apns-notification.md
[Notify users with Notification Hubs: Mobile Services]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Submit an app page]: https://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: https://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: https://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-ios
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-ios
[Push notifications to app users]: /develop/mobile/tutorials/push-notifications-to-users-ios
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[JavaScript and HTML]: ../get-started-with-push-js.md
[Windows Developer Preview registration steps for Mobile Services]: ../mobile-services-windows-developer-preview-registration.md
[wns object]: https://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: https://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for iOS]: https://msdn.microsoft.com/library/jj927168.aspx
