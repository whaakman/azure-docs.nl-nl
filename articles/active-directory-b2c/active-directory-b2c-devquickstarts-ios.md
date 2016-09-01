<properties
    pageTitle="Azure Active Directory B2C: een web-API aanroepen vanuit een iOS-toepassing met bibliotheken van derden | Microsoft Azure"
    description="In dit artikel wordt beschreven hoe u een iOS-takenlijst-app maakt die een Node.js-web-API aanroept met behulp van OAuth 2.0-bearer-tokens die een bibliotheek van derden gebruiken"
    services="active-directory-b2c"
    documentationCenter="ios"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags ms.service="active-directory-b2c" ms.workload="identity" ms.tgt_pltfrm="na" ms.devlang="objectivec" ms.topic="hero-article"

    ms.date="07/26/2016"
    ms.author="brandwe"/>

# Azure AD B2C: een web-API aanroepen vanuit een iOS-toepassing die een bibliotheek van derden gebruikt

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Op het Microsoft Identity-platform wordt gebruikgemaakt van open standaarden, zoals OAuth2 en OpenID Connect. Ontwikkelaars kunnen daardoor gebruikmaken van elke gewenste bibliotheek die ze met onze services willen integreren. We willen ontwikkelaars graag helpen bij het gebruik van ons platform met andere bibliotheken. Daarom hebben we een aantal scenario's ontwikkeld om te demonstreren hoe ontwikkelaars bibliotheken van derden kunnen configureren om verbinding te maken met het Microsoft Identity-platform. De meeste bibliotheken die de [OAuth2-specificatie RFC6749](https://tools.ietf.org/html/rfc6749) implementeren, kunnen verbinding maken met het Microsoft Identity-platform.


Als u nog geen ervaring hebt met OAuth2 of OpenID Connect, zal een groot gedeelte van deze voorbeeldconfiguratie u niet veel zeggen. U wordt geadviseerd eerst een beknopt [overzicht van het hier gedocumenteerde protocol te bekijken](active-directory-b2c-reference-protocols.md).

> [AZURE.NOTE]
    Voor sommige functies van ons platform die een expressie in deze standaarden hebben, zoals Voorwaardelijke toegang en Intune-beleidsbeheer, moet u onze Microsoft Azure Identity-bibliotheken (open source) gebruiken. 
   
Niet alle Azure Active Directory-scenario's en -functies worden ondersteund door het B2C-platform.  Lees de informatie over [B2C-beperkingen](active-directory-b2c-limitations.md) als u wilt weten of u het B2C-platform moet gebruiken.


## Een Azure AD B2C-directory maken

Voordat u Azure AD B2C kunt gebruiken, moet u een directory, of tenant, maken. Een directory is een container voor alle gebruikers, apps, groepen en meer. Als u nog geen directory hebt, [maakt u een B2C-directory](active-directory-b2c-get-started.md) voordat u verdergaat.

## Een app maken

Vervolgens maakt u een app in uw B2C-directory. Hiermee voorziet u Azure AD van de informatie die nodig is om veilig te communiceren met uw app. Zowel de app als de web-API worden in dit geval aangegeven met één **toepassings-id** omdat ze samen één logische app vormen. Volg [deze instructies](active-directory-b2c-app-registration.md) om een app te maken. Zorg ervoor dat:

- U een **mobiel apparaat** opneemt in de toepassing.
- U de **toepassings-id** kopieert die is toegewezen aan uw app. Deze hebt u ook later nodig.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## Het beleid maken

In Azure AD B2C wordt elke gebruikerservaring gedefinieerd door [beleid](active-directory-b2c-reference-policies.md). Deze app bevat één Identity-ervaring: een gecombineerde aanmelding/registratie. U moet deze beleidsregel maken voor elk type, zoals wordt beschreven in het [naslagartikel voor beleid](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy). Wanneer u het beleid maakt:

- Kiest u **Weergavenaam** en registratiekenmerken in het beleid.
- Kiest u **Weergavenaam**- en **Object-id**-toepassingsclaims voor elk beleid. U kunt ook andere claims kiezen.
- Kopieert u de **naam** van elk beleid nadat u dit hebt gemaakt. Deze moet het voorvoegsel `b2c_1_` bevatten.  U hebt de beleidsnaam later nodig.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nadat u beleidsregels hebt gemaakt, kunt u de app maken.


