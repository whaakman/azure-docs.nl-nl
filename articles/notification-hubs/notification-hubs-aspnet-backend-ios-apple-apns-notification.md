---
title: Pushmeldingen verzenden naar specifieke gebruikers met behulp van Azure Notification Hubs | Microsoft Docs
description: Leer hoe u pushmeldingen kunt verzenden naar specifieke gebruikers met behulp van Azure Notification Hubs.
documentationcenter: ios
author: jwargo
manager: patniko
editor: spelluru
services: notification-hubs
ms.assetid: 1f7d1410-ef93-4c4b-813b-f075eed20082
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: 9b6c0715cb85e245aba94adfb8b33d0d07ece9a9
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58084409"
---
# <a name="tutorial-push-notifications-to-specific-users-using-azure-notification-hubs"></a>Zelfstudie: Pushmeldingen verzenden naar specifieke gebruikers met behulp van Azure Notification Hubs

[!INCLUDE [notification-hubs-selector-aspnet-backend-notify-users](../../includes/notification-hubs-selector-aspnet-backend-notify-users.md)]

In deze zelfstudie wordt uitgelegd hoe u met Azure Notification Hubs pushmeldingen kunt verzenden naar een specifieke app-gebruiker op een specifiek apparaat. Een ASP.NET WebAPI-back-end wordt gebruikt om clients te verifiëren en voor het genereren van meldingen, zoals wordt weergegeven in het onderwerp richtlijnen [registreren vanuit uw back-end](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend).

In deze zelfstudie voert u de volgende stappen uit:

> [!div class="checklist"]
> * Het WebAPI-project maken
> * Clients verifiëren bij de WebAPI-back-end
> * Registreren voor meldingen met behulp van de WebAPI-back-end
> * Meldingen verzenden vanuit de WebAPI-back-end
> * De nieuwe WebAPI-back-end publiceren
> * Uw iOS-app wijzigen
> * De toepassing testen

## <a name="prerequisites"></a>Vereisten

Deze zelfstudie wordt ervan uitgegaan dat u hebt gemaakt en uw notification hub geconfigureerd zoals beschreven in [aan de slag met Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md). In deze zelfstudie is ook de vereiste voor de [(iOS) beveiligde Push](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md) zelfstudie.
Als u gebruiken van Mobile Apps als uw back-endservice wilt, raadpleegt u de [Mobile Apps aan de slag met Pushmeldingen](../app-service-mobile/app-service-mobile-ios-get-started-push.md).

[!INCLUDE [notification-hubs-aspnet-backend-notifyusers](../../includes/notification-hubs-aspnet-backend-notifyusers.md)]

## <a name="modify-your-ios-app"></a>Uw iOS-app wijzigen

1. Open de app met één pagina weergave die u hebt gemaakt in de [aan de slag met Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md) zelfstudie.

   > [!NOTE]
   > In deze sectie wordt ervan uitgegaan dat uw project is geconfigureerd met de naam van een lege organisatie. Als dat niet het geval is, moet u de naam van uw organisatie aan alle klassenamen van de toevoegen aan het begin.

2. In de `Main.storyboard` bestand, de onderdelen weergegeven in de schermafbeelding van de objectbibliotheek toe te voegen.

    ![Hoofd-storyboard in Xcode interface builder bewerken][1]

   * **Gebruikersnaam**: Een UITextField met tijdelijke aanduiding voor tekst, *gebruikersnaam invoeren*, direct onder de verzenden resultaten worden gelabeld en beperkt tot de linker- en rechtermarge en onder het label van de resultaten verzenden.
   * **Wachtwoord**: Een UITextField met tijdelijke aanduiding voor tekst, *wachtwoord invoeren*, direct onder de gebruikersnaam tekst veld en gebonden aan de linker- en rechtermarge en onder het tekstveld gebruikersnaam. Controleer de **tekstinvoer Secure** onder de optie in de Inspector kenmerk *retourneren sleutel*.
   * **Meld u**: Een UIButton onmiddellijk met het label onder het wachtwoordtekstveld en schakel de **ingeschakeld** onder de optie in de Inspector kenmerken *-inhoud*
   * **WNS**: Een label en switch om in te schakelen met het verzenden van de melding Windows Notification Service als deze is ingesteld op de hub. Zie de [Windows aan de slag](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) zelfstudie.
   * **GCM**: Label en schakel over naar de melding verzenden naar Google Cloud Messaging, als deze is ingesteld op de hub. Zie [Android aan de slag](notification-hubs-android-push-notification-google-gcm-get-started.md) zelfstudie.
   * **APNS**: Label en de switch waarmee de melding verzenden naar de Apple-Platform Notification-Service.
   * **Ontvanger Username:A** UITextField met tijdelijke aanduiding voor tekst, *ontvanger gebruikersnaamtag*, direct onder de GCM labelen en gebonden aan de linker- en rechtermarge en onder het label GCM.

     Sommige onderdelen zijn toegevoegd de [aan de slag met Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md) zelfstudie.

