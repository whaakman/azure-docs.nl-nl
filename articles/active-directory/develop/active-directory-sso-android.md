---
title: Het inschakelen van eenmalige aanmelding voor cross-app voor Android met behulp van ADAL | Microsoft Docs
description: 'Het gebruik van de functies van de ADAL-SDK voor eenmalige aanmelding inschakelen tussen uw toepassingen. '
services: active-directory
documentationcenter: 
author: danieldobalian
manager: mtillman
editor: 
ms.assetid: 40710225-05ab-40a3-9aec-8b4e96b6b5e7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: android
ms.devlang: java
ms.topic: article
ms.date: 04/07/2017
ms.author: dadobali
ms.custom: aaddev
ms.openlocfilehash: 7d832ecf3e9c64088a75cc88551879b4e09df715
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-enable-cross-app-sso-on-android-using-adal"></a>Het inschakelen van eenmalige aanmelding voor cross-app voor Android met behulp van ADAL
Mits eenmalige aanmelding (SSO) zodat gebruikers alleen moeten eenmaal hun referenties invoeren en deze referenties automatisch laten samenwerken binnen wordt nu toepassingen door klanten verwacht. De problemen in hun gebruikersnaam en wachtwoord invoeren op een klein scherm, vaak keren gecombineerd met een extra factor (2FA), zoals een telefoongesprek of ge-code, resulteert in een snelle ergernis als een gebruiker heeft om dit te doen meer dan één keer voor het product.

Bovendien, als u een identiteitsplatform die van andere toepassingen zoals Microsoft-Accounts of een werkaccount van Office365 gebruikmaken mogelijk toepast, verwachten klanten dat de referenties die moeten zijn beschikbaar voor gebruik in hun toepassingen ongeacht de leverancier.

De Microsoft Identity-platform, samen met onze SDK's van Microsoft Identity doet dit harde werk voor u en biedt u de mogelijkheid om u te overleggen van uw klanten met eenmalige aanmelding zich binnen uw eigen reeks toepassingen of, net als bij onze broker-capaciteit en de verificator-toepassingen, via het hele apparaat.

In dit scenario wordt uitgelegd hoe u onze SDK in uw toepassing voordeel bieden aan uw klanten configureren.

In dit scenario is van toepassing op:

* Azure Active Directory
* Azure Active Directory B2C
* Azure Active Directory B2B
* Voorwaardelijke toegang voor Azure Active Directory

