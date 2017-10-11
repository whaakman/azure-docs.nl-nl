---
title: Azure Mobile Engagement iOS SDK-integratie | Microsoft Docs
description: Meest recente updates en procedures voor iOS SDK voor Azure Mobile Engagement
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 947ea44b-00c1-450f-9a3b-74437954dc56
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 07/17/2017
ms.author: piyushjo
ms.openlocfilehash: 01fdbb43c21ac6932e8462f4a6507fc63e50542d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-integrate-engagement-on-ios"></a>Hoe integreren Engagement voor iOS
> [!div class="op_single_selector"]
> * [Windows Universal](mobile-engagement-windows-store-integrate-engagement.md)
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
> * [iOS](mobile-engagement-ios-integrate-engagement.md)
> * [Android](mobile-engagement-android-integrate-engagement.md)
>
>

Deze procedure beschrijft de eenvoudigste manier Engagement Analytics en bewaking van functies in uw iOS-toepassing te activeren.

De Engagement SDK vereist iOS7 + en Xcode 8 +: het implementatiedoel van uw toepassing moet ten minste iOS 7.

> [!NOTE]
> Als u echt afhankelijk van XCode 7 zijn en u kunt de [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh). Er is een bekend probleem in de vorige versie van de Reach-module tijdens het uitvoeren van de iOS-10-apparaten Zie [de reach-module integratie](mobile-engagement-ios-integrate-engagement-reach.md) voor meer informatie. Als u kiest voor het gebruik van de SDK-v3.2.4 en maar overslaan de `UserNotifications.framework` importeren in de volgende stap.
>
>

