---
title: Het inschakelen van eenmalige aanmelding voor cross-app voor iOS met behulp van ADAL | Microsoft Docs
description: 'Het gebruik van de functies van de ADAL-SDK voor eenmalige aanmelding inschakelen voor uw toepassingen. '
services: active-directory
author: CelesteDG
manager: mtillman
ms.assetid: d042d6da-7503-4e20-bb55-06917de01fcd
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 04/07/2017
ms.author: celested
ms.reviewer: brandwe
ms.custom: aaddev
ms.openlocfilehash: 203cb4f57cfa50a17b66a9b70a44568e57ec4835
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39601974"
---
# <a name="how-to-enable-cross-app-sso-on-ios-using-adal"></a>Het inschakelen van eenmalige aanmelding voor cross-app voor iOS met ADAL

Met eenmalige aanmelding (SSO) zodat gebruikers alleen hoeft te eenmaal zijn referenties invoeren en deze referenties automatisch laten werken via wordt nu toepassingen door klanten verwacht. De problemen in hun gebruikersnaam en wachtwoord invoeren op een klein scherm, vaak tijden in combinatie met een extra factor (2FA), zoals een telefoongesprek of ge-sms't-code, resulteert in een snelle ontevredenheid als een gebruiker heeft om dit te doen voor uw product meer dan één keer.

Bovendien, als u een identity-platform die van andere toepassingen zoals Microsoft-Accounts of werk uit Office 365 gebruikmaken mogelijk toepast, verwachten klanten dat die referenties die moeten worden gebruikt voor al hun toepassingen, ongeacht de leverancier.

De Microsoft Identity-platform, samen met onze SDK's van Microsoft Identity doet dit harde werk voor u en biedt u de mogelijkheid om te Verras uw klanten met eenmalige aanmelding zich binnen uw eigen reeks toepassingen of, net als bij onze broker-functie en de verificator toepassingen en het hele apparaat.

In dit scenario wordt uitgelegd hoe u onze SDK in uw toepassing voor dit voordeel voor uw klanten te configureren.

In dit scenario van toepassing op:

* Azure Active Directory
* Azure Active Directory B2C
* Azure Active Directory B2B
* Voorwaardelijke toegang voor Azure Active Directory

