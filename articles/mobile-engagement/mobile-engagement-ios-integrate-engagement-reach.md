---
title: Azure Mobile Engagement iOS SDK-integratie bereiken | Microsoft Docs
description: Meest recente updates en procedures voor iOS SDK voor Azure Mobile Engagement
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 1f5f5857-867c-40c5-9d76-675a343a0296
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 12/13/2016
ms.author: piyushjo
ms.openlocfilehash: ba74e0c442ac10f096d465f989e03d2ceae8cd88
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-integrate-engagement-reach-on-ios"></a>Het integreren van Engagement bereiken op iOS
U moet volgen de procedure integratie in de [hoe integreren Engagement voor iOS-document](mobile-engagement-ios-integrate-engagement.md) voordat u deze handleiding.

Deze documentatie vereist XCode 8. Als u echt afhankelijk van XCode 7 zijn en u kunt de [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh). Er is een bekend probleem in de vorige versie bij het uitvoeren op iOS-10-apparaten: Er zijn geen kennisgevingen systeem waarop actie is ondernomen. Om op te lossen dit er voor het implementeren van de afgeschafte API `application:didReceiveRemoteNotification:` in uw app delegeren als volgt:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [!IMPORTANT]
> **Deze tijdelijke oplossing wordt niet aanbevolen** zoals dit gedrag in een toekomstige (zelfs secundaire) iOS-versie-upgrade wijzigen kunt omdat deze API voor iOS is afgeschaft. U moet zo snel mogelijk overschakelen naar XCode 8.
>
>

