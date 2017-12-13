---
title: Notification Hubs belangrijk nieuws zelfstudie - iOS
description: Informatie over het gebruik van Azure Service Bus Notification Hubs voor belangrijk nieuws meldingen verzenden naar iOS-apparaten.
services: notification-hubs
documentationcenter: ios
author: ysxu
manager: erikre
editor: 
ms.assetid: 6ead4169-deff-4947-858c-8c6cf03cc3b2
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 8aec171b46df3e0e7f2a2d3cc9d44084d064e6fd
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2017
---
# <a name="use-notification-hubs-to-send-breaking-news"></a>Notification Hubs gebruiken om belangrijk nieuws te verzenden
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a>Overzicht
Dit onderwerp leest u het gebruik van Azure Notification Hubs voor belangrijk nieuws meldingen naar een iOS-app-broadcast. Als u klaar gaat u kunnen registreren voor nieuwscategorieën die u geïnteresseerd bent in op te splitsen en pushmeldingen voor deze categorieën ontvangen. Dit scenario is een algemene patroon voor veel apps waarbij moeten meldingen worden verzonden naar groepen gebruikers die interesse in deze, zoals RSS-lezer, apps voor muziek ventilatoren, enzovoort eerder is gedeclareerd.

Broadcast-scenario's zijn ingeschakeld door een of meer *labels* bij het maken van een registratie in de notification hub. Wanneer u meldingen worden verzonden naar een label, ontvangen alle apparaten die zijn geregistreerd voor het label de melding. Omdat tags gewoon tekenreeksen zijn, hoeven niet vooraf zijn ingericht. Raadpleeg voor meer informatie over tags [Notification Hubs-Routering en code-expressies](notification-hubs-tags-segment-push-message.md).

## <a name="prerequisites"></a>Vereisten
In dit onderwerp is gebaseerd op de app die u hebt gemaakt in [aan de slag met Notification Hubs][get-started]. Voordat u deze zelfstudie begint, u moet al hebt voltooid [aan de slag met Notification Hubs][get-started].

## <a name="add-category-selection-to-the-app"></a>Categorieselectie toevoegen aan de app.
De eerste stap is het toevoegen van de UI-elementen naar uw bestaande storyboard waarmee de gebruiker kan de categorieën selecteren om te registreren. De categorieën die door een gebruiker is geselecteerd worden op het apparaat opgeslagen. Wanneer de app wordt gestart, wordt de apparaatregistratie van een in uw notification hub met de geselecteerde categorieën gemaakt als labels.

1. Voeg de volgende onderdelen van de objectbibliotheek in uw MainStoryboard_iPhone.storyboard:
   
   * Een label met 'Op te splitsen nieuws'-tekst
   * Labels met Categorieteksten "Wereld", 'Politiek', 'Business', 'Technologie', 'Wetenschappelijke', 'Sport'
   * Zes switches, één per categorie instellen elke switch **status** worden **uit** standaard.
   * Een knop met het label 'Abonneren'
     
     Een storyboard ziet er als volgt:
     
     ![][3]
2. In de editor assistent aansluitingen voor alle schakelopties maken en het aanroepen van 'WorldSwitch', 'PoliticsSwitch', 'BusinessSwitch', 'TechnologySwitch', 'ScienceSwitch', 'SportsSwitch'
3. Een actie voor de knop met de naam 'abonneren' maken. Uw ViewController.h moeten het volgende bevatten:
   
        @property (weak, nonatomic) IBOutlet UISwitch *WorldSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *PoliticsSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *BusinessSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *TechnologySwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *ScienceSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *SportsSwitch;
   
        - (IBAction)subscribe:(id)sender;
4. Maak een nieuwe **Cocoa Touch klasse** aangeroepen `Notifications`. Kopieer de volgende code in de sectie interface van het bestand Notifications.h:
   
        @property NSData* deviceToken;
   
        - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName;
   
        - (void)storeCategoriesAndSubscribeWithCategories:(NSArray*)categories
                    completion:(void (^)(NSError* error))completion;
   
        - (NSSet*)retrieveCategories;
   
        - (void)subscribeWithCategories:(NSSet*)categories completion:(void (^)(NSError *))completion;
5. De volgende importinstructie aan Notifications.m toevoegen:
   
        #import <WindowsAzureMessaging/WindowsAzureMessaging.h>
6. Kopieer de volgende code in de Implementatiesectie van het bestand Notifications.m.
   
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



    Deze klasse maakt gebruik van lokale opslag op te slaan en het ophalen van de categorieën van nieuws dat dit apparaat ontvangt. Het bevat ook, een methode om te registreren voor deze categorieën met behulp van een [sjabloon](notification-hubs-templates-cross-platform-push-messages.md) registratie.

