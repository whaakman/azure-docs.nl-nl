---
title: Azure Notification Hubs beveiligde Push
description: Informatie over het veilig pushmeldingen verzendt naar een iOS-app van Azure. Codevoorbeelden geschreven in Objective-C en C#.
documentationcenter: ios
author: ysxu
manager: erikre
editor: 
services: notification-hubs
ms.assetid: 17d42b0a-2c80-4e35-a1ed-ed510d19f4b4
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: e5f09fb3716303bb21fe7442aa6fa8832174838e
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="azure-notification-hubs-secure-push"></a>Azure Notification Hubs beveiligde Push
> [!div class="op_single_selector"]
> * [Windows Universal](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
> * [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
> * [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)
> 
> 

## <a name="overview"></a>Overzicht
Push notification-ondersteuning in Microsoft Azure kunt u toegang tot een eenvoudig te gebruiken, meerdere platforms, uitgebreid pushinfrastructuur, die aanzienlijk de implementatie van pushmeldingen voor consumenten- en enterprise-toepassingen voor mobiele platforms vereenvoudigt.

Vanwege regelgeving of veiligheidsbeperkingen, soms een toepassing wilt iets opnemen in de melding die via de standaard push notification-infrastructuur kan niet worden verzonden. Deze zelfstudie wordt beschreven hoe dezelfde ervaring bereiken door te sturen van gevoelige gegevens via een veilige en geverifieerde verbinding tussen de client-apparaat en de back-end voor de app.

Op een hoog niveau is de stroom als volgt uit:

1. De back-end app:
   * Winkels beveiligde nettolading in back-enddatabase.
   * De ID van deze melding verzendt naar het apparaat (geen beveiligde gegevens verzonden).
2. De app op het apparaat tijdens het ontvangen van de melding:
   * Het apparaat neemt contact op met de back-end de nettolading van de beveiligde aanvragen.
   * De app kan de nettolading van de weergegeven als een melding op het apparaat.

Het is belangrijk te weten dat in de voorgaande stroom (en in deze zelfstudie), gaan we ervan uit dat het apparaat geen verificatietoken in de lokale opslag opslaat nadat de gebruiker zich aanmeldt. Dit garandeert een volledig naadloze ervaring als het apparaat de melding van de beveiligde nettolading met dit token kan ophalen. Als uw toepassing geen verificatietokens opslaat op het apparaat, of als deze tokens kunnen verlopen, moet een algemene melding dat de gebruiker wordt gevraagd om de app te starten door de app apparaat bij ontvangst van de melding worden weergegeven. De app vervolgens verifieert de gebruiker en toont de nettolading van de meldingen.

Deze Secure Push-zelfstudie laat zien hoe veilig een pushmelding verzenden. De zelfstudie bouwt voort op de [gebruikers waarschuwen](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) zelfstudie, dus u moet eerst de stappen in deze zelfstudie voltooien.

> [!NOTE]
> Deze zelfstudie wordt ervan uitgegaan dat u hebt gemaakt en uw notification hub geconfigureerd zoals beschreven in [aan de slag met Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md).
> 
> 

[!INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-ios-project"></a>Wijzigen van het iOS-project
Nu dat u uw back-end app verzenden gewijzigd alleen de *id* van een melding die u moet wijzigen van uw iOS-app voor het verwerken van deze kennisgeving en terugbellen van uw back-end voor het ophalen van het beveiligde bericht moet worden weergegeven.

Voor dit doel te bereiken, moeten we schrijven van de logica voor het ophalen van de beveiligde inhoud van de back-end van de app.

1. In **AppDelegate.m**, zorg ervoor dat de app worden geregistreerd voor de achtergrond meldingen zodat de id van de melding verzonden vanaf de back-end worden verwerkt. Voeg de **UIRemoteNotificationTypeNewsstandContentAvailability** optie in didFinishLaunchingWithOptions:
   
        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeNewsstandContentAvailability];
2. In uw **AppDelegate.m** een Implementatiesectie toevoegen aan de bovenkant met de volgende declaratie:
   
        @interface AppDelegate ()
        - (void) retrieveSecurePayloadWithId:(int)payloadId completion: (void(^)(NSString*, NSError*)) completion;
        @end
3. Voeg in de Implementatiesectie met de volgende code, waarbij u de tijdelijke aanduiding vervangt `{back-end endpoint}` met het eindpunt voor uw back-end eerder hebt verkregen:

```
        NSString *const GetNotificationEndpoint = @"{back-end endpoint}/api/notifications";

        - (void) retrieveSecurePayloadWithId:(int)payloadId completion: (void(^)(NSString*, NSError*)) completion;
        {
            // check if authenticated
            ANHViewController* rvc = (ANHViewController*) self.window.rootViewController;
            NSString* authenticationHeader = rvc.registerClient.authenticationHeader;
            if (!authenticationHeader) return;


            NSURLSession* session = [NSURLSession
                                     sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                                     delegate:nil
                                     delegateQueue:nil];


            NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/%d", GetNotificationEndpoint, payloadId]];
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
            [request setHTTPMethod:@"GET"];
            NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@", authenticationHeader];
            [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {
                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (!error && httpResponse.statusCode == 200)
                {
                    NSLog(@"Received secure payload: %@", [[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding]);

                    NSMutableDictionary *json = [NSJSONSerialization JSONObjectWithData:data options: NSJSONReadingMutableContainers error: &error];

                    completion([json objectForKey:@"Payload"], nil);
                }
                else
                {
                    NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                    if (error)
                        completion(nil, error);
                    else {
                        completion(nil, [NSError errorWithDomain:@"APICall" code:httpResponse.statusCode userInfo:nil]);
                    }
                }
            }];
            [dataTask resume];
        }
```

    This method calls your app back-end to retrieve the notification content using the credentials stored in the shared preferences.

1. Nu hebben we het binnenkomende bericht te verwerken en de bovenstaande methode gebruiken voor het ophalen van de inhoud om weer te geven. We hebben eerst inschakelen voor de iOS-app worden uitgevoerd op de achtergrond wanneer u een push-bericht ontvangen. In **XCode**, selecteert u uw app-project in het linkerdeelvenster en klik op het doel van uw belangrijkste app in de **doelen** sectie in het centrale deelvenster.
2. Klik vervolgens op uw **mogelijkheden** tabblad aan de bovenkant van de centrale deelvenster en controleer de **Remote Notifications** selectievakje.
   
    ![][IOS1]
3. In **AppDelegate.m** Voeg de volgende methode voor het afhandelen van pushmeldingen toe:
   
        -(void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult))completionHandler
        {
            NSLog(@"%@", userInfo);
   
            [self retrieveSecurePayloadWithId:[[userInfo objectForKey:@"secureId"] intValue] completion:^(NSString * payload, NSError *error) {
                if (!error) {
                    // show local notification
                    UILocalNotification* localNotification = [[UILocalNotification alloc] init];
                    localNotification.fireDate = [NSDate dateWithTimeIntervalSinceNow:0];
                    localNotification.alertBody = payload;
                    localNotification.timeZone = [NSTimeZone defaultTimeZone];
                    [[UIApplication sharedApplication] scheduleLocalNotification:localNotification];
   
                    completionHandler(UIBackgroundFetchResultNewData);
                } else {
                    completionHandler(UIBackgroundFetchResultFailed);
                }
            }];
   
        }
   
    Houd er rekening mee dat is het raadzaam om het geval van een eigenschap van de header ontbreekt verificatie of afwijzing verwerkt door de back-end. De specifieke verwerking van deze gevallen afhankelijk zijn van voornamelijk op de doel-gebruikerservaring. Een mogelijkheid is om een melding met een algemene prompt voor de gebruiker te verifiëren voor het ophalen van de werkelijke melding weer te geven.

## <a name="run-the-application"></a>De toepassing uitvoeren
Voor het uitvoeren van de toepassing, het volgende doen:

1. Voer de app op een fysiek iOS-apparaat (push notifications niet in de simulator werkt) in XCode.
2. Voer een gebruikersnaam en wachtwoord in de iOS-app gebruikersinterface. Deze kunnen zich een willekeurige tekenreeks, maar moeten dezelfde waarde.
3. Klik in de iOS-app UI **aanmelden**. Klik vervolgens op **push verzenden**. Hier ziet u de beveiligde melding wordt weergegeven in het meldingencentrum.

[IOS1]: ./media/notification-hubs-aspnet-backend-ios-secure-push/secure-push-ios-1.png