### <a name="enable-your-app-to-receive-silent-push-notifications"></a>Ontvangen van achtergrond-pushmeldingen inschakelen voor de app
[!INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

## <a name="integration-steps"></a>Stappen voor integratie
### <a name="embed-the-engagement-reach-sdk-into-your-ios-project"></a>De SDK voor het bereiken van Engagement insluiten in uw iOS-project
* De Reach-sdk toevoegen aan uw Xcode-project. Ga in Xcode naar **Project \> toevoegen aan project** en kies de `EngagementReach` map.

### <a name="modify-your-application-delegate"></a>Uw toepassingsgemachtigde wijzigen
* Aan de bovenkant van het bestand uitvoering door de module Engagement bereiken te importeren:

      [...]
      #import "AEReachModule.h"
* In de methode `applicationDidFinishLaunching:` of `application:didFinishLaunchingWithOptions:`, maakt u een reach-module en geef dit door aan uw bestaande initialisatieregel:

      - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
        AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
        [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
        [...]

        return YES;
      }
* Wijzig **'icon.png'** tekenreeks met de naam van de installatiekopie die u wilt gebruiken als uw meldingspictogram.
* Als u wilt gebruiken, de optie *Update badge waarde* in Reach-campagnes of als u wilt gebruiken van native pushberichten \<SaaS/Reach API/campagne indeling Native Push\> campagnes, moet u de module beheren het badge pictogram zelf (wordt automatisch door de toepassing badge gewist en ook opnieuw instellen van de waarde die is opgeslagen door Engagement telkens wanneer de toepassing gestart of foregrounded is) Reach laten. Dit wordt gedaan door de volgende regel toe te voegen na de initialisatie van de Reach-module:

      [reach setAutoBadgeEnabled:YES];
* Als u wilt voor het afhandelen van Reach-gegevens-push, moet u gebruiken om uw toepassingsgemachtigde voldoen aan de `AEReachDataPushDelegate` protocol. Voeg de volgende regel toe na de initialisatie van de Reach-module:

      [reach setDataPushDelegate:self];
* Vervolgens kunt u de methoden implementeren `onDataPushStringReceived:` en `onDataPushBase64ReceivedWithDecodedBody:andEncodedBody:` in uw toepassingsgemachtigde:

      -(BOOL)didReceiveStringDataPushWithCategory:(NSString*)category body:(NSString*)body
      {
         NSLog(@"String data push message with category <%@> received: %@", category, body);
         return YES;
      }

      -(BOOL)didReceiveBase64DataPushWithCategory:(NSString*)category decodedBody:(NSData *)decodedBody encodedBody:(NSString *)encodedBody
      {
         NSLog(@"Base64 data push message with category <%@> received: %@", category, encodedBody);
         // Do something useful with decodedBody like updating an image view
         return YES;
      }

### <a name="category"></a>Category
De categorieparameter is optioneel bij het maken van een campagne Push-gegevens en kunt dat u gegevens wilt filteren pushes. Dit is handig als u wilt pushen van verschillende typen van `Base64` gegevens en wilt u het type waardoor ze identificeren voordat ze worden geparseerd.

**Uw toepassing is nu gereed voor het ontvangen en reach inhoud weer te geven.**

## <a name="how-to-receive-announcements-and-polls-at-any-time"></a>Het ontvangen van aankondigingen en polls op elk gewenst moment
Engagement kunt Reach meldingen verzenden aan uw eindgebruikers op elk gewenst moment met behulp van de Apple Push Notification Service.

Als u deze functionaliteit wilt, hebt u voor het voorbereiden van uw aanvraag voor Apple pushmeldingen en uw toepassingsgemachtigde wijzigen.

### <a name="prepare-your-application-for-apple-push-notifications"></a>Uw aanvraag voor Apple pushmeldingen voorbereiden
Volg de guide: [het voorbereiden van uw aanvraag voor Apple Pushmeldingen](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)

### <a name="add-the-necessary-client-code"></a>De benodigde clientcode toevoegen
*Uw toepassing hebt op dit moment een geregistreerde Apple push-certificaat in de Engagement-frontend.*

Als deze niet al gebeurt, moet u de toepassing voor het ontvangen van pushmeldingen registreren.

* Importeer de `User Notification` framework:

        #import <UserNotifications/UserNotifications.h>
* Voeg de volgende regel wanneer uw toepassing wordt gestart (meestal in `application:didFinishLaunchingWithOptions:`):

        if (NSFoundationVersionNumber >= NSFoundationVersionNumber_iOS_8_0)
        {
            if (NSFoundationVersionNumber > NSFoundationVersionNumber_iOS_9_x_Max)
            {
                [UNUserNotificationCenter.currentNotificationCenter requestAuthorizationWithOptions:(UNAuthorizationOptionBadge | UNAuthorizationOptionSound | UNAuthorizationOptionAlert) completionHandler:^(BOOL granted, NSError * _Nullable error) {}];
            }else
            {
                [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert)   categories:nil]];
            }
            [application registerForRemoteNotifications];
        }
        else
        {
            [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
        }

Vervolgens moet u ter engagement het apparaattoken geretourneerd door de servers van Apple. Dit doet u in de methode met de naam `application:didRegisterForRemoteNotificationsWithDeviceToken:` in uw toepassingsgemachtigde:

    - (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken
    {
        [[EngagementAgent shared] registerDeviceToken:deviceToken];
    }

U hebt ten slotte de Engagement SDK in kennis wanneer uw toepassing een externe melding ontvangt. Roep de methode hiervoor `applicationDidReceiveRemoteNotification:fetchCompletionHandler:` in uw toepassingsgemachtigde:

    - (void)application:(UIApplication*)application didReceiveRemoteNotification:(NSDictionary*)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
    }

> [!IMPORTANT]
> Standaard is de completionHandler bepaalt Engagement bereiken. Als u handmatig reageren wilt op de `handler` blokkeren in uw code, kunt u nil doorgeven voor de `handler` argument en het voltooien van het besturingselement zelf blokkeren. Zie de `UIBackgroundFetchResult` type voor een lijst van mogelijke waarden.
>
>

### <a name="full-example"></a>Compleet voorbeeld
Hier volgt een voorbeeld van een volledige integratie:

    #pragma mark -
    #pragma mark Application lifecycle

    - (BOOL)application:(UIApplication*)application didFinishLaunchingWithOptions:(NSDictionary*)launchOptions
    {
      /* Reach module */
      AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
      [reach setAutoBadgeEnabled:YES];

      /* Engagement initialization */
      [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
      [[EngagementAgent shared] setPushDelegate:self];

      /* Views */
      [window addSubview:[tabBarController view]];
      [window makeKeyAndVisible];

      [application registerForRemoteNotificationTypes:UIRemoteNotificationTypeAlert|UIRemoteNotificationTypeBadge|UIRemoteNotificationTypeSound];
      return YES;
    }

    - (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken
    {
      [[EngagementAgent shared] registerDeviceToken:deviceToken];
    }

    - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
    }

### <a name="resolve-unusernotificationcenter-delegate-conflicts"></a>UNUserNotificationCenter gemachtigde conflicten oplossen

*Als uw toepassing noch een van de bibliotheken van de derde partij implementeert een `UNUserNotificationCenterDelegate` en vervolgens kunt u dit gedeelte overslaan.*

Een `UNUserNotificationCenter` gemachtigde wordt gebruikt door de SDK voor het bewaken van de levenscyclus van de Engagement-meldingen op apparaten waarop iOS 10 of hoger. De SDK heeft een eigen implementatie van de `UNUserNotificationCenterDelegate` protocol, maar er mag slechts één `UNUserNotificationCenter` delegeren per toepassing. Geen andere gedelegeerde toegevoegd aan de `UNUserNotificationCenter` object met de Engagement een conflict veroorzaken. Als de SDK de of alle andere leveranciers gemachtigde detecteert wordt deze niet zijn eigen implementatie gebruiken om een waarschuwingsbericht geeft u de conflicten op te lossen. U moet de Engagement-logica toevoegen aan uw eigen gemachtigde om de conflicten oplossen.

Er zijn twee manieren om dit te bereiken.

Voorstel 1, door de gemachtigde doorsturen aanroepen naar de SDK:

    #import <UIKit/UIKit.h>
    #import "EngagementAgent.h"
    #import <UserNotifications/UserNotifications.h>


    @interface MyAppDelegate : NSObject <UIApplicationDelegate, UNUserNotificationCenterDelegate>
    @end

    @implementation MyAppDelegate

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterWillPresentNotification:notification withCompletionHandler:completionHandler]
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterDidReceiveNotificationResponse:response withCompletionHandler:completionHandler]
    }
    @end

