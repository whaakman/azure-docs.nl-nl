---
title: Azure AD v2.0-OAuth-autorisatie Code stroom | Microsoft Docs
description: Building webtoepassingen die gebruikmaken van Azure AD-implementatie van het OAuth 2.0-verificatieprotocol.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: ae1d7d86-7098-468c-aa32-20df0a10ee3d
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: 13841e1ef6298ffc85f6ccf5cb2a60e5a9b992b3
ms.sourcegitcommit: 39397603c8534d3d0623ae4efbeca153df8ed791
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56099374"
---
# <a name="v20-protocols---oauth-20-authorization-code-flow"></a>v2.0-protocollen - OAuth 2.0-autorisatiecodestroom

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

De OAuth 2.0-autorisatiecode verlenen kan worden gebruikt in apps die zijn geïnstalleerd op een apparaat toegang te krijgen tot beveiligde bronnen, zoals web-API's. Met behulp van de app model v2.0-implementatie van OAuth 2.0, kunt u zich aanmelden en API-toegang tot uw mobiele en bureaublad-apps toevoegen. Deze handleiding is taalonafhankelijk en wordt beschreven hoe u berichten verzenden en ontvangen HTTP zonder gebruik van een van de [Azure open source-verificatiebibliotheken](active-directory-authentication-libraries.md).

> [!NOTE]
> Niet alle Azure Active Directory-scenario's en functies worden ondersteund door het v2.0-eindpunt. Meer informatie over om te bepalen als u het v2.0-eindpunt moet gebruiken, [v2.0 beperkingen](active-directory-v2-limitations.md).

