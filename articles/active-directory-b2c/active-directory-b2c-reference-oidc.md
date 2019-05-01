---
title: Web-aanmelding met OpenID Connect - Azure Active Directory B2C | Microsoft Docs
description: Bouw webtoepassingen met behulp van de OpenID Connect-verificatieprotocol in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 4137360fadab0206c6569b58d6a9a0519ce74450
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64703939"
---
# <a name="web-sign-in-with-openid-connect-in-azure-active-directory-b2c"></a>Web-aanmelding met OpenID verbinding maken in Azure Active Directory B2C

OpenID Connect is een protocol voor verificatie, gebouwd op OAuth 2.0, die kan worden gebruikt om veilig gebruikers zich aanmelden voor webtoepassingen. Met behulp van de Azure Active Directory B2C (Azure AD B2C)-implementatie van OpenID Connect, u kunt uitbesteden gebruikersregistratie, aanmelding en andere identiteitsbeheer in uw web-apps aan Azure Active Directory (Azure AD). Deze handleiding wordt beschreven hoe u dit niet doet, op een taalonafhankelijke manier. Dit wordt beschreven hoe u berichten verzenden en ontvangen HTTP zonder onze open source-bibliotheken.

[OpenID Connect](https://openid.net/specs/openid-connect-core-1_0.html) kunt u de OAuth 2.0 uitbreiden *autorisatie* protocol voor gebruik als een *verificatie* protocol. Deze verificatieprotocol Hiermee kunt u eenmalige aanmelding. Dit introduceert het concept van een *ID-token*, waarmee de client om te controleren of de identiteit van de gebruiker en basisprofielgegevens informatie over de gebruiker te verkrijgen.

Omdat deze kunt u OAuth 2.0 uitbreiden, kan deze ook toepassingen aan te schaffen veilig *toegangstokens*. U kunt toegangstokens gebruiken voor toegang tot resources die worden beveiligd door een [autorisatieserver](active-directory-b2c-reference-protocols.md). OpenID Connect wordt aanbevolen als u een webtoepassing die worden gehost op een server en gebruikt via een browser bouwt. Als u wilt toevoegen van identiteitsbeheer naar uw mobiele of bureaubladtoepassingen met behulp van Azure AD B2C, moet u [OAuth 2.0](active-directory-b2c-reference-oauth-code.md) in plaats van de OpenID Connect. Zie voor meer informatie over tokens, de [overzicht van de tokens in Azure Active Directory B2C](active-directory-b2c-reference-tokens.md)

Azure AD B2C breidt het standaard OpenID Connect-protocol om te doen meer dan een eenvoudige verificatie en autorisatie. Het geeft de [gebruiker stroom parameter](active-directory-b2c-reference-policies.md), ervaringen waarmee u kunt gebruik van OpenID Connect-gebruiker toevoegen aan uw toepassing, zoals aanmelden, aanmelding en Profielbeheer.

## <a name="send-authentication-requests"></a>Verzenden van aanvragen voor authenticatie

Wanneer uw webtoepassing moet de gebruiker verifiëren en uitvoeren van de gebruikersstroom van een, het kunt instellen dat de gebruiker de `/authorize` eindpunt. De gebruiker maakt een actie afhankelijk van de gebruikersstroom.

In deze aanvraag, de client geeft aan dat de machtigingen die nodig is om te verkrijgen van de gebruiker in de `scope` parameter en de gebruikersstroom moet worden uitgevoerd de `p` parameter. Drie voorbeelden vindt u in de volgende secties (met regeleinden voor de leesbaarheid), elk met behulp van de stroom van een andere gebruiker. Ophalen van een idee van hoe elke aanvraag werkt, probeer de aanvraag in een browser plakken en uitvoeren. U kunt vervangen `fabrikamb2c` met de naam van uw tenant als u hebt en een beleid hebt gemaakt.

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

| Parameter | Vereist | Description |
| --------- | -------- | ----------- |
| client_id | Ja | De aanvraag-ID die de [Azure-portal](https://portal.azure.com/) toegewezen aan uw toepassing. |
| response_type | Ja | Moet een ID-token bevatten voor de OpenID Connect. Als uw webtoepassing ook tokens moet voor het aanroepen van een web-API, kunt u `code+id_token`. |
| redirect_uri | Nee | De `redirect_uri` parameter van uw toepassing, waarbij verificatiereacties kunnen worden verzonden en ontvangen door uw toepassing. Het moet exact overeenkomen met een van de `redirect_uri` parameters die u hebt geregistreerd in Azure portal, behalve dat het moet een URL-codering. |
| scope | Ja | Een door spaties gescheiden lijst met bereiken. De `openid` bereik geeft aan dat een machtiging om te melden bij de gebruiker en gegevens over de gebruiker in de vorm van ID-tokens ophalen. De `offline_access` bereik is optioneel voor webtoepassingen. Hiermee wordt aangegeven dat uw toepassing moet een *vernieuwingstoken* voor uitgebreide toegang tot bronnen. |
| response_mode | Nee | De methode die wordt gebruikt voor het verzenden van de resulterende autorisatiecode terug naar uw toepassing. Het kan zijn `query`, `form_post`, of `fragment`.  De `form_post` antwoordmodus wordt aanbevolen voor de beste beveiliging. |
| state | Nee | Een waarde die is opgenomen in de aanvraag die ook in het token antwoord wordt geretourneerd. Een tekenreeks van de inhoud die u wilt dat kan zijn. Een willekeurig gegenereerde unieke waarde wordt meestal gebruikt voor aanvallen van aanvraag voor cross-site kunnen worden vervalst. De status wordt ook gebruikt voor het coderen van informatie over de status van de gebruiker in de toepassing voordat de verificatieaanvraag heeft plaatsgevonden, zoals de pagina die ze al had geopend. |
| nonce | Ja | Een waarde die is opgenomen in de aanvraag (gegenereerd door de toepassing) die is opgenomen in de resulterende ID-token als een claim. De toepassing kunt vervolgens controleren of deze waarde token opnieuw afspelen aanvallen te verkleinen. De waarde is doorgaans een willekeurige unieke tekenreeks die kan worden gebruikt voor het identificeren van de oorsprong van de aanvraag. |
| p | Ja | De gebruikersstroom die wordt uitgevoerd. Dit is de naam van een beleid dat gemaakt in uw Azure AD B2C-tenant. De naam van de gebruikersstroom moet beginnen met `b2c\_1\_`. |
| prompt | Nee | Het type van de interactie van de gebruiker die is vereist. De enige geldige waarde op dit moment is `login`, waardoor de gebruiker zijn referenties invoeren voor deze aanvraag. |

Op dit moment wordt de gebruiker gevraagd om de werkstroom te voltooien. De gebruiker kan hebben om zich aan met een sociale id of meld u aanmelden voor de directory in te voeren hun gebruikersnaam en wachtwoord. Er zijn mogelijk andere aantal stappen, afhankelijk van hoe de gebruikersstroom wordt gedefinieerd.

Nadat de gebruiker is voltooid de gebruikersstroom, een antwoord wordt geretourneerd naar uw toepassing vanaf de aangegeven `redirect_uri` parameter, met behulp van de methode die opgegeven in de `response_mode` parameter. Het antwoord is hetzelfde voor elk van de voorgaande gevallen die onafhankelijk is van de gebruikersstroom.

Een geslaagde respons met `response_mode=fragment` zou er als volgt uitzien:

```
GET https://aadb2cplayground.azurewebsites.net/#
id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parameter | Description |
| --------- | ----------- |
| id_token | De ID-token dat de toepassing worden aangevraagd. U kunt de ID-token gebruiken om te controleren of de identiteit van de gebruiker en beginnen met een sessie met de gebruiker. |
| code | De autorisatiecode die de toepassing worden aangevraagd, als u hebt gebruikt `response_type=code+id_token`. De toepassing kan de autorisatiecode gebruiken om aan te vragen van een toegangstoken voor een doelbron. Autorisatiecodes verlopen doorgaans na ongeveer 10 minuten. |
| state | Als een `state` parameter is opgenomen in de aanvraag, dezelfde waarde moet worden weergegeven in het antwoord. De toepassing moet controleren of de `state` waarden in de aanvraag en respons identiek zijn. |

Foutberichten kunnen ook worden verzonden naar de `redirect_uri` parameter zodat de toepassing deze op de juiste wijze kan verwerken:

```
GET https://aadb2cplayground.azurewebsites.net/#
error=access_denied
&error_description=the+user+canceled+the+authentication
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parameter | Description |
| --------- | ----------- |
| error | Een code die kan worden gebruikt voor het classificeren van de typen fouten die optreden. |
| error_description | Een specifiek foutbericht die kan helpen bij het identificeren van de hoofdoorzaak van een verificatiefout. |
| state | Als een `state` parameter is opgenomen in de aanvraag, dezelfde waarde moet worden weergegeven in het antwoord. De toepassing moet controleren of de `state` waarden in de aanvraag en respons identiek zijn. |

## <a name="validate-the-id-token"></a>De ID-token te valideren

Zojuist hebt ontvangen van een ID-token is niet voldoende om de gebruiker te verifiëren. De ID-token handtekening valideren en controleer of de claims in het token per vereisten van uw toepassing. Maakt gebruik van Azure AD B2C [JSON Web Tokens (JWTs)](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) en cryptografie met openbare sleutels voor het ondertekenen van tokens en controleren of ze geldig zijn. Er zijn veel open source-bibliotheken die beschikbaar zijn voor het valideren van JWTs, afhankelijk van de taal van voorkeur. U wordt aangeraden deze opties verkennen in plaats van uw eigen validatielogica implementeren. 

Azure AD B2C is een eindpunt metagegevens OpenID Connect, waarmee een toepassing voor informatie over Azure AD B2C tijdens runtime. Deze informatie omvat eindpunten, de inhoud van tokens en voor token-ondertekening van sleutels. Er is een document met de JSON-metagegevens voor elke gebruikersstroom in uw B2C-tenant. Bijvoorbeeld, het document met metagegevens voor de `b2c_1_sign_in` gebruikersstroom in `fabrikamb2c.onmicrosoft.com` bevindt zich in:

`https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in`

Een van de eigenschappen van dit configuratiedocument is `jwks_uri`, waarvan de waarde voor dezelfde gebruikersstroom zou zijn:

`https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in`.

Om te bepalen welke gebruikersstroom is gebruikt in een ID voor ondertekening van tokens (en waar om op te halen de metagegevens), hebt u twee opties. Eerst, de gebruikersnaam van de stroom is opgenomen in de `acr` claim in het ID-token. De andere mogelijkheid is het coderen van de gebruikersstroom in de waarde van de `state` parameter wanneer u de aanvraag uitgeven, en vervolgens worden ontsleuteld om te controleren welke gebruikersstroom is gebruikt. Een van beide methoden is geldig.

Nadat het document met metagegevens van het eindpunt van de OpenID Connect metagegevens die u hebt gekocht, kunt u de openbare RSA-256-sleutels voor het valideren van de handtekening van de ID-token. Er zijn mogelijk meerdere sleutels die worden vermeld op dit eindpunt, elk geïdentificeerd door een `kid` claim. De koptekst van de ID-token bevat ook een `kid` claim, waarmee wordt aangegeven welke van deze sleutels is gebruikt voor het ondertekenen van de ID-token.

Nadat u de handtekening van de ID-token hebt gevalideerd, zijn er verschillende claims die u nodig hebt om te controleren. Bijvoorbeeld:

- Valideer de `nonce` claim om token opnieuw afspelen aanvallen te voorkomen. De waarde is wat u hebt opgegeven in de aanmeldingsaanvraag.
- Valideer de `aud` claim om ervoor te zorgen dat de ID-token dat is uitgegeven voor uw toepassing. De waarde moet de toepassings-ID van uw toepassing.
- Valideer de `iat` en `exp` claims om ervoor te zorgen dat de ID-token dat nog niet is verlopen.

Er zijn ook verschillende meer validaties die moeten worden uitgevoerd. De validaties worden beschreven in de [OpenID verbinding maken met Core Spec](https://openid.net/specs/openid-connect-core-1_0.html). U kunt ook om aanvullende claims, afhankelijk van uw scenario te valideren. Sommige algemene validaties zijn onder andere:

- Ervoor zorgen dat de gebruiker/organisatie zich heeft aangemeld voor de toepassing.
- Ervoor zorgen dat de gebruiker juiste autorisatie/bevoegdheden heeft.
- Ervoor zorgen dat een bepaalde sterkte van verificatie heeft plaatsgevonden, zoals Azure multi-factor Authentication.

Nadat u de ID-token hebt gevalideerd, kunt u een sessie beginnen met de gebruiker. U kunt de claims in het ID-token gebruiken om informatie te verkrijgen over de gebruiker in uw toepassing. Toepassingen voor deze informatie zijn onder andere weergeven, records en autorisatie.

## <a name="get-a-token"></a>Een token verkrijgen

Als u uw webtoepassing met gebruikersstromen alleen worden uitgevoerd, kunt u de volgende gedeelten overslaan. Deze secties zijn alleen van toepassing op toepassingen die u wilt aanbrengen geverifieerde aanroepen naar een web-API en ook worden beveiligd door Azure AD B2C.

De autorisatiecode die u hebt aangeschaft, kun je (met behulp van `response_type=code+id_token`) voor een token naar de gewenste resource door te sturen een `POST` aanvragen naar de `/token` eindpunt. Op dit moment is de enige bron die u kunt een token voor aanvragen uw web-API van de back-end in de toepassing. De overeenkomst voor het aanvragen van een token aan uzelf is het gebruik van de client-ID van uw toepassing als het bereik:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://fabrikamb2c.b2clogin.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>
```

| Parameter | Vereist | Description |
| --------- | -------- | ----------- |
| p | Ja | De gebruikersstroom die is gebruikt voor het verkrijgen van de autorisatiecode. U kunt een andere gebruikersstroom niet gebruiken in deze aanvraag. Deze parameter toevoegen aan de query-tekenreeks, niet aan de hoofdtekst van het bericht. |
| client_id | Ja | De aanvraag-ID die de [Azure-portal](https://portal.azure.com/) toegewezen aan uw toepassing. |
| grant_type | Ja | Het type van de toekenning, die moet worden `authorization_code` voor de autorisatiecodestroom. |
| scope | Nee | Een door spaties gescheiden lijst met bereiken. De `openid` bereik geeft aan dat een machtiging om te melden bij de gebruiker en gegevens over de gebruiker in de vorm van id_token parameters. Het kan worden gebruikt voor het ophalen van tokens, voor uw toepassing uit eigen back-end web-API, die wordt vertegenwoordigd door de dezelfde toepassings-ID als de client. De `offline_access` bereik geeft aan dat uw toepassing een vernieuwingstoken voor uitgebreide toegang tot bronnen moet. |
| code | Ja | De autorisatiecode die u hebt verkregen in het begin van de gebruikersstroom. |
| redirect_uri | Ja | De `redirect_uri` parameter van de toepassing waar u de autorisatiecode ontvangen. |
| client_secret | Ja | Het toepassingsgeheim van de die is gegenereerd in de [Azure-portal](https://portal.azure.com/). Dit toepassingsgeheim is een belangrijke artefact. U moet ze veilig opslaan op uw server. Deze clientgeheim op periodieke basis wijzigen. |

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
| Parameter | Description |
| --------- | ----------- |
| not_before | De tijd waarop het token wordt beschouwd als geldig is, in epoche-tijd. |
| token_type | De waarde van het type token. `Bearer` is het enige type dat wordt ondersteund. |
| access_token | De ondertekende JWT-token dat u hebt aangevraagd. |
| scope | De bereiken waarvoor het token geldig is. |
| expires_in | De hoeveelheid tijd die het toegangstoken ongeldig (in seconden is). |
| refresh_token | Een vernieuwingstoken OAuth 2.0. De toepassing kunt dit token gebruiken om aanvullende tokens verkrijgen nadat het huidige token is verlopen. Vernieuwen van tokens kunnen worden gebruikt voor toegang tot resources behouden gedurende langere tijd wordt opgelost. Het bereik `offline_access` moet zijn gebruikt in de autorisatie en token aanvragen om te kunnen ontvangen van een vernieuwingstoken. |

Foutberichten er als volgt uitzien:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parameter | Description |
| --------- | ----------- |
| error | Een code die kan worden gebruikt voor het classificeren van typen fouten die optreden. |
| error_description | Een bericht die kan helpen bij het identificeren van de hoofdoorzaak van een verificatiefout. |

## <a name="use-the-token"></a>Het token gebruiken

Nu dat u hebt een toegangstoken is verkregen, kunt u het token in aanvragen naar uw back-end web-API's door op te nemen in de `Authorization` header:

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-the-token"></a>Vernieuwen van het token

ID-tokens verlopen in een korte periode. Vernieuw de tokens nadat ze zijn verlopen om door te gaan die wordt toegang tot bronnen. U kunt een token vernieuwen door het indienen van een andere `POST` aanvragen naar de `/token` eindpunt. Dit moment bieden de `refresh_token` parameter in plaats van de `code` parameter:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://fabrikamb2c.b2clogin.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=openid offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>
```

| Parameter | Vereist | Description |
| --------- | -------- | ----------- |
| p | Ja | De gebruikersstroom die is gebruikt om de oorspronkelijke vernieuwingstoken te verkrijgen. U kunt een andere gebruikersstroom niet gebruiken in deze aanvraag. Deze parameter toevoegen aan de query-tekenreeks, niet aan de hoofdtekst van het bericht. |
| client_id | Ja | De aanvraag-ID die de [Azure-portal](https://portal.azure.com/) toegewezen aan uw toepassing. |
| grant_type | Ja | Het type van de toekenning, dit een vernieuwingstoken voor dit gedeelte van de autorisatiecodestroom moet. |
| scope | Nee | Een door spaties gescheiden lijst met bereiken. De `openid` bereik geeft aan dat een machtiging om te melden bij de gebruiker en gegevens over de gebruiker in de vorm van ID-tokens ophalen. Het kan worden gebruikt voor het verzenden van tokens aan uw toepassing uit eigen back-end web-API, die wordt vertegenwoordigd door de dezelfde toepassings-ID als de client. De `offline_access` bereik geeft aan dat uw toepassing een vernieuwingstoken voor uitgebreide toegang tot bronnen moet. |
| redirect_uri | Nee | De `redirect_uri` parameter van de toepassing waar u de autorisatiecode ontvangen. |
| refresh_token | Ja | De oorspronkelijke vernieuwingstoken dat is verkregen in het tweede gedeelte van de stroom. De `offline_access` bereik moet worden gebruikt in de autorisatie en token aanvragen om te kunnen ontvangen van een vernieuwingstoken. |
| client_secret | Ja | Het toepassingsgeheim van de die is gegenereerd in de [Azure-portal](https://portal.azure.com/). Dit toepassingsgeheim is een belangrijke artefact. U moet ze veilig opslaan op uw server. Deze clientgeheim op periodieke basis wijzigen. |

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
| Parameter | Description |
| --------- | ----------- |
| not_before | De tijd waarop het token wordt beschouwd als geldig is, in epoche-tijd. |
| token_type | De waarde van het type token. `Bearer` is het enige type dat wordt ondersteund. |
| access_token | De ondertekende JWT-token dat is aangevraagd. |
| scope | Het bereik waarvoor het token geldig is. |
| expires_in | De hoeveelheid tijd die het toegangstoken ongeldig (in seconden is). |
| refresh_token | Een vernieuwingstoken OAuth 2.0. De toepassing kunt dit token gebruiken om aanvullende tokens verkrijgen nadat het huidige token is verlopen. Vernieuwen van tokens kunnen worden gebruikt voor toegang tot resources behouden gedurende langere tijd wordt opgelost. |

Foutberichten er als volgt uitzien:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parameter | Description |
| --------- | ----------- |
| error | Een code die kan worden gebruikt voor het classificeren van typen fouten die optreden. |
| error_description | Een bericht die kan helpen bij het identificeren van de hoofdoorzaak van een verificatiefout. |

## <a name="send-a-sign-out-request"></a>Verzenden van een aanvragen voor afmelden

Als u de gebruiker bij de App aanmelden wilt, is het niet genoeg om te wissen van cookies van de toepassing of de sessie met de gebruiker anders te beëindigen. De gebruiker omgeleid naar Azure AD B2C afmelden. Als u niet om dit te doen, is het mogelijk dat de gebruiker kunnen andere referenties voor uw toepassing zonder hun referenties opnieuw in te voeren.

U kunt gewoon de gebruiker omleiden de `end_session` eindpunt dat wordt vermeld in de metagegevens van de OpenID Connect document die eerder zijn beschreven:

```
GET https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/logout?
p=b2c_1_sign_in
&post_logout_redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
```

| Parameter | Vereist | Description |
| --------- | -------- | ----------- |
| p | Ja | De gebruikersstroom die u wilt gebruiken voor het ondertekenen van de gebruiker buiten uw toepassing. |
| post_logout_redirect_uri | Nee | De URL die de gebruiker moet worden omgeleid naar na geslaagde afmelden. Als deze niet opgenomen is, toont Azure AD B2C de gebruiker een algemeen foutbericht. |

De gebruiker om te leiden de `end_session` eindpunt wist enkele van de status van de gebruiker eenmalige aanmelding met Azure AD B2C, maar deze niet de gebruiker buiten de sessie van hun sociale id-provider (IDP) ondertekenen. Als de gebruiker de dezelfde IDP tijdens een volgende aanmelding selecteert, zijn ze geverifieerd, zonder hun referenties in te voeren. Als een gebruiker wil afmelden bij de toepassing, betekent dit niet noodzakelijkerwijs dat ze willen afmelden bij hun Facebook-account. Echter als lokale accounts worden gebruikt, de gebruikerssessie wordt beëindigd goed.

