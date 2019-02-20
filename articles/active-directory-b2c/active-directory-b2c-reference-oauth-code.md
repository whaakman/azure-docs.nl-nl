---
title: Autorisatiecodestroom - Azure Active Directory B2C | Microsoft Docs
description: Meer informatie over het bouwen van web-apps met behulp van Azure AD B2C en OpenID Connect-verificatieprotocol.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 4ee67f07965036a71151d7b6a5092b9a76d94999
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56428683"
---
# <a name="oauth-20-authorization-code-flow-in-azure-active-directory-b2c"></a>OAuth 2.0-autorisatiecodestroom in Azure Active Directory B2C

U kunt de OAuth 2.0-autorisatiecode verlenen in apps die zijn geïnstalleerd op een apparaat toegang te krijgen tot beveiligde bronnen, zoals web-API's. Met behulp van de Azure Active Directory B2C (Azure AD B2C)-implementatie van OAuth 2.0, kunt u registratie, aanmelding toevoegen en andere identiteitsbeheer taken naar uw mobiele en bureaublad-apps. In dit artikel is taalonafhankelijk. In het artikel wordt beschreven hoe u berichten verzenden en ontvangen HTTP zonder gebruik van een open source-bibliotheken.

De OAuth 2.0-autorisatiecodestroom wordt beschreven in [sectie 4.1 van de OAuth 2.0-specificatie](https://tools.ietf.org/html/rfc6749). U kunt deze gebruiken voor verificatie en autorisatie in de meeste [toepassingstypen](active-directory-b2c-apps.md), met inbegrip van webtoepassingen en systeemeigen geïnstalleerde toepassingen. U kunt de OAuth 2.0-autorisatiecodestroom veilig toegangstokens verkrijgen en vernieuwen van tokens voor uw toepassingen, die kunnen worden gebruikt voor toegang tot resources die worden beveiligd door een [autorisatieserver](active-directory-b2c-reference-protocols.md).  Het vernieuwingstoken dat kan de client en nieuwe toegang verkrijgen (vernieuwen) tokens wanneer het toegangstoken is verlopen, doorgaans na een uur.

In dit artikel is gericht op de **openbare clients** OAuth 2.0-autorisatiecodestroom. Een openbare client is elke clienttoepassing die niet kan vertrouwd worden voor de integriteit van een wachtwoord voor het toepassingsgeheim veilig te houden. Dit omvat mobiele apps, bureaubladtoepassingen en in wezen elke toepassing die wordt uitgevoerd op een apparaat en moet toegangstokens te verkrijgen. 

> [!NOTE]
> Identiteitsbeheer toevoegen aan een web-app met behulp van Azure AD B2C, gebruikt u [OpenID Connect](active-directory-b2c-reference-oidc.md) in plaats van OAuth 2.0.

Azure AD B2C breidt de standaard die OAuth 2.0 stromen hiervoor meer dan een eenvoudige verificatie en autorisatie. Het geeft de [gebruiker stroom parameter](active-directory-b2c-reference-policies.md). Met gebruikersstromen, kunt u OAuth 2.0 gebruiken om toe te voegen van gebruikerservaringen voor uw toepassing, zoals registratie, aanmelding en Profielbeheer. Id-providers die gebruikmaken van de OAuth 2.0-protocol zijn [Amazon](active-directory-b2c-setup-amzn-app.md), [Azure Active Directory](active-directory-b2c-setup-oidc-azure-active-directory.md), [Facebook](active-directory-b2c-setup-fb-app.md), [GitHub](active-directory-b2c-setup-github-app.md), [ Google](active-directory-b2c-setup-goog-app.md), en [LinkedIn](active-directory-b2c-setup-li-app.md).

In het voorbeeld van de HTTP-aanvragen in dit artikel gebruiken we ons voorbeeld van Azure AD B2C-directory **fabrikamb2c.onmicrosoft.com**. We gebruiken ook onze voorbeeld-toepassingen en -stromen. U kunt ook zelf de aanvragen proberen met behulp van deze waarden, of kunt u deze vervangen door uw eigen waarden.
Meer informatie over het [ophalen van uw eigen Azure AD B2C-directory-, toepassings- en gebruiker stromen](#use-your-own-azure-ad-b2c-directory).

## <a name="1-get-an-authorization-code"></a>1. Een autorisatiecode ophalen
De autorisatiecodestroom begint met de client zodat de gebruiker de `/authorize` eindpunt. Dit is de interactieve deel van de stroom, waarbij de gebruiker actie onderneemt. In deze aanvraag, de client geeft aan dat de `scope` parameter de machtigingen die nodig is om te verkrijgen van de gebruiker. In de `p` parameter, dit geeft aan dat de gebruikersstroom om uit te voeren. De volgende drie voorbeelden (met regeleinden voor de leesbaarheid) elke gebruikmaken van een andere gebruiker-stroom.

### <a name="use-a-sign-in-user-flow"></a>Een gebruikersstroom aanmelden gebruiken
```
GET https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_sign_in
```

### <a name="use-a-sign-up-user-flow"></a>Gebruik een proefaccount gebruikersstroom
```
GET https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_sign_up
```

### <a name="use-an-edit-profile-user-flow"></a>Gebruik een gebruikersstroom profiel bewerken
```
GET https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_edit_profile
```

| Parameter | Vereist? | Description |
| --- | --- | --- |
| client_id |Vereist |De toepassings-ID die is toegewezen aan uw app in de [Azure-portal](https://portal.azure.com). |
| response_type |Vereist |Het reactietype, waaronder moet `code` voor de autorisatiecodestroom. |
| redirect_uri |Vereist |De omleidings-URI van uw app, waarbij verificatiereacties worden verzonden en ontvangen door uw app. Het moet een van de omleidings-URI's die u hebt geregistreerd in de portal exact overeenkomen, behalve dat het moet URL gecodeerd. |
| scope |Vereist |Een door spaties gescheiden lijst met bereiken. Een waarde één scope geeft u aan Azure Active Directory (Azure AD) beide van de machtigingen die worden aangevraagd. Met behulp van de client-ID als het bereik geeft aan dat uw app moet een toegangstoken dat kan worden gebruikt voor uw eigen service of web-API, vertegenwoordigd door de dezelfde client-ID.  De `offline_access` bereik geeft aan dat uw app een vernieuwingstoken voor lange levensduur hebben toegang tot bronnen moet. Ook kunt u de `openid` bereik om aan te vragen van een ID-token van Azure AD B2C. |
| response_mode |Aanbevolen |De methode die u gebruikt voor het verzenden van de resulterende autorisatiecode terug naar de app. Kan het zijn `query`, `form_post`, of `fragment`. |
| state |Aanbevolen |Een waarde die is opgenomen in de aanvraag die kan bestaan uit een tekenreeks van de inhoud die u wilt gebruiken. Normaal gesproken wordt een willekeurig gegenereerde unieke waarde gebruikt voor cross-site-aanvraag kunnen worden vervalst aanvallen te voorkomen. De status wordt ook gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat de verificatieaanvraag heeft plaatsgevonden. Bijvoorbeeld, de pagina die de gebruiker was op of de gebruikersstroom die werd uitgevoerd. |
| p |Vereist |De gebruikersstroom die wordt uitgevoerd. Dit is de naam van een beleid dat is gemaakt in uw Azure AD B2C-directory. De waarde voor de stroom gebruikersnaam moet beginnen met **b2c\_1\_**. Zie voor meer informatie over gebruikersstromen [Azure AD B2C-gebruikersstromen](active-directory-b2c-reference-policies.md). |
| prompt |Optioneel |Het type van de interactie van de gebruiker die is vereist. Op dit moment de enige geldige waarde is `login`, waardoor de gebruiker zijn referenties invoeren voor deze aanvraag. Eenmalige aanmelding wordt pas van kracht. |

Op dit moment wordt de gebruiker gevraagd om de werkstroom van de gebruikersstroom te voltooien. Dit kan betrekking hebben op de voeren hun gebruikersnaam en wachtwoord, gebruiker zich aanmeldt met een sociale ID aanmelden voor de map of een andere aantal stappen. Acties van de gebruiker, is afhankelijk van hoe de gebruikersstroom wordt gedefinieerd.

Nadat de gebruiker is voltooid de gebruikersstroom, Azure AD retourneert een antwoord aan uw app op de waarde die u hebt gebruikt voor `redirect_uri`. Gebruikt de methode die is opgegeven in de `response_mode` parameter. Het antwoord is precies hetzelfde voor elk van de gebruiker actie scenario's, onafhankelijk van de gebruikersstroom die is uitgevoerd.

Een geslaagde respons die gebruikmaakt van `response_mode=query` ziet er als volgt:

```
GET urn:ietf:wg:oauth:2.0:oob?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...        // the authorization_code, truncated
&state=arbitrary_data_you_can_receive_in_the_response                // the value provided in the request
```

| Parameter | Description |
| --- | --- |
| code |De autorisatiecode die de app heeft aangevraagd. De app kan de autorisatiecode gebruiken om aan te vragen van een toegangstoken voor een doelbron. Er worden autorisatiecodes zeer eenvoudige. Normaal gesproken verloopt deze na ongeveer 10 minuten. |
| state |Zie de volledige beschrijving van de tabel in de voorgaande sectie. Als een `state` parameter is opgenomen in de aanvraag, dezelfde waarde moet worden weergegeven in het antwoord. De app moet controleren of de `state` waarden in de aanvraag en respons identiek zijn. |

Foutberichten kunnen ook worden verzonden naar de omleidings-URI zodat de app deze op de juiste wijze kan verwerken:

```
GET urn:ietf:wg:oauth:2.0:oob?
error=access_denied
&error_description=The+user+has+cancelled+entering+self-asserted+information
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parameter | Description |
| --- | --- |
| error |Een tekenreeks voor de foutcode die u gebruiken kunt voor het classificeren van de typen fouten die optreden. U kunt ook de tekenreeks gebruiken om te reageren op fouten. |
| error_description |Een bericht specifieke fout die u kan helpen de hoofdoorzaak van een verificatiefout identificeren. |
| state |Zie de beschrijving van het volledige in de voorgaande tabel. Als een `state` parameter is opgenomen in de aanvraag, dezelfde waarde moet worden weergegeven in het antwoord. De app moet controleren of de `state` waarden in de aanvraag en respons identiek zijn. |

## <a name="2-get-a-token"></a>2. Een token verkrijgen
Nu dat u hebt een autorisatiecode hebt verkregen, kun je de `code` voor een token dat de beoogde resource door te sturen van een POST-aanvraag naar de `/token` eindpunt. In Azure AD B2C is de enige resource die u kunt aanvragen een token voor uw web-API van de back-end in de app. De overeenkomst die wordt gebruikt voor het aanvragen van een token aan uzelf is het gebruik van client-ID van uw app als het bereik:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://fabrikamb2c.b2clogin.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob

```

| Parameter | Vereist? | Description |
| --- | --- | --- |
| p |Vereist |De gebruikersstroom die is gebruikt voor het verkrijgen van de autorisatiecode. U kunt een andere gebruikersstroom niet gebruiken in deze aanvraag. Houd er rekening mee dat u deze parameter toevoegen de *querytekenreeks*, dus niet in de hoofdtekst van het bericht. |
| client_id |Vereist |De toepassings-ID die is toegewezen aan uw app in de [Azure-portal](https://portal.azure.com). |
| grant_type |Vereist |Het type verlenen. Voor de autorisatiecodestroom het machtigingstype moet `authorization_code`. |
| scope |Aanbevolen |Een door spaties gescheiden lijst met bereiken. Een waarde één scope geeft u aan Azure AD zowel de machtigingen die worden aangevraagd. Met behulp van de client-ID als het bereik geeft aan dat uw app moet een toegangstoken dat kan worden gebruikt voor uw eigen service of web-API, vertegenwoordigd door de dezelfde client-ID.  De `offline_access` bereik geeft aan dat uw app een vernieuwingstoken voor lange levensduur hebben toegang tot bronnen moet.  Ook kunt u de `openid` bereik om aan te vragen van een ID-token van Azure AD B2C. |
| code |Vereist |De autorisatiecode die u hebt verkregen in de eerste zijde van de stroom. |
| redirect_uri |Vereist |De omleidings-URI van de toepassing waar u de autorisatiecode ontvangen. |

Een geslaagde respons token ziet er als volgt:

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| Parameter | Description |
| --- | --- |
| not_before |De tijd waarop het token wordt beschouwd als geldig is, in epoche-tijd. |
| token_type |De waarde van het type token. Het enige type die ondersteuning biedt voor Azure AD is Bearer. |
| access_token |De ondertekende JSON Web Token (JWT) dat u hebt aangevraagd. |
| scope |De bereiken die het token geldig is. U kunt ook bereiken die in de cache-tokens gebruiken voor later gebruik. |
| expires_in |De hoeveelheid tijd die het token geldig is (in seconden). |
| refresh_token |Een vernieuwingstoken OAuth 2.0. De app kunt dit token gebruiken om aanvullende tokens verkrijgen nadat het huidige token is verlopen. Vernieuwen van tokens worden lange levensduur hebben. U kunt ze gebruiken voor toegang tot resources behouden gedurende langere tijd wordt opgelost. Zie voor meer informatie de [naslaginformatie over Azure AD B2C-tokens](active-directory-b2c-reference-tokens.md). |

Foutberichten er als volgt:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parameter | Description |
| --- | --- |
| error |Een tekenreeks voor de foutcode die u gebruiken kunt voor het classificeren van de typen fouten die optreden. U kunt ook de tekenreeks gebruiken om te reageren op fouten. |
| error_description |Een bericht specifieke fout die u kan helpen de hoofdoorzaak van een verificatiefout identificeren. |

## <a name="3-use-the-token"></a>3. Het token gebruiken
Nu dat u hebt een toegangstoken is verkregen, kunt u het token in aanvragen naar uw back-end web-API's door op te nemen in de `Authorization` header:

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="4-refresh-the-token"></a>4. Vernieuwen van het token
Toegangstokens en ID-tokens zijn tijdelijke. Nadat ze zijn verlopen, kunt u deze om door te gaan voor toegang tot resources moet vernieuwen. U doet dit door verzenden naar een andere POST-aanvraag de `/token` eindpunt. Dit moment bieden de `refresh_token` in plaats van de `code`:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://fabrikamb2c.b2clogin.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&client_secret=JqQX2PNo9bpM0uEihUPzyrh&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob
```

| Parameter | Vereist? | Description |
| --- | --- | --- |
| p |Vereist |De gebruikersstroom die is gebruikt om de oorspronkelijke vernieuwingstoken te verkrijgen. U kunt een andere gebruikersstroom niet gebruiken in deze aanvraag. Houd er rekening mee dat u deze parameter toevoegen de *querytekenreeks*, dus niet in de hoofdtekst van het bericht. |
| client_id |Vereist |De toepassings-ID die is toegewezen aan uw app in de [Azure-portal](https://portal.azure.com). |
| client_secret |Vereist |De waarde voor client_secret die is gekoppeld aan uw client_id in de [Azure-portal](https://portal.azure.com). |
| grant_type |Vereist |Het type verlenen. Voor deze kant van de autorisatiecodestroom, het machtigingstype moet `refresh_token`. |
| scope |Aanbevolen |Een door spaties gescheiden lijst met bereiken. Een waarde één scope geeft u aan Azure AD zowel de machtigingen die worden aangevraagd. Met behulp van de client-ID als het bereik geeft aan dat uw app moet een toegangstoken dat kan worden gebruikt voor uw eigen service of web-API, vertegenwoordigd door de dezelfde client-ID.  De `offline_access` bereik geeft aan dat uw app een vernieuwingstoken voor lange levensduur hebben toegang tot bronnen moet.  Ook kunt u de `openid` bereik om aan te vragen van een ID-token van Azure AD B2C. |
| redirect_uri |Optioneel |De omleidings-URI van de toepassing waar u de autorisatiecode ontvangen. |
| refresh_token |Vereist |De oorspronkelijke vernieuwingstoken die u hebt verkregen in het tweede gedeelte van de stroom. |

Een geslaagde respons token ziet er als volgt:

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| Parameter | Description |
| --- | --- |
| not_before |De tijd waarop het token wordt beschouwd als geldig is, in epoche-tijd. |
| token_type |De waarde van het type token. Het enige type die ondersteuning biedt voor Azure AD is Bearer. |
| access_token |De ondertekende JWT dat u hebt aangevraagd. |
| scope |De bereiken die het token geldig is. U kunt ook de bereiken die in de cache-tokens gebruiken voor later gebruik. |
| expires_in |De hoeveelheid tijd die het token geldig is (in seconden). |
| refresh_token |Een vernieuwingstoken OAuth 2.0. De app kunt dit token gebruiken om aanvullende tokens verkrijgen nadat het huidige token is verlopen. Vernieuwen van tokens worden lange levensduur hebben en kan worden gebruikt voor toegang tot resources behouden gedurende langere tijd wordt opgelost. Zie voor meer informatie de [naslaginformatie over Azure AD B2C-tokens](active-directory-b2c-reference-tokens.md). |

Foutberichten er als volgt:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parameter | Description |
| --- | --- |
| error |Een tekenreeks voor de foutcode die u gebruiken kunt voor het classificeren van typen fouten die optreden. U kunt ook de tekenreeks gebruiken om te reageren op fouten. |
| error_description |Een bericht specifieke fout die u kan helpen de hoofdoorzaak van een verificatiefout identificeren. |

## <a name="use-your-own-azure-ad-b2c-directory"></a>Uw eigen Azure AD B2C-directory gebruiken
Als u wilt proberen deze aanvragen zelf, moet u de volgende stappen uitvoeren. Vervang de voorbeeldwaarden die we in dit artikel door uw eigen waarden gebruikt.

1. [Een Azure AD B2C-directory maken](active-directory-b2c-get-started.md). Gebruik de naam van uw directory in de aanvragen.
2. [Maken van een toepassing](active-directory-b2c-app-registration.md) verkrijgen van een toepassings-ID en een omleidings-URI. Een systeemeigen client opnemen in uw app.
3. [Maken van uw gebruikersstromen](active-directory-b2c-reference-policies.md) om op te halen van de gebruiker namen van de stroom.

