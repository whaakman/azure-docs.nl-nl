---
title: Het inschakelen van eenmalige aanmelding voor verschillende Apps op Android met behulp van ADAL | Microsoft Docs
description: Het gebruik van de functies van de ADAL-SDK voor eenmalige aanmelding inschakelen voor uw toepassingen.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 40710225-05ab-40a3-9aec-8b4e96b6b5e7
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: android
ms.devlang: java
ms.topic: article
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: dadobali
ms.custom: aaddev
ms.openlocfilehash: 9f0cc19ae220d27de620e5bd347fe78d9bfab2d5
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55093288"
---
# <a name="how-to-enable-cross-app-sso-on-android-using-adal"></a>Procedure: SSO voor cross-app op Android met behulp van ADAL inschakelen

[!INCLUDE [active-directory-develop-applies-v1-adal](../../../includes/active-directory-develop-applies-v1-adal.md)]

Eenmalige aanmelding (SSO) kan gebruikers slechts één keer zijn referenties invoeren en deze referenties automatisch werken tussen toepassingen en -platforms die andere toepassingen (zoals Microsoft-Accounts of een werkaccount van Microsoft 365 gebruiken kunnen) hebben geen de uitgever van belang.

Van Microsoft identity-platform, samen met de SDK's, kunt eenvoudig eenmalige aanmelding binnen uw eigen suite van apps, of met de broker-functie en de verificator-toepassingen, voor het hele apparaat inschakelen.

In deze procedures leert u over het configureren van de SDK in uw toepassing voor eenmalige aanmelding voor uw klanten.

## <a name="prerequisites"></a>Vereisten

Deze instructies wordt ervan uitgegaan dat u weet hoe u:

- Uw app met behulp van de oude portal voor Azure Active Directory (Azure AD) inrichten. Zie voor meer informatie, [een app registreren bij de Azure AD v1.0-eindpunt](quickstart-v1-add-azure-ad-app.md)
- Integreer uw toepassing met de [Azure AD Android SDK](https://github.com/AzureAD/azure-activedirectory-library-for-android).

## <a name="single-sign-on-concepts"></a>Concepten voor eenmalige aanmelding

### <a name="identity-brokers"></a>Identity brokers

Microsoft biedt toepassingen voor elk mobiel platform waarmee voor het overbruggen van referenties voor toepassingen van verschillende leveranciers en verbeterde functies waarvoor een enkele veilige plaats waar om referenties te valideren. Deze heten **brokers**.

Op iOS en Android, brokers geleverd via downloadbare toepassingen dat klanten onafhankelijk installeren of naar het apparaat gepusht door een bedrijf die beheert sommige of alle van de apparaten van hun werknemers. Brokers ondersteuning voor beheer van beveiliging voor sommige toepassingen of het hele apparaat op basis van de configuratie van de IT-beheerder. In Windows, wordt deze functionaliteit verstrekt door een accountkiezer die is ingebouwd in het besturingssysteem, ook wel technisch gezien als de Webauthenticatiebroker.

#### <a name="broker-assisted-login"></a>Assisted Broker-aanmelding

Ondersteuning van de Broker-aanmelding zijn aanmelding ervaringen die optreden in de broker-toepassing en de opslag en de beveiliging van de broker gebruiken voor het delen van referenties voor alle toepassingen op het apparaat die van toepassing het identity-platform zijn. Het gevolg hiervan wordt van uw toepassingen zijn afhankelijk van de broker gebruikers zich aanmelden. Op iOS en Android, worden deze brokers geleverd via downloadbare toepassingen dat klanten onafhankelijk installeren of op het apparaat kunnen worden geactiveerd door een bedrijf dat het apparaat voor de gebruiker wordt beheerd. Een voorbeeld van dit type toepassing is de Microsoft Authenticator-toepassing op iOS. In Windows, wordt deze functionaliteit verstrekt door een accountkiezer die is ingebouwd in het besturingssysteem, ook wel technisch gezien als de Webauthenticatiebroker.
De ervaring verschilt per platform en kan soms verstorend voor gebruikers zijn als het niet correct worden beheerd. U bent waarschijnlijk meest bekend zijn met dit patroon als u de Facebook-toepassing geïnstalleerd en Facebook verbinding maken met gebruik van een andere toepassing. Het identity-platform gebruikt hetzelfde patroon.

Op Android, wordt de accountkiezer boven op uw toepassing, dit minder verstorend voor de gebruiker is weergegeven.

#### <a name="how-the-broker-gets-invoked"></a>Hoe de broker wordt aangeroepen

Als een compatibel broker is geïnstalleerd op het apparaat, zoals de Microsoft Authenticator-toepassing, de identiteit van de SDK's wordt automatisch doen het werk van het aanroepen van de broker voor u wanneer een gebruiker geeft aan dat ze willen aanmelden met een account van het identity-platform.

#### <a name="how-microsoft-ensures-the-application-is-valid"></a>Hoe zorgt Microsoft ervoor dat de toepassing is geldig

De noodzaak om te controleren of de identiteit van een aanroep van de toepassing de broker is van cruciaal belang voor de beveiliging die is opgegeven in de ondersteuning van de broker-aanmeldingen. iOS en Android niet afgedwongen door unieke id's die alleen geldig voor een bepaalde toepassing zijn zodat schadelijke toepassingen mogelijk '"een legitieme toepassings-id vervalsen en de tokens die zijn bedoeld voor de legitieme aanvraag ontvangen. Om ervoor te zorgen dat Microsoft altijd communiceert met de juiste toepassing tijdens runtime, de ontwikkelaar gevraagd voor een aangepaste redirectURI bij het registreren van hun toepassing met Microsoft. **Hoe ontwikkelaars deze omleidings-URI moeten stellen wordt besproken in de hieronder beschreven.** Deze aangepaste redirectURI bevat de vingerafdruk van het certificaat van de toepassing en wordt ervoor gezorgd dat uniek zijn voor de toepassing door de Google Play Store. Wanneer een toepassing de broker aanroept, gevraagd de broker de Android-besturingssysteem om te voorzien de vingerafdruk van het certificaat dat de broker genoemd. De broker biedt de vingerafdruk van dit certificaat naar Microsoft in de aanroep naar de identiteitssysteem. Als de vingerafdruk van het certificaat van de toepassing komt niet overeen met de vingerafdruk van het certificaat die aan ons wordt verstrekt door de ontwikkelaar tijdens de registratie, wordt toegang is geweigerd tot de tokens voor de resource die de toepassing aanvraagt. Deze controle zorgt ervoor dat alleen de door de ontwikkelaar van de geregistreerde toepassing tokens ontvangen.

Brokered-SSO-aanmeldingen hebben de volgende voordelen:

* Gebruikerservaringen SSO voor al hun toepassingen, ongeacht de leverancier.
* Uw toepassing kunt gebruiken meer geavanceerde functies zoals voorwaardelijke toegang en ondersteuning voor Intune-scenario's.
* Uw toepassing kan ondersteuning voor verificatie op basis van certificaten voor zakelijke gebruikers.
* Ervaring voor beter beveiligd aanmelden als de identiteit van de toepassing en de gebruiker worden geverifieerd door de broker-toepassing met algoritmen voor extra beveiliging en versleuteling.

Dit is een weergave van de werking van de SDK's met de broker-toepassingen voor eenmalige aanmelding inschakelen:

```
+------------+ +------------+   +-------------+
|            | |            |   |             |
|   App 1    | |   App 2    |   |   Someone   |
|            | |            |   |    else's   |
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

### <a name="turning-on-sso-for-broker-assisted-sso"></a>Eenmalige aanmelding telefonische inschakelen van eenmalige aanmelding voor broker

De mogelijkheid om een toepassing te gebruiken een broker die is geïnstalleerd op het apparaat is standaard uitgeschakeld. Als u wilt uw toepassing met de broker gebruiken, moet u enkele aanvullende configuratie en code toevoegen aan uw toepassing.

Zijn de stappen te volgen:

1. Schakel de broker-modus in uw toepassingscode aanroepen met de SDK van MS
2. Tot stand brengen van een nieuwe omleidings-URI en bieden die zowel de app als uw app-registratie
3. Instellen van de juiste machtigingen in het Android-manifest

#### <a name="step-1-enable-broker-mode-in-your-application"></a>Stap 1: Schakel de broker-modus in uw toepassing

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

De broker-toepassing in Android maakt gebruik van de Accounts Manager-functie van de Android-besturingssysteem voor het beheer van referenties voor toepassingen. Als u wilt gebruiken van de broker in Android moet uw app-manifest machtigingen hebben voor AccountManager accounts gebruiken. Deze machtigingen worden besproken in de [hier Google documentatie voor Account Manager](https://developer.android.com/reference/android/accounts/AccountManager.html)

In het bijzonder zijn deze machtigingen:

```
GET_ACCOUNTS
USE_CREDENTIALS
MANAGE_ACCOUNTS
```

### <a name="youve-configured-sso"></a>U kunt eenmalige aanmelding hebt geconfigureerd.

Nu de identiteit van de SDK automatisch worden zowel het delen van referenties voor uw toepassingen en aanroepen van de broker, indien aanwezig zijn op hun apparaat.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [eenmalige aanmelding in de SAML-protocol](single-sign-on-saml-protocol.md)