Of voorstel 2, door het overnemen van de `AEUserNotificationHandler` klasse

    #import "AEUserNotificationHandler.h"
    #import "EngagementAgent.h"

    @interface CustomUserNotificationHandler :AEUserNotificationHandler
    @end

    @implementation CustomUserNotificationHandler

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center willPresentNotification:notification withCompletionHandler:completionHandler];
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse: UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center didReceiveNotificationResponse:response withCompletionHandler:completionHandler];
    }

    @end

> [!NOTE]
> U kunt bepalen of een melding afkomstig van Engagement of niet door het doorgeven van is de `userInfo` woordenlijst met de Agent `isEngagementPushPayload:` klasse-methode.

Zorg ervoor dat de `UNUserNotificationCenter` gemachtigde van het object is ingesteld op uw gemachtigde binnen ofwel de `application:willFinishLaunchingWithOptions:` of de `application:didFinishLaunchingWithOptions:` methode van de toepassingsgemachtigde van uw.
Bijvoorbeeld, als u de bovenstaande voorstel 1 geïmplementeerd:

      - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
        // Any other code

        [UNUserNotificationCenter currentNotificationCenter].delegate = self;
        return YES;
      }

## <a name="how-to-customize-campaigns"></a>Het aanpassen van de campagnes
### <a name="notifications"></a>Meldingen
Er zijn twee typen meldingen: systeem en in-app-meldingen.

Systeemmeldingen worden afgehandeld door iOS en kunnen niet worden aangepast.

In-app-meldingen zijn gemaakt van een weergave die dynamisch wordt toegevoegd aan het huidige toepassingsvenster. Dit is een melding overlay genoemd. Melding overlays zijn ideaal voor een snelle integratie omdat ze hoeft u niet om een weergave in uw toepassing te wijzigen.

#### <a name="layout"></a>Lay-out
Voor het wijzigen van het uiterlijk van uw in-app-meldingen u gewoon het bestand kan wijzigen `AENotificationView.xib` aan uw behoeften, zolang u de labelwaarden en typen van de bestaande subweergaven behouden.

Standaard worden in-app-meldingen weergegeven aan de onderkant van het scherm. Als u liever bovenaan in het scherm worden weergegeven, bewerkt u de opgegeven `AENotificationView.xib` en wijzig de `AutoSizing` eigenschap van de belangrijkste weergave zodat deze aan de bovenkant van de superview kan worden bewaard.

#### <a name="categories"></a>Categorieën
Als u de opgegeven indeling wijzigt, wijzigt u het uiterlijk van al uw meldingen. Categorieën kunnen u verschillende gerichte lijkt (mogelijk gedrag) definiëren voor meldingen. Een categorie kan worden opgegeven wanneer u een Reach-campagne maken. Houd er rekening mee dat categorieën ook kunnen u aanpassen aankondigingen en polls, die verderop in dit document wordt beschreven.