De voorgaande document wordt ervan uitgegaan dat u weet hoe [inrichten van toepassingen in de oude portal voor Azure Active Directory](active-directory-how-to-integrate.md) en uw toepassing met geïntegreerde de [Microsoft Identity Android SDK](https://github.com/AzureAD/azure-activedirectory-library-for-android).

## <a name="sso-concepts-in-the-microsoft-identity-platform"></a>SSO-concepten in het Identiteitsplatform van Microsoft
### <a name="microsoft-identity-brokers"></a>Microsoft Identity Beleggingsmakelaars
Microsoft biedt toepassingen voor elk mobiel platform voor het overbruggen van referenties voor toepassingen van verschillende leveranciers en kan voor speciale verbeterde functies waarvoor een enkele veilige plaats waar om referenties te valideren. We noemen deze **beleggingsmakelaars**. Op iOS en Android zijn deze opgenomen in downloadbare toepassingen dat klanten afzonderlijk installeren of door een bedrijf die enkele of alle van het apparaat voor hun werknemers beheert op het apparaat kunnen worden geactiveerd. Deze beleggingsmakelaars ondersteuning voor het beheer van beveiliging voor sommige toepassingen of het hele apparaat op basis van wat de IT-beheerders willen. In Windows, wordt deze functionaliteit verstrekt door een ingebouwd in het besturingssysteem en technisch als de Webauthenticatiebroker bekend kiezen.

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
| Azure SDK  | | Azure SDK  |  | Azure SDK   |
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
 
 De noodzaak om te controleren of de identiteit van een aanroep van de toepassing die de broker is van cruciaal belang voor de beveiliging we in de broker bieden ondersteunde aanmeldingen. Geen iOS of Android zorgt ervoor dat unieke id's die alleen geldig voor een bepaalde toepassing zijn zodat schadelijke toepassingen mogelijk '' een legitieme toepassings-id vervalsen en de tokens die zijn bedoeld voor de toepassing legitieme ontvangen. Om ervoor te zorgen dat we altijd communiceren met de juiste toepassing tijdens runtime, vraagt u de ontwikkelaar een aangepaste redirectURI bieden bij het registreren van de toepassing met Microsoft. **Hoe ontwikkelaars deze omleidings-URI moeten stellen wordt hieronder in detail besproken.** Deze aangepaste redirectURI de vingerafdruk van het certificaat van de toepassing bevat en uniek zijn voor de toepassing door de Google Play Store is gewaarborgd. Wanneer een toepassing de broker aanroept, vraagt de broker het besturingssysteem Android om te voorzien de vingerafdruk van het certificaat dat de broker genoemd. De broker biedt de vingerafdruk van dit certificaat naar Microsoft in de aanroep naar ons identiteitssysteem. Als de vingerafdruk van het certificaat van de toepassing komt niet overeen met de vingerafdruk van het certificaat aan ons door de ontwikkelaar tijdens de registratie wordt geleverd, wordt er toegang tot de tokens voor de resource van de toepassing aanvragen geweigerd. Deze controle zorgt ervoor dat alleen de toepassing die zijn geregistreerd door de ontwikkelaar tokens ontvangt.

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
|  ADAL SDK  | |  ADAL SDK  |   |  ADAL SDK   |
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
We gebruiken hier de ADAL-Android SDK:

* Geen broker assisted eenmalige aanmelding voor uw suite van apps inschakelen
* Ondersteuning voor broker-ondersteunde eenmalige aanmelding inschakelen

### <a name="turning-on-sso-for-non-broker-assisted-sso"></a>Eenmalige aanmelding inschakelen voor niet-broker ondersteund eenmalige aanmelding
Geen broker assisted SSO op toepassingen met beheren de Microsoft Identity-SDK's voor veel van de complexiteit van eenmalige aanmelding voor u. Dit omvat de juiste gebruiker zoeken in de cache en onderhouden van een lijst met aangemelde gebruikers om te zoeken.

Eenmalige aanmelding inschakelen voor toepassingen die u bezit, moet u het volgende doen:

1. Zorg ervoor dat alle gebruikers van uw toepassingen dezelfde Client-ID of id van toepassing.
2. Zorg ervoor dat al uw toepassingen hebben dezelfde set SharedUserID.
3. Zorg ervoor dat alle toepassingen dezelfde handtekeningcertificaat van de uit de Google Play store delen, zodat u opslag kunt delen.

#### <a name="step-1-using-the-same-client-id--application-id-for-all-the-applications-in-your-suite-of-apps"></a>Stap 1: Met behulp van dezelfde Client-ID / toepassing-ID voor alle toepassingen in uw suite van apps
In de volgorde voor het platform voor Microsoft Identity weten dat het delen van tokens via uw toepassingen is toegestaan, moet elk van uw toepassingen delen van hetzelfde Client-ID of id van toepassing. Dit is de unieke id die aan u is opgegeven bij de registratie van uw eerste toepassing in de portal.

U vraagt zich misschien af hoe u verschillende apps met de Microsoft Identity-service wordt geïdentificeerd als deze gebruikmaakt van de dezelfde toepassing-ID. Het antwoord is met de **omleidings-URI's**. Elke toepassing kan meerdere omleidings-URI's geregistreerd in de portal voor onboarding hebben. Elke app in de suite hebben een verschillende omleidings-URI. Een voorbeeld van hoe dit eruitziet lager is dan:

App1 omleidings-URI:`msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D`

App2 omleidings-URI:`msauth://com.example.userapp1/KmB7PxIytyLkbGHuI%2UitkW%2Fejk%4E`

App3 omleidings-URI:`msauth://com.example.userapp2/Pt85PxIyvbLkbKUtBI%2SitkW%2Fejk%9F`

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

#### <a name="step-2-configuring-shared-storage-in-android"></a>Stap 2: Gedeelde opslag configureren in Android
Instellen van de `SharedUserID` valt buiten het bereik van dit document, maar kan worden geleerd door de Google Android-documentatie lezen op de [Manifest](http://developer.android.com/guide/topics/manifest/manifest-element.html). Wat is het belangrijk is dat u besluit dat u wilt uw sharedUserID wordt aangeroepen en wordt deze gebruikt op al uw toepassingen.

Zodra u hebt de `SharedUserID` in al uw toepassingen bent u klaar voor gebruik van eenmalige aanmelding.

> [!WARNING]
> Wanneer u delen van opslag in uw toepassingen kunt elke toepassing gebruikers verwijderen of slechter verwijderen van alle tokens inlezen in uw toepassing. Dit is vooral rampzalig zijn als er toepassingen die afhankelijk zijn van de tokens werk op de achtergrond. Het delen van opslag, betekent dat u voorzichtig in bewerkingen voor alle verwijderen via de Microsoft Identity-SDK's moet zijn.
> 
> 

Dat is alles. De Microsoft Identity-SDK wordt nu referenties delen in al uw toepassingen. De lijst met gebruikers wordt ook gedeeld met exemplaren van een toepassing.

### <a name="turning-on-sso-for-broker-assisted-sso"></a>Eenmalige aanmelding inschakelen voor broker ondersteund eenmalige aanmelding
De mogelijkheid om een toepassing te gebruiken broker die is geïnstalleerd op het apparaat is **standaard uitgeschakeld**. Als u wilt gebruiken van uw toepassing met de broker moet u enkele aanvullende configuratiestappen en code toevoegen aan uw toepassing.

De stappen te volgen zijn:

1. Schakel de broker-modus in uw toepassingscode aanroep van de SDK MS
2. Stel een nieuwe omleidings-URI en bieden die zowel de app en uw app-registratie
3. Instellen van de juiste machtigingen in de Android-manifestbestand

#### <a name="step-1-enable-broker-mode-in-your-application"></a>Stap 1: Broker-modus inschakelen in uw toepassing
De mogelijkheid voor uw toepassing gebruik van de broker is ingeschakeld wanneer u de 'instellingen' of de eerste installatie van de verificatie-instantie maakt. U doen dit door het instellen van uw type ApplicationSettings in uw code:

```
AuthenticationSettings.Instance.setUseBroker(true);
```


#### <a name="step-2-establish-a-new-redirect-uri-with-your-url-scheme"></a>Stap 2: Een nieuwe omleidings-URI met URL-schema maken
Om ervoor te zorgen dat we de referentie-tokens altijd naar de juiste toepassing terugkeren, moeten we controleren of dat we noemen terug naar de toepassing op een manier die het besturingssysteem Android kunt controleren. Het besturingssysteem Android maakt gebruik van de hash van het certificaat in de Google Play store. Dit kan niet door een rogue-toepassing worden vervalst. Daarom we maken gebruik van dit samen met de URI van onze toepassing broker om ervoor te zorgen dat de tokens worden geretourneerd naar de juiste toepassing. Moet u voor het maken van deze unieke omleidings-URI voor beide in uw toepassing instellen als een omleidings-URI in onze portal voor ontwikkelaars.

De omleidings-URI moet de juiste indeling zijn:

`msauth://packagename/Base64UrlencodedSignature`

Voorbeeld: *msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D*

Deze omleidings-URI moet worden opgegeven in uw app registreren met de [Azure-portal](https://portal.azure.com/). Zie voor meer informatie over Azure AD-app-registratie [integreren met Azure Active Directory](active-directory-how-to-integrate.md).

#### <a name="step-3-set-up-the-correct-permissions-in-your-application"></a>Stap 3: De juiste machtigingen in uw toepassing instellen
Onze toepassing broker in Android gebruikt de Accounts Manager-functie van het besturingssysteem Android voor het beheren van referenties in toepassingen. Om te gebruiken van de broker in Android moet uw app-manifest machtigingen hebben voor AccountManager accounts gebruiken. Dit wordt uitgebreid beschreven in de [Google documentatie voor Account Manager hier](http://developer.android.com/reference/android/accounts/AccountManager.html)

In het bijzonder zijn deze machtigingen:

```
GET_ACCOUNTS
USE_CREDENTIALS
MANAGE_ACCOUNTS
```

### <a name="youve-configured-sso"></a>U kunt eenmalige aanmelding hebt geconfigureerd.
Nu wordt de Microsoft Identity-SDK automatisch zowel referenties delen in uw toepassingen en aanroepen van de broker, indien aanwezig zijn op hun apparaat.

