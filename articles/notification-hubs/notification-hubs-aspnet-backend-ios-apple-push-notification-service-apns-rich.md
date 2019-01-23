---
title: Azure Notification Hubs uitgebreide Push
description: Leer hoe u uitgebreide pushmeldingen verzenden naar een iOS-app van Azure. Codevoorbeelden geschreven in Objective-C en C#.
documentationcenter: ios
services: notification-hubs
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: 590304df-c0a4-46c5-8ef5-6a6486bb3340
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: dd808a04dff77388248bf7309f5ff804e6dd065c
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54447749"
---
# <a name="azure-notification-hubs-rich-push"></a>Azure Notification Hubs uitgebreide Push

## <a name="overview"></a>Overzicht

Om gebruikers met directe uitgebreide inhoud, wilt een toepassing pushen buiten tekst zonder opmaak. Deze meldingen bevordering van interactie van gebruikers en die de inhoud, zoals URL's, geluiden en afbeeldingen/coupons. In deze zelfstudie bouwt voort op de [gebruikers waarschuwen](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) onderwerp, en laat zien hoe u pushmeldingen te verzenden die nettoladingen (bijvoorbeeld, afbeelding).

Deze zelfstudie is compatibel met iOS 7 en 8.

  ![][IOS1]

Op hoog niveau:

1. De back-end:
   * De nettolading van de uitgebreide opgeslagen (in dit geval installatiekopie) in de back-end-database/lokale opslag
   * ID van deze uitgebreide melding verzendt naar het apparaat
2. De App op het apparaat:
   * Neemt contact op met de back-end het aanvragen van de nettolading van de uitgebreide met de ID die wordt ontvangen
   * Meldingen voor gebruikers op het apparaat wordt verzonden wanneer het ophalen van gegevens voltooid is en de nettolading bevat onmiddellijk wanneer gebruikers Tik voor meer informatie

## <a name="webapi-project"></a>WebAPI Project

1. Open in Visual Studio, de **AppBackend** project dat u hebt gemaakt in de [gebruikers waarschuwen](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) zelfstudie.
2. Verkrijgen van een installatiekopie die u wilt gebruikers met een melding ontvangen en plaatst deze in een **img** map in uw projectmap.
3. Klik op **alle bestanden weergeven** in Solution Explorer met de rechtermuisknop op de map **opnemen In Project**.
4. Met de installatiekopie hebt geselecteerd, wijzigt u de bijbehorende actie bouwen in het venster Eigenschappen naar **ingesloten bron**.

    ![][IOS2]
5. In `Notifications.cs`, voeg de volgende instructie:

    ```c#
    using System.Reflection;
    ```
