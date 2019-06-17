---
title: Pushmeldingen naar Swift iOS-apps die met Azure Notification Hubs | Microsoft Docs
description: Leer hoe u pushmeldingen naar Swift iOS-apps die met Azure Notification Hubs.
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
ms.openlocfilehash: a4773ddd8114659118e89cfee57e73ddb39ff6b6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67116672"
---
# <a name="tutorial-push-notifications-to-swift-ios-apps-that-use-the-notification-hubs-rest-api"></a>Zelfstudie: Pushmeldingen verzenden naar Swift iOS-apps die gebruikmaken van de Notification Hubs REST-API

> [!div class="op_single_selector"]
> * [Objective-C](notification-hubs-ios-apple-push-notification-apns-get-started.md)
> * [Swift](notification-hubs-ios-push-notifications-swift-apps-get-started.md)

In deze zelfstudie gebruikt u Azure Notification Hubs pushmeldingen verzendt naar een Swift op basis van iOS-toepassing met behulp van de [REST-API](/rest/api/notificationhubs/). Ook maakt u een lege iOS-app die pushmeldingen via ontvangt de [Apple Push Notification service (APNs)](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html#//apple_ref/doc/uid/TP40008194-CH8-SW1).

In deze zelfstudie doorloopt u de volgende stappen uit:

> [!div class="checklist"]
> * De CSR-bestand genereren.
> * Vragen om uw app voor pushmeldingen.
> * Maak een inrichtingsprofiel voor de app.
> * Een Notification Hub maken.
> * De notification hub configureren met APNs-informatie.
> * Uw iOS-app verbinden met een notification hub.
> * Testen van de oplossing.

## <a name="prerequisites"></a>Vereisten

Als u wilt volgen, hebt u het volgende nodig:

- Doorlopen [Azure Notification Hubs-overzicht](notification-hubs-push-notification-overview.md) als u niet bekend bent met de service.
- Voor meer informatie over [registraties en installatie](notification-hubs-push-notification-registration-management.md).
- Een actief [Apple Developer-Account](https://developer.apple.com).
- Een Mac met Xcode, samen met een geldige developer-certificaat geïnstalleerd in uw sleutelketen.
- Een fysieke iPhone-apparaat die u kunt uitvoeren en fouten opsporen in, als u pushmeldingen kunt verzenden met de simulator niet testen.
- Uw fysieke iPhone-apparaat is geregistreerd de [Apple-Portal](https://developer.apple.com) en is gekoppeld aan uw certificaat.
- Een [Azure-abonnement](https://portal.azure.com) waar u kunt maken en beheren van resources.

Zelfs als u nog geen ervaring met iOS-ontwikkeling hebt, zou het mogelijk om te volgen de stappen voor het maken van dit voorbeeld eerste principes. Kunt u echter gebruikmaken van vertrouwd zijn met de volgende concepten:

- Het bouwen van iOS-apps met Xcode en Swift.
- Configureren van een [Azure Notification hub](notification-hubs-ios-apple-push-notification-apns-get-started.md) voor iOS.
- De [Apple Developer-Portal](https://developer.apple.com) en de [Azure-portal](https://portal.azure.com).

> [!NOTE]
> De notification hub wordt geconfigureerd voor het gebruik van de **Sandbox** alleen verificatiemodus. U moet met deze verificatiemodus niet gebruiken voor productieworkloads.

[!INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

## <a name="connect-your-ios-app-to-a-notification-hub"></a>Uw iOS-app verbinden met een notification hub

In deze sectie bouwt u de iOS-app waarmee de verbinding met de notification hub maken.  

### <a name="create-an-ios-project"></a>Een iOS-project maken

1. Maak in Xcode een nieuw iOS-project en selecteer de sjabloon **Single View Application** (Toepassing met één weergave).

1. Bij het instellen van de opties voor het nieuwe project:

   1. Geef de **Productnaam** (PushDemo) en **organisatie-id** (`com.<organization>`) die u hebt gebruikt bij het instellen van **bundel-id** in de Apple Developer-Portal.

   1. Kies de **Team** die de **App-ID** is ingesteld voor.

   1. Stel de **taal** naar **Swift**.

   1. Selecteer **Next**.

1. Maak een nieuwe map met de naam **SupportingFiles**.

1. Maak een nieuw p-list-bestand met de naam **devsettings.plist** in de **SupportingFiles** map. Zorg ervoor dat u deze map toevoegen die uw **gitignore** bestand, zodat deze niet toegewezen is als u werkt met een git-opslagplaats. In een productie-app, wilt u waarschijnlijk worden voorwaardelijk instellen van deze geheime gegevens als onderdeel van een geautomatiseerd buildproces. Deze instellingen worden niet behandeld in dit scenario.

1. Update **devsettings.plist** om op te nemen van de volgende configuratie-items met behulp van uw eigen waarden van de notification hub die u hebt ingericht:

   | Sleutel                            | Type                     | Value                     |
   |--------------------------------| -------------------------| --------------------------|
   | notificationHubKey             | String                   | <hubKey>                  |
   | notificationHubKeyName         | String                   | <hubKeyName>              |
   | notificationHubName            | String                   | <hubName>                 |
   | notificationHubNamespace       | String                   | <hubNamespace>            |

   U kunt de vereiste waarden vinden door te navigeren naar de resource voor notification hub in Azure portal. In het bijzonder de **notificationHubName** en **notificationHubNamespace** waarden zijn in de rechterbovenhoek van de **Essentials** samenvatting binnen de **Overzicht** pagina.

   ![Overzicht van Notification Hubs Essentials](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/hub-essentials.png)

   U vindt hier ook de **notificationHubKeyName** en **notificationHubKey** waarden door te navigeren naar **toegangsbeleid** en selecteren van de desbetreffende  **Het toegangsbeleid**, zoals `DefaultFullSharedAccessSignature`. Hierna kopiëren van de **Primary Connection String** de waarde wordt voorafgegaan door `SharedAccessKeyName=` voor `notificationHubKeyName` en de waarde wordt voorafgegaan door `SharedAccessKey=` voor de `notificationHubKey`.

   De verbindingsreeks moet de volgende indeling:

   ```xml
   Endpoint=sb://<namespace>.servicebus.windows.net/;SharedAccessKeyName=<notificationHubKeyName>;SharedAccessKey=<notificationHubKey>
   ```

   Geef deze om eenvoudig te houden, `DefaultFullSharedAccessSignature` zodat u het token gebruiken kunt om meldingen te verzenden. In de praktijk het `DefaultListenSharedAccessSignature` zou een betere keuze voor situaties waarin u wilt dat alleen om meldingen te ontvangen.

1. Onder **Projectnavigator**, selecteer de **projectnaam** en selecteer vervolgens de **algemene** tabblad.

1. Vinden **identiteit** en stel de **bundel-id** zodat deze overeenkomt met de waarde `com.<organization>.PushDemo`, die wordt de waarde gebruikt voor de **App-ID** in een vorige stap.

1. Zoek **ondertekening**, en selecteer vervolgens de juiste **Team** voor uw **Apple Developer-Account**. De **Team** waarde moet overeenkomen met het account waarmee u uw certificaten en -profielen gemaakt.

1. Xcode moet automatisch Haal de juiste **Inrichtingsprofiel** waarde op basis van de **bundel-id**. Als u niet de nieuwe ziet **Inrichtingsprofiel** waarde, vernieuw de profielen voor het **identiteit voor ondertekening van** hiervoor **Xcode**  >  **Voorkeuren** > **Account** > **Details weergeven**. Selecteer **identiteit voor ondertekening van**, en selecteer vervolgens de **vernieuwen** knop in de rechterbenedenhoek om te downloaden van de profielen.

1. Selecteer de **mogelijkheden** tabblad en zorg ervoor dat **pushmeldingen** zijn ingeschakeld.

1. Open uw **AppDelegate.swift** bestand voor het implementeren van de **UNUserNotificationCenterDelegate** -protocol en voeg de volgende code toe aan het begin van de klasse:

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

    Deze leden gaat u later gebruiken. Specifiek, gebruikt u de **tags** en **genericTemplate** leden als onderdeel van de registratie. Zie voor meer informatie over labels [labels voor registraties](notification-hubs-tags-segment-push-message.md) en [sjabloonregistraties](notification-hubs-templates-cross-platform-push-messages.md).

1. In hetzelfde bestand, voeg de volgende code aan de **didFinishLaunchingWithOptions** functie:

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

    Deze code haalt de instellingswaarden van **devsettings.plist**, stelt de **AppDelegate** klasse als de **UNUserNotificationCenter** delegeren, aanvragen autorisatie voor pushmeldingen te verzenden en gesprekken **registerForRemoteNotifications**.

    Deze om eenvoudig te houden, de code ondersteunt *iOS 10 en later alleen*. U kunt ondersteuning voor vorige versies van het besturingssysteem toevoegen door voorwaardelijk met behulp van de desbetreffende API's en benaderingen net zoals u normaal zou doen.

1. Voeg in hetzelfde bestand de volgende functies:

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

    De code wordt gebruikgemaakt van de **installationId** en **pushChannel** waarden om te registreren bij de notification hub. In dit geval u **UIDevice.current.identifierForVendor** voor een unieke waarde voor het identificeren van het apparaat en klik vervolgens opmaak de **deviceToken** voor de gewenste  **pushChannel** waarde. De **showAlert** functie bestaat gewoon om weer te geven berichttekst voor demonstratiedoeleinden te gebruiken.

1. Klik in de **AppDelegate.swift** bestand, toevoegen de **willPresent** en **didReceive** functioneert **UNUserNotificationCenterDelegate**. Deze functies, wordt er een waarschuwing weergeven wanneer ze een bericht weergegeven dat een app wordt uitgevoerd in de voorgrond of op de achtergrond respectievelijk.

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

1. Voeg afdrukken instructies toe aan de onderkant van de **didRegisterForRemoteNotificationsWithDeviceToken** functie om te controleren of **installationId** en **pushChannel** worden toegewezen waarden.

1. Maak de **modellen**, **Services**, en **hulpprogramma's voor** mappen voor de fundamentele onderdelen die u hierna zullen toevoegen aan het project later opnieuw.

1. Controleer of het project wordt gemaakt en wordt uitgevoerd op een fysiek apparaat. Pushmeldingen kunnen niet worden getest met behulp van de simulator.

### <a name="create-models"></a>Modellen maken

In deze stap maakt u een set van modellen om weer te geven de [Notification Hubs REST API](/rest/api/notificationhubs/) nettoladingen en voor het opslaan van de vereiste gedeelde toegang token signature (SAS) gegevens.

1. Voeg een nieuw Swift-bestand met de naam **PushTemplate.swift** naar de **modellen** map. Dit model biedt een struct die vertegenwoordigt de **hoofdtekst** van een afzonderlijke sjabloon als onderdeel van de **DeviceInstallation** nettolading.

    ```swift
    import Foundation

    struct PushTemplate : Codable {
        let body : String

        init(withBody body : String) {
            self.body = body
        }
    }
    ```

1. Voeg een nieuw Swift-bestand met de naam **DeviceInstallation.swift** naar de **modellen** map. Dit bestand definieert een struct die vertegenwoordigt de payload voor het maken of bijwerken van een **Apparaatinstallatie**. Voeg de volgende code naar het bestand:

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

1. Voeg een nieuw Swift-bestand met de naam **TokenData.swift** naar de **modellen** map. Dit model wordt gebruikt voor het opslaan van een SAS-token, samen met de vervaldatum.

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

Notification Hubs gebruiken dezelfde beveiligingsinfrastructuur als Azure Service Bus. Voor het aanroepen van de REST-API, moet u [programmatisch genereert een SAS-token](/rest/api/eventhub/generate-sas-token) die kunnen worden gebruikt de **autorisatie** -header van de aanvraag.  

Het resulterende token niet in de volgende indeling:

```xml
SharedAccessSignature sig=<UrlEncodedSignature>&se=<ExpiryEpoch>&skn=<KeyName>&sr=<UrlEncodedResourceUri>
```

Het proces zelf omvat de dezelfde zes belangrijke stappen:  

1. De vervaldatum in Computing [UNIX epoche-tijd](https://en.wikipedia.org/wiki/Unix_time) indeling, wat betekent het aantal seconden dat verstreken sinds middernacht Coordinated Universal Time, 1 januari 1970.
1. Opmaak de **ResourceUrl** die staat voor de resource die u probeert te krijgen tot dus is het percentage gecodeerd en kleine letters. De **ResourceUrl** heeft de vorm `'https://<namespace>.servicebus.windows.net/<hubName>'`.
1. Voorbereiden van de **StringToSign**, die is opgemaakt als `'<UrlEncodedResourceUrl>\n<ExpiryEpoch>'`.
1. Cloudcomputing en Base64-codering de **handtekening** met behulp van de HMAC-SHA256-hash van de **StringToSign** waarde. De hash-waarde wordt gebruikt met de **sleutel** deel uitmaakt van de **Connection String** voor de desbetreffende **autorisatieregel**.
1. Opmaak van de met Base64 gecodeerde **handtekening** dus is het percentage gecodeerd.
1. Het token in de verwachte indeling maken met behulp van de **UrlEncodedSignature**, **ExpiryEpoch**, **KeyName**, en **UrlEncodedResourceUrl** waarden.

Zie de [documentatie voor Azure Service Bus](../service-bus-messaging/service-bus-sas.md) voor een uitgebreider overzicht van de handtekening voor gedeelde toegang en hoe Azure Service Bus en Notification Hubs gebruiken.

Voor de doeleinden van dit snelle voorbeeld, gaat u gebruik van Apple open-source **CommonCrypto** bibliotheek om te helpen bij de hash van de handtekening. Een C-bibliotheek is, is het niet toegankelijk in Swift buiten het vak. U kunt de bibliotheek beschikbaar maken met behulp van een bridging header.

Toevoegen en configureren van de bridging-header:

1. Selecteer in Xcode, **bestand** > **nieuw** > **bestand** > **headerbestand**. Naam van het header-bestand **BridgingHeader.h**.

1. Bewerk het bestand te importeren **CommonHMAC.h**:

    ```swift
    #import <CommonCrypto/CommonHMAC.h>

    #ifndef BridgingHeader_h
    #define BridgingHeader_h


    #endif /* BridgingHeader_h */
    ```

1. Bijwerken van het doel **Build Settings** om te verwijzen naar de bridging-header:

   1. Open de **gebouw instellingen** tabblad en schuif omlaag naar de **Swift Compiler** sectie.

   1. Zorg ervoor dat de **installeren Objective-C compatibiliteit Header** optie is ingesteld op **Ja**.

   1. Geef het bestandspad `'<ProjectName>/BridgingHeader.h'` in de **Objective-C bridging Header** optie. Dit is het pad naar onze bridging header.

   Als u deze opties niet vinden, zorgt u ervoor dat u hebt de **alle** weergave die is geselecteerd in plaats van **Basic** of **aangepaste**.

   Er zijn veel van derden opensource-wrapper clientbibliotheken beschikbaar die mogelijk met behulp van **CommonCrypto** iets eenvoudiger. Beschrijving van deze bibliotheken is echter buiten het bereik van dit artikel.

1. Voeg een nieuw Swift-bestand met de naam **TokenUtility.swift** binnen de **hulpprogramma's voor** map en voeg de volgende code toe:

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

   Dit hulpprogramma bevat de logica die verantwoordelijk is voor het genereren van het SAS-token.

   Zoals eerder beschreven de **getSasToken** functie coördineert de stappen op hoog niveau nodig voor het voorbereiden van het token. De functie wordt aangeroepen door de van installatieservice verderop in deze zelfstudie.

   De andere twee functies worden aangeroepen door de **getSasToken** functie: **sha256HMac** voor het berekenen van de handtekening en **urlEncodedString** voor de gekoppelde URL-codering tekenreeks. De **urlEncodedString** is vereist als het niet mogelijk om de vereiste uitvoer met behulp van de ingebouwde **addingPercentEncoding** functie.

   De [iOS SDK voor Azure Storage](https://github.com/Azure/azure-storage-ios/blob/master/Lib/Azure%20Storage%20Client%20Library/Azure%20Storage%20Client%20Library/AZSUtil.m) is een uitstekende voorbeeld van hoe u deze bewerkingen in Objective-c benadering Meer informatie over Azure Service Bus SAS-tokens kan worden gevonden de [documentatie voor Azure Service Bus](../service-bus-messaging/service-bus-sas.md).

### <a name="verify-the-sas-token"></a>Controleer of het SAS-token

Voordat u de installatieservice in de client implementeert, moet u controleren of de app correct het SAS-token genereert met behulp van uw HTTP-hulpprogramma naar keuze. Voor de toepassing van deze zelfstudie wordt u onze hulpprogramma naar keuze is **Postman**.

Een op de juiste wijze is geplaatst afdrukken instructie of een onderbrekingspunt gebruiken om te weten de **installationId** en de **token** waarden die worden gegenereerd door de app.

Volg deze stappen om aan te roepen de **installaties** API:

1. In **Postman**, een nieuw tabblad geopend.

1. De aanvraag ingesteld op **ophalen** en geeft u het volgende adres:

    ```xml
    https://<namespace>.servicebus.windows.net/<hubName>/installations/<installationId>?api-version=2015-01
    ```

1. Configureer de aanvraagheaders als volgt:

   | Sleutel           | Value            |
   | ------------- | ---------------- |
   | Content-Type  | application/json |
   | Autorisatie | <sasToken>       |
   | x-ms-version  | 2015-01          |

1. Selecteer de **Code** knop die wordt weergegeven in de rechterbovenhoek onder de **opslaan** knop. De aanvraag moet in het volgende voorbeeld als volgt uitzien:

    ```html
    GET /<hubName>/installations/<installationId>?api-version=2015-01 HTTP/1.1
    Host: <namespace>.servicebus.windows.net
    Content-Type: application/json
    Authorization: <sasToken>
    x-ms-version: 2015-01
    Cache-Control: no-cache
    Postman-Token: <postmanToken>
    ```

1. Selecteer de knop **Verzenden**.

Er bestaat geen registratie voor de opgegeven **installationId** op dit moment. De verificatie moet resulteren in een antwoord '404 niet gevonden' in plaats van een '401 Unauthorized'-antwoord. Dit resultaat te bevestigen dat de SAS-token is geaccepteerd.

### <a name="implement-the-installation-service-class"></a>De installatie van service-klasse implementeren

Vervolgens implementeert u onze basic wrapper rond de [installaties REST-API](/rest/api/notificationhubs/create-overwrite-installation).  

Voeg een nieuw Swift-bestand met de naam **NotificationRegistrationService.swift** onder de **Services** map, en voeg vervolgens de volgende code toe aan dit bestand:

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

De vereiste details zijn opgegeven als onderdeel van de initialisatie. Tags en sjablonen (optioneel) worden doorgegeven aan de **registreren** functie moet deel uitmaken van de **Apparaatinstallatie** JSON-nettolading.  

De **registreren** functie roept de andere persoonlijke functies voor het voorbereiden van de aanvraag. Nadat een antwoord is ontvangen, wordt de voltooiing wordt genoemd en geeft aan of de registratie gelukt is.  

Het eindpunt van de aanvraag wordt samengesteld door de **getBaseAddress** functie. De bouw maakt gebruik van de notification hub-parameters *naamruimte* en *naam* die zijn opgegeven tijdens de initialisatie.  

De **getSasToken** functie wordt gecontroleerd of het momenteel opgeslagen token geldig is. Als het token is niet geldig, de functie is aangeroepen **TokenUtility** om een nieuw token te genereren en slaat u deze voordat u een waarde retourneren.

Ten slotte **encodeToJson** converteert de respectieve modelobjecten naar JSON voor gebruik als onderdeel van de aanvraagtekst.

### <a name="invoke-the-notification-hubs-rest-api"></a>De Notification Hubs REST-API aanroepen

Het bijwerken van de laatste stap **AppDelegate** gebruiken **NotificationRegistrationService** om te registreren bij onze **NotificationHub**.

1. Open **AppDelegate.swift** en toevoegen van een variabele op klasseniveau voor het opslaan van een verwijzing naar de **NotificationRegistrationService**:

    ```swift
    var registrationService : NotificationRegistrationService?
    ```

1. Bijwerken in hetzelfde bestand, de **didRegisterForRemoteNotificationsWithDeviceToken** functie voor het initialiseren van de **NotificationRegistrationService** met de vereiste parameters en vervolgens Roep de **registreren** functie.

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

        // Call register, passing in the tags and template parameters
        self.registrationService!.register(withTags: tags, andTemplates: ["genericTemplate" : self.genericTemplate]) { (result) in
            if !result {
                print("Registration issue")
            } else {
                print("Registered")
            }
        }
    }
    ```

    Deze om eenvoudig te houden, gaat u een aantal afdrukken instructies gebruiken voor het bijwerken van het uitvoervenster met het resultaat van de **registreren** bewerking.

1. Nu bouwen en uitvoeren van de app op een fysiek apparaat. U ziet 'Registered' in het uitvoervenster weergegeven.

## <a name="test-the-solution"></a>Testen van de oplossing

Onze app in deze fase is geregistreerd bij **NotificationHub** en pushmeldingen kan ontvangen. Het foutopsporingsprogramma stopt in Xcode en sluit de app als deze momenteel wordt uitgevoerd. Controleer vervolgens of de **Apparaatinstallatie** details zijn zoals verwacht en onze app kan nu ontvangen van pushmeldingen.  

### <a name="verify-the-device-installation"></a>De Apparaatinstallatie van het controleren

U kunt nu dezelfde aanvraag maken als u eerder hebt gedaan met behulp van **Postman** voor [controleren van de SAS-token](#verify-the-sas-token). Ervan uitgaande dat de SAS-token niet is verlopen, moet het antwoord nu de installatiegegevens die u hebt opgegeven, zoals de sjablonen en -tags bevatten.

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

Er is de snelste manier om te testen kunt u nu meldingen ontvangen om te bladeren naar de notification hub in Azure portal:

1. In de Azure-portal, blader naar de **overzicht** tabblad op de notification hub.

1. Selecteer **verzenden testen**, is hoger dan de **Essentials** overzicht in de linkerbovenhoek van de portal-venster:

    ![Notification Hubs Essentials samenvatting Test knop verzenden](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/hub-essentials-test-send.png)

1. Kies **aangepaste sjabloon** uit de **Platforms** lijst.

1. Voer **12345** voor de **verzenden naar Tagexpressie**. U deze tag eerder hebt opgegeven in de installatie.

1. Eventueel bewerken de **bericht** in de JSON-nettolading:

    ![Notification Hubs Test verzenden](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/hub-test-send.png)

1. Selecteer **Verzenden**. De portal moet geven aan of de melding is verzonden naar het apparaat:

    ![Resultaten van Notification Hubs Test verzenden](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/hub-test-send-result.png)

    Ervan uitgaande dat de app wordt niet uitgevoerd op de voorgrond is geplaatst, moet er een melding in de **Meldingencentrum** op uw apparaat. Op de melding tikt moet de app openen en weergeven van de waarschuwing.

    ![Voorbeeld van de melding ontvangen](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/test-send-notification-received.png)

### <a name="send-a-test-notification-mail-carrier"></a>Een Testmelding (e-Mail vervoerder) verzenden

U kunt meldingen verzenden via de [REST-API](/rest/api/notificationhubs/) met behulp van **Postman**, die mogelijk een handige manier om te testen.

1. Open een nieuw tabblad in **Postman**.

1. De aanvraag ingesteld op **POST**, en voer het volgende adres:

    ```xml
    https://<namespace>.servicebus.windows.net/<hubName>/messages/?api-version=2015-01
    ```

1. Configureer de aanvraagheaders als volgt:

   | Sleutel                            | Value                          |
   | ------------------------------ | ------------------------------ |
   | Content-Type                   | application/json;charset=utf-8 |
   | Autorisatie                  | <sasToken>                     |
   | ServiceBusNotification-Format  | sjabloon                       |
   | Tags                           | "12345"                        |

1. Configureren van de aanvraag **hoofdtekst** gebruiken **RAW - JSON (application.json)** met de volgende JSON-nettolading:

    ```json
    {
       "message" : "Hello from Postman!"
    }
    ```

1. Selecteer de **Code** knop, deze bevindt zich onder de **opslaan** knop in de rechterbovenhoek van het venster. De aanvraag moet in het volgende voorbeeld als volgt uitzien:

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

1. Selecteer de knop **Verzenden**.

U moet een code van de status geslaagd ophalen en de melding op het clientapparaat.

## <a name="next-steps"></a>Volgende stappen
U hebt nu een eenvoudige iOS Swift-app verbonden met een notification hub via de [REST-API](/rest/api/notificationhubs/) en kan verzenden en ontvangen van meldingen. Raadpleeg voor meer informatie de volgende artikelen:

- [Overzicht van Azure Notification Hubs](notification-hubs-push-notification-overview.md)
- [Notification Hubs REST-API 's](/rest/api/notificationhubs/)
- [Notification Hubs SDK voor back-end-bewerkingen](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)
- [Notification Hubs SDK op GitHub](https://github.com/Azure/azure-notificationhubs)
- [Registreren bij back-end toepassing](notification-hubs-ios-aspnet-register-user-from-backend-to-push-notification.md)
- [Registratiebeheer](notification-hubs-push-notification-registration-management.md)
- [Werken met labels](notification-hubs-tags-segment-push-message.md) 
- [Werken met aangepaste sjablonen](notification-hubs-templates-cross-platform-push-messages.md)
- [Service Bus-toegangsbeheer met handtekeningen voor gedeelde toegang](../service-bus-messaging/service-bus-sas.md)
- [Programmatisch genereren van SAS-tokens](/rest/api/eventhub/generate-sas-token)
- [Apple-beveiliging: algemene crypto](https://developer.apple.com/security/)
- [UNIX-epoche-tijd](https://en.wikipedia.org/wiki/Unix_time)
- [HMAC](https://en.wikipedia.org/wiki/HMAC)