De volgende stappen zijn voldoende is voor het activeren van het rapport van logboeken die nodig zijn voor alle statistische gegevens over gebruikers, sessies, activiteiten, Crashes en Technicals berekenen. Het rapport van logboeken die nodig zijn voor het berekenen van andere statistieken zoals gebeurtenissen, fouten en taken worden uitgevoerd handmatig met behulp van de Engagement-API (Zie [hoe u de geavanceerde Mobile Engagement API in uw iOS-app-tagging](mobile-engagement-ios-use-engagement-api.md) aangezien deze statistieken afhankelijk van de toepassing zijn.

## <a name="embed-the-engagement-sdk-into-your-ios-project"></a>De SDK Engagement insluiten in uw iOS-project
* De iOS SDK downloaden van [hier](http://aka.ms/qk2rnj).
* De Engagement-SDK toevoegen aan uw iOS-project: in Xcode, klik met de rechtermuisknop op uw project en selecteer **'Add files to...'** en kies de `EngagementSDK` map.
* Engagement vereist aanvullende frameworks werkt: in de Projectverkenner, opent u het deelvenster van uw project en selecteer het juiste doel. Open vervolgens de **'Buildfasen'** tabblad en in de **'Link Binary With Libraries'** menu deze frameworks toevoegen:

  * `UserNotifications.framework`-de koppeling als instellen`Optional`
  * `AdSupport.framework`-de koppeling als instellen`Optional`
  * `SystemConfiguration.framework`
  * `CoreTelephony.framework`
  * `CFNetwork.framework`
  * `CoreLocation.framework`
  * `libxml2.dylib`

> [!NOTE]
> Het framework AdSupport kan worden verwijderd. Engagement moet dit framework voor het verzamelen van de IDFA. Echter, IDFA-verzameling kan worden uitgeschakeld \<ios sdk-engagement idfa\> om te voldoen aan het nieuwe Apple-beleid met betrekking tot deze ID.
>
>

## <a name="initialize-the-engagement-sdk"></a>Initialiseren van de Engagement SDK
U moet uw Toepassingsgemachtigde wijzigen:

* Importeer de Engagement-agent aan de bovenkant van het implementatiebestand van uw:

      [...]
      #import "EngagementAgent.h"
* Engagement te initialiseren in de methode '**applicationDidFinishLaunching:**'of'**toepassing: didFinishLaunchingWithOptions:**':

      - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
      {
        [...]
        [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
        [...]
      }

## <a name="basic-reporting"></a>Basic-rapportage
### <a name="recommended-method-overload-your-uiviewcontroller-classes"></a>Aanbevolen methode: overbelasting uw `UIViewController` klassen
Om het rapport van de logboeken die zijn vereist voor Engagement berekenen gebruikers, sessies, activiteiten, Crashes en technische statistieken activeert, kunt u gewoon aanbrengen alle uw `UIViewController` onderliggende klassen overnemen van de `EngagementViewController` klassen (dezelfde regel voor `UITableViewController`  - \> `EngagementTableViewController`).

**Zonder Engagement:**

    #import <UIKit/UIKit.h>

    @interface Tab1ViewController : UIViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

**Met Engagement:**

    #import <UIKit/UIKit.h>
    #import "EngagementViewController.h"

    @interface Tab1ViewController : EngagementViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

### <a name="alternate-method-call-startactivity-manually"></a>Alternatieve methode: call `startActivity()` handmatig
Als u niet kunt of niet wilt overbelasten uw `UIViewController` klassen, in plaats daarvan kunt u uw activiteiten starten door het aanroepen van `EngagementAgent`van methoden rechtstreeks.

> [!IMPORTANT]
> Het bestand iOS SDK automatisch roept de `endActivity()` methode wanneer de toepassing wordt gesloten. Het is dus *maximaal* aanbevolen om aan te roepen de `startActivity` methode wanneer de activiteit van de gebruiker wijzigt, en zo de *nooit* aanroepen de `endActivity` methode, aangezien u deze methode aanroept zorgt ervoor dat de huidige sessie moet worden beëindigd.
>
>

## <a name="location-reporting"></a>Locatierapportage
Toepassingen kunnen gebruikmaken van locatie bijhouden voor statistische doeleinden alleen toegestaan Apple gebruiksrechtovereenkomst niet. Het verdient dus locatie-rapporten alleen inschakelen als uw toepassingen ook gebruikmaken van de locatie voor een andere reden bijhouden.

Beginnen met iOS 8, moet u een beschrijving van hoe uw app gebruikmaakt van locatie-services door in te stellen van een tekenreeks op voor de sleutel opgeven [NSLocationWhenInUseUsageDescription] of [NSLocationAlwaysUsageDescription] in uw app Info.plist-bestand. Als u de Rapportlocatie van het op de achtergrond met Engagement wilt, kunt u de sleutel NSLocationAlwaysUsageDescription toevoegen. In andere gevallen moet u de sleutel NSLocationWhenInUseUsageDescription toevoegen. Houd er rekening mee dat u zowel NSLocationAlwaysAndWhenInUseUsageDescription als NSLocationWhenInUseUsageDescription rapport achtergrond locatie op iOS 11 moet.

### <a name="lazy-area-location-reporting"></a>Vertraagde locatiemelding
Vertraagde locatiemelding kunt voor het rapporteren van het land, streek en plaats die is gekoppeld aan apparaten. Dit type locatie reporting maakt alleen gebruik van netwerklocaties (gebaseerd op de cel-ID of Wi-Fi). Het gebied van het apparaat wordt maximaal eenmaal per sessie gerapporteerd. De GPS wordt nooit gebruikt en dit type locatie rapport heeft slechts weinig (en niet naar Nee zeggen) dus impact op de accu.

Gerapporteerde gebieden worden gebruikt voor het berekenen van geografische statistieken over gebruikers, sessies, gebeurtenissen en fouten. Ze kunnen ook worden gebruikt als criterium in Reach-campagnes. De laatste bekende area gerapporteerd voor een apparaat kan worden opgehaald dank aan de [apparaat-API].

Voeg de volgende regel na tijdens de initialisatie van de agent Engagement zodat vertraagde locatiemelding:

    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
    {
      [...]
      [[EngagementAgent shared] setLazyAreaLocationReport:YES];
      [...]
    }

### <a name="real-time-location-reporting"></a>Rapportage van realtime locatie
Rapportage van realtime locatie kunt voor het rapporteren van de breedtegraad en lengtegraad die is gekoppeld aan apparaten. Standaard alleen gebruik van dit type locatie reporting netwerklocaties (gebaseerd op de cel-ID of Wi-Fi), en de rapportage is alleen beschikbaar wanneer de toepassing wordt uitgevoerd in de voorgrond (dat wil zeggen tijdens een sessie).

Realtime-locaties zijn *niet* om te berekenen van statistieken. Hun enige doel is dat het gebruik van realtime geofencing \<Reach-doelgroep-geofencing\> criterium in Reach-campagnes.

Voeg de volgende regel na tijdens de initialisatie van de agent Engagement zodat rapportage van realtime locatie:

    [[EngagementAgent shared] setRealtimeLocationReport:YES];

#### <a name="gps-based-reporting"></a>GPS op basis van rapportage
Rapportage van realtime locatie alleen gebruikt standaard op basis van netwerklocaties. Als u wilt het gebruik van een GPS inschakelen op basis van locaties (die veel meer nauwkeurige), toevoegen:

    [[EngagementAgent shared] setFineRealtimeLocationReport:YES];

#### <a name="background-reporting"></a>Achtergrond rapportage
Rapportage van realtime locatie is standaard alleen actief wanneer de toepassing wordt uitgevoerd in de voorgrond (dat wil zeggen tijdens een sessie). Schakel de rapportage ook op de achtergrond door toevoegen:

    [[EngagementAgent shared] setBackgroundRealtimeLocationReport:YES withLaunchOptions:launchOptions];

> [!NOTE]
> Wanneer de toepassing op de achtergrond, wordt alleen uitgevoerd op het netwerk locaties worden gerapporteerd, zelfs als u de GPS ingeschakeld.
>
>

Implementatie van deze functie moet worden gebeld [startMonitoringSignificantLocationChanges] wanneer uw toepassing gaat in de achtergrond. Let erop dat deze automatisch opnieuw uw toepassing in de achtergrond Start wordt als een nieuwe locatie gebeurtenis binnenkomt.

## <a name="advanced-reporting"></a>Geavanceerde rapportage
Eventueel, als u rapporteren toepassing specifieke gebeurtenissen, fouten en taken wilt, moet u de Engagement-API gebruiken via de methoden van de `EngagementAgent` klasse. Een object van deze klasse kan worden opgehaald door het aanroepen van de `[EngagementAgent shared]` statische methode.

De Engagement-API kunt u alle Engagement geavanceerde mogelijkheden gebruiken en wordt beschreven in de manier waarop de Engagement-API gebruiken voor iOS (en van de technische documentatie over de `EngagementAgent` klasse).

## <a name="disable-idfa-collection"></a>Uitschakelen van IDFA-verzameling
Standaard Engagement gebruikt de [IDFA] voor het aanduiden van een gebruiker. Maar als u reclame elders in de app niet gebruikt, kunt u geweigerd door het controleproces App Store. IDFA-verzameling kan worden uitgeschakeld door het toevoegen van de preprocessor macro `ENGAGEMENT_DISABLE_IDFA` in uw pch-bestand (of in de `Build Settings` van uw toepassing). Dit zorgt ervoor dat er geen verwijzingen naar is `ASIdentifierManager`, `advertisingIdentifier` of `isAdvertisingTrackingEnabled` bij het opbouwen van de toepassing.

Integratie in de **prefix.pch** bestand:

    #define ENGAGEMENT_DISABLE_IDFA
    ...

U kunt controleren of de IDFA-verzameling is goed uitgeschakeld in uw toepassing door het controleren van de logboeken van de Engagement-test. Zie de integratie testen\<ios-sdk-engagement-test-idfa\> documentatie voor meer informatie.

## <a name="disable-log-reporting"></a>Melden van logboek uitschakelen
### <a name="using-a-method-call"></a>Met behulp van een methodeaanroep van
Als u stoppen met het verzenden van Logboeken Engagement wilt, kunt u bellen:

    [[EngagementAgent shared] setEnabled:NO];

Deze aanroep is permanent: hierbij `NSUserDefaults` de gegevens op te slaan.

U kunt reporting opnieuw door het aanroepen van dezelfde functie met logboek inschakelen `YES`.

### <a name="integration-in-your-settings-bundle"></a>Integratie in de bundel van uw instellingen
In plaats van deze functie aanroept, kunt u deze instelling ook integreren rechtstreeks in uw bestaande `Settings.bundle` bestand. De tekenreeks `engagement_agent_enabled` moeten worden gebruikt als een de voorkeur-id en het moeten zijn gekoppeld aan een schakeloptie (`PSToggleSwitchSpecifier`).

Het volgende voorbeeld van `Settings.bundle` ziet u hoe u deze implementeert:

    <dict>
        <key>PreferenceSpecifiers</key>
        <array>
            <dict>
                <key>DefaultValue</key>
                <true/>
                <key>Key</key>
                <string>engagement_agent_enabled</string>
                <key>Title</key>
                <string>Log reporting enabled</string>
                <key>Type</key>
                <string>PSToggleSwitchSpecifier</string>
            </dict>
        </array>
        <key>StringsTable</key>
        <string>Root</string>
    </dict>

<!-- URLs. -->
[apparaat-API]: http://go.microsoft.com/?linkid=9876094
[NSLocationWhenInUseUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26
[NSLocationAlwaysUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18
[startMonitoringSignificantLocationChanges]:http://developer.apple.com/library/IOs/#documentation/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html#//apple_ref/occ/instm/CLLocationManager/startMonitoringSignificantLocationChanges
[IDFA]:https://developer.apple.com/library/ios/documentation/AdSupport/Reference/ASIdentifierManager_Ref/ASIdentifierManager.html#//apple_ref/occ/instp/ASIdentifierManager/advertisingIdentifier