3. **CTRL** van de onderdelen in de weergave te slepen `ViewController.h` en deze nieuwe uitgangen toe te voegen.

    ```objc
    @property (weak, nonatomic) IBOutlet UITextField *UsernameField;
    @property (weak, nonatomic) IBOutlet UITextField *PasswordField;
    @property (weak, nonatomic) IBOutlet UITextField *RecipientField;
    @property (weak, nonatomic) IBOutlet UITextField *NotificationField;

    // Used to enable the buttons on the UI
    @property (weak, nonatomic) IBOutlet UIButton *LogInButton;
    @property (weak, nonatomic) IBOutlet UIButton *SendNotificationButton;

    // Used to enabled sending notifications across platforms
    @property (weak, nonatomic) IBOutlet UISwitch *WNSSwitch;
    @property (weak, nonatomic) IBOutlet UISwitch *GCMSwitch;
    @property (weak, nonatomic) IBOutlet UISwitch *APNSSwitch;

    - (IBAction)LogInAction:(id)sender;
    ```

4. In `ViewController.h`, voeg de volgende `#define` na uw importinstructies. Vervang de `<Enter Your Backend Endpoint>` tijdelijke aanduiding met de doel-URL die u gebruikt voor het implementeren van uw app back-end in de vorige sectie. Bijvoorbeeld `http://your_backend.azurewebsites.net`.

    ```objc
    #define BACKEND_ENDPOINT @"<Enter Your Backend Endpoint>"
    ```

5. In uw project, maakt u een nieuwe klasse Cocoa Touch met de naam `RegisterClient` voor interactie met de ASP.NET back-end u hebt gemaakt. Maken van de klasse is overgenomen van `NSObject`. Voeg de volgende code in de `RegisterClient.h`.

    ```objc
    @interface RegisterClient : NSObject

    @property (strong, nonatomic) NSString* authenticationHeader;

    -(void) registerWithDeviceToken:(NSData*)token tags:(NSSet*)tags
        andCompletion:(void(^)(NSError*))completion;

    -(instancetype) initWithEndpoint:(NSString*)Endpoint;

    @end
    ```

6. In de `RegisterClient.m`, werken de `@interface` sectie:

    ```objc
    @interface RegisterClient ()

    @property (strong, nonatomic) NSURLSession* session;
    @property (strong, nonatomic) NSURLSession* endpoint;

    -(void) tryToRegisterWithDeviceToken:(NSData*)token tags:(NSSet*)tags retry:(BOOL)retry
                andCompletion:(void(^)(NSError*))completion;
    -(void) retrieveOrRequestRegistrationIdWithDeviceToken:(NSString*)token
                completion:(void(^)(NSString*, NSError*))completion;
    -(void) upsertRegistrationWithRegistrationId:(NSString*)registrationId deviceToken:(NSString*)token
                tags:(NSSet*)tags andCompletion:(void(^)(NSURLResponse*, NSError*))completion;

    @end
    ```

7. Vervang de `@implementation` sectie in de RegisterClient.m door de volgende code:

    ```objc
    @implementation RegisterClient

    // Globals used by RegisterClient
    NSString *const RegistrationIdLocalStorageKey = @"RegistrationId";

    -(instancetype) initWithEndpoint:(NSString*)Endpoint
    {
        self = [super init];
        if (self) {
            NSURLSessionConfiguration* config = [NSURLSessionConfiguration defaultSessionConfiguration];
            _session = [NSURLSession sessionWithConfiguration:config delegate:nil delegateQueue:nil];
            _endpoint = Endpoint;
        }
        return self;
    }

    -(void) registerWithDeviceToken:(NSData*)token tags:(NSSet*)tags
                andCompletion:(void(^)(NSError*))completion
    {
        [self tryToRegisterWithDeviceToken:token tags:tags retry:YES andCompletion:completion];
    }

    -(void) tryToRegisterWithDeviceToken:(NSData*)token tags:(NSSet*)tags retry:(BOOL)retry
                andCompletion:(void(^)(NSError*))completion
    {
        NSSet* tagsSet = tags?tags:[[NSSet alloc] init];

        NSString *deviceTokenString = [[token description]
            stringByTrimmingCharactersInSet:[NSCharacterSet characterSetWithCharactersInString:@"<>"]];
        deviceTokenString = [[deviceTokenString stringByReplacingOccurrencesOfString:@" " withString:@""]
                                uppercaseString];

        [self retrieveOrRequestRegistrationIdWithDeviceToken: deviceTokenString
            completion:^(NSString* registrationId, NSError *error) {
            NSLog(@"regId: %@", registrationId);
            if (error) {
                completion(error);
                return;
            }

            [self upsertRegistrationWithRegistrationId:registrationId deviceToken:deviceTokenString
                tags:tagsSet andCompletion:^(NSURLResponse * response, NSError *error) {
                if (error) {
                    completion(error);
                    return;
                }

                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*)response;
                if (httpResponse.statusCode == 200) {
                    completion(nil);
                } else if (httpResponse.statusCode == 410 && retry) {
                    [self tryToRegisterWithDeviceToken:token tags:tags retry:NO andCompletion:completion];
                } else {
                    NSLog(@"Registration error with response status: %ld", (long)httpResponse.statusCode);

                    completion([NSError errorWithDomain:@"Registration" code:httpResponse.statusCode
                                userInfo:nil]);
                }

            }];
        }];
    }

    -(void) upsertRegistrationWithRegistrationId:(NSString*)registrationId deviceToken:(NSData*)token
                tags:(NSSet*)tags andCompletion:(void(^)(NSURLResponse*, NSError*))completion
    {
        NSDictionary* deviceRegistration = @{@"Platform" : @"apns", @"Handle": token,
                                                @"Tags": [tags allObjects]};
        NSData* jsonData = [NSJSONSerialization dataWithJSONObject:deviceRegistration
                            options:NSJSONWritingPrettyPrinted error:nil];

        NSLog(@"JSON registration: %@", [[NSString alloc] initWithData:jsonData
                                            encoding:NSUTF8StringEncoding]);

        NSString* endpoint = [NSString stringWithFormat:@"%@/api/register/%@", _endpoint,
                                registrationId];
        NSURL* requestURL = [NSURL URLWithString:endpoint];
        NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
        [request setHTTPMethod:@"PUT"];
        [request setHTTPBody:jsonData];
        NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@",
                                                self.authenticationHeader];
        [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];
        [request setValue:@"application/json" forHTTPHeaderField:@"Content-Type"];

        NSURLSessionDataTask* dataTask = [self.session dataTaskWithRequest:request
            completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
        {
            if (!error)
            {
                completion(response, error);
            }
            else
            {
                NSLog(@"Error request: %@", error);
                completion(nil, error);
            }
        }];
        [dataTask resume];
    }

    -(void) retrieveOrRequestRegistrationIdWithDeviceToken:(NSString*)token
                completion:(void(^)(NSString*, NSError*))completion
    {
        NSString* registrationId = [[NSUserDefaults standardUserDefaults]
                                    objectForKey:RegistrationIdLocalStorageKey];

        if (registrationId)
        {
            completion(registrationId, nil);
            return;
        }

        // request new one & save
        NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/api/register?handle=%@",
                                _endpoint, token]];
        NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
        [request setHTTPMethod:@"POST"];
        NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@",
                                                self.authenticationHeader];
        [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

        NSURLSessionDataTask* dataTask = [self.session dataTaskWithRequest:request
            completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
        {
            NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
            if (!error && httpResponse.statusCode == 200)
            {
                NSString* registrationId = [[NSString alloc] initWithData:data
                    encoding:NSUTF8StringEncoding];

                // remove quotes
                registrationId = [registrationId substringWithRange:NSMakeRange(1,
                                    [registrationId length]-2)];

                [[NSUserDefaults standardUserDefaults] setObject:registrationId
                    forKey:RegistrationIdLocalStorageKey];
                [[NSUserDefaults standardUserDefaults] synchronize];

                completion(registrationId, nil);
            }
            else
            {
                NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                if (error)
                    completion(nil, error);
                else {
                    completion(nil, [NSError errorWithDomain:@"Registration" code:httpResponse.statusCode
                                userInfo:nil]);
                }
            }
        }];
        [dataTask resume];
    }

    @end
    ```

    Deze code implementeert de logica die wordt beschreven in het artikel richtlijnen [registreren vanuit uw back-end](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend) met NSURLSession voor het uitvoeren van REST-aanroepen naar de back-end van uw app en NSUserDefaults voor het lokaal opslaan van de registratie-id die wordt geretourneerd door de Notification hub.

    Deze klasse moet de eigenschap `authorizationHeader` moet worden ingesteld om te kunnen goed werken. Deze eigenschap is ingesteld door de `ViewController` klasse na de aanmelding.

