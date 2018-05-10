---
title: Gelokaliseerde pushmeldingen op iOS-apparaten met behulp van Azure Notification Hubs | Microsoft Docs
description: Informatie over het gebruik van gelokaliseerde pushmeldingen op iOS-apparaten met behulp van Azure Notification Hubs.
services: notification-hubs
documentationcenter: ios
author: dimazaid
manager: kpiteira
editor: spelluru
ms.assetid: 484914b5-e081-4a05-a84a-798bbd89d428
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 04/14/2018
ms.author: dimazaid
ms.openlocfilehash: b3d74086ee233da50138aff00d8da78aa0243a75
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="tutorial-push-localized-notifications-to-ios-devices-using-azure-notification-hubs"></a>Zelfstudie: Pushmeldingen gelokaliseerde op iOS-apparaten met behulp van Azure Notification Hubs 
> [!div class="op_single_selector"]
> * [Windows Store in C#](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
> * [iOS](notification-hubs-ios-xplat-localized-apns-push-notification.md)
> 

Deze zelfstudie leert u hoe u de [sjablonen](notification-hubs-templates-cross-platform-push-messages.md) functie van Azure Notification Hubs voor belangrijk nieuws meldingen die door de taal en het apparaat zijn gelokaliseerd uitzending. In deze zelfstudie begint u met de iOS-app gemaakt in [Notification Hubs gebruiken om belangrijk nieuws te verzenden]. Als u klaar kunt u zich registreren voor categorieën die u geïnteresseerd bent in, geef de taal waarin u de meldingen wilt ontvangen en pushmeldingen voor de geselecteerde categorieën in die taal ontvangen.

Er zijn twee delen in dit scenario:

* iOS-app kan client apparaten een taal op te geven en zich abonneren op andere belangrijk nieuwscategorieën;
* De back-end zendt de meldingen, met behulp van de **tag** en **sjabloon** functies van Azure Notification Hubs.

In deze zelfstudie maakt uitvoeren u de volgende stappen:

> [!div class="checklist"]
> * De gebruikersinterface van de app bijwerken
> * De iOS-app bouwen
> * Gelokaliseerde Sjabloonmeldingen verzenden vanuit .NET-consoletoepassing
> * Gelokaliseerde Sjabloonmeldingen verzenden vanuit het apparaat


## <a name="overview"></a>Overzicht
In [Notification Hubs gebruiken om belangrijk nieuws te verzenden], u een app die gebruikt gebouwd **labels** abonneren op meldingen voor verschillende nieuwscategorieën. Veel apps echter meerdere markten zijn gericht en lokalisatie vereisen. Dit betekent dat de inhoud van de meldingen zelf hebt kunnen worden gelokaliseerd en verzonden naar de juiste set met apparaten. Deze zelfstudie leert u hoe u de **sjabloon** functie van Notification Hubs eenvoudig leveren gelokaliseerde belangrijk nieuws meldingen.

> [!NOTE]
> Een manier om gelokaliseerde meldingen te verzenden, is om meerdere versies van elke tag te maken. Bijvoorbeeld, ter ondersteuning van Engels, Frans en Mandarijn, moet u drie verschillende tags voor wereldnieuws: 'world_en', 'world_fr' en 'world_ch'. Vervolgens moet u een gelokaliseerde versie van de wereldnieuws verzenden naar elk van deze tags. In dit onderwerp kunt u sjablonen gebruiken om te voorkomen dat de komst van tags en de vereiste van meerdere berichten verzenden.

Sjablonen zijn op een hoog niveau een manier om op te geven hoe een specifiek apparaat een melding moet ontvangen. De sjabloon geeft de indeling van de exacte nettolading door te verwijzen naar de eigenschappen die deel van het bericht is verzonden door back-end van uw app uitmaken. In uw geval verzendt u een landinstelling networkdirect-bericht met alle ondersteunde talen:

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

Vervolgens zorgt u ervoor dat apparaten registreren met een sjabloon die naar de juiste eigenschap verwijst. Bijvoorbeeld een iOS-app die wil registreren voor Franse nieuws registreert met de volgende syntaxis:

    {
        aps:{
            alert: "$(News_French)"
        }
    }

Zie voor meer informatie over sjablonen [sjablonen](notification-hubs-templates-cross-platform-push-messages.md) artikel.

## <a name="prerequisites"></a>Vereisten

- Voltooi de [Pushmeldingen op specifieke iOS-apparaten](notification-hubs-ios-xplat-segmented-apns-push-notification.md) zelfstudie en hebben de code die beschikbaar zijn, omdat deze zelfstudie is gebaseerd rechtstreeks op die code.
- Visual Studio 2012 of later is optioneel.

## <a name="update-the-app-user-interface"></a>De gebruikersinterface van de app bijwerken
In deze sectie maakt u de app op te splitsen nieuws die u hebt gemaakt in het onderwerp wijzigen [Notification Hubs gebruiken om belangrijk nieuws te verzenden] gelokaliseerd belangrijk nieuws met behulp van sjablonen om te verzenden.

Voeg in uw MainStoryboard_iPhone.storyboard een gesegmenteerde besturingselement met de drie talen: Engels, Frans en Mandarijn.

![][13]

Controleer of een IBOutlet toevoegen aan uw ViewController.h, zoals wordt weergegeven in de volgende afbeelding:

![][14]

## <a name="build-the-ios-app"></a>De iOS-app bouwen
1. Voeg in uw Notification.h de *retrieveLocale* methode, en wijzig de store en abonneren methoden, zoals wordt weergegeven in de volgende code:
   
    ```obj-c
        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet*) categories completion: (void (^)(NSError* error))completion;
   
        - (void) subscribeWithLocale:(int) locale categories:(NSSet*) categories completion:(void (^)(NSError *))completion;
   
        - (NSSet*) retrieveCategories;
   
        - (int) retrieveLocale;
   
    ```
    Wijzig in uw Notification.m de *storeCategoriesAndSubscribe* methode door de landinstellings-parameter toe te voegen en het opslaan in de standaardinstellingen voor gebruikers:
   
    ```obj-c
        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];
            [defaults setInteger:locale forKey:@"BreakingNewsLocale"];
            [defaults synchronize];
   
            [self subscribeWithLocale: locale categories:categories completion:completion];
        }
    ````
    Wijzig vervolgens de *abonneren* methode om te nemen van de landinstellingen:
   
    ```obj-c
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
    You use the method *registerTemplateWithDeviceToken*, instead of *registerNativeWithDeviceToken*. When you register for a template, you have to provide the json template and also a name for the template (as the app might want to register different templates). Make sure to register your categories as tags, as you want to make sure to receive the notifciations for those news.
   
    Add a method to retrieve the locale from the user default settings:
   
    ```obj-c
        - (int) retrieveLocale {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
   
            int locale = [defaults integerForKey:@"BreakingNewsLocale"];
   
            return locale < 0?0:locale;
        }
    ```
2. Nu dat u de klasse meldingen hebt gewijzigd, wordt u om ervoor te zorgen dat de ViewController wordt gebruikgemaakt van de nieuwe UISegmentControl hebt. Voeg de volgende regel in de *viewDidLoad* methode om ervoor te zorgen dat de landinstelling die momenteel is geselecteerd:
   
    ```obj-c
        self.Locale.selectedSegmentIndex = [notifications retrieveLocale];
     ```  
    Klik op uw *abonneren* methode, wijzigt de aanroep van de *storeCategoriesAndSubscribe* naar de volgende code:
   
    ```obj-c
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
3. Tot slot moet u bijwerken de *didRegisterForRemoteNotificationsWithDeviceToken* methode in uw AppDelegate.m, zodat u uw inschrijving correct vernieuwen kunt wanneer uw app wordt gestart. Wijzigen van de aanroep van de *abonneren* methode van meldingen door de volgende code:
   
    ```obj-c
        NSSet* categories = [self.notifications retrieveCategories];
        int locale = [self.notifications retrieveLocale];
        [self.notifications subscribeWithLocale: locale categories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];
    ```

## <a name="optional-send-localized-template-notifications-from-net-console-app"></a>(optioneel) Gelokaliseerde Sjabloonmeldingen verzenden vanuit .NET-consoletoepassing
[!INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]

## <a name="optional-send-localized-template-notifications-from-the-device"></a>(optioneel) Gelokaliseerde Sjabloonmeldingen verzenden vanuit het apparaat
Als u geen toegang tot Visual Studio hebben of wilt testen verzenden van de Sjabloonmeldingen gelokaliseerde rechtstreeks vanuit de app op het apparaat. U kunt de gelokaliseerde Sjabloonparameters naar toevoegen de `SendNotificationRESTAPI` methode die u hebt gedefinieerd in de vorige zelfstudie.

    ```obj-c
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
In deze zelfstudie maakt verzonden u gelokaliseerde meldingen naar iOS-apparaten. Als u wilt weten hoe u pushmeldingen aan specifieke gebruikers van iOS-apps, gaat u door naar de volgende zelfstudie: 

> [!div class="nextstepaction"]
>[Pushmeldingen verzenden naar specifieke gebruikers](notification-hubs-aspnet-backend-ios-apple-apns-notification.md)

<!-- Images. -->

[13]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized1.png
[14]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized2.png






<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
[Notification Hubs gebruiken om belangrijk nieuws te verzenden]: /manage/services/notification-hubs/breaking-news-ios
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs: ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Notify users with Notification Hubs: Mobile Services]: /manage/services/notification-hubs/notify-users
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-ios
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-ios
[Push notifications to app users]: /develop/mobile/tutorials/push-notifications-to-users-ios
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[JavaScript and HTML]: ../get-started-with-push-js.md

[Windows Developer Preview registration steps for Mobile Services]: ../mobile-services-windows-developer-preview-registration.md
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
