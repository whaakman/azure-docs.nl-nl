---
title: Over het gebruik iOS-SDK voor Azure Mobile Apps
description: Over het gebruik iOS-SDK voor Azure Mobile Apps
services: app-service\mobile
documentationcenter: ios
author: conceptdev
editor: ''
ms.assetid: 4e8e45df-c36a-4a60-9ad4-393ec10b7eb9
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 10/01/2016
ms.author: crdun
ms.openlocfilehash: 8eef75a6e1f4f05aa6d7ce8f9e6fdda52162d0bc
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52960716"
---
# <a name="how-to-use-ios-client-library-for-azure-mobile-apps"></a>Over het gebruik iOS-clientbibliotheek voor Azure Mobile Apps

[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Deze handleiding leert u hoe u algemene scenario's met behulp van de meest recente uitvoert [Azure Mobile Apps-iOS SDK][1]. Als u niet bekend bent met Azure Mobile Apps, voltooi eerst [Azure Mobile Apps snel starten] voor het maken van een back-end, een tabel maken en een vooraf gemaakte iOS Xcode-project downloaden. In deze handleiding, we ons richten op de client-side-iOS SDK. Zie voor meer informatie over de SDK-serverzijde voor de back-end, de Server SDK HOWTOs.

## <a name="reference-documentation"></a>Referentiedocumentatie

De referentiedocumentatie voor de iOS-client-SDK bevindt zich hier: [Azure Mobile Apps-iOS-Client verwijzing][2].

## <a name="supported-platforms"></a>Ondersteunde Platforms

De iOS-SDK biedt ondersteuning voor projecten Objective-C, Swift 2.2-projecten en projecten Swift 2.3 voor iOS-versie 8.0 of hoger.

De verificatie 'server-stroom' gebruikmaakt van een webweergave voor de gebruikersinterface weergegeven.  Als het apparaat niet kan om weer te geven van een UI WebView en vervolgens een andere methode voor verificatie is vereist dat is buiten het bereik van het product.  
Deze SDK is dus niet geschikt is voor controle van het type of op dezelfde manier beperkte apparaten.

## <a name="Setup"></a>Installatie en vereisten

Deze handleiding wordt ervan uitgegaan dat u een back-end hebt gemaakt met een tabel. Deze handleiding wordt ervan uitgegaan dat de tabel hetzelfde schema als de tabellen in deze zelfstudies heeft. Deze handleiding wordt ervan uitgegaan dat in uw code kunt u verwijzen naar `MicrosoftAzureMobile.framework` en importeer `MicrosoftAzureMobile/MicrosoftAzureMobile.h`.

## <a name="create-client"></a>Hoe:-Client maken

Voor toegang tot een back-end van Azure Mobile Apps in uw project, maakt u een `MSClient`. Vervang `AppUrl` met de app-URL. U kunt laten `gatewayURLString` en `applicationKey` leeg zijn. Als u een gateway voor verificatie instellen, vullen `gatewayURLString` met de URL van de gateway.

**Objective-C**:

```objc
MSClient *client = [MSClient clientWithApplicationURLString:@"AppUrl"];
```

**SWIFT**:

```swift
let client = MSClient(applicationURLString: "AppUrl")
```

## <a name="table-reference"></a>Hoe: tabelverwijzing maken

Maak een verwijzing naar de back-endtabel als u gegevens wilt bekijken of bijwerken. Vervang `TodoItem` door de naam van uw tabel

**Objective-C**:

```objc
MSTable *table = [client tableWithName:@"TodoItem"];
```

**SWIFT**:

```swift
let table = client.tableWithName("TodoItem")
```

## <a name="querying"></a>Hoe: gegevens op te vragen

Query voor het maken van een databasequery uitvoeren op de `MSTable` object. De volgende query haalt alle items in `TodoItem` en logboeken van de tekst van elk item.

**Objective-C**:

```objc
[table readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) { // error is nil if no error occured
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) { // items is NSArray of records that match query
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**SWIFT**:

```swift
table.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

## <a name="filtering"></a>Hoe: Filter gegevens geretourneerd

Om resultaten te filteren, zijn er veel beschikbare opties.

Als u wilt filteren met behulp van een predicaat, gebruikt u een `NSPredicate` en `readWithPredicate`. De volgende filters gegevens om te zoeken alleen onvolledig Todo-items geretourneerd.

**Objective-C**:

```objc
// Create a predicate that finds items where complete is false
NSPredicate * predicate = [NSPredicate predicateWithFormat:@"complete == NO"];
// Query the TodoItem table
[table readWithPredicate:predicate completion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**SWIFT**:

```swift
// Create a predicate that finds items where complete is false
let predicate =  NSPredicate(format: "complete == NO")
// Query the TodoItem table
table.readWithPredicate(predicate) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

## <a name="query-object"></a>Hoe: MSQuery gebruiken

Voor een complexe query (met inbegrip van sorteren en paginering) uitgevoerd, maakt u een `MSQuery` object, rechtstreeks of via een predicaat:

**Objective-C**:

```objc
MSQuery *query = [table query];
MSQuery *query = [table queryWithPredicate: [NSPredicate predicateWithFormat:@"complete == NO"]];
```

**SWIFT**:

```swift
let query = table.query()
let query = table.queryWithPredicate(NSPredicate(format: "complete == NO"))
```

`MSQuery` kunt u verschillende query-gedrag beheren.

* Volgorde van de resultaten opgeven
* Beperken welke velden u wilt retourneren
* Beperken hoeveel records retourneren
* Geef het totale aantal in antwoord
* Geef aangepaste queryreeksparameters in de aanvraag
* Extra functies toepassen

Voer een `MSQuery` query door het aanroepen van `readWithCompletion` op het object.

## <a name="sorting"></a>Hoe: sorteren van gegevens met MSQuery

Als u wilt sorteren resultaten, kunt u een voorbeeld laten we kijken. Als u wilt sorteren op Oplopend veld 'text' en klik vervolgens op 'complete' aflopend, aanroepen `MSQuery` als volgt te werk:

**Objective-C**:

```objc
[query orderByAscending:@"text"];
[query orderByDescending:@"complete"];
[query readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**SWIFT**:

```swift
query.orderByAscending("text")
query.orderByDescending("complete")
query.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

## <a name="selecting"></a><a name="parameters"></a>Hoe: velden, beperken en queryreeksparameters met MSQuery uitvouwen

Als u wilt beperken velden moeten worden geretourneerd in een query, geeft u de namen van de velden in de **selectFields** eigenschap. In dit voorbeeld retourneert alleen de tekst en voltooide velden:

**Objective-C**:

```objc
query.selectFields = @[@"text", @"complete"];
```

**SWIFT**:

```swift
query.selectFields = ["text", "complete"]
```

Aanvullende queryreeksparameters bevatten in de serveraanvraag (bijvoorbeeld, omdat ze maakt gebruik van een aangepast script voor de server-side), vullen `query.parameters` als volgt te werk:

**Objective-C**:

```objc
query.parameters = @{
    @"myKey1" : @"value1",
    @"myKey2" : @"value2",
};
```

**SWIFT**:

```swift
query.parameters = ["myKey1": "value1", "myKey2": "value2"]
```

## <a name="paging"></a>Hoe: configureren van het formaat van pagina

Met Azure Mobile Apps bepaalt de grootte van het aantal records dat tegelijk uit de back-end-tabellen worden opgehaald. Een aanroep van `pull` gegevens zou vervolgens batch-up van gegevens, op basis van deze paginagrootte, totdat er zijn geen records meer om op te halen.

Het is mogelijk het configureren van een pagina grootte met **MSPullSettings** zoals hieronder wordt weergegeven. De standaardpagina heeft 50 en in het onderstaande voorbeeld gewijzigd in 3.

U kunt de grootte van een andere pagina voor betere prestaties kan configureren. Hebt u een groot aantal kleine gegevensrecords, vermindert de paginagrootte van een hoge het aantal retouren van de server.

Deze instelling bepaalt u alleen de paginagrootte aan de clientzijde. Als de client om een groter formaat vraagt dan de back-end van Mobile Apps ondersteunt, de paginagrootte wordt beperkt tot het maximum dat de back-end is geconfigureerd voor ondersteuning.

Deze instelling is ook de *getal* gegevensrecords, niet de *bytegrootte*.

Als u het formaat van de client verhogen, moet u ook de paginagrootte van de op de server te verhogen. Zie ["hoe: pas de grootte van de tabel wisselbestand"](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) voor de stappen om dit te doen.

**Objective-C**:

```objc
  MSPullSettings *pullSettings = [[MSPullSettings alloc] initWithPageSize:3];
  [table  pullWithQuery:query queryId:@nil settings:pullSettings
                        completion:^(NSError * _Nullable error) {
                               if(error) {
                    NSLog(@"ERROR %@", error);
                }
                           }];
```

**SWIFT**:

```swift
let pullSettings = MSPullSettings(pageSize: 3)
table.pullWithQuery(query, queryId:nil, settings: pullSettings) { (error) in
    if let err = error {
        print("ERROR ", err)
    }
}
```

## <a name="inserting"></a>Hoe: gegevens invoegen

Voor het invoegen van een nieuwe tabelrij, maakt u een `NSDictionary` en aanroepen van `table insert`. Als [Dynamische Schema] is ingeschakeld, Azure App Service voor een mobiele back-end genereert automatisch nieuwe kolommen op basis van de `NSDictionary`.

Als `id` is niet opgegeven, wordt de back-end genereert automatisch een nieuwe unieke ID. Geef uw eigen `id` e-mailadres gebruiken adressen, gebruikersnamen, of uw eigen aangepaste waarden als de ID. Bieden van uw eigen ID gemakkelijker joins en database zakelijke logica.

De `result` bevat het nieuwe item dat is ingevoegd. Afhankelijk van uw logische server, heeft deze mogelijk aanvullende of gewijzigde gegevens in vergelijking met het wat is doorgegeven aan de server.

**Objective-C**:

```objc
NSDictionary *newItem = @{@"id": @"custom-id", @"text": @"my new item", @"complete" : @NO};
[table insert:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**SWIFT**:

```swift
let newItem = ["id": "custom-id", "text": "my new item", "complete": false]
table.insert(newItem) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

## <a name="modifying"></a>Hoe: gegevens wijzigen

Voor het bijwerken van een bestaande rij wijzigen van een item en de aanroep `update`:

**Objective-C**:

```objc
NSMutableDictionary *newItem = [oldItem mutableCopy]; // oldItem is NSDictionary
[newItem setValue:@"Updated text" forKey:@"text"];
[table update:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**SWIFT**:

```swift
if let newItem = oldItem.mutableCopy() as? NSMutableDictionary {
    newItem["text"] = "Updated text"
    table2.update(newItem as [NSObject: AnyObject], completion: { (result, error) -> Void in
        if let err = error {
            print("ERROR ", err)
        } else if let item = result {
            print("Todo Item: ", item["text"])
        }
    })
}
```

U kunt ook opgeven de rij-ID en het bijgewerkte veld:

**Objective-C**:

```objc
[table update:@{@"id":@"custom-id", @"text":"my EDITED item"} completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**SWIFT**:

```swift
table.update(["id": "custom-id", "text": "my EDITED item"]) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

Ten minste de `id` kenmerk moet zijn ingesteld bij het maken van updates.

## <a name="deleting"></a>Hoe: gegevens verwijderen

Als u wilt verwijderen van een item, aanroepen `delete` aan het item:

**Objective-C**:

```objc
[table delete:item completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

**SWIFT**:

```swift
table.delete(newItem as [NSObject: AnyObject]) { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

U kunt ook verwijderen door op te geven van een rij-ID:

**Objective-C**:

```objc
[table deleteWithId:@"37BBF396-11F0-4B39-85C8-B319C729AF6D" completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

**SWIFT**:

```swift
table.deleteWithId("37BBF396-11F0-4B39-85C8-B319C729AF6D") { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

Ten minste de `id` kenmerk moet zijn ingesteld wanneer er maken wordt verwijderd.

## <a name="customapi"></a>Hoe: aangepaste API oproept

Met een aangepaste API kunt u geen back-end-functionaliteit beschikbaar maken. Dit hoeft niet te worden toegewezen aan een tabelbewerking. Niet alleen doen krijgt u meer controle over messaging, maar u kunt zelfs lezen/set headers en wijzig de indeling van de hoofdtekst van antwoord. Lees voor meer informatie over het maken van een aangepaste API in de back-end, [aangepaste API's](app-service-mobile-node-backend-how-to-use-server-sdk.md#work-easy-apis)

Voor het aanroepen van een aangepaste API aanroepen `MSClient.invokeAPI`. De aanvraag en respons inhoud worden behandeld als JSON. Gebruik van andere mediatypen, [gebruik de andere overbelasting van `invokeAPI` ] [ 5].  Om een `GET` aanvragen in plaats van een `POST` aanvragen, setparameter `HTTPMethod` naar `"GET"` en de parameter `body` naar `nil` (Aangezien GET-aanvragen beschikken niet over de berichttekst.) Als uw aangepaste API biedt ondersteuning voor andere HTTP-termen, wijzigt u `HTTPMethod` op de juiste wijze.

**Objective-C**:

```objc
[self.client invokeAPI:@"sendEmail"
                  body:@{ @"contents": @"Hello world!" }
            HTTPMethod:@"POST"
            parameters:@{ @"to": @"bill@contoso.com", @"subject" : @"Hi!" }
               headers:nil
            completion: ^(NSData *result, NSHTTPURLResponse *response, NSError *error) {
                if(error) {
                    NSLog(@"ERROR %@", error);
                } else {
                    // Do something with result
                }
            }];
```

**SWIFT**:

```swift
client.invokeAPI("sendEmail",
            body: [ "contents": "Hello World" ],
            HTTPMethod: "POST",
            parameters: [ "to": "bill@contoso.com", "subject" : "Hi!" ],
            headers: nil)
            {
                (result, response, error) -> Void in
                if let err = error {
                    print("ERROR ", err)
                } else if let res = result {
                          // Do something with result
                }
        }
```

## <a name="templates"></a>Hoe: Register pushen sjablonen platformonafhankelijke meldingen verzenden

Voor het registreren van sjablonen, geeft u de sjablonen met uw **client.push registerDeviceToken** methode in uw client-app.

**Objective-C**:

```objc
[client.push registerDeviceToken:deviceToken template:iOSTemplate completion:^(NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    }
}];
```

**SWIFT**:

```swift
client.push?.registerDeviceToken(NSData(), template: iOSTemplate, completion: { (error) in
    if let err = error {
        print("ERROR ", err)
    }
})
```

De sjablonen zijn van het type NSDictionary en kunnen bevatten meerdere sjablonen in de volgende indeling:

**Objective-C**:

```objc
NSDictionary *iOSTemplate = @{ @"templateName": @{ @"body": @{ @"aps": @{ @"alert": @"$(message)" } } } };
```

**SWIFT**:

```swift
let iOSTemplate = ["templateName": ["body": ["aps": ["alert": "$(message)"]]]]
```

Alle tags worden verwijderd uit de aanvraag voor beveiliging.  Zie voor informatie over het toevoegen van tags op installaties of -sjablonen in installaties [werken met de .NET back-endserver SDK voor Azure Mobile Apps][4].  Om meldingen te verzenden met behulp van deze geregistreerde sjablonen, werken met [Notification Hubs-API's][3].

## <a name="errors"></a>Hoe: verwerken van fouten

Wanneer u een mobiele back-end voor Azure App Service aanroept, wordt het blok voltooiing bevat een `NSError` parameter. Wanneer er een fout optreedt, wordt met deze parameter niet nul is. U moet in uw code, controleert u deze parameter en fout indien nodig, af te handelen, zoals geïllustreerd in de voorgaande codefragmenten.

Het bestand [ `<WindowsAzureMobileServices/MSError.h>` ] [ 6] definieert de constanten `MSErrorResponseKey`, `MSErrorRequestKey`, en `MSErrorServerItemKey`. Om meer gegevens met betrekking tot de fout:

**Objective-C**:

```objc
NSDictionary *serverItem = [error.userInfo objectForKey:MSErrorServerItemKey];
```

**SWIFT**:

```swift
let serverItem = error.userInfo[MSErrorServerItemKey]
```

Het bestand definieert bovendien constanten voor elke foutcode:

**Objective-C**:

```objc
if (error.code == MSErrorPreconditionFailed) {
```

**SWIFT**:

```swift
if (error.code == MSErrorPreconditionFailed) {
```

## <a name="adal"></a>Hoe: verificatie van gebruikers met de Active Directory-Verificatiebibliotheek

Gebruikers aanmelden bij uw toepassing met behulp van Azure Active Directory kunt u de Active Directory Authentication Library (ADAL). Met behulp van een id-provider SDK-client stroom verificatie is het beter voor het gebruik van de `loginWithProvider:completion:` methode.  Clientverificatie voor de stroom biedt een meer systeemeigen UX uiterlijk en kunt u extra aanpassingen.

1. Uw mobiele app back-end voor AAD-aanmelding configureren door de [App Service configureren voor Active Directory-aanmelding] [ 7] zelfstudie. Zorg ervoor dat u de optionele stap voor het registreren van een systeemeigen clienttoepassing. Voor iOS-, raden wij aan dat de omleidings-URI van het formulier is `<app-scheme>://<bundle-id>`. Zie voor meer informatie de [ADAL iOS snelstartgids][8].
2. ADAL met Cocoapods installeren. Bewerk uw Podfile zodanig dat de definitie van de volgende vervangen **uw PROJECT** met de naam van uw Xcode-project:

        source 'https://github.com/CocoaPods/Specs.git'
        link_with ['YOUR-PROJECT']
        xcodeproj 'YOUR-PROJECT'

   en de schil:

        pod 'ADALiOS'

3. Met behulp van de Terminal en voer `pod install` uit de map waarin u uw project en open vervolgens de gegenereerde Xcode-werkruimte (niet op het project).
4. Voeg de volgende code aan uw toepassing, op basis van de taal die u gebruikt. In elk, moet u deze vervangingen:

   * Vervang **INSERT-instantie-HERE** met de naam van de tenant waarin u uw toepassing hebt ingericht. De indeling moet https://login.microsoftonline.com/contoso.onmicrosoft.com. Deze waarde kan worden gekopieerd vanaf het tabblad domein in uw Azure Active Directory in de [Azure Portal].
   * Vervang **INSERT-RESOURCE-ID-HERE** met de client-ID voor de back-end van uw mobiele app. U vindt de client-ID van de **Geavanceerd** tabblad onder **Azure Active Directory-instellingen** in de portal.
   * Vervang **INSERT-CLIENT-ID-HERE** met de client-ID die u hebt gekopieerd uit de toepassing native client.
   * Vervang **INSERT-OMLEIDINGS-URI-HERE** met van uw site */.auth/login/done* eindpunt, met behulp van het HTTPS-schema. Deze waarde moet zijn vergelijkbaar met *https://contoso.azurewebsites.net/.auth/login/done*.

**Objective-C**:

```objc
#import <ADALiOS/ADAuthenticationContext.h>
#import <ADALiOS/ADAuthenticationSettings.h>
// ...
- (void) authenticate:(UIViewController*) parent
            completion:(void (^) (MSUser*, NSError*))completionBlock;
{
    NSString *authority = @"INSERT-AUTHORITY-HERE";
    NSString *resourceId = @"INSERT-RESOURCE-ID-HERE";
    NSString *clientId = @"INSERT-CLIENT-ID-HERE";
    NSURL *redirectUri = [[NSURL alloc]initWithString:@"INSERT-REDIRECT-URI-HERE"];
    ADAuthenticationError *error;
    ADAuthenticationContext *authContext = [ADAuthenticationContext authenticationContextWithAuthority:authority error:&error];
    authContext.parentController = parent;
    [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
    [authContext acquireTokenWithResource:resourceId
                                    clientId:clientId
                                redirectUri:redirectUri
                            completionBlock:^(ADAuthenticationResult *result) {
                                if (result.status != AD_SUCCEEDED)
                                {
                                    completionBlock(nil, result.error);;
                                }
                                else
                                {
                                    NSDictionary *payload = @{
                                                            @"access_token" : result.tokenCacheStoreItem.accessToken
                                                            };
                                    [client loginWithProvider:@"aad" token:payload completion:completionBlock];
                                }
                            }];
}
```

**SWIFT**:

```swift
// add the following imports to your bridging header:
//        #import <ADALiOS/ADAuthenticationContext.h>
//        #import <ADALiOS/ADAuthenticationSettings.h>

func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
    let authority = "INSERT-AUTHORITY-HERE"
    let resourceId = "INSERT-RESOURCE-ID-HERE"
    let clientId = "INSERT-CLIENT-ID-HERE"
    let redirectUri = NSURL(string: "INSERT-REDIRECT-URI-HERE")
    var error: AutoreleasingUnsafeMutablePointer<ADAuthenticationError?> = nil
    let authContext = ADAuthenticationContext(authority: authority, error: error)
    authContext.parentController = parent
    ADAuthenticationSettings.sharedInstance().enableFullScreen = true
    authContext.acquireTokenWithResource(resourceId, clientId: clientId, redirectUri: redirectUri) { (result) in
            if result.status != AD_SUCCEEDED {
                completion(nil, result.error)
            }
            else {
                let payload: [String: String] = ["access_token": result.tokenCacheStoreItem.accessToken]
                client.loginWithProvider("aad", token: payload, completion: completion)
            }
        }
}
```

## <a name="facebook-sdk"></a>Hoe: verificatie van gebruikers met de Facebook-SDK voor iOS

De Facebook-SDK voor iOS kunt u gebruikers zich in uw toepassing met Facebook.  Met behulp van een stroom clientverificatie is het beter voor het gebruik van de `loginWithProvider:completion:` methode.  De verificatie van de client-stroom biedt een meer systeemeigen UX uiterlijk en kunt u extra aanpassingen.

1. Uw mobiele app back-end voor Facebook-aanmelding configureren door de [App Service configureren voor aanmelding via Facebook] [ 9] zelfstudie.
2. De Facebook-SDK voor iOS installeren door de [Facebook SDK voor iOS - aan de slag] [ 10] documentatie. In plaats van het maken van een app, kunt u het iOS-platform toevoegen aan uw bestaande registreren.
3. Van Facebook-documentatie bevat enkele Objective-C-code in de gemachtigde voor de App. Als u **Swift**, kunt u de volgende vertalingen voor AppDelegate.swift:

    ```swift
    // Add the following import to your bridging header:
    //        #import <FBSDKCoreKit/FBSDKCoreKit.h>

    func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
        FBSDKApplicationDelegate.sharedInstance().application(application, didFinishLaunchingWithOptions: launchOptions)
        // Add any custom logic here.
        return true
    }

    func application(application: UIApplication, openURL url: NSURL, sourceApplication: String?, annotation: AnyObject?) -> Bool {
        let handled = FBSDKApplicationDelegate.sharedInstance().application(application, openURL: url, sourceApplication: sourceApplication, annotation: annotation)
        // Add any custom logic here.
        return handled
    }
    ```
4. Naast het toevoegen van `FBSDKCoreKit.framework` aan uw project, moet u ook een verwijzing naar toevoegen `FBSDKLoginKit.framework` op dezelfde manier.
5. Voeg de volgende code aan uw toepassing, op basis van de taal die u gebruikt.

    **Objective-C**:

    ```objc
    #import <FBSDKLoginKit/FBSDKLoginKit.h>
    #import <FBSDKCoreKit/FBSDKAccessToken.h>
    // ...
    - (void) authenticate:(UIViewController*) parent
                completion:(void (^) (MSUser*, NSError*)) completionBlock;
    {
        FBSDKLoginManager *loginManager = [[FBSDKLoginManager alloc] init];
        [loginManager
            logInWithReadPermissions: @[@"public_profile"]
            fromViewController:parent
            handler:^(FBSDKLoginManagerLoginResult *result, NSError *error) {
                if (error) {
                    completionBlock(nil, error);
                } else if (result.isCancelled) {
                    completionBlock(nil, error);
                } else {
                    NSDictionary *payload = @{
                                            @"access_token":result.token.tokenString
                                            };
                    [client loginWithProvider:@"facebook" token:payload completion:completionBlock];
                }
            }];
    }
    ```

    **SWIFT**:

    ```swift
    // Add the following imports to your bridging header:
    //        #import <FBSDKLoginKit/FBSDKLoginKit.h>
    //        #import <FBSDKCoreKit/FBSDKAccessToken.h>

    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let loginManager = FBSDKLoginManager()
        loginManager.logInWithReadPermissions(["public_profile"], fromViewController: parent) { (result, error) in
            if (error != nil) {
                completion(nil, error)
            }
            else if result.isCancelled {
                completion(nil, error)
            }
            else {
                let payload: [String: String] = ["access_token": result.token.tokenString]
                client.loginWithProvider("facebook", token: payload, completion: completion)
            }
        }
    }
    ```

## <a name="twitter-fabric"></a>Hoe: verificatie van gebruikers met Twitter Fabric voor iOS

Infrastructuur voor iOS kunt u gebruikers zich in uw toepassing met Twitter. Clientverificatie voor de stroom is het beter voor het gebruik van de `loginWithProvider:completion:` methode, zoals deze biedt een meer systeemeigen UX uiterlijk en kunt u extra aanpassingen.

1. Uw mobiele app back-end voor aanmelding bij Twitter configureren door de [App Service configureren voor aanmelding met Twitter](../app-service/app-service-mobile-how-to-configure-twitter-authentication.md) zelfstudie.
2. Fabric toevoegen aan uw project door de [Infrastructuur voor iOS - aan de slag] documentatie en het instellen van TwitterKit.

   > [!NOTE]
   > Fabric maakt standaard een Twitter-toepassing voor u. Het maken van een toepassing registreert de Consumer Key- en Consumer Secret die u eerder hebt met de volgende codefragmenten hebt gemaakt, kunt u voorkomen.    U kunt ook kunt u de sleutel van de consument- en Consumer Secret waarden die u in App Service met de waarden die u ziet opgeeft in vervangen de [Dashboard voor Infrastructuurresources]. Als u deze optie kiest, moet u de callback-URL instellen op een plaatsaanduidingswaarde zoals `https://<yoursitename>.azurewebsites.net/.auth/login/twitter/callback`.

    Als u gebruiken de geheimen die u eerder hebt gemaakt wilt, moet u de volgende code toevoegen aan de gemachtigde voor uw App:

    **Objective-C**:

    ```objc
    #import <Fabric/Fabric.h>
    #import <TwitterKit/TwitterKit.h>
    // ...
    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
    {
        [[Twitter sharedInstance] startWithConsumerKey:@"your_key" consumerSecret:@"your_secret"];
        [Fabric with:@[[Twitter class]]];
        // Add any custom logic here.
        return YES;
    }
    ```

    **SWIFT**:

    ```swift
    import Fabric
    import TwitterKit
    // ...
    func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
        Twitter.sharedInstance().startWithConsumerKey("your_key", consumerSecret: "your_secret")
        Fabric.with([Twitter.self])
        // Add any custom logic here.
        return true
    }
    ```

3. Voeg de volgende code aan uw toepassing, op basis van de taal die u gebruikt.

    **Objective-C**:

    ```objc
    #import <TwitterKit/TwitterKit.h>
    // ...
    - (void)authenticate:(UIViewController*)parent completion:(void (^) (MSUser*, NSError*))completionBlock
    {
        [[Twitter sharedInstance] logInWithCompletion:^(TWTRSession *session, NSError *error) {
            if (session) {
                NSDictionary *payload = @{
                                            @"access_token":session.authToken,
                                            @"access_token_secret":session.authTokenSecret
                                        };
                [client loginWithProvider:@"twitter" token:payload completion:completionBlock];
            } else {
                completionBlock(nil, error);
            }
        }];
    }
    ```

    **SWIFT**:

    ```swift
    import TwitterKit
    // ...
    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let client = self.table!.client
        Twitter.sharedInstance().logInWithCompletion { session, error in
            if (session != nil) {
                let payload: [String: String] = ["access_token": session!.authToken, "access_token_secret": session!.authTokenSecret]
                client.loginWithProvider("twitter", token: payload, completion: completion)
            } else {
                completion(nil, error)
            }
        }
    }
    ```

## <a name="google-sdk"></a>Hoe: verificatie van gebruikers met de Google-In SDK voor iOS

De Google-In SDK voor iOS kunt u gebruikers Meld u aan bij uw toepassing met behulp van een Google-account.  Google onlangs aangekondigd wijzigingen in hun OAuth-beveiligingsbeleid.  Deze wijzigingen in het beleid moet het gebruik van de Google-SDK in de toekomst.

1. Uw mobiele app back-end voor aanmelding bij Google configureren door de [App Service configureren voor aanmelding bij Google](../app-service/app-service-mobile-how-to-configure-google-authentication.md) zelfstudie.
2. De Google-SDK voor iOS installeren door de [Start Google Sign-In voor iOS - integratie](https://developers.google.com/identity/sign-in/ios/start-integrating) documentatie. De sectie 'Verifiëren met een back-endserver', kunt u overslaan.
3. Voeg het volgende toe van de gemachtigde `signIn:didSignInForUser:withError:` methode op basis van de taal die u gebruikt.

    **Objective-C**:
    ```objc
    NSDictionary *payload = @{
                                @"id_token":user.authentication.idToken,
                                @"authorization_code":user.serverAuthCode
                                };

    [client loginWithProvider:@"google" token:payload completion:^(MSUser *user, NSError *error) {
        // ...
    }];
    ```

    **SWIFT**:

    ```swift
    let payload: [String: String] = ["id_token": user.authentication.idToken, "authorization_code": user.serverAuthCode]
    client.loginWithProvider("google", token: payload) { (user, error) in
        // ...
    }
    ```

4. Zorg ervoor dat u ook het volgende toevoegen aan `application:didFinishLaunchingWithOptions:` in de gemachtigde voor uw app, "SERVER_CLIENT_ID" vervangen door de dezelfde ID die u gebruikt voor het configureren van App Service in stap 1.

    **Objective-C**:

    ```objc
    [GIDSignIn sharedInstance].serverClientID = @"SERVER_CLIENT_ID";
    ```

     **SWIFT**:

    ```swift
    GIDSignIn.sharedInstance().serverClientID = "SERVER_CLIENT_ID"
    ```

5. De volgende code toevoegen aan uw toepassing in een UIViewController waarmee de `GIDSignInUIDelegate` protocol op basis van de taal die u gebruikt.  U bent afgemeld voordat opnieuw wordt ondertekend en hoewel u niet hoeft uw referenties opnieuw invoeren, verschijnt een dialoogvenster voor toestemming.  Deze methode alleen aanroepen wanneer het sessietoken is verlopen.

   **Objective-C**:

    ```objc
    #import <Google/SignIn.h>
    // ...
    - (void)authenticate
    {
            [GIDSignIn sharedInstance].uiDelegate = self;
            [[GIDSignIn sharedInstance] signOut];
            [[GIDSignIn sharedInstance] signIn];
    }
    ```

   **SWIFT**:

    ```swift
    // ...
    func authenticate() {
        GIDSignIn.sharedInstance().uiDelegate = self
        GIDSignIn.sharedInstance().signOut()
        GIDSignIn.sharedInstance().signIn()
    }
    ```

<!-- Anchors. -->

[What is Mobile Services]: #what-is
[Concepts]: #concepts
[Setup and Prerequisites]: #Setup
[How to: Create the Mobile Services client]: #create-client
[How to: Create a table reference]: #table-reference
[How to: Query data from a mobile service]: #querying
[Filter returned data]: #filtering
[Sort returned data]: #sorting
[Return data in pages]: #paging
[Select specific columns]: #selecting
[How to: Bind data to the user interface]: #binding
[How to: Insert data into a mobile service]: #inserting
[How to: Modify data in a mobile service]: #modifying
[How to: Authenticate users]: #authentication
[Cache authentication tokens]: #caching-tokens
[How to: Upload images and large files]: #blobs
[How to: Handle errors]: #errors
[How to: Design unit tests]: #unit-testing
[How to: Customize the client]: #customizing
[Customize request headers]: #custom-headers
[Customize data type serialization]: #custom-serialization
[Next Steps]: #next-steps
[How to: Use MSQuery]: #query-object

<!-- Images. -->

<!-- URLs. -->
[Azure Mobile Apps snel starten]: app-service-mobile-ios-get-started.md

[Add Mobile Services to Existing App]: /develop/mobile/tutorials/get-started-data
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Validate and modify data in Mobile Services by using server scripts]: /develop/mobile/tutorials/validate-modify-and-augment-data-ios
[Mobile Services SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[Authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[iOS SDK]: https://developer.apple.com/xcode
[Azure Portal]: https://portal.azure.com/
[Handling Expired Tokens]: https://go.microsoft.com/fwlink/p/?LinkId=301955
[Live Connect SDK]: https://go.microsoft.com/fwlink/p/?LinkId=301960
[Permissions]: https://msdn.microsoft.com/library/windowsazure/jj193161.aspx
[Service-side Authorization]: mobile-services-javascript-backend-service-side-authorization.md
[Use scripts to authorize users]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[Dynamische Schema]: https://go.microsoft.com/fwlink/p/?LinkId=296271
[How to: access custom parameters]: /develop/mobile/how-to-guides/work-with-server-scripts#access-headers
[Create a table]: https://msdn.microsoft.com/library/windowsazure/jj193162.aspx
[NSDictionary object]: https://go.microsoft.com/fwlink/p/?LinkId=301965
[ASCII control codes C0 and C1]: https://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[CLI to manage Mobile Services tables]: /cli/azure/get-started-with-az-cli2
[Conflict-Handler]: mobile-services-ios-handling-conflicts-offline-data.md#add-conflict-handling

[Dashboard voor infrastructuurresources]: https://www.fabric.io/home
[Infrastructuur voor iOS - aan de slag]: https://docs.fabric.io/ios/fabric/getting-started.html
[1]: https://github.com/Azure/azure-mobile-apps-ios-client/blob/master/README.md#ios-client-sdk
[2]: https://azure.github.io/azure-mobile-apps-ios-client/
[3]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[4]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags
[5]: https://azure.github.io/azure-mobile-services/iOS/v3/Classes/MSClient.html#//api/name/invokeAPI:data:HTTPMethod:parameters:headers:completion:
[6]: https://github.com/Azure/azure-mobile-services/blob/master/sdk/iOS/src/MSError.h
[7]: ../app-service/app-service-mobile-how-to-configure-active-directory-authentication.md
[8]:../active-directory/develop/quickstart-v1-ios.md
[9]: ../app-service/app-service-mobile-how-to-configure-facebook-authentication.md
[10]: https://developers.facebook.com/docs/ios/getting-started
