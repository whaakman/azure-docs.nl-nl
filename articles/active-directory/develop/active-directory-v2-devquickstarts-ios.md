---
title: Aanmelden toevoegen aan een iOS-toepassing met behulp van het Azure AD v2.0-eindpunt | Microsoft Docs
description: Het bouwen van een iOS-app die gebruikers met beide persoonlijke Microsoft-account aanmeldt en werk- of schoolaccount accounts met behulp van de bibliotheken van derden.
services: active-directory
documentationcenter: 
author: brandwe
manager: mtillman
editor: 
ms.assetid: fd3603c0-42f7-438c-87b5-a52d20d6344b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 01/07/2017
ms.author: brandwe
ms.custom: aaddev
ms.openlocfilehash: 398ddbd004b4a12f4aa79ed64cc85f0e5bc5407a
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="add-sign-in-to-an-ios-app-using-a-third-party-library-with-graph-api-using-the-v20-endpoint"></a>Aanmelden voor een iOS-app met behulp van een derde partij-bibliotheek met Graph API met behulp van het v2.0-eindpunt toevoegen
Op het Microsoft Identity-platform wordt gebruikgemaakt van open standaarden, zoals OAuth2 en OpenID Connect. Ontwikkelaars kunnen een bibliotheek die ze willen integreren in onze services gebruiken. Om te helpen ons platform gebruiken met andere bibliotheken ontwikkelaars, hebben we enkele scenario's zoals deze voorbeelden van het configureren van derden bibliotheken verbinding maken met het identiteitsplatform van Microsoft geschreven. De meeste bibliotheken die implementeren [de RFC6749 OAuth2-specificatie](https://tools.ietf.org/html/rfc6749) verbinding kunnen maken met het identiteitsplatform van Microsoft.

Met de toepassing die in dit scenario maakt, kunnen gebruikers zich aanmelden bij hun organisatie en zoekt u naar anderen binnen hun organisatie met behulp van de Graph API.

Als u geen ervaring met OAuth2 of OpenID Connect, wellicht veel van de configuratie van deze niet verstandig aan u. Het is raadzaam dat u leest [v2.0-protocollen - stromen van OAuth 2.0 autorisatie Code](active-directory-v2-protocols-oauth-code.md) voor de achtergrond.

> [!NOTE]
> Sommige functies van ons platform die u een expressie in de OAuth2 of OpenID Connect standaarden, zoals voorwaardelijke toegang en beheer van Intune-beleid hebt, moeten u onze open-source bibliotheken voor Microsoft Azure identiteit gebruiken.
> 
> 

Het v2.0-eindpunt biedt geen ondersteuning voor alle Azure Active Directory-scenario's en onderdelen.

> [!NOTE]
> Meer informatie over om te bepalen of moet u het v2.0-eindpunt, [v2.0 beperkingen](active-directory-v2-limitations.md).
> 
> 

## <a name="download-code-from-github"></a>Code vanuit GitHub downloaden
De code voor deze zelfstudie wordt onderhouden in [GitHub](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-v2).  Als u wilt volgen, kunt u [basis van de app downloaden als een ZIP-](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) of het geraamte:

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

U kunt ook het voorbeeld downloaden en meteen aan de slag:

