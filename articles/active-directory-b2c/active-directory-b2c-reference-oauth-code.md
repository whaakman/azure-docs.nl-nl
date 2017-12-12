---
title: Autorisatiecodestroom - Azure AD B2C | Microsoft Docs
description: Informatie over het bouwen van web-apps met behulp van Azure AD B2C en OpenID Connect-verificatieprotocol.
services: active-directory-b2c
documentationcenter: 
author: saeedakhter-msft
manager: mtillman
editor: parakhj
ms.assetid: c371aaab-813a-4317-97df-b62e2f53d865
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2017
ms.author: saeedakhter-msft
ms.openlocfilehash: 99a292c6be66016264e528525a5920667207b605
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-b2c-oauth-20-authorization-code-flow"></a>Azure Active Directory B2C: OAuth 2.0-autorisatiecodestroom
U kunt het verlenen van OAuth 2.0 autorisatie code in apps die zijn geïnstalleerd op een apparaat toegang te krijgen tot beveiligde bronnen, zoals web-API's gebruiken. Met behulp van de Azure Active Directory B2C (Azure AD B2C)-implementatie van OAuth 2.0, u kunt toevoegen, registreren, aanmelden en andere identiteitsbeheer taken naar uw mobiele en bureaublad-apps. In dit artikel is taalonafhankelijk. In het artikel wordt beschreven hoe verzenden en ontvangen van HTTP-berichten zonder gebruik van een open source-bibliotheken.

<!-- TODO: Need link to libraries -->

