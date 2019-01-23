---
title: De huidige gebruiker voor pushmeldingen registreren met behulp van Web-API | Microsoft Docs
description: Leer hoe u registratie voor pushberichten in een iOS-app met Azure Notification Hubs aanvragen wanneer de registratie wordt uitgevoerd door de ASP.NET-Web-API.
services: notification-hubs
documentationcenter: ios
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: 4e3772cf-20db-4b9f-bb74-886adfaaa65d
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: 67baa204d50d1319559abcc58e0ae00e1810ebaf
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54452646"
---
# <a name="register-the-current-user-for-push-notifications-by-using-aspnet"></a>De huidige gebruiker voor pushmeldingen registreren met behulp van ASP.NET

> [!div class="op_single_selector"]
> * [iOS](notification-hubs-ios-aspnet-register-user-from-backend-to-push-notification.md)

## <a name="overview"></a>Overzicht

Dit onderwerp ziet u hoe u registratie voor pushberichten met Azure Notification Hubs aanvragen wanneer de registratie wordt uitgevoerd door de ASP.NET-Web-API. In dit gedeelte de zelfstudie is een vervolg [meldingen verzenden naar gebruikers met Notification Hubs]. U moet al hebt voltooid de vereiste stappen in deze zelfstudie om te maken van de geverifieerde mobiele service. Zie voor meer informatie over de gebruikers waarschuwen scenario [meldingen verzenden naar gebruikers met Notification Hubs].

## <a name="update-your-app"></a>Uw app bijwerken

1. Voeg de volgende onderdelen van de objectbibliotheek in uw MainStoryboard_iPhone.storyboard:

   * **Label**: "Push naar de gebruiker met Notification Hubs"
   * **Label**: "InstallationId"
   * **Label**: "User"
   * **Tekstveld**: "User"
   * **Label**: 'Wachtwoord'
   * **Tekstveld**: 'Wachtwoord'
   * **Knop**: "Login"

    Op dit moment uw storyboard ziet er als volgt uit:

    ![][0]

2. In de editor assistent uitgangen van de geschakelde besturingselementen maken en ze aanroept, verbinding maken met de velden met de View-Controller (gemachtigde) en maken een **actie** voor de **aanmelding** knop.

    ![][1]

    Het bestand BreakingNewsViewController.h moet nu bevatten de volgende code:

    ```objc
    @property (weak, nonatomic) IBOutlet UILabel *installationId;
    @property (weak, nonatomic) IBOutlet UITextField *User;
    @property (weak, nonatomic) IBOutlet UITextField *Password;

    - (IBAction)login:(id)sender;
    ```
3. Maak een klasse met de naam `DeviceInfo`, en kopieer de volgende code in de sectie van de interface van het bestand DeviceInfo.h:

    ```objc
    @property (readonly, nonatomic) NSString* installationId;
    @property (nonatomic) NSData* deviceToken;
    ```
4. Kopieer de volgende code in de Implementatiesectie van het bestand DeviceInfo.m:

    ```objc
    @synthesize installationId = _installationId;

    - (id)init {
        if (!(self = [super init]))
            return nil;

        NSUserDefaults *defaults = [NSUserDefaults standardUserDefaults];
        _installationId = [defaults stringForKey:@"PushToUserInstallationId"];
        if(!_installationId) {
            CFUUIDRef newUUID = CFUUIDCreate(kCFAllocatorDefault);
            _installationId = (__bridge_transfer NSString *)CFUUIDCreateString(kCFAllocatorDefault, newUUID);
            CFRelease(newUUID);

            //store the install ID so we don't generate a new one next time
            [defaults setObject:_installationId forKey:@"PushToUserInstallationId"];
            [defaults synchronize];
        }

        return self;
    }

    - (NSString*)getDeviceTokenInHex {
        const unsigned *tokenBytes = [[self deviceToken] bytes];
        NSString *hexToken = [NSString stringWithFormat:@"%08X%08X%08X%08X%08X%08X%08X%08X",
                                ntohl(tokenBytes[0]), ntohl(tokenBytes[1]), ntohl(tokenBytes[2]),
                                ntohl(tokenBytes[3]), ntohl(tokenBytes[4]), ntohl(tokenBytes[5]),
                                ntohl(tokenBytes[6]), ntohl(tokenBytes[7])];
        return hexToken;
    }
    ```