```
git clone git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

## <a name="register-an-app"></a>Een app registreren
Maakt een nieuwe app op de [toepassing registratieportal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), of de gedetailleerde stappen op [het registreren van een app met het v2.0-eindpunt](active-directory-v2-app-registration.md).  Zorg ervoor dat:

* Kopieer de **toepassings-Id** die toegewezen aan uw app, omdat u hebt deze snel nodig.
* Voeg de **Mobile** platform voor uw app.
* Kopieer de **omleidings-URI** vanuit de portal. Moet u de standaardwaarde van `urn:ietf:wg:oauth:2.0:oob`.

## <a name="download-the-third-party-nxoauth2-library-and-create-a-workspace"></a>De derde partij NXOAuth2 bibliotheek downloaden en een werkruimte maken
Voor dit scenario moet u de OAuth2Client vanuit GitHub, namelijk een OAuth2-bibliotheek voor Mac OS X- en iOS (Cocoa en Cocoa touch) gebruikt. Deze bibliotheek is gebaseerd op concept 10 van de OAuth2-specificatie. Het systeemeigen toepassingsprofiel implementeert en biedt ondersteuning voor het eindpunt voor autorisatie van de gebruiker. Dit zijn alle dingen die u wilt integreren met het identiteitsplatform van Microsoft.

### <a name="add-the-library-to-your-project-by-using-cocoapods"></a>De bibliotheek toevoegen aan uw project via CocoaPods
CocoaPods is een hulpmiddel voor afhankelijkheidsbeheer voor Xcode-projecten. Hiermee worden de vorige installatiestappen automatisch beheerd.

```
$ vi Podfile
```
1. Voeg het volgende aan de podfile toe:
   
    ```
     platform :ios, '8.0'
   
     target 'QuickStart' do
   
     pod 'NXOAuth2Client'
   
     end
    ```
2. De podfile laden via CocoaPods. Hiermee maakt u een nieuwe Xcode-werkruimte die u gaat laden.
   
    ```
    $ pod install
    ...
    $ open QuickStart.xcworkspace
    ```

## <a name="explore-the-structure-of-the-project"></a>De structuur van het project verkennen
De volgende structuur is ingesteld voor het project in het geraamte opgenomen:

* Een Master-weergave met een zoekopdracht UPN
* Een detailweergave voor de gegevens over de geselecteerde gebruiker
* Een aanmelding weergave waar een gebruiker bij de app aanmelden zich om op te vragen van de grafiek

Er wordt verplaatst naar verschillende bestanden in de basis voor verificatie toevoegen. Andere onderdelen van de code, zoals de visual code, niet van toepassing zijn op identiteit, maar zijn beschikbaar voor u.

## <a name="set-up-the-settingsplst-file-in-the-library"></a>Instellen van het bestand settings.plst in de bibliotheek
* Open in de Quick Start-project het `settings.plist` bestand. Vervang de waarden van de elementen in de sectie in overeenstemming met de waarden die u in de Azure portal gebruikt. Uw code verwijst naar deze waarden als de Active Directory Authentication Library wordt gebruikt.
  * De `clientId` is de client-ID van uw toepassing die u hebt gekopieerd uit de portal.
  * De `redirectUri` is de omleidings-URL die de portal worden opgegeven.

## <a name="set-up-the-nxoauth2client-library-in-your-loginviewcontroller"></a>De bibliotheek NXOAuth2Client in uw LoginViewController instellen
De bibliotheek NXOAuth2Client moet sommige waarden ophalen instellen. Nadat u die taak hebt voltooid, kunt u het verkregen token de Graph-API aan te roepen. Omdat `LoginView` wordt aangeroepen op elk gewenst moment moeten we verifiëren, is het verstandig om configuratiewaarden in aan dat bestand.

* Laten we enkele Voeg waarden toe aan de `LoginViewController.m` bestand in te stellen de context voor verificatie en autorisatie. Meer informatie over de waarden volgt u de code.
  
    ```objc
    NSString *scopes = @"openid offline_access User.Read";
    NSString *authURL = @"https://login.microsoftonline.com/common/oauth2/v2.0/authorize";
    NSString *loginURL = @"https://login.microsoftonline.com/common/login";
    NSString *bhh = @"urn:ietf:wg:oauth:2.0:oob?code=";
    NSString *tokenURL = @"https://login.microsoftonline.com/common/oauth2/v2.0/token";
    NSString *keychain = @"com.microsoft.azureactivedirectory.samples.graph.QuickStart";
    static NSString * const kIDMOAuth2SuccessPagePrefix = @"session_state=";
    NSURL *myRequestedUrl;
    NSURL *myLoadedUrl;
    bool loginFlow = FALSE;
    bool isRequestBusy;
    NSURL *authcode;
    ```

Bekijk meer informatie over de code in.

De eerste tekenreeks is voor `scopes`.  De `User.Read` waarde kunt u lezen van het profiel van de basis van de aangemelde gebruiker.

U kunt meer informatie over de beschikbare scopes op [Microsoft Graph-machtigingsbereiken](https://graph.microsoft.io/docs/authorization/permission_scopes).

Voor `authURL`, `loginURL`, `bhh`, en `tokenURL`, moet u de waarden die eerder is opgegeven. Als u de open-source bibliotheken voor Microsoft Azure identiteit gebruikt, halen we deze gegevens voor u met behulp van onze metagegevenseindpunt. Deze waarden zijn al voor u uitgepakt.

De waarde `keychain` is de container die de bibliotheek NXOAuth2Client gebruikt voor het maken van een sleutelhanger waarin uw tokens worden opgeslagen. Als u wilt ophalen van eenmalige aanmelding (SSO) via talrijke apps, kunt u de dezelfde sleutelketen opgeven in elk van uw toepassingen en aanvragen van het gebruik van die sleutelhanger in uw Xcode-rechten. Dit wordt uitgelegd in de Apple-documentatie.

De rest van deze waarden zijn vereist voor de bibliotheek gebruiken en maken van de plaatsen waar u bij het uitvoeren van de waarden voor de context.

### <a name="create-a-url-cache"></a>Een URL-cache maken
Binnen `(void)viewDidLoad()`, die altijd wordt aangeroepen nadat de weergave wordt geladen, de volgende code primes een cache voor onze gebruik.

Voeg de volgende code toe:

```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    self.loginView.delegate = self;
    [self setupOAuth2AccountStore];
    [self requestOAuth2Access];
    NSURLCache *URLCache = [[NSURLCache alloc] initWithMemoryCapacity:4 * 1024 * 1024
                                                         diskCapacity:20 * 1024 * 1024
                                                             diskPath:nil];
    [NSURLCache setSharedURLCache:URLCache];

}
```

### <a name="create-a-webview-for-sign-in"></a>Maken van een webweergave voor aanmelden
Een webweergave kan de gebruiker gevraagd om aanvullende factoren zoals SMS-bericht (indien geconfigureerd) of foutberichten terug naar de gebruiker. Hier stelt u de webweergave boven en vervolgens de code voor het afhandelen van de retouraanroepen die in de webweergave van de services identiteit gebeurt later te schrijven.

```objc
-(void)requestOAuth2Access {
    //to sign in to Microsoft APIs using OAuth2, we must show an embedded browser (UIWebView)
    [[NXOAuth2AccountStore sharedStore] requestAccessToAccountWithType:@"myGraphService"
                                   withPreparedAuthorizationURLHandler:^(NSURL *preparedURL) {
                                       //navigate to the URL returned by NXOAuth2Client

                                       NSURLRequest *r = [NSURLRequest requestWithURL:preparedURL];
                                       [self.loginView loadRequest:r];
                                   }];
}
```

### <a name="override-the-webview-methods-to-handle-authentication"></a>De WebView-methoden voor het afhandelen van verificatie overschrijven
Om te laten de webweergave wat er gebeurt wanneer een gebruiker aanmelden moet, zoals eerder besproken, kunt u de volgende code plakken.

```objc
- (void)resolveUsingUIWebView:(NSURL *)URL {

    // We get the auth token from a redirect so we need to handle that in the webview.

    if (![NSThread isMainThread]) {
        [self performSelectorOnMainThread:@selector(resolveUsingUIWebView:) withObject:URL waitUntilDone:YES];
        return;
    }

    NSURLRequest *hostnameURLRequest = [NSURLRequest requestWithURL:URL cachePolicy:NSURLRequestUseProtocolCachePolicy timeoutInterval:10.0f];
    isRequestBusy = YES;
    [self.loginView loadRequest:hostnameURLRequest];

    NSLog(@"resolveUsingUIWebView ready (status: UNKNOWN, URL: %@)", self.loginView.request.URL);
}