8. In `ViewController.h`, Voeg een `#import` -instructie voor `RegisterClient.h`. Voeg vervolgens een verklaring voor het apparaattoken toe en de verwijzing naar een `RegisterClient` exemplaar de `@interface` sectie:

    ```objc
    #import "RegisterClient.h"

    @property (strong, nonatomic) NSData* deviceToken;
    @property (strong, nonatomic) RegisterClient* registerClient;
    ```

9. ViewController.m, Voeg een verklaring persoonlijke methode in de `@interface` sectie:

    ```objc
    @interface ViewController () <UITextFieldDelegate, NSURLConnectionDataDelegate, NSXMLParserDelegate>

    // create the Authorization header to perform Basic authentication with your app back-end
    -(void) createAndSetAuthenticationHeaderWithUsername:(NSString*)username
                    AndPassword:(NSString*)password;

    @end
    ```

    > [!NOTE]
    > Het volgende codefragment is niet het schema voor een veilige verificatie, vervangt u de implementatie van de `createAndSetAuthenticationHeaderWithUsername:AndPassword:` met uw specifieke verificatiemechanisme die genereert een verificatietoken om te worden verbruikt door de register-client-klasse, bijvoorbeeld OAuth, Active Directory.

10. Klik in de `@implementation` sectie van `ViewController.m`, voeg de volgende code, waarmee de implementatie voor het instellen van de apparaat-token en verificatie-header worden toegevoegd.

    ```objc
    -(void) setDeviceToken: (NSData*) deviceToken
    {
        _deviceToken = deviceToken;
        self.LogInButton.enabled = YES;
    }

    -(void) createAndSetAuthenticationHeaderWithUsername:(NSString*)username
                    AndPassword:(NSString*)password;
    {
        NSString* headerValue = [NSString stringWithFormat:@"%@:%@", username, password];

        NSData* encodedData = [[headerValue dataUsingEncoding:NSUTF8StringEncoding] base64EncodedDataWithOptions:NSDataBase64EncodingEndLineWithCarriageReturn];

        self.registerClient.authenticationHeader = [[NSString alloc] initWithData:encodedData
                                                    encoding:NSUTF8StringEncoding];
    }

    -(BOOL)textFieldShouldReturn:(UITextField *)textField
    {
        [textField resignFirstResponder];
        return YES;
    }
    ```

    U ziet hoe het apparaattoken stellen, stelt u het logboek in knop. Het is omdat de view-controller wordt als onderdeel van de actie aanmelding wordt geregistreerd voor pushmeldingen kunt verzenden met de back-end. Daarom kan niet wilt dat Log In actie toegankelijk totdat het token device juist is ingesteld. U kunt de aanmelding van de push-registratie loskoppelen, zolang de voormalige voordat de laatste gebeurt.

11. In ViewController.m, gebruikt u de volgende codefragmenten voor het implementeren van de actiemethode voor uw **aanmelden** knop en een methode voor het verzenden van het meldingsbericht met behulp van de ASP.NET-back-end.

    ```objc
    - (IBAction)LogInAction:(id)sender {
        // create authentication header and set it in register client
        NSString* username = self.UsernameField.text;
        NSString* password = self.PasswordField.text;

        [self createAndSetAuthenticationHeaderWithUsername:username AndPassword:password];

        __weak ViewController* selfie = self;
        [self.registerClient registerWithDeviceToken:self.deviceToken tags:nil
            andCompletion:^(NSError* error) {
            if (!error) {
                dispatch_async(dispatch_get_main_queue(),
                ^{
                    selfie.SendNotificationButton.enabled = YES;
                    [self MessageBox:@"Success" message:@"Registered successfully!"];
                });
            }
        }];
    }

    - (void)SendNotificationASPNETBackend:(NSString*)pns UsernameTag:(NSString*)usernameTag
                Message:(NSString*)message
    {
        NSURLSession* session = [NSURLSession
            sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration] delegate:nil
            delegateQueue:nil];

        // Pass the pns and username tag as parameters with the REST URL to the ASP.NET backend
        NSURL* requestURL = [NSURL URLWithString:[NSString
            stringWithFormat:@"%@/api/notifications?pns=%@&to_tag=%@", BACKEND_ENDPOINT, pns,
            usernameTag]];

        NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
        [request setHTTPMethod:@"POST"];

        // Get the mock authenticationheader from the register client
        NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@",
            self.registerClient.authenticationHeader];
        [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

        //Add the notification message body
        [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];
        [request setHTTPBody:[message dataUsingEncoding:NSUTF8StringEncoding]];

        // Execute the send notification REST API on the ASP.NET Backend
        NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
            completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
        {
            NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
            if (error || httpResponse.statusCode != 200)
            {
                NSString* status = [NSString stringWithFormat:@"Error Status for %@: %d\nError: %@\n",
                                    pns, httpResponse.statusCode, error];
                dispatch_async(dispatch_get_main_queue(),
                ^{
                    // Append text because all 3 PNS calls may also have information to view
                    [self.sendResults setText:[self.sendResults.text stringByAppendingString:status]];
                });
                NSLog(status);
            }

            if (data != NULL)
            {
                xmlParser = [[NSXMLParser alloc] initWithData:data];
                [xmlParser setDelegate:self];
                [xmlParser parse];
            }
        }];
        [dataTask resume];
    }
    ```