## De code downloaden

De code voor deze zelfstudie wordt onderhouden in [GitHub](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-b2c).  Om verder te gaan, kunt u [de app downloaden als een ZIP](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-b2c)/archive/master.zip) of deze klonen:

```
git clone git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-b2c.git
```

Of download de voltooide code en ga direct aan de slag: 

```
git clone --branch complete git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-b2c.git
```

## De bibliotheek van derden nxoauth2 downloaden en een werkruimte starten

Voor dit scenario gebruikt u de OAuth2Client vanuit GitHub, een OAuth2-bibliotheek voor Mac OS X en iOS (Cocoa & Cocoa touch). Deze bibliotheek is gebaseerd op concept 10 van de OAuth2-specificatie. Hiermee wordt het systeemeigen toepassingsprofiel geïmplementeerd met ondersteuning van het verificatie-eindpunt van de eindgebruiker. Dit is alles wat u nodig hebt voor integratie met het Microsoft Identity-platform.

### De bibliotheek aan uw project toevoegen met CocoaPods

CocoaPods is een hulpmiddel voor afhankelijkheidsbeheer voor Xcode-projecten. Hiermee worden bovenstaande installatiestappen automatisch beheerd.

```
$ vi Podfile
```
Voeg het volgende aan de podfile toe:

```
 platform :ios, '8.0'
 
 target 'SampleforB2C' do
 
 pod 'NXOAuth2Client'
 
 end
```

Laad de podfile nu met CocoaPods. Hiermee maakt u een nieuwe Xcode-werkruimte die u gaat laden.

```
$ pod install
...
$ open SampleforB2C.xcworkspace

```

## De structuur van het project

Voor dit project is in het raamwerk de volgende structuur gemaakt:

* Een **modelweergave** met een taakvenster
* Een **weergave Taak toevoegen** voor de gegevens over de geselecteerde taak
* Een **aanmeldingsweergave** waarmee een gebruiker zich kan aanmelden bij de app.

U zult naar diverse bestanden in het project gaan om verificatie toe te voegen. Andere onderdelen van de code, zoals de Visual-code, hebben geen betrekking op Identity en zijn al voor u gemaakt.

## Het `settings.plist`-bestand voor de toepassing maken

Het is eenvoudiger om de toepassing te configureren als er een centrale locatie voor de configuratiewaarden is. Het is dan ook duidelijker wat elke instelling in uw toepassing doet. U gebruikt de *eigenschappenlijst* als een manier om deze waarden aan de toepassing door te geven.

* Het `settings.plist`-bestand onder `Supporting Files` in uw toepassingswerkruimte maken of openen

* Voer de volgende waarden in (deze worden zo dadelijk besproken)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>accountIdentifier</key>
    <string>B2C_Acccount</string>
    <key>clientID</key>
    <string><client ID></string>
    <key>clientSecret</key>
    <string></string>
    <key>authURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/oauth2/v2.0/authorize?p=<policy name></string>
    <key>loginURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/login</string>
    <key>bhh</key>
    <string>urn:ietf:wg:oauth:2.0:oob</string>
    <key>tokenURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/oauth2/v2.0/token?p=<policy name></string>
    <key>keychain</key>
    <string>com.microsoft.azureactivedirectory.samples.graph.QuickStart</string>
    <key>contentType</key>
    <string>application/x-www-form-urlencoded</string>
    <key>taskAPI</key>
    <string>https://aadb2cplayground.azurewebsites.net</string>