1. In het bestand AppDelegate.h een importinstructie voor Notifications.h toevoegen en een eigenschap voor een exemplaar van de klasse meldingen toevoegen:
   
        #import "Notifications.h"
   
        @property (nonatomic) Notifications* notifications;
2. In de **didFinishLaunchingWithOptions** methode in AppDelegate.m, voeg de code voor het initialiseren van het exemplaar van de meldingen aan het begin van de methode.  
   
    `HUBNAME`en `HUBLISTENACCESS` (gedefinieerd in hubinfo.h) al de `<hub name>` en `<connection string with listen access>` tijdelijke aanduidingen vervangen door de naam van uw notification hub en de verbindingsreeks voor *DefaultListenSharedAccessSignature* die u eerder hebt verkregen.
   
        self.notifications = [[Notifications alloc] initWithConnectionString:HUBLISTENACCESS HubName:HUBNAME];
   
   > [!NOTE]
   > Omdat de referenties die worden gedistribueerd met een client-app niet over het algemeen veilig, moet u de sleutel voor listen toegang alleen distribueren met uw clientapp. Luisteren toegang kunnen uw app registreren voor meldingen, maar bestaande registraties kan niet worden gewijzigd en kunnen niet worden meldingen verzonden. De volledige toegang tot de sleutel wordt gebruikt in een beveiligde back-endservice voor het verzenden van meldingen en bestaande registraties wijzigen.
   > 
   > 
3. In de **didRegisterForRemoteNotificationsWithDeviceToken** methode in AppDelegate.m, vervang de code in de methode met de volgende code naar het apparaattoken doorgeven aan de klasse meldingen. De klasse meldingen wordt uitgevoerd om het registreren voor meldingen met de categorieën. Als de gebruiker categorieselecties wijzigt, noemen we de `subscribeWithCategories` methode in reactie op de **abonneren** knop bijwerken.
   
   > [!NOTE]
   > Omdat het apparaattoken toegewezen door de Apple Push Notification Service (APNS) kan op elk gewenst moment kans, kunt u moet registreren voor meldingen vaak ter voorkoming van fouten van de melding. In dit voorbeeld registreert voor melding van elke keer dat de app wordt gestart. Voor apps die vaak worden uitgevoerd, kunt meer dan één keer per dag, u waarschijnlijk overslaan registratie om bandbreedte te besparen als minder dan een dag is verstreken sinds de vorige registratie.
   > 
   > 
   
        self.notifications.deviceToken = deviceToken;
   
        // Retrieves the categories from local storage and requests a registration for these categories
        // each time the app starts and performs a registration.
   
        NSSet* categories = [self.notifications retrieveCategories];
        [self.notifications subscribeWithCategories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];

    Houd er rekening mee dat op dit moment moet er geen andere code in de **didRegisterForRemoteNotificationsWithDeviceToken** methode.

1. De volgende methoden moeten al aanwezig zijn in AppDelegate.m voltooid de [aan de slag met Notification Hubs] [ get-started] zelfstudie.  Als dat niet het geval is, toe te voegen.
   
    -(leeg) MessageBox:(NSString *) Titel bericht:(NSString *) tekstbericht {
   
        UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
            cancelButtonTitle:@"OK" otherButtonTitles: nil];
        [alert show];
    }
   
   * (leeg) toepassing:(UIApplication *) toepassing didReceiveRemoteNotification: (NSDictionary *) gebruikersgegevens {NSLog (@"% @", gebruikersgegevens);   [self MessageBox:@"Notification" bericht: [[gebruikersgegevens objectForKey:@"aps"] valueForKey:@"alert"]];}
   
   Deze methode meldingen ontvangen wanneer de app wordt uitgevoerd door een eenvoudige weer te geven afhandelt **UIAlert**.
2. Voeg een importinstructie voor AppDelegate.h in ViewController.m, en kopieer de volgende code in XCode gegenereerde **abonneren** methode. Deze code wordt de registratie voor het gebruik van de nieuwe categorielabels die de gebruiker heeft gekozen in de gebruikersinterface worden bijgewerkt.
   
       ```
       #import "Notifications.h"
       ```
   
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
   
   Deze methode maakt u een **NSMutableArray** van de categorieën en gebruikt de **meldingen** klasse voor het opslaan van de lijst in de lokale opslag en registreert de bijbehorende tags voor uw notification hub. Wanneer categorieën worden gewijzigd, wordt de registratie opnieuw gemaakt met de nieuwe categorieën.
