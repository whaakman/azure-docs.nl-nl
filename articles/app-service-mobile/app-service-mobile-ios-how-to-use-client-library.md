---
title: Hoe gebruik iOS SDK voor Azure Mobile Apps
description: Hoe gebruik iOS SDK voor Azure Mobile Apps
services: app-service\mobile
documentationcenter: ios
author: ysxu
manager: yochayk
editor: 
ms.assetid: 4e8e45df-c36a-4a60-9ad4-393ec10b7eb9
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 10/01/2016
ms.author: yuaxu
ms.openlocfilehash: 63dd283605553297a7dc8feab90c8bcbd716d5de
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2017
---
# <a name="how-to-use-ios-client-library-for-azure-mobile-apps"></a>Hoe gebruik iOS-clientbibliotheek voor Azure Mobile Apps
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Deze handleiding leert u veelvoorkomende scenario's met behulp van de meest recente uitvoeren [iOS SDK voor Azure Mobile Apps][1]. Als u niet bekend met Azure Mobile Apps bent, eerst voltooien [Azure Mobile Apps Quick Start] voor het maken van een back-end van een tabel maken en een vooraf samengestelde iOS Xcode-project downloaden. In deze handleiding richten we op de client-side iOS SDK. Zie voor meer informatie over de SDK-serverzijde voor de back-end, de HOWTOs van de SDK-Server.

## <a name="reference-documentation"></a>Referentiedocumentatie
De documentatie bij de client-iOS SDK bevindt zich hier: [Azure Mobile Apps iOS referentie Client][2].

## <a name="supported-platforms"></a>Ondersteunde Platforms
De iOS SDK biedt ondersteuning voor Objective-C-projecten, Swift 2.2 projecten en Swift 2.3 projecten voor iOS-versie 8.0 of hoger.

De verificatie 'server-flow' maakt gebruik van een webweergave voor de gebruikersinterface weergegeven.  Als het apparaat is niet een UI WebView presentatie mogelijk en vervolgens een andere verificatiemethode is vereist is die buiten het bereik van het product.  
Deze SDK is derhalve niet geschikt voor controle-type of op dezelfde manier beperkte apparaten.

## <a name="Setup"></a>Het installatieprogramma en vereisten
Deze handleiding wordt ervan uitgegaan dat u een back-end hebt gemaakt met een tabel. Deze handleiding wordt ervan uitgegaan dat de tabel heeft van hetzelfde schema als de tabellen in deze zelfstudies. Deze handleiding wordt ervan uitgegaan dat in uw code u verwijzen naar `MicrosoftAzureMobile.framework` en importeer `MicrosoftAzureMobile/MicrosoftAzureMobile.h`.

## <a name="create-client"></a>How to: Client maken
Als u een back-end van Azure Mobile Apps in uw project, maak een `MSClient`. Vervang `AppUrl` met de app-URL. U laten `gatewayURLString` en `applicationKey` leeg. Als u een gateway voor verificatie instelt, vullen `gatewayURLString` met de URL van de gateway.

**Objective-C**:

```
MSClient *client = [MSClient clientWithApplicationURLString:@"AppUrl"];
```

**SWIFT**:

```
let client = MSClient(applicationURLString: "AppUrl")
```


## <a name="table-reference"></a>How to: tabelverwijzing maken
Maak een verwijzing naar de back-endtabel als u gegevens wilt bekijken of bijwerken. Vervang `TodoItem` door de naam van uw tabel

**Objective-C**:

```
MSTable *table = [client tableWithName:@"TodoItem"];
```

**SWIFT**:

```
let table = client.tableWithName("TodoItem")
```


## <a name="querying"></a>Procedure: een Query over gegevens
Query voor het maken van een databasequery uitvoeren op de `MSTable` object. De volgende query haalt alle items in `TodoItem` en registreert de tekst van elk item.

**Objective-C**:

```
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

```
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

## <a name="filtering"></a>How to: Filter gegevens geretourneerd
Om resultaten te filteren, zijn er veel opties beschikbaar.

Als u wilt filteren met behulp van een predicaat, gebruiken een `NSPredicate` en `readWithPredicate`. De volgende filters resultaatgegevens om alleen onvolledige Todo-items te vinden.

**Objective-C**:

```
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

```
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

