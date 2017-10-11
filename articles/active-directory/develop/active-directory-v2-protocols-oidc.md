---
title: Azure Active Directory-v2.0 en het protocol OpenID Connect | Microsoft Docs
description: Webtoepassingen bouwen met behulp van de Azure AD v2.0-implementatie van het OpenID Connect-verificatieprotocol.
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: a4875997-3aac-4e4c-b7fe-2b4b829151ce
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: d63692f02b3dec50a1e7df034b8915bb450b4cfd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# Azure Active Directory-v2.0 en het protocol OpenID Connect
OpenID Connect is een authenticatieprotocol dat is gebaseerd op OAuth 2.0 die u gebruiken kunt voor het veilig Meld u aan een gebruiker aan een webtoepassing. Wanneer u het v2.0-eindpunt implementatie van OpenID Connect gebruikt, kunt u aanmelden en toegang tot API toevoegen aan uw web gebaseerde apps. In dit artikel wordt beschreven hoe u te doen dit onafhankelijk van de taal. We beschrijven hoe verzenden en ontvangen van HTTP-berichten zonder een Microsoft open source-bibliotheken.

> [!NOTE]
> Het v2.0-eindpunt biedt geen ondersteuning voor alle Azure Active Directory-scenario's en onderdelen. Meer informatie over om te bepalen of het v2.0-eindpunt moet worden gebruikt, [v2.0 beperkingen](active-directory-v2-limitations.md).
> 
> 

[OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html) breidt het OAuth 2.0 *autorisatie* protocol moet worden gebruikt als een *verificatie* protocol, zodat u één uitvoeren kunt aanmelding met OAuth. OpenID Connect introduceert het concept van een *token ID*, dit is een beveiligingstoken waarmee de client de identiteit van de gebruiker. Het token ID wordt ook basisprofiel informatie over de gebruiker. Omdat het OpenID Connect breidt OAuth 2.0, apps veilig kunnen verkrijgen *toegang tot tokens*, die kan worden gebruikt voor toegang tot resources die zijn beveiligd met een [autorisatie server](active-directory-v2-protocols.md#the-basics). Het is raadzaam dat u OpenID Connect als u een [webtoepassing](active-directory-v2-flows.md#web-apps) die wordt gehost op een server en geopend via een browser.

## Protocol diagram: aanmelden
De meest eenvoudige aanmelding-stroom heeft de stappen die u in het volgende diagram. Elke stap in detail in dit artikel worden beschreven.

![Protocol OpenID Connect: aanmelden](../../media/active-directory-v2-flows/convergence_scenarios_webapp.png)

## Het metagegevensdocument OpenID Connect ophalen
Beschrijving van een document met metagegevens die de meeste van de vereiste informatie voor een app uit te voeren aanmelden bevat OpenID Connect. Dit omvat gegevens zoals de URL's te gebruiken en de locatie van de openbare handtekeningsleutels van de service. Dit is het metagegevensdocument OpenID Connect dat moet u voor het v2.0-eindpunt:

```
https://login.microsoftonline.com/{tenant}/v2.0/.well-known/openid-configuration
```

De `{tenant}` kan duren voordat een van de vier waarden:

| Waarde | Beschrijving |
| --- | --- |
| `common` |Gebruikers met een persoonlijk Microsoft-account en een account voor werk of school van Azure Active Directory (Azure AD) kunnen aanmelden bij de toepassing. |
| `organizations` |Alleen gebruikers met een werk- of schoolaccounts van Azure AD kunnen aanmelden bij de toepassing. |
| `consumers` |Alleen gebruikers met een persoonlijk Microsoft-account kunnen aanmelden bij de toepassing. |
| `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` of `contoso.onmicrosoft.com` |Alleen gebruikers met een werk- of schoolaccount van een specifieke Azure AD tenant kan aanmelden bij de toepassing. Ofwel de aangepaste domeinnaam van de Azure AD-tenant of GUID-id van de tenant kan worden gebruikt. |

De metagegevens is een eenvoudige JSON JavaScript Object Notation ()-document. Zie het volgende fragment voor een voorbeeld. De inhoud van het codefragment is volledig worden beschreven in de [specificatie van het OpenID Connect](https://openid.net).

```
{
  "authorization_endpoint": "https:\/\/login.microsoftonline.com\/common\/oauth2\/v2.0\/authorize",
  "token_endpoint": "https:\/\/login.microsoftonline.com\/common\/oauth2\/v2.0\/token",
  "token_endpoint_auth_methods_supported": [
    "client_secret_post",
    "private_key_jwt"
  ],
  "jwks_uri": "https:\/\/login.microsoftonline.com\/common\/discovery\/v2.0\/keys",

  ...

}
```

Normaal gesproken wilt u dit metagegevensdocument gebruiken voor het configureren van een bibliotheek OpenID Connect of de SDK. de bibliotheek zou de metagegevens voor het werk gebruiken. Als u een vooraf build OpenID Connect-bibliotheek niet gebruikt, kunt u de stappen in de rest van dit artikel uitvoeren aanmelden in een web-app met behulp van het v2.0-eindpunt volgen.

## Verzending van de aanvraag voor aanmelden
Als uw web-app moet de gebruiker te verifiëren, kan de gebruiker worden doorgestuurd de `/authorize` eindpunt. Deze aanvraag is vergelijkbaar met de eerste zijde van de [OAuth 2.0-autorisatiecodestroom](active-directory-v2-protocols-oauth-code.md), met deze belangrijke verschillen:

* De aanvraag moet bevatten de `openid` bereik de `scope` parameter.
* De `response_type` vergezeld gaan van de parameter `id_token`.
* De aanvraag moet bevatten de `nonce` parameter.

Bijvoorbeeld:

```
// Line breaks are for legibility only.

GET https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=form_post
&scope=openid
&state=12345
&nonce=678910
```

> [!TIP]
> Klik op de volgende koppeling om het uitvoeren van deze aanvraag. Nadat u zich aanmeldt, wordt uw browser wordt omgeleid naar https://localhost/myapp/ met een token ID in de adresbalk. Let op: maakt gebruik van deze aanvraag `response_mode=query` (voor demonstratiedoeleinden alleen). Het is raadzaam dat u `response_mode=form_post`.
> <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=openid&response_mode=query&state=12345&nonce=678910" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/Authorize...</a>
> 
> 

| Parameter | Voorwaarde | Beschrijving |
| --- | --- | --- |
| Tenant |Vereist |U kunt de `{tenant}` waarde in het pad van de aanvraag om te bepalen wie bij de toepassing aanmelden zich. De toegestane waarden zijn `common`, `organizations`, `consumers`, en het tenant-id's. Zie voor meer informatie [protocol basisbeginselen](active-directory-v2-protocols.md#endpoints). |
| client_id |Vereist |De aanvraag-ID die de [Registratieportal toepassing](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) toegewezen aan uw app. |
| response_type |Vereist |Moet bevatten `id_token` voor OpenID Connect aanmelden. Het kan ook betekenen dat andere `response_types` waarden, zoals `code`. |
| redirect_uri |Aanbevolen |De omleidings-URI van uw app, waarbij verificatie reacties kunnen worden verzonden en ontvangen door uw app. Moet exact overeenkomen met een van de omleidings-URI's die u in de portal hebt geregistreerd, behalve dat het moet een URL zijn gecodeerd. |
| Bereik |Vereist |Een door spaties gescheiden lijst met bereiken. Voor het OpenID Connect, het bereik moet bevatten `openid`, die wordt omgezet in de machtiging 'Aanmelden u' in de gebruikersinterface van de toestemming. U kunt ook andere bereiken opnemen in deze aanvraag voor toestemming wordt gevraagd. |
| nonce |Vereist |Een waarde die is opgenomen in de aanvraag, die worden gegenereerd door de app, die wordt opgenomen in de resulterende id_token-waarde als een claim. De app kunt controleren of deze waarde om te beperken token replay-aanvallen. De waarde is doorgaans een willekeurige, unieke tekenreeks die kan worden gebruikt voor het identificeren van de oorsprong van de aanvraag. |
| response_mode |Aanbevolen |Hiermee geeft u de methode die moet worden gebruikt voor het verzenden van de resulterende autorisatiecode terug naar uw app. Een van `query`, `form_post`, of `fragment`. Voor webtoepassingen, wordt u aangeraden `response_mode=form_post`, zodat de meest beveiligde overdracht van tokens voor uw toepassing. |
| state |Aanbevolen |Een waarde die is opgenomen in de aanvraag die ook in het token antwoord worden geretourneerd. Een tekenreeks van alle inhoud die u wilt dat kan zijn. Een willekeurig gegenereerde unieke waarde wordt doorgaans gebruikt voor [aanvraagvervalsing op meerdere sites aanvallen te voorkomen](http://tools.ietf.org/html/rfc6749#section-10.12). De status wordt ook gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat het verificatieverzoek opgetreden, zoals de pagina of de gebruiker is op weergave. |
| prompt |Optioneel |Geeft het type van de interactie van de gebruiker die is vereist. De enige geldige waarden op dit moment zijn `login`, `none`, en `consent`. De `prompt=login` claim zorgt ervoor dat gebruikers hun referenties invoeren voor deze aanvraag dat eenmalige aanmelding wordt genegeerd. De `prompt=none` claim is het tegenovergestelde. Deze claim zorgt ervoor dat de gebruiker niet vragen beeïndigen krijgt. Als de aanvraag kan achtergrond via eenmalige aanmelding worden voltooid, het v2.0-eindpunt een fout geretourneerd. De `prompt=consent` claim het toestemming OAuth dialoogvenster wordt geactiveerd nadat de gebruiker zich aanmeldt. Het dialoogvenster vraagt de gebruiker machtigingen verlenen voor de app. |
| login_hint |Optioneel |Deze parameter kunt u het veld username en e-mail van het adres van de aanmeldingspagina voor de gebruiker vooraf worden ingevuld als u de gebruikersnaam tevoren weten. Vaak apps deze parameter gebruiken tijdens de hervalidatie na al uitpakken van de gebruikersnaam van een eerdere aanmelden met behulp van de `preferred_username` claim. |
| domain_hint |Optioneel |Deze waarde kan zijn `consumers` of `organizations`. Als de opgenomen, slaat deze het detectieproces op basis van e-mail die de gebruiker zich op het v2.0-aanmeldingspagina, voor een iets meer gestroomlijnde gebruikerservaring doorloopt. Vaak apps Gebruik deze parameter tijdens opnieuw worden geverifieerd door het uitpakken van de `tid` claim uit het token ID. Als de `tid` claim waarde is `9188040d-6c67-4c5b-b112-36a304b66dad`, gebruik `domain_hint=consumers`. Gebruik anders `domain_hint=organizations`. |

Op dit moment wordt de gebruiker gevraagd om hun referenties invoeren en de verificatie te voltooien. Het v2.0-eindpunt verifieert dat de gebruiker heeft ingestemd met de machtigingen die zijn aangegeven in de `scope` queryparameter. Als de gebruiker niet aan een van deze machtigingen heeft ingestemd, wordt het v2.0-eindpunt de gebruiker toe te staan de vereiste machtigingen. U kunt meer lezen over [machtigingen, toestemming en multitenant apps](active-directory-v2-scopes.md).

Nadat de gebruiker wordt geverifieerd en toestemming verleent het v2.0-eindpunt retourneert een antwoord aan uw app op de aangegeven omleidings-URI met de methode die is opgegeven in de `response_mode` parameter.

### Geslaagde reactie
Een geslaagde reactie wanneer u `response_mode=form_post` ziet er als volgt:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&state=12345
```

| Parameter | Beschrijving |
| --- | --- |
| id_token |Het token ID die de app wordt aangevraagd. U kunt de `id_token` parameter om te controleren van de identiteit van de gebruiker en beginnen met een sessie met de gebruiker. Zie voor meer informatie over het ID-tokens en de inhoud van de [v2.0-eindpunt tokens verwijzing](active-directory-v2-tokens.md). |
| state |Als een `state` parameter is opgenomen in de aanvraag dezelfde waarde moet worden weergegeven in het antwoord. De app dient te verifiëren dat de statuswaarden in de aanvraag en antwoord identiek zijn. |

### Foutbericht
Foutberichten kunnen ook worden verzonden naar de omleidings-URI, zodat de app te kan verwerken. Een foutmelding ziet er als volgt:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parameter | Beschrijving |
| --- | --- |
| error |Een tekenreeks van de fout code die u gebruiken kunt voor het classificeren van typen fouten die optreden en om te reageren op fouten. |
| error_description |Een specifiek foutbericht die u kan helpen de hoofdoorzaak van een verificatiefout identificeren. |

### Foutcodes voor autorisatie eindpunt fouten
De volgende tabel worden foutcodes beschreven die kunnen worden geretourneerd in de `error` parameter van het foutbericht:

| Foutcode | Beschrijving | Clientactie |
| --- | --- | --- |
| invalid_request |Protocolfout, zoals een ontbrekende vereiste parameter. |Herstel en verzend de aanvraag opnieuw. Dit is een fout ontwikkeling dat meestal wordt onderschept tijdens de eerste test. |
| unauthorized_client |De clienttoepassing kan niet een autorisatiecode aanvragen. |Dit gebeurt meestal wanneer de clienttoepassing is niet geregistreerd in Azure AD of is niet toegevoegd aan Azure AD-tenant van de gebruiker. De toepassing kan de gebruiker gevraagd met instructies om de toepassing installeren en toevoegen aan Azure AD. |
| ACCESS_DENIED |De resource-eigenaar toestemming geweigerd. |De clienttoepassing kan de gebruiker die deze kan niet worden voortgezet tenzij de gebruiker akkoord gaat melden. |
| unsupported_response_type |De autorisatie-server biedt geen ondersteuning voor het antwoordtype in de aanvraag. |Herstel en verzend de aanvraag opnieuw. Dit is een fout ontwikkeling dat meestal wordt onderschept tijdens de eerste test. |
| server_error |De server heeft een onverwachte fout aangetroffen. |De aanvraag opnieuw proberen. Deze fouten kunnen worden veroorzaakt door tijdelijke omstandigheden. Aan de gebruiker kan de clienttoepassing verklaren dat het antwoord is vertraagd vanwege een tijdelijke fout. |
| temporarily_unavailable |De server is tijdelijk te druk bezet om de aanvraag te verwerken. |De aanvraag opnieuw proberen. Aan de gebruiker kan de clienttoepassing verklaren dat het antwoord is vertraagd doordat een tijdelijke situatie. |
| invalid_resource |De doelresource is ongeldig omdat deze niet bestaat, Azure AD kan niet worden gevonden of is niet correct geconfigureerd. |Hiermee wordt aangegeven dat de resource als deze bestaat, is niet geconfigureerd in de tenant. De toepassing kan het bericht met instructies voor het installeren van de toepassing en deze naar Azure AD toe te voegen. |

## Het token ID valideren
Ontvangen van een token ID is niet voldoende om de gebruiker te verifiëren. U moet ook de handtekening van het token ID valideren en controleer of de claims in het token per vereisten van uw app. Maakt gebruik van het v2.0-eindpunt [JSON Web Tokens (JWTs)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) en cryptografie met openbare sleutels voor het ondertekenen van tokens en controleren of ze geldig zijn.

U kunt kiezen voor het valideren van het token ID in de clientcode, maar een gebruikelijk om is stuurt het token ID naar een back-endserver en er de validatie uitvoeren. Nadat u hebt geverifieerd dat de handtekening van het token ID, moet u controleren of een paar claims. Voor meer informatie, met inbegrip van informatie over [valideren van tokens](active-directory-v2-tokens.md#validating-tokens) en [belangrijke informatie over het ondertekenen van sleutelrollover](active-directory-v2-tokens.md#validating-tokens), Zie de [v2.0 tokens verwijzing](active-directory-v2-tokens.md). U wordt aangeraden met behulp van een bibliotheek parseren en valideren van tokens. Er is ten minste één van deze bibliotheken beschikbaar zijn voor de meeste talen en platforms.
<!--TODO: Improve the information on this-->

Ook kunt u aanvullende verklaren, afhankelijk van uw scenario. Sommige algemene validaties zijn onder andere:

* Zorg ervoor dat de gebruiker of organisatie heeft aangemeld voor de app.
* Zorg ervoor dat de gebruiker de autorisatie of de bevoegdheden is vereist.
* Zorg ervoor dat een bepaalde sterkte van verificatie heeft plaatsgevonden, zoals multi-factor authentication-server.

Zie voor meer informatie over de claims in een token ID, de [v2.0-eindpunt tokens verwijzing](active-directory-v2-tokens.md).

Nadat u hebt het token ID volledig gevalideerd, kunt u beginnen met een sessie met de gebruiker. De claims in het token ID gebruiken voor informatie over de gebruiker in uw app. U kunt deze informatie gebruiken voor de weergave, registreert en autorisaties.

## Afmelden aanvraag verzenden
Als u afmelden van de gebruiker van uw app wilt, is het niet voldoende om te wissen van cookies van uw app of anders de gebruikerssessie te beëindigen. U moet ook de gebruiker omgeleid naar het v2.0-eindpunt om af te melden. Als u dit doet, de gebruiker opnieuw geverifieerd naar uw app zonder hun referenties opnieuw invoeren omdat ze een geldig één aanmeldingssessie met het v2.0-eindpunt.

U kunt omleiden door de gebruiker de `end_session_endpoint` vermeld in het document met OpenID Connect metagegevens:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/logout?
post_logout_redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
```

| Parameter | Voorwaarde | Beschrijving |
| ----------------------- | ------------------------------- | ------------ |
| post_logout_redirect_uri | Aanbevolen | De URL waarnaar de gebruiker wordt omgeleid naar na het afmelden is. Als de parameter niet opgenomen is, wordt de gebruiker een algemeen bericht dat wordt gegenereerd door het v2.0-eindpunt weergegeven. Deze URL moet overeenkomen met een van de omleiding die URI 's geregistreerd voor uw toepassing in de app-portal voor wachtwoordregistratie.  |

## Eenmalige afmelding
Wanneer het omleiden van de gebruiker de `end_session_endpoint`, het v2.0-eindpunt wist de gebruikerssessie vanuit de browser. Echter, de gebruiker mogelijk nog steeds zijn aangemeld bij andere toepassingen die Microsoft-accounts voor verificatie gebruiken. Voor deze toepassingen voor het ondertekenen van de gebruiker uit tegelijk, het v2.0 eindpunt een HTTP GET-aanvraag verzendt naar de geregistreerde `LogoutUrl` van alle toepassingen die de gebruiker momenteel is aangemeld bij. Toepassingen op deze aanvraag moeten reageren door alle sessies waarin de gebruiker te wissen en retourneren een `200` antwoord.  Als u afmelden ondersteuning voor eenmalige aanmelding in uw toepassing wilt, moet u deze implementeren een `LogoutUrl` in de code van uw toepassing.  U kunt instellen de `LogoutUrl` van de app-portal voor wachtwoordregistratie.

## Protocol diagram: Token verkrijgen
Veel WebApps moeten niet alleen de gebruiker te melden bij, maar ook voor toegang tot een webservice namens de gebruiker met behulp van OAuth. Dit scenario combineert OpenID Connect voor de verificatie bij het ophalen van een autorisatiecode die u gebruiken kunt voor toegangstokens ophalen als u van de OAuth-autorisatiecodestroom gebruikmaakt tegelijk.

De volledige OpenID Connect aanmelden en tokens verkrijgen stroom lijkt op het volgende diagram. Elke stap in de volgende secties van het artikel in detail worden beschreven.

![Protocol OpenID Connect: Token verkrijgen](../../media/active-directory-v2-flows/convergence_scenarios_webapp_webapi.png)

## Toegangstokens ophalen
Te verkrijgen toegangstokens, wijzig de aanvraag aanmelden:

```
// Line breaks are for legibility only.

GET https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e        // Your registered Application ID
&response_type=id_token%20code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F       // Your registered redirect URI, URL encoded
&response_mode=form_post                              // 'query', 'form_post', or 'fragment'
&scope=openid%20                                      // Include both 'openid' and scopes that your app needs  
offline_access%20                                         
https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&state=12345                                          // Any value, provided by your app
&nonce=678910                                         // Any value, provided by your app
```

> [!TIP]
> Klik op de volgende koppeling om het uitvoeren van deze aanvraag. Nadat u zich aanmeldt, wordt uw browser wordt omgeleid naar https://localhost/myapp/ met een token ID en een code in de adresbalk. Let op: maakt gebruik van deze aanvraag `response_mode=query` (voor demonstratiedoeleinden alleen). Het is raadzaam dat u `response_mode=form_post`.
> <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token%20code&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&response_mode=query&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&state=12345&nonce=678910" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/Authorize...</a>
> 
> 

Inclusief machtiging scopes in de aanvraag en met `response_type=id_token code`, het v2.0-eindpunt zorgt ervoor dat de gebruiker heeft ingestemd met de machtigingen die zijn aangegeven in de `scope` queryparameter. Het resultaat een autorisatiecode aan uw app voor het uitwisselen van een toegangstoken.

### Geslaagde reactie
Een geslaagde reactie van het gebruik van `response_mode=form_post` ziet er als volgt:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&state=12345
```

| Parameter | Beschrijving |
| --- | --- |
| id_token |Het token ID die de app wordt aangevraagd. U kunt het token ID gebruiken om te controleren van de identiteit van de gebruiker en beginnen met een sessie met de gebruiker. U vindt meer informatie over het ID-tokens en de inhoud in de [v2.0-eindpunt tokens verwijzing](active-directory-v2-tokens.md). |
| code |De autorisatiecode die de app wordt aangevraagd. De app kunt u de autorisatiecode gebruiken om aan te vragen van een toegangstoken voor de doelresource. Er is een autorisatiecode zeer tijdelijke. Normaal gesproken verloopt een autorisatiecode over ongeveer tien minuten. |
| state |Als een parameter state is opgenomen in de aanvraag, moet dezelfde waarde weergegeven in het antwoord. De app dient te verifiëren dat de statuswaarden in de aanvraag en antwoord identiek zijn. |

### Foutbericht
Foutberichten kunnen ook worden verzonden naar de omleidings-URI, zodat de app ze op de juiste wijze kan verwerken. Een foutmelding ziet er als volgt:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parameter | Beschrijving |
| --- | --- |
| error |Een tekenreeks van de fout code die u gebruiken kunt voor het classificeren van typen fouten die optreden en om te reageren op fouten. |
| error_description |Een specifiek foutbericht die u kan helpen de hoofdoorzaak van een verificatiefout identificeren. |

Zie voor een beschrijving van mogelijke foutcodes en aanbevolen clientantwoorden [foutcodes voor autorisatie eindpunt fouten](#error-codes-for-authorization-endpoint-errors).

Wanneer u een autorisatiecode en een token ID hebt, kunt u de gebruiker zich aanmeldt, toegangstokens ophalen namens hen. De gebruiker in ondertekenen, moet u het token ID valideren [precies zoals beschreven](#validate-the-id-token). Als u toegangstokens, volg de stappen onze [OAuth-protocoldocumentatie](active-directory-v2-protocols-oauth-code.md#request-an-access-token).