- (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType {

    NSLog(@"webView:shouldStartLoadWithRequest: %@ (%li)", request.URL, (long)navigationType);

    // The webview is where all the communication happens. Slightly complicated.

    myLoadedUrl = [webView.request mainDocumentURL];
    NSLog(@"***Loaded url: %@", myLoadedUrl);

    //if the UIWebView is showing our authorization URL or consent URL, show the UIWebView control
    if ([request.URL.absoluteString rangeOfString:authURL options:NSCaseInsensitiveSearch].location != NSNotFound) {
        self.loginView.hidden = NO;
    } else if ([request.URL.absoluteString rangeOfString:loginURL options:NSCaseInsensitiveSearch].location != NSNotFound) {
        //otherwise hide the UIWebView, we've left the authorization flow
        self.loginView.hidden = NO;
    } else if ([request.URL.absoluteString rangeOfString:bhh options:NSCaseInsensitiveSearch].location != NSNotFound) {
        //otherwise hide the UIWebView, we've left the authorization flow
        self.loginView.hidden = YES;
        [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
    }
    else {
        self.loginView.hidden = NO;
        //read the Location from the UIWebView, this is how Microsoft APIs is returning the
        //authentication code and relation information. This is controlled by the redirect URL we chose to use from Microsoft APIs
        //continue the OAuth2 flow
       // [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
    }

    return YES;

}
```

### <a name="write-code-to-handle-the-result-of-the-oauth2-request"></a>Code schrijven om het resultaat van de OAuth2-aanvraag af te handelen
De volgende code wordt de URL van de omleiding die als resultaat de webweergave geeft afgehandeld. Als verificatie niet geslaagd is, probeert de code het opnieuw. De fout die u kunt zien in de beheerconsole of asynchroon verwerken krijgt ondertussen van de bibliotheek.

```objc
- (void)handleOAuth2AccessResult:(NSString *)accessResult {

    AppData* data = [AppData getInstance];

    //parse the response for success or failure
     if (accessResult)
    //if success, complete the OAuth2 flow by handling the redirect URL and obtaining a token
     {
         [[NXOAuth2AccountStore sharedStore] handleRedirectURL:accessResult];
    } else {
        //start over
        [self requestOAuth2Access];
    }
}
```

### <a name="set-up-the-oauth-context-called-account-store"></a>Instellen van de OAuth-Context (accountarchief genoemd)
U kunt hier aanroepen `-[NXOAuth2AccountStore setClientID:secret:authorizationURL:tokenURL:redirectURL:forAccountType:]` op de gedeelde accountarchief voor elke service die u wilt toegang krijgen tot de toepassing. Het accounttype is een tekenreeks die wordt gebruikt als een id voor een bepaalde service. Omdat u de Graph API opent, wordt de code verwijst naar als `"myGraphService"`. U instellen een zien die aangeven wanneer iets verandert met het token vervolgens. Nadat u het token, u terugkeert de gebruiker terug naar de `masterView`.

```objc
- (void)setupOAuth2AccountStore {


        AppData* data = [AppData getInstance];

    [[NXOAuth2AccountStore sharedStore] setClientID:data.clientId
                                             secret:data.secret
                                              scope:[NSSet setWithObject:scopes]
                                   authorizationURL:[NSURL URLWithString:authURL]
                                           tokenURL:[NSURL URLWithString:tokenURL]
                                        redirectURL:[NSURL URLWithString:data.redirectUriString]
                                      keyChainGroup: keychain
                                     forAccountType:@"myGraphService"];

    [[NSNotificationCenter defaultCenter] addObserverForName:NXOAuth2AccountStoreAccountsDidChangeNotification
                                                      object:[NXOAuth2AccountStore sharedStore]
                                                       queue:nil
                                                  usingBlock:^(NSNotification *aNotification) {
                                                      if (aNotification.userInfo) {
                                                          //account added, we have access
                                                          //we can now request protected data
                                                          NSLog(@"Success!! We have an access token.");
                                                          dispatch_async(dispatch_get_main_queue(),^ {

                                                              MasterViewController* masterViewController = [self.storyboard instantiateViewControllerWithIdentifier:@"masterView"];
                                                              [self.navigationController pushViewController:masterViewController animated:YES];
                                                          });
                                                      } else {
                                                          //account removed, we lost access
                                                      }
                                                  }];

    [[NSNotificationCenter defaultCenter] addObserverForName:NXOAuth2AccountStoreDidFailToRequestAccessNotification
                                                      object:[NXOAuth2AccountStore sharedStore]
                                                       queue:nil
                                                  usingBlock:^(NSNotification *aNotification) {
                                                      NSError *error = [aNotification.userInfo objectForKey:NXOAuth2AccountStoreErrorKey];
                                                      NSLog(@"Error!! %@", error.localizedDescription);
                                                  }];
}
```

## <a name="set-up-the-master-view-to-search-and-display-the-users-from-the-graph-api"></a>Instellen van de Master-weergave te zoeken en weer van de gebruikers van de Graph API
Een model-View-Controller (MVC)-app die wordt weergegeven van de geretourneerde gegevens in het raster is buiten het bereik van dit scenario en veel online zelfstudies wordt uitgelegd hoe u een bouwen. Alle deze code is in het geraamte bestand. U moet echter te maken met een aantal items in deze MVC-toepassing:

* Wanneer een gebruiker iets in het zoekveld onderscheppen
* Een object van de gegevens terug naar de MasterView bieden, zodat de resultaten kan worden weergegeven in het raster

We doen die hieronder.

### <a name="add-a-check-to-see-if-youre-logged-in"></a>Toevoegen van een selectievakje om te zien als u bent aangemeld
De toepassing biedt weinig als de gebruiker niet is ondertekend, dus is het verstandig om te controleren of er al een token in de cache. Als dat niet het geval is, wordt u omgeleid naar de LoginView voor de gebruiker aan te melden. Als u intrekt, wordt de beste manier acties uitvoeren wanneer een weergave wordt geladen is met de `viewDidLoad()` methode waarmee Apple ons.

```objc
- (void)viewDidLoad {
    [super viewDidLoad];


    NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
    NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];

        if (accounts.count == 0) {

        dispatch_async(dispatch_get_main_queue(),^ {

            LoginViewController* userSelectController = [self.storyboard instantiateViewControllerWithIdentifier:@"LoginUserView"];
            [self.navigationController pushViewController:userSelectController animated:YES];
        });
        }
```

### <a name="update-the-table-view-when-data-is-received"></a>De weergave van de tabel niet bijwerken wanneer gegevens worden ontvangen
Wanneer de Graph API gegevens retourneert, moet u de gegevens worden weergegeven. Voor eenvoud, moet u hier de code voor het bijwerken van de tabel is. U kunt alleen de juiste waarden in uw MVC standaardtekst code plakken.

```objc
#pragma mark - Table View

- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView {
    return 1;
}

- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section {

        return [upnArray count];
}

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {

    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:@"TaskPrototypeCell" forIndexPath:indexPath];

    if ( cell == nil ) {
        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:@"TaskPrototypeCell"];
    }

    User *user = nil;
     user = [upnArray objectAtIndex:indexPath.row];


    // Configure the cell
    cell.textLabel.text = user.name;
    [cell setAccessoryType:UITableViewCellAccessoryDisclosureIndicator];

    return cell;
}

