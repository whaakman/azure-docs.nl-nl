---
title: Pushmeldingen naar Swift iOS-apps met behulp van Azure Notification Hubs | Microsoft Docs
description: Leer hoe u pushmeldingen naar Swift iOS-apps met behulp van Azure Notification Hubs.
services: notification-hubs
documentationcenter: ios
author: mikeparker104
manager: patniko
editor: spelluru
ms.assetid: 4e3772cf-20db-4b9f-bb74-886adfaaa65d
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 05/21/2019
ms.author: miparker
ms.openlocfilehash: 64a05c6cbd412953ae10b31efc1d141c9cefd062
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/21/2019
ms.locfileid: "65994764"
---
# <a name="tutorial-push-notifications-to-swift-ios-apps-using-the-notification-hubs-rest-api"></a>Zelfstudie: Pushmeldingen verzenden naar Swift iOS-apps met behulp van de Notification Hubs REST-API

> [!div class="op_single_selector"]
> * [Objective-C](notification-hubs-ios-apple-push-notification-apns-get-started.md)
> * [Swift](notification-hubs-ios-push-notifications-swift-apps-get-started.md)

In deze zelfstudie gebruikt u Azure Notification Hubs pushmeldingen verzendt naar een iOS Swift-gebaseerde toepassing met de [REST-API](/rest/api/notificationhubs/). U maakt een lege iOS-app die pushmeldingen ontvangt met de [Apple Push Notification Service (APNs)](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html#//apple_ref/doc/uid/TP40008194-CH8-SW1).

In deze zelfstudie voert u de volgende stappen uit:

> [!div class="checklist"]
> * Het bestand met de aanvraag voor certificaatondertekening genereren
> * Uw app registreren voor pushmeldingen
> * Een inrichtingsprofiel voor de app maken
> * Een Notification Hub maken
> * De notification hub configureren met de APNS-gegevens
> * Uw iOS-app verbinden met Notification Hubs
> * Testen van de oplossing

## <a name="prerequisites"></a>Vereisten
Als u wilt volgen, hebt u het volgende nodig:

- Doorloop [Azure Notification Hubs-overzicht](notification-hubs-push-notification-overview.md) als u niet bekend bent met de service. 
- Meer informatie over rapporten en -installatie: [Registratiebeheer](notification-hubs-push-notification-registration-management.md)
- Een actief [Apple Developer-Account](https://developer.apple.com) 
- Een Mac met Xcode, samen met een geldige developer-certificaat geïnstalleerd in uw sleutelketen
- Een fysieke iPhone-apparaat die u kunt uitvoeren en fouten opsporen met (is het niet mogelijk om pushmeldingen te testen met de simulator)
- Uw fysieke iPhone-apparaat is geregistreerd de [Apple-Portal](https://developer.apple.com) en is gekoppeld aan uw certificaat
- Een [Azure-abonnement](https://portal.azure.com) waar u kunt maken en beheren van resources

Het moet mogelijk zijn om mee te maken van dit voorbeeld eerste principes zonder eerdere ervaring. Vertrouwd zijn met de volgende concepten zijn echter nuttig zijn:

- Het bouwen van iOS-apps met Xcode en Swift
- Configureren van een [Azure Notification Hub](notification-hubs-ios-apple-push-notification-apns-get-started.md) voor iOS
- Vertrouwd zijn met de [Apple Developer-Portal](https://developer.apple.com) en de [Azure-portal](https://portal.azure.com)

> [!NOTE]
> De notification hub wordt geconfigureerd voor het gebruik van de *Sandbox* alleen verificatiemodus. U moet met deze verificatiemodus niet gebruiken voor productieworkloads.

[!INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

## <a name="connect-your-ios-app-to-notification-hubs"></a>Uw iOS-app verbinden met Notification Hubs
In deze sectie bouwt u de iOS-app waarmee de verbinding met de notification hub maken.  

### <a name="create-an-ios-project"></a>Een iOS-project maken
1. In **Xcode**, maak een nieuw iOS-project en selecteer de **enkele toepassingsweergave** sjabloon.
2. Bij het instellen van de opties voor het nieuwe project, als volgt te werk:
    1. Gebruik dezelfde **Productnaam** (dat wil zeggen, **PushDemo**) en **organisatie-id** (dat wil zeggen, `com.<organization>`) dat u hebt gebruikt tijdens de **bundel Id** is ingesteld in de **Apple Developer-Portal**. 
    2. Kies de **Team** die de **App-ID** is ingesteld voor.
    3. Stel de **taal** naar **Swift**.
    4. Klik op **Volgende**
3. Maak een nieuwe map met de naam **SupportingFiles**.
4. Maak een nieuwe **plist** bestand met de naam **devsettings.plist** onder de **SupportingFiles** map. Zorg ervoor dat u deze map toevoegen die uw **gitignore** bestand, zodat deze niet toegewezen is als u werkt met een **git-repo**. In een productie-app, wilt u waarschijnlijk worden voorwaardelijk instellen van deze geheime gegevens als onderdeel van een geautomatiseerd buildproces. Het wordt echter niet behandeld als onderdeel van deze procedure.
5. Update **devsettings.plist** om op te nemen van de volgende configuratie-items (met behulp van uw eigen waarden uit de **Notification Hub** u ingericht):

   | Sleutel                            | Type                     | Value                     |               
   |--------------------------------| -------------------------| --------------------------|
   | notificationHubKey             | String                   | \<hubKey\>                |
   | notificationHubKeyName         | String                   | \<hubKeyName\>            |
   | notificationHubName            | String                   | \<hubName\>               |
   | notificationHubNamespace       | String                   | \<hubNamespace\>          |
    
   U kunt de vereiste waarden vinden door te navigeren naar de **Notification Hub** resource in de **Azure-portal**. U vindt de **notificationHubName** en de **notificationHubNamespace** waarden in de rechterbovenhoek van de **Essentials** samenvatting binnen de  **Overzicht** pagina.

   ![Overzicht van Notification Hubs Essentials](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/hub-essentials.png)

   U vindt de **notificationHubKeyName** en **notificationHubKey** waarden door te navigeren naar **toegangsbeleid**, te klikken op de desbetreffende **toegang Beleid**. Bijvoorbeeld `DefaultFullSharedAccessSignature`. Kopieer vervolgens uit de **Primary Connection String** de waarde wordt voorafgegaan door `SharedAccessKeyName=` voor `notificationHubKeyName` en de waarde wordt voorafgegaan door `SharedAccessKey=` voor de `notificationHubKey`. De verbindingsreeks moet de volgende indeling:
    
   ```xml
   Endpoint=sb://<namespace>.servicebus.windows.net/;SharedAccessKeyName=<notificationHubKeyName>;SharedAccessKey=<notificationHubKey>
   ```

   Als u wilt Houd het eenvoudig, gebruikt u *DefaultFullSharedAccessSignature* zodat u het token gebruiken kunt om ook meldingen te verzenden. Echter, in de praktijk het `DefaultListenSharedAccessSignature` zou een betere keuze voor situaties waarin u wilt dat alleen om meldingen te ontvangen.       
6. Onder **Projectnavigator**, klikt u op de **projectnaam**, klikt u vervolgens op de **algemene** tabblad
7. Vinden **identiteit**, stelt u de **bundel-id** waarde zodat deze overeenkomt met de die werd gebruikt voor de **App-ID** (van de vorige stap) dat wil zeggen, `'com.<organization>.PushDemo'`
8. Zoek **ondertekening**, controleer vervolgens of u de juiste **Team** voor uw **Apple Developer-Account** (het account waarmee u uw certificaten en -profielen hebt gemaakt ouder).  **Xcode** moet automatisch Haal de juiste **Inrichtingsprofiel** op basis van de **bundel-id**. Als u niet de nieuwe ziet **Inrichtingsprofiel**, vernieuw de profielen voor het **identiteit voor ondertekening van** (*Xcode > Voorkeuren > Account > Details weergeven*). Te klikken op de **identiteit voor ondertekening van**, vervolgens te klikken op de **vernieuwen** knop in de rechterbenedenhoek de profielen moet downloaden.
9. Selecteer de **mogelijkheden** tabblad en zorg ervoor dat **pushmeldingen** zijn ingeschakeld.
10. Open uw **AppDelegate.swift** bestand voor het implementeren van de *UNUserNotificationCenterDelegate* -protocol en voeg de volgende code toe aan het begin van de klasse:
    
    ```swift
    @UIApplicationMain
    class AppDelegate: UIResponder, UIApplicationDelegate, UNUserNotificationCenterDelegate {
        
        ...

        var configValues: NSDictionary?
        var notificationHubNamespace : String?
        var notificationHubName : String?
        var notificationHubKeyName : String?
        var notificationHubKey : String?
        let tags = ["12345"]
        let genericTemplate = PushTemplate(withBody: "{\"aps\":{\"alert\":\"$(message)\"}}")
        
        ...
    }
    ```

    Deze leden gaat u later gebruiken. De *tags* en *genericTemplate* wordt gebruikt als onderdeel van de registratie. Zie voor meer informatie over labels [labels voor registraties](notification-hubs-tags-segment-push-message.md) en [sjabloonregistraties](notification-hubs-templates-cross-platform-push-messages.md).
 
11. Voeg in hetzelfde bestand de volgende code in de *didFinishLaunchingWithOptions* functie:

    ```swift
    if let path = Bundle.main.path(forResource: "devsettings", ofType: "plist") {
        if let configValues = NSDictionary(contentsOfFile: path) {
            self.notificationHubNamespace = configValues["notificationHubNamespace"] as? String
            self.notificationHubName = configValues["notificationHubName"] as? String
            self.notificationHubKeyName = configValues["notificationHubKeyName"] as? String
            self.notificationHubKey = configValues["notificationHubKey"] as? String
        }
    }
        
    if #available(iOS 10.0, *){
        UNUserNotificationCenter.current().delegate = self
        UNUserNotificationCenter.current().requestAuthorization(options: [.alert, .sound, .badge]) {
            (granted, error) in
                
            if (granted)
            {
                DispatchQueue.main.async {
                    application.registerForRemoteNotifications()
                }
            }
        }
    }

    return true
    ```

    Deze code haalt de instellingswaarden van **devsettings.plist**, stelt de **AppDelegate** klasse als de *UNUserNotificationCenter* delegeren, aanvragen autorisatie voor pushmeldingen te verzenden en gesprekken *registerForRemoteNotifications*.
    
    Deze om eenvoudig te houden, de code ondersteunt **iOS 10 en hoger alleen**. U kunt de ondersteuning voor oudere versies van het besturingssysteem toevoegen door voorwaardelijk met behulp van de desbetreffende API's en benaderingen net zoals u normaal zou doen.

12. Voeg in hetzelfde bestand de volgende functies:

    ```swift
    func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
        let installationId = (UIDevice.current.identifierForVendor?.description)!
        let pushChannel = deviceToken.reduce("", {$0 + String(format: "%02X", $1)})
    }

    func showAlert(withText text : String) {
        let alertController = UIAlertController(title: "PushDemo", message: text, preferredStyle: UIAlertControllerStyle.alert)
        alertController.addAction(UIAlertAction(title: "OK", style: UIAlertActionStyle.default,handler: nil))
        self.window?.rootViewController?.present(alertController, animated: true, completion: nil)
    }
    ```

    De code wordt gebruikgemaakt van de *installationId* en *pushChannel* waarden om te registreren bij de **Notification Hub**. In dit geval u *UIDevice.current.identifierForVendor* ons voorziet een unieke waarde om te bepalen van het apparaat en formatteer de *deviceToken* ons voorziet de gewenste *pushChannel* waarde. De *showAlert* functie is eenvoudig om weer te geven berichttekst voor demonstratiedoeleinden te gebruiken.

13. Klik in **AppDelegate.swift**, toevoegen de *willPresent* en *didReceive* **UNUserNotificationCenterDelegate** functioneert een waarschuwing weergeven wanneer de melding ontvangen wanneer de app wordt uitgevoerd in de voorgrond en achtergrond respectievelijk
    
    ```swift
    @available(iOS 10.0, *)
    func userNotificationCenter(_ center: UNUserNotificationCenter, 
        willPresent notification: UNNotification, 
        withCompletionHandler completionHandler: @escaping (UNNotificationPresentationOptions) -> Void) {
        showAlert(withText: notification.request.content.body)
    }
    
    @available(iOS 10.0, *)
    func userNotificationCenter(_ center: UNUserNotificationCenter, 
        didReceive response: UNNotificationResponse, 
        withCompletionHandler completionHandler: @escaping () -> Void) {
        showAlert(withText: response.notification.request.content.body)
    }
    ```    

14. Voeg afdrukken instructies toe aan de onderkant van de *didRegisterForRemoteNotificationsWithDeviceToken* functie om te controleren of *installationId* en *pushChannel* worden toegewezen waarden
15. Mappen maken (**modellen**, **Services**, en **hulpprogramma's voor**) voor de fundamentele onderdelen die u zullen aan het project later toevoegen hierna
16. Controleer of het project wordt gemaakt en wordt uitgevoerd op een fysiek apparaat (push notifications kunnen niet worden getest met behulp van de simulator)

### <a name="create-models"></a>Modellen maken
In deze stap maakt u een set van modellen om weer te geven de [Notification Hubs REST API](/rest/api/notificationhubs/) nettoladingen en voor het opslaan van de vereiste **SAS-token** gegevens.


1.  Voeg een nieuw swift-bestand naar de **modellen** met de naam **PushTemplate.swift**. Dit model biedt een struct die vertegenwoordigt de **hoofdtekst** van een afzonderlijke sjabloon als onderdeel van de **DeviceInstallation** nettolading:
    
    ```swift
    import Foundation

    struct PushTemplate : Codable {
        let body : String
    
        init(withBody body : String) {
            self.body = body
        }
    }
    ```

2. Voeg een nieuw swift-bestand naar de **modellen** map met de naam **DeviceInstallation.swift**. Dit bestand definieert een struct die vertegenwoordigt de payload voor het maken of bijwerken van een **Apparaatinstallatie**. Voeg de volgende code naar het bestand:
    
    ```swift
    import Foundation

    struct DeviceInstallation : Codable {
        let installationId : String
        let pushChannel : String
        let platform : String = "apns"
        var tags : [String]
        var templates : Dictionary<String, PushTemplate>
    
        init(withInstallationId installationId : String, andPushChannel pushChannel : String) {
            self.installationId = installationId
            self.pushChannel = pushChannel
            self.tags = [String]()
            self.templates = Dictionary<String, PushTemplate>()
        }
    }
    ```

3.  Voeg een nieuw swift bestand onder **modellen** met de naam **TokenData.swift**. Dit model wordt gebruikt voor het opslaan van een **SAS-token** samen met de vervaldatum

    ```swift
    import Foundation

    struct TokenData {
    
        let token : String
        let expiration : Int

        init(withToken token : String, andTokenExpiration expiration : Int) {
            self.token = token
            self.expiration = expiration
        }
    }
    ```

### <a name="generate-a-sas-token"></a>Een SAS-token genereren
**Notification Hubs** dezelfde beveiliging-infrastructuur als **Azure Service Bus**. Voor het aanroepen van de REST-API, moet u [programmatisch genereert een SAS-token](/rest/api/eventhub/generate-sas-token) die kunnen worden gebruikt de **autorisatie** -header van de aanvraag.  

Het resulterende token niet in de volgende indeling: 

```xml
SharedAccessSignature sig=<UrlEncodedSignature>&se=<ExpiryEpoch>&skn=<KeyName>&sr=<UrlEncodedResourceUri>
```

Het proces zelf omvat de dezelfde zes belangrijke stappen:  

1.  De vervaldatum in Computing [UNIX epoche-tijd](https://en.wikipedia.org/wiki/Unix_time) indeling (seconden verstreken sinds 00:00:00 UTC 1 januari 1970)
2.  Opmaak de **ResourceUrl** (dat staat voor de resource die u probeert te openen, dat wil zeggen, `'https://\<namespace\>.servicebus.windows.net/\<hubName\>'`) dus is het percentage gecodeerd en kleine letters
3.  Voorbereiden van de **StringToSign**, die bestaat uit `'\<**UrlEncodedResourceUrl**\>\n\<**ExpiryEpoch**\>'`
4.  Computers (en de base 64-codering) de **handtekening** met behulp van de **HMAC-SHA256** van de **StringToSign** waarde met de **sleutel** deel uitmaakt van de **Connection String** (voor de desbetreffende **autorisatieregel**)
5.  De base 64 gecodeerde opmaak **handtekening** dus is het percentage gecodeerd
6.  Maken van de **token** in de verwachte indeling met de **UrlEncodedSignature**, **ExpiryEpoch**, **KeyName**, en **UrlEncodedResourceUrl** waarden

Zie de [documentatie voor Azure Service Bus](../service-bus-messaging/service-bus-sas.md) voor een uitgebreider overzicht van **Shared Access Signature** en hoe deze wordt gebruikt door **Azure Service Bus** en  **Notification Hubs**.

Voor de doeleinden van dit snelle voorbeeld, gaat u gebruik van Apple open-source **CommonCrypto** bibliotheek om te helpen bij de hash van de handtekening. Een C-bibliotheek is, is het niet toegankelijk in Swift out-of-the-box. Echter, kunt u deze beschikbaar maken met behulp van een bridging header. Toevoegen en configureren van de bridging-header:

1. In **Xcode**, gaat u naar **bestand**, klikt u vervolgens **nieuw**, klikt u vervolgens **bestand** en selecteer **headerbestand** naam *'BridgingHeader.h'*
2. Bewerk het bestand te importeren **CommonHMAC**

    ```swift
    #import <CommonCrypto/CommonHMAC.h>

    #ifndef BridgingHeader_h
    #define BridgingHeader_h


    #endif /* BridgingHeader_h */
    ```

3. Bijwerken van het doel **Build Settings** om te verwijzen naar de bridging-header. Open de **gebouw instellingen** tabblad en schuif omlaag naar de **Swift Compiler** sectie. Zorg ervoor dat de **installeren Objective-C compatibiliteit Header** optie ingesteld op **Ja** en voert u het bestandspad naar onze bridging header in de **Objective-C bridging Header**   dat wil zeggen de optie `'\<ProjectName\>/BridgingHeader.h'`. Als u deze opties niet vinden, controleert u of u hebt de **alle** weergave die is geselecteerd (in plaats van **Basic** of **aangepaste**).
    
   Er zijn veel van derden opensource-wrapper bibliotheken beschikbaar, waardoor met behulp van **CommonCrypto** iets eenvoudiger. Het ligt buiten het bereik van dit artikel.

4. Voeg een nieuw Swift bestand onder de **hulpprogramma's voor** map met de naam **TokenUtility.swift** en voeg de volgende code toe:

   ```swift
   import Foundation

   struct TokenUtility {    
        typealias Context = UnsafeMutablePointer<CCHmacContext>
    
        static func getSasToken(forResourceUrl resourceUrl : String, withKeyName keyName : String, andKey key : String, andExpiryInSeconds expiryInSeconds : Int = 3600) -> TokenData {
            let expiry = (Int(NSDate().timeIntervalSince1970) + expiryInSeconds).description
            let encodedUrl = urlEncodedString(withString: resourceUrl)
            let stringToSign = "\(encodedUrl)\n\(expiry)"
            let hashValue = sha256HMac(withData: stringToSign.data(using: .utf8)!, andKey: key.data(using: .utf8)!)
            let signature = hashValue.base64EncodedString(options: .init(rawValue: 0))
            let encodedSignature = urlEncodedString(withString: signature)
            let sasToken = "SharedAccessSignature sr=\(encodedUrl)&sig=\(encodedSignature)&se=\(expiry)&skn=\(keyName)"
            let tokenData = TokenData(withToken: sasToken, andTokenExpiration: expiryInSeconds)
        
            return tokenData
        }
    
        private static func sha256HMac(withData data : Data, andKey key : Data) -> Data {
            let context = Context.allocate(capacity: 1)
            CCHmacInit(context, CCHmacAlgorithm(kCCHmacAlgSHA256), (key as NSData).bytes, size_t((key as NSData).length))
            CCHmacUpdate(context, (data as NSData).bytes, (data as NSData).length)
            var hmac = Array<UInt8>(repeating: 0, count: Int(CC_SHA256_DIGEST_LENGTH))
            CCHmacFinal(context, &hmac)
        
            let result = NSData(bytes: hmac, length: hmac.count)
            context.deallocate()
        
            return result as Data
        }
    
        private static func urlEncodedString(withString stringToConvert : String) -> String {
            var encodedString = ""
            let sourceUtf8 = (stringToConvert as NSString).utf8String
            let length = strlen(sourceUtf8)
        
            let charArray: [Character] = [ ".", "-", "_", "~", "a", "z", "A", "Z", "0", "9"]
            let asUInt8Array = String(charArray).utf8.map{ Int8($0) }
        
            for i in 0..<length {
                let currentChar = sourceUtf8![i]
            
                if (currentChar == asUInt8Array[0] || currentChar == asUInt8Array[1] || currentChar == asUInt8Array[2] || currentChar == asUInt8Array[3] ||
                    (currentChar >= asUInt8Array[4] && currentChar <= asUInt8Array[5]) ||
                    (currentChar >= asUInt8Array[6] && currentChar <= asUInt8Array[7]) ||
                    (currentChar >= asUInt8Array[8] && currentChar <= asUInt8Array[9])) {
                    encodedString += String(format:"%c", currentChar)
                }
                else {
                    encodedString += String(format:"%%%02x", currentChar)
                }
            }
        
            return encodedString
        }
    }
   ```
    
    Dit hulpprogramma bevat de logica die verantwoordelijk is voor het genereren van de **SAS-token**. De *getSasToken* functie coördineert de stappen op hoog niveau nodig voor het voorbereiden van het token, zoals eerder beschreven en wordt aangeroepen door de installatieservice in de stappen verderop in deze zelfstudie. De andere twee functies worden aangeroepen door de *getSasToken functie*; *sha256HMac* voor het berekenen van de handtekening en *urlEncodedString* voor het coderen van de desbetreffende Url-reeks. De *urlEncodedString* functie is vereist als deze is niet mogelijk om de vereiste uitvoer met behulp van de ingebouwde *addingPercentEncoding* functie. De [iOS SDK voor Azure Storage](https://github.com/Azure/azure-storage-ios/blob/master/Lib/Azure%20Storage%20Client%20Library/Azure%20Storage%20Client%20Library/AZSUtil.m) behandeld als een uitstekende voorbeeld van hoe u deze bewerkingen benadering achteruitgaat in Objective-c Meer informatie over **Azure Service Bus SAS-tokens** kunt u vinden in de [documentatie voor Azure Service Bus](../service-bus-messaging/service-bus-sas.md). 

### <a name="verify-the-sas-token"></a>Controleer of het SAS-token
Voordat u de van installatieservice in de client implementeren, kunt u controleren of de app correct genereert de **SAS-token** met behulp van uw http-hulpprogramma naar keuze. Voor het kader van dit artikel, onze hulpprogramma naar keuze is **Postman**.

Noteer de *installationId* en de *token* waarden die worden gegenereerd door de app met behulp van een op de juiste wijze is geplaatst instructie of onderbrekingspunt afdrukken. 

Volg deze stappen om aan te roepen de *installaties* API:

1. In **Postman**, een nieuw tabblad openen
2. De aanvraag ingesteld op **ophalen** en het volgende adres:

    ```xml
    https://<namespace>.servicebus.windows.net/<hubName>/installations/<installationId>?api-version=2015-01
    ```

3. Configureer de aanvraagheaders als volgt:
    
   | Sleutel           | Value            |
   | ------------- | ---------------- |
   | Inhoudstype  | application/json |
   | Autorisatie | \<sasToken\>     |
   | x-ms-version  | 2015-01          |

4. Klik op de **Code** knop (rechtsboven onder de **opslaan** knop). De aanvraag moet in het onderstaande voorbeeld als volgt uitzien:

    ```html
    GET /<hubName>/installations/<installationId>?api-version=2015-01 HTTP/1.1
    Host: <namespace>.servicebus.windows.net
    Content-Type: application/json
    Authorization: <sasToken>
    x-ms-version: 2015-01
    Cache-Control: no-cache
    Postman-Token: <postmanToken>
    ```

5. Klik op de **verzenden** knop

Er bestaat geen registratie voor de opgegeven *installationId* op dit moment maar dit moet leiden tot een **404 niet gevonden** antwoord in plaats van **401-niet gemachtigd**. Dit resultaat moet bevestigen dat de **SAS-token** is geaccepteerd.

### <a name="implement-the-installation-service-class"></a>De installatie van service-klasse implementeren
Vervolgens implementeert u onze basic wrapper rond de [installaties REST-API](/rest/api/notificationhubs/create-overwrite-installation).  

Voeg een nieuw Swift bestand onder de **Services** map met de naam **NotificationRegistrationService.swift**, klikt u vervolgens de volgende code toevoegen aan dit bestand:

```swift
import Foundation

class NotificationRegistrationService {
    private let tokenizedBaseAddress: String = "https://%@.servicebus.windows.net/%@"
    private let tokenizedCreateOrUpdateInstallationRequest = "/installations/%@?api-version=%@"
    private let session = URLSession(configuration: URLSessionConfiguration.default)
    private let apiVersion = "2015-01"
    private let jsonEncoder = JSONEncoder()
    private let defaultHeaders: [String : String]
    private let installationId : String
    private let pushChannel : String
    private let hubNamespace : String
    private let hubName : String
    private let keyName : String
    private let key : String
    private var tokenData : TokenData? = nil
    
    init(withInstallationId installationId : String,
            andPushChannel pushChannel : String,
            andHubNamespace hubNamespace : String,
            andHubName hubName : String,
            andKeyName keyName : String,
            andKey key: String) {
        self.installationId = installationId
        self.pushChannel = pushChannel
        self.hubNamespace = hubNamespace
        self.hubName = hubName
        self.keyName = keyName
        self.key = key
        self.defaultHeaders = ["Content-Type": "application/json", "x-ms-version": apiVersion]
    }
    
    func register(
        withTags tags : [String]? = nil,
        andTemplates templates : Dictionary<String, PushTemplate>? = nil,
        completeWith completion: ((_ result: Bool) -> ())? = nil) {
        
        var deviceInstallation = DeviceInstallation(withInstallationId: installationId, andPushChannel: pushChannel)
        
        if let tags = tags {
            deviceInstallation.tags = tags
        }
        
        if let templates = templates {
            deviceInstallation.templates = templates
        }
        
        if let deviceInstallationJson = encodeToJson(deviceInstallation) {
            let sasToken = getSasToken()
            let requestUrl = String.init(format: tokenizedCreateOrUpdateInstallationRequest, installationId, apiVersion)
            let apiEndpoint = "\(getBaseAddress())\(requestUrl)"
            
            var request = URLRequest(url: URL(string: apiEndpoint)!)
            request.httpMethod = "PUT"
            
            for (key,value) in self.defaultHeaders {
                request.addValue(value, forHTTPHeaderField: key)
            }
            
            request.addValue(sasToken, forHTTPHeaderField: "Authorization")
            request.httpBody = Data(deviceInstallationJson.utf8)
            
            (self.session.dataTask(with: request) { dat, res, err in
                if let completion = completion {
                        completion(err == nil && (res as! HTTPURLResponse).statusCode == 200)
                }
            }).resume()
        }
    }
    
    private func getBaseAddress() -> String {
        return String.init(format: tokenizedBaseAddress, hubNamespace, hubName)
    }
    
    private func getSasToken() -> String {
        if (tokenData == nil ||
            Date(timeIntervalSince1970: Double((tokenData?.expiration)!)) < Date(timeIntervalSinceNow: -(5 * 60))) {
            self.tokenData = TokenUtility.getSasToken(forResourceUrl: getBaseAddress(), withKeyName: self.keyName, andKey: self.key)
        }

        return (tokenData?.token)!
    }
    
    private func encodeToJson<T : Encodable>(_ object: T) -> String? {
        do {
            let jsonData = try jsonEncoder.encode(object)
            if let jsonString = String(data: jsonData, encoding: .utf8) {
                return jsonString
            } else {
                return nil
            }
        }
        catch {
            return nil
        }
    }
}
```
 
De vereiste details zijn opgegeven als onderdeel van de initialisatie. Tags en sjablonen (optioneel) worden doorgegeven aan de *registreren* functie moet deel uitmaken van de **Apparaatinstallatie** JSON-nettolading.  

De *registreren* functie aanroept met de persoonlijke functies om voor te bereiden van de aanvraag. Wanneer een reactie wordt ontvangen, wordt de voltooiing genoemd die aangeeft of de registratie geslaagd of mislukt is.  

Het eindpunt van de aanvraag wordt samengesteld door de *getBaseAddress* werken met behulp van de **Notification Hub** parameters **naamruimte** en **naam**opgegeven tijdens de initialisatie.  

De *getSasToken* functie wordt gecontroleerd of het momenteel opgeslagen token geldig is, anders wordt het houden aan de **TokenUtility** naar een nieuw token genereren en op te slaan voordat u een waarde retourneren. 

Ten slotte de *encodeToJson* wordt de respectieve modelobjecten converteren naar JSON voor gebruik als onderdeel van de aanvraagtekst.

### <a name="invoke-the-notification-hubs-rest-api"></a>De Notification Hubs REST-API aanroepen
De laatste stap is om bij te werken **AppDelegate** gebruiken de **NotifiationRegistrationService** om te registreren bij onze **NotificationHub**. 

1. Open **AppDelegate.swift** en toevoegen van een variabele op klasseniveau voor het opslaan van een verwijzing naar de **NoficiationRegistrationService**:

    ```swift
    var registrationService : NotificationRegistrationService?
    ```

2. In hetzelfde bestand, werken de *didRegisterForRemoteNotificationsWithDeviceToken* functie voor het initialiseren van de **NotificationRegistrationService** met de vereiste parameters, roept u vervolgens de *registreren* functie.

    ```swift
    func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
        let installationId = (UIDevice.current.identifierForVendor?.description)!
        let pushChannel = deviceToken.reduce("", {$0 + String(format: "%02X", $1)})

        // Initialize the Notification Registration Service
        self.registrationService = NotificationRegistrationService(
            withInstallationId: installationId,
            andPushChannel: pushChannel,
            andHubNamespace: notificationHubNamespace!,
            andHubName: notificationHubName!,
            andKeyName: notificationHubKeyName!,
            andKey: notificationHubKey!)
    
        // Call register passing in the tags and template parameters
        self.registrationService!.register(withTags: tags, andTemplates: ["genericTemplate" : self.genericTemplate]) { (result) in
            if !result {
                print("Registration issue")
            } else {
                print("Registered")
            }
        }
    }
    ```

    Deze om eenvoudig te houden, gaat u een aantal afdrukken instructies gebruiken voor het bijwerken van het uitvoervenster met het resultaat van de *registreren* bewerking. 

3. Nu bouwen en uitvoeren van de app (op een fysiek apparaat). U ziet **'Registered'** in het uitvoervenster weergegeven.

## <a name="test-the-solution"></a>Testen van de oplossing
In dit stadium is onze app is geregistreerd bij **NotificationHub** en pushmeldingen kan ontvangen. In **Xcode**, het foutopsporingsprogramma stopt en sluit de app (als deze momenteel wordt uitgevoerd). Vervolgens controleert u dat de **Apparaatinstallatie** details zijn zoals verwacht en onze app kan nu inderdaad ontvangen van pushmeldingen.  

### <a name="verify-the-device-installation"></a>De Apparaatinstallatie van het controleren
U kunt nu dezelfde aanvraag maken net als eerder met behulp van **Postman** voor [controleren van de SAS-token](#verify-the-sas-token). Ervan uitgaande dat de **SAS-token** nog niet is verlopen, het antwoord moet nu bevatten de installatiegegevens die u hebt opgegeven, zoals de sjablonen en tags.  

```json
{
    "installationId": "<installationId>",
    "pushChannel": "<pushChannel>",
    "pushChannelExpired": false,
    "platform": "apns",
    "expirationTime": "9999-12-31T23:59:59.9999999Z",
    "tags": [
        "12345"
    ],
    "templates": {
        "genericTemplate": {
            "body": "{\"aps\":{\"alert\":\"$(message)\"}}",
            "tags": [
                "genericTemplate"
            ]
        }
    }
}
```

### <a name="send-a-test-notification-azure-portal"></a>Verzenden van een Testmelding (Azure portal)
De snelste manier om te testen kunt u nu meldingen ontvangen is om te navigeren naar de **Notification Hub** in de **Azure-portal**.

1. In de **Azure-portal**, gaat u naar de **overzicht** tabblad op uw **Notification Hub**
2. Klik op **verzenden testen** (linksboven) boven de **Essentials** samenvatting

    ![Notification Hubs Essentials samenvatting Test knop verzenden](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/hub-essentials-test-send.png)
3. Kies **aangepaste sjabloon** uit de lijst met **Platforms**
4. Voer **12345** voor de **verzenden naar Tagexpressie** (u kunt deze tag had opgegeven in onze installatie)
5. Bewerken (optioneel) de **bericht** in de JSON-nettolading
    
    ![Notification Hubs Test verzenden](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/hub-test-send.png)
6. Klik op **verzenden** en de portal moet aangeven of de melding is verzonden naar het apparaat

    ![Resultaten van Notification Hubs Test verzenden](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/hub-test-send-result.png)

    U ziet ook een melding in de **Meldingencentrum** op uw apparaat (ervan uitgaande dat de app actief is op de voorgrond is geplaatst). Tikken op de melding moet de app openen en weergeven van de waarschuwing.

    ![Voorbeeld van de melding ontvangen](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/test-send-notification-received.png)

### <a name="send-a-test-notification-postman"></a>Een Testmelding (Postman) verzenden
U kunt meldingen verzenden via de desbetreffende [REST-API](/rest/api/notificationhubs/) via **Postman** als bron en het mogelijk een handige manier om te testen. 

1. In **Postman**, een nieuw tabblad openen
2. De aanvraag ingesteld op **POST** en voer het volgende adres:

    ```xml
    https://<namespace>.servicebus.windows.net/<hubName>/messages/?api-version=2015-01
    ```

3. Configureer de aanvraagheaders als volgt:
    
   | Sleutel                            | Value                          |
   | ------------------------------ | ------------------------------ |
   | Inhoudstype                   | application/json;charset=utf-8 |
   | Autorisatie                  | \<sasToken\>                   |
   | ServiceBusNotification-Format  | sjabloon                       |
   | Tags                           | "12345"                        |

4. Configureren van de aanvraag **hoofdtekst** gebruiken **RAW - JSON (application.json)** met de volgende JSON-nettolading:

    ```json
    {
       "message" : "Hello from Postman!"
    }
    ```

5. Klik op de **Code** knop (rechtsboven onder de **opslaan** knop). De aanvraag moet in het onderstaande voorbeeld als volgt uitzien:

    ```html
    POST /<hubName>/messages/?api-version=2015-01 HTTP/1.1
    Host: <namespace>.servicebus.windows.net
    Content-Type: application/json;charset=utf-8.
    ServiceBusNotification-Format: template
    Tags: "12345"
    Authorization: <sasToken>
    Cache-Control: no-cache
    Postman-Token: <postmanToken>

    {
        "message" : "Hello from Postman!"
    }
    ```

5. Klik op de **verzenden** knop

U moet een code van de status geslaagd ophalen en de melding op het clientapparaat.

## <a name="next-steps"></a>Volgende stappen
U hebt nu een eenvoudige iOS Swift-app verbonden met een **Notification Hub** via de [REST-API](/rest/api/notificationhubs/) en kan verzenden en ontvangen van meldingen. Raadpleeg voor meer informatie de volgende artikelen: 

- [Overzicht van Azure Notification Hubs](notification-hubs-push-notification-overview.md)
- [Notification Hubs REST-API 's](/rest/api/notificationhubs/)
- [Notification Hubs SDK voor back-end-bewerkingen](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)
- [Notification Hubs SDK op GitHub](https://github.com/Azure/azure-notificationhubs)
- [Met back-end toepassing registreren](notification-hubs-ios-aspnet-register-user-from-backend-to-push-notification.md)
- [Registratiebeheer](notification-hubs-push-notification-registration-management.md)
- [Werken met labels](notification-hubs-tags-segment-push-message.md) 
- [Werken met aangepaste sjablonen](notification-hubs-templates-cross-platform-push-messages.md)
- [Service Bus-toegangsbeheer met handtekeningen voor gedeelde toegang](../service-bus-messaging/service-bus-sas.md)
- [Programmatisch genereren van SAS-tokens](/rest/api/eventhub/generate-sas-token)
- [Apple-beveiliging: algemene crypto](https://developer.apple.com/security/)
- [UNIX-epoche-tijd](https://en.wikipedia.org/wiki/Unix_time)
- [HMAC](https://en.wikipedia.org/wiki/HMAC)