12. Bijwerken van de actie voor de **melding verzenden** knop gebruikt u de back-end van ASP.NET en verzenden naar een PNS ingeschakeld door een switch.

    ```objc
    - (IBAction)SendNotificationMessage:(id)sender
    {
        //[self SendNotificationRESTAPI];
        [self SendToEnabledPlatforms];
    }

    -(void)SendToEnabledPlatforms
    {
        NSString* json = [NSString stringWithFormat:@"\"%@\"",self.notificationMessage.text];

        [self.sendResults setText:@""];

        if ([self.WNSSwitch isOn])
            [self SendNotificationASPNETBackend:@"wns" UsernameTag:self.RecipientField.text Message:json];

        if ([self.GCMSwitch isOn])
            [self SendNotificationASPNETBackend:@"gcm" UsernameTag:self.RecipientField.text Message:json];

        if ([self.APNSSwitch isOn])
            [self SendNotificationASPNETBackend:@"apns" UsernameTag:self.RecipientField.text Message:json];
    }
    ```

13. In de `ViewDidLoad` functie, het volgende toevoegen aan een exemplaar maken van de `RegisterClient` instantie en stel de gemachtigde voor uw tekstvelden.

    ```objc
    self.UsernameField.delegate = self;
    self.PasswordField.delegate = self;
    self.RecipientField.delegate = self;
    self.registerClient = [[RegisterClient alloc] initWithEndpoint:BACKEND_ENDPOINT];
    ```

14. Nu beschikbaar in de `AppDelegate.m`, verwijdert u alle inhoud van de methode `application:didRegisterForPushNotificationWithDeviceToken:` en vervang deze door de volgende (Zorg ervoor dat de weergavecontroller de meest recente apparaattoken opgehaald uit de APNs bevat):

    ```objc
    // Add import to the top of the file
    #import "ViewController.h"

    - (void)application:(UIApplication *)application
                didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
    {
        ViewController* rvc = (ViewController*) self.window.rootViewController;
        rvc.deviceToken = deviceToken;
    }
    ```

15. Ten slotte in `AppDelegate.m`, zorg ervoor dat u hebt de volgende methode:

    ```objc
    - (void)application:(UIApplication *)application didReceiveRemoteNotification: (NSDictionary *)userInfo {
        NSLog(@"%@", userInfo);
        [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
    }
    ```

## <a name="test-the-application"></a>De toepassing testen

1. In XCode, moet u de app uitvoeren op een fysiek iOS-apparaat (push-meldingen niet in de simulator werken).
2. In de iOS-app UI, voert u dezelfde waarde voor zowel gebruikersnaam en wachtwoord. Klik vervolgens op **aanmelden**.

    ![iOS testen toepassing][2]

3. U ziet een melding van de registratie van geslaagde pop-upvenster. Klik op **OK**.

    ![iOS testen melding weergegeven][3]

4. In de **ontvanger gebruikersnaamtag* tekst en voer de naam label gebruikt in combinatie met de registratie van een ander apparaat.
5. Geef een bericht en klikt u op **melding verzenden**. Alleen de apparaten waarvoor een registratie met de naam van geadresseerde gebruikerstag ontvangen de melding. Het is alleen naar gebruikers verzonden.

    ![iOS Testmelding gelabelde][4]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u pushmeldingen kunt verzenden naar specifieke gebruikers door een tag te koppelen aan hun registraties. Als u wilt weten hoe u locatiegebaseerde pushmeldingen kunt verzenden, gaat u verder met de volgende zelfstudie: 

> [!div class="nextstepaction"]
>[Locatiegebaseerde pushmeldingen verzenden](notification-hubs-push-bing-spatial-data-geofencing-notification.md)

[1]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-interface.png
[2]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-enter-user-pwd.png
[3]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-registered.png
[4]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-enter-msg.png
