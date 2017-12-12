---
title: Azure AD v2.0 OAuth-autorisatie Code stroom | Microsoft Docs
description: Gebouw webtoepassingen die gebruikmaken van Azure AD-implementatie van het protocol OAuth 2.0-verificatie.
services: active-directory
documentationcenter: 
author: dstrockis
manager: mtillman
editor: 
ms.assetid: ae1d7d86-7098-468c-aa32-20df0a10ee3d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 06d5000e30850156781496c32ac549ecc0772f46
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="v20-protocols---oauth-20-authorization-code-flow"></a>Protocollen v2.0 - OAuth 2.0-Autorisatiecodestroom
Het verlenen van OAuth 2.0 autorisatie code kan worden gebruikt in apps die zijn geïnstalleerd op een apparaat toegang te krijgen tot beveiligde bronnen, zoals web-API's.  Met de app model v2.0-implementatie van OAuth 2.0, kunt u aanmelden en API toegang tot uw mobiele en bureaublad-apps toevoegen.  Deze handleiding is taalonafhankelijk en wordt beschreven hoe u berichten verzenden en ontvangen HTTP zonder gebruik van een van onze open source-bibliotheken.

> [!NOTE]
> Niet alle Azure Active Directory-scenario's en functies worden ondersteund door het v2.0-eindpunt.  Meer informatie over om te bepalen of moet u het v2.0-eindpunt, [v2.0 beperkingen](active-directory-v2-limitations.md).
> 
> 

