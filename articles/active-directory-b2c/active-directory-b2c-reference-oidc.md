---
title: Web-aanmelding met OpenID verbinding maken in Azure Active Directory B2C | Microsoft Docs
description: Het bouwen van toepassingen met behulp van de Azure Active Directory-implementatie van de OpenID Connect-verificatieprotocol.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 39a3164c27fa30250fe08e864db889eac844f646
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55173000"
---
# <a name="azure-active-directory-b2c-web-sign-in-with-openid-connect"></a>Azure Active Directory B2C: Webaanmelding met OpenID Connect
OpenID Connect is een protocol voor verificatie, gebouwd op OAuth 2.0, die kan worden gebruikt om veilig gebruikers zich aanmelden voor webtoepassingen. Met behulp van de Azure Active Directory B2C (Azure AD B2C)-implementatie van OpenID Connect, u kunt uitbesteden gebruikersregistratie, aanmelding en andere identiteitsbeheer in uw web-apps aan Azure Active Directory (Azure AD). Deze handleiding wordt beschreven hoe u dit niet doet, op een taalonafhankelijke manier. Dit wordt beschreven hoe u berichten verzenden en ontvangen HTTP zonder onze open source-bibliotheken.

[OpenID Connect](https://openid.net/specs/openid-connect-core-1_0.html) kunt u de OAuth 2.0 uitbreiden *autorisatie* protocol voor gebruik als een *verificatie* protocol. Hiermee kunt u om uit te voeren eenmalige aanmelding via OAuth. Dit introduceert het concept van een *ID-token*, dit is een beveiligingstoken dat kan de client de identiteit van de gebruiker en basisprofielgegevens informatie over de gebruiker te verkrijgen.

Omdat deze kunt u OAuth 2.0 uitbreiden, kunt deze ook apps aan te schaffen veilig *toegangstokens*. U kunt access_tokens voor toegang tot resources die worden beveiligd door een [autorisatieserver](active-directory-b2c-reference-protocols.md#the-basics). We raden aan OpenID verbinding maken als u een webtoepassing die wordt gehost op een server en toegankelijk is via een browser. Als u toevoegen van identiteitsbeheer aan uw mobiele of bureaubladtoepassingen toepassingen wilt met behulp van Azure AD B2C, moet u [OAuth 2.0](active-directory-b2c-reference-oauth-code.md) in plaats van de OpenID Connect.

Azure AD B2C breidt het standaard OpenID Connect-protocol om te doen meer dan een eenvoudige verificatie en autorisatie. Het geeft de [gebruiker stroom parameter](active-directory-b2c-reference-policies.md), waarmee u kunt OpenID Connect gebruikt om toe te voegen gebruikerservaringen--zoals aanmelden, aanmelding en Profielbeheer--aan uw app. Hier, we laten zien hoe u OpenID Connect en gebruiker stromen gebruiken voor het implementeren van elk van deze ervaringen in uw webtoepassingen. Ook leert u hoe u aan de toegangstokens voor toegang tot web-API's.

De voorbeeld-HTTP-aanvragen in de volgende sectie gebruik ons voorbeeld B2C-directory, fabrikamb2c.onmicrosoft.com, evenals in onze voorbeeldtoepassing https://aadb2cplayground.azurewebsites.net, en gebruikersstromen. U vrij om uit te proberen de aanvragen zelf met behulp van deze waarden, of kunt u deze vervangen door uw eigen.
Meer informatie over het [ophalen van uw eigen B2C-tenant, de toepassing en de gebruiker stromen](#use-your-own-b2c-directory).

## <a name="send-authentication-requests"></a>Verzenden van aanvragen voor authenticatie
Wanneer uw web-app moet de gebruiker verifiëren en uitvoeren van een beleid, het kunt instellen dat de gebruiker de `/authorize` eindpunt. Dit is de interactieve gedeelte van de stroom, waarbij de gebruiker optreden, afhankelijk van de gebruikersstroom maakt.

In deze aanvraag, de client geeft aan dat de machtigingen die nodig is om te verkrijgen van de gebruiker in de `scope` parameter en de gebruikersstroom om uit te voeren de `p` parameter. Drie voorbeelden vindt u in de volgende secties (met regeleinden voor de leesbaarheid), elk met behulp van de stroom van een andere gebruiker. Ophalen van een idee van hoe elke aanvraag werkt, probeer de aanvraag in een browser plakken en uitvoeren.

#### <a name="use-a-sign-in-user-flow"></a>Een gebruikersstroom aanmelden gebruiken
```
GET https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_in
```

#### <a name="use-a-sign-up-user-flow"></a>Gebruik een proefaccount gebruikersstroom
```
GET https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_up
```

#### <a name="use-an-edit-profile-user-flow"></a>Gebruik een gebruikersstroom profiel bewerken
```
GET https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
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
| response_type |Vereist |Het antwoordtype een ID-token voor de OpenID Connect bevatten moet. Als uw web-app ook tokens moet voor het aanroepen van een web-API, kunt u `code+id_token`, zoals we hier hebben gedaan. |
| redirect_uri |Aanbevolen |De `redirect_uri` parameter van uw app, waarbij verificatiereacties kunnen worden verzonden en ontvangen door uw app. Het moet exact overeenkomen met een van de `redirect_uri` parameters die u in de portal hebt geregistreerd, behalve dat het URL-codering moet worden. |
| scope |Vereist |Een door spaties gescheiden lijst met bereiken. Een waarde één scope geeft u aan Azure AD beide machtigingen die worden aangevraagd. De `openid` bereik geeft aan dat een machtiging om te melden bij de gebruiker en gegevens over de gebruiker in de vorm van ID-tokens (later meer hierover later in dit artikel). De `offline_access` bereik is optioneel voor web-apps. Hiermee wordt aangegeven dat uw app moet een *vernieuwingstoken* voor lange levensduur hebben toegang tot bronnen. |
| response_mode |Aanbevolen |De methode die moet worden gebruikt voor het verzenden van de resulterende autorisatiecode terug naar de app. Het kan zijn `query`, `form_post`, of `fragment`.  De `form_post` antwoordmodus wordt aanbevolen voor de beste beveiliging. |
| state |Aanbevolen |Een waarde die is opgenomen in de aanvraag die ook in het token antwoord wordt geretourneerd. Een tekenreeks van de inhoud die u wilt dat kan zijn. Een willekeurig gegenereerde unieke waarde wordt meestal gebruikt voor aanvallen van aanvraag voor cross-site kunnen worden vervalst. De status wordt ook gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat de verificatieaanvraag heeft plaatsgevonden, zoals de pagina die ze al had geopend. |
| nonce |Vereist |Een waarde die is opgenomen in de aanvraag (gegenereerd door de app) die worden opgenomen in de resulterende ID-token als een claim. De app kunt vervolgens controleren of deze waarde token opnieuw afspelen aanvallen te verkleinen. De waarde is doorgaans een willekeurige unieke tekenreeks die kan worden gebruikt voor het identificeren van de oorsprong van de aanvraag. |
| p |Vereist |De gebruikersstroom die wordt uitgevoerd. Dit is de naam van een beleid dat is gemaakt in uw B2C-tenant. De waarde voor de stroom gebruikersnaam moet beginnen met `b2c\_1\_`. Meer informatie over beleidsregels en de [extensible gebruiker stroom framework](active-directory-b2c-reference-policies.md). |
| prompt |Optioneel |Het type van de interactie van de gebruiker die is vereist. De enige geldige waarde op dit moment is `login`, waardoor de gebruiker zijn referenties invoeren voor deze aanvraag. Eenmalige aanmelding wordt pas van kracht. |

Op dit moment wordt de gebruiker gevraagd om de werkstroom van de gebruikersstroom te voltooien. Dit kan betrekking hebben op de voeren hun gebruikersnaam en wachtwoord, gebruiker zich aanmeldt met een sociale ID aanmelden voor de map of een andere aantal stappen, afhankelijk van hoe de gebruikersstroom wordt gedefinieerd.

Nadat de gebruiker is voltooid de gebruikersstroom, Azure AD retourneert een antwoord naar uw app op de aangegeven `redirect_uri` parameter, met behulp van de methode die is opgegeven in de `response_mode` parameter. Het antwoord is hetzelfde voor elk van de voorgaande gevallen, onafhankelijk van de gebruikersstroom die wordt uitgevoerd.

Een geslaagde respons met `response_mode=fragment` zou er als volgt uitzien:

```
GET https://aadb2cplayground.azurewebsites.net/#
id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parameter | Beschrijving |
| --- | --- |
| id_token |De ID-token dat de app worden aangevraagd. U kunt de ID-token gebruiken om te controleren of de identiteit van de gebruiker en beginnen met een sessie met de gebruiker. Meer informatie over ID-tokens en de inhoud ervan zijn opgenomen in de [naslaginformatie over Azure AD B2C-tokens](active-directory-b2c-reference-tokens.md). |
| code |De autorisatiecode dat de app is aangevraagd, als u hebt gebruikt `response_type=code+id_token`. De app kan de autorisatiecode gebruiken om aan te vragen van een toegangstoken voor een doelbron. Er worden autorisatiecodes zeer eenvoudige. Normaal gesproken verloopt deze na ongeveer 10 minuten. |
| state |Als een `state` parameter is opgenomen in de aanvraag, dezelfde waarde moet worden weergegeven in het antwoord. De app moet controleren of de `state` waarden in de aanvraag en respons identiek zijn. |

Foutberichten kunnen ook worden verzonden naar de `redirect_uri` parameter zodat de app deze op de juiste wijze kan verwerken:

```
GET https://aadb2cplayground.azurewebsites.net/#
error=access_denied
&error_description=the+user+canceled+the+authentication
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parameter | Beschrijving |
| --- | --- |
| error |Een foutcode tekenreeks die kan worden gebruikt voor het classificeren van typen fouten die optreden en die kan worden gebruikt om te reageren op fouten. |
| error_description |Een specifieke foutbericht dat een ontwikkelaar kan helpen de hoofdoorzaak van een verificatiefout identificeren. |
| state |Zie de volledige beschrijving van de eerste tabel in deze sectie. Als een `state` parameter is opgenomen in de aanvraag, dezelfde waarde moet worden weergegeven in het antwoord. De app moet controleren of de `state` waarden in de aanvraag en respons identiek zijn. |

## <a name="validate-the-id-token"></a>De ID-token te valideren
Zojuist hebt ontvangen van een ID-token is niet voldoende om de gebruiker te verifiëren. U moet de ID-token handtekening valideren en controleer of de claims in het token per vereisten van uw app. Maakt gebruik van Azure AD B2C [JSON Web Tokens (JWTs)](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) en cryptografie met openbare sleutels voor het ondertekenen van tokens en controleren of ze geldig zijn.

Er zijn veel open source-bibliotheken die beschikbaar zijn voor het valideren van JWTs, afhankelijk van de taal van voorkeur. U wordt aangeraden deze opties verkennen in plaats van uw eigen validatielogica implementeren. De informatie hier is nuttig bij het bepalen van het correct gebruik van deze bibliotheken.

Azure AD B2C is een eindpunt metagegevens OpenID Connect, waarmee een app voor het ophalen van informatie over Azure AD B2C tijdens runtime. Deze informatie omvat eindpunten, de inhoud van tokens en voor token-ondertekening van sleutels. Er is een document met de JSON-metagegevens voor elke gebruikersstroom in uw B2C-tenant. Bijvoorbeeld, het document met metagegevens voor de `b2c_1_sign_in` gebruikersstroom in `fabrikamb2c.onmicrosoft.com` bevindt zich in:

`https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in`

Een van de eigenschappen van dit configuratiedocument is `jwks_uri`, waarvan de waarde voor dezelfde gebruikersstroom zou zijn:

`https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in`.

Om te bepalen welke gebruikersstroom is gebruikt in een ID voor ondertekening van tokens (en waar om op te halen de metagegevens), hebt u twee opties. Eerst, de gebruikersnaam van de stroom is opgenomen in de `acr` claim in het ID-token. Zie voor meer informatie over het parseren van de claims van een ID-token dat de [naslaginformatie over Azure AD B2C-tokens](active-directory-b2c-reference-tokens.md). De andere mogelijkheid is het coderen van de gebruikersstroom in de waarde van de `state` parameter wanneer u de aanvraag uitgeven, en vervolgens worden ontsleuteld om te controleren welke gebruikersstroom is gebruikt. Een van beide methoden is geldig.

Nadat het document met metagegevens van het eindpunt van de OpenID Connect metagegevens die u hebt gekocht, kunt u de openbare RSA-256-sleutels (die zich op dit eindpunt) gebruiken voor het valideren van de handtekening van de ID-token. Er zijn mogelijk meerdere sleutels die worden vermeld op dit eindpunt op een willekeurig moment in-time, elk geïdentificeerd door een `kid` claim. De koptekst van de ID-token bevat ook een `kid` claim, waarmee wordt aangegeven welke van deze sleutels is gebruikt voor het ondertekenen van de ID-token. Zie voor meer informatie de [naslaginformatie over Azure AD B2C-tokens](active-directory-b2c-reference-tokens.md) (in de sectie op [valideren van tokens](active-directory-b2c-reference-tokens.md#token-validation), met name).
<!--TODO: Improve the information on this-->

Nadat u de handtekening van de ID-token hebt gevalideerd, zijn er verschillende claims die u nodig hebt om te controleren. Bijvoorbeeld:

* U moet controleren of de `nonce` claim om token opnieuw afspelen aanvallen te voorkomen. De waarde is wat u hebt opgegeven in de aanmeldingsaanvraag.
* U moet controleren of de `aud` claim om ervoor te zorgen dat de ID-token dat is uitgegeven voor uw app. De waarde moet de toepassings-ID van uw app.
* U moet controleren of de `iat` en `exp` claims om ervoor te zorgen dat de ID-token niet is verlopen.

Er zijn ook verschillende meer validaties die moeten worden uitgevoerd. Deze worden beschreven in de [OpenID verbinding maken met Core Spec](https://openid.net/specs/openid-connect-core-1_0.html).  U kunt ook om aanvullende claims, afhankelijk van uw scenario te valideren. Sommige algemene validaties zijn onder andere:

* Ervoor zorgen dat de gebruiker/organisatie zich heeft aangemeld voor de app.
* Ervoor zorgen dat de gebruiker juiste autorisatie/bevoegdheden heeft.
* Ervoor zorgen dat een bepaalde sterkte van verificatie heeft plaatsgevonden, zoals Azure multi-factor Authentication.

Zie voor meer informatie over de claims in een ID-token dat de [naslaginformatie over Azure AD B2C-tokens](active-directory-b2c-reference-tokens.md).

Nadat u de ID-token hebt gevalideerd, kunt u een sessie begint met de gebruiker. U kunt de claims in het ID-token gebruiken om informatie te verkrijgen over de gebruiker in uw app. Toepassingen voor deze informatie zijn onder andere weergeven, records en autorisatie.

## <a name="get-a-token"></a>Een token verkrijgen
Als u uw web-app uit te voeren alleen gebruikersstromen nodig hebt, kunt u de volgende gedeelten overslaan. Deze secties zijn alleen van toepassing op web-apps die u wilt aanbrengen geverifieerde aanroepen naar een web-API en ook worden beveiligd door Azure AD B2C.

De autorisatiecode die u hebt aangeschaft, kun je (met behulp van `response_type=code+id_token`) voor een token naar de gewenste resource door te sturen een `POST` aanvragen naar de `/token` eindpunt. Op dit moment is de enige bron die u kunt een token voor aanvragen uw web-API van de back-end in de app. De overeenkomst voor het aanvragen van een token aan uzelf is het gebruik van client-ID van uw app als het bereik:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://fabrikamb2c.b2clogin.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>

```

| Parameter | Vereist? | Beschrijving |
| --- | --- | --- |
| p |Vereist |De gebruikersstroom die is gebruikt voor het verkrijgen van de autorisatiecode. U kunt een andere gebruikersstroom niet gebruiken in deze aanvraag. Houd er rekening mee dat u deze parameter aan toevoegen de queryreeks, niet op de `POST` hoofdtekst. |
| client_id |Vereist |De aanvraag-ID die de [Azure-portal](https://portal.azure.com/) toegewezen aan uw app. |
| grant_type |Vereist |Het type van de toekenning, die moet worden `authorization_code` voor de autorisatiecodestroom. |
| scope |Aanbevolen |Een door spaties gescheiden lijst met bereiken. Een waarde één scope geeft u aan Azure AD beide machtigingen die worden aangevraagd. De `openid` bereik geeft aan dat een machtiging om te melden bij de gebruiker en gegevens over de gebruiker in de vorm van id_token parameters. Het kan worden gebruikt voor het ophalen van tokens aan uw app eigen back-end web-API, die wordt vertegenwoordigd door de dezelfde toepassings-ID als de client. De `offline_access` bereik geeft aan dat uw app een vernieuwingstoken voor lange levensduur hebben toegang tot bronnen moet. |
| code |Vereist |De autorisatiecode die u hebt verkregen in de eerste zijde van de stroom. |
| redirect_uri |Vereist |De `redirect_uri` parameter van de toepassing waar u de autorisatiecode ontvangen. |
| client_secret |Vereist |Het toepassingsgeheim van de die u hebt gegenereerd in de [Azure-portal](https://portal.azure.com/). Dit toepassingsgeheim is een belangrijke artefact. U moet ze veilig opslaan op uw server. U moet ook deze clientgeheim op periodieke basis draaien. |

Een geslaagde respons token ziet eruit zoals:

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
| not_before |De tijd waarop het token wordt beschouwd als geldig is, in epoche-tijd. |
| token_type |De waarde van het type token. Het enige type dat Azure AD ondersteunt `Bearer`. |
| access_token |De ondertekende JWT-token dat u hebt aangevraagd. |
| scope |De bereiken waarvoor het token geldig is. Deze kunnen worden gebruikt voor het opslaan van tokens voor later gebruik. |
| expires_in |De hoeveelheid tijd die het toegangstoken ongeldig (in seconden is). |
| refresh_token |Een vernieuwingstoken OAuth 2.0. De app kunt dit token gebruiken om aanvullende tokens verkrijgen nadat het huidige token is verlopen. Vernieuwen van tokens worden lange levensduur hebben en kan worden gebruikt voor toegang tot resources behouden gedurende langere tijd wordt opgelost. Raadpleeg voor meer informatie de [naslaginformatie over B2C-tokens](active-directory-b2c-reference-tokens.md). Houd er rekening mee dat u moet hebben gebruikt het bereik `offline_access` in de autorisatie en token aanvragen om te kunnen ontvangen van een vernieuwingstoken. |

Foutberichten er als volgt uitzien:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parameter | Beschrijving |
| --- | --- |
| error |Een foutcode tekenreeks die kan worden gebruikt voor het classificeren van typen fouten die optreden en die kan worden gebruikt om te reageren op fouten. |
| error_description |Een specifieke foutbericht dat een ontwikkelaar kan helpen de hoofdoorzaak van een verificatiefout identificeren. |

## <a name="use-the-token"></a>Het token gebruiken
Nu dat u hebt een toegangstoken is verkregen, kunt u het token in aanvragen naar uw back-end web-API's door op te nemen in de `Authorization` header:

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-the-token"></a>Vernieuwen van het token
ID-tokens zijn tijdelijke. U moet ze vernieuwen nadat ze zijn verlopen om door te gaan die wordt toegang tot bronnen. U kunt dit doen door het indienen van een andere `POST` aanvragen naar de `/token` eindpunt. Dit moment bieden de `refresh_token` parameter in plaats van de `code` parameter:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://fabrikamb2c.b2clogin.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=openid offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>
```

| Parameter | Vereist | Beschrijving |
| --- | --- | --- |
| p |Vereist |De gebruikersstroom die is gebruikt om de oorspronkelijke vernieuwingstoken te verkrijgen. U kunt een andere gebruikersstroom niet gebruiken in deze aanvraag. Houd er rekening mee dat u deze parameter aan de query-tekenreeks, niet aan de hoofdtekst van het bericht toevoegt. |
| client_id |Vereist |De aanvraag-ID die de [Azure-portal](https://portal.azure.com/) toegewezen aan uw app. |
| grant_type |Vereist |Het type van de toekenning, dit een vernieuwingstoken voor deze kant van de autorisatiecodestroom moet. |
| scope |Aanbevolen |Een door spaties gescheiden lijst met bereiken. Een waarde één scope geeft u aan Azure AD beide machtigingen die worden aangevraagd. De `openid` bereik geeft aan dat een machtiging om te melden bij de gebruiker en gegevens over de gebruiker in de vorm van ID-tokens ophalen. Het kan worden gebruikt voor het ophalen van tokens aan uw app eigen back-end web-API, die wordt vertegenwoordigd door de dezelfde toepassings-ID als de client. De `offline_access` bereik geeft aan dat uw app een vernieuwingstoken voor lange levensduur hebben toegang tot bronnen moet. |
| redirect_uri |Aanbevolen |De `redirect_uri` parameter van de toepassing waar u de autorisatiecode ontvangen. |
| refresh_token |Vereist |De oorspronkelijke vernieuwingstoken die u hebt verkregen in het tweede gedeelte van de stroom. Houd er rekening mee dat u moet hebben gebruikt het bereik `offline_access` in de autorisatie en token aanvragen om te kunnen ontvangen van een vernieuwingstoken. |
| client_secret |Vereist |Het toepassingsgeheim van de die u hebt gegenereerd in de [Azure-portal](https://portal.azure.com/). Dit toepassingsgeheim is een belangrijke artefact. U moet ze veilig opslaan op uw server. U moet ook deze clientgeheim op periodieke basis draaien. |

Een geslaagde respons token ziet eruit zoals:

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
| not_before |De tijd waarop het token wordt beschouwd als geldig is, in epoche-tijd. |
| token_type |De waarde van het type token. Het enige type dat Azure AD ondersteunt `Bearer`. |
| access_token |De ondertekende JWT-token dat u hebt aangevraagd. |
| scope |Het adresbereik op dat het token is ongeldig voor, die kan worden gebruikt voor het opslaan van tokens voor later gebruik. |
| expires_in |De hoeveelheid tijd die het toegangstoken ongeldig (in seconden is). |
| refresh_token |Een vernieuwingstoken OAuth 2.0. De app kunt dit token gebruiken om aanvullende tokens verkrijgen nadat het huidige token is verlopen.  Vernieuwen van tokens worden lange levensduur hebben en kan worden gebruikt voor toegang tot resources behouden gedurende langere tijd wordt opgelost. Voor meer informatie raadpleegt u de [naslaginformatie over B2C-tokens](active-directory-b2c-reference-tokens.md). |

Foutberichten er als volgt uitzien:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parameter | Beschrijving |
| --- | --- |
| error |Een foutcode tekenreeks die kan worden gebruikt voor het classificeren van typen fouten die optreden en die kan worden gebruikt om te reageren op fouten. |
| error_description |Een specifieke foutbericht dat een ontwikkelaar kan helpen de hoofdoorzaak van een verificatiefout identificeren. |

## <a name="send-a-sign-out-request"></a>Verzenden van een aanvragen voor afmelden
Als u de gebruiker buiten de app aanmelden wilt, is het niet voldoende is om te wissen van de cookies of anderszins einde van uw app de sessie met de gebruiker. U moet de gebruiker ook omleiden naar Azure AD zich afmelden. Als u niet om dit te doen, is het mogelijk dat de gebruiker kunnen andere referenties voor uw app zonder hun referenties opnieuw in te voeren. Dit is omdat ze een geldig aanmeldings-sessie met Azure AD.

U kunt gewoon de gebruiker omleiden de `end_session` eindpunt dat wordt weergegeven in het metagegevensdocument voor OpenID Connect beschreven sectie eerder in de 'valideren van de ID-token":

```
GET https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/logout?
p=b2c_1_sign_in
&post_logout_redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
```

| Parameter | Vereist? | Beschrijving |
| --- | --- | --- |
| p |Vereist |De gebruikersstroom die u wilt gebruiken voor het ondertekenen van de gebruiker buiten uw toepassing. |
| post_logout_redirect_uri |Aanbevolen |De URL die de gebruiker moet worden omgeleid naar na geslaagde afmelden. Als deze niet opgenomen is, Azure AD B2C wordt de gebruiker een algemene weergegeven. |

> [!NOTE]
> Hoewel de gebruiker om te leiden de `end_session` enkele van de status van de gebruiker eenmalige aanmelding met Azure AD B2C, eindpunt worden gewist, wordt de gebruiker buiten de sessie van hun sociale id-provider (IDP) niet ondertekent. Als de gebruiker de dezelfde IDP tijdens een volgende aanmelding selecteert, ze zal opnieuw worden geverifieerd, zonder hun referenties in te voeren. Als een gebruiker wil afmelden bij uw B2C-toepassing, komt dit niet noodzakelijkerwijs dat ze willen afmelden bij hun Facebook-account. Echter, in het geval van lokale accounts de gebruikerssessie wordt beëindigd goed.
> 
> 

## <a name="use-your-own-b2c-tenant"></a>Gebruik uw eigen B2C-tenant
Als u deze aanvragen zelf wilt proberen wilt, moet u eerst deze drie stappen uitvoeren en vervang de voorbeeldwaarden door uw eigen eerder is beschreven:

1. [Een B2C-tenant maken](active-directory-b2c-get-started.md), en gebruik de naam van uw tenant in de aanvragen.
2. [Maken van een toepassing](active-directory-b2c-app-registration.md) verkrijgen van een toepassings-ID. Een web-app/web-API in uw app opnemen. (Optioneel) Maak een toepassingsgeheim.
3. [Maken van uw gebruikersstromen](active-directory-b2c-reference-policies.md) om op te halen van de gebruiker namen van de stroom.