```

### <a name="provide-a-way-to-call-the-graph-api-when-someone-types-in-the-search-field"></a>Bieden een manier om de Graph API aanroepen wanneer iemand in het zoekveld typt
Wanneer een gebruiker typt een zoekopdracht in het vak, moet u die via shove voor Graph API. De `GraphAPICaller` klasse, die u in de volgende code bouwt, scheidt u de lookup-functionaliteit van de presentatie. Nu gaan we de code schrijven waarmee willekeurige tekens search-feeds voor Graph API. We dit doen door op te geven van een methode met de naam `lookupInGraph`, wat de tekenreeks die u zoeken wilt naar duurt.

```objc

-(void)lookupInGraph:(NSString *)searchText {
if (searchText.length > 0) {

    };



        [GraphAPICaller searchUserList:searchText completionBlock:^(NSMutableArray* returnedUpns, NSError* error) {
            if (returnedUpns) {


                upnArray = returnedUpns;


            }
            else
            {
                UIAlertView *alertView = [[UIAlertView alloc]initWithTitle:nil message:[[NSString alloc]initWithFormat:@"Error : %@", error.localizedDescription] delegate:nil cancelButtonTitle:@"Retry" otherButtonTitles:@"Cancel", nil];

                [alertView setDelegate:self];

                dispatch_async(dispatch_get_main_queue(),^ {
                    [alertView show];
                });
            }


        }];


}
```

## <a name="write-a-helper-class-to-access-the-graph-api"></a>Schrijven van een helperklasse voor toegang tot de Graph API
Dit is de kern van de toepassing. Terwijl de rest code in het standaard MVC bij Apple invoegen is, schrijven hier u code voor de grafiek niet opvragen als het gebruikerstypen en ga daarna terug die gegevens. Dit is de code en een gedetailleerde uitleg erop volgt.

### <a name="create-a-new-objective-c-header-file"></a>Maak een nieuw Objective C-header-bestand
Noem het bestand `GraphAPICaller.h`, en voeg de volgende code.

```objc
@interface GraphAPICaller : NSObject<NSURLConnectionDataDelegate>