</dict>
</plist>
```

Hier volgt een gedetailleerde beschrijving.


U ziet dat u voor `authURL`, `loginURL`, `bhh` en `tokenURL` de naam van uw tenant moet invullen. Dit is de tenantnaam van uw B2C-tenant die aan u is toegewezen. Bijvoorbeeld `kidventusb2c.onmicrosoft.com`.Als u onze Microsoft Azure Identity-bibliotheken (open source) gebruikt, worden deze gegevens voor u opgehaald via ons eindpunt voor metagegevens. Deze waarden zijn al voor u uitgepakt.

Voor meer informatie over B2C-tenantnamen raadpleegt u [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)

De waarde `keychain` is de container die de bibliotheek NXOAuth2Client gebruikt voor het maken van een sleutelhanger waarin uw tokens worden opgeslagen. Als u eenmalige aanmelding wilt instellen voor verschillende toepassingen, kunt u dezelfde sleutelhanger opgeven in elk van uw toepassingen en het gebruik van deze sleutelhanger vereisen in uw XCode-rechten. Dit wordt beschreven in de Apple-documentatie.

In de `<policy name>` aan het einde van elke URL plaatst u het beleid dat u hierboven hebt gemaakt. Afhankelijk van de stroom worden deze beleidsregels door de app aangeroepen.

De `taskAPI` is het REST-eindpunt dat u met het B2C-token aanroept om taken toe te voegen of bestaande taken op te vragen. Dit is specifiek voor dit voorbeeld ingesteld. U hoeft het voor dit voorbeeld niet te wijzigen.

De overige waarden zijn vereist om de bibliotheek te gebruiken; hiermee worden simpelweg plaatsen gemaakt waarmee u waarden naar de context kunt overbrengen.

Nu het `settings.plist`-bestand is gemaakt, hebt u code nodig om het te lezen.

## Een AppData-klasse instellen om de instellingen te lezen

U gaat een eenvoudig bestand maken dat het hierboven gemaakte `settngs.plist`-bestand parseert en de instellingen in de toekomst beschikbaar maakt voor elke klasse. Omdat u niet telkens wanneer een klasse deze gegevens nodig heeft, een nieuwe kopie van de gegevens wilt maken, gaat u een Singleton-patroon gebruiken en telkens hetzelfde exemplaar retourneren wanneer om de instellingen wordt gevraagd

* Maak een `AppData.h`-bestand:

```objc
#import <Foundation/Foundation.h>

@interface AppData : NSObject

@property(strong) NSString *accountIdentifier;
@property(strong) NSString *taskApiString;
@property(strong) NSString *authURL;
@property(strong) NSString *clientID;
@property(strong) NSString *loginURL;
@property(strong) NSString *bhh;
@property(strong) NSString *keychain;
@property(strong) NSString *tokenURL;
@property(strong) NSString *clientSecret;
@property(strong) NSString *contentType;

+ (id)getInstance;

@end
```

* Maak een `AppData.m`-bestand:

```objc
#import "AppData.h"

@implementation AppData