6. Bijwerken van de gehele `Notifications` klasse met de volgende code. Zorg ervoor dat Vervang de tijdelijke aanduidingen door uw notification hub referenties en de bestandsnaam van de installatiekopie.

    ```c#
    public class Notification {
        public int Id { get; set; }
        // Initial notification message to display to users
        public string Message { get; set; }
        // Type of rich payload (developer-defined)
        public string RichType { get; set; }
        public string Payload { get; set; }
        public bool Read { get; set; }
    }

    public class Notifications {
        public static Notifications Instance = new Notifications();

        private List<Notification> notifications = new List<Notification>();

        public NotificationHubClient Hub { get; set; }

        private Notifications() {
            // Placeholders: replace with the connection string (with full access) for your notification hub and the hub name from the Azure Classics Portal
            Hub = NotificationHubClient.CreateClientFromConnectionString("{conn string with full access}",  "{hub name}");
        }

        public Notification CreateNotification(string message, string richType, string payload) {
            var notification = new Notification() {
                Id = notifications.Count,
                Message = message,
                RichType = richType,
                Payload = payload,
                Read = false
            };

            notifications.Add(notification);

            return notification;
        }

        public Stream ReadImage(int id) {
            var assembly = Assembly.GetExecutingAssembly();
            // Placeholder: image file name (for example, logo.png).
            return assembly.GetManifestResourceStream("AppBackend.img.{logo.png}");
        }
    }
    ```

   > [!NOTE]
   > (optioneel) Raadpleeg [insluiten en toegang krijgen tot bronnen met behulp van Visual C# ](https://support.microsoft.com/kb/319292) voor meer informatie over het toevoegen en de projectresources te verkrijgen.

7. In `NotificationsController.cs`, definiëren ' NotificationsController met de volgende codefragmenten. Dit wordt een eerste op de achtergrond uitgebreide meldings-id naar apparaat verzonden en kan aan de clientzijde voor het ophalen van de installatiekopie van:

    ```c#
    // Return http response with image binary
    public HttpResponseMessage Get(int id) {
        var stream = Notifications.Instance.ReadImage(id);

        var result = new HttpResponseMessage(HttpStatusCode.OK);
        result.Content = new StreamContent(stream);
        // Switch in your image extension for "png"
        result.Content.Headers.ContentType = new System.Net.Http.Headers.MediaTypeHeaderValue("image/{png}");

        return result;
    }

    // Create rich notification and send initial silent notification (containing id) to client
    public async Task<HttpResponseMessage> Post() {
        // Replace the placeholder with image file name
        var richNotificationInTheBackend = Notifications.Instance.CreateNotification("Check this image out!", "img",  "{logo.png}");

        var usernameTag = "username:" + HttpContext.Current.User.Identity.Name;

        // Silent notification with content available
        var aboutUser = "{\"aps\": {\"content-available\": 1, \"sound\":\"\"}, \"richId\": \"" + richNotificationInTheBackend.Id.ToString() + "\",  \"richMessage\": \"" + richNotificationInTheBackend.Message + "\", \"richType\": \"" + richNotificationInTheBackend.RichType + "\"}";

        // Send notification to apns
        await Notifications.Instance.Hub.SendAppleNativeNotificationAsync(aboutUser, usernameTag);

        return Request.CreateResponse(HttpStatusCode.OK);
    }
    ```
8. We gaan deze app naar een Azure-Website nu opnieuw zodat deze toegankelijk is vanaf alle apparaten implementeren. Klik met de rechtermuisknop op het project **AppBackend** en selecteer **Publiceren**.
9. Selecteer de Azure-Website als het doel van uw publiceren. Meld u aan met uw Azure-account en selecteer een bestaande of nieuwe Website en noteer de **doel-URL** eigenschap in de **verbinding** tabblad. Naar deze URL wordt verderop in deze zelfstudie verwezen als uw *back-endeindpunt*. Klik op **Publish**.

## <a name="modify-the-ios-project"></a>Wijzigen van het iOS-project

Nu dat u hebt gewijzigd dat uw back-end voor het verzenden van alleen de *id* van een melding, wijzigt u uw iOS-app voor het verwerken van deze id en het uitgebreide bericht ophalen uit uw back-end.

1. Open uw iOS-project en externe meldingen inschakelen door te gaan naar het doel van de belangrijkste app in de **doelen** sectie.
2. Klik op **mogelijkheden**, schakel **Achtergrondmodi**, en controleer de **Remote Notifications** selectievakje.

    ![][IOS3]
3. Open `Main.storyboard`, en zorg ervoor dat u een View-Controller (aangeduid als Start View-Controller in deze zelfstudie) van [gebruiker inlichten](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) zelfstudie.
4. Toevoegen een **navigatie Controller** aan uw storyboard en controle en sleep naar Start Weergavecontroller u gemakkelijk de **hoofdmap weergave** van navigatie. Zorg ervoor dat de **initiële View-Controller Is** in kenmerken inspector is geselecteerd voor de navigatie-Controller alleen.
5. Voeg een **View-Controller** storyboard en toevoegen een **Afbeeldingsweergave**. Dit is de pagina die gebruikers zien wanneer ze ervoor kiezen om meer te weten te komen door te klikken op de melding. Uw storyboard moet er als volgt uitzien:

    ![][IOS4]
6. Klik op de **start View-Controller** in de hoofd-storyboard en zorg ervoor dat het heeft **homeViewController** als de **aangepaste klasse** en **Storyboard ID**onder de inspector identiteit.
7. Doe hetzelfde voor installatiekopie View-Controller als **imageViewController**.
8. Maak vervolgens een nieuwe View-Controller-klasse met de titel **imageViewController** voor het afhandelen van de gebruikersinterface die u zojuist hebt gemaakt.
9. In **imageViewController.h**, Voeg het volgende toe van de controller interface declaraties. Zorg ervoor dat u control ingedrukt houden van de weergave van de installatiekopie van hoofd-storyboard aan deze eigenschappen om te koppelen van de twee:

    ```objc
    @property (weak, nonatomic) IBOutlet UIImageView *myImage;
    @property (strong) UIImage* imagePayload;
    ```
10. In `imageViewController.m`, Voeg het volgende toe aan het einde van `viewDidload`:

    ```objc
    // Display the UI Image in UI Image View
    [self.myImage setImage:self.imagePayload];
    ```
11. In `AppDelegate.m`, importeren van de installatiekopie-controller die u hebt gemaakt:

    ```objc
    #import "imageViewController.h"
    ```
12. Een sectie interface met de volgende declaratie toevoegen:

    ```objc
    @interface AppDelegate ()

    @property UIImage* imagePayload;
    @property NSDictionary* userInfo;
    @property BOOL iOS8;

    // Obtain content from backend with notification id
    - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion;

    // Redirect to Image View Controller after notification interaction
    - (void)redirectToImageViewWithImage: (UIImage *)img;

    @end
    ```
13. In `AppDelegate`, zorg ervoor dat uw app wordt geregistreerd voor meldingen op de achtergrond `application: didFinishLaunchingWithOptions`:

    ```objc
    // Software version
    self.iOS8 = [[UIApplication sharedApplication] respondsToSelector:@selector(registerUserNotificationSettings:)] && [[UIApplication sharedApplication] respondsToSelector:@selector(registerForRemoteNotifications)];

    // Register for remote notifications for iOS8 and previous versions
    if (self.iOS8) {
        NSLog(@"This device is running with iOS8.");

        // Action
        UIMutableUserNotificationAction *richPushAction = [[UIMutableUserNotificationAction alloc] init];
        richPushAction.identifier = @"richPushMore";
        richPushAction.activationMode = UIUserNotificationActivationModeForeground;
        richPushAction.authenticationRequired = NO;
        richPushAction.title = @"More";

        // Notification category
        UIMutableUserNotificationCategory* richPushCategory = [[UIMutableUserNotificationCategory alloc] init];
        richPushCategory.identifier = @"richPush";
        [richPushCategory setActions:@[richPushAction] forContext:UIUserNotificationActionContextDefault];

        // Notification categories
        NSSet* richPushCategories = [NSSet setWithObjects:richPushCategory, nil];

        UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
                                                UIUserNotificationTypeAlert |
                                                UIUserNotificationTypeBadge
                                                                                    categories:richPushCategories];

        [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
        [[UIApplication sharedApplication] registerForRemoteNotifications];

    }
    else {
        // Previous iOS versions
        NSLog(@"This device is running with iOS7 or earlier versions.");

        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeNewsstandContentAvailability];
    }

    return YES;
    ```

14. Vervang in de volgende implementatie voor `application:didRegisterForRemoteNotificationsWithDeviceToken` aan de hoofd-storyboard gebruikersinterface wordt gewijzigd in aanmerking nemen:

    ```objc
    // Access navigation controller which is at the root of window
    UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
    // Get home view controller from stack on navigation controller
    homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
    hvc.deviceToken = deviceToken;
    ```
15. Vervolgens voegt u de volgende methoden voor het `AppDelegate.m` ophalen van de installatiekopie van het eindpunt en een lokale melding verzenden wanneer het ophalen van voltooid is. Zorg ervoor dat u de tijdelijke aanduiding vervangt `{backend endpoint}` aan uw back endeindpunt:

    ```objc
    NSString *const GetNotificationEndpoint = @"{backend endpoint}/api/notifications";

    // Helper: retrieve notification content from backend with rich notification id
    - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion {
        UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
        homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
        NSString* authenticationHeader = hvc.registerClient.authenticationHeader;
        // Check if authenticated
        if (!authenticationHeader) return;

        NSURLSession* session = [NSURLSession
                                sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                                delegate:nil
                                delegateQueue:nil];

        NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/%d", GetNotificationEndpoint, richId]];
        NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
        [request setHTTPMethod:@"GET"];
        NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@", authenticationHeader];
        [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

        NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {

            NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
            if (!error && httpResponse.statusCode == 200) {
                // From NSData to UIImage
                self.imagePayload = [UIImage imageWithData:data];

                completion(nil);
            }
            else {
                NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                if (error)
                    completion(error);
                else {
                    completion([NSError errorWithDomain:@"APICall" code:httpResponse.statusCode userInfo:nil]);
                }
            }
        }];
        [dataTask resume];
    }

    // Handle silent push notifications when id is sent from backend
    - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler {
        self.userInfo = userInfo;
        int richId = [[self.userInfo objectForKey:@"richId"] intValue];
        NSString* richType = [self.userInfo objectForKey:@"richType"];

        // Retrieve image data
        if ([richType isEqualToString:@"img"]) {  
            [self retrieveRichImageWithId:richId completion:^(NSError* error) {
                if (!error){
                    // Send local notification
                    UILocalNotification* localNotification = [[UILocalNotification alloc] init];

                    // "5" is arbitrary here to give you enough time to quit out of the app and receive push notifications
                    localNotification.fireDate = [NSDate dateWithTimeIntervalSinceNow:5];
                    localNotification.userInfo = self.userInfo;
                    localNotification.alertBody = [self.userInfo objectForKey:@"richMessage"];
                    localNotification.timeZone = [NSTimeZone defaultTimeZone];

                    // iOS8 categories
                    if (self.iOS8) {
                        localNotification.category = @"richPush";
                    }

                    [[UIApplication sharedApplication] scheduleLocalNotification:localNotification];

                    handler(UIBackgroundFetchResultNewData);
                }
                else{
                    handler(UIBackgroundFetchResultFailed);
                }
            }];
        }
        // Add "else if" here to handle more types of rich content such as url, sound files, etc.
    }
    ```
16. De lokale melding hierboven worden verwerkt door het openen van de installatiekopie view-controller in `AppDelegate.m` met de volgende methoden:

    ```objc
    // Helper: redirect users to image view controller
    - (void)redirectToImageViewWithImage: (UIImage *)img {
        UINavigationController *navigationController = (UINavigationController*) self.window.rootViewController;
        UIStoryboard *mainStoryboard = [UIStoryboard storyboardWithName:@"Main" bundle: nil];
        imageViewController *imgViewController = [mainStoryboard instantiateViewControllerWithIdentifier: @"imageViewController"];
        // Pass data/image to image view controller
        imgViewController.imagePayload = img;

        // Redirect
        [navigationController pushViewController:imgViewController animated:YES];
    }

    // Handle local notification sent above in didReceiveRemoteNotification
    - (void)application:(UIApplication *)application didReceiveLocalNotification:(UILocalNotification *)notification {
        if (application.applicationState == UIApplicationStateActive) {
            // Show in-app alert with an extra "more" button
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:notification.alertBody delegate:self cancelButtonTitle:@"OK" otherButtonTitles:@"More", nil];
            [alert show];
        }
        // App becomes active from user's tap on notification
        else {
            [self redirectToImageViewWithImage:self.imagePayload];
        }
    }

    // Handle buttons in in-app alerts and redirect with data/image
    - (void)alertView:(UIAlertView *)alertView clickedButtonAtIndex:(NSInteger)buttonIndex {
        // Handle "more" button
        if (buttonIndex == 1)
        {
            [self redirectToImageViewWithImage:self.imagePayload];
        }
        // Add "else if" here to handle more buttons
    }

    // Handle notification setting actions in iOS8
    - (void)application:(UIApplication *)application handleActionWithIdentifier:(NSString *)identifier forLocalNotification:(UILocalNotification *)notification completionHandler:(void (^)())completionHandler {
        // Handle richPush related buttons
        if ([identifier isEqualToString:@"richPushMore"]) {
            [self redirectToImageViewWithImage:self.imagePayload];
        }
        completionHandler();
    }
    ```

## <a name="run-the-application"></a>De toepassing wordt uitgevoerd

1. In XCode, moet u de app uitvoeren op een fysiek iOS-apparaat (push-meldingen niet in de simulator werken).
2. Voer een gebruikersnaam en wachtwoord van de dezelfde waarde voor verificatie en klik in de iOS-app UI **aanmelden**.
3. Klik op **pushmelding verzonden** en ziet u een waarschuwing in de app. Als u klikt op **meer**, u naar de installatiekopie die u hebt gekozen om op te nemen in de back-end van uw app wordt geopend.
4. U kunt ook klikken op **pushmelding verzonden** en druk onmiddellijk op de knop Start van uw apparaat. In enkele ogenblikken ontvangt u een pushmelding ontvangen. Als u erop tikt of klikt u op meer, wordt u in uw app en de inhoud van de uitgebreide gebracht.

[IOS1]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-1.png
[IOS2]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-2.png
[IOS3]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-3.png
[IOS4]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-4.png
