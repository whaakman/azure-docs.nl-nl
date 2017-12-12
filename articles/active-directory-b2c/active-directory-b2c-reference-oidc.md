---
title: Web-aanmelden met OpenID Connect - Azure AD B2C | Microsoft Docs
description: Maken van webtoepassingen met behulp van de Azure Active Directory-implementatie van het OpenID Connect-verificatieprotocol
services: active-directory-b2c
documentationcenter: 
author: saeedakhter-msft
manager: mtillman
editor: parakhj
ms.assetid: 21d420c8-3c10-4319-b681-adf2e89e7ede
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2017
ms.author: saeedakhter-msft
ms.openlocfilehash: 0eb4194307d1d3953fa1cd88ac014ac7c2ba7311
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-b2c-web-sign-in-with-openid-connect"></a>Azure Active Directory B2C: Web aanmelden met OpenID Connect
OpenID Connect is een protocol voor verificatie, gebouwd op OAuth 2.0, die kunnen worden gebruikt voor het veilig Meld u aan gebruikers met webtoepassingen. Met behulp van de Azure Active Directory B2C (Azure AD B2C)-implementatie van OpenID Connect, u kunt uitbesteden registreren, aanmelden en andere identiteitsbeheer in uw webtoepassingen met Azure Active Directory (Azure AD). Deze handleiding wordt beschreven hoe u dit doet op een manier taalonafhankelijke. Wordt beschreven hoe u berichten verzenden en ontvangen HTTP zonder gebruik van een van onze open source-bibliotheken.

[OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html) breidt het OAuth 2.0 *autorisatie* protocol voor gebruik als een *verificatie* protocol. Hiermee kunt u eenmalige aanmelding met behulp van OAuth uitvoeren. Dit introduceert het concept van een *token ID*, dit is een beveiligingstoken waarmee de client de identiteit van de gebruiker en basisprofiel informatie verkrijgen over de gebruiker.