+ (id)getInstance {
  static AppData *instance = nil;
  static dispatch_once_t onceToken;

  dispatch_once(&onceToken, ^{
    instance = [[self alloc] init];

    NSDictionary *dictionary = [NSDictionary
        dictionaryWithContentsOfFile:[[NSBundle mainBundle]
                                         pathForResource:@"settings"
                                                  ofType:@"plist"]];
    instance.accountIdentifier = [dictionary objectForKey:@"accountIdentifier"];
    instance.clientID = [dictionary objectForKey:@"clientID"];
    instance.clientSecret = [dictionary objectForKey:@"clientSecret"];
    instance.authURL = [dictionary objectForKey:@"authURL"];
    instance.loginURL = [dictionary objectForKey:@"loginURL"];
    instance.bhh = [dictionary objectForKey:@"bhh"];
    instance.tokenURL = [dictionary objectForKey:@"tokenURL"];
    instance.keychain = [dictionary objectForKey:@"keychain"];
    instance.contentType = [dictionary objectForKey:@"contentType"];
    instance.taskApiString = [dictionary objectForKey:@"taskAPI"];

  });

  return instance;
}
@end
```

Nu hebt u eenvoudig toegang tot de gegevens door `  AppData *data = [AppData getInstance];` in een van de klassen aan te roepen, zoals u hieronder ziet.



## De NXOAuth2Client-bibliotheek in uw AppDelegate instellen

U moet enkele waarden instellen voor de NXOAuthClient-bibliotheek. Als dat is gebeurd, kunt u het verkregen token gebruiken om de REST API aan te roepen. Omdat u weet dat `AppDelegate` wordt aangeroepen wanneer u de toepassing laadt, is het handig om de configuratiewaarden ook in dat bestand neer te zetten.
* Bestand `AppDelegate.m` openen

* Importeer enkele headerbestanden die u later gaat gebruiken.

```objc
#import "NXOAuth2.h" // the Identity library we are using
#import "AppData.h" // the class we just created we will use to load the settings of our application
```

* De methode `setupOAuth2AccountStore` toevoegen in de AppDelegate

U moet een AccountStore maken en hieraan de gegevens doorgeven die u zojuist hebt opgehaald uit het `settings.plist`-bestand.

Er zijn een aantal dingen waarmee u op dit moment rekening moet houden met betrekking tot de B2C-service om deze code begrijpelijker te maken:


1. Azure AD B2C gebruikt het *beleid* dat door de queryparameters is verstrekt, om uw aanvraag af te handelen. Azure Active Directory kan zo als een onafhankelijke service fungeren, alleen voor uw toepassing. Om deze extra queryparameters te kunnen verstrekken, moet u aan de methode `kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters:` de parameters uit het aangepast beleid doorgeven. 

2. Azure AD B2C gebruikt bereiken op nagenoeg dezelfde manier als andere OAuth2-servers. Maar omdat het gebruik van B2C evenzeer gaat over het verifiëren van een gebruiker als over de toegang tot resources, zijn bepaalde bereiken absoluut vereist voor een correcte werking van de stroom. Dit is het `openid`-bereik. De Microsoft Identity-SDK's verstrekken het `openid`-bereik automatisch. U ziet dat dus niet in de SDK-configuratie. Omdat u een bibliotheek van derden gebruikt, moet u dit bereik echter opgeven.

```objc
- (void)setupOAuth2AccountStore {
  AppData *data = [AppData getInstance]; // The singleton we use to get the settings

  NSDictionary *customHeaders =
      [NSDictionary dictionaryWithObject:@"application/x-www-form-urlencoded"
                                  forKey:@"Content-Type"];

  // Azure B2C needs
  // kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters for
  // sending policy to the server,
  // therefore we use -setConfiguration:forAccountType:
  NSDictionary *B2cConfigDict = @{
    kNXOAuth2AccountStoreConfigurationClientID : data.clientID,
    kNXOAuth2AccountStoreConfigurationSecret : data.clientSecret,
    kNXOAuth2AccountStoreConfigurationScope :
        [NSSet setWithObjects:@"openid", data.clientID, nil],
    kNXOAuth2AccountStoreConfigurationAuthorizeURL :
        [NSURL URLWithString:data.authURL],
    kNXOAuth2AccountStoreConfigurationTokenURL :
        [NSURL URLWithString:data.tokenURL],
    kNXOAuth2AccountStoreConfigurationRedirectURL :
        [NSURL URLWithString:data.bhh],
    kNXOAuth2AccountStoreConfigurationCustomHeaderFields : customHeaders,
    //      kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters:customAuthenticationParameters
  };

  [[NXOAuth2AccountStore sharedStore] setConfiguration:B2cConfigDict
                                        forAccountType:data.accountIdentifier];
}
```
Vervolgens moet u ervoor zorgen dat u deze in de AppDelegate aanroept onder de methode `didFinishLaunchingWithOptions:`. 

```
[self setupOAuth2AccountStore];
```


## Een `LoginViewController`-klasse maken die u gaat gebruiken voor de afhandeling van verificatieaanvragen

Hier wordt een webweergave gebruikt voor aanmelding bij het account. Op die manier kan de gebruiker om extra factoren worden gevraagd, zoals een sms-bericht (indien geconfigureerd) en kunnen er foutberichten aan de gebruiker worden getoond. Hier wordt de webweergave ingesteld. De code voor de afhandeling van de callbacks die vanuit de Microsoft Identity-service plaatsvinden in de webweergave, wordt later geschreven.

* Een `LoginViewController.h`-klasse maken

```objc
@interface LoginViewController : UIViewController <UIWebViewDelegate>
@property(weak, nonatomic) IBOutlet UIWebView *loginView; // Our webview that we will use to do authentication