+(void) searchUserList:(NSString*)searchString
       completionBlock:(void (^) (NSMutableArray*, NSError* error))completionBlock;

@end
```

Hier ziet u dat een opgegeven methode een tekenreeks zijn wordt en een completionBlock retourneert. Deze completionBlock omdat u mogelijk hebt geraden, wordt de tabel bijwerken door te geven van een object met ingevulde gegevens in realtime als de gebruiker.

### <a name="create-a-new-objective-c-file"></a>Maak een nieuw Objective C-bestand
Noem het bestand `GraphAPICaller.m`, en voeg de volgende methode toe.

```objc
+(void) searchUserList:(NSString*)searchString
       completionBlock:(void (^) (NSMutableArray* Users, NSError* error)) completionBlock
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }

    AppData* data = [AppData getInstance];

    NSString *graphURL = [NSString stringWithFormat:@"%@%@/users", data.graphApiUrlString, data.apiversion];

    NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
    NSDictionary* params = [self convertParamsToDictionary:searchString];

    NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];
    [NXOAuth2Request performMethod:@"GET"
                        onResource:[NSURL URLWithString:graphURL]
                   usingParameters:params
                       withAccount:accounts[0]
               sendProgressHandler:^(unsigned long long bytesSend, unsigned long long bytesTotal) {
                   // e.g., update a progress indicator
               }
                   responseHandler:^(NSURLResponse *response, NSData *responseData, NSError *error) {
                       // Process the response
                       if (responseData) {
                           NSError *error;
                           NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:responseData options:0 error:nil];
                           NSLog(@"Graph Response was: %@", dataReturned);

                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                           // Don't be thrown off by the key name being "value". It really is the name of the
                           // first node. :-)

                           //each object is a key value pair
                           NSDictionary *keyValuePairs;
                           NSMutableArray* Users = [[NSMutableArray alloc]init];

                           for(int i =0; i < graphDataArray.count; i++)
                           {
                               keyValuePairs = [graphDataArray objectAtIndex:i];

                               User *s = [[User alloc]init];
                               s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                               s.name =[keyValuePairs valueForKey:@"displayName"];
                               s.mail =[keyValuePairs valueForKey:@"mail"];
                               s.businessPhones =[keyValuePairs valueForKey:@"businessPhones"];
                               s.mobilePhones =[keyValuePairs valueForKey:@"mobilePhone"];


                               [Users addObject:s];
                           }

                           completionBlock(Users, nil);
                       }
                       else
                       {
                           completionBlock(nil, error);
                       }

                   }];
}

