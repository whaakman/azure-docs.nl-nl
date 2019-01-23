---
title: Azure Notification Hubs Secure Push
description: Leer hoe u veilige pushmeldingen verzenden naar een iOS-app van Azure. Codevoorbeelden geschreven in Objective-C en C#.
documentationcenter: ios
author: jwargo
manager: patniko
editor: spelluru
services: notification-hubs
ms.assetid: 17d42b0a-2c80-4e35-a1ed-ed510d19f4b4
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: d88bdb1eaeb95413df84bf69ed4fc763b6d4901f
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54449264"
---
# <a name="azure-notification-hubs-secure-push"></a>Azure Notification Hubs Secure Push

> [!div class="op_single_selector"]
> * [Windows Universal](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
> * [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
> * [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)

## <a name="overview"></a>Overzicht

Push notification-ondersteuning in Microsoft Azure kunt u toegang tot een eenvoudig te gebruiken, meerdere platforms, uitgebreide pushinfrastructuur, die de implementatie van pushmeldingen voor consumenten- en enterprise-toepassingen voor mobiele apparaten vereenvoudigt platforms.

Vanwege regelgeving of veiligheidsbeperkingen, soms een toepassing wilt iets opnemen in de melding dat kan niet worden verzonden via de infrastructuur voor pushmeldingen voor standard. In deze zelfstudie wordt beschreven hoe u dezelfde ervaring bereiken door het verzenden van gevoelige gegevens via een veilige, geverifieerde verbinding tussen het clientapparaat en de back-end.

Op hoog niveau is de stroom als volgt uit:

1. De app-back-end:
   * Winkels beveiligde nettolading in back-end-database.
   * De ID van deze melding verzonden naar het apparaat (geen beveiligde gegevens wordt verzonden).
2. De app op het apparaat bij de ontvangst van de melding:
   * Het apparaat neemt contact op met de back-end de nettolading van de beveiligde aanvragen.
   * De app kan de nettolading van de weergegeven als een melding op het apparaat.

Het is belangrijk te weten dat in de vorige stroom (en in deze zelfstudie), gaan we ervan uit dat het apparaat een verificatietoken in de lokale opslag opslaat nadat de gebruiker zich aanmeldt. Dit garandeert een naadloze ervaring als het apparaat van de melding van de beveiligde nettolading met behulp van dit token kunt ophalen. Als uw toepassing geen verificatietokens op het apparaat slaat, of als deze tokens kunnen worden verlopen, een algemene melding waarin de gebruiker wordt gevraagd om de app te starten moet worden weergegeven aan de apparaat-app, nadat de melding is ontvangen. De app vervolgens verifieert de gebruiker en de nettolading van de melding ziet.

Deze beveiligde Push-zelfstudie leert hoe u een push-bericht veilig verzendt. De zelfstudie bouwt voort op de [gebruikers waarschuwen](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) zelfstudie, dus u moet eerst de stappen in deze zelfstudie voltooien.

> [!NOTE]
> Deze zelfstudie wordt ervan uitgegaan dat u hebt gemaakt en uw notification hub geconfigureerd zoals beschreven in [aan de slag met Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md).

[!INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-ios-project"></a>Wijzigen van het iOS-project

Nu dat u hebt gewijzigd dat uw app-back-end voor het verzenden van alleen de *ID* van een melding die u moet uw iOS-app voor het verwerken van waarmee de melding en uw back-end om op te halen van het beveiligde bericht moet worden weergegeven voor de terugbelfunctie wijzigen.

Voor dit doel te bereiken, hebben we schrijven de logica voor het ophalen van de beveiligde inhoud vanuit de app-back-end.

1. In `AppDelegate.m`, zorg ervoor dat de app wordt geregistreerd voor op de achtergrond meldingen, zodat de meldings-ID worden verwerkt die zijn verzonden vanaf de back-end. Voeg de `UIRemoteNotificationTypeNewsstandContentAvailability` optie in didFinishLaunchingWithOptions:

    ```objc
    [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeNewsstandContentAvailability];
    ```
2. In uw `AppDelegate.m` toevoegen van een Implementatiesectie bovenaan, bij de declaratie van het volgende:

    ```objc
    @interface AppDelegate ()
    - (void) retrieveSecurePayloadWithId:(int)payloadId completion: (void(^)(NSString*, NSError*)) completion;
    @end
    ```
3. Voeg in de Implementatiesectie met de volgende code, waarbij u de tijdelijke aanduiding vervangt door `{back-end endpoint}` met het eindpunt voor uw back-end eerder hebt opgehaald:

    ```objc
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

    Deze methode roept uw app-back-end om op te halen van de inhoud van meldingen met behulp van de referenties die zijn opgeslagen in de gedeelde voorkeuren.

4. Nu hebben we het binnenkomende bericht te verwerken en gebruik de bovenstaande methode om het ophalen van de inhoud om weer te geven. We hebben eerst inschakelen voor de iOS-app om uit te voeren op de achtergrond wanneer er een push-bericht ontvangen. In **XCode**, selecteert u uw app-project in het linkerdeelvenster en klik op het doel van uw belangrijkste app in de **doelen** sectie in het centrale deelvenster.
5. Klik vervolgens op uw **mogelijkheden** tabblad aan de bovenkant van uw centrale deelvenster en controleer de **Remote Notifications** selectievakje.

    ![][IOS1]

6. In `AppDelegate.m` voegt u de volgende methode voor het afhandelen van pushmeldingen te verzenden:

    ```objc
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
    ```

    Houd er rekening mee dat het verdient het geval van een eigenschap van de koptekst ontbreekt verificatie- of weigerings-verwerken van de back-end. De specifieke verwerking van dergelijke gevallen is afhankelijk van voornamelijk op uw doel-gebruikerservaring. Een optie is om een melding met een algemene prompt voor de gebruiker om te verifiëren om op te halen van de werkelijke melding weer te geven.

## <a name="run-the-application"></a>De toepassing wordt uitgevoerd

Voor het uitvoeren van de toepassing, het volgende doen:

1. In XCode, moet u de app uitvoeren op een fysiek iOS-apparaat (push-meldingen niet in de simulator werken).
2. Voer een gebruikersnaam en wachtwoord in de iOS-app UI. Dit kunnen een willekeurige tekenreeks zijn, maar ze moeten dezelfde waarde.
3. Klik in de iOS-app UI op **aanmelden**. Klik vervolgens op **pushmelding verzonden**. Hier ziet u de beveiligde melding wordt weergegeven in het meldingencentrum.

[IOS1]: ./media/notification-hubs-aspnet-backend-ios-secure-push/secure-push-ios-1.png