- (void)handleOAuth2AccessResult:(NSURL *)accessResult; // Allows us to get a token after we've received an Access code.
- (void)setupOAuth2AccountStore; // We will need to add to our OAuth2AccountStore we setup in our AppDelegate
- (void)requestOAuth2Access; // This is where we invoke our webview.
```

U gaat alle hieronder genoemde methoden maken.

> [AZURE.NOTE] 
    Zorg ervoor dat u de `loginView` verbindt met de werkelijke webweergave in uw storyboard. Anders hebt u geen webweergave die kan worden weergegeven wanneer er een verificatie moet worden uitgevoerd.

* Een `LoginViewController.m`-klasse maken

* Enkele variabelen toevoegen waarin de status wordt opgeslagen tijden de verificatie

```objc
NSURL *myRequestedUrl; \\ The URL request to Azure Active Directory 
NSURL *myLoadedUrl; \\ The URL loaded for Azure Active Directory
bool loginFlow = FALSE; 
bool isRequestBusy; \\ A way to give status to the thread that the request is still happening
NSURL *authcode; \\ A placeholder for our auth code.
```

* De WebView-methoden voor het afhandelen van verificatie overschrijven

Zoals hierboven aangegeven, moet u de webweergave instrueren wat er moet gebeuren wanneer een gebruiker zich aanmeldt. U kunt onderstaande code gewoon knippen en plakken.

```objc
- (void)resolveUsingUIWebView:(NSURL *)URL {
  // We get the auth token from a redirect so we need to handle that in the
  // webview.

  if (![NSThread isMainThread]) {
    [self performSelectorOnMainThread:@selector(resolveUsingUIWebView:)
                           withObject:URL
                        waitUntilDone:YES];
    return;
  }

  NSURLRequest *hostnameURLRequest =
      [NSURLRequest requestWithURL:URL
                       cachePolicy:NSURLRequestUseProtocolCachePolicy
                   timeoutInterval:10.0f];
  isRequestBusy = YES;
  [self.loginView loadRequest:hostnameURLRequest];

  NSLog(@"resolveUsingUIWebView ready (status: UNKNOWN, URL: %@)",
        self.loginView.request.URL);
}