Voor het registreren van een categorie-handler voor uw meldingen die u wilt toevoegen, een aanroep van wanneer de reach-module is geïnitialiseerd.

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerNotifier:myNotifier forCategory:@"my_category"];
    ...

`myNotifier`moet een exemplaar van een object dat aan het protocol voldoet `AENotifier`.

U kunt de protocolmethoden implementeren door uzelf of u kunt de bestaande klasse reimplement `AEDefaultNotifier` die al meeste werk wordt uitgevoerd.

Als u definiëren van de weergave van meldingen voor een specifieke categorie wilt, voert u bijvoorbeeld het volgende voorbeeld:

    #import "AEDefaultNotifier.h"
    #import "AENotificationView.h"
    @interface MyNotifier : AEDefaultNotifier
    @end

    @implementation MyNotifier

    -(NSString*)nibNameForCategory:(NSString*)category
    {
      return "MyNotificationView";
    }

    @end

Dit eenvoudige voorbeeld van de categorie wordt ervan uitgegaan dat u hebt een bestand met de naam `MyNotificationView.xib` in de bundel hoofdtoepassing. Als de methode is niet gevonden met een overeenkomende `.xib`, de melding wordt niet weergegeven en Engagement een bericht in de console wordt uitgevoerd.

Het opgegeven kroontjespen-bestand moet de volgende regels voldoen:

* Het moet slechts één weergave bevatten.
* Subweergaven moeten van hetzelfde type als die binnen het opgegeven kroontjespen-bestand de naam`AENotificationView.xib`
* Subweergaven moet dezelfde tags hebben als de waarden in de opgegeven kroontjespen-bestand met de naam`AENotificationView.xib`

> [!TIP]
> Alleen de opgegeven kroontjespen bestand kopiëren, met de naam `AENotificationView.xib`, en beginnen met werken vanaf daar. Maar wees voorzichtig en de weergave in dit bestand kroontjespen is gekoppeld aan de klasse `AENotificationView`. Deze klasse opnieuw gedefinieerd voor de methode `layoutSubViews` te verplaatsen en het formaat ervan subweergaven volgens context. U kunt vervangen door een `UIView` of u aangepaste weergave-klasse.
>
>

Als u meer gedetailleerde aanpassing van uw meldingen moet (als u bijvoorbeeld wilt dat uw weergave laden rechtstreeks vanuit de code), het is raadzaam te verdiepen in de opgegeven bron code en klasse de documentatie van `Protocol ReferencesDefaultNotifier` en `AENotifier`.

Houd er rekening mee dat u de dezelfde melder voor meerdere categorieën gebruiken kunt.

U kunt ook de standaard-melder als volgt gedefinieerd:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerNotifier:myNotifier forCategory:kAEReachDefaultCategory];

##### <a name="notification-handling"></a>Afhandeling van melding
Wanneer u de standaardcategorie gebruikt, een aantal methoden levenscyclus worden aangeroepen op de `AEReachContent` object rapport statistieken en de status van de campagne werken:

* Wanneer de melding wordt weergegeven in de toepassing, de `displayNotification` methode (die statistieken) wordt aangeroepen door `AEReachModule` als `handleNotification:` retourneert `YES`.
* Als de melding wordt gesloten, de `exitNotification` methode wordt aangeroepen, statistiek wordt gerapporteerd en volgende campagnes kunnen nu worden verwerkt.
* Als u de melding klikt, `actionNotification` is aangeroepen, statistiek is gerapporteerd en de bijbehorende actie is uitgevoerd.

Als uw implementatie van `AENotifier` het standaardgedrag omzeilt u aan te roepen van deze methoden van de levenscyclus van de door u zelf hebt. De volgende voorbeelden ziet enkele gevallen waarbij het standaardgedrag wordt overgeslagen:

* U kunt niet uitbreiden `AEDefaultNotifier`, zoals u categorie verwerking helemaal geïmplementeerd.
* U overrode `prepareNotificationView:forContent:`, zorg ervoor dat toegewezen ten minste `onNotificationActioned` of `onNotificationExited` op een van uw U.I bepaalt.