De OAuth 2.0-autorisatiecodestroom wordt beschreven in in [sectie 4.1 van de OAuth 2.0-specificatie](http://tools.ietf.org/html/rfc6749).  Dit wordt gebruikt voor verificatie en autorisatie uitvoeren in het merendeel van de app-typen, inclusief [web-apps](active-directory-v2-flows.md#web-apps) en [systeemeigen geïnstalleerde apps](active-directory-v2-flows.md#mobile-and-native-apps).  Hiermee kunt apps veilig verkrijgen access_tokens die kan worden gebruikt voor toegang tot resources die zijn beveiligd met het v2.0-eindpunt.  

## <a name="protocol-diagram"></a>Protocol-diagram
Op een hoog niveau ziet de hele authenticatiestroom voor een native/mobile-applicatie er nogal zo:

![OAuth autorisatiecode stroom](../../media/active-directory-v2-flows/convergence_scenarios_native.png)

## <a name="request-an-authorization-code"></a>Aanvraag een autorisatiecode
De autorisatiecodestroom begint met de client voor het routeren van de gebruiker de `/authorize` eindpunt.  De client wijst de machtigingen die te verkrijgen van de gebruiker moet op deze aanvraag:

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&state=12345
```

> [!TIP]
> Klik in de onderstaande koppeling voor het uitvoeren van deze aanvraag. Na het aanmelden, uw browser wordt omgeleid naar `https://localhost/myapp/` met een `code` in de adresbalk.
> <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=code&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&response_mode=query&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&state=12345" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/Authorize...</a>
> 
> 

| Parameter |  | Beschrijving |
| --- | --- | --- |
| Tenant |Vereist |De `{tenant}` waarde in het pad van de aanvraag kan worden gebruikt om te bepalen wie bij de toepassing aanmelden kunt.  De toegestane waarden zijn `common`, `organizations`, `consumers`, en het tenant-id's.  Zie voor meer details [protocol basisbeginselen](active-directory-v2-protocols.md#endpoints). |
| client_id |Vereist |De toepassings-Id die de portal voor wachtwoordregistratie ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) toegewezen van uw app. |
| response_type |Vereist |Moet bevatten `code` voor de autorisatiecodestroom. |
| redirect_uri |Aanbevolen |De redirect_uri van uw app, waarbij verificatie reacties kunnen worden verzonden en ontvangen door uw app.  Er moet een van de redirect_uris die u in de portal hebt geregistreerd, behalve het url-codering moet exact overeenkomen.  Voor mobiele en systeemeigen apps, moet u de standaardwaarde van `https://login.microsoftonline.com/common/oauth2/nativeclient`. |
| Bereik |Vereist |Een door spaties gescheiden lijst met [scopes](active-directory-v2-scopes.md) dat u wilt dat de gebruiker toe te staan. |
| response_mode |Aanbevolen |Hiermee geeft u de methode die moet worden gebruikt voor het verzenden van het resulterende token terug naar uw app.  Kan `query` of `form_post`. |
| state |Aanbevolen |Een waarde die is opgenomen in de aanvraag die ook in het token antwoord worden geretourneerd.  Een tekenreeks van inhoud die u wenst dat kan zijn.  Een willekeurig gegenereerde unieke waarde wordt doorgaans gebruikt voor [voorkomen van aanvraagvervalsing op meerdere sites aanvallen](http://tools.ietf.org/html/rfc6749#section-10.12).  De status wordt ook gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat het verificatieverzoek opgetreden, zoals de pagina of de weergave op. |
| prompt |optioneel |Geeft het type van de interactie van de gebruiker die is vereist.  De enige geldige waarden op dit moment zijn 'aanmelding', 'none', ' toestemming geven '.  `prompt=login`wordt de gebruiker dwingen te hun referenties invoeren voor deze aanvraag, het negatief maken van eenmalige aanmelding.  `prompt=none`is het tegenovergestelde - dan gegarandeerd dat de gebruiker niet vragen beeïndigen krijgt.  Als de aanvraag kan achtergrond via eenmalige aanmelding worden voltooid, wordt het v2.0-eindpunt een fout geretourneerd.  `prompt=consent`activeert de toestemming OAuth dialoogvenster nadat de gebruiker zich aanmeldt, de gebruiker machtigen om de app wordt gevraagd. |
| login_hint |optioneel |Kan worden gebruikt voor het vervullen van het veld gebruikersnaam, e-mailadres van de aanmeldingspagina voor de gebruiker vooraf als u hun gebruikersnaam tevoren weet.  Vaak apps Gebruik deze parameter tijdens hernieuwde verificatie de gebruikersnaam die al worden opgehaald uit een eerdere aanmelden met de `preferred_username` claim. |
| domain_hint |optioneel |Een van `consumers` of `organizations`.  Als opgenomen, wordt het proces detectie op basis van e-mailbericht wordt overgeslagen die gebruiker doorloopt op de v2.0 aanmeldingspagina wordt, wat leidt tot een iets meer gestroomlijnde gebruikerservaring.  Vaak apps wordt met deze parameter gebruikt tijdens de hervalidatie door het uitpakken van de `tid` van een vorige aanmelden.  Als de `tid` claim waarde is `9188040d-6c67-4c5b-b112-36a304b66dad`, moet u `domain_hint=consumers`.  Gebruik anders `domain_hint=organizations`. |

Op dit moment wordt de gebruiker wordt gevraagd hun referenties invoeren en de authenticatie voltooien.  Het v2.0-eindpunt er ook voor zorgen dat de gebruiker heeft ingestemd met de machtigingen die zijn aangegeven in de `scope` queryparameter.  Als de gebruiker niet aan een van deze machtigingen heeft ingestemd, wordt deze vraagt de gebruiker toe te staan de vereiste machtigingen.  Details van [machtigingen, toestemming en multitenant apps vindt u hier](active-directory-v2-scopes.md).

Als de gebruiker wordt geverifieerd en toestemming verleent, het v2.0-eindpunt resulteert in een antwoord aan uw app op de aangegeven `redirect_uri`, via de methode die is opgegeven in de `response_mode` parameter.

#### <a name="successful-response"></a>Geslaagde reactie
Een geslaagde reactie met `response_mode=query` lijkt:

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=12345
```

| Parameter | Beschrijving |
| --- | --- |
| code |De authorization_code die de app wordt aangevraagd. De app kunt u de autorisatiecode gebruiken om aan te vragen van een toegangstoken voor de doelresource.  Authorization_codes zijn zeer korte levensduur, doorgaans ze verlopen na ongeveer 10 minuten. |
| state |Als een parameter state is opgenomen in de aanvraag, moet dezelfde waarde weergegeven in het antwoord. De app dient te verifiëren dat de statuswaarden in de aanvraag en antwoord identiek zijn. |

#### <a name="error-response"></a>Foutbericht
Foutberichten kunnen ook worden verzonden naar de `redirect_uri` zodat de app ze op de juiste wijze kan verwerken:

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parameter | Beschrijving |
| --- | --- |
| error |Een tekenreeks van de fout code die kan worden gebruikt voor het classificeren van typen fouten die optreden en kan worden gebruikt om te reageren op fouten. |
| error_description |Een specifieke foutbericht dat een ontwikkelaar kan helpen de hoofdoorzaak van een verificatiefout identificeren. |

#### <a name="error-codes-for-authorization-endpoint-errors"></a>Foutcodes voor autorisatie eindpunt fouten
De volgende tabel beschrijft de verschillende foutcodes die kunnen worden geretourneerd in de `error` parameter van het foutbericht.

| Foutcode | Beschrijving | Clientactie |
| --- | --- | --- |
| invalid_request |Protocolfout, zoals een ontbrekende vereiste parameter. |Herstel en verzend de aanvraag opnieuw. Dit is een ontwikkeling fout wordt meestal onderschept tijdens de eerste test. |
| unauthorized_client |De clienttoepassing is niet toegestaan om aan te vragen een autorisatiecode. |Dit gebeurt meestal wanneer de clienttoepassing is niet geregistreerd in Azure AD of is niet toegevoegd aan Azure AD-tenant van de gebruiker. De toepassing kan het bericht met instructies voor het installeren van de toepassing en deze naar Azure AD toe te voegen. |
| ACCESS_DENIED |Resource-eigenaar toestemming geweigerd |De clienttoepassing kan de gebruiker die deze kan niet worden voortgezet tenzij de gebruiker akkoord gaat melden. |
| unsupported_response_type |De autorisatie-server biedt geen ondersteuning voor het antwoordtype in de aanvraag. |Herstel en verzend de aanvraag opnieuw. Dit is een ontwikkeling fout wordt meestal onderschept tijdens de eerste test. |
| server_error |De server heeft een onverwachte fout aangetroffen. |De aanvraag opnieuw proberen. Deze fouten kunnen worden veroorzaakt door tijdelijke omstandigheden. Aan de gebruiker kan de clienttoepassing verklaren dat het antwoord is vertraagd vanwege een tijdelijke fout. |
| temporarily_unavailable |De server is tijdelijk te druk bezet om de aanvraag te verwerken. |De aanvraag opnieuw proberen. Aan de gebruiker kan de clienttoepassing verklaren dat het antwoord is vertraagd vanwege een tijdelijk probleem. |
| invalid_resource |De doelresource is ongeldig omdat deze niet bestaat, Azure AD kan niet worden gevonden of is niet correct geconfigureerd. |Dit betekent dat de resource als deze bestaat, is niet geconfigureerd in de tenant. De toepassing kan het bericht met instructies voor het installeren van de toepassing en deze naar Azure AD toe te voegen. |

## <a name="request-an-access-token"></a>Aanvragen van een toegangstoken
Nu dat u een authorization_code aangeschaft hebt en gemachtigd door de gebruiker, u kunt gebruikmaken van de `code` voor een `access_token` tot de gewenste resource, door te sturen een `POST` aanvraag voor de `/token` eindpunt:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&code=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq3n8b2JRLk4OxVXr...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=authorization_code
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```

> [!TIP]
> Probeer deze aanvraag uit te voeren in Postman! (Vergeet niet ter vervanging van de `code`) [ ![uitvoeren in Postman](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)
> 
> 

| Parameter |  | Beschrijving |
| --- | --- | --- |
| Tenant |Vereist |De `{tenant}` waarde in het pad van de aanvraag kan worden gebruikt om te bepalen wie bij de toepassing aanmelden kunt.  De toegestane waarden zijn `common`, `organizations`, `consumers`, en het tenant-id's.  Zie voor meer details [protocol basisbeginselen](active-directory-v2-protocols.md#endpoints). |
| client_id |Vereist |De toepassings-Id die de portal voor wachtwoordregistratie ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) toegewezen van uw app. |
| grant_type |Vereist |Moet `authorization_code` voor de autorisatiecodestroom. |
| Bereik |Vereist |Een door spaties gescheiden lijst met bereiken.  De bereiken die in deze fase wordt aangevraagd moet gelijk aan of een subset van de bereiken die in de eerste fase wordt aangevraagd.  Als de bereiken die is opgegeven in deze aanvraag span meerdere resource-servers, wordt een token voor de resource die is opgegeven in het eerste bereik geretourneerd door het v2.0-eindpunt.  Raadpleeg voor een meer gedetailleerde uitleg van bereiken [machtigingen, toestemming en -scopes](active-directory-v2-scopes.md). |
| code |Vereist |De authorization_code die u hebt verkregen in de eerste zijde van de stroom. |
| redirect_uri |Vereist |Dezelfde redirect_uri-waarde die is gebruikt voor het verkrijgen van de authorization_code. |
| client_secret |vereist voor web-apps |De toepassingsgeheim die u in de portal van de registratie van de app voor uw app hebt gemaakt.  Deze mag niet worden gebruikt in een eigen app omdat client_secrets betrouwbaar kunnen niet worden opgeslagen op apparaten.  Het is vereist voor de web-apps en web-API's die u de mogelijkheid hebt voor het opslaan van de client_secret veilig op de server. |

#### <a name="successful-response"></a>Geslaagde reactie
Een geslaagde reactie token, ziet er als:

```
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```
| Parameter | Beschrijving |
| --- | --- |
| access_token |Het aangevraagde toegangstoken. De app kunt dit token gebruiken om de beveiligde resource, zoals een web-API te verifiëren. |
| token_type |Geeft de waarde van het type token. Het enige type die ondersteuning biedt voor Azure AD is Bearer |
| expires_in |Hoe lang het toegangstoken is ongeldig (in seconden). |
| Bereik |De bereiken die de access_token is geldig voor. |
| refresh_token |Een OAuth 2.0-vernieuwingstoken. De app kunt gebruiken dit token aanvullende toegangstokens verkrijgen nadat het huidige toegangstoken is verlopen.  Refresh_tokens zijn lange levensduur hebben en kunnen worden gebruikt voor toegang tot bronnen voor langere tijd te behouden.  Raadpleeg voor meer details over de [v2.0 tokenverwijzing](active-directory-v2-tokens.md). <br> **Opmerking:** alleen opgegeven als `offline_access` bereik is aangevraagd. |
| id_token |Een niet-ondertekende JSON Web Token (JWT). De app kan base64Url decoderen de segmenten van dit token informatie opvragen over de gebruiker die zich aangemeld. De app kan de waarden in de cache en deze weer te geven, maar deze niet verstandig deze beveiligingsgrenzen of autorisatie.  Zie voor meer informatie over id_tokens de [v2.0-eindpunt tokenverwijzing](active-directory-v2-tokens.md). <br> **Opmerking:** alleen opgegeven als `openid` bereik is aangevraagd. |
#### <a name="error-response"></a>Foutbericht
Foutberichten ziet er als volgt:

```
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

| Parameter | Beschrijving |
| --- | --- |
| error |Een tekenreeks van de fout code die kan worden gebruikt voor het classificeren van typen fouten die optreden en kan worden gebruikt om te reageren op fouten. |
| error_description |Een specifieke foutbericht dat een ontwikkelaar kan helpen de hoofdoorzaak van een verificatiefout identificeren. |
| error_codes |Een lijst van STS specifieke foutcodes die bij het diagnostische gegevens helpen. |
| tijdstempel |De tijd waarop de fout is opgetreden. |
| trace_id |Een unieke id voor de aanvraag die bij het diagnostische gegevens helpen. |
| correlation_id |Een unieke id voor de aanvraag die bij het diagnostische gegevens over de onderdelen helpen. |

#### <a name="error-codes-for-token-endpoint-errors"></a>Foutcodes voor token-eindpunt fouten
| Foutcode | Beschrijving | Clientactie |
| --- | --- | --- |
| invalid_request |Protocolfout, zoals een ontbrekende vereiste parameter. |Herstel en verzend de aanvraag opnieuw |
| invalid_grant |De autorisatiecode is ongeldig of is verlopen. |Probeer een nieuwe aanvraag naar de `/authorize` eindpunt |
| unauthorized_client |De geverifieerde client is niet gemachtigd deze machtiging grant methode gebruiken. |Dit gebeurt meestal wanneer de clienttoepassing is niet geregistreerd in Azure AD of is niet toegevoegd aan Azure AD-tenant van de gebruiker. De toepassing kan het bericht met instructies voor het installeren van de toepassing en deze naar Azure AD toe te voegen. |
| invalid_client |Clientverificatie is mislukt. |De clientreferenties zijn niet geldig. Als u wilt oplossen, werkt de beheerder van de toepassing de referenties. |
| unsupported_grant_type |De autorisatie-server biedt geen ondersteuning voor de machtiging grant-type. |Het type grant in de aanvraag wijzigen. Dit type fout moet worden uitgevoerd tijdens de ontwikkeling en worden gedetecteerd tijdens de eerste test. |
| invalid_resource |De doelresource is ongeldig omdat deze niet bestaat, Azure AD kan niet worden gevonden of is niet correct geconfigureerd. |Dit betekent dat de resource als deze bestaat, is niet geconfigureerd in de tenant. De toepassing kan het bericht met instructies voor het installeren van de toepassing en deze naar Azure AD toe te voegen. |
| interaction_required |De aanvraag vereist gebruikersinteractie. Bijvoorbeeld, is een stap extra authenticatie vereist. |Probeer de aanvraag met dezelfde resource. |
| temporarily_unavailable |De server is tijdelijk te druk bezet om de aanvraag te verwerken. |De aanvraag opnieuw proberen. Aan de gebruiker kan de clienttoepassing verklaren dat het antwoord is vertraagd vanwege een tijdelijk probleem. |

## <a name="use-the-access-token"></a>Gebruik het toegangstoken
Nu dat u hebt gekregen een `access_token`, kunt u het token in hun aanvragen aan de Web-API's door te nemen in de `Authorization` header:

> [!TIP]
> Het uitvoeren van deze aanvraag in Postman! (Vervangen door de `Authorization` header eerste) [ ![uitvoeren in Postman](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)
> 
> 

```
GET /v1.0/me/messages
Host: https://graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-the-access-token"></a>Het toegangstoken vernieuwen
Access_tokens korte worden gehouden en moet u deze vernieuwen nadat ze zijn verlopen om door te gaan met het openen van bronnen.  U kunt dit doen door het indienen van een andere `POST` aanvraag voor de `/token` eindpunt, deze keer mits de `refresh_token` in plaats van de `code`:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=refresh_token
&client_secret=JqQX2PNo9bpM0uEihUPzyrh      // NOTE: Only required for web apps
```

> [!TIP]
> Probeer deze aanvraag uit te voeren in Postman! (Vergeet niet ter vervanging van de `refresh_token`) [ ![uitvoeren in Postman](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)
> 
> 

| Parameter |  | Beschrijving |
| --- | --- | --- |
| Tenant |Vereist |De `{tenant}` waarde in het pad van de aanvraag kan worden gebruikt om te bepalen wie bij de toepassing aanmelden kunt.  De toegestane waarden zijn `common`, `organizations`, `consumers`, en het tenant-id's.  Zie voor meer details [protocol basisbeginselen](active-directory-v2-protocols.md#endpoints). |
| client_id |Vereist |De toepassings-Id die de portal voor wachtwoordregistratie ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) toegewezen van uw app. |
| grant_type |Vereist |Moet `refresh_token` voor deze fase van de autorisatiecodestroom. |
| Bereik |Vereist |Een door spaties gescheiden lijst met bereiken.  De bereiken die in deze fase wordt aangevraagd moet gelijk aan of een subset van de bereiken die in de oorspronkelijke authorization_code aanvraag arm wordt aangevraagd.  Als de bereiken die is opgegeven in deze aanvraag span meerdere resource-servers, wordt een token voor de resource die is opgegeven in het eerste bereik geretourneerd door het v2.0-eindpunt.  Raadpleeg voor een meer gedetailleerde uitleg van bereiken [machtigingen, toestemming en -scopes](active-directory-v2-scopes.md). |
| refresh_token |Vereist |De refresh_token die u hebt verkregen in de tweede fase van de stroom. |
| redirect_uri |Vereist |Dezelfde redirect_uri-waarde die is gebruikt voor het verkrijgen van de authorization_code. |
| client_secret |vereist voor web-apps |De toepassingsgeheim die u in de portal van de registratie van de app voor uw app hebt gemaakt.  Deze mag niet worden gebruikt in een eigen app omdat client_secrets betrouwbaar kunnen niet worden opgeslagen op apparaten.  Het is vereist voor de web-apps en web-API's die u de mogelijkheid hebt voor het opslaan van de client_secret veilig op de server. |

#### <a name="successful-response"></a>Geslaagde reactie
Een geslaagde reactie token, ziet er als:

```
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```
| Parameter | Beschrijving |
| --- | --- |
| access_token |Het aangevraagde toegangstoken. De app kunt dit token gebruiken om de beveiligde resource, zoals een web-API te verifiëren. |
| token_type |Geeft de waarde van het type token. Het enige type die ondersteuning biedt voor Azure AD is Bearer |
| expires_in |Hoe lang het toegangstoken is ongeldig (in seconden). |
| Bereik |De bereiken die de access_token is geldig voor. |
| refresh_token |Een nieuwe OAuth 2.0-vernieuwingstoken. U moet de oude vernieuwingstoken vervangen door deze recent overgenomen vernieuwingstoken om te controleren of dat het vernieuwen van tokens geldig voor zo lang mogelijk blijven. <br> **Opmerking:** alleen opgegeven als `offline_access` bereik is aangevraagd. |
| id_token |Een niet-ondertekende JSON Web Token (JWT). De app kan base64Url decoderen de segmenten van dit token informatie opvragen over de gebruiker die zich aangemeld. De app kan de waarden in de cache en deze weer te geven, maar deze niet verstandig deze beveiligingsgrenzen of autorisatie.  Zie voor meer informatie over id_tokens de [v2.0-eindpunt tokenverwijzing](active-directory-v2-tokens.md). <br> **Opmerking:** alleen opgegeven als `openid` bereik is aangevraagd. |

#### <a name="error-response"></a>Foutbericht
```
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

| Parameter | Beschrijving |
| --- | --- |
| error |Een tekenreeks van de fout code die kan worden gebruikt voor het classificeren van typen fouten die optreden en kan worden gebruikt om te reageren op fouten. |
| error_description |Een specifieke foutbericht dat een ontwikkelaar kan helpen de hoofdoorzaak van een verificatiefout identificeren. |
| error_codes |Een lijst van STS specifieke foutcodes die bij het diagnostische gegevens helpen. |
| tijdstempel |De tijd waarop de fout is opgetreden. |
| trace_id |Een unieke id voor de aanvraag die bij het diagnostische gegevens helpen. |
| correlation_id |Een unieke id voor de aanvraag die bij het diagnostische gegevens over de onderdelen helpen. |

Zie voor een beschrijving van de foutcodes en de aanbevolen clientactie [foutcodes voor token-eindpunt fouten](#error-codes-for-token-endpoint-errors).

