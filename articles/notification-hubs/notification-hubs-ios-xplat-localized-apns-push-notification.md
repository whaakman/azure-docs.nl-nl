---
title: Notification Hubs gelokaliseerd op te splitsen nieuws-zelfstudie voor iOS
description: Informatie over het gebruik van Azure Service Bus Notification Hubs om gelokaliseerde belangrijk nieuws meldingen (iOS) te verzenden.
services: notification-hubs
documentationcenter: ios
author: ysxu
manager: erikre
editor: 
ms.assetid: 484914b5-e081-4a05-a84a-798bbd89d428
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: fd2b7d9dfd4f432bbcbaa3ed76f8bec0b9677e17
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="use-notification-hubs-to-send-localized-breaking-news-to-ios-devices"></a>Notification Hubs gebruiken gelokaliseerde belangrijk nieuws verzenden naar iOS-apparaten
> [!div class="op_single_selector"]
> * [Windows Store in C#](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
> * [iOS](notification-hubs-ios-xplat-localized-apns-push-notification.md)
> 
> 

## <a name="overview"></a>Overzicht
Dit onderwerp leest u hoe u de [sjablonen](notification-hubs-templates-cross-platform-push-messages.md) functie van Azure Notification Hubs voor belangrijk nieuws meldingen die door de taal en het apparaat zijn gelokaliseerd uitzending. In deze zelfstudie begint u met de iOS-app gemaakt in [Notification Hubs gebruiken om belangrijk nieuws te verzenden]. Als u klaar is, kunt u zich registreren voor u geïnteresseerd bent in categorieën, geeft u de taal waarin u de meldingen ontvangen en pushmeldingen voor de geselecteerde categorieën in die taal ontvangen.

Er zijn twee delen in dit scenario:

* iOS-app kan client apparaten een taal op te geven en zich abonneren op andere belangrijk nieuwscategorieën;
* de back-end zendt de meldingen, met behulp van de **tag** en **sjabloon** funcites van Azure Notification Hubs.

## <a name="prerequisites"></a>Vereisten
U moet al hebt voltooid de [Notification Hubs gebruiken om belangrijk nieuws te verzenden] zelfstudie en hebben de code die beschikbaar zijn, omdat deze zelfstudie is gebaseerd rechtstreeks op die code.

Visual Studio 2012 of later is optioneel.

## <a name="template-concepts"></a>Sjabloon-concepten
In [Notification Hubs gebruiken om belangrijk nieuws te verzenden] u een app die gebruikt gebouwd **labels** abonneren op meldingen voor verschillende nieuwscategorieën.
Veel apps echter meerdere markten zijn gericht en lokalisatie vereisen. Dit betekent dat de inhoud van de meldingen zelf hebt kunnen worden gelokaliseerd en verzonden naar de juiste set met apparaten.
In dit onderwerp wordt wordt getoond hoe u de **sjabloon** functie van Notification Hubs eenvoudig leveren gelokaliseerde belangrijk nieuws meldingen.

Opmerking: Er is een manier om gelokaliseerde meldingen te verzenden is om meerdere versies van elke tag te maken. Bijvoorbeeld ter ondersteuning van Engels, Frans en Mandarijn, zou moeten we drie verschillende tags voor wereldnieuws: 'world_en', 'world_fr' en 'world_ch'. Er moet een gelokaliseerde versie van de wereldnieuws verzenden naar elk van deze tags. In dit onderwerp gebruiken we sjablonen om te voorkomen dat de komst van tags en de vereiste van meerdere berichten verzenden.

Sjablonen zijn op een hoog niveau een manier om op te geven hoe een specifiek apparaat een melding moet ontvangen. De sjabloon geeft de indeling van de exacte nettolading door te verwijzen naar de eigenschappen die deel van het bericht is verzonden door back-end van uw app uitmaken. In ons geval ontvangt van ons een landinstelling networkdirect-bericht met alle ondersteunde talen:

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

Vervolgens we zorgt ervoor dat apparaten registreren met een sjabloon die naar de juiste eigenschap verwijst. Een iOS-app die wil registreren voor Franse nieuws registreren bijvoorbeeld het volgende:

    {
        aps:{
            alert: "$(News_French)"
        }
    }

Sjablonen zijn een zeer krachtige functie voor meer informatie over in onze [sjablonen](notification-hubs-templates-cross-platform-push-messages.md) artikel.

## <a name="the-app-user-interface"></a>De gebruikersinterface van de app
Er wordt nu de op te splitsen nieuws-app die u hebt gemaakt in het onderwerp wijzigen [Notification Hubs gebruiken om belangrijk nieuws te verzenden] gelokaliseerd belangrijk nieuws met behulp van sjablonen om te verzenden.

Voeg een gesegmenteerde besturingselement met de drie talen die wordt ondersteund in uw MainStoryboard_iPhone.storyboard: Engels, Frans en Mandarijn.

![][13]

Controleer of een IBOutlet toevoegen aan uw ViewController.h, zoals hieronder wordt weergegeven:

![][14]

## <a name="building-the-ios-app"></a>De iOS-app bouwen
1. Voeg in uw Notification.h de *retrieveLocale* methode, en wijzig de store en abonneren methoden, zoals hieronder wordt weergegeven:
   
        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet*) categories completion: (void (^)(NSError* error))completion;
   
        - (void) subscribeWithLocale:(int) locale categories:(NSSet*) categories completion:(void (^)(NSError *))completion;
   
        - (NSSet*) retrieveCategories;
   
        - (int) retrieveLocale;
   
    Wijzig in uw Notification.m de *storeCategoriesAndSubscribe* methode door de landinstellings-parameter toe te voegen en het opslaan in de standaardinstellingen voor gebruikers:
   
        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];
            [defaults setInteger:locale forKey:@"BreakingNewsLocale"];
            [defaults synchronize];
   
            [self subscribeWithLocale: locale categories:categories completion:completion];
        }
   
    Wijzig vervolgens de *abonneren* methode om te nemen van de landinstellingen:
   
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
   
    Houd er rekening mee hoe we zijn nu via de methode *registerTemplateWithDeviceToken*, in plaats van *registerNativeWithDeviceToken*. We hebben het json-sjabloon en ook een naam voor de sjabloon opgeven (zoals de app registreren verschillende sjablonen wilt mogelijk) wanneer er zich voor een sjabloon registreert. Zorg voor de categorieën registreren als labels, zoals we zeker weten willen voor het ontvangen van de notifciations voor deze nieuws.
   
    Een methode voor het ophalen van de landinstellingen van de standaardinstellingen van de gebruiker toevoegen:
   
        - (int) retrieveLocale {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
   
            int locale = [defaults integerForKey:@"BreakingNewsLocale"];
   
            return locale < 0?0:locale;
        }
