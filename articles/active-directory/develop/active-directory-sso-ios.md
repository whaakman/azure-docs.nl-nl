---
title: Het inschakelen van eenmalige aanmelding voor cross-app voor iOS met ADAL | Microsoft Docs
description: 'Het gebruik van de functies van de ADAL-SDK voor eenmalige aanmelding inschakelen tussen uw toepassingen. '
services: active-directory
documentationcenter: 
author: brandwe
manager: mbaldwin
editor: 
ms.assetid: d042d6da-7503-4e20-bb55-06917de01fcd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 04/07/2017
ms.author: brandwe
ms.custom: aaddev
ms.openlocfilehash: 73b8ed7e6a153a0790f7eae9bd51bb2e554ae72e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-enable-cross-app-sso-on-ios-using-adal"></a>Het inschakelen van eenmalige aanmelding voor cross-app voor iOS met ADAL
Mits eenmalige aanmelding (SSO) zodat gebruikers alleen moeten eenmaal hun referenties invoeren en deze referenties automatisch laten samenwerken binnen wordt nu toepassingen door klanten verwacht. De problemen in hun gebruikersnaam en wachtwoord invoeren op een klein scherm, vaak keren gecombineerd met een extra factor (2FA), zoals een telefoongesprek of ge-code, resulteert in een snelle ergernis als een gebruiker heeft om dit te doen meer dan één keer voor het product.

Bovendien, als u een identiteitsplatform die van andere toepassingen zoals Microsoft-Accounts of een werkaccount van Office365 gebruikmaken mogelijk toepast, verwachten klanten dat de referenties die moeten zijn beschikbaar voor gebruik in hun toepassingen ongeacht de leverancier.

De Microsoft Identity-platform, samen met onze SDK's van Microsoft Identity doet dit harde werk voor u en biedt u de mogelijkheid om u te overleggen van uw klanten met eenmalige aanmelding zich binnen uw eigen reeks toepassingen of, net als bij onze broker-capaciteit en de verificator-toepassingen, via het hele apparaat.

In dit scenario wordt uitgelegd hoe u onze SDK in uw toepassing voordeel bieden aan uw klanten configureren.

In dit scenario is van toepassing op:

* Azure Active Directory
* Azure Active Directory B2C
* Azure Active Directory B2B
* Voorwaardelijke toegang voor Azure Active Directory

