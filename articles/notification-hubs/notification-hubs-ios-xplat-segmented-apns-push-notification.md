---
title: Pushmeldingen verzenden naar specifieke iOS-apparaten met Azure Notification Hubs | Microsoft Docs
description: In deze zelfstudie leert u hoe u Azure Notification Hubs gebruiken om pushmeldingen te verzenden naar specifieke iOS-apparaten.
services: notification-hubs
documentationcenter: ios
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: 6ead4169-deff-4947-858c-8c6cf03cc3b2
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: dd625dba0e125ccf993af524a0ab0c0cc66555fb
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57834214"
---
# <a name="tutorial-push-notifications-to-specific-ios-devices-using-azure-notification-hubs"></a>Zelfstudie: Pushmeldingen verzenden naar specifieke iOS-apparaten met Azure Notification Hubs

[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a>Overzicht

Deze zelfstudie leert u hoe u Azure Notification Hubs gebruiken om te zenden van belangrijke nieuwsfeiten naar een iOS-app. Als u klaar bent, u kunt registreren voor het belangrijke nieuwscategorieën waarin u geïnteresseerd bent, en ontvangen van pushmeldingen voor deze categorieën. Dit scenario is een algemeen patroon voor veel apps die meldingen moeten verzenden naar groepen gebruikers die eerder hebben aangegeven in bepaalde onderwerpen geïnteresseerd te zijn, zoals een RSS-lezer, apps voor muziekfans, enzovoort.

Broadcast-scenario's zijn mogelijk door een of meer *tags* (of labels) toe te voegen wanneer u een registratie maakt in Notifications Hub. Wanneer meldingen worden verzonden naar een label, ontvangen apparaten die zijn geregistreerd voor het label de melding. Omdat tags niet meer dan tekenreeksen zijn, hoeven ze niet vooraf te worden opgesteld. Zie [Notification Hubs-routering en tagexpressies](notification-hubs-tags-segment-push-message.md) voor meer informatie over tags.

In deze zelfstudie voert u de volgende stappen uit:

> [!div class="checklist"]
> * Een categorieselectie toevoegen aan de app
> * Getagde meldingen verzenden
> * Meldingen verzenden vanuit het apparaat
> * De app uitvoeren en meldingen genereren

## <a name="prerequisites"></a>Vereisten

In dit onderwerp bouwt voort op de app die u hebt gemaakt in [zelfstudie: Pushmeldingen naar iOS-apps met behulp van Azure Notification Hubs][get-started]. Voordat u deze zelfstudie begint, u moet al hebt voltooid [zelfstudie: Pushmeldingen naar iOS-apps met behulp van Azure Notification Hubs][get-started].

## <a name="add-category-selection-to-the-app"></a>Categorieselectie toevoegen aan de app

De eerste stap is de UI-elementen toevoegen aan uw bestaande storyboard waarmee de gebruiker selecteert categorieën om u te registreren. De geselecteerde categorieën worden op het apparaat opgeslagen. Wanneer de app wordt gestart, wordt er een apparaatregistratie gemaakt in uw meldingshub, met de geselecteerde categorieën als tags.

1. In uw **MainStoryboard_iPhone.storyboard** Voeg de volgende onderdelen van de objectbibliotheek:

   * Een label met de tekst "Belangrijke nieuws",
   * Labels met Categorieteksten "Wereld", 'Politiek', 'Zakelijke', "-technologie", "Wetenschap", 'Sport'
   * Zes switches, één per categorie, stelt elke switch **status** moet **uit** standaard.
   * Een knop met het label "Subscribe"

     Uw storyboard moet er als volgt uitzien:

     ![Xcode-interface builder][3]

2. In de editor assistent verkooppunten voor alle schakelopties maken en het aanroepen van "WorldSwitch", "PoliticsSwitch", "BusinessSwitch", "TechnologySwitch", "ScienceSwitch", "SportsSwitch"
3. Maak een actie voor de knop met de naam `subscribe`; uw `ViewController.h` mag de volgende code:

    ```objc
    @property (weak, nonatomic) IBOutlet UISwitch *WorldSwitch;
    @property (weak, nonatomic) IBOutlet UISwitch *PoliticsSwitch;
    @property (weak, nonatomic) IBOutlet UISwitch *BusinessSwitch;
    @property (weak, nonatomic) IBOutlet UISwitch *TechnologySwitch;
    @property (weak, nonatomic) IBOutlet UISwitch *ScienceSwitch;
    @property (weak, nonatomic) IBOutlet UISwitch *SportsSwitch;

    - (IBAction)subscribe:(id)sender;
    ```

4. Maak een nieuwe **Cocoa Touch klasse** met de naam `Notifications`. Kopieer de volgende code in de interface-sectie van het bestand Notifications.h:

    ```objc
    @property NSData* deviceToken;

    - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName;

    - (void)storeCategoriesAndSubscribeWithCategories:(NSArray*)categories
                completion:(void (^)(NSError* error))completion;

    - (NSSet*)retrieveCategories;

    - (void)subscribeWithCategories:(NSSet*)categories completion:(void (^)(NSError *))completion;
    ```

5. Voeg de volgende importinstructie toe Notifications.m:

    ```objc
    #import <WindowsAzureMessaging/WindowsAzureMessaging.h>
    ```

6. Kopieer de volgende code in de Implementatiesectie van het bestand Notifications.m.

    ```objc
    SBNotificationHub* hub;

    - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName{

        hub = [[SBNotificationHub alloc] initWithConnectionString:listenConnectionString
                                    notificationHubPath:hubName];

        return self;
    }

    - (void)storeCategoriesAndSubscribeWithCategories:(NSSet *)categories completion:(void (^)(NSError *))completion {
        NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
        [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];

        [self subscribeWithCategories:categories completion:completion];
    }

    - (NSSet*)retrieveCategories {
        NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];

        NSArray* categories = [defaults stringArrayForKey:@"BreakingNewsCategories"];

        if (!categories) return [[NSSet alloc] init];
        return [[NSSet alloc] initWithArray:categories];
    }

    - (void)subscribeWithCategories:(NSSet *)categories completion:(void (^)(NSError *))completion
    {
        //[hub registerNativeWithDeviceToken:self.deviceToken tags:categories completion: completion];

        NSString* templateBodyAPNS = @"{\"aps\":{\"alert\":\"$(messageParam)\"}}";

        [hub registerTemplateWithDeviceToken:self.deviceToken name:@"simpleAPNSTemplate" 
            jsonBodyTemplate:templateBodyAPNS expiryTemplate:@"0" tags:categories completion:completion];
    }
    ```

    Deze klasse maakt gebruik van lokale opslag voor het opslaan en ophalen van de categorieën van nieuws die dit apparaat ontvangt. Het bevat ook, een methode om u te registreren voor deze categorieën met behulp van een [sjabloon](notification-hubs-templates-cross-platform-push-messages.md) registratie.

7. In de `AppDelegate.h` bestand, een importinstructie voor toevoegen `Notifications.h` en voegt u de eigenschap voor een exemplaar van de `Notifications` klasse:

    ```objc
    #import "Notifications.h"

    @property (nonatomic) Notifications* notifications;
    ```

8. In de `didFinishLaunchingWithOptions` methode in `AppDelegate.m`, voeg de code voor het initialiseren van de meldingen instantie aan het begin van de methode toe.  
    `HUBNAME` en `HUBLISTENACCESS` (gedefinieerd in `hubinfo.h`) moet al hebben de `<hub name>` en `<connection string with listen access>` tijdelijke aanduidingen vervangen door de naam van uw notification hub en de verbindingsreeks voor *DefaultListenSharedAccessSignature*die u eerder hebt verkregen

    ```objc
    self.notifications = [[Notifications alloc] initWithConnectionString:HUBLISTENACCESS HubName:HUBNAME];
    ```

    > [!NOTE]
    > Omdat referenties die worden gedistribueerd met een client-app meestal niet beveiligd zijn, moet u met uw client-app alleen de sleutel voor listen-toegang distribueren. Uw app kan dan worden geregistreerd voor meldingen, maar bestaande registraties kunnen niet worden gewijzigd, en er kunnen geen meldingen worden verzonden. De sleutel voor volledige toegang wordt gebruikt in een beveiligde back-endservice voor het verzenden van meldingen en het wijzigen van bestaande registraties.

9. In de `didRegisterForRemoteNotificationsWithDeviceToken` methode in `AppDelegate.m`, vervang de code in de methode met de volgende code om door te geven van het apparaattoken aan de `notifications` klasse. De `notifications` klasse voert de registreren voor meldingen met de categorieën. Als de gebruiker categorieselecties wijzigt, roept de `subscribeWithCategories` methode in reactie op de **abonneren** knop deze kunnen worden bijgewerkt.

    > [!NOTE]
    > Omdat het apparaattoken dat is toegewezen door de Apple Push Notification Service (APNS) kan kans op elk gewenst moment, dient u te registreren voor meldingen vaak ter voorkoming van fouten van de melding. In dit voorbeeld wordt er elke keer dat de app wordt gestart een registratie voor meldingen vastgelegd. Voor apps die u regelmatig uitvoert (meer dan één keer per dag), kunt u de registratie waarschijnlijk overslaan om bandbreedte te besparen als er minder dan een dag is verstreken sinds de vorige registratie.

    ```objc
    self.notifications.deviceToken = deviceToken;

    // Retrieves the categories from local storage and requests a registration for these categories
    // each time the app starts and performs a registration.

    NSSet* categories = [self.notifications retrieveCategories];
    [self.notifications subscribeWithCategories:categories completion:^(NSError* error) {
        if (error != nil) {
            NSLog(@"Error registering for notifications: %@", error);
        }
    }];
    ```

    Op dit moment, mogen er geen andere code in de `didRegisterForRemoteNotificationsWithDeviceToken` methode.

10. Moeten aanwezig zijn in de volgende methoden al `AppDelegate.m` van het voltooien van de [aan de slag met Notification Hubs] [ get-started] zelfstudie. Als dat niet het geval is, toe te voegen.

    ```objc
    -(void)MessageBox:(NSString *)title message:(NSString *)messageText
    {

        UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
            cancelButtonTitle:@"OK" otherButtonTitles: nil];
        [alert show];
    }

    * (void)application:(UIApplication *)application didReceiveRemoteNotification:
       (NSDictionary *)userInfo {
       NSLog(@"%@", userInfo);
       [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
     }
    ```

    Deze methode verwerkt meldingen ontvangen wanneer de app wordt uitgevoerd door een eenvoudige weer te geven **UIAlert**.

11. In `ViewController.m`, Voeg een `import` -instructie voor `AppDelegate.h` en kopieer de volgende code in de XCode-gegenereerd `subscribe` methode. Deze code werkt de registratie voor het gebruik van de nieuwe categorielabels die de gebruiker ervoor in de gebruikersinterface gekozen heeft.

    ```objc
    #import "Notifications.h"

    NSMutableArray* categories = [[NSMutableArray alloc] init];

    if (self.WorldSwitch.isOn) [categories addObject:@"World"];
    if (self.PoliticsSwitch.isOn) [categories addObject:@"Politics"];
    if (self.BusinessSwitch.isOn) [categories addObject:@"Business"];
    if (self.TechnologySwitch.isOn) [categories addObject:@"Technology"];
    if (self.ScienceSwitch.isOn) [categories addObject:@"Science"];
    if (self.SportsSwitch.isOn) [categories addObject:@"Sports"];

    Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];

    [notifications storeCategoriesAndSubscribeWithCategories:categories completion: ^(NSError* error) {
        if (!error) {
            [(AppDelegate*)[[UIApplication sharedApplication]delegate] MessageBox:@"Notification" message:@"Subscribed!"];
        } else {
            NSLog(@"Error subscribing: %@", error);
        }
    }];
    ```

    Deze methode maakt u een `NSMutableArray` van de categorieën en maakt gebruik van de `Notifications` klasse voor het opslaan van de lijst in de lokale opslag, maar wordt geregistreerd met de notification hub de bijbehorende labels. Wanneer categorieën worden gewijzigd, wordt de registratie opnieuw gemaakt met de nieuwe categorieën.

12. In `ViewController.m`, voeg de volgende code in de `viewDidLoad` methode om in te stellen van de gebruikersinterface op basis van de eerder opgeslagen categorieën.

    ```objc
    // This updates the UI on startup based on the status of previously saved categories.

    Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];

    NSSet* categories = [notifications retrieveCategories];

    if ([categories containsObject:@"World"]) self.WorldSwitch.on = true;
    if ([categories containsObject:@"Politics"]) self.PoliticsSwitch.on = true;
    if ([categories containsObject:@"Business"]) self.BusinessSwitch.on = true;
    if ([categories containsObject:@"Technology"]) self.TechnologySwitch.on = true;
    if ([categories containsObject:@"Science"]) self.ScienceSwitch.on = true;
    if ([categories containsObject:@"Sports"]) self.SportsSwitch.on = true;
    ```

De app kan nu een set categorieën opslaan in de lokale opslag van apparaat wordt gebruikt om te registreren bij de notification hub wanneer de app wordt gestart. De gebruiker kan de selectie van categorieën op runtime en klikt u op wijzigen de `subscribe` methode voor het bijwerken van de registratie van het apparaat. Vervolgens maakt bijwerken u de app voor het verzenden van het laatste nieuws te verzenden rechtstreeks in de app zelf.

## <a name="optional-send-tagged-notifications"></a>(optioneel) Met tags meldingen verzenden

Als u geen toegang tot Visual Studio, kunt u direct doorgaan naar de volgende sectie en meldingen verzenden vanuit de app zelf. U kunt ook de juiste sjabloon melding verzenden de [Azure-portal] met behulp van het foutopsporingstabblad voor uw notification hub.

[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="optional-send-notifications-from-the-device"></a>(optioneel) Meldingen verzenden vanuit het apparaat

Normaal gesproken meldingen moeten worden verzonden door een back-endservice, maar u kunt belangrijke nieuwsfeiten rechtstreeks vanuit de app verzenden. Om dit te doen, die u bijwerkt de `SendNotificationRESTAPI` methode die u hebt gedefinieerd in de [aan de slag met Notification Hubs] [ get-started] zelfstudie.

1. In `ViewController.m`, werken de `SendNotificationRESTAPI` methode als volgt zodat deze accepteert een parameter voor de categorie-tag en verzendt de juiste [sjabloon](notification-hubs-templates-cross-platform-push-messages.md) melding.

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
        json = [NSString stringWithFormat:@"{\"messageParam\":\"Breaking %@ News : %@\"}",
                categoryTag, self.notificationMessage.text];

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

2. In `ViewController.m`, werken de `Send Notification` actie zoals weergegeven in de code die volgt. Zodat deze de meldingen met behulp van elke tag afzonderlijk worden verzonden en naar meerdere platforms stuurt.

    ```objc
    - (IBAction)SendNotificationMessage:(id)sender
    {
        self.sendResults.text = @"";

        NSArray* categories = [NSArray arrayWithObjects: @"World", @"Politics", @"Business",
                                @"Technology", @"Science", @"Sports", nil];

        // Lets send the message as breaking news for each category to WNS, FCM, and APNS
        // using a template.
        for(NSString* category in categories)
        {
            [self SendNotificationRESTAPI:category];
        }
    }
    ```

3. Bouw uw project opnieuw op en zorg ervoor dat u hebt geen fouten in de build.

## <a name="run-the-app-and-generate-notifications"></a>De app uitvoeren en meldingen genereren

1. Druk op de knop uitvoeren op het project bouwen en de app te starten. Selecteer enkele belangrijke nieuws opties om u te abonneren op en druk vervolgens op de **abonneren** knop. U ziet een dialoogvenster die wijzen op dat de meldingen hebt geabonneerd op.

    ![Voorbeeld van de melding op iOS][1]

    Als u ervoor kiest **abonneren**, de app converteert u de geselecteerde categorieën naar tags en vraagt u een nieuwe device Registration service voor de geselecteerde tags van de notification hub.

2. Voer een bericht wordt verzonden als belangrijk nieuws druk vervolgens op de **melding verzenden** knop. U kunt ook uitvoeren de .NET-consoletoepassing om meldingen te genereren.

    ![Voorkeuren voor meldingen wijzigen in iOS][2]

3. Elk apparaat geabonneerd op het laatste nieuws, ontvangt de belangrijke nieuwsfeiten die u zojuist hebt verzonden.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie, kunt u meldingen uitgezonden naar specifieke iOS-apparaten die zijn geregistreerd voor de categorieën verzonden. Als u wilt weten hoe u gelokaliseerde pushmeldingen, Ga naar de volgende zelfstudie:

> [!div class="nextstepaction"]
>[Gelokaliseerde pushmeldingen verzenden](notification-hubs-ios-xplat-localized-apns-push-notification.md)

<!-- Images. -->
[1]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-subscribed.png
[2]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios1.png
[3]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios2.png

<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: https://msdn.microsoft.com/library/jj927168.aspx
[Use Notification Hubs to broadcast localized breaking news]: notification-hubs-ios-xplat-localized-apns-push-notification.md
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs]: notification-hubs-aspnet-backend-ios-notify-users.md
[Notification Hubs Guidance]: https://msdn.microsoft.com/library/dn530749.aspx
[Notification Hubs How-To for iOS]: https://msdn.microsoft.com/library/jj927168.aspx
[get-started]: notification-hubs-ios-apple-push-notification-apns-get-started.md
[Azure-portal]: https://portal.azure.com