Omdat het OAuth 2.0 uitbreidt, kunt u eveneens apps veilig verkrijgen *toegang tot tokens*. U kunt access_tokens voor toegang tot bronnen die zijn beveiligd met een [autorisatie server](active-directory-b2c-reference-protocols.md#the-basics). Het is raadzaam OpenID Connect als u een webtoepassing die wordt gehost op een server en is toegankelijk via een browser maakt. Als u Identiteitsbeheer toevoegen aan uw mobiele of bureaubladtoepassingen toepassingen wilt met behulp van Azure AD B2C, moet u [OAuth 2.0](active-directory-b2c-reference-oauth-code.md) in plaats van het OpenID Connect.

Azure AD B2C breidt het OpenID Connect-standaardprotocol hiervoor meer dan een eenvoudige verificatie en autorisatie. Het geeft de [Beleidsparameter](active-directory-b2c-reference-policies.md), waardoor u OpenID Connect gebruiken om toe te voegen gebruikerservaringen--zoals aanmelden, aanmelden en Profielbeheer--aan uw app. Hier wordt beschreven hoe u OpenID Connect en beleidsregels gebruiken om elk van deze ervaringen te implementeren in uw webtoepassingen. Ook ziet u hoe toegangstokens ophalen voor toegang tot web-API's.

De voorbeeld-HTTP-aanvragen in de volgende sectie Gebruik onze voorbeeld B2C-directory, fabrikamb2c.onmicrosoft.com, evenals onze voorbeeldtoepassing https://aadb2cplayground.azurewebsites.net en beleidsregels. U kunt gratis uitproberen de aanvragen zelf met behulp van deze waarden, of kunt u deze vervangen door uw eigen.
Meer informatie over hoe [ophalen van uw eigen B2C-tenant-, toepassings- en beleid](#use-your-own-b2c-directory).

## <a name="send-authentication-requests"></a>Verificatieaanvragen verzenden
Wanneer uw web-app moet de gebruiker verifiëren en uitvoeren van een beleid, de gebruiker kan worden doorgestuurd de `/authorize` eindpunt. Dit is het interactieve gedeelte van de stroom waarin de gebruiker wordt optreden, afhankelijk van het beleid.

In deze aanvraag, de client geeft aan de machtigingen die nodig zijn voor het verkrijgen van de gebruiker in de `scope` parameter en het beleid uit te voeren de `p` parameter. Drie voorbeelden vindt u in de volgende secties (met regeleinden voor leesbaarheid), elk met een ander beleid. Voor een idee krijgen hoe elke aanvraag werkt, probeer de aanvraag in een browser te plakken en het uitvoeren.

#### <a name="use-a-sign-in-policy"></a>Een beleid voor aanmelden gebruiken
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_in
```

#### <a name="use-a-sign-up-policy"></a>Een registratiebeleid gebruiken
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_up
```

#### <a name="use-an-edit-profile-policy"></a>Gebruik van een beleid voor het profiel bewerken
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_edit_profile
```

| Parameter | Vereist? | Beschrijving |
| --- | --- | --- |
| client_id |Vereist |De aanvraag-ID die de [Azure-portal](https://portal.azure.com/) toegewezen aan uw app. |
| response_type |Vereist |Het antwoordtype die een token ID voor het OpenID Connect moet bevatten. Als uw web-app ook tokens moet voor het aanroepen van een web-API, kunt u `code+id_token`, zoals we hier hebt gedaan. |
| redirect_uri |Aanbevolen |De `redirect_uri` parameter van uw app, waarbij verificatie reacties kunnen worden verzonden en ontvangen door uw app. Deze moet exact overeenkomen met een van de `redirect_uri` parameters die u in de portal hebt geregistreerd, behalve dat het moet een URL zijn gecodeerd. |
| Bereik |Vereist |Een door spaties gescheiden lijst met bereiken. Een waarde op één scope geeft u aan Azure AD beide machtigingen die worden aangevraagd. De `openid` bereik geeft aan dat een machtiging voor het aanmelden van de gebruiker en gegevens over de gebruiker in de vorm van een ID-tokens (later meer op deze later in dit artikel) ophalen. De `offline_access` bereik is optioneel voor web-apps. Hiermee wordt aangegeven dat uw app moet een *vernieuwingstoken* voor lange levensduur hebben toegang tot bronnen. |
| response_mode |Aanbevolen |De methode die moet worden gebruikt voor het verzenden van de resulterende autorisatiecode terug naar uw app. Het kan zijn `query`, `form_post`, of `fragment`.  De `form_post` antwoord-modus wordt aanbevolen voor de beste beveiliging. |
| state |Aanbevolen |Een waarde die is opgenomen in de aanvraag die ook in het token antwoord wordt geretourneerd. Een tekenreeks van inhoud die u wilt dat kan zijn. Een willekeurig gegenereerde unieke waarde wordt doorgaans gebruikt voor het voorkomen van aanvraagvervalsing op meerdere sites aanvallen. De status wordt ook gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat het verificatieverzoek opgetreden, zoals de pagina die ze op waren. |
| nonce |Vereist |Een waarde die is opgenomen in de aanvraag (gegenereerd door de app) die wordt opgenomen in het resulterende token ID als een claim. De app kunt vervolgens controleren of deze waarde om te beperken token replay-aanvallen. De waarde is doorgaans een willekeurige unieke tekenreeks die kan worden gebruikt voor het identificeren van de oorsprong van de aanvraag. |
| P |Vereist |Het beleid dat wordt uitgevoerd. Dit is de naam van een beleid dat is gemaakt in uw B2C-tenant. De waarde van de beleid-naam moet beginnen met `b2c\_1\_`. Meer informatie over het beleid en de [uitbreidbaar beleidsframework](active-directory-b2c-reference-policies.md). |
| prompt |Optioneel |Het type van de interactie van de gebruiker die is vereist. De enige geldige waarde op dit moment is `login`, waardoor gebruikers hun referenties invoeren voor deze aanvraag. Eenmalige aanmelding wordt pas van kracht. |

Op dit moment wordt de gebruiker gevraagd om de werkstroom van het beleid te voltooien. Dit kan hebben betrekking op de gebruiker invoeren van hun gebruikersnaam en wachtwoord, aanmelden met een sociale identiteit aanmelden voor de map of een andere aantal stappen, afhankelijk van hoe het beleid wordt gedefinieerd.

Nadat de gebruiker het beleid voltooit, Azure AD retourneert een antwoord aan uw app op de aangegeven `redirect_uri` parameter met de methode die is opgegeven in de `response_mode` parameter. Het antwoord is hetzelfde voor elk van de bovenstaande gevallen, onafhankelijk van het beleid dat wordt uitgevoerd.

Een geslaagde reactie met `response_mode=fragment` eruit als:

```
GET https://aadb2cplayground.azurewebsites.net/#
id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parameter | Beschrijving |
| --- | --- |
| id_token |Het token ID die de app wordt aangevraagd. U kunt het token ID gebruiken om te controleren van de identiteit van de gebruiker en beginnen met een sessie met de gebruiker. Meer informatie over het ID-tokens en de inhoud ervan zijn opgenomen in de [Azure AD B2C tokenverwijzing](active-directory-b2c-reference-tokens.md). |
| code |De autorisatiecode dat de app is aangevraagd, als u hebt gebruikt `response_type=code+id_token`. De app kunt u de autorisatiecode gebruiken om aan te vragen van een toegangstoken voor een doelbron. Autorisatiecodes zijn zeer tijdelijke. Normaal gesproken verlopen ze na ongeveer 10 minuten. |
| state |Als een `state` parameter is opgenomen in de aanvraag dezelfde waarde moet worden weergegeven in het antwoord. De app moet controleren of de `state` waarden in de aanvraag en antwoord identiek zijn. |

Foutberichten kunnen ook worden verzonden naar de `redirect_uri` parameter zodat de app ze op de juiste wijze kan verwerken:

```
GET https://aadb2cplayground.azurewebsites.net/#
error=access_denied
&error_description=the+user+canceled+the+authentication
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parameter | Beschrijving |
| --- | --- |
| error |Een tekenreeks foutcode die kan worden gebruikt voor het classificeren van typen fouten die optreden en die kunnen worden gebruikt om te reageren op fouten. |
| error_description |Een specifieke foutbericht dat een ontwikkelaar kan helpen de hoofdoorzaak van een verificatiefout identificeren. |
| state |Zie de volledige beschrijving van de eerste tabel in deze sectie. Als een `state` parameter is opgenomen in de aanvraag dezelfde waarde moet worden weergegeven in het antwoord. De app moet controleren of de `state` waarden in de aanvraag en antwoord identiek zijn. |

## <a name="validate-the-id-token"></a>Het token ID valideren
Zojuist hebt ontvangen van een token ID is niet voldoende om de gebruiker te verifiëren. U moet de handtekening van het token ID valideren en controleer of de claims in het token per vereisten van uw app. Maakt gebruik van Azure AD B2C [JSON Web Tokens (JWTs)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) en cryptografie met openbare sleutels voor het ondertekenen van tokens en controleren of ze geldig zijn.

Er zijn veel open source-bibliotheken die beschikbaar zijn voor het valideren van JWTs, afhankelijk van de taal van voorkeur. U wordt aangeraden deze opties verkennen in plaats van uw eigen validatielogica implementeren. De informatie hier erg nuttig zijn in hoe de die bibliotheken niet normaal kunnen gebruiken.

Azure AD B2C heeft een eindpunt OpenID Connect metagegevens waarmee een app voor het ophalen van informatie over Azure AD B2C tijdens runtime. Deze informatie omvat eindpunten, inhoud van tokens en token-ondertekening sleutels. Er is een JSON-metagegevens-document voor elk beleid in uw B2C-tenant. Het metagegevensdocument voor de `b2c_1_sign_in` beleid in `fabrikamb2c.onmicrosoft.com` bevindt zich op:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in`

Een van de eigenschappen van dit document configuratie is `jwks_uri`, waarvan de waarde voor hetzelfde beleid zou worden:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in`.

Om te bepalen welk beleid is gebruikt in een ID voor ondertekening van token (en waar de metagegevens ophalen), hebt u twee opties. Eerst de naam van het beleid is opgenomen in de `acr` claim in het token ID. Zie voor informatie over het parseren van de claims van een token ID, de [Azure AD B2C tokenverwijzing](active-directory-b2c-reference-tokens.md). De andere optie is voor het coderen van het beleid in de waarde van de `state` parameter wanneer u de aanvraag en vervolgens worden ontsleuteld om te bepalen welk beleid wordt gebruikt. Beide methoden is geldig.

Nadat u het metagegevensdocument van het eindpunt van de metagegevens OpenID Connect hebt aangeschaft, kunt u de openbare RSA-256-sleutels (die zich op dit eindpunt) gebruiken om de handtekening van het token ID te valideren. Er is mogelijk meerdere sleutels die aan dit eindpunt op elk gewenst moment worden vermeld in de tijd die u kunt herkennen door een `kid` claim. De koptekst van het token ID bevat ook een `kid` claim, waarmee wordt aangegeven welke van deze sleutels is gebruikt voor het ondertekenen van het token ID. Zie voor meer informatie de [Azure AD B2C tokenverwijzing](active-directory-b2c-reference-tokens.md) (het gedeelte [valideren van tokens](active-directory-b2c-reference-tokens.md#token-validation), met name).
<!--TODO: Improve the information on this-->

Nadat u hebt geverifieerd dat de handtekening van het token ID, zijn er verschillende claims die u nodig hebt om te controleren. Bijvoorbeeld:

* U moet controleren of de `nonce` claim om token replayaanvallen te voorkomen. De waarde moet zijn opgegeven in de aanvraag aanmelden.
* U moet controleren of de `aud` claim om ervoor te zorgen dat het ID-token is uitgegeven voor uw app. De waarde moet de toepassings-ID van uw app.
* U moet controleren of de `iat` en `exp` claims om ervoor te zorgen dat het token ID niet is verlopen.

Er zijn ook enkele meer validaties die moeten worden uitgevoerd. Deze worden beschreven in de [OpenID Connect Core Spec](http://openid.net/specs/openid-connect-core-1_0.html).  U kunt ook aanvullende verklaren, afhankelijk van uw scenario. Sommige algemene validaties zijn onder andere:

* Ervoor zorgen dat/organisatie van de gebruiker heeft aangemeld voor de app.
* Ervoor te zorgen dat de gebruiker de juiste autorisatie/bevoegdheden heeft.
* Ervoor zorgen dat een bepaalde sterkte van verificatie heeft plaatsgevonden, zoals Azure multi-factor Authentication.

Zie voor meer informatie over de claims in een token ID, de [Azure AD B2C tokenverwijzing](active-directory-b2c-reference-tokens.md).

Nadat u hebt het token ID gevalideerd, kunt u beginnen met een sessie met de gebruiker. U kunt de claims in het token ID gebruiken om informatie te verkrijgen over de gebruiker in uw app. Gebruikt voor deze gegevens omvatten weergeven, records en autorisatie.

## <a name="get-a-token"></a>Een token ophalen
Als u uw web-app beleidsregels alleen worden uitgevoerd moet, kunt u de volgende secties overslaan. Deze secties zijn alleen van toepassing op web-apps die u wilt maken, geverifieerde aanroepen naar een web-API en zijn ook beveiligd door Azure AD B2C.

U kunt gebruikmaken van de autorisatiecode die u hebt verkregen (met behulp van `response_type=code+id_token`) voor een token op de gewenste resource door te sturen een `POST` aanvraag voor de `/token` eindpunt. De enige bron die u kunt een token voor aanvragen is op dit moment uw web-API van de back-end in de app. De overeenkomst voor het aanvragen van een token aan uzelf is het gebruik van uw app client-ID als het bereik:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>

```

| Parameter | Vereist? | Beschrijving |
| --- | --- | --- |
| P |Vereist |Het beleid dat is gebruikt voor het ophalen van de autorisatiecode. U kunt een ander beleid niet gebruiken in deze aanvraag. Opmerking dat u deze parameter toevoegen aan de query-tekenreeks niet op de `POST` hoofdtekst. |
| client_id |Vereist |De aanvraag-ID die de [Azure-portal](https://portal.azure.com/) toegewezen aan uw app. |
| grant_type |Vereist |Het type grant moet `authorization_code` voor de autorisatiecodestroom. |
| Bereik |Aanbevolen |Een door spaties gescheiden lijst met bereiken. Een waarde op één scope geeft u aan Azure AD beide machtigingen die worden aangevraagd. De `openid` bereik geeft aan dat een machtiging voor het aanmelden van de gebruiker en gegevens over de gebruiker in de vorm van id_token parameters ophalen. Het kan worden gebruikt om op te halen van tokens, voor uw app eigen back-end web-API, die wordt vertegenwoordigd door dezelfde toepassings-ID als de client. De `offline_access` bereik geeft aan dat uw app een vernieuwingstoken voor lange levensduur hebben toegang tot bronnen nodig. |
| code |Vereist |De autorisatiecode die u hebt verkregen in de eerste zijde van de stroom. |
| redirect_uri |Vereist |De `redirect_uri` parameter van de toepassing waarin u de autorisatiecode ontvangen. |
| client_secret |Vereist |De toepassingsgeheim dat u hebt gegenereerd in de [Azure-portal](https://portal.azure.com/). Deze toepassing geheime sleutel is een belangrijke beveiligingsupdate artefact. U moet het veilig opslaan op uw server. U moet ook deze clientgeheim periodiek draaien. |

Een geslaagde reactie token ziet eruit als:

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
| token_type |De waarde van het type token. Het enige type dat ondersteunt Azure AD is `Bearer`. |
| access_token |De ondertekende JWT-token dat u hebt aangevraagd. |
| Bereik |De scopes waarvoor het token geldig is. Deze kunnen worden gebruikt voor het opslaan van tokens voor later gebruik. |
| expires_in |De lengte van de tijd die het toegangstoken ongeldig (in seconden is). |
| refresh_token |Een OAuth 2.0-vernieuwingstoken. De app kunt dit token gebruiken om aanvullende tokens verkrijgen nadat het huidige token is verlopen. Vernieuwen van tokens worden lange levensduur hebben en kunnen worden gebruikt voor toegang tot bronnen voor langere tijd te behouden. Raadpleeg voor meer informatie de [B2C tokenverwijzing](active-directory-b2c-reference-tokens.md). U moet hebben gebruikt het bereik `offline_access` voor de verificatie- en aanvragen voor beveiligingstokens kunnen een vernieuwingstoken dat is ontvangen. |

Foutberichten ziet er als:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parameter | Beschrijving |
| --- | --- |
| error |Een tekenreeks foutcode die kan worden gebruikt voor het classificeren van typen fouten die optreden en die kunnen worden gebruikt om te reageren op fouten. |
| error_description |Een specifieke foutbericht dat een ontwikkelaar kan helpen de hoofdoorzaak van een verificatiefout identificeren. |

## <a name="use-the-token"></a>Gebruik het token
Nu dat u hebt een toegangstoken is verkregen, kunt u het token in aanvragen naar uw back-end web-API's door te nemen in de `Authorization` header:

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-the-token"></a>Het token vernieuwen
ID-tokens zijn tijdelijke. U moet ze vernieuwen nadat ze zijn verlopen om door te gaan wordt toegang tot bronnen. U kunt dit doen door het indienen van een andere `POST` aanvraag voor de `/token` eindpunt. Deze tijd bieden de `refresh_token` parameter in plaats van de `code` parameter:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=openid offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>
```

| Parameter | Vereist | Beschrijving |
| --- | --- | --- |
| P |Vereist |Het beleid dat is gebruikt voor het verkrijgen van het oorspronkelijke vernieuwingstoken. U kunt een ander beleid niet gebruiken in deze aanvraag. Houd er rekening mee dat u deze parameter aan de queryreeks, niet aan de POST-instantie toevoegt. |
| client_id |Vereist |De aanvraag-ID die de [Azure-portal](https://portal.azure.com/) toegewezen aan uw app. |
| grant_type |Vereist |Het type toekennen, die een vernieuwingstoken voor deze fase van de autorisatiecodestroom moet zijn. |
| Bereik |Aanbevolen |Een door spaties gescheiden lijst met bereiken. Een waarde op één scope geeft u aan Azure AD beide machtigingen die worden aangevraagd. De `openid` bereik geeft aan dat een machtiging voor het aanmelden van de gebruiker en gegevens over de gebruiker in de vorm van een ID-tokens verkrijgen. Het kan worden gebruikt om op te halen van tokens, voor uw app eigen back-end web-API, die wordt vertegenwoordigd door dezelfde toepassings-ID als de client. De `offline_access` bereik geeft aan dat uw app een vernieuwingstoken voor lange levensduur hebben toegang tot bronnen nodig. |
| redirect_uri |Aanbevolen |De `redirect_uri` parameter van de toepassing waarin u de autorisatiecode ontvangen. |
| refresh_token |Vereist |Het oorspronkelijke vernieuwingstoken die u hebt verkregen in de tweede fase van de stroom. U moet hebben gebruikt het bereik `offline_access` voor de verificatie- en aanvragen voor beveiligingstokens kunnen een vernieuwingstoken dat is ontvangen. |
| client_secret |Vereist |De toepassingsgeheim dat u hebt gegenereerd in de [Azure-portal](https://portal.azure.com/). Deze toepassing geheime sleutel is een belangrijke beveiligingsupdate artefact. U moet het veilig opslaan op uw server. U moet ook deze clientgeheim periodiek draaien. |

Een geslaagde reactie token ziet eruit als:

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
| token_type |De waarde van het type token. Het enige type dat ondersteunt Azure AD is `Bearer`. |
| access_token |De ondertekende JWT-token dat u hebt aangevraagd. |
| Bereik |Het bereik dat het token geldig is voor, die kan worden gebruikt voor het opslaan van tokens voor later gebruik. |
| expires_in |De lengte van de tijd die het toegangstoken ongeldig (in seconden is). |
| refresh_token |Een OAuth 2.0-vernieuwingstoken. De app kunt dit token gebruiken om aanvullende tokens verkrijgen nadat het huidige token is verlopen.  Vernieuwen van tokens worden lange levensduur hebben en kunnen worden gebruikt voor toegang tot bronnen voor langere tijd te behouden. Raadpleeg voor meer details over de [B2C tokenverwijzing](active-directory-b2c-reference-tokens.md). |

Foutberichten ziet er als:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parameter | Beschrijving |
| --- | --- |
| error |Een tekenreeks foutcode die kan worden gebruikt voor het classificeren van typen fouten die optreden en die kunnen worden gebruikt om te reageren op fouten. |
| error_description |Een specifieke foutbericht dat een ontwikkelaar kan helpen de hoofdoorzaak van een verificatiefout identificeren. |

## <a name="send-a-sign-out-request"></a>Afmelden aanvraag verzenden
Als u wilt dat de gebruiker buiten de app te melden, is het niet genoeg is voor het wissen van cookies of anderszins einde van uw app de sessie met de gebruiker. U moet de gebruiker ook omleiden naar Azure AD om af te melden. Als u dit niet doet, kan de gebruiker mogelijk om andere referenties voor uw app zonder hun referenties opnieuw invoeren. Dit komt doordat er een geldige eenmalige aanmelding sessie met Azure AD.

U kunt gewoon omleiden door de gebruiker de `end_session` eindpunt dat wordt weergegeven in het metagegevensdocument OpenID Connect eerder in de 'valideren van het token ID' sectie beschreven:

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/logout?
p=b2c_1_sign_in
&post_logout_redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
```

| Parameter | Vereist? | Beschrijving |
| --- | --- | --- |
| P |Vereist |Het beleid dat u gebruiken wilt voor het ondertekenen van de gebruiker buiten uw toepassing. |
| post_logout_redirect_uri |Aanbevolen |De URL die de gebruiker wordt omgeleid naar na geslaagde afmelding plaatsvindt. Als deze niet opgenomen is, Azure AD B2C ziet de gebruiker een algemeen foutbericht. |

> [!NOTE]
> Hoewel het routeren van de gebruiker de `end_session` eindpunt enkele van de status van de gebruiker één aanmelding met Azure AD B2C wordt gewist, wordt de gebruiker buiten hun sessie sociale identity-provider (IDP) niet ondertekent. Als de gebruiker de dezelfde IDP tijdens een volgende aanmelden selecteert, zal ze worden geverifieerd, zonder dat hun referenties invoeren. Als een gebruiker wil te melden bij uw B2C-toepassing, betekent het niet noodzakelijkerwijs dat ze wilt afmelden bij hun account Facebook. Echter, in het geval van lokale accounts de gebruikerssessie wordt beëindigd goed.
> 
> 

## <a name="use-your-own-b2c-tenant"></a>Gebruik uw eigen B2C-tenant
Als u proberen deze aanvragen voor uzelf wilt, moet u eerst deze drie stappen uitvoert en vervang de voorbeeldwaarden die hierboven worden beschreven, door uw eigen:

1. [Maken van een B2C-tenant](active-directory-b2c-get-started.md), en de naam van uw tenant te gebruiken in de aanvragen.
2. [Maken van een toepassing](active-directory-b2c-app-registration.md) verkrijgen van een toepassings-ID. Een web-app of web-API opnemen in uw app. Maak eventueel een toepassingsgeheim.
3. [Maken van uw beleid](active-directory-b2c-reference-policies.md) om op te halen van de beleidsnamen van uw.