```

We gaan met deze methode in detail.

De kern van deze code is in de `NXOAuth2Request`, methode waarmee de parameters die u al hebt gedefinieerd in het bestand settings.plist.

De eerste stap is om de juiste Graph API-aanroep samen te stellen. Omdat u aanroept `/users`, u opgeven dat door deze te voegen aan de resource Graph API samen met de versie. Het zinvol om deze in een bestand met externe instellingen omdat deze wijzigen kunnen, zoals de API ontwikkeld.

```objc
NSString *graphURL = [NSString stringWithFormat:@"%@%@/users", data.graphApiUrlString, data.apiversion];
```

Vervolgens moet u parameters opgeven die u ook voor de Graph API-aanroep biedt. Het is *erg belangrijk* plaats de parameters niet in het resource-eindpunt omdat die is verwijderd voor alle niet-URI die voldoen tekens tijdens runtime. Alle querycode moet worden opgegeven in de parameters.

```objc

NSDictionary* params = [self convertParamsToDictionary:searchString];
```

U wellicht opgevallen dat hiermee een `convertParamsToDictionary` methode die u nog niet hebt geschreven. Laten we dit nu doen aan het einde van het bestand:

```objc
+(NSDictionary*) convertParamsToDictionary:(NSString*)searchString
{
    NSMutableDictionary* dictionary = [[NSMutableDictionary alloc]init];

        NSString *query = [NSString stringWithFormat:@"startswith(givenName, '%@')", searchString];

           [dictionary setValue:query forKey:@"$filter"];



    return dictionary;
}

