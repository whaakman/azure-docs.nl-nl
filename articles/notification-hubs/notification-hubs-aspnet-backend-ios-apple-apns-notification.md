---
title: Pushmeldingen aan specifieke gebruikers met Azure Notification Hubs | Microsoft Docs
description: Leer hoe u pushmeldingen kunt verzenden naar specifieke gebruikers met behulp van Azure Notification Hubs.
documentationcenter: ios
author: dimazaid
manager: kpiteira
editor: spelluru
services: notification-hubs
ms.assetid: 1f7d1410-ef93-4c4b-813b-f075eed20082
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 04/13/2018
ms.author: dimazaid
ms.openlocfilehash: 36d70c40e3de7bd38cdfc566da37060cdcea9060
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2018
ms.locfileid: "33777532"
---
# <a name="tutorial-push-notifications-to-specific-users-using-azure-notification-hubs"></a>Zelfstudie: Aan specifieke gebruikers met Azure Notification Hubs Pushmeldingen
[!INCLUDE [notification-hubs-selector-aspnet-backend-notify-users](../../includes/notification-hubs-selector-aspnet-backend-notify-users.md)]

In deze zelfstudie wordt uitgelegd hoe u met Azure Notification Hubs pushmeldingen kunt verzenden naar een specifieke app-gebruiker op een specifiek apparaat. Een ASP.NET WebAPI-back-end wordt gebruikt om clients te verifiëren en het genereren van meldingen, zoals wordt weergegeven in het onderwerp richtlijnen [registreren van uw app back-end](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend).

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
Deze zelfstudie wordt ervan uitgegaan dat u hebt gemaakt en uw notification hub geconfigureerd zoals beschreven in [aan de slag met Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md). Deze zelfstudie is ook de vereiste voor de [(iOS) beveiligen Push](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md) zelfstudie.
Als u Mobile Apps gebruiken als uw back-endservice wilt, raadpleegt u de [Mobile Apps aan de slag met Pushmeldingen](../app-service-mobile/app-service-mobile-ios-get-started-push.md).

[!INCLUDE [notification-hubs-aspnet-backend-notifyusers](../../includes/notification-hubs-aspnet-backend-notifyusers.md)]

## <a name="modify-your-ios-app"></a>Uw iOS-app wijzigen
1. Open de app voor het weergeven van één pagina u hebt gemaakt in de [aan de slag met Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md) zelfstudie.
   
   > [!NOTE]
   > Deze sectie wordt ervan uitgegaan dat uw project is geconfigureerd met een leeg organisatienaam. Als dat niet het geval is, moet u de naam van uw organisatie aan alle klassenamen van de toevoegen aan het begin.
   > 
   > 
2. In de `Main.storyboard`, de onderdelen weergegeven in de schermafbeelding van de objectbibliotheek toe te voegen.
   
    ![][1]
   
   * **Gebruikersnaam**: A UITextField met tijdelijke tekst *gebruikersnaam invoeren*, direct onder de verzenden resultaten labelen en beperkt tot de linker- en rechtermarge en onder het label van de resultaten verzenden.
   * **Wachtwoord**: A UITextField met tijdelijke tekst *wachtwoord invoeren*, direct onder de gebruikersnaam tekst veld en beperkte voor de linker- en rechtermarge en onder het tekstveld gebruikersnaam. Controleer de **tekstinvoer Secure** onder de optie in de Inspector kenmerk *retourneren sleutel*.
   * **Meld u bij**: A UIButton gelabeld direct onder het wachtwoordtekstveld en schakelt u de **ingeschakeld** onder de optie in de Inspector kenmerken *-inhoud*
   * **WNS**: Label en switch kunt u de Windows Notification Service van de melding te verzenden als deze is ingesteld op de hub. Zie de [Windows aan de slag](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) zelfstudie.
   * **GCM**: Label en switch kunt u de melding verzenden naar het Google Cloud Messaging als deze is ingesteld op de hub. Zie [Android aan de slag](notification-hubs-android-push-notification-google-gcm-get-started.md) zelfstudie.
   * **APNS**: Label en switch kunt u de melding verzenden naar de Apple-Platform Notification-Service.
   * **Ontvanger Username:A** UITextField met tijdelijke tekst *ontvanger gebruikersnaam tag*, onmiddellijk onder de GCM labelen en beperkt de linker- en rechtermarge en onder het GCM-label.

    Sommige onderdelen zijn toegevoegd aan de [aan de slag met Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md) zelfstudie.

1. **CTRL** van de onderdelen in de weergave naar ViewController.h slepen en deze nieuwe uitgangen toevoegen.
   
    ```obj-c
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
2. In ViewController.h, voeg de volgende `#define` na uw importinstructies. Vervang de *< Voer uw back-end eindpunt\>*  aanduiding voor items met de doel-URL die u gebruikt voor het implementeren van uw app back-end in de vorige sectie. Bijvoorbeeld *http://you_backend.azurewebsites.net*.
   
    ```obj-c
        #define BACKEND_ENDPOINT @"<Enter Your Backend Endpoint>"
    ```
3. In het project, maak een nieuwe **klasse Raak Cocoa** met de naam **RegisterClient** aan interface met de ASP.NET back-end u hebt gemaakt. Maken van de klasse is overgenomen van `NSObject`. Voeg vervolgens de volgende code in de RegisterClient.h.

    ```obj-c   
        @interface RegisterClient : NSObject
   
        @property (strong, nonatomic) NSString* authenticationHeader;
   
        -(void) registerWithDeviceToken:(NSData*)token tags:(NSSet*)tags
            andCompletion:(void(^)(NSError*))completion;
   
        -(instancetype) initWithEndpoint:(NSString*)Endpoint;
   
        @end
    ```
