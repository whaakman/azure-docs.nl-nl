---
title: Het inschakelen van eenmalige aanmelding voor verschillende Apps op Android met behulp van ADAL | Microsoft Docs
description: 'Het gebruik van de functies van de ADAL-SDK voor eenmalige aanmelding inschakelen voor uw toepassingen. '
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 40710225-05ab-40a3-9aec-8b4e96b6b5e7
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: android
ms.devlang: java
ms.topic: article
ms.date: 06/13/2018
ms.author: celested
ms.reviewer: dadobali
ms.custom: aaddev
ms.openlocfilehash: 4abf6bd2d82753e22d4fde92e219109274ce36be
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39602016"
---
# <a name="how-to-enable-cross-app-sso-on-android-using-adal"></a>Het inschakelen van eenmalige aanmelding voor verschillende Apps op Android met behulp van ADAL
Eenmalige aanmelding (SSO) bieden, zodat gebruikers slechts hoeft eenmaal zijn referenties invoeren en deze referenties automatisch werkt voor toepassingen is nu een industriestandaard. De problemen in hun gebruikersnaam en wachtwoord invoeren op een klein scherm, vaak tijden in combinatie met een extra factor (2FA), zoals een telefoongesprek of ge-sms't-code, resulteert in een ontevredenheid als een gebruiker heeft aan meer dan één keer voor aanmelding.

Bovendien, als u een identity-platform die van andere toepassingen zoals Microsoft-Accounts of werk uit Microsoft365 gebruikmaken mogelijk toepast, verwachten klanten dat de referenties voor het beschikbaar zijn voor gebruik in hun toepassingen, ongeacht de uitgever.

De Microsoft Identity-platform, samen met de SDK's van Microsoft Identity biedt u de mogelijkheid om te Verras uw klanten met eenmalige aanmelding zich binnen uw eigen reeks toepassingen of, net als bij de broker-functie en de Authenticator-toepassingen, over de hele het apparaat.

In dit scenario wordt uitgelegd hoe u het configureren van de SDK in uw toepassing voor eenmalige aanmelding voor uw klanten.