## <a name="query-object"></a>How to: MSQuery gebruiken
Als een complexe query (inclusief sortering en paginering) uitvoert, maakt u een `MSQuery` object, rechtstreeks of via een predikaat:

**Objective-C**:

```
MSQuery *query = [table query];
MSQuery *query = [table queryWithPredicate: [NSPredicate predicateWithFormat:@"complete == NO"]];
```

**SWIFT**:

```
let query = table.query()
let query = table.queryWithPredicate(NSPredicate(format: "complete == NO"))
```

`MSQuery`kunt u verschillende query gedrag bepalen.

* Geef de volgorde van resultaten
* Beperken welke velden u wilt retourneren
* Hoeveel records om terug te beperken
* Totaal aantal opgeven om in het antwoord
* Aangepaste queryreeksparameters in aanvraag opgeven
* Aanvullende functies toepassen

Uitvoeren van een `MSQuery` query door het aanroepen van `readWithCompletion` op het object.

## <a name="sorting"></a>How to: gegevens met MSQuery sorteren
Om te sorteren resultaten, kunt u een voorbeeld gaan we kijken. Aanroepen om te sorteren op het veld 'text' oplopende, vervolgens op het 'complete' aflopende, `MSQuery` als volgt te werk:

**Objective-C**:

```
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

```
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


## <a name="selecting"></a><a name="parameters"></a>How to: beperken van velden en vouw queryreeksparameters met MSQuery
Geef de namen van de velden in om te beperken velden moeten worden geretourneerd in een query, het **selectFields** eigenschap. In dit voorbeeld retourneert alleen de tekst en voltooide velden:

**Objective-C**:

```
query.selectFields = @[@"text", @"complete"];
```

**SWIFT**:

```
query.selectFields = ["text", "complete"]
```

Als u wilt aanvullende queryreeksparameters opnemen in de aanvraag van de server (bijvoorbeeld omdat ze maakt gebruik van een aangepast script van de server-side), vullen `query.parameters` als volgt te werk:

**Objective-C**:

```
query.parameters = @{
    @"myKey1" : @"value1",
    @"myKey2" : @"value2",
};
```

**SWIFT**:

```
query.parameters = ["myKey1": "value1", "myKey2": "value2"]
```

## <a name="paging"></a>How to: paginaformaat configureren
Met Azure Mobile Apps bepaalt de grootte van het aantal records dat tegelijk uit de back-end-tabellen worden opgehaald. Een aanroep van `pull` gegevens zou vervolgens batch-up van gegevens, op basis van dit paginaformaat tot er zijn geen records meer om op te halen.

Het is mogelijk voor het configureren van een pagina grootte met **MSPullSettings** zoals hieronder wordt weergegeven. Grootte van de pagina is 50 en het onderstaande voorbeeld gewijzigd in 3.

U kunt een ander paginaformaat Prestatieoverwegingen configureren. Als u een groot aantal kleine gegevensrecords hebt, minder een hoge paginaformaat server retourbewerkingen.

Deze instelling bepaalt de paginagrootte aan de clientzijde. Als de client om een pagina groter vraagt dan de back-end van Mobile Apps wordt ondersteund, is het paginaformaat beperkt tot het maximum dat de back-end is geconfigureerd voor ondersteuning.

Deze instelling is ook de *getal* gegevensrecords, niet de *bytegrootte*.

Als u het formaat van de client verhoogt, moet u ook de paginagrootte van de op de server te verhogen. Zie [' How to: pas de grootte van de tabel paginering '](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) voor de stappen om dit te doen.

**Objective-C**:

```
  MSPullSettings *pullSettings = [[MSPullSettings alloc] initWithPageSize:3];
  [table  pullWithQuery:query queryId:@nil settings:pullSettings
                        completion:^(NSError * _Nullable error) {
                               if(error) {
                    NSLog(@"ERROR %@", error);
                }
                           }];
```


**SWIFT**:

```
let pullSettings = MSPullSettings(pageSize: 3)
table.pullWithQuery(query, queryId:nil, settings: pullSettings) { (error) in
    if let err = error {
        print("ERROR ", err)
    }
}
```

## <a name="inserting"></a>How to: gegevens invoegen
Voor het invoegen van een rij in een nieuwe tabel maken van een `NSDictionary` en oproepen `table insert`. Als [dynamische Schema] is ingeschakeld, de Azure App Service mobiele back-end genereert automatisch nieuwe kolommen op basis van de `NSDictionary`.

Als `id` is niet opgegeven, wordt de back-end genereert automatisch een nieuwe unieke ID. Geef uw eigen `id` e-mailadres gebruiken adressen, gebruikersnamen, of uw eigen aangepaste waarden als de ID. Uw eigen ID bieden gemakkelijker joins en database zakelijke logica.

De `result` bevat het nieuwe item dat is ingevoegd. Afhankelijk van de logische server, kan deze extra of gewijzigde gegevens vergeleken met wat is doorgegeven aan de server hebben.

**Objective-C**:

```
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

```
let newItem = ["id": "custom-id", "text": "my new item", "complete": false]
table.insert(newItem) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

## <a name="modifying"></a>How to: gegevens wijzigen
Voor het bijwerken van een bestaande rij wijzigen van een item en de aanroep `update`:

**Objective-C**:

```
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

```
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

U kunt ook de rij-ID en het bijgewerkte veld opgeven:

**Objective-C**:

```
[table update:@{@"id":@"custom-id", @"text":"my EDITED item"} completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**SWIFT**:

```
table.update(["id": "custom-id", "text": "my EDITED item"]) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

Ten minste de `id` kenmerk moet worden ingesteld bij het maken van updates.

## <a name="deleting"></a>How to: gegevens verwijderen
Als u wilt een item verwijdert, worden aangeroepen `delete` met het item:

**Objective-C**:

```
[table delete:item completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

**SWIFT**:

```
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