2. Nu dat we onze klasse meldingen hebt gewijzigd, hebben we om ervoor te zorgen dat onze ViewController wordt gebruikgemaakt van de nieuwe UISegmentControl. Voeg de volgende regel in de *viewDidLoad* methode om ervoor te zorgen dat de landinstelling die momenteel is geselecteerd:
   
        self.Locale.selectedSegmentIndex = [notifications retrieveLocale];
   
    Klik op uw *abonneren* methode, wijzigt de aanroep van de *storeCategoriesAndSubscribe* voor de volgende:
   
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
3. Tot slot moet u bijwerken de *didRegisterForRemoteNotificationsWithDeviceToken* methode in uw AppDelegate.m, zodat u uw inschrijving correct vernieuwen kunt wanneer uw app wordt gestart. Wijzigen van de aanroep van de *abonneren* methode van meldingen door het volgende:
   
        NSSet* categories = [self.notifications retrieveCategories];
        int locale = [self.notifications retrieveLocale];
        [self.notifications subscribeWithLocale: locale categories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];

## <a name="optional-send-localized-template-notifications-from-net-console-app"></a>(optioneel) Gelokaliseerde Sjabloonmeldingen verzenden vanuit .NET-consoletoepassing.
[!INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]

## <a name="optional-send-localized-template-notifications-from-the-device"></a>(optioneel) Gelokaliseerde Sjabloonmeldingen verzenden vanuit het apparaat
Als u geen toegang tot Visual Studio hebben of wilt testen verzenden van de Sjabloonmeldingen gelokaliseerde rechtstreeks vanuit de app op het apparaat.  U kunt eenvoudig toevoegen de gelokaliseerde Sjabloonparameters naar de `SendNotificationRESTAPI` methode die u hebt gedefinieerd in de vorige zelfstudie.

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




## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het gebruik van sjablonen:

* [Waarschuw gebruikers met Notification Hubs: ASP.NET]
* [Waarschuw gebruikers met Notification Hubs: Mobile Services]

<!-- Images. -->

[13]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized1.png
[14]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized2.png






<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
[Notification Hubs gebruiken om belangrijk nieuws te verzenden]: /manage/services/notification-hubs/breaking-news-ios
[Mobile Service]: /develop/mobile/tutorials/get-started
[Waarschuw gebruikers met Notification Hubs: ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Waarschuw gebruikers met Notification Hubs: Mobile Services]: /manage/services/notification-hubs/notify-users
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