De voorgaande document wordt ervan uitgegaan dat u weet hoe u [inrichten voor Azure Active Directory-toepassingen in de oude portal](active-directory-how-to-integrate.md) en geïntegreerd van uw toepassing met de [Microsoft Identity iOS SDK](https://github.com/AzureAD/azure-activedirectory-library-for-objc).

## <a name="sso-concepts-in-the-microsoft-identity-platform"></a>Concepten van eenmalige aanmelding in het Microsoft Identity-Platform

### <a name="microsoft-identity-brokers"></a>Microsoft Identity Brokers

Microsoft biedt toepassingen voor elk mobiel platform voor het overbruggen van referenties voor toepassingen van verschillende leveranciers en kan voor speciale verbeterde functies waarvoor een enkele veilige plaats waar om referenties te valideren. We noemen deze **brokers**. Op iOS en Android worden deze brokers geleverd via downloadbare toepassingen dat klanten onafhankelijk installeren of op het apparaat kunnen worden geactiveerd door een bedrijf dat sommige of alle van het apparaat voor hun werknemers beheert. Deze brokers ondersteuning voor beheer van beveiliging voor sommige toepassingen of het hele apparaat op basis van wat de IT-beheerders kunnen vereisen. In Windows, wordt deze functionaliteit verstrekt door een accountkiezer die is ingebouwd in het besturingssysteem, ook wel technisch gezien als de Webauthenticatiebroker.

Lees verder voor meer informatie over hoe we deze brokers gebruiken en hoe uw klanten mogelijk zien ze in de aanmeldingsprocedure voor het Microsoft Identity-platform.

### <a name="patterns-for-logging-in-on-mobile-devices"></a>Patronen voor logboekregistratie in op mobiele apparaten

Toegang tot de referenties op apparaten als volgt twee algemene patronen voor het Microsoft Identity-platform:

* Niet-broker assisted aanmeldingen
* Assisted aanmeldingen Broker

#### <a name="non-broker-assisted-logins"></a>Niet-broker assisted aanmeldingen

Niet-broker assisted aanmeldingen zijn aanmelding ervaringen die inline met de toepassing gebeuren en het gebruik van de lokale opslag op het apparaat voor de toepassing. Deze opslag kan worden gedeeld met toepassingen, maar de referenties zijn nauw gekoppeld aan de app of suite van apps met behulp van deze referentie. U hebt waarschijnlijk is dit in veel mobiele toepassingen bij het invoeren van een gebruikersnaam en wachtwoord in de toepassing zelf.

Deze aanmeldingen hebben de volgende voordelen:

* Er bestaat een gebruikerservaring volledig in de toepassing.
* Referenties kunnen worden gedeeld met toepassingen die zijn ondertekend met hetzelfde certificaat, biedt een eenmalige aanmelding mogelijk met uw suite van toepassingen.
* Controle over de ervaring van het aanmelden wordt geleverd aan de toepassing vóór en na het aanmelden.

Deze aanmeldingen hebben de volgende nadelen:

* Gebruiker kan geen eenmalige aanmelding optreden in alle apps die gebruikmaken van een Microsoft-Identity alleen via de Microsoft-Identities die uw toepassing is geconfigureerd.
* Uw toepassing kan niet worden gebruikt met meer geavanceerde functies zoals voorwaardelijke toegang of gebruik de Intune-suite met producten.
* Uw toepassing kan geen ondersteuning voor verificatie op basis van certificaten voor zakelijke gebruikers.

Dit is een weergave van de werking van de Microsoft Identity-SDK's met de gedeelde opslag van uw toepassingen SSO inschakelen:

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

Ondersteuning van de Broker-aanmelding zijn aanmelding ervaringen die optreden in de broker-toepassing en de opslag en de beveiliging van de broker gebruiken voor het delen van referenties voor alle toepassingen op het apparaat die van toepassing het Microsoft Identity-platform zijn. Dit betekent dat uw toepassingen zijn afhankelijk van de broker gebruikers zich aanmelden. Op iOS en Android, worden deze brokers geleverd via downloadbare toepassingen dat klanten onafhankelijk installeren of gepusht naar het apparaat door een bedrijf dat het apparaat voor de gebruiker wordt beheerd. Een voorbeeld van dit type toepassing is de Microsoft Authenticator-toepassing op iOS. Deze functionaliteit wordt in Windows geleverd door een accountkiezer die is ingebouwd in het besturingssysteem, ook wel technisch gezien als de Webauthenticatiebroker.
De ervaring verschilt per platform en kan soms verstorend voor gebruikers zijn als het niet correct worden beheerd. U bent waarschijnlijk meest bekend zijn met dit patroon als u de Facebook-toepassing geïnstalleerd en Facebook verbinding maken met gebruik van een andere toepassing. Het Microsoft Identity-platform gebruikt hetzelfde patroon.

Voor iOS die dit tot een 'overgang leidt"komt animatie waarbij uw toepassing op de achtergrond tijdens de toepassingen van de Microsoft Authenticator wordt verzonden naar de voorgrond is geplaatst voor de gebruiker selecteren welk account ze wilt zich aanmelden met. 

Voor Android en Windows de accountkiezer boven op uw toepassing wordt weergegeven, dit is minder verstorend voor de gebruiker.

#### <a name="how-the-broker-gets-invoked"></a>Hoe de broker wordt aangeroepen

Als een compatibel broker is geïnstalleerd op het apparaat, zoals de Microsoft Authenticator-toepassing, doen de Microsoft Identity-SDK's automatisch het werk van het aanroepen van de broker voor u wanneer een gebruiker geeft aan dat ze willen aanmelden met een van de Microsoft-account Identity-platform. Dit account wordt mogelijk een persoonlijk Microsoft-Account, een werk of school-account, of een account dat u opgeeft en host in Azure met behulp van onze producten B2C en B2B.

#### <a name="how-we-ensure-the-application-is-valid"></a>Hoe we ervoor dat de toepassing is geldig
 
 De noodzaak om te controleren of de identiteit van de aanroep van een toepassing die de broker is van cruciaal belang voor de beveiliging we in de broker bieden telefonische aanmeldingen. Geen iOS of Android wordt afgedwongen unieke id's die alleen geldig voor een bepaalde toepassing zijn zodat schadelijke toepassingen mogelijk '"een legitieme toepassings-id vervalsen en de tokens die zijn bedoeld voor de legitieme aanvraag ontvangen. Om ervoor te zorgen dat er altijd communicatie is met de juiste toepassing tijdens runtime, vragen we de ontwikkelaar voor een aangepaste redirectURI bij het registreren van hun toepassing met Microsoft. **Hoe ontwikkelaars deze omleidings-URI moeten stellen wordt besproken in de hieronder beschreven.** Deze aangepaste redirectURI bevat de bundel-ID van de toepassing en moet uniek zijn voor de toepassing door de Apple App Store is gewaarborgd. Wanneer een toepassing de broker aanroept, gevraagd de broker de iOS-besturingssysteem om te voorzien de bundel-ID die de broker aangeroepen. De broker biedt deze bundel-ID naar Microsoft in de aanroep naar ons identiteitssysteem. Als de bundel-ID van de toepassing komt niet overeen met de bundel-ID die aan ons wordt verstrekt door de ontwikkelaar tijdens de registratie, weigert we de aanvragen van toegang tot de tokens voor de resource van de toepassing. Deze controle zorgt ervoor dat alleen de door de ontwikkelaar van de geregistreerde toepassing tokens ontvangen.

**De ontwikkelaar is de keuze van als de Microsoft Identity-SDK de broker roept of de niet-broker assisted stroom gebruikt.** Maar als de ontwikkelaar kiest niet voor de ondersteuning van de broker-stroom gaat het voordeel verloren van het gebruik van eenmalige aanmelding referenties van de gebruiker kan al hebt toegevoegd op het apparaat en wordt voorkomen dat de toepassing die wordt gebruikt met functies voor Microsoft biedt de klanten zoals voorwaardelijke toegang en mogelijkheden voor Intune certificaten gebaseerde verificatie.

Deze aanmeldingen hebben de volgende voordelen:

* Gebruikerservaringen SSO voor al hun toepassingen, ongeacht de leverancier.
* Uw toepassing kunt meer geavanceerde functies zoals voorwaardelijke toegang gebruikt of de Intune-suite met producten.
* Uw toepassing kan ondersteuning voor verificatie op basis van certificaten voor zakelijke gebruikers.
* Veel beter beveiligd aanmelden ervaring als de identiteit van de toepassing en de gebruiker worden geverifieerd door de broker-toepassing met algoritmen voor extra beveiliging en versleuteling.

Deze aanmeldingen hebben de volgende nadelen:

* In iOS wordt de gebruiker overgebracht buiten de ervaring van uw toepassing terwijl referenties worden gekozen.
* Verlies van de mogelijkheid voor het beheren van de aanmeldingservaring voor uw klanten in uw toepassing.

Dit is een weergave van de werking van de Microsoft Identity-SDK's met de broker-toepassingen voor eenmalige aanmelding inschakelen:

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

Met deze achtergrondinformatie die u moet kunnen beter te begrijpen en implementeren van eenmalige aanmelding in uw toepassing met behulp van de Microsoft Identity-platform en SDK's hebt.

## <a name="enabling-cross-app-sso-using-adal"></a>Eenmalige aanmelding van verschillende Apps met behulp van ADAL inschakelen

Hier gebruiken we de ADAL iOS SDK aan:

* Niet-broker assisted eenmalige aanmelding voor uw suite van apps inschakelen
* Ondersteuning voor ondersteuning van de broker eenmalige aanmelding inschakelen

### <a name="turning-on-sso-for-non-broker-assisted-sso"></a>Eenmalige aanmelding telefonische inschakelen van eenmalige aanmelding voor niet-broker

De eenmalige aanmelding voor niet-broker assisted verschillende toepassingen beheren de Microsoft Identity-SDK's voor veel van de complexiteit van eenmalige aanmelding voor u. Dit omvat de juiste gebruiker zoeken in de cache en onderhouden van een lijst met aangemelde gebruikers voor u om op te vragen.

Eenmalige aanmelding inschakelen voor toepassingen die u bezit, moet u het volgende doen:

1. Zorg ervoor dat alle gebruikers in uw toepassingen op dezelfde Client-ID of toepassings-ID.
2. Zorg ervoor dat al uw toepassingen de dezelfde handtekeningcertificaat van Apple, delen zodat u van sleutelhangers delen kunt
3. Aanvragen van hetzelfde recht sleutelhanger voor elk van uw toepassingen.
4. Vertel het Microsoft Identity-SDK's over de gedeelde sleutelketen dat u wenst te gebruiken.

#### <a name="using-the-same-client-id--application-id-for-all-the-applications-in-your-suite-of-apps"></a>Met behulp van dezelfde Client-ID / toepassing-ID voor alle toepassingen in uw apps-pakket

In de volgorde voor het Microsoft Identity-platform om te weten dat het is toegestaan voor het delen van tokens voor uw toepassingen, moet elk van uw toepassingen voor het delen van dezelfde Client-ID of toepassings-ID. Dit is de unieke id die aan u is opgegeven bij de registratie van uw eerste toepassing in de portal.

U vraagt zich misschien af hoe u verschillende apps met de Microsoft Identity-service wordt geïdentificeerd als deze gebruikmaakt van de dezelfde toepassings-ID. Het antwoord is met de **omleidings-URI's**. Elke toepassing kan meerdere omleidings-URI's geregistreerd in de portal voor onboarding hebben. Elke app in uw pakket heeft een andere omleidings-URI. Een voorbeeld van hoe dit eruitziet lager is dan:

App1 omleidings-URI: `x-msauth-mytestiosapp://com.myapp.mytestapp`

App2 omleidings-URI: `x-msauth-mytestiosapp://com.myapp.mytestapp2`

App3 omleidings-URI: `x-msauth-mytestiosapp://com.myapp.mytestapp3`

....

Deze worden genest onder dezelfde client-ID / toepassings-ID en opgezocht op basis van de omleidings-URI u terug naar ons in de configuratie van de SDK.

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


*Houd er rekening mee dat de indeling van deze omleidings-URI's hieronder wordt uitgelegd. U kunt een omleidings-URI, tenzij u wilt ondersteunen voor de broker in dat geval moeten ze ziet er ongeveer als de bovenstaande*

#### <a name="create-keychain-sharing-between-applications"></a>Sleutelhanger delen tussen toepassingen maken

Delen van sleutelketens inschakelen valt buiten het bereik van dit document en in het document wordt gedekt door Apple [mogelijkheden toevoegen](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html). Wat is het belangrijk is dat u wat u wilt uw sleutelhanger bepalen te worden aangeroepen en het toevoegen van die mogelijkheid voor al uw toepassingen.

Als u over rechten instellen correct u ziet een bestand in uw projectmap, hebben recht `entitlements.plist` die iets dat lijkt op het volgende bevat:

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

Nadat u de rechten van een sleutelhanger ingeschakeld in elk van uw toepassingen, en u klaar bent voor gebruik van eenmalige aanmelding, vertelt u de Microsoft Identity-SDK over uw sleutelketen met behulp van de volgende instelling in uw `ADAuthenticationSettings` met de volgende instellingen:

```
defaultKeychainSharingGroup=@"com.myapp.mycache";
```

> [!WARNING]
> Wanneer u een sleutelhanger via uw toepassingen delen kunt elke toepassing gebruikers verwijderen of nog erger alle tokens in uw toepassing te verwijderen. Dit is vooral rampzalig zijn als uw toepassingen die afhankelijk zijn van de tokens werk op de achtergrond. Een sleutelhanger delen verwijderen betekent dat u voorzichtig in alle moet bewerkingen via de Microsoft Identity-SDK's.

Dat is alles. De Microsoft Identity-SDK wordt nu referenties voor al uw toepassingen delen. De lijst met gebruikers kan ook worden gedeeld tussen toepassingsexemplaren.

### <a name="turning-on-sso-for-broker-assisted-sso"></a>Eenmalige aanmelding telefonische inschakelen van eenmalige aanmelding voor broker

De mogelijkheid om een toepassing te gebruiken een broker die is geïnstalleerd op het apparaat is **standaard uitgeschakeld**. Als u wilt gebruiken van uw toepassing met de broker moet u enkele aanvullende configuratie en code toevoegen aan uw toepassing.

Zijn de stappen te volgen:

1. De broker-modus in de aanroep van de code van uw toepassing met de SDK van MS inschakelen.
2. Tot stand brengen van een nieuwe omleidings-URI en bieden die zowel de app als uw app-registratie.
3. Registreren van een URL-schema.
4. Ondersteuning voor iOS9: een machtiging toevoegen aan uw info.plist-bestand.

#### <a name="step-1-enable-broker-mode-in-your-application"></a>Stap 1: Broker-modus inschakelen in uw toepassing

De mogelijkheid voor uw toepassing voor het gebruik van de broker is ingeschakeld wanneer u de 'context' of de eerste installatie van uw verificatieobject maakt. Dit doet u door het instellen van het type van uw referenties in uw code:

```
/*! See the ADCredentialsType enumeration definition for details */
@propertyADCredentialsType credentialsType;
```
De `AD_CREDENTIALS_AUTO` instelling zorgt ervoor dat de Microsoft Identity-SDK om te proberen om aan te roepen om naar de broker `AD_CREDENTIALS_EMBEDDED` wordt voorkomen dat de Microsoft Identity-SDK aanroepen naar de broker.

#### <a name="step-2-registering-a-url-scheme"></a>Stap 2: Een URL-schema registreren

Het Microsoft Identity-platform maakt gebruik van URL's voor het aanroepen van de broker en vervolgens terugkeren naar de toepassing. Als u wilt dat retour voltooien moet u een URL-schema voor uw toepassing die het Microsoft Identity-platform weet over geregistreerd. Dit kan zijn naast eventuele andere app-schema's die kan u eerder hebt geregistreerd met uw toepassing.

> [!WARNING]
> U wordt aangeraden het URL-schema vrij uniek is voor het minimaliseren van de kans op een andere app met behulp van de dezelfde URL-schema te maken. Apple worden niet afgedwongen door de uniekheid van URL-schema's die zijn geregistreerd in de appstore.
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

#### <a name="step-3-establish-a-new-redirect-uri-with-your-url-scheme"></a>Stap 3: Tot stand brengen van een nieuwe omleidings-URI met de URL-schema

Om ervoor te zorgen dat we de referentie-tokens altijd naar de juiste toepassing terugkeren, moeten we om te controleren of dat we aanroepen om uw toepassing op een manier die de iOS-besturingssysteem kunt controleren. De iOS-besturingssysteem rapporteert aan de broker-toepassingen van Microsoft de bundel-ID van de aanroepen van deze toepassing. Dit kan niet door een rogue-toepassing worden vervalst. Daarom gebruiken we dit samen met de URI van de broker-toepassing om ervoor te zorgen dat de tokens terug naar de juiste toepassing keert. We dat u voor het maken van deze unieke omleidings-URI, zowel in uw toepassing instellen als een omleidings-URI in de portal voor ontwikkelaars.

De omleidings-URI moet zich in de juiste vorm van:

`<app-scheme>://<your.bundle.id>`

bijvoorbeeld: *x-msauth-mytestiosapp://com.myapp.mytestapp*

Deze omleidings-URI moet worden opgegeven in uw app-registratie met de [Azure-portal](https://portal.azure.com/). Zie voor meer informatie over Azure AD-app-registratie [integreren met Azure Active Directory](active-directory-how-to-integrate.md).

##### <a name="step-3a-add-a-redirect-uri-in-your-app-and-dev-portal-to-support-certificate-based-authentication"></a>Stap 3a: Voeg een omleidings-URI in uw app en dev-portal voor de ondersteuning van verificatie op basis van certificaat

Ondersteuning voor certificaat-verificatie op basis van een tweede "msauth" moet worden geregistreerd in uw toepassing en de [Azure-portal](https://portal.azure.com/) voor het afhandelen van verificatie via certificaat als u wilt toevoegen die ondersteuning bieden in uw toepassing.

`msauth://code/<broker-redirect-uri-in-url-encoded-form>`

bijvoorbeeld: *msauth://code/x-msauth-mytestiosapp%3A%2F%2Fcom.myapp.mytestapp*

#### <a name="step-4-ios9-add-a-configuration-parameter-to-your-app"></a>Stap 4: iOS9: een configuratieparameter toevoegen aan uw app

– CanOpenURL maakt gebruik van ADAL: om te controleren of de broker op het apparaat is geïnstalleerd. In iOS vergrendeld 9 Apple wat schema's een toepassing kunnen opvragen. U moet 'msauth' toevoegen aan de sectie LSApplicationQueriesSchemes van uw `info.plist file`.

<key>LSApplicationQueriesSchemes</key> <array> <string>msauth</string></array>

### <a name="youve-configured-sso"></a>U kunt eenmalige aanmelding hebt geconfigureerd.

Nu wordt de Microsoft Identity-SDK automatisch zowel delen referenties voor uw toepassingen en aanroepen van de broker, indien aanwezig zijn op hun apparaat.