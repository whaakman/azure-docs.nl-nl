---
title: IOS-SDK voor Azure Mobile Apps gebruiken
description: IOS-SDK voor Azure Mobile Apps gebruiken
services: app-service\mobile
documentationcenter: ios
author: elamalani
editor: ''
ms.assetid: 4e8e45df-c36a-4a60-9ad4-393ec10b7eb9
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 06/25/2019
ms.author: emalani
ms.openlocfilehash: 898bf082874a1e9bf26dd094a6a0fe55417c9d8e
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2019
ms.locfileid: "68851067"
---
# <a name="how-to-use-ios-client-library-for-azure-mobile-apps"></a>IOS-client bibliotheek gebruiken voor Azure Mobile Apps

[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

> [!NOTE]
> Visual Studio App Center is investeren in nieuwe en geïntegreerde services die in de ontwikkeling van mobiele apps kunnen worden ontwikkeld. Ontwikkel aars kunnen services **bouwen**, **testen** en **distribueren** om een continue integratie-en leverings pijplijn in te stellen. Zodra de app is geïmplementeerd, kunnen ontwikkel aars de status en het gebruik van hun app bewaken met behulp van de **analyse** -en **diagnose** Services en gebruikers benaderen met behulp van de **Push** service. Ontwikkel aars kunnen ook gebruikmaken van **auth** voor het verifiëren van hun gebruikers en **gegevens** service om app-gegevens in de Cloud op te slaan en te synchroniseren. Bekijk [app Center](https://appcenter.ms/?utm_source=zumo&utm_campaign=/app-service-mobile-ios-how-to-use-client-library) vandaag nog.
>

## <a name="overview"></a>Overzicht
Deze hand leiding leert u veelvoorkomende scenario's uit te voeren met de nieuwste [Azure Mobile apps IOS SDK][1]. Als u nog geen ervaring hebt met Azure Mobile Apps, voltooit u eerst [azure Mobile Apps Quick Start] om een back-end te maken, een tabel te maken en een vooraf gebaseerd IOS Xcode-project te downloaden. In deze hand leiding richten we ons op de iOS-SDK aan de client zijde. Zie de server SDK HOWTOs voor meer informatie over de SDK aan de server zijde voor de back-end.

## <a name="reference-documentation"></a>Referentiedocumentatie

De referentie documentatie voor de iOS-client-SDK bevindt zich hier: [Naslag informatie voor Azure Mobile apps IOS-client][2].

## <a name="supported-platforms"></a>Ondersteunde platforms

De iOS-SDK ondersteunt doel-C-projecten, Swift 2,2-projecten en snelle 2,3-projecten voor iOS-versies 8,0 of hoger.

De verificatie server stroom gebruikt een webweergave voor de weer gegeven gebruikers interface.  Als het apparaat geen gebruikers interface voor webweergave kan presen teren, is een andere verificatie methode vereist die buiten het bereik van het product valt.  
Deze SDK is dus niet geschikt voor controle-en soort gelijke apparaten.

## <a name="Setup"></a>Installatie en vereisten

In deze hand leiding wordt ervan uitgegaan dat u een back-end hebt gemaakt met een tabel. In deze hand leiding wordt ervan uitgegaan dat de tabel hetzelfde schema heeft als de tabellen in deze zelf studies. In deze hand leiding wordt ervan uitgegaan dat u in uw `MicrosoftAzureMobile.framework` code verwijst `MicrosoftAzureMobile/MicrosoftAzureMobile.h`naar en importeert.

## <a name="create-client"></a>Procedures: Client maken

Als u toegang wilt krijgen tot een Azure Mobile Apps-back- `MSClient`end in uw project, maakt u een. Vervang `AppUrl` door de app-URL. U kunt deze `gatewayURLString` `applicationKey` leeg laten. Als u een gateway voor verificatie instelt, vult `gatewayURLString` u de URL van de gateway in.

**Objective-C**:

```objc
MSClient *client = [MSClient clientWithApplicationURLString:@"AppUrl"];
```

**Swift**:

```swift
let client = MSClient(applicationURLString: "AppUrl")
```

## <a name="table-reference"></a>Procedures: Tabel verwijzing maken

Maak een verwijzing naar de back-endtabel als u gegevens wilt bekijken of bijwerken. Vervang `TodoItem` door de naam van uw tabel

**Objective-C**:

```objc
MSTable *table = [client tableWithName:@"TodoItem"];
```

**Swift**:

```swift
let table = client.tableWithName("TodoItem")
```

## <a name="querying"></a>Procedures: Query gegevens

Als u een database query wilt maken, `MSTable` moet u een query uitvoeren op het object. Met de volgende query worden alle items in `TodoItem` opgehaald en wordt de tekst van elk item geregistreerd.

**Objective-C**:

```objc
[table readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) { // error is nil if no error occurred
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) { // items is NSArray of records that match query
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Swift**:

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

## <a name="filtering"></a>Procedures: Geretourneerde gegevens filteren

Er zijn veel beschik bare opties om de resultaten te filteren.

Als u wilt filteren met behulp van een `NSPredicate` predikaat, gebruikt u een en. `readWithPredicate` Met de volgende filters worden gegevens geretourneerd om alleen onvolledige TODO-items te vinden.

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

**Swift**:

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

## <a name="query-object"></a>Procedures: MSQuery gebruiken

Als u een complexe query wilt uitvoeren (met inbegrip van sorteren en `MSQuery` pagineren), maakt u een object rechtstreeks of met behulp van een predikaat:

**Objective-C**:

```objc
MSQuery *query = [table query];
MSQuery *query = [table queryWithPredicate: [NSPredicate predicateWithFormat:@"complete == NO"]];
```

**Swift**:

```swift
let query = table.query()
let query = table.queryWithPredicate(NSPredicate(format: "complete == NO"))
```

`MSQuery`Hiermee kunt u verschillende query gedrag beheren.

* Volg orde van resultaten opgeven
* Beperken welke velden moeten worden geretourneerd
* Beperk het aantal records dat moet worden geretourneerd
* Geef het totale aantal op in antwoord
* Aangepaste query teken reeks parameters opgeven in de aanvraag
* Aanvullende functies Toep assen

Voer een `MSQuery` query uit door `readWithCompletion` op het object te roepen.

## <a name="sorting"></a>Procedures: Gegevens sorteren met MSQuery

We kijken naar een voor beeld om de resultaten te sorteren. Als u wilt sorteren op veld ' tekst ', gevolgd door ' volledig ' `MSQuery` , kunt u het volgende doen:

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

**Swift**:

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

## <a name="selecting"></a><a name="parameters"></a>Procedures: Velden beperken en query reeks parameters uitbreiden met MSQuery

Als u de velden die in een query worden geretourneerd wilt beperken, geeft u de namen van de velden in de eigenschap **selectFields** op. In dit voor beeld worden alleen de velden tekst en voltooid geretourneerd:

**Objective-C**:

```objc
query.selectFields = @[@"text", @"complete"];
```

**Swift**:

```swift
query.selectFields = ["text", "complete"]
```

Als u aanvullende query teken reeks parameters wilt toevoegen aan de server aanvraag (bijvoorbeeld omdat er een aangepast script aan de server zijde gebruikmaakt) `query.parameters` , gaat u als volgt te werk:

**Objective-C**:

```objc
query.parameters = @{
    @"myKey1" : @"value1",
    @"myKey2" : @"value2",
};
```

**Swift**:

```swift
query.parameters = ["myKey1": "value1", "myKey2": "value2"]
```

## <a name="paging"></a>Procedures: Pagina grootte configureren

Met Azure Mobile Apps bepaalt het pagina formaat het aantal records dat wordt opgehaald uit de back-end-tabellen. Een aanroep `pull` van gegevens zou vervolgens de gegevens op basis van deze pagina grootte batcheert, totdat er geen records meer worden opgehaald.

Het is mogelijk om een pagina grootte te configureren met behulp van **MSPullSettings** , zoals hieronder wordt weer gegeven. De standaard pagina grootte is 50, en in het volgende voor beeld wordt deze gewijzigd in 3.

U kunt om prestatie redenen een andere pagina grootte configureren. Als u een groot aantal kleine gegevens records hebt, vermindert een hoge pagina grootte het aantal retouren van de server.

Met deze instelling bepaalt u alleen de pagina grootte aan de client zijde. Als de client vraagt om een grotere pagina grootte dan de Mobile Apps back-end ondersteunt, wordt de pagina grootte beperkt tot het maximum dat de back-end heeft geconfigureerd om te ondersteunen.

Deze instelling is ook het *aantal* gegevens records, niet de *byte grootte*.

Als u het pagina formaat van de client verg root, moet u ook de pagina grootte op de server verg Roten. Zie [' How to: Wijzig de grootte](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) van de tabel paginering voor de stappen die u moet uitvoeren.

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

**Swift**:

```swift
let pullSettings = MSPullSettings(pageSize: 3)
table.pullWithQuery(query, queryId:nil, settings: pullSettings) { (error) in
    if let err = error {
        print("ERROR ", err)
    }
}
```

## <a name="inserting"></a>Procedures: Gegevens invoegen

Als u een nieuwe tabelrij wilt invoegen, maakt `NSDictionary` u een `table insert`en aanroepen. Als [dynamisch schema] is ingeschakeld, worden met de mobiele back-end van Azure app service automatisch nieuwe `NSDictionary`kolommen gegenereerd op basis van de.

Als `id` niet wordt vermeld, wordt door de back-end automatisch een nieuwe unieke id gegenereerd. Geef uw eigen `id` instellingen op voor het gebruik van e-mail adressen, gebruikers namen of uw eigen aangepaste waarden als id. Het opgeven van uw eigen ID kan samen voegingen en de logica van de bedrijfs gerichte data base vereenvoudigen.

Het `result` bevat het nieuwe item dat is ingevoegd. Afhankelijk van de server logica zijn er mogelijk extra of gewijzigde gegevens in vergelijking met wat er is door gegeven aan de-server.

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

**Swift**:

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

## <a name="modifying"></a>Procedures: Gegevens wijzigen

Als u een bestaande rij wilt bijwerken, wijzigt u een `update`item en roept u het volgende aan:

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

**Swift**:

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

U kunt ook de rij-ID en het bijgewerkte veld opgeven:

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

**Swift**:

```swift
table.update(["id": "custom-id", "text": "my EDITED item"]) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

Het kenmerk moet mini `id` maal worden ingesteld bij het maken van updates.

## <a name="deleting"></a>Procedures: Gegevens verwijderen

Als u een item wilt verwijderen `delete` , roept u het item aan:

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

**Swift**:

```swift
table.delete(newItem as [NSObject: AnyObject]) { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

U kunt ook verwijderen door een rij-ID op te geven:

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

**Swift**:

```swift
table.deleteWithId("37BBF396-11F0-4B39-85C8-B319C729AF6D") { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

U moet mini maal `id` het kenmerk instellen bij het verwijderen.

## <a name="customapi"></a>Procedures: Aangepaste API aanroepen

Met een aangepaste API kunt u alle back-end-functies zichtbaar maken. Deze hoeft niet te worden toegewezen aan een tabel bewerking. U krijgt niet alleen meer controle over berichten, maar u kunt zelfs kopteksten lezen/instellen en de indeling van de antwoord tekst wijzigen.

Voor het aanroepen van een aangepaste `MSClient.invokeAPI`API roept u de aanroep. De aanvraag-en antwoord inhoud worden behandeld als JSON. Als u andere media typen wilt gebruiken, [gebruikt u de `invokeAPI`andere overbelasting van ][5].  Als u een `GET` aanvraag wilt indienen in `POST` plaats van een aanvraag, `"GET"` stelt u para `nil` meter `HTTPMethod` in op en para meter `body` in (aangezien Get-aanvragen geen bericht tekst hebben.) Als uw aangepaste API andere HTTP-woorden ondersteunt, wijzigt `HTTPMethod` u de juiste.

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

**Swift**:

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

## <a name="templates"></a>Procedures: Push-sjablonen registreren om cross-platform meldingen te verzenden

Als u sjablonen wilt registreren, geeft u sjablonen door met de methode **client. push registerDeviceToken** in uw client-app.

**Objective-C**:

```objc
[client.push registerDeviceToken:deviceToken template:iOSTemplate completion:^(NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    }
}];
```

**Swift**:

```swift
client.push?.registerDeviceToken(NSData(), template: iOSTemplate, completion: { (error) in
    if let err = error {
        print("ERROR ", err)
    }
})
```

Uw sjablonen zijn van het type NSDictionary en kunnen meerdere sjablonen bevatten in de volgende indeling:

**Objective-C**:

```objc
NSDictionary *iOSTemplate = @{ @"templateName": @{ @"body": @{ @"aps": @{ @"alert": @"$(message)" } } } };
```

**Swift**:

```swift
let iOSTemplate = ["templateName": ["body": ["aps": ["alert": "$(message)"]]]]
```

Alle labels worden verwijderd uit de aanvraag voor beveiliging.  Als u labels wilt toevoegen aan installaties of sjablonen binnen installaties, raadpleegt u [werken met de .net back-end-server-SDK voor Azure Mobile apps][4].  Als u meldingen wilt verzenden met behulp van deze geregistreerde sjablonen, kunt u werken met [Notification hubs-api's][3].

## <a name="errors"></a>Procedures: Fouten afhandelen

Wanneer u een Azure app service mobiele back-end aanroept, bevat het `NSError` voltooiings blok een para meter. Als er een fout optreedt, is deze para meter niet-Nil. In uw code moet u deze para meter controleren en de fout zo nodig afhandelen, zoals wordt getoond in de voor gaande code fragmenten.

Het bestand [`<WindowsAzureMobileServices/MSError.h>`][6] definieert de constanten `MSErrorResponseKey`, `MSErrorRequestKey`en `MSErrorServerItemKey`. Meer informatie over de fout:

**Objective-C**:

```objc
NSDictionary *serverItem = [error.userInfo objectForKey:MSErrorServerItemKey];
```

**Swift**:

```swift
let serverItem = error.userInfo[MSErrorServerItemKey]
```

Daarnaast definieert het bestand constanten voor elke fout code:

**Objective-C**:

```objc
if (error.code == MSErrorPreconditionFailed) {
```

**Swift**:

```swift
if (error.code == MSErrorPreconditionFailed) {
```

## <a name="adal"></a>Procedures: Gebruikers verifiëren met de Active Directory Authentication Library

U kunt de Active Directory Authentication Library (ADAL) gebruiken voor het ondertekenen van gebruikers in uw toepassing met behulp van Azure Active Directory. Verificatie van client stromen met behulp van een id-provider-SDK `loginWithProvider:completion:` is de voor keur aan het gebruik van de-methode.  Verificatie van client stromen biedt een meer systeem eigen UX en maakt het mogelijk om extra aanpassingen aan te brengen.

1. Configureer de back-end van uw mobiele app voor AAD-aanmelding door de zelf studie [app service voor Active Directory-aanmelding configureren te][7] volgen. Zorg ervoor dat u de optionele stap voor het registreren van een systeem eigen client toepassing hebt voltooid. Voor iOS is het raadzaam om de omleidings-URI van `<app-scheme>://<bundle-id>`het formulier te maken. Raadpleeg de [ADAL IOS Quick][8]start voor meer informatie.
2. Installeer ADAL met behulp van Cocoapods. Bewerk uw Podfile voor het toevoegen van de volgende definitie, waarbij **u uw-project** vervangt door de naam van uw Xcode-project:

        source 'https://github.com/CocoaPods/Specs.git'
        link_with ['YOUR-PROJECT']
        xcodeproj 'YOUR-PROJECT'

   en de Pod:

        pod 'ADALiOS'

3. Gebruik de terminal om uit `pod install` te voeren vanuit de map met uw project en open vervolgens de gegenereerde Xcode-werk ruimte (niet het project).
4. Voeg de volgende code toe aan uw toepassing, afhankelijk van de taal die u gebruikt. Voer de volgende vervangingen uit:

   * Vervang de **INVOEG instantie** door de naam van de Tenant waarin u uw toepassing hebt ingericht. De notatie moet zijn https://login.microsoftonline.com/contoso.onmicrosoft.com. Deze waarde kan worden gekopieerd van het tabblad domein in uw Azure Active Directory in de [Azure-portal].
   * Vervang de **Insert-resource-id hier** door de client-id voor de back-end van uw mobiele app. U kunt de client-ID verkrijgen via het tabblad **Geavanceerd** onder **Azure Active Directory instellingen** in de portal.
   * Vervang **Insert-client-id-hier** door de client-id die u hebt gekopieerd uit de systeem eigen client toepassing.
   * Vervang **Insert-redirect-Uri-hier** met het */.auth/login/done* -eind punt van uw site met behulp van het HTTPS-schema. Deze waarde moet gelijk zijn aan *https://contoso.azurewebsites.net/.auth/login/done* .

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

**Swift**:

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

## <a name="facebook-sdk"></a>Procedures: Gebruikers verifiëren met de Facebook SDK voor iOS

U kunt de Facebook-SDK voor iOS gebruiken om gebruikers te ondertekenen in uw toepassing met Facebook.  Het gebruik van een client stroom verificatie verdient de voor keur `loginWithProvider:completion:` aan het gebruik van de-methode.  De client stroom verificatie biedt een meer systeem eigen UX en maakt het mogelijk om extra aanpassingen aan te brengen.

1. Configureer de back-end van uw mobiele app voor Facebook-aanmelding door de zelf studie [app service voor Facebook-aanmelding configureren te][9] volgen.
2. Installeer de Facebook-SDK voor iOS door de documentatie [van de Facebook-SDK voor IOS-aan de slag][10] te volgen. In plaats van een app te maken, kunt u het iOS-platform toevoegen aan uw bestaande registratie.
3. De documentatie van Facebook bevat een aantal objectieve C-code in de gemachtigde van de app. Als u **Swift**gebruikt, kunt u de volgende vertalingen gebruiken voor AppDelegate. Swift:

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
4. Naast het toevoegen `FBSDKCoreKit.framework` aan uw project, moet u ook een verwijzing naar `FBSDKLoginKit.framework` op dezelfde manier toevoegen.
5. Voeg de volgende code toe aan uw toepassing, afhankelijk van de taal die u gebruikt.

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

    **Swift**:

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

## <a name="twitter-fabric"></a>Procedures: Gebruikers verifiëren met Twitter Fabric voor iOS

U kunt infra structuur voor iOS gebruiken om gebruikers te ondertekenen in uw toepassing met behulp van Twitter. Client flow-verificatie verdient de voor keur aan `loginWithProvider:completion:` het gebruik van de-methode, omdat deze een meer systeem eigen UX-ervaring biedt en extra aanpassing mogelijk maakt.

1. Configureer de back-end van uw mobiele app voor Twitter-aanmelding door de zelf studie [app service voor Twitter-aanmelding configureren te](../app-service/configure-authentication-provider-twitter.md) volgen.
2. Voeg infrastructuur resources toe aan uw project door de documentatie [Infra structuur voor iOS-aan de slag] te volgen en TwitterKit in te stellen.

   > [!NOTE]
   > Infra structuur maakt standaard een Twitter-toepassing voor u. U kunt voor komen dat u een toepassing maakt door de consument sleutel en het consument geheim te registreren dat u eerder hebt gemaakt met behulp van de volgende code fragmenten.    U kunt ook de door u verstrekte waarden App Service voor de consumenten sleutel en het consument geheim vervangen door de waarden die u in het [infrastructuur dashboard]ziet. Als u deze optie kiest, moet u ervoor zorgen dat u de URL voor terugbellen instelt op een `https://<yoursitename>.azurewebsites.net/.auth/login/twitter/callback`waarde voor de tijdelijke aanduiding, zoals.

    Als u ervoor kiest om de geheimen te gebruiken die u eerder hebt gemaakt, voegt u de volgende code toe aan de gemachtigde van de app:

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

    **Swift**:

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

3. Voeg de volgende code toe aan uw toepassing, afhankelijk van de taal die u gebruikt.

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

    **Swift**:

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

## <a name="google-sdk"></a>Procedures: Gebruikers verifiëren met de Google Sign-in SDK voor iOS

U kunt de Google-aanmeld-SDK voor iOS gebruiken om gebruikers in uw toepassing te ondertekenen met een Google-account.  Google heeft onlangs wijzigingen in het OAuth-beveiligings beleid aangekondigd.  Met deze beleids wijzigingen is het gebruik van de Google SDK in de toekomst vereist.

1. Configureer de back-end van uw mobiele app voor Google-aanmelding door de zelf studie [app service voor Google-aanmelding configureren te](../app-service/configure-authentication-provider-google.md) volgen.
2. Installeer de Google SDK voor iOS door de [Google-aanmeldings voor IOS te](https://developers.google.com/identity/sign-in/ios/start-integrating) volgen. U kunt de sectie verifiëren met een back-end-server overs Laan.
3. Voeg het volgende toe aan de methode `signIn:didSignInForUser:withError:` van uw gemachtigde, afhankelijk van de taal die u gebruikt.

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

    **Swift**:

    ```swift
    let payload: [String: String] = ["id_token": user.authentication.idToken, "authorization_code": user.serverAuthCode]
    client.loginWithProvider("google", token: payload) { (user, error) in
        // ...
    }
    ```

4. Zorg ervoor dat u `application:didFinishLaunchingWithOptions:` in de gemachtigde van de app ook het volgende toevoegt, waarbij u SERVER_CLIENT_ID vervangt door de id die u hebt gebruikt om app service te configureren in stap 1.

    **Objective-C**:

    ```objc
    [GIDSignIn sharedInstance].serverClientID = @"SERVER_CLIENT_ID";
    ```

     **Swift**:

    ```swift
    GIDSignIn.sharedInstance().serverClientID = "SERVER_CLIENT_ID"
    ```

5. Voeg de volgende code toe aan uw toepassing in een UIViewController die het `GIDSignInUIDelegate` protocol implementeert, afhankelijk van de taal die u gebruikt.  U bent afgemeld voordat u zich opnieuw aanmeldt, maar u hoeft uw referenties niet opnieuw in te voeren. u ziet dan een dialoog venster voor toestemming.  Roep deze methode alleen aan als het sessie token is verlopen.

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

   **Swift**:

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
[Azure Mobile Apps Quick Start]: app-service-mobile-ios-get-started.md

[Add Mobile Services to Existing App]: /develop/mobile/tutorials/get-started-data
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Validate and modify data in Mobile Services by using server scripts]: /develop/mobile/tutorials/validate-modify-and-augment-data-ios
[Mobile Services SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[Authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[iOS SDK]: https://developer.apple.com/xcode
[Azure-portal]: https://portal.azure.com/
[Handling Expired Tokens]: https://go.microsoft.com/fwlink/p/?LinkId=301955
[Live Connect SDK]: https://go.microsoft.com/fwlink/p/?LinkId=301960
[Permissions]: https://msdn.microsoft.com/library/windowsazure/jj193161.aspx
[Service-side Authorization]: mobile-services-javascript-backend-service-side-authorization.md
[Use scripts to authorize users]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[Dynamisch schema]: https://go.microsoft.com/fwlink/p/?LinkId=296271
[How to: access custom parameters]: /develop/mobile/how-to-guides/work-with-server-scripts#access-headers
[Create a table]: https://msdn.microsoft.com/library/windowsazure/jj193162.aspx
[NSDictionary object]: https://go.microsoft.com/fwlink/p/?LinkId=301965
[ASCII control codes C0 and C1]: https://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[CLI to manage Mobile Services tables]: /cli/azure/get-started-with-az-cli2
[Conflict-Handler]: mobile-services-ios-handling-conflicts-offline-data.md#add-conflict-handling

[Infrastructuur dashboard]: https://www.fabric.io/home
[Infra structuur voor iOS-aan de slag]: https://docs.fabric.io/ios/fabric/getting-started.html
[1]: https://github.com/Azure/azure-mobile-apps-ios-client/blob/master/README.md#ios-client-sdk
[2]: https://azure.github.io/azure-mobile-apps-ios-client/
[3]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[4]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags
[5]: https://azure.github.io/azure-mobile-services/iOS/v3/Classes/MSClient.html#//api/name/invokeAPI:data:HTTPMethod:parameters:headers:completion:
[6]: https://github.com/Azure/azure-mobile-services/blob/master/sdk/iOS/src/MSError.h
[7]: ../app-service/configure-authentication-provider-aad.md
[8]:../active-directory/develop/quickstart-v1-ios.md
[9]: ../app-service/configure-authentication-provider-facebook.md
[10]: https://developers.facebook.com/docs/ios/getting-started