```
[table deleteWithId:@"37BBF396-11F0-4B39-85C8-B319C729AF6D" completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

**SWIFT**:

```
table.deleteWithId("37BBF396-11F0-4B39-85C8-B319C729AF6D") { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

Ten minste de `id` kenmerk moet worden ingesteld wanneer u wordt verwijderd.

## <a name="customapi"></a>How to: aangepaste API aanroepen
Met een aangepaste API kan geen back-end-functies worden blootgesteld. Heeft geen toewijzen aan een tabel-bewerking. Niet alleen doet u meer controle krijgen over messaging, maar u kunt zelfs lezen/set headers en wijzigt u de indeling van het antwoord-instantie. Lees voor meer informatie over het maken van een aangepaste API gebruiken op de back-end, [aangepaste API's](app-service-mobile-node-backend-how-to-use-server-sdk.md#work-easy-apis)

Voor het aanroepen van aangepaste API aanroepen `MSClient.invokeAPI`. De aanvraag en antwoord inhoud worden behandeld als JSON. Andere typen media [gebruik de andere overbelasting van `invokeAPI` ] [ 5].  Om ervoor een `GET` aanvragen in plaats van een `POST` aanvraagt, setparameter `HTTPMethod` naar `"GET"` en parameter `body` naar `nil` (omdat de GET-aanvragen beschikt niet over de berichttekst.) Als uw aangepaste API andere HTTP-termen ondersteunt, wijzigt u `HTTPMethod` op de juiste wijze.

**Objective-C**:

```
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

```
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

## <a name="templates"></a>Hoe: Register push sjablonen om platformoverschrijdende meldingen te verzenden
Voor het registreren van sjablonen doorgeven sjablonen met uw **client.push registerDeviceToken** methode in uw client-app.

**Objective-C**:

```
[client.push registerDeviceToken:deviceToken template:iOSTemplate completion:^(NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    }
}];
```

**SWIFT**:

```
    client.push?.registerDeviceToken(NSData(), template: iOSTemplate, completion: { (error) in
        if let err = error {
            print("ERROR ", err)
        }
    })
```

Uw sjablonen van het type NSDictionary en kunnen meerdere sjablonen in de volgende indeling bevatten:

**Objective-C**:

```
NSDictionary *iOSTemplate = @{ @"templateName": @{ @"body": @{ @"aps": @{ @"alert": @"$(message)" } } } };
```

**SWIFT**:

```
let iOSTemplate = ["templateName": ["body": ["aps": ["alert": "$(message)"]]]]
```

Alle codes worden verwijderd van de aanvraag voor beveiliging.  Zie voor informatie over het toevoegen van labels op installaties of -sjablonen in installaties [werken met de .NET-back-endserver SDK voor Azure Mobile Apps][4].  Om meldingen te verzenden met behulp van deze geregistreerde sjablonen, werken met [Notification Hubs-API's][3].

## <a name="errors"></a>How to: afhandelen van fouten
Wanneer u een Azure App Service mobiele back-end aanroept, wordt het blok voltooiing bevat een `NSError` parameter. Wanneer er een fout optreedt, wordt met deze parameter niet nul is. U moet deze parameter inschakelt en verwerken van de fout naar behoefte, zoals wordt beschreven in de voorgaande codefragmenten in uw code.

Het bestand [ `<WindowsAzureMobileServices/MSError.h>` ] [ 6] definieert de constanten `MSErrorResponseKey`, `MSErrorRequestKey`, en `MSErrorServerItemKey`. Ophalen van meer gegevens met betrekking tot de volgende fout:

**Objective-C**:

```
NSDictionary *serverItem = [error.userInfo objectForKey:MSErrorServerItemKey];
```

**SWIFT**:

```
let serverItem = error.userInfo[MSErrorServerItemKey]
```

Het bestand definieert bovendien constanten voor elke foutcode:

**Objective-C**:

```
if (error.code == MSErrorPreconditionFailed) {
```

**SWIFT**:

```
if (error.code == MSErrorPreconditionFailed) {
```

## <a name="adal"></a>Procedure: verificatie van gebruikers met de Active Directory Authentication Library
U kunt de Active Directory Authentication Library (ADAL) gebruiken voor het ondertekenen van gebruikers in uw toepassing met Azure Active Directory. Stroom clientverificatie met een id-provider SDK heeft de voorkeur boven met behulp van de `loginWithProvider:completion:` methode.  Stroom clientverificatie biedt een meer systeemeigen UX idee en kunt u extra aanpassingen.

1. Uw mobiele app back-end voor aanmelding bij de AAD instellen door de [App Service configureren voor Active Directory-aanmelding] [ 7] zelfstudie. Zorg ervoor dat de optionele stap voor het registreren van een systeemeigen clienttoepassing van voltooien. Voor iOS-, we raden aan dat de omleidings-URI van het formulier is `<app-scheme>://<bundle-id>`. Zie voor meer informatie de [ADAL iOS Quick Start][8].
2. Installeren met behulp van Cocoapods ADAL. Uw Podfile zodanig dat de volgende definitie bewerken vervangen **uw PROJECT** met de naam van uw Xcode-project:

        source 'https://github.com/CocoaPods/Specs.git'
        link_with ['YOUR-PROJECT']
        xcodeproj 'YOUR-PROJECT'

   en een schil:

        pod 'ADALiOS'
3. Met behulp van de Terminal en voer `pod install` uit de directory waarin u uw project en open vervolgens de gegenereerde Xcode-werkruimte (niet op het project).
4. De volgende code toevoegen aan uw toepassing, volgens de taal die u gebruikt. In elk, moet u deze vervangingen:

   * Vervang **INSERT-instantie-hier** met de naam van de tenant waarin u uw toepassing hebt ingericht. De indeling moet https://login.microsoftonline.com/contoso.onmicrosoft.com. Deze waarde kan worden gekopieerd vanaf het tabblad domein in uw Azure Active Directory in de [Azure-portal].
   * Vervang **INSERT RESOURCE-ID hier** met de client-ID voor uw back-end voor de mobiele app. U vindt de client-ID van de **Geavanceerd** tabblad onder **Azure Active Directory-instellingen** in de portal.
   * Vervang **INSERT-CLIENT-ID-hier** met de client-ID die u hebt gekopieerd uit de native client-toepassing.
   * Vervang **INSERT-OMLEIDINGS-URI-hier** aan uw site */.auth/login/done* eindpunt, met behulp van het HTTPS-schema. Deze waarde moet er ongeveer als *https://contoso.azurewebsites.net/.auth/login/done*.

**Objective-C**:

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


**SWIFT**:

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

## <a name="facebook-sdk"></a>Procedure: verificatie van gebruikers met de Facebook-SDK voor iOS
U kunt de Facebook SDK voor iOS gebruiken voor het ondertekenen van gebruikers in uw toepassing met Facebook.  Met behulp van een stroom clientverificatie heeft de voorkeur boven met behulp van de `loginWithProvider:completion:` methode.  De verificatie van de client-stroom biedt een meer systeemeigen UX idee en kunt u extra aanpassingen.

1. Uw mobiele app back-end voor aanmelding bij Facebook instellen door de [App Service configureren voor aanmelding Facebook] [ 9] zelfstudie.
2. De Facebook-SDK voor iOS installeren door de [Facebook SDK voor iOS - aan de slag] [ 10] documentatie. In plaats van een app maken, kunt u het iOS-platform toevoegen aan uw bestaande registratie.
3. Facebook van documentatie bevat enkele Objective-C-code in de gemachtigde voor de App. Als u **Swift**, kunt u de volgende vertalingen voor AppDelegate.swift:

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
4. Naast het toevoegen van `FBSDKCoreKit.framework` aan uw project een verwijzing naar ook toevoegen `FBSDKLoginKit.framework` op dezelfde manier.
5. De volgende code toevoegen aan uw toepassing, volgens de taal die u gebruikt.

**Objective-C**:

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

**SWIFT**:

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

## <a name="twitter-fabric"></a>Procedure: verificatie van gebruikers met Twitter Fabric voor iOS
Fabric voor iOS kunt u gebruikers zich aanmelden in uw toepassing met Twitter. Verificatie van client-stroom heeft de voorkeur boven met behulp van de `loginWithProvider:completion:` methode, zoals deze biedt een meer systeemeigen UX idee en kunt u extra aanpassingen.

1. Instellen van uw mobiele app back-end voor Twitter aanmelding door de [App Service configureren voor aanmelding Twitter](../app-service/app-service-mobile-how-to-configure-twitter-authentication.md) zelfstudie.
2. Fabric toevoegen aan uw project door de [Fabric voor iOS - aan de slag] documentatie en TwitterKit in te stellen.

   > [!NOTE]
   > Standaard Fabric Twitter-toepassing voor u gemaakt. U kunt voorkomen dat een toepassing maken via het registreren van de Consumer-sleutel en de consumentgeheim eerder met de volgende codefragmenten hebt gemaakt.    U kunt ook kunt u de sleutel van de Consumer en consumentgeheim waarden die u in App Service met de waarden die u ziet opgeeft in vervangen de [Dashboard voor Infrastructuurresources]. Als u deze optie kiest, moet u de callback-URL, zoals ingesteld in een tijdelijke aanduidingswaarde, `https://<yoursitename>.azurewebsites.net/.auth/login/twitter/callback`.
   >
   >

    Als u gebruiken de geheimen die u eerder hebt gemaakt wilt, moet u de volgende code toevoegen aan uw App gemachtigde:

    **Objective-C**:

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

    **SWIFT**:

        import Fabric
        import TwitterKit
        // ...
        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
            Twitter.sharedInstance().startWithConsumerKey("your_key", consumerSecret: "your_secret")
            Fabric.with([Twitter.self])
            // Add any custom logic here.
            return true
        }
3. De volgende code toevoegen aan uw toepassing, volgens de taal die u gebruikt.

**Objective-C**:

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

**SWIFT**:

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

## <a name="google-sdk"></a>Procedure: verificatie van gebruikers met de Google-In SDK voor iOS
U kunt de Google-In SDK voor iOS gebruiken voor het ondertekenen van gebruikers in uw toepassing met behulp van een Google-account.  Google aangekondigd onlangs wijzigingen in hun OAuth-beveiligingsbeleid.  Deze wijzigingen vereisen het gebruik van de Google-SDK in de toekomst.

1. Instellen van uw mobiele app back-end voor Google aanmelding door de [het configureren van App Service voor Google aanmelding](../app-service/app-service-mobile-how-to-configure-google-authentication.md) zelfstudie.
2. Installeer de Google SDK voor iOS door de [Google Sign-In voor iOS - Start integreren](https://developers.google.com/identity/sign-in/ios/start-integrating) documentatie. U kunt de sectie 'Verifiëren met een back-endserver' overslaan.
3. Voeg het volgende toe aan uw gemachtigde `signIn:didSignInForUser:withError:` methode volgens de taal die u gebruikt.

**Objective-C**:

        NSDictionary *payload = @{
                                  @"id_token":user.authentication.idToken,
                                  @"authorization_code":user.serverAuthCode
                                  };

        [client loginWithProvider:@"google" token:payload completion:^(MSUser *user, NSError *error) {
            // ...
        }];

**SWIFT**:

        let payload: [String: String] = ["id_token": user.authentication.idToken, "authorization_code": user.serverAuthCode]
        client.loginWithProvider("google", token: payload) { (user, error) in
            // ...
        }

1. Zorg ervoor dat u ook het volgende toevoegen aan `application:didFinishLaunchingWithOptions:` in uw app gemachtigde, 'SERVER_CLIENT_ID' met dezelfde ID die u gebruikt voor het configureren van App Service in stap 1 vervangen.

**Objective-C**:

         [GIDSignIn sharedInstance].serverClientID = @"SERVER_CLIENT_ID";

 **SWIFT**:

        GIDSignIn.sharedInstance().serverClientID = "SERVER_CLIENT_ID"


1. De volgende code toevoegen aan uw toepassing in een UIViewController waarmee de `GIDSignInUIDelegate` protocol, volgens de taal die u gebruikt.  U bent afgemeld voordat opnieuw wordt ondertekend en hoewel u niet hoeft uw referenties opnieuw invoeren, ziet u een dialoogvenster toestemming.  Deze methode pas aanroepen wanneer het sessietoken is verlopen.

   **Objective-C**:

       #import <Google/SignIn.h>
       // ...
       - (void)authenticate
       {
               [GIDSignIn sharedInstance].uiDelegate = self;
               [[GIDSignIn sharedInstance] signOut];
               [[GIDSignIn sharedInstance] signIn];
        }

   **SWIFT**:

       // ...
       func authenticate() {
           GIDSignIn.sharedInstance().uiDelegate = self
           GIDSignIn.sharedInstance().signOut()
           GIDSignIn.sharedInstance().signIn()
       }

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
[Azure Mobile Apps Quick Start]: app-service-mobile-ios-get-started.md

[Add Mobile Services to Existing App]: /develop/mobile/tutorials/get-started-data
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Validate and modify data in Mobile Services by using server scripts]: /develop/mobile/tutorials/validate-modify-and-augment-data-ios
[Mobile Services SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[Authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[iOS SDK]: https://developer.apple.com/xcode
[Azure-portal]: https://portal.azure.com/
[Handling Expired Tokens]: http://go.microsoft.com/fwlink/p/?LinkId=301955
[Live Connect SDK]: http://go.microsoft.com/fwlink/p/?LinkId=301960
[Permissions]: http://msdn.microsoft.com/library/windowsazure/jj193161.aspx
[Service-side Authorization]: mobile-services-javascript-backend-service-side-authorization.md
[Use scripts to authorize users]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[dynamische Schema]: http://go.microsoft.com/fwlink/p/?LinkId=296271
[How to: access custom parameters]: /develop/mobile/how-to-guides/work-with-server-scripts#access-headers
[Create a table]: http://msdn.microsoft.com/library/windowsazure/jj193162.aspx
[NSDictionary object]: http://go.microsoft.com/fwlink/p/?LinkId=301965
[ASCII control codes C0 and C1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[CLI to manage Mobile Services tables]: /cli/azure/get-started-with-az-cli2
[Conflict-Handler]: mobile-services-ios-handling-conflicts-offline-data.md#add-conflict-handling

[Dashboard voor Infrastructuurresources]: https://www.fabric.io/home
[Fabric voor iOS - aan de slag]: https://docs.fabric.io/ios/fabric/getting-started.html
[1]: https://github.com/Azure/azure-mobile-apps-ios-client/blob/master/README.md#ios-client-sdk
[2]: http://azure.github.io/azure-mobile-apps-ios-client/
[3]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[4]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags
[5]: http://azure.github.io/azure-mobile-services/iOS/v3/Classes/MSClient.html#//api/name/invokeAPI:data:HTTPMethod:parameters:headers:completion:
[6]: https://github.com/Azure/azure-mobile-services/blob/master/sdk/iOS/src/MSError.h
[7]: ../app-service/app-service-mobile-how-to-configure-active-directory-authentication.md
[8]: ../active-directory/active-directory-devquickstarts-ios.md
[9]: ../app-service/app-service-mobile-how-to-configure-facebook-authentication.md
[10]: https://developers.facebook.com/docs/ios/getting-started