3. In ViewController.m, voeg de volgende code in de **viewDidLoad** methode de gebruikersinterface instellen op basis van de eerder opgeslagen categorieën.

        // This updates the UI on startup based on the status of previously saved categories.

        Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];

        NSSet* categories = [notifications retrieveCategories];

        if ([categories containsObject:@"World"]) self.WorldSwitch.on = true;
        if ([categories containsObject:@"Politics"]) self.PoliticsSwitch.on = true;
        if ([categories containsObject:@"Business"]) self.BusinessSwitch.on = true;
        if ([categories containsObject:@"Technology"]) self.TechnologySwitch.on = true;
        if ([categories containsObject:@"Science"]) self.ScienceSwitch.on = true;
        if ([categories containsObject:@"Sports"]) self.SportsSwitch.on = true;



De app kan nu een set van categorieën worden opgeslagen in de lokale opslag van apparaat wordt gebruikt om u te registreren bij de notification hub wanneer de app wordt gestart.  De gebruiker kan de selectie van categorieën op runtime en klik op wijzigen de **abonneren** methode voor het bijwerken van de registratie voor het apparaat. Vervolgens kunt u de app het laatste nieuws om meldingen te verzenden rechtstreeks in de app zelf wordt bijgewerkt.

## <a name="optional-sending-tagged-notifications"></a>(optioneel) Verzenden van meldingen met tags
Als u geen toegang tot Visual Studio hebt, kunt u met de volgende sectie overslaan en meldingen verzenden vanuit de app zelf. U kunt ook de juiste sjabloon melding verzenden de [Azure-portal] met behulp van het foutopsporingstabblad voor uw notification hub. 

[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="optional-send-notifications-from-the-device"></a>(optioneel) Meldingen verzenden vanuit het apparaat
Normaal gesproken meldingen moeten worden verzonden door een back-endservice maar belangrijk nieuws om meldingen te verzenden rechtstreeks vanuit de app. Hiervoor wordt bijgewerkt de `SendNotificationRESTAPI` methode die is gedefinieerd in de [aan de slag met Notification Hubs] [ get-started] zelfstudie.

1. In de update ViewController.m de `SendNotificationRESTAPI` methode als volgt zodat het accepteert de parameter voor het label categorie en verzendt de juiste [sjabloon](notification-hubs-templates-cross-platform-push-messages.md) melding.
   
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
2. In de update ViewController.m de **melding verzenden** actie, zoals wordt weergegeven in de volgende code. Zodat het wordt verzenden van meldingen met elke tag afzonderlijk en naar meerdere platforms verzenden.

        - (IBAction)SendNotificationMessage:(id)sender
        {
            self.sendResults.text = @"";

            NSArray* categories = [NSArray arrayWithObjects: @"World", @"Politics", @"Business",
                                    @"Technology", @"Science", @"Sports", nil];

            // Lets send the message as breaking news for each category to WNS, GCM, and APNS
            // using a template.
            for(NSString* category in categories)
            {
                [self SendNotificationRESTAPI:category];
            }
        }



1. Bouw het project opnieuw op en controleer of er geen fouten in de build.

## <a name="run-the-app-and-generate-notifications"></a>Voer de app en meldingen genereren
1. Druk op de knop uitvoeren op het project bouwen en de app te starten. Sommige belangrijk nieuws opties selecteren om te abonneren op en druk vervolgens op de **abonneren** knop. U ziet een dialoogvenster waarmee wordt aangegeven in dat de meldingen bent geabonneerd op.
   
    ![][1]
   
    Wanneer u de optie **abonneren**, de app de geselecteerde categorieën converteert naar labels en een nieuwe apparaatregistratie voor de geselecteerde codes aanvragen van de notification hub.
2. Voer een bericht wordt verzonden als belangrijk nieuws druk de **melding verzenden** knop. Ook uitvoeren van de .NET-consoletoepassing om meldingen te genereren.
   
    ![][2]
3. Elk apparaat geabonneerd op belangrijk nieuws ontvangt de belangrijk nieuws-meldingen die u zojuist hebt verzonden.

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd hoe belangrijk nieuws per categorie-broadcast. Houd rekening met het voltooien van een van de volgende zelfstudies die andere geavanceerde scenario's voor Notification Hubs markeren:

* **[Notification Hubs gebruiken voor het uitzenden van gelokaliseerde belangrijk nieuws]**
  
    Informatie over het uitbreiden van de app belangrijk nieuws zodat gelokaliseerde verzenden van meldingen.

<!-- Images. -->
[1]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-subscribed.png
[2]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios1.png
[3]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios2.png








<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
[Notification Hubs gebruiken voor het uitzenden van gelokaliseerde belangrijk nieuws]: notification-hubs-ios-xplat-localized-apns-push-notification.md
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs]: notification-hubs-aspnet-backend-ios-notify-users.md
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/dn530749.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[get-started]: /manage/services/notification-hubs/get-started-notification-hubs-ios/
[Azure-portal]: https://portal.azure.com