```
Vervolgens gebruiken we de `NXOAuth2Request` methode terug gegevens ophalen van de API in JSON-indeling.

```objc
NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];
    [NXOAuth2Request performMethod:@"GET"
                        onResource:[NSURL URLWithString:graphURL]
                   usingParameters:params
                       withAccount:accounts[0]
               sendProgressHandler:^(unsigned long long bytesSend, unsigned long long bytesTotal) {
                   // e.g., update a progress indicator
               }
                   responseHandler:^(NSURLResponse *response, NSData *responseData, NSError *error) {
                       // Process the response
                       if (responseData) {
                           NSError *error;
                           NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:responseData options:0 error:nil];
                           NSLog(@"Graph Response was: %@", dataReturned);

                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];
```

Tot slot gaan we kijken hoe u de gegevens terug naar de MasterViewController. De gegevens retourneert als geserialiseerd en moet worden gedeserialiseerd en geladen in een object dat de MainViewController kan gebruiken. Voor dit doel het basisproject heeft een `User.m/h` -bestand dat wordt gemaakt van een gebruikersobject. Vullen van dat object gebruiker met informatie van de grafiek.

```objc
                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                           // Don't be thrown off by the key name being "value". It really is the name of the
                           // first node. :-)

                           //each object is a key value pair
                           NSDictionary *keyValuePairs;
                           NSMutableArray* Users = [[NSMutableArray alloc]init];

                           for(int i =0; i < graphDataArray.count; i++)
                           {
                               keyValuePairs = [graphDataArray objectAtIndex:i];

                               User *s = [[User alloc]init];
                               s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                               s.name =[keyValuePairs valueForKey:@"displayName"];
                               s.mail =[keyValuePairs valueForKey:@"mail"];
                               s.businessPhones =[keyValuePairs valueForKey:@"businessPhones"];
                               s.mobilePhones =[keyValuePairs valueForKey:@"mobilePhone"];


                               [Users addObject:s];
```


## <a name="run-the-sample"></a>Het voorbeeld uitvoert
Als u hebt het basisproject gebruikt of gevolgd samen met de procedure moet nu uw toepassing uitvoeren. De simulator Start en op **aanmelden** om de toepassing te gebruiken.

## <a name="get-security-updates-for-our-product"></a>Beveiligingsupdates voor onze product
We raden u aan wanneer er beveiligingsincidenten door bezoeken optreden meldingen ontvangt de [Security TechCenter](https://technet.microsoft.com/security/dd252948) en u te abonneren op Security Advisory Alerts.