5. In PushToUserAppDelegate.h, voegt u de volgende eigenschap singleton toe:

    ```objc
    @property (strong, nonatomic) DeviceInfo* deviceInfo;
    ```
6. In de `didFinishLaunchingWithOptions` methode in PushToUserAppDelegate.m, voeg de volgende code toe:

    ```objc
    self.deviceInfo = [[DeviceInfo alloc] init];

    [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];
    ```

    De eerste regel initialiseert de `DeviceInfo` singleton. De tweede regel begint de registratie voor pushmeldingen, die al aanwezig is als u al hebt voltooid de [aan de slag met Notification Hubs] zelfstudie.
7. In PushToUserAppDelegate.m, implementeert u de methode `didRegisterForRemoteNotificationsWithDeviceToken` in uw AppDelegate en voeg de volgende code toe:

    ```objc
    self.deviceInfo.deviceToken = deviceToken;
    ```

    Hiermee stelt u het apparaattoken voor de aanvraag.

   > [!NOTE]
   > Op dit moment mogen er geen andere codes die bij deze methode. Als u al een aanroep naar de `registerNativeWithDeviceToken` methode die is toegevoegd wanneer u voltooid de [aan de slag met Notification Hubs](notification-hubs-ios-apple-push-notification-apns-get-started.md) zelfstudie, moet u Voeg een opmerking of verwijder deze aanroep.

8. In de `PushToUserAppDelegate.m` bestand, voeg de volgende handlermethode toe:

    ```objc
    * (void) application:(UIApplication *) application didReceiveRemoteNotification:(NSDictionary *)userInfo {
       NSLog(@"%@", userInfo);
       UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
                             [userInfo objectForKey:@"inAppMessage"] delegate:nil cancelButtonTitle:
                             @"OK" otherButtonTitles:nil, nil];
       [alert show];
    }
    ```

    Deze methode wordt een waarschuwing weergegeven in de gebruikersinterface wanneer uw app meldingen, ontvangt terwijl deze wordt uitgevoerd.

9. Open de `PushToUserViewController.m` bestand en het toetsenbord retourneren in de volgende implementatie:

    ```objc
    - (BOOL)textFieldShouldReturn:(UITextField *)theTextField {
        if (theTextField == self.User || theTextField == self.Password) {
            [theTextField resignFirstResponder];
        }
        return YES;
    }
    ```

10. In de `viewDidLoad` methode in de `PushToUserViewController.m` bestand, initialiseren het `installationId` labelen als volgt:

    ```objc
    DeviceInfo* deviceInfo = [(PushToUserAppDelegate*)[[UIApplication sharedApplication]delegate] deviceInfo];
    Self.installationId.text = deviceInfo.installationId;
    ```

11. Voeg de volgende eigenschappen in de interface in `PushToUserViewController.m`:

    ```objc
    @property (readonly) NSOperationQueue* downloadQueue;
    - (NSString*)base64forData:(NSData*)theData;
    ```