De OAuth 2.0-autorisatiecodestroom wordt beschreven in [sectie 4.1 van de OAuth 2.0-specificatie](http://tools.ietf.org/html/rfc6749). U kunt deze gebruiken voor verificatie en autorisatie in de meeste typen van de app, inclusief [web-apps](active-directory-b2c-apps.md#web-apps) en [systeemeigen geïnstalleerde apps](active-directory-b2c-apps.md#mobile-and-native-apps). U kunt het OAuth 2.0-autorisatiecodestroom veilig verkrijgen *toegang tot tokens* voor uw apps, die kan worden gebruikt voor toegang tot resources die zijn beveiligd met een [autorisatie server](active-directory-b2c-reference-protocols.md#the-basics).

In dit artikel is gericht op de **openbare clients** OAuth 2.0-autorisatiecodestroom. Een openbare-client is elke clienttoepassing die niet kan vertrouwd worden voor de integriteit van een geheim wachtwoord veilig te behouden. Dit omvat mobiele apps, bureaublad-apps en in wezen elke toepassing die wordt uitgevoerd op een apparaat en moet toegangstokens ophalen. 

> [!NOTE]
> Gebruiken voor het identiteitsbeheer toevoegen aan een web-app met behulp van Azure AD B2C, [OpenID Connect](active-directory-b2c-reference-oidc.md) in plaats van OAuth 2.0.

Azure AD B2C breidt de standaard die OAuth 2.0, loopt u meer dan een eenvoudige verificatie en autorisatie kunt doen. Het geeft de [Beleidsparameter](active-directory-b2c-reference-policies.md). Met ingebouwde beleidsregels, kunt u OAuth 2.0 gebruikerservaringen zoals toevoegen aan uw app registreren, aanmelden en Profielbeheer. In dit artikel wordt beschreven hoe u OAuth 2.0 en beleidsregels gebruiken om elk van deze ervaringen te implementeren in uw eigen toepassingen. We ook hoe u toegangstokens ophalen voor toegang tot web-API's.

In het voorbeeld HTTP-aanvragen in dit artikel, gebruiken we onze voorbeeld Azure AD B2C-directory **fabrikamb2c.onmicrosoft.com**. We gebruiken ook onze voorbeeldtoepassing en beleidsregels. U kunt de aanvragen zelf met behulp van deze waarden, of kunt u deze vervangen door uw eigen waarden.
Meer informatie over hoe [ophalen van uw eigen Azure AD B2C-directory, toepassingen en beleidsregels](#use-your-own-azure-ad-b2c-directory).

## <a name="1-get-an-authorization-code"></a>1. Een autorisatiecode ophalen
De autorisatiecodestroom begint met de client voor het routeren van de gebruiker de `/authorize` eindpunt. Dit is het interactieve deel van de stroom waarin de gebruiker neemt actie. In deze aanvraag is de client geeft aan de `scope` parameter de machtigingen die nodig zijn voor het verkrijgen van de gebruiker. In de `p` parameter, betekent dit het beleid uit te voeren. De volgende drie voorbeelden (met regeleinden voor leesbaarheid) elk gebruik een ander beleid.

### <a name="use-a-sign-in-policy"></a>Een beleid voor aanmelden gebruiken
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_sign_in
```

### <a name="use-a-sign-up-policy"></a>Een registratiebeleid gebruiken
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_sign_up
```

### <a name="use-an-edit-profile-policy"></a>Gebruik van een beleid voor het profiel bewerken
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_edit_profile
```

| Parameter | Vereist? | Beschrijving |
| --- | --- | --- |
| client_id |Vereist |De toepassings-ID die is toegewezen aan uw app in de [Azure-portal](https://portal.azure.com). |
| response_type |Vereist |Het antwoordtype, waaronder moet `code` voor de autorisatiecodestroom. |
| redirect_uri |Vereist |De omleidings-URI van uw app, waarbij verificatie antwoorden zijn verzonden en ontvangen door uw app. Deze moet exact overeenkomen met een van de omleidings-URI's die u hebt geregistreerd in de portal, behalve dat het URL-codering moet worden. |
| Bereik |Vereist |Een door spaties gescheiden lijst met bereiken. Een waarde op één scope geeft u aan Azure Active Directory (Azure AD) beide machtigingen die worden aangevraagd. Met behulp van de client-ID als het bereik geeft aan dat uw app moet een toegangstoken die kan worden gebruikt op basis van uw eigen service of web-API, vertegenwoordigd door de dezelfde client-ID.  De `offline_access` bereik geeft aan dat uw app een vernieuwingstoken voor de lange levensduur hebben toegang tot bronnen moet. Ook kunt u de `openid` bereik voor het aanvragen van een token ID bij Azure AD B2C. |
| response_mode |Aanbevolen |De methode die u gebruikt voor het verzenden van de resulterende autorisatiecode terug naar uw app. Kan `query`, `form_post`, of `fragment`. |
| state |Aanbevolen |Een waarde die is opgenomen in de aanvraag die in het token antwoord wordt geretourneerd. Het kan zijn dat een tekenreeks van inhoud die u wilt gebruiken. Normaal gesproken wordt een willekeurig gegenereerde unieke waarde gebruikt, om aanvraagvervalsing op meerdere sites aanvallen te voorkomen. De status wordt ook gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat de verificatieaanvraag heeft plaatsgevonden. Bijvoorbeeld, de pagina die de gebruiker is op of het beleid dat werd uitgevoerd. |
| P |Vereist |Het beleid dat wordt uitgevoerd. Dit is de naam van een beleid dat is gemaakt in uw Azure AD B2C-directory. De waarde van de beleid-naam moet beginnen met **b2c\_1\_**. Zie voor meer informatie over het beleid, [ingebouwde beleid van Azure AD B2C](active-directory-b2c-reference-policies.md). |
| prompt |Optioneel |Het type van de interactie van de gebruiker die is vereist. De enige geldige waarde is momenteel `login`, waardoor gebruikers hun referenties invoeren voor deze aanvraag. Eenmalige aanmelding wordt pas van kracht. |

Op dit moment wordt de gebruiker gevraagd om de werkstroom van het beleid te voltooien. Dit kan de gebruiker invoeren van hun gebruikersnaam en wachtwoord, aanmelden met een sociale identiteit aanmelden voor de map of andere aantal stappen inhouden. Acties van de gebruiker is afhankelijk van hoe het beleid wordt gedefinieerd.

Nadat de gebruiker het beleid voltooit, Azure AD retourneert een antwoord aan uw app op de waarde die u gebruikt voor `redirect_uri`. Gebruikt de methode die is opgegeven in de `response_mode` parameter. Het antwoord is exact hetzelfde zijn voor elk van de gebruiker actie scenario's, onafhankelijk van het beleid dat is uitgevoerd.

Een geslaagde reactie die gebruikmaakt van `response_mode=query` ziet er als volgt:

```
GET urn:ietf:wg:oauth:2.0:oob?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...        // the authorization_code, truncated
&state=arbitrary_data_you_can_receive_in_the_response                // the value provided in the request
```

| Parameter | Beschrijving |
| --- | --- |
| code |De autorisatiecode die de app wordt aangevraagd. De app kunt u de autorisatiecode gebruiken om aan te vragen van een toegangstoken voor een doelbron. Autorisatiecodes zijn zeer tijdelijke. Normaal gesproken verlopen ze na ongeveer 10 minuten. |
| state |Zie de volledige beschrijving van de tabel in de vorige sectie. Als een `state` parameter is opgenomen in de aanvraag dezelfde waarde moet worden weergegeven in het antwoord. De app moet controleren of de `state` waarden in de aanvraag en antwoord identiek zijn. |

Foutberichten kunnen ook worden verzonden naar de omleidings-URI zodat de app ze op de juiste wijze kan verwerken:

```
GET urn:ietf:wg:oauth:2.0:oob?
error=access_denied
&error_description=The+user+has+cancelled+entering+self-asserted+information
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parameter | Beschrijving |
| --- | --- |
| error |Een tekenreeks van de fout code die u gebruiken kunt voor het classificeren van de typen fouten die optreden. U kunt ook de tekenreeks gebruiken om te reageren op fouten. |
| error_description |Een specifiek foutbericht die u kan helpen de hoofdoorzaak van een verificatiefout identificeren. |
| state |Zie de volledige beschrijving in de voorgaande tabel. Als een `state` parameter is opgenomen in de aanvraag dezelfde waarde moet worden weergegeven in het antwoord. De app moet controleren of de `state` waarden in de aanvraag en antwoord identiek zijn. |

## <a name="2-get-a-token"></a>2. Een token ophalen
Nu dat u een autorisatiecode hebt aangeschaft, kunt u inwisselen de `code` voor een token dat de beoogde resource met het verzenden van een POST-aanvraag voor de `/token` eindpunt. In Azure AD B2C is de enige resource die u kunt aanvragen een token voor uw web-API van de back-end in de app. De overeenkomst die wordt gebruikt voor het aanvragen van een token aan uzelf is het gebruik van uw app client-ID als het bereik:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob

```

| Parameter | Vereist? | Beschrijving |
| --- | --- | --- |
| P |Vereist |Het beleid dat is gebruikt voor het ophalen van de autorisatiecode. U kunt een ander beleid niet gebruiken in deze aanvraag. Houd er rekening mee dat u, deze parameter moet naar toevoegt de *querytekenreeks*, niet in de hoofdtekst van de POST. |
| client_id |Vereist |De toepassings-ID die is toegewezen aan uw app in de [Azure-portal](https://portal.azure.com). |
| grant_type |Vereist |Het type verlenen. Voor de autorisatiecodestroom het grant-type moet `authorization_code`. |
| Bereik |Aanbevolen |Een door spaties gescheiden lijst met bereiken. Een waarde op één scope geeft u aan Azure AD zowel de machtigingen die worden aangevraagd. Met behulp van de client-ID als het bereik geeft aan dat uw app moet een toegangstoken die kan worden gebruikt op basis van uw eigen service of web-API, vertegenwoordigd door de dezelfde client-ID.  De `offline_access` bereik geeft aan dat uw app een vernieuwingstoken voor de lange levensduur hebben toegang tot bronnen moet.  Ook kunt u de `openid` bereik voor het aanvragen van een token ID bij Azure AD B2C. |
| code |Vereist |De autorisatiecode die u hebt verkregen in de eerste zijde van de stroom. |
| redirect_uri |Vereist |De omleidings-URI van de toepassing waarin u de autorisatiecode ontvangen. |

Een geslaagde reactie token ziet er als volgt:

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
| Parameter | Beschrijving |
| --- | --- |
| not_before |De tijd waarop het token wordt beschouwd als geldige epoche tijdstip. |
| token_type |De waarde van het type token. Het enige type dat ondersteuning biedt voor Azure AD is Bearer. |
| access_token |De ondertekende JSON Web Token (JWT) die u hebt aangevraagd. |
| Bereik |De bereiken die de token geldig is. U kunt bereiken die in cache tokens gebruiken voor later gebruik. |
| expires_in |De lengte van de tijd die het token geldig is (in seconden). |
| refresh_token |Een OAuth 2.0-vernieuwingstoken. De app kunt dit token gebruiken om aanvullende tokens verkrijgen nadat het huidige token is verlopen. Vernieuwen van tokens zijn lange levensduur hebben. U kunt deze gebruiken voor het bewaren van toegang tot bronnen voor langere tijd. Zie voor meer informatie de [Azure AD B2C tokenverwijzing](active-directory-b2c-reference-tokens.md). |

-Foutberichten moeten uitzien:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parameter | Beschrijving |
| --- | --- |
| error |Een tekenreeks van de fout code die u gebruiken kunt voor het classificeren van de typen fouten die optreden. U kunt ook de tekenreeks gebruiken om te reageren op fouten. |
| error_description |Een specifiek foutbericht die u kan helpen de hoofdoorzaak van een verificatiefout identificeren. |

## <a name="3-use-the-token"></a>3. Gebruik het token
Nu dat u hebt een toegangstoken is verkregen, kunt u het token in aanvragen naar uw back-end web-API's door te nemen in de `Authorization` header:

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="4-refresh-the-token"></a>4. Het token vernieuwen
Toegangstokens en ID-tokens zijn tijdelijke. Nadat ze zijn verlopen, moet u ze toegang houden tot bronnen te vernieuwen. Om dit te doen, een andere POST-aanvraag verzendt naar de `/token` eindpunt. Deze tijd bieden de `refresh_token` in plaats van de `code`:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob
```

| Parameter | Vereist? | Beschrijving |
| --- | --- | --- |
| P |Vereist |Het beleid dat is gebruikt voor het verkrijgen van het oorspronkelijke vernieuwingstoken. U kunt een ander beleid niet gebruiken in deze aanvraag. Houd er rekening mee dat u, deze parameter moet naar toevoegt de *querytekenreeks*, niet in de hoofdtekst van de POST. |
| client_id |Aanbevolen |De toepassings-ID die is toegewezen aan uw app in de [Azure-portal](https://portal.azure.com). |
| grant_type |Vereist |Het type verlenen. Voor deze fase van de autorisatiecodestroom, het type grant moet `refresh_token`. |
| Bereik |Aanbevolen |Een door spaties gescheiden lijst met bereiken. Een waarde op één scope geeft u aan Azure AD zowel de machtigingen die worden aangevraagd. Met behulp van de client-ID als het bereik geeft aan dat uw app moet een toegangstoken die kan worden gebruikt op basis van uw eigen service of web-API, vertegenwoordigd door de dezelfde client-ID.  De `offline_access` bereik geeft aan dat uw app een vernieuwingstoken voor lange levensduur hebben toegang tot bronnen nodig.  Ook kunt u de `openid` bereik voor het aanvragen van een token ID bij Azure AD B2C. |
| redirect_uri |Optioneel |De omleidings-URI van de toepassing waarin u de autorisatiecode ontvangen. |
| refresh_token |Vereist |Het oorspronkelijke vernieuwingstoken die u hebt verkregen in de tweede fase van de stroom. |

Een geslaagde reactie token ziet er als volgt:

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
| Parameter | Beschrijving |
| --- | --- |
| not_before |De tijd waarop het token wordt beschouwd als geldige epoche tijdstip. |
| token_type |De waarde van het type token. Het enige type dat ondersteuning biedt voor Azure AD is Bearer. |
| access_token |De ondertekende JWT die u hebt aangevraagd. |
| Bereik |De bereiken die de token geldig is. Ook kunt u de bereiken voor het cache-tokens voor later gebruik. |
| expires_in |De lengte van de tijd die het token geldig is (in seconden). |
| refresh_token |Een OAuth 2.0-vernieuwingstoken. De app kunt dit token gebruiken om aanvullende tokens verkrijgen nadat het huidige token is verlopen. Vernieuwen van tokens worden lange levensduur hebben en kunnen worden gebruikt voor toegang tot bronnen voor langere tijd te behouden. Zie voor meer informatie de [Azure AD B2C tokenverwijzing](active-directory-b2c-reference-tokens.md). |

-Foutberichten moeten uitzien:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parameter | Beschrijving |
| --- | --- |
| error |Een tekenreeks van de fout code die u gebruiken kunt voor het classificeren van typen fouten die optreden. U kunt ook de tekenreeks gebruiken om te reageren op fouten. |
| error_description |Een specifiek foutbericht die u kan helpen de hoofdoorzaak van een verificatiefout identificeren. |

## <a name="use-your-own-azure-ad-b2c-directory"></a>Uw eigen Azure AD B2C-directory gebruiken
Als u wilt proberen deze aanvragen zelf, moet u de volgende stappen uitvoeren. Vervang de voorbeeldwaarden die wordt gebruikt in dit artikel met uw eigen waarden.

1. [Een Azure AD B2C-directory maken](active-directory-b2c-get-started.md). Gebruik de naam van uw directory in de aanvragen.
2. [Maken van een toepassing](active-directory-b2c-app-registration.md) om een toepassings-ID en een omleidings-URI te verkrijgen. Een systeemeigen client opnemen in uw app.
3. [Maken van uw beleid](active-directory-b2c-reference-policies.md) om op te halen van de beleidsnamen van uw.