De voorgaande document wordt ervan uitgegaan dat u weet hoe [inrichten van toepassingen in de oude portal voor Azure Active Directory](active-directory-how-to-integrate.md) en uw toepassing met geïntegreerde de [Microsoft Identity iOS SDK](https://github.com/AzureAD/azure-activedirectory-library-for-objc).

## <a name="sso-concepts-in-the-microsoft-identity-platform"></a>SSO-concepten in het Identiteitsplatform van Microsoft
### <a name="microsoft-identity-brokers"></a>Microsoft Identity Beleggingsmakelaars
Microsoft biedt toepassingen voor elk mobiel platform voor het overbruggen van referenties voor toepassingen van verschillende leveranciers en kan voor speciale verbeterde functies waarvoor een enkele veilige plaats waar om referenties te valideren. We noemen deze **beleggingsmakelaars**. Op iOS en Android worden deze beleggingsmakelaars opgegeven downloadbare toepassingen dat klanten afzonderlijk installeren of door een bedrijf die enkele of alle van het apparaat voor hun werknemers beheert op het apparaat kunnen worden geactiveerd. Deze beleggingsmakelaars ondersteuning voor het beheer van beveiliging voor sommige toepassingen of het hele apparaat op basis van wat de IT-beheerders willen. In Windows, wordt deze functionaliteit verstrekt door een ingebouwd in het besturingssysteem en technisch als de Webauthenticatiebroker bekend kiezen.

Voor meer informatie over hoe we gebruiken deze beleggingsmakelaars en hoe uw klanten mogelijk ze in hun aanmelding stroom voor het lezen van Microsoft Identity-platform.

### <a name="patterns-for-logging-in-on-mobile-devices"></a>Patronen voor logboekregistratie in op mobiele apparaten
Toegang tot de referenties op apparaten als volgt twee basispatronen die voor de Microsoft Identity-platform:

* Niet-broker assisted-aanmeldingen
* Assisted aanmeldingen Broker

#### <a name="non-broker-assisted-logins"></a>Niet-broker assisted-aanmeldingen
Niet-broker assisted aanmeldingen zijn aanmelding ervaringen die gebeuren inline met de toepassing en het gebruik van de lokale opslag op het apparaat voor die toepassing. Deze opslag kan worden gedeeld tussen toepassingen, maar de referenties zijn nauw gekoppeld aan de app of suite van apps met behulp van deze referentie. U hebt waarschijnlijk is dit in veel mobiele toepassingen wanneer u een gebruikersnaam en wachtwoord in de toepassing zelf invoert.

Deze aanmeldingen hebben de volgende voordelen:

* Er bestaat een gebruikerservaring volledig in de toepassing.
* Referenties kunnen worden gedeeld door toepassingen die zijn ondertekend met hetzelfde certificaat, een ervaring voor één aanmelding om uw pakket toepassingen te bieden.
* Besturingselement rond de ervaring van logboekregistratie in is opgegeven voor de toepassing vóór en na het aanmelden.

Deze aanmeldingen hebben de volgende nadelen:

* Kan niet-gebruikerservaring eenmalige aanmelding via alle apps die gebruikmaken van een Microsoft-Identity alleen via de Microsoft-Identities die uw toepassing is geconfigureerd.
* Uw toepassing kan niet worden gebruikt met meer geavanceerde zakelijke functies zoals voorwaardelijke toegang, of gebruik het InTune-pakket.
* Uw toepassing ondersteunt geen verificatie op basis van certificaten voor zakelijke gebruikers.

Hier volgt een weergave over de werking van de Microsoft Identity-SDK's met de gedeelde opslag van uw toepassingen en eenmalige aanmelding inschakelen:

```
+------------+ +------------+  +-------------+
|            | |            |  |             |
|   App 1    | |   App 2    |  |   App 3     |
|            | |            |  |             |
|            | |            |  |             |
+------------+ +------------+  +-------------+
| ADAL SDK  |  |  ADAL SDK  |  |  ADAK SDK   |
+------------+-+------------+--+-------------+
|                                            |
|            App Shared Storage              |
+--------------------------------------------+
```

#### <a name="broker-assisted-logins"></a>Assisted aanmeldingen Broker
Aanmeldingen Broker-ondersteunde zijn aanmelding ervaringen die optreden in de broker-toepassing en de opslag en de beveiliging van de broker gebruiken voor het delen van referenties voor alle toepassingen die de Microsoft Identity-platform van toepassing op het apparaat. Dit betekent dat uw toepassingen afhankelijk zijn van de broker gebruikers aanmelden. Op iOS en Android worden deze beleggingsmakelaars opgegeven downloadbare toepassingen dat klanten afzonderlijk installeren of op het apparaat kunnen worden geactiveerd door een bedrijf die het apparaat voor de gebruiker beheert. Een voorbeeld van dit type toepassing is de Microsoft Authenticator-toepassing op iOS. In Windows worden deze functionaliteit wordt verstrekt door een ingebouwd in het besturingssysteem en technisch als de Webauthenticatiebroker bekend kiezen.
De ervaring varieert per platform en kan soms worden verstoren aan gebruikers als dat niet correct worden beheerd. U kunt waarschijnlijk meest bekend zijn met dit patroon als u de Facebook-toepassing geïnstalleerd en Facebook-verbinding van een andere toepassing. De Microsoft Identity-platform gebruikt hetzelfde patroon.

Voor iOS die dit tot een 'overgang leidt' komt animatie waarbij uw toepassing wordt verzonden naar de achtergrond tijdens de Microsoft Authenticator toepassingen naar de achtergrond voor de gebruiker te selecteren welke account die ze graag willen aanmelden.  

Voor Android en Windows wordt de account-kiezer boven op uw toepassing minder verstoren aan de gebruiker weergegeven.

#### <a name="how-the-broker-gets-invoked"></a>Hoe de broker opgehaald aangeroepen
Als een compatibel broker is geïnstalleerd op het apparaat, zoals de toepassing Microsoft Authenticator doet de Microsoft Identity-SDK's automatisch het werk van de broker voor u wordt aangeroepen wanneer een gebruiker aangeeft voor het aanmelden met een account van de Microsoft Identity-platform. Dit account kan worden een persoonlijk Microsoft-Account, werk of schoolaccount of een account dat u opgeeft en host in Azure met behulp van onze producten B2C en B2B. 

 #### <a name="how-we-ensure-the-application-is-valid"></a>Hoe we zorgen dat de toepassing is geldig
 
 De noodzaak om te controleren of de identiteit van een aanroep van de toepassing die de broker is van cruciaal belang voor de beveiliging we in de broker bieden ondersteunde aanmeldingen. Geen iOS of Android zorgt ervoor dat unieke id's die alleen geldig voor een bepaalde toepassing zijn zodat schadelijke toepassingen mogelijk '' een legitieme toepassings-id vervalsen en de tokens die zijn bedoeld voor de toepassing legitieme ontvangen. Om ervoor te zorgen dat we altijd communiceren met de juiste toepassing tijdens runtime, vraagt u de ontwikkelaar een aangepaste redirectURI bieden bij het registreren van de toepassing met Microsoft. **Hoe ontwikkelaars deze omleidings-URI moeten stellen wordt hieronder in detail besproken.** Deze aangepaste redirectURI de bundel-ID van de toepassing bevat en uniek zijn voor de toepassing door de Apple App Store is gewaarborgd. Wanneer een toepassing de broker aanroept, vraagt de broker de iOS-besturingssysteem om te voorzien de bundel-ID die de broker aangeroepen. De broker biedt deze bundel-ID naar Microsoft in de aanroep naar ons identiteitssysteem. Als de bundel-ID van de toepassing komt niet overeen met de bundel-ID aan ons door de ontwikkelaar tijdens de registratie wordt geleverd, wordt er geweigerd aanvragen van toegang tot de tokens voor de bron van de toepassing. Deze controle zorgt ervoor dat alleen de toepassing die zijn geregistreerd door de ontwikkelaar tokens ontvangt.

**De ontwikkelaar heeft de keuze van als de SDK van Microsoft Identity de broker roept of gebruikt de niet-broker assisted-stroom.** Als de ontwikkelaar ervoor kiest geen gebruik van de broker-ondersteunde stroom te ze echter het voordeel verliezen van het gebruik van eenmalige aanmeldingsreferenties dat de gebruiker mogelijk al toegevoegd op het apparaat en wordt voorkomen dat de toepassing wordt gebruikt met zakelijke functies die Microsoft zijn klanten zoals voorwaardelijke toegang, beheermogelijkheden voor Intune en verificatie op basis van certificaten biedt.

Deze aanmeldingen hebben de volgende voordelen:

* Gebruiker merkt dat eenmalige aanmelding via hun toepassingen ongeacht de leverancier.
* Uw toepassing kunt meer geavanceerde zakelijke functies zoals voorwaardelijke toegang gebruiken of de InTune-productreeks gebruikt.
* Uw toepassing kan ondersteuning voor verificatie op basis van certificaten voor zakelijke gebruikers.
* Veel veiligere aanmeldingservaring als de identiteit van de toepassing en de gebruiker worden geverifieerd door de toepassing broker met algoritmen voor extra beveiliging en versleuteling.

Deze aanmeldingen hebben de volgende nadelen:

* De gebruiker is overgegaan buiten de ervaring van uw toepassing in iOS terwijl referenties worden gekozen.
* Verlies van de mogelijkheid voor het beheren van de aanmeldingservaring voor uw klanten in uw toepassing.

Hier volgt een weergave over de werking van de Microsoft Identity-SDK's met de toepassingen broker eenmalige aanmelding inschakelen:

```
+------------+ +------------+   +-------------+
|            | |            |   |             |
|   App 1    | |   App 2    |   |   Someone   |
|            | |            |   |    Else's   |
|            | |            |   |     App     |
+------------+ +------------+   +-------------+
| Azure SDK  | | Azure SDK  |   | Azure SDK   |
+-----+------+-+-----+------+-  +-------+-----+
      |              |                  |
      |       +------v------+           |
      |       |             |           |
      |       | Microsoft   |           |
      +-------> Broker      |^----------+
              | Application
              |             |
              +-------------+
              |             |
              |   Broker    |
              |   Storage   |
              |             |
              +-------------+
```

Uitgerust met deze achtergrondinformatie u kunnen moet om beter te begrijpen en eenmalige aanmelding implementeren in uw toepassing met de Microsoft Identity-platform en SDK's.

## <a name="enabling-cross-app-sso-using-adal"></a>Inschakelen van verschillende Apps eenmalige aanmelding met ADAL
Hier gebruiken we de ADAL iOS SDK naar:

* Geen broker assisted eenmalige aanmelding voor uw suite van apps inschakelen
* Ondersteuning voor broker-ondersteunde eenmalige aanmelding inschakelen

### <a name="turning-on-sso-for-non-broker-assisted-sso"></a>Eenmalige aanmelding inschakelen voor niet-broker ondersteund eenmalige aanmelding
Geen broker assisted SSO op toepassingen met beheren de Microsoft Identity-SDK's voor veel van de complexiteit van eenmalige aanmelding voor u. Dit omvat de juiste gebruiker zoeken in de cache en onderhouden van een lijst met aangemelde gebruikers om te zoeken.

Eenmalige aanmelding inschakelen voor toepassingen die u bezit, moet u het volgende doen:

1. Zorg ervoor dat alle gebruikers van uw toepassingen dezelfde Client-ID of id van toepassing.
2. Zorg ervoor dat alle toepassingen het dezelfde handtekeningcertificaat van Apple delen, zodat u sleutelhangers kan delen
3. Aanvragen het dezelfde sleutelhanger recht voor elk van uw toepassingen.
4. Geef de Microsoft Identity-SDK's over de gedeelde sleutelhanger dat u wenst te gebruiken.

#### <a name="using-the-same-client-id--application-id-for-all-the-applications-in-your-suite-of-apps"></a>Met behulp van dezelfde Client-ID / toepassing-ID voor alle toepassingen in uw suite van apps
In de volgorde voor het platform voor Microsoft Identity weten dat het delen van tokens via uw toepassingen is toegestaan, moet elk van uw toepassingen delen van hetzelfde Client-ID of id van toepassing. Dit is de unieke id die aan u is opgegeven bij de registratie van uw eerste toepassing in de portal.

U vraagt zich misschien af hoe u verschillende apps met de Microsoft Identity-service wordt geïdentificeerd als deze gebruikmaakt van de dezelfde toepassing-ID. Het antwoord is met de **omleidings-URI's**. Elke toepassing kan meerdere omleidings-URI's geregistreerd in de portal voor onboarding hebben. Elke app in de suite hebben een verschillende omleidings-URI. Een voorbeeld van hoe dit eruitziet lager is dan:

App1 omleidings-URI:`x-msauth-mytestiosapp://com.myapp.mytestapp`

App2 omleidings-URI:`x-msauth-mytestiosapp://com.myapp.mytestapp2`

App3 omleidings-URI:`x-msauth-mytestiosapp://com.myapp.mytestapp3`

....

Deze zijn genest onder dezelfde client-ID / toepassings-ID en opgezocht op basis van de omleidings-URI die u in de configuratie van de SDK aan ons retourneren.

```
+-------------------+
|                   |
|  Client ID        |
+---------+---------+
          |
          |           +-----------------------------------+
          |           |  App 1 Redirect URI               |
          +----------^+                                   |
          |           +-----------------------------------+
          |
          |           +-----------------------------------+
          +----------^+  App 2 Redirect URI               |
          |           |                                   |
          |           +-----------------------------------+
          |
          +----------^+-----------------------------------+
                      |  App 3 Redirect URI               |
                      |                                   |
                      +-----------------------------------+

```


*Houd er rekening mee dat de indeling van deze omleidings-URI's hieronder worden beschreven. U kunt een omleidings-URI tenzij u wilt ondersteunen de broker in dat geval moeten ze ziet er ongeveer als de bovenstaande*

#### <a name="create-keychain-sharing-between-applications"></a>Sleutelhanger delen tussen toepassingen maken
Inschakelen van het delen van sleutelketens valt buiten het bereik van dit document en in het document wordt gedekt door Apple [mogelijkheden toevoegen](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html). Wat is het belangrijk is dat u wat de sleutelketen bepalen moet worden aangeroepen en voeg die mogelijkheid toe op alle toepassingen met uw.

Wanneer u hebt rechten instellen correct u ziet een bestand in uw projectmap recht `entitlements.plist` die iets dat op het volgende lijkt bevat:

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>keychain-access-groups</key>
    <array>
        <string>$(AppIdentifierPrefix)com.myapp.mytestapp</string>
        <string>$(AppIdentifierPrefix)com.myapp.mycache</string>
    </array>
</dict>
</plist>
```

Zodra u het recht sleutelhanger is ingeschakeld in elk van uw toepassingen hebt en u klaar bent voor gebruik van eenmalige aanmelding, laat u de Identity-SDK van Microsoft over de sleutelketen met behulp van de volgende instelling in uw `ADAuthenticationSettings` met de volgende instellingen:

```
defaultKeychainSharingGroup=@"com.myapp.mycache";
```

> [!WARNING]
> Wanneer u een sleutelhanger in uw toepassingen delen kan elke toepassing of verwijderen gebruikers slechter alle tokens inlezen in uw toepassing. Dit is vooral rampzalig zijn als er toepassingen die afhankelijk zijn van de tokens werk op de achtergrond. Een sleutelhanger delen verwijderen betekent dat u voorzichtig in alle moet bewerkingen via de Microsoft Identity-SDK's.
> 
> 

Dat is alles. De Microsoft Identity-SDK wordt nu referenties delen in al uw toepassingen. De lijst met gebruikers wordt ook gedeeld met exemplaren van een toepassing.

### <a name="turning-on-sso-for-broker-assisted-sso"></a>Eenmalige aanmelding inschakelen voor broker ondersteund eenmalige aanmelding
De mogelijkheid om een toepassing te gebruiken broker die is geïnstalleerd op het apparaat is **standaard uitgeschakeld**. Als u wilt gebruiken van uw toepassing met de broker moet u enkele aanvullende configuratiestappen en code toevoegen aan uw toepassing.

De stappen te volgen zijn:

1. Schakel de broker-modus in uw toepassingscode aanroep van de MS-SDK.
2. Stel een nieuw omleidings-URI en bieden die zowel de app en de registratie van uw app.
3. Een URL-schema wordt geregistreerd.
4. Ondersteuning voor iOS9: een machtiging voor het toevoegen aan uw info.plist-bestand.

#### <a name="step-1-enable-broker-mode-in-your-application"></a>Stap 1: Broker-modus inschakelen in uw toepassing
De mogelijkheid voor uw toepassing gebruik van de broker is ingeschakeld wanneer u de 'context' of de eerste installatie van de verificatie-object maakt. U doen dit door het instellen van het type van uw referenties in uw code:

```
/*! See the ADCredentialsType enumeration definition for details */
@propertyADCredentialsType credentialsType;
```
De `AD_CREDENTIALS_AUTO` instelling kunnen de Microsoft Identity SDK om te proberen aan te roepen voor de broker `AD_CREDENTIALS_EMBEDDED` wordt voorkomen dat de SDK van Microsoft Identity aanroepen van de broker.

#### <a name="step-2-registering-a-url-scheme"></a>Stap 2: Registreren van een URL-schema
De Microsoft Identity-platform gebruikt de URL's voor de broker aanroepen en vervolgens terugkeren naar uw toepassing. Voor het voltooien van deze round trip moet u een URL-schema voor uw toepassing waarmee u het platform voor Microsoft Identity weten over geregistreerd. Dit kan zijn naast een andere app-schema's die kan u eerder hebt geregistreerd met uw toepassing.

> [!WARNING]
> Is het raadzaam om het URL-schema redelijk uniek is voor het beperken van de kans op een andere app met behulp van dezelfde URL-schema. Apple worden niet afgedwongen door de uniekheid van URL-schema's die zijn geregistreerd in de appstore.
> 
> 

Hieronder volgt een voorbeeld van hoe deze wordt weergegeven in de projectconfiguratie van uw. U kunt dit ook doen in XCode ook:

```
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>com.myapp.mytestapp</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>x-msauth-mytestiosapp</string>
        </array>
    </dict>
</array>
```

#### <a name="step-3-establish-a-new-redirect-uri-with-your-url-scheme"></a>Stap 3: Een nieuwe omleidings-URI met URL-schema vaststellen
Om ervoor te zorgen dat we de referentie-tokens altijd naar de juiste toepassing terugkeren, moeten we controleren of dat we noemen terug naar de toepassing op een manier die het iOS-besturingssysteem kunt controleren. Rapporten over de iOS-besturingssysteem voor de broker-toepassingen van Microsoft de bundel-ID van de toepassing het aanroepen. Dit kan niet door een rogue-toepassing worden vervalst. Daarom we maken gebruik van dit samen met de URI van onze toepassing broker om ervoor te zorgen dat de tokens worden geretourneerd naar de juiste toepassing. Moet u voor het maken van deze unieke omleidings-URI voor beide in uw toepassing instellen als een omleidings-URI in onze portal voor ontwikkelaars.

De omleidings-URI moet de juiste indeling zijn:

`<app-scheme>://<your.bundle.id>`

Voorbeeld: *x-msauth-mytestiosapp://com.myapp.mytestapp*

Deze omleidings-URI moet worden opgegeven in uw app registreren met de [Azure-portal](https://portal.azure.com/). Zie voor meer informatie over Azure AD-app-registratie [integreren met Azure Active Directory](active-directory-how-to-integrate.md).

##### <a name="step-3a-add-a-redirect-uri-in-your-app-and-dev-portal-to-support-certificate-based-authentication"></a>Stap 3a: Voeg een omleidings-URI in uw app en Developer-portal voor de ondersteuning van verificatie op basis
Verificatie van ondersteuning cert op basis van een tweede 'msauth' moet worden geregistreerd in uw toepassing en de [Azure-portal](https://portal.azure.com/) voor het afhandelen van verificatie via certificaat als u wilt toevoegen die ondersteuning bieden in uw toepassing.

`msauth://code/<broker-redirect-uri-in-url-encoded-form>`

Voorbeeld: *msauth://code/x-msauth-mytestiosapp%3A%2F%2Fcom.myapp.mytestapp*

#### <a name="step-4-ios9-add-a-configuration-parameter-to-your-app"></a>Stap 4: iOS9: een configuratieparameter toevoegen aan uw app
– CanOpenURL maakt gebruik van ADAL: om te controleren of de broker op het apparaat is geïnstalleerd. In iOS vergrendeld 9 Apple wat schema's een toepassing kunnen opvragen. U moet 'msauth' toevoegen aan de sectie LSApplicationQueriesSchemes van uw `info.plist file`.

<key>LSApplicationQueriesSchemes</key>

<array><string>msauth</string>
</array>

### <a name="youve-configured-sso"></a>U kunt eenmalige aanmelding hebt geconfigureerd.
Nu wordt de Microsoft Identity-SDK automatisch zowel referenties delen in uw toepassingen en aanroepen van de broker, indien aanwezig zijn op hun apparaat.