12. Vervolgens voegt u de volgende implementatie:

    ```objc
    - (NSOperationQueue *)downloadQueue {
        if (!_downloadQueue) {
            _downloadQueue = [[NSOperationQueue alloc] init];
            _downloadQueue.name = @"Download Queue";
            _downloadQueue.maxConcurrentOperationCount = 1;
        }
        return _downloadQueue;
    }

    // base64 encoding
    - (NSString*)base64forData:(NSData*)theData
    {
        const uint8_t* input = (const uint8_t*)[theData bytes];
        NSInteger length = [theData length];

        static char table[] = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/=";

        NSMutableData* data = [NSMutableData dataWithLength:((length + 2) / 3) * 4];
        uint8_t* output = (uint8_t*)data.mutableBytes;

        NSInteger i;
        for (i=0; i < length; i += 3) {
            NSInteger value = 0;
            NSInteger j;
            for (j = i; j < (i + 3); j++) {
                value <<= 8;

                if (j < length) {
                    value |= (0xFF & input[j]);
                }
            }

            NSInteger theIndex = (i / 3) * 4;
            output[theIndex + 0] =                    table[(value >> 18) & 0x3F];
            output[theIndex + 1] =                    table[(value >> 12) & 0x3F];
            output[theIndex + 2] = (i + 1) < length ? table[(value >> 6)  & 0x3F] : '=';
            output[theIndex + 3] = (i + 2) < length ? table[(value >> 0)  & 0x3F] : '=';
        }

        return [[NSString alloc] initWithData:data encoding:NSASCIIStringEncoding];
    }
    ```

13. Kopieer de volgende code in de `login` handlermethode die zijn gemaakt door XCode:

    ```objc
    DeviceInfo* deviceInfo = [(PushToUserAppDelegate*)[[UIApplication sharedApplication]delegate] deviceInfo];

    // build JSON
    NSString* json = [NSString stringWithFormat:@"{\"platform\":\"ios\", \"instId\":\"%@\", \"deviceToken\":\"%@\"}", deviceInfo.installationId, [deviceInfo getDeviceTokenInHex]];

    // build auth string
    NSString* authString = [NSString stringWithFormat:@"%@:%@", self.User.text, self.Password.text];

    NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:[NSURL URLWithString:@"http://nhnotifyuser.azurewebsites.net/api/register"]];
    [request setHTTPMethod:@"POST"];
    [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];
    [request addValue:[@([json lengthOfBytesUsingEncoding:NSUTF8StringEncoding]) description] forHTTPHeaderField:@"Content-Length"];
    [request addValue:@"application/json" forHTTPHeaderField:@"Content-Type"];
    [request addValue:[NSString stringWithFormat:@"Basic %@",[self base64forData:[authString dataUsingEncoding:NSUTF8StringEncoding]]] forHTTPHeaderField:@"Authorization"];

    // connect with POST
    [NSURLConnection sendAsynchronousRequest:request queue:[self downloadQueue] completionHandler:^(NSURLResponse* response, NSData* data, NSError* error) {
        // add UIAlert depending on response.
        if (error != nil) {
            NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*)response;
            if ([httpResponse statusCode] == 200) {
                UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Back-end registration" message:@"Registration successful" delegate:nil cancelButtonTitle: @"OK" otherButtonTitles:nil, nil];
                [alert show];
            } else {
                NSLog(@"status: %ld", (long)[httpResponse statusCode]);
            }
        } else {
            NSLog(@"error: %@", error);
        }
    }];
    ```

    Deze methode wordt zowel een installatie-ID en het kanaal voor pushmeldingen en verzendt dit, samen met het apparaattype naar de geverifieerde Web-API-methode die u een registratie in Notification Hubs maakt. Deze Web-API is gedefinieerd in [meldingen verzenden naar gebruikers met Notification Hubs].

Nu dat de client-app is bijgewerkt, Ga terug naar de [meldingen verzenden naar gebruikers met Notification Hubs] en bijwerken van de mobiele service om meldingen te verzenden via Notification Hubs.

<!-- Anchors. -->

<!-- Images. -->
[0]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios1.png
[1]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios2.png

<!-- URLs. -->
[Meldingen verzenden naar gebruikers met Notification Hubs]: notification-hubs-aspnet-backend-ios-apple-apns-notification.md
[Aan de slag met Notification Hubs]: notification-hubs-ios-apple-push-notification-apns-get-started.md