De voorgaande document wordt ervan uitgegaan dat u weet hoe u aan het integreren van uw toepassing met de [Microsoft Identity Android SDK](https://github.com/AzureAD/azure-activedirectory-library-for-android).

## <a name="sso-concepts-in-the-microsoft-identity-platform"></a>Concepten van eenmalige aanmelding in het Microsoft Identity-Platform
### <a name="microsoft-identity-brokers"></a>Microsoft Identity Brokers
Microsoft biedt toepassingen voor elk mobiel platform voor het overbruggen van referenties voor toepassingen van verschillende leveranciers en kan voor speciale verbeterde functies waarvoor een enkele veilige plaats waar om referenties te valideren. De SDK-aanroepen van deze **brokers**. Op iOS en Android, de brokers geleverd via downloadbare toepassingen dat klanten onafhankelijk installeren of op het apparaat kunnen worden geactiveerd door een bedrijf dat sommige of alle van het apparaat voor hun werknemers beheert. Deze brokers ondersteuning voor beheer van beveiliging voor sommige toepassingen of het hele apparaat op basis van wat de IT-beheerders kunnen vereisen. In Windows, wordt deze functionaliteit verstrekt door een accountkiezer die is ingebouwd in het besturingssysteem, ook wel technisch gezien als de Webauthenticatiebroker.

#### <a name="broker-assisted-logins"></a>Assisted aanmeldingen Broker
Ondersteuning van de Broker-aanmelding zijn aanmelding ervaringen die optreden in de broker-toepassing en de opslag en de beveiliging van de broker gebruiken voor het delen van referenties voor alle toepassingen op het apparaat die van toepassing het Microsoft Identity-platform zijn. Het gevolg hiervan wordt van uw toepassingen zijn afhankelijk van de broker gebruikers zich aanmelden. Op iOS en Android, worden deze brokers geleverd via downloadbare toepassingen dat klanten onafhankelijk installeren of op het apparaat kunnen worden geactiveerd door een bedrijf dat het apparaat voor de gebruiker wordt beheerd. Een voorbeeld van dit type toepassing is de Microsoft Authenticator-toepassing op iOS. In Windows, wordt deze functionaliteit verstrekt door een accountkiezer die is ingebouwd in het besturingssysteem, ook wel technisch gezien als de Webauthenticatiebroker.
De ervaring verschilt per platform en kan soms verstorend voor gebruikers zijn als het niet correct worden beheerd. U bent waarschijnlijk meest bekend zijn met dit patroon als u de Facebook-toepassing geïnstalleerd en Facebook verbinding maken met gebruik van een andere toepassing. Het Microsoft Identity-platform gebruikt hetzelfde patroon.

Op Android, wordt de accountkiezer boven op uw toepassing, dit minder verstorend voor de gebruiker is weergegeven.

#### <a name="how-the-broker-gets-invoked"></a>Hoe de broker wordt aangeroepen
Als een compatibel broker is geïnstalleerd op het apparaat, zoals de Microsoft Authenticator-toepassing, doen de Microsoft Identity-SDK's automatisch het werk van het aanroepen van de broker voor u wanneer een gebruiker geeft aan dat ze willen aanmelden met een van de Microsoft-account Identity-platform. 
 
 #### <a name="how-microsoft-ensures-the-application-is-valid"></a>Hoe zorgt Microsoft ervoor dat de toepassing is geldig
 
 De noodzaak om te controleren of de identiteit van een aanroep van de toepassing de broker is van cruciaal belang voor de beveiliging die is opgegeven in de ondersteuning van de broker-aanmeldingen. iOS en Android niet afgedwongen door unieke id's die alleen geldig voor een bepaalde toepassing zijn zodat schadelijke toepassingen mogelijk '"een legitieme toepassings-id vervalsen en de tokens die zijn bedoeld voor de legitieme aanvraag ontvangen. Om ervoor te zorgen dat Microsoft altijd communiceert met de juiste toepassing tijdens runtime, de ontwikkelaar gevraagd voor een aangepaste redirectURI bij het registreren van hun toepassing met Microsoft. **Hoe ontwikkelaars deze omleidings-URI moeten stellen wordt besproken in de hieronder beschreven.** Deze aangepaste redirectURI bevat de vingerafdruk van het certificaat van de toepassing en wordt ervoor gezorgd dat uniek zijn voor de toepassing door de Google Play Store. Wanneer een toepassing de broker aanroept, gevraagd de broker de Android-besturingssysteem om te voorzien de vingerafdruk van het certificaat dat de broker genoemd. De broker biedt de vingerafdruk van dit certificaat naar Microsoft in de aanroep naar de identiteitssysteem. Als de vingerafdruk van het certificaat van de toepassing komt niet overeen met de vingerafdruk van het certificaat die aan ons wordt verstrekt door de ontwikkelaar tijdens de registratie, wordt toegang is geweigerd tot de tokens voor de resource die de toepassing aanvraagt. Deze controle zorgt ervoor dat alleen de door de ontwikkelaar van de geregistreerde toepassing tokens ontvangen.

Brokered-SSO-aanmeldingen hebben de volgende voordelen:

* Gebruikerservaringen SSO voor al hun toepassingen, ongeacht de leverancier.
* Uw toepassing kunt gebruiken meer geavanceerde functies zoals voorwaardelijke toegang en ondersteuning voor Intune-scenario's.
* Uw toepassing kan ondersteuning voor verificatie op basis van certificaten voor zakelijke gebruikers.
* Ervaring voor beter beveiligd aanmelden als de identiteit van de toepassing en de gebruiker worden geverifieerd door de broker-toepassing met algoritmen voor extra beveiliging en versleuteling.

Dit is een weergave van de werking van de Microsoft Identity-SDK's met de broker-toepassingen voor eenmalige aanmelding inschakelen:

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

Met deze achtergrondinformatie die u moet kunnen beter te begrijpen en implementeren van eenmalige aanmelding in uw toepassing met behulp van de Microsoft Identity-platform en SDK's hebt.

### <a name="turning-on-sso-for-broker-assisted-sso"></a>Eenmalige aanmelding telefonische inschakelen van eenmalige aanmelding voor broker
De mogelijkheid om een toepassing te gebruiken een broker die is geïnstalleerd op het apparaat is **standaard uitgeschakeld**. Als u wilt uw toepassing met de broker gebruiken, moet u enkele aanvullende configuratie en code toevoegen aan uw toepassing.

Zijn de stappen te volgen:

1. Schakel de broker-modus in uw toepassingscode aanroepen met de SDK van MS
2. Tot stand brengen van een nieuwe omleidings-URI en bieden die zowel de app als uw app-registratie
3. Instellen van de juiste machtigingen in het Android-manifest

#### <a name="step-1-enable-broker-mode-in-your-application"></a>Stap 1: Broker-modus inschakelen in uw toepassing
De mogelijkheid voor uw toepassing voor het gebruik van de broker is ingeschakeld wanneer u de 'instellingen' of de eerste installatie van de verificatie-exemplaar maakt. Om dit te doen in uw app:

```
AuthenticationSettings.Instance.setUseBroker(true);
```

#### <a name="step-2-establish-a-new-redirect-uri-with-your-url-scheme"></a>Stap 2: Tot stand brengen van een nieuwe omleidings-URI met de URL-schema
Om ervoor te zorgen dat de juiste toepassing recevies de geretourneerde de referentie op die tokens, er is een nodig om te controleren of de aanroep naar de toepassing op een manier die de Android-besturingssysteem kunt controleren. Het besturingssysteem Android maakt gebruik van de hash van het certificaat in de Google Play store. Deze hash van het certificaat kan niet door een rogue-toepassing worden vervalst. Samen met de URI van de broker-toepassing, zorgt Microsoft ervoor dat de tokens terug naar de juiste toepassing keert. Een unieke redirect URI is vereist om te worden geregistreerd op de toepassing.

De omleidings-URI moet zich in de juiste vorm van:

`msauth://packagename/Base64UrlencodedSignature`

bijvoorbeeld: *msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D*

U kunt deze omleidings-URI registreren in uw app-registratie met de [Azure-portal](https://portal.azure.com/). Zie voor meer informatie over Azure AD-app-registratie [integreren met Azure Active Directory](active-directory-how-to-integrate.md).

#### <a name="step-3-set-up-the-correct-permissions-in-your-application"></a>Stap 3: De juiste machtigingen in uw toepassing instellen
De broker-toepassing in Android maakt gebruik van de Accounts Manager-functie van de Android-besturingssysteem voor het beheer van referenties voor toepassingen. Als u wilt gebruiken van de broker in Android moet uw app-manifest machtigingen hebben voor AccountManager accounts gebruiken. Deze machtigingen worden besproken in de [hier Google documentatie voor Account Manager](http://developer.android.com/reference/android/accounts/AccountManager.html)

In het bijzonder zijn deze machtigingen:

```
GET_ACCOUNTS
USE_CREDENTIALS
MANAGE_ACCOUNTS
```

### <a name="youve-configured-sso"></a>U kunt eenmalige aanmelding hebt geconfigureerd.
Nu wordt de Microsoft Identity-SDK automatisch zowel delen referenties voor uw toepassingen en aanroepen van de broker, indien aanwezig zijn op hun apparaat.