> [!WARNING]
> Als `handleNotification:` er wordt een uitzondering, de inhoud wordt verwijderd en `drop` is aangeroepen, dit wordt gemeld in statistieken en volgende campagnes kunnen nu worden verwerkt.
>
>

#### <a name="include-notification-as-part-of-an-existing-view"></a>Melding als onderdeel van een bestaande weergave opnemen
Overlays zijn ideaal voor een snelle integratie, maar kunnen worden soms niet handig of ongewenste neveneffecten hebben.

Als u niet tevreden met het systeem overlay in een aantal weergaven bent, kunt u deze kunt aanpassen voor deze weergaven.

U kunt besluiten te nemen van onze lay-out voor de melding in uw bestaande weergaven. Om dit te doen, moet u er twee implementatie stijlen is:

1. De weergave van meldingen met interface builder toevoegen

   * Open *Builder Interface*
   * Een 320 x 60 (of als u op iPad 768 x 60) plaatst `UIView` waar u de melding wilt weergeven
   * De waarde van het label voor deze weergave zodanig instellen: **36822491**
2. De weergave van meldingen via programmacode toevoegen. Voeg de volgende code alleen wanneer de weergave is geïnitialiseerd:

       UIView* notificationView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, 320, 60)]; //Replace x and y coordinate values to your needs.
       notificationView.tag = NOTIFICATION_AREA_VIEW_TAG;
       [self.view addSubview:notificationView];

`NOTIFICATION_AREA_VIEW_TAG`macro vindt u in `AEDefaultNotifier.h`.

> [!NOTE]
> De standaard-melder detecteert automatisch dat de indeling van de melding is opgenomen in deze weergave en wordt een overlay niet toevoegen voor.
>
>

### <a name="announcements-and-polls"></a>Aankondigingen en polls
#### <a name="layouts"></a>Indelingen
U kunt de bestanden wijzigen `AEDefaultAnnouncementView.xib` en `AEDefaultPollView.xib` , zolang u de labelwaarden en typen van de bestaande subweergaven behouden.

#### <a name="categories"></a>Categorieën
##### <a name="alternate-layouts"></a>Alternatieve indelingen
Zoals meldingen, kan de campagne categorie worden gebruikt voor alternatieve indelingen voor uw aankondigingen en polls hebben.

Als u wilt een categorie voor een aankondiging maakt, moet u uitbreiden **AEAnnouncementViewController** en registreer deze zodra de reach-module is geïnitialiseerd:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerAnnouncementController:[MyCustomAnnouncementViewController class] forCategory:@"my_category"];

> [!NOTE]
> Telkens wanneer een gebruiker wordt klikt u op een melding naar een aankondiging met de categorie ' Mijn\_categorie ', uw geregistreerde weergavebesturing (in dat geval `MyCustomAnnouncementViewController`) wordt geïnitialiseerd door het aanroepen van de methode `initWithAnnouncement:` en de weergave wordt toegevoegd aan het huidige toepassingsvenster.
>
>

Bij de implementatie van de `AEAnnouncementViewController` klasse hebt, moet u de eigenschap lezen `announcement` uw subweergaven initialiseren. Houd rekening met het volgende voorbeeld, waarbij twee labels zijn geïnitialiseerd met behulp van `title` en `body` eigenschappen van de `AEReachAnnouncement` klasse:

    -(void)loadView
    {
        [super loadView];

        UILabel* titleLabel = [[UILabel alloc] initWithFrame:CGRectMake(10, 20, 300, 60)];
        titleLabel.font = [UIFont systemFontOfSize:32.0];
        titleLabel.text = self.announcement.title;

        UILabel* bodyLabel = [[UILabel alloc] initWithFrame:CGRectMake(10, 20, 300, 60)];
        bodyLabel.font = [UIFont systemFontOfSize:24.0];
        bodyLabel.text = self.announcement.body;

        [self.view addSubview:titleLabel];
        [self.view addSubview:bodyLabel];
    }

Als u niet wilt dat uw weergaven laden door uzelf, maar u alleen wilt gebruiken als de standaard aankondiging lay-out, kunt u gewoon de aangepaste weergave-controller breidt u de opgegeven klasse `AEDefaultAnnouncementViewController`. In dat geval wordt het bestand kroontjespen dubbele `AEDefaultAnnouncementView.xib` en wijzig deze zodat deze kan worden geladen door de aangepaste weergave-controller (voor een domeincontroller met de naam `CustomAnnouncementViewController`, moet u het bestand kroontjespen aanroepen `CustomAnnouncementView.xib`).

Als u wilt vervangen de standaardcategorie van aankondigingen, uw domeincontroller aangepaste weergave voor de categorie die is gedefinieerd in gewoon registreren `kAEReachDefaultCategory`:

    [reach registerAnnouncementController:[MyCustomAnnouncementViewController class] forCategory:kAEReachDefaultCategory];

Polls kunnen op dezelfde manier worden aangepast:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerPollController:[MyCustomPollViewController class] forCategory:@"my_category"];

Deze tijd, de opgegeven `MyCustomPollViewController` moet uitbreiden `AEPollViewController`. Of u kunt kiezen uit te breiden van de standaard-controller: `AEDefaultPollViewController`.

> [!IMPORTANT]
> Vergeet niet om aan te roepen ofwel `action` (`submitAnswers:` voor aangepaste poll weergave domeincontrollers) of `exit` methode voordat de weergavebesturing wordt gesloten. Anders statistieken niet verzonden (d.w.z. geen analytics op de campagne) en meer belangrijker nog volgende campagnes wordt niet gewaarschuwd totdat het toepassingsproces opnieuw wordt gestart.
>
>

##### <a name="implementation-example"></a>Voorbeeldimplementatie
In deze implementatie is de weergave aangepaste aankondiging geladen uit een externe xib-bestand.

Zoals voor aanpassing van de geavanceerde melding, is het raadzaam om te kijken naar de broncode van de standaardimplementatie.

`CustomAnnouncementViewController.h`

    //Interface
    @interface CustomAnnouncementViewController : AEAnnouncementViewController {
      UILabel* titleLabel;
      UITextView* descTextView;
      UIWebView* htmlWebView;
      UIButton* okButton;
      UIButton* cancelButton;
    }

    @property (nonatomic, retain) IBOutlet UILabel* titleLabel;
    @property (nonatomic, retain) IBOutlet UITextView* descTextView;
    @property (nonatomic, retain) IBOutlet UIWebView* htmlWebView;
    @property (nonatomic, retain) IBOutlet UIButton* okButton;
    @property (nonatomic, retain) IBOutlet UIButton* cancelButton;

    -(IBAction)okButtonClicked:(id)sender;
    -(IBAction)cancelButtonClicked:(id)sender;

`CustomAnnouncementViewController.m`

    //Implementation
    @implementation CustomAnnouncementViewController
    @synthesize titleLabel;
    @synthesize descTextView;
    @synthesize htmlWebView;
    @synthesize okButton;
    @synthesize cancelButton;

    -(id)initWithAnnouncement:(AEReachAnnouncement*)anAnnouncement
    {
      self = [super initWithNibName:@"CustomAnnouncementViewController" bundle:nil];
      if (self != nil) {
        self.announcement = anAnnouncement;
      }
      return self;
    }

    - (void) dealloc
    {
      [titleLabel release];
      [descTextView release];
      [htmlWebView release];
      [okButton release];
      [cancelButton release];
      [super dealloc];
    }

    - (void)viewDidLoad {
      [super viewDidLoad];

      /* Init announcement title */
      titleLabel.text = self.announcement.title;

      /* Init announcement body */
      if(self.announcement.type == AEAnnouncementTypeHtml)
      {
        titleLabel.hidden = YES;
        htmlWebView.hidden = NO;
        [htmlWebView loadHTMLString:self.announcement.body baseURL:[NSURL URLWithString:@"http://localhost/"]];
      }
      else
      {
        titleLabel.hidden = NO;
        htmlWebView.hidden = YES;
        descTextView.text = self.announcement.body;
      }

      /* Set action button label */
      if([self.announcement.actionLabel length] > 0)
        [okButton setTitle:self.announcement.actionLabel forState:UIControlStateNormal];

      /* Set exit button label */
      if([self.announcement.exitLabel length] > 0)
        [cancelButton setTitle:self.announcement.exitLabel forState:UIControlStateNormal];
    }

    #pragma mark Actions

    -(IBAction)okButtonClicked:(id)sender
    {
        [self action];
    }

    -(IBAction)cancelButtonClicked:(id)sender
    {
        [self exit];
    }

    @end