De OAuth 2.0-autorisatiecodestroom wordt beschreven in [sectie 4.1 van de OAuth 2.0-specificatie](https://tools.ietf.org/html/rfc6749). Deze wordt gebruikt voor verificatie en autorisatie in het merendeel van de app-typen, met inbegrip van [web-apps](v2-app-types.md#web-apps) en [systeemeigen geïnstalleerde apps](v2-app-types.md#mobile-and-native-apps). De stroom kunt apps aan te schaffen veilig access_tokens die kunnen worden gebruikt voor toegang tot resources die zijn beveiligd door het v2.0-eindpunt. 

## <a name="protocol-diagram"></a>Protocol-diagram

Op hoog niveau, de gehele verificatiestroom voor een native/mobiele toepassing iets er zo uit:

![Stroom voor OAuth-autorisatiecode](./media/v2-oauth2-auth-code-flow/convergence_scenarios_native.png)

## <a name="request-an-authorization-code"></a>Aanvragen van een autorisatiecode

De autorisatiecodestroom begint met de client zodat de gebruiker de `/authorize` eindpunt. In deze aanvraag, de client geeft aan dat de machtigingen die nodig zijn om te verkrijgen van de gebruiker:

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fuser.read
&state=12345
```

> [!TIP]
> Klik op de onderstaande koppeling voor het uitvoeren van deze aanvraag. Na het aanmelden, moet uw browser worden omgeleid naar `https://localhost/myapp/` met een `code` in de adresbalk.
> <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=code&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&response_mode=query&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fuser.read&state=12345" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/authorize...</a>
> 
> 

| Parameter    | Vereist/optioneel | Description |
|--------------|-------------|--------------|
| `tenant`    | vereist    | De `{tenant}` waarde in het pad van de aanvraag kan worden gebruikt om te bepalen wie zich bij de toepassing aanmelden kan. De toegestane waarden zijn `common`, `organizations`, `consumers`, en tenant-id's. Zie voor meer details [protocol basisbeginselen](active-directory-v2-protocols.md#endpoints).  |
| `client_id`   | vereist    | De toepassing (client)-ID die de portal voor wachtwoordregistratie ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) of **(Preview) van de App-registraties** optreden in de Azure portal voor uw app is toegewezen.  |
| `response_type` | vereist    | Moet bevatten `code` voor de autorisatiecodestroom.       |
| `redirect_uri`  | Aanbevolen | De redirect_uri van uw app, waarbij verificatiereacties kunnen worden verzonden en ontvangen door uw app. Het moet een van de redirect_uris die u in de portal hebt geregistreerd, behalve het url-codering moet exact overeenkomen. Voor mobiele en systeemeigen apps, moet u de standaardwaarde van `https://login.microsoftonline.com/common/oauth2/nativeclient`.   |
| `scope`  | vereist    | Een door spaties gescheiden lijst van [scopes](v2-permissions-and-consent.md) dat u wilt dat de gebruiker toe te staan. |
| `response_mode`   | Aanbevolen | Hiermee geeft u de methode die moet worden gebruikt voor het verzenden van het resulterende token terug naar uw app. Een van de volgende kan zijn:<br/><br/>- `query`<br/>- `fragment`<br/>- `form_post`<br/><br/>`query` biedt de code als een queryreeks-parameter op uw omleidings-URI. Als u een ID-token met behulp van de impliciete stroom aanvragen, u niet gebruiken `query` zoals opgegeven in de [OpenID spec](https://openid.net/specs/oauth-v2-multiple-response-types-1_0.html#Combinations). Als u alleen de code aanvragen, kunt u `query`, `fragment`, of `form_post`. `form_post` voert een bericht met de code op uw omleidings-URI. Zie voor meer informatie, [OpenID Connect-protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code).  |
| `state`                 | Aanbevolen | Een waarde die is opgenomen in de aanvraag die wordt ook in het token antwoord geretourneerd. Een tekenreeks van de inhoud die u wenst dat kan zijn. Een willekeurig gegenereerde unieke waarde wordt meestal gebruikt voor [cross-site-aanvraag kunnen worden vervalst aanvallen](https://tools.ietf.org/html/rfc6749#section-10.12). De waarde kan ook informatie over de status van de gebruiker in de app coderen voordat de verificatieaanvraag heeft plaatsgevonden, zoals de pagina of de weergave die ze al had geopend. |
| `prompt`  | optioneel    | Geeft het type tussenkomst van de gebruiker die is vereist. De enige geldige waarden op dit moment zijn `login`, `none`, en `consent`.<br/><br/>- `prompt=login` wordt de gebruiker te dwingen zijn referenties invoeren voor deze aanvraag, zodat eenmalige aanmelding.<br/>- `prompt=none` is het tegenovergestelde - dit zorgt ervoor dat de gebruiker niet wordt weergegeven met een interactieve prompt dan ook. Als de aanvraag kan niet worden voltooid op de achtergrond via eenmalige aanmelding, het v2.0-eindpunt retourneert een `interaction_required` fout.<br/>- `prompt=consent` het dialoogvenster OAuth wordt worden geactiveerd nadat de gebruiker zich heeft aangemeld, waarin de gebruiker wordt om de app-machtigingen te verlenen. |
| `login_hint`  | optioneel    | Kan worden gebruikt om het veld gebruikersnaam, e-mailadres van de aanmeldingspagina voor de gebruiker vooraf worden ingevuld als u hun gebruikersnaam tevoren weten. Vaak apps Gebruik deze parameter tijdens hernieuwde verificatie de gebruikersnaam die al worden geëxtraheerd uit een vorige aanmelden met behulp van de `preferred_username` claim.   |
| `domain_hint`  | optioneel    | Een van `consumers` of `organizations`.<br/><br/>Als opgenomen, wordt overgeslagen tijdens de detectie op basis van een e-mailadres van die gebruiker doorloopt op de pagina v2.0 aanmelden leidt tot een iets meer gestroomlijnde gebruikerservaring. Vaak apps wordt deze parameter gebruikt tijdens hernieuwde verificatie door te extraheren de `tid` van een vorige aanmelding. Als de `tid` claim waarde `9188040d-6c67-4c5b-b112-36a304b66dad`, moet u `domain_hint=consumers`. Gebruik anders `domain_hint=organizations`.  |
| `code_challenge_method` | optioneel    | De methode die wordt gebruikt om te coderen de `code_verifier` voor de `code_challenge` parameter. Een van de volgende waarden kan zijn:<br/><br/>- `plain` <br/>- `S256`<br/><br/>Als uitgesloten, `code_challenge` wordt ervan uitgegaan dat als tekst zonder opmaak als `code_challenge` is opgenomen. Azure AAD v2.0 ondersteunt zowel `plain` en `S256`. Zie voor meer informatie de [PKCE RFC](https://tools.ietf.org/html/rfc7636). |
| `code_challenge`  | optioneel | Gebruikt voor het beveiligen van code autorisatietoekenningen via Proof-sleutel voor Code Exchange (PKCE) van een systeemeigen client. Vereist als `code_challenge_method` is opgenomen. Zie voor meer informatie de [PKCE RFC](https://tools.ietf.org/html/rfc7636). |

Op dit moment is de gebruiker gevraagd zijn referenties invoeren en de verificatie voltooien. Het v2.0-eindpunt wordt er ook voor zorgen dat de gebruiker heeft ingestemd met de machtigingen die zijn aangegeven in de `scope` queryparameter. Als de gebruiker heeft niet ingestemd met een van deze machtigingen, wordt u gevraagd de gebruiker akkoord gaan met de vereiste machtigingen. Details van [machtigingen en toestemming apps met meerdere tenants vindt u hier](v2-permissions-and-consent.md).

Zodra de gebruiker verifieert en toestemming verleent, het v2.0-eindpunt wordt een antwoord naar uw app op de aangegeven `redirect_uri`, met behulp van de methode die is opgegeven de `response_mode` parameter.

#### <a name="successful-response"></a>Geslaagde reactie

Een geslaagde respons met `response_mode=query` ziet eruit als:

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=12345
```

| Parameter | Description  |
|-----------|--------------|
| `code` | De authorization_code die de app heeft aangevraagd. De app kan de autorisatiecode gebruiken om aan te vragen van een toegangstoken voor de doelresource. Authorization_codes zijn zeer korte levensduur hebben, doorgaans deze verloopt na ongeveer 10 minuten. |
| `state` | Als een parameter state is opgenomen in de aanvraag, dezelfde waarde moet worden weergegeven in het antwoord. De app moet controleren of dat de provincie-waarden in de aanvraag en respons identiek zijn. |

#### <a name="error-response"></a>Foutbericht

Foutberichten kunnen ook worden verzonden naar de `redirect_uri` zodat de app deze op de juiste wijze kan verwerken:

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parameter | Description  |
|----------|------------------|
| `error`  | Een tekenreeks voor de foutcode die kan worden gebruikt voor het classificeren van typen fouten die optreden en kan worden gebruikt om te reageren op fouten. |
| `error_description` | Een specifieke foutbericht dat een ontwikkelaar kan helpen de hoofdoorzaak van een verificatiefout identificeren. |

#### <a name="error-codes-for-authorization-endpoint-errors"></a>Foutcodes voor endpoint-verificatiefouten

De volgende tabel beschrijft de verschillende foutcodes die kunnen worden geretourneerd in de `error` parameter van het foutbericht.

| Foutcode  | Description    | Clientactie   |
|-------------|----------------|-----------------|
| `invalid_request` | Protocolfout in, zoals een ontbrekende vereiste parameter. | Los en verzend de aanvraag opnieuw. Dit is een ontwikkeling fout meestal aangetroffen tijdens de eerste test. |
| `unauthorized_client` | De clienttoepassing is niet toegestaan om aan te vragen van een autorisatiecode. | Deze fout treedt meestal op wanneer de clienttoepassing is niet geregistreerd in Azure AD of is niet toegevoegd aan Azure AD-tenant van de gebruiker. De toepassing kan het bericht met instructies voor het installeren van de toepassing en toe te voegen aan Azure AD. |
| `access_denied`  | Resource-eigenaar toestemming geweigerd  | De clienttoepassing kan de gebruiker die deze kan niet worden voortgezet, tenzij de gebruiker toestemming heeft melden. |
| `unsupported_response_type` | De autorisatie-server biedt geen ondersteuning voor het antwoord van het type in de aanvraag. | Los en verzend de aanvraag opnieuw. Dit is een ontwikkeling fout wordt meestal onderschept tijdens de eerste test.  |
| `server_error`  | De server heeft een onverwachte fout aangetroffen.| De aanvraag opnieuw. Deze fouten kunnen worden veroorzaakt door tijdelijke omstandigheden. De clienttoepassing mogelijk uitleggen aan de gebruiker dat de reactie op een tijdelijke fout is vertraagd. |
| `temporarily_unavailable`   | De server is tijdelijk bezet en kan de aanvraag te verwerken. | De aanvraag opnieuw. De clienttoepassing mogelijk uitleggen aan de gebruiker dat de reactie is vertraagd vanwege een tijdelijke situatie. |
| `invalid_resource`  | De doelresource is ongeldig omdat deze niet bestaat, Azure AD kan deze niet vinden of deze is niet correct geconfigureerd. | Deze fout geeft aan dat de resource als deze bestaat, is niet geconfigureerd in de tenant. De toepassing kan het bericht met instructies voor het installeren van de toepassing en toe te voegen aan Azure AD. |
| `login_required` | Te veel of geen gebruikers gevonden | De client aangevraagde verificatie op de achtergrond (`prompt=none`), maar één gebruiker is niet gevonden. Dit kan betekenen dat er meerdere gebruikers actief zijn in de sessie, of als er geen gebruikers. Dit houdt rekening met de gekozen tenant (bijvoorbeeld, als er actieve 2 AAD-accounts en een MSA zijn en `consumers` is gekozen, verificatie op de achtergrond werkt). |
| `interaction_required` | De aanvraag gebruikersinteractie is vereist. | Een aanvullende stap of toestemming is vereist. Probeer de aanvraag zonder `prompt=none`. |

## <a name="request-an-access-token"></a>Aanvraag een toegangstoken

Nu dat u hebt verkregen een authorization_code en gemachtigd door de gebruiker, kun je de `code` voor een `access_token` naar de gewenste resource. Dit doen door het verzenden van een `POST` aanvragen naar de `/token` eindpunt:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fuser.read
&code=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq3n8b2JRLk4OxVXr...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=authorization_code
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```

> [!TIP]
> Probeer deze aanvraag wordt uitgevoerd in Postman. (Vergeet niet om te vervangen de `code`) [ ![uitvoeren in Postman](./media/v2-oauth2-auth-code-flow/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)

| Parameter  | Vereist/optioneel | Description     |
|------------|-------------------|----------------|
| `tenant`   | vereist   | De `{tenant}` waarde in het pad van de aanvraag kan worden gebruikt om te bepalen wie zich bij de toepassing aanmelden kan. De toegestane waarden zijn `common`, `organizations`, `consumers`, en tenant-id's. Zie voor meer details [protocol basisbeginselen](active-directory-v2-protocols.md#endpoints).  |
| `client_id` | vereist  | De aanvraag-ID die de portal voor wachtwoordregistratie ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) uw app is toegewezen. |
| `grant_type` | vereist   | Moet `authorization_code` voor de autorisatiecodestroom.   |
| `scope`      | vereist   | Een door spaties gescheiden lijst met bereiken. De bereiken die in dit gedeelte wordt aangevraagd moet gelijk aan of een subset van de bereiken die in de eerste arm wordt aangevraagd. Als de bereiken die is opgegeven in deze aanvraag meerdere resource-server omvatten, wordt een token voor de resource die is opgegeven in de eerste scope geretourneerd door het v2.0-eindpunt. Raadpleeg voor een meer gedetailleerde uitleg van bereiken, [machtigingen en toestemming scopes](v2-permissions-and-consent.md). |
| `code`          | vereist  | De authorization_code die u hebt verkregen in de eerste zijde van de stroom. |
| `redirect_uri`  | vereist  | Dezelfde redirect_uri-waarde die is gebruikt voor het verkrijgen van de authorization_code. |
| `client_secret` | vereist voor web-apps | Het toepassingsgeheim die u hebt gemaakt in de portal voor app-registratie voor uw app. Deze mag niet worden gebruikt in een systeemeigen app omdat client_secrets op betrouwbare wijze kunnen niet worden opgeslagen op apparaten. Dit is vereist voor de web-apps en web-API's, waarvoor de mogelijkheid om op te slaan de waarde voor client_secret veilig op de server.  Het clientgeheim moet URL gecodeerd voordat het wordt verzonden.  |
| `code_verifier` | optioneel  | De dezelfde code_verifier die is gebruikt voor het verkrijgen van de authorization_code. Vereist als PKCE is gebruikt in de autorisatieaanvraag voor het verlenen van code. Zie voor meer informatie de [PKCE RFC](https://tools.ietf.org/html/rfc7636). |

### <a name="successful-response"></a>Geslaagde reactie

Een geslaagde respons token ziet er als:

```json
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fuser.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```

| Parameter     | Description   |
|---------------|------------------------------|
| `access_token`  | Het aangevraagde toegangstoken. De app kan dit token gebruikt voor verificatie bij de beveiligde resource, zoals een web-API.  |
| `token_type`    | Geeft aan dat de waarde van het token. Het enige type die ondersteuning biedt voor Azure AD is Bearer |
|` expires_in`    | Hoe lang het toegangstoken is ongeldig (in seconden). |
| `scope`         | De bereiken die de access_token is geldig voor. |
| `refresh_token` | Een vernieuwingstoken OAuth 2.0. De app kan dit token gebruiken als u meer toegangstokens verkrijgen nadat het huidige toegangstoken is verlopen. Refresh_tokens worden lange levensduur hebben en kan worden gebruikt voor toegang tot resources behouden gedurende langere tijd wordt opgelost. Raadpleeg voor meer informatie over het vernieuwen van een toegangstoken de [hieronder](#refresh-the-access-token). <br> **Opmerking:** Alleen opgegeven als `offline_access` bereik is aangevraagd. |
| `id_token`      | Een JSON Webtoken (JWT). De app kan worden gedecodeerd de segmenten van dit token informatie opvragen over de gebruiker die zijn aangemeld. De app kan de waarden in de cache en deze weer te geven, maar deze moet niet gebruiken voor autorisatie of grenzen voor netwerkbeveiliging. Zie voor meer informatie over id_tokens, de [ `id_token reference` ](id-tokens.md). <br> **Opmerking:** Alleen opgegeven als `openid` bereik is aangevraagd. |

### <a name="error-response"></a>Foutbericht

Foutberichten ziet er als:

```json
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/mail.read is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
  "error_codes": [
    70011
  ],
  "timestamp": "2016-01-09 02:02:12Z",
  "trace_id": "255d1aef-8c98-452f-ac51-23d051240864",
  "correlation_id": "fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7"
}
```

| Parameter         | Description    |
|-------------------|----------------|
| `error`       | Een tekenreeks voor de foutcode die kan worden gebruikt voor het classificeren van typen fouten die optreden en kan worden gebruikt om te reageren op fouten. |
| `error_description` | Een specifieke foutbericht dat een ontwikkelaar kan helpen de hoofdoorzaak van een verificatiefout identificeren. |
| `error_codes` | Een lijst van de STS-specifieke foutcodes die bij het diagnostische gegevens helpen.  |
| `timestamp`   | De tijd waarop de fout is opgetreden. |
| `trace_id`    | Een unieke id voor de aanvraag die bij het diagnostische gegevens helpen. |
| `correlation_id` | Een unieke id voor de aanvraag die bij het diagnostische gegevens voor onderdelen helpen. |

### <a name="error-codes-for-token-endpoint-errors"></a>Foutcodes voor token-eindpunt-fouten

| Foutcode         | Description        | Clientactie    |
|--------------------|--------------------|------------------|
| `invalid_request`  | Protocolfout in, zoals een ontbrekende vereiste parameter. | Los en verzend de aanvraag opnieuw   |
| `invalid_grant`    | De autorisatiecode of het PKCE code controle is ongeldig of is verlopen. | Probeer een nieuwe aanvraag naar de `/authorize` eindpunt en controleer of de parameter code_verifier juist is.  |
| `unauthorized_client` | De geverifieerde client is niet gemachtigd voor het gebruik van deze toekenningstype autorisatie. | Dit gebeurt meestal wanneer de clienttoepassing is niet geregistreerd in Azure AD of is niet toegevoegd aan Azure AD-tenant van de gebruiker. De toepassing kan het bericht met instructies voor het installeren van de toepassing en toe te voegen aan Azure AD. |
| `invalid_client` | Clientverificatie is mislukt.  | De referenties van de client zijn niet geldig. De beheerder van de toepassing om op te lossen, worden de referenties bijgewerkt.   |
| `unsupported_grant_type` | De autorisatie-server biedt geen ondersteuning voor het toekenningstype autorisatie. | Wijzig het type verlenen in de aanvraag. Dit type fout moet worden uitgevoerd tijdens de ontwikkeling en worden gedetecteerd tijdens de eerste test. |
| `invalid_resource` | De doelresource is ongeldig omdat deze niet bestaat, Azure AD kan deze niet vinden of deze is niet correct geconfigureerd. | Dit geeft aan dat de resource als deze bestaat, is niet geconfigureerd in de tenant. De toepassing kan het bericht met instructies voor het installeren van de toepassing en toe te voegen aan Azure AD.  |
| `interaction_required` | De aanvraag gebruikersinteractie is vereist. Bijvoorbeeld, is een aanvullende stap vereist. | Probeer de aanvraag met dezelfde resource.  |
| `temporarily_unavailable` | De server is tijdelijk bezet en kan de aanvraag te verwerken. | De aanvraag opnieuw. De clienttoepassing mogelijk uitleggen aan de gebruiker dat de reactie is vertraagd vanwege een tijdelijke situatie. |

## <a name="use-the-access-token"></a>Het toegangstoken gebruiken

Nu dat u hebt gekregen een `access_token`, kunt u het token in aanvragen voor Web-API's door op te nemen in de `Authorization` header:

> [!TIP]
> Deze aanvraag in Postman uitvoeren! (Vervang de `Authorization` header eerste) [ ![uitvoeren in Postman](./media/v2-oauth2-auth-code-flow/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)

```
GET /v1.0/me/messages
Host: https://graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-the-access-token"></a>Vernieuwen van het toegangstoken

Access_tokens zijn korte duur en moet u deze vernieuwen nadat ze zijn verlopen om door te gaan met het openen van bronnen. U kunt dit doen door het indienen van een andere `POST` aanvragen naar de `/token` eindpunt, dit keer met de `refresh_token` in plaats van de `code`.  Vernieuwen van tokens geldig zijn voor alle machtigingen die de client al is toestemming geven voor - dus een vernieuwingstoken dat is uitgegeven voor een aanvraag voor `scope=mail.read` kan worden gebruikt om aan te vragen van een nieuw toegangstoken voor `scope=api://contoso.com/api/UseResource`.  

Vernieuwen van tokens geen opgegeven levensduur. De levensduur van vernieuwingstokens zijn meestal relatief lange. Echter, in sommige gevallen, vernieuwingstokens verlopen, worden ingetrokken of niet over voldoende bevoegdheden voor de gewenste actie. Uw toepassing moet verwachten en verwerken [fouten die zijn geretourneerd door het eindpunt voor token-uitgifte](#error-codes-for-token-endpoint-errors) correct.  Houd er rekening mee vernieuwingstokens worden niet ingetrokken wanneer het voor het nieuwe toegangstokens te verkrijgen. 

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fuser.read
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=refresh_token
&client_secret=JqQX2PNo9bpM0uEihUPzyrh      // NOTE: Only required for web apps
```

> [!TIP]
> Probeer deze aanvraag wordt uitgevoerd in Postman. (Vergeet niet om te vervangen de `refresh_token`) [ ![uitvoeren in Postman](./media/v2-oauth2-auth-code-flow/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)
> 
> 

| Parameter     |                | Description        |
|---------------|----------------|--------------------|
| `tenant`        | vereist     | De `{tenant}` waarde in het pad van de aanvraag kan worden gebruikt om te bepalen wie zich bij de toepassing aanmelden kan. De toegestane waarden zijn `common`, `organizations`, `consumers`, en tenant-id's. Zie voor meer details [protocol basisbeginselen](active-directory-v2-protocols.md#endpoints).   |
| `client_id`     | vereist    | De **(client) toepassings-ID** die de app-registratieportal ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) of **(Preview) van de App-registraties** heeft ervaring in Azure portal uw app is toegewezen.  |
| `grant_type`    | vereist    | Moet `refresh_token` voor deze kant van de autorisatiecodestroom. |
| `scope`         | vereist    | Een door spaties gescheiden lijst met bereiken. De bereiken die in dit gedeelte wordt aangevraagd moet gelijk aan of een subset van de bereiken die in de oorspronkelijke authorization_code aanvraag arm wordt aangevraagd. Als de bereiken die is opgegeven in deze aanvraag meerdere resource-server omvatten, wordt een token voor de resource die is opgegeven in de eerste scope geretourneerd door het v2.0-eindpunt. Raadpleeg voor een meer gedetailleerde uitleg van bereiken, [machtigingen en toestemming scopes](v2-permissions-and-consent.md). |
| `refresh_token` | vereist    | De refresh_token die u hebt verkregen in het tweede gedeelte van de stroom. |
| `redirect_uri`  | vereist    | Dezelfde redirect_uri-waarde die is gebruikt voor het verkrijgen van de authorization_code. |
| `client_secret` | vereist voor web-apps | Het toepassingsgeheim die u hebt gemaakt in de portal voor app-registratie voor uw app. Deze mag niet worden gebruikt in een systeemeigen app omdat client_secrets op betrouwbare wijze kunnen niet worden opgeslagen op apparaten. Dit is vereist voor de web-apps en web-API's, waarvoor de mogelijkheid om op te slaan de waarde voor client_secret veilig op de server.                                                                                                                                                    |

#### <a name="successful-response"></a>Geslaagde reactie

Een geslaagde respons token ziet er als:

```json
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fuser.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```
| Parameter     | Description         |
|---------------|-------------------------------------------------------------|
| `access_token`  | Het aangevraagde toegangstoken. De app kan dit token gebruikt voor verificatie bij de beveiligde resource, zoals een web-API. |
| `token_type`    | Geeft aan dat de waarde van het token. Het enige type die ondersteuning biedt voor Azure AD is Bearer |
| `expires_in`    | Hoe lang het toegangstoken is ongeldig (in seconden).   |
| `scope`         | De bereiken die de access_token is geldig voor.    |
| `refresh_token` | Een nieuwe vernieuwingstoken voor OAuth 2.0. Met deze nieuwe verworven vernieuwingstoken om te controleren of dat uw vernieuwingstokens blijven geldig voor zo lang mogelijk moet u de oude vernieuwingstoken vervangen. <br> **Opmerking:** Alleen opgegeven als `offline_access` bereik is aangevraagd.|
| `id_token`      | Een niet-ondertekende JSON Web Token (JWT). De app kan worden gedecodeerd de segmenten van dit token informatie opvragen over de gebruiker die zijn aangemeld. De app kan de waarden in de cache en deze weer te geven, maar deze moet niet gebruiken voor autorisatie of grenzen voor netwerkbeveiliging. Zie voor meer informatie over id_tokens, de [ `id_token reference` ](id-tokens.md). <br> **Opmerking:** Alleen opgegeven als `openid` bereik is aangevraagd. |

#### <a name="error-response"></a>Foutbericht

```json
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/mail.read is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
  "error_codes": [
    70011
  ],
  "timestamp": "2016-01-09 02:02:12Z",
  "trace_id": "255d1aef-8c98-452f-ac51-23d051240864",
  "correlation_id": "fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7"
}
```

| Parameter         | Description                                                                                        |
|-------------------|----------------------------------------------------------------------------------------------------|
| `error`           | Een tekenreeks voor de foutcode die kan worden gebruikt voor het classificeren van typen fouten die optreden en kan worden gebruikt om te reageren op fouten. |
| `error_description` | Een specifieke foutbericht dat een ontwikkelaar kan helpen de hoofdoorzaak van een verificatiefout identificeren.           |
| `error_codes` |Een lijst van de STS-specifieke foutcodes die bij het diagnostische gegevens helpen. |
| `timestamp` | De tijd waarop de fout is opgetreden. |
| `trace_id` | Een unieke id voor de aanvraag die bij het diagnostische gegevens helpen. |
| c`orrelation_id` | Een unieke id voor de aanvraag die bij het diagnostische gegevens voor onderdelen helpen. |

Zie voor een beschrijving van de foutcodes en de aanbevolen clientactie [foutcodes voor token-eindpunt fouten](#error-codes-for-token-endpoint-errors).