- (BOOL)webView:(UIWebView *)webView
    shouldStartLoadWithRequest:(NSURLRequest *)request
                navigationType:(UIWebViewNavigationType)navigationType {
  AppData *data = [AppData getInstance];

  NSLog(@"webView:shouldStartLoadWithRequest: %@ (%li)", request.URL,
        (long)navigationType);

  // The webview is where all the communication happens. Slightly complicated.

  myLoadedUrl = [webView.request mainDocumentURL];
  NSLog(@"***Loaded url: %@", myLoadedUrl);

  // if the UIWebView is showing our authorization URL or consent URL, show the
  // UIWebView control
  if ([request.URL.absoluteString rangeOfString:data.authURL
                                        options:NSCaseInsensitiveSearch]
          .location != NSNotFound) {
    self.loginView.hidden = NO;
  } else if ([request.URL.absoluteString rangeOfString:data.loginURL
                                               options:NSCaseInsensitiveSearch]
                 .location != NSNotFound) {
    // otherwise hide the UIWebView, we've left the authorization flow
    self.loginView.hidden = NO;
  } else if ([request.URL.absoluteString rangeOfString:data.bhh
                                               options:NSCaseInsensitiveSearch]
                 .location != NSNotFound) {
    // otherwise hide the UIWebView, we've left the authorization flow
    self.loginView.hidden = YES;
    [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
  } else {
    self.loginView.hidden = NO;
    // read the Location from the UIWebView, this is how Microsoft APIs is
    // returning the
    // authentication code and relation information. This is controlled by the
    // redirect URL we chose to use from Microsoft APIs
    // continue the OAuth2 flow
    // [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
  }

  return YES;
}

```

* Code schrijven om het resultaat van de OAuth2-aanvraag af te handelen

U hebt code nodig om de redirectURL van de webweergave af te handelen. Als dit niet is geslaagd, probeert u het opnieuw. Ondertussen levert de bibliotheek de fout die u in de console kunt zien of asynchroon kunt afhandelen. 

```objc
- (void)handleOAuth2AccessResult:(NSURL *)accessResult {
  // parse the response for success or failure
  if (accessResult)
  // if success, complete the OAuth2 flow by handling the redirect URL and
  // obtaining a token
  {
    [[NXOAuth2AccountStore sharedStore] handleRedirectURL:accessResult];
  } else {
    // start over
    [self requestOAuth2Access];
  }
}
```

* Stel de meldingsfactory’s in.

U maakt dezelfde methode als in de `AppDelegate` hierboven, maar dit keer voegt u enkele `NSNotification`s toe om ons te laten weten wat er in de service gebeurt. Er wordt een waarnemer ingesteld die ons vertelt wanneer er iets met het token verandert. Zodra u het token hebt, keert de gebruiker terug naar de `masterView`.



```objc
- (void)setupOAuth2AccountStore {
  [[NSNotificationCenter defaultCenter]
      addObserverForName:NXOAuth2AccountStoreAccountsDidChangeNotification
                  object:[NXOAuth2AccountStore sharedStore]
                   queue:nil
              usingBlock:^(NSNotification *aNotification) {
                if (aNotification.userInfo) {
                  // account added, we have access
                  // we can now request protected data
                  NSLog(@"Success!! We have an access token.");
                  dispatch_async(dispatch_get_main_queue(), ^{

                    MasterViewController *masterViewController =
                        [self.storyboard
                            instantiateViewControllerWithIdentifier:@"master"];
                    [self.navigationController
                        pushViewController:masterViewController
                                  animated:YES];
                  });
                } else {
                  // account removed, we lost access
                }
              }];

  [[NSNotificationCenter defaultCenter]
      addObserverForName:NXOAuth2AccountStoreDidFailToRequestAccessNotification
                  object:[NXOAuth2AccountStore sharedStore]
                   queue:nil
              usingBlock:^(NSNotification *aNotification) {
                NSError *error = [aNotification.userInfo
                    objectForKey:NXOAuth2AccountStoreErrorKey];
                NSLog(@"Error!! %@", error.localizedDescription);
              }];
}

```
* Code toevoegen waarmee de gebruiker wordt afgehandeld wanneer er een aanvraag wordt geïnitieerd voor systeemeigen aanmelding

U gaat een methode maken die wordt aangeroepen wanneer er een verificatieaanvraag binnenkomt. Dit wordt de methode waarmee daadwerkelijk een webweergave wordt gemaakt

```objc
- (void)requestOAuth2Access {
  AppData *data = [AppData getInstance];

  // in order to login to Mircosoft APIs using OAuth2 we must show an embedded
  // browser (UIWebView)
  [[NXOAuth2AccountStore sharedStore]
           requestAccessToAccountWithType:data.accountIdentifier
      withPreparedAuthorizationURLHandler:^(NSURL *preparedURL) {
        // navigate to the URL returned by NXOAuth2Client

        NSURLRequest *r = [NSURLRequest requestWithURL:preparedURL];
        [self.loginView loadRequest:r];
      }];
}
```

* Tot slot gaan we alle methoden die u hierboven hebt geschreven, aanroepen telkens wanneer de `LoginViewController` wordt geladen. Daarvoor voegt u deze methoden toe aan de `viewDidLoad`-methode van Apple

```objc
  [super viewDidLoad];
  // Do any additional setup after loading the view.

  // OAuth2 Code

  self.loginView.delegate = self;
  [self requestOAuth2Access];
  [self setupOAuth2AccountStore];
  NSURLCache *URLCache =
      [[NSURLCache alloc] initWithMemoryCapacity:4 * 1024 * 1024
                                    diskCapacity:20 * 1024 * 1024
                                        diskPath:nil];
  [NSURLCache setSharedURLCache:URLCache];
```

U hebt nu de belangrijkste manier gemaakt waarop u met de toepassing communiceert voor aanmelding. Nadat u zich hebt aangemeld, moet u de tokens gebruiken die u hebt ontvangen. Daarvoor maakt u ondersteunende code die REST API's aanroept om deze bibliotheek te gebruiken.


## Een `GraphAPICaller`-klasse maken om de aanvragen voor een REST API af te handelen

Telkens wanneer uw app wordt geladen, wordt er ook een configuratie geladen. Zodra u een token hebt, moet u daar iets mee doen. 

* Een `GraphAPICaller.h`-bestand maken

```objc
@interface GraphAPICaller : NSObject <NSURLConnectionDataDelegate>

+ (void)addTask:(Task *)task
completionBlock:(void (^)(bool, NSError *error))completionBlock;

+ (void)getTaskList:(void (^)(NSMutableArray *, NSError *error))completionBlock;

@end
```

U ziet in deze code dat er twee methoden worden gemaakt: één om de taken op te halen uit een API en een tweede om taken toe te voegen aan de API.

Nu de interface is ingesteld, gaat u de daadwerkelijke implementatie toevoegen:

* Maak een `GraphAPICaller.m file`

```objc
@implementation GraphAPICaller

// 
// Gets the tasks from our REST endpoint we specified in settings
//

+ (void)getTaskList:(void (^)(NSMutableArray *, NSError *))completionBlock

{
  AppData *data = [AppData getInstance];

  NSString *taskURL =
      [NSString stringWithFormat:@"%@%@", data.taskApiString, @"/api/tasks"];

  NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
  NSMutableArray *Tasks = [[NSMutableArray alloc] init];

  NSArray *accounts = [store accountsWithAccountType:data.accountIdentifier];
  [NXOAuth2Request performMethod:@"GET"
      onResource:[NSURL URLWithString:taskURL]
      usingParameters:nil
      withAccount:accounts[0]
      sendProgressHandler:^(unsigned long long bytesSend,
                            unsigned long long bytesTotal) {
        // e.g., update a progress indicator
      }
      responseHandler:^(NSURLResponse *response, NSData *responseData,
                        NSError *error) {
        // Process the response
        if (!error) {
          NSDictionary *dataReturned =
              [NSJSONSerialization JSONObjectWithData:responseData
                                              options:0
                                                error:nil];
          NSLog(@"Graph Response was: %@", dataReturned);

          if ([dataReturned count] != 0) {

            for (NSMutableDictionary *theTask in dataReturned) {

              Task *t = [[Task alloc] init];
              t.name = [theTask valueForKey:@"Text"];

              [Tasks addObject:t];
            }
          }

          completionBlock(Tasks, nil);
        } else {
          completionBlock(nil, error);
        }

      }];
}

// 
// Adds a task from our REST endpoint we specified in settings
//

+ (void)addTask:(Task *)task
completionBlock:(void (^)(bool, NSError *error))completionBlock {

  AppData *data = [AppData getInstance];

  NSString *taskURL =
      [NSString stringWithFormat:@"%@%@", data.taskApiString, @"/api/tasks"];

  NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
  NSDictionary *params = [self convertParamsToDictionary:task.name];

  NSArray *accounts = [store accountsWithAccountType:data.accountIdentifier];
  [NXOAuth2Request performMethod:@"POST"
      onResource:[NSURL URLWithString:taskURL]
      usingParameters:params
      withAccount:accounts[0]
      sendProgressHandler:^(unsigned long long bytesSend,
                            unsigned long long bytesTotal) {
        // e.g., update a progress indicator
      }
      responseHandler:^(NSURLResponse *response, NSData *responseData,
                        NSError *error) {
        // Process the response
        if (responseData) {
          NSDictionary *dataReturned =
              [NSJSONSerialization JSONObjectWithData:responseData
                                              options:0
                                                error:nil];
          NSLog(@"Graph Response was: %@", dataReturned);

          completionBlock(TRUE, nil);
        } else {
          completionBlock(FALSE, error);
        }

      }];
}

+ (NSDictionary *)convertParamsToDictionary:(NSString *)task {
  NSMutableDictionary *dictionary = [[NSMutableDictionary alloc] init];

  [dictionary setValue:task forKey:@"Text"];

  return dictionary;
}

@end
```

## De voorbeeld-app uitvoeren

Ontwikkel en voer ten slotte de app uit in Xcode. Registreer u of meld u aan bij de app en maak taken voor een aangemelde gebruiker. Meld u af en meld u opnieuw aan als een andere gebruiker, en maak taken voor die gebruiker.

U ziet dat de taken per gebruiker op de API worden opgeslagen, omdat de API de identiteit van de gebruiker afleidt uit het toegangstoken dat het ontvangt.


## Volgende stappen

Nu kunt u verder met geavanceerdere B2C-onderwerpen. U kunt het volgende proberen:

[Een Node.js-web-API aanroepen vanuit een Node.js-web-app]()

[De UX voor een B2C-app aanpassen]()



<!--HONumber=ago16_HO4-->


