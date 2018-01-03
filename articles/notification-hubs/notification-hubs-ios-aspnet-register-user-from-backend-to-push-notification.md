---
title: De huidige gebruiker voor pushmeldingen registreren met behulp van Web-API | Microsoft Docs
description: Informatie over het aanvragen van registratie voor pushberichten in een iOS-app met Azure Notification Hubs wanneer de registratie wordt uitgevoerd door ASP.NET Web API.
services: notification-hubs
documentationcenter: ios
author: ysxu
manager: erikre
editor: 
ms.assetid: 4e3772cf-20db-4b9f-bb74-886adfaaa65d
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: fd56bb2dd627b31f00363851a4e76484aa382988
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="register-the-current-user-for-push-notifications-by-using-aspnet"></a>De huidige gebruiker voor pushmeldingen registreren met behulp van ASP.NET
> [!div class="op_single_selector"]
> * [iOS](notification-hubs-ios-aspnet-register-user-from-backend-to-push-notification.md)
> 
> 

## <a name="overview"></a>Overzicht
Dit onderwerp leest u hoe u kunt registratie voor pushberichten met Azure Notification Hubs aanvragen wanneer de registratie wordt uitgevoerd door ASP.NET Web API. In dit onderwerp breidt de zelfstudie [meldingen verzenden naar gebruikers met Notification Hubs]. U moet nog hebt voltooid de vereiste stappen in deze zelfstudie voor het maken van de geverifieerde mobiele service. Zie voor meer informatie over de gebruikers waarschuwen scenario [meldingen verzenden naar gebruikers met Notification Hubs].

## <a name="update-your-app"></a>Uw app bijwerken
1. Voeg de volgende onderdelen van de objectbibliotheek in uw MainStoryboard_iPhone.storyboard:
   
   * **Label**: 'Push naar de gebruiker met Notification Hubs'
   * **Label**: 'Omwille van'
   * **Label**: 'Gebruiker'
   * **Tekstveld**: 'Gebruiker'
   * **Label**: 'Password'
   * **Tekstveld**: 'Password'
   * **Knop**: 'Aanmelding'
     
     Op dit moment een storyboard ziet er als volgt:
     
      ![][0]
2. In de editor assistent aansluitingen van de geschakelde besturingselementen maken en ze aanroept, de tekstvelden verbinden met de weergavebesturing (gemachtigde) en maken een **actie** voor de **aanmelding** knop.
   
       ![][1]
   
       Your BreakingNewsViewController.h file should now contain the following code:
   
        @property (weak, nonatomic) IBOutlet UILabel *installationId;
        @property (weak, nonatomic) IBOutlet UITextField *User;
        @property (weak, nonatomic) IBOutlet UITextField *Password;
   
        - (IBAction)login:(id)sender;
3. Maak een klasse met de naam **DeviceInfo**, en kopieer de volgende code naar het gedeelte van de interface van het bestand DeviceInfo.h:
   
        @property (readonly, nonatomic) NSString* installationId;
        @property (nonatomic) NSData* deviceToken;
4. Kopieer de volgende code in de Implementatiesectie van het bestand DeviceInfo.m:
   
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
5. In PushToUserAppDelegate.h, voegt u de volgende eigenschap singleton:
   
        @property (strong, nonatomic) DeviceInfo* deviceInfo;
6. In de **didFinishLaunchingWithOptions** methode in PushToUserAppDelegate.m, voeg de volgende code:
   
        self.deviceInfo = [[DeviceInfo alloc] init];
   
        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];
   
    De eerste regel initialiseert de **DeviceInfo** singleton. De tweede regel begint de registratie voor pushmeldingen, die al aanwezig is dat u al hebt voltooid de [aan de slag met Notification Hubs] zelfstudie.
7. In PushToUserAppDelegate.m, implementeert u de methode **didRegisterForRemoteNotificationsWithDeviceToken** in uw AppDelegate en voeg de volgende code:
   
        self.deviceInfo.deviceToken = deviceToken;
   
    Hiermee stelt u het apparaattoken voor de aanvraag.
   
   > [!NOTE]
   > Op dit moment moet niet er een andere code in deze methode. Als u al een aanroep van de **registerNativeWithDeviceToken** methode die is toegevoegd wanneer u voltooid de [aan de slag met Notification Hubs](/manage/services/notification-hubs/get-started-notification-hubs-ios/) zelfstudie, moet u commentarieer of verwijder deze aanroep.
   > 
   > 
8. In het bestand PushToUserAppDelegate.m, voegt u de volgende handlermethode:
   
   * (leeg) toepassing:(UIApplication *) toepassing didReceiveRemoteNotification:(NSDictionary *) gebruikersgegevens {NSLog (@"% @", gebruikersgegevens);   UIAlertView * waarschuwing = [[UIAlertView toewijzingseenheid] initWithTitle:@"Notification" bericht: [gebruikersgegevens objectForKey:@"inAppMessage"] gemachtigde: nil cancelButtonTitle: @"OK" otherButtonTitles:nil, nil];   [waarschuwing weergeven]; }
   
   Deze methode wordt een waarschuwing weergegeven in de gebruikersinterface wanneer uw app meldingen ontvangt terwijl deze wordt uitgevoerd.
9. Open het bestand PushToUserViewController.m en het toetsenbord retourneren in de volgende implementatie:
   
        - (BOOL)textFieldShouldReturn:(UITextField *)theTextField {
            if (theTextField == self.User || theTextField == self.Password) {
                [theTextField resignFirstResponder];
            }
            return YES;
        }
10. In de **viewDidLoad** methode in het bestand PushToUserViewController.m initialiseren van het label voor de installatie-id als volgt:
    
         DeviceInfo* deviceInfo = [(PushToUserAppDelegate*)[[UIApplication sharedApplication]delegate] deviceInfo];
         Self.installationId.text = deviceInfo.installationId;
11. De volgende eigenschappen op de interface in PushToUserViewController.m toevoegen:
    
        @property (readonly) NSOperationQueue* downloadQueue;
        - (NSString*)base64forData:(NSData*)theData;
12. Voeg de volgende implementatie:
    
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
13. Kopieer de volgende code in de **aanmelding** gemaakt door XCode handler-methode:
    
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
    
    Deze methode opgehaald van een installatie-ID en de kanaal voor pushmeldingen en verzendt, samen met het apparaattype op de geverifieerde Web API-methode die een registratie in Notification Hubs maakt. Deze Web-API is gedefinieerd in [meldingen verzenden naar gebruikers met Notification Hubs].

Nu dat de client-app is bijgewerkt en terugkeren naar de [meldingen verzenden naar gebruikers met Notification Hubs] en bijwerken van de mobiele service om meldingen te verzenden via Notification Hubs.

<!-- Anchors. -->

<!-- Images. -->
[0]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios1.png
[1]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios2.png

<!-- URLs. -->
[meldingen verzenden naar gebruikers met Notification Hubs]: /manage/services/notification-hubs/notify-users-aspnet

[aan de slag met Notification Hubs]: /manage/services/notification-hubs/get-started-notification-hubs-ios