4. In de `RegisterClient.m`, werken de `@interface` sectie:

    ```obj-c   
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
5. Vervang de `@implementation` sectie in het RegisterClient.m met de volgende code:

    ```obj-c
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

    Deze code implementeert de logica die is beschreven in het artikel richtlijnen [registreren van uw app back-end](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend) back-end van uw app met NSURLSession voor het uitvoeren van REST-aanroepen en NSUserDefaults voor het lokaal opslaan van de registratie-id die wordt geretourneerd door de Notification hub.

    Deze klasse moet de eigenschap **authorizationHeader** goede werking zijn ingesteld. Deze eigenschap is ingesteld door de **ViewController** klasse na de aanmelding.

1. Voeg in ViewController.h, een `#import` -instructie voor RegisterClient.h. Voeg een declaratie voor het apparaattoken vervolgens en verwijzing naar een `RegisterClient` exemplaar de `@interface` sectie:

    ```obj-c   
        #import "RegisterClient.h"
   
        @property (strong, nonatomic) NSData* deviceToken;
        @property (strong, nonatomic) RegisterClient* registerClient;
    ```
2. In ViewController.m, voegt u een persoonlijke methodedeclaratie in de `@interface` sectie:
   
    ```obj-c
        @interface ViewController () <UITextFieldDelegate, NSURLConnectionDataDelegate, NSXMLParserDelegate>
   
        // create the Authorization header to perform Basic authentication with your app back-end
        -(void) createAndSetAuthenticationHeaderWithUsername:(NSString*)username
                        AndPassword:(NSString*)password;
   
        @end
    ```

    > [!NOTE]
    > Het volgende codefragment is niet een beveiligde verificatieschema, vervang de uitvoering van de **createAndSetAuthenticationHeaderWithUsername:AndPassword:** met uw specifieke verificatiemechanisme die genereert geen verificatietoken om te worden verbruikt door het register clientklasse, zoals OAuth, Active Directory.
1. Klik in de `@implementation` sectie van `ViewController.m`, voeg de volgende code, waarmee de implementatie voor het instellen van de apparaat-token en de verificatie-header worden toegevoegd.
   
    ```obj-c
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
   
    U ziet hoe het apparaattoken instellen kunt het logboek in knop. Het is omdat als onderdeel van de actie voor aanmelding, de weergavebesturing zich voor pushmeldingen met de back-end voor de app registreert. Daarom wil niet aanmelden actie niet toegankelijk totdat het apparaattoken correct zijn ingesteld. U kunt loskoppelen van de aanmelding van de push-registratie, zolang de voormalige voordat de laatste gebeurt.
2. In ViewController.m, gebruikt u de volgende codefragmenten voor het implementeren van de actiemethode voor uw **aanmelden** knop en een methode om het meldingsbericht met behulp van de ASP.NET-back-end te verzenden.
   
    ```obj-c
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

1. Bijwerken van de actie voor de **melding verzenden** knop gebruik van de ASP.NET-back-end en verzenden naar een PNS ingeschakeld door een switch.

    ```obj-c
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

1. In de functie **ViewDidLoad**, het volgende exemplaar maken van de RegisterClient-instantie en instellen van de gemachtigde voor de velden toevoegen.
   
    ```obj-c
       self.UsernameField.delegate = self;
       self.PasswordField.delegate = self;
       self.RecipientField.delegate = self;
       self.registerClient = [[RegisterClient alloc] initWithEndpoint:BACKEND_ENDPOINT];
    ```
2. Nu in **AppDelegate.m**, verwijdert u de inhoud van de methode `application:didRegisterForPushNotificationWithDeviceToken:` en vervang deze door het volgende om ervoor te zorgen dat de weergavebesturing de meest recente apparaattoken opgehaald uit APNs bevat:
   
    ```obj-c
       // Add import to the top of the file
       #import "ViewController.h"
   
       - (void)application:(UIApplication *)application
                   didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
       {
           ViewController* rvc = (ViewController*) self.window.rootViewController;
           rvc.deviceToken = deviceToken;
       }
    ```
3. Ten slotte in **AppDelegate.m**, zorg ervoor dat u hebt de volgende methode:

    ```obj-c   
       - (void)application:(UIApplication *)application didReceiveRemoteNotification: (NSDictionary *)userInfo {
           NSLog(@"%@", userInfo);
           [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
       }
    ```

## <a name="test-the-application"></a>De toepassing testen
1. Voer de app op een fysiek iOS-apparaat (push notifications niet in de simulator werkt) in XCode.
2. Geef dezelfde waarde voor zowel de gebruikersnaam en wachtwoord in de iOS-app gebruikersinterface. Klik vervolgens op **aanmelden**.
   
    ![][2]
3. U ziet een melding van de registratie geslaagd pop-upvenster. Klik op **OK**.
   
    ![][3]
4. In de **ontvanger gebruikersnaam tag* tekst en voer de code van de gebruiker-name gebruikt met de registratie van een ander apparaat.
5. Geef een bericht en klik op **melding verzenden**. Alleen de apparaten waarvoor een registratie met de code van de geadresseerde gebruiker name ontvangen de melding. Het is alleen naar gebruikers verzonden.
   
    ![][4]

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd hoe u pushmeldingen kunt verzenden naar specifieke gebruikers door een tag te koppelen aan hun registraties. Als u wilt weten hoe u locatiegebaseerde pushmeldingen kunt verzenden, gaat u verder met de volgende zelfstudie: 

> [!div class="nextstepaction"]
>[Locatiegebaseerde pushmeldingen verzenden](notification-hubs-push-bing-spartial-data-geofencing-notification.md)


[1]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-interface.png
[2]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-enter-user-pwd.png
[3]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-registered.png
[4]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-enter-msg.png
