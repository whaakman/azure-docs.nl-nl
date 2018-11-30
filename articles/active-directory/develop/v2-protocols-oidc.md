---
title: Azure Active Directory v2.0 en de OpenID Connect-protocol | Microsoft Docs
description: Bouw webtoepassingen met behulp van de Azure AD v2.0-implementatie van de OpenID Connect-verificatieprotocol.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: a4875997-3aac-4e4c-b7fe-2b4b829151ce
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: 756d00786005fb6de26ff363d4e233fc28b48687
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2018
ms.locfileid: "52426839"
---
# <a name="azure-active-directory-v20-and-the-openid-connect-protocol"></a>Azure Active Directory v2.0 en de OpenID Connect-protocol

OpenID Connect is gebouwd op OAuth 2.0 waarmee u kunt veilig zich in een gebruiker naar een webtoepassing verificatieprotocol. Wanneer u het v2.0-eindpunt-implementatie van OpenID Connect, kunt u aanmelden en API-toegang toevoegen aan uw webtoepassingen. In dit artikel laat zien hoe u doet dit onafhankelijk van de taal en wordt beschreven hoe u berichten verzenden en ontvangen HTTP zonder gebruik van een Microsoft open source-bibliotheken.

> [!NOTE]
> Het v2.0-eindpunt biedt geen ondersteuning voor alle Azure Active Directory (Azure AD)-scenario's en onderdelen. Om te bepalen of het v2.0-eindpunt moet worden gebruikt, lees meer over [v2.0 beperkingen](active-directory-v2-limitations.md).

[OpenID Connect](https://openid.net/specs/openid-connect-core-1_0.html) kunt u de OAuth 2.0 uitbreiden *autorisatie* protocol moet worden gebruikt als een *verificatie* protocol, zodat u kunt doen eenmalige aanmelding met OAuth. OpenID Connect introduceert het concept van een *ID-token*, dit is een beveiligingstoken dat kan de client om de identiteit van de gebruiker te verifiëren. De ID-token wordt ook basisprofielgegevens informatie over de gebruiker. Omdat de OpenID Connect, kunt u OAuth 2.0 uitbreiden, apps veilig kunnen verkrijgen *toegangstokens*, die kan worden gebruikt voor toegang tot resources die worden beveiligd door een [autorisatieserver](active-directory-v2-protocols.md#the-basics). Het v2.0-eindpunt kunt ook apps van derden die zijn geregistreerd bij Azure AD om uit te geven van de toegangstokens voor beveiligde resources, zoals Web-API's. Zie voor meer informatie over het instellen van een toepassing om uit te geven toegangstokens [over het registreren van een app met het v2.0-eindpunt](quickstart-v2-register-an-app.md). Raden wij aan dat u OpenID verbinding maken als u bouwt een [webtoepassing](v2-app-types.md#web-apps) dat is gehost op een server en toegankelijk is via een browser.

## <a name="protocol-diagram-sign-in"></a>Diagram van protocol: aanmelding

De meest eenvoudige stroom aanmelden heeft de stappen in het volgende diagram wordt weergegeven. Elke stap wordt in dit artikel beschreven.

![OpenID Connect-protocol: aanmelding](./media/v2-protocols-oidc/convergence_scenarios_webapp.png)

## <a name="fetch-the-openid-connect-metadata-document"></a>Ophalen van het metagegevensdocument voor OpenID Connect

OpenID Connect beschrijving van een document met metagegevens die de meeste van de informatie die nodig zijn voor een app uit te voeren aanmelden bevat. Dit omvat gegevens zoals de URL's te gebruiken en de locatie van de openbare ondersteuningssleutels van de service. Dit is de OpenID Connect-metagegevensdocument die moet u voor het v2.0-eindpunt:

```
https://login.microsoftonline.com/{tenant}/v2.0/.well-known/openid-configuration
```
> [!TIP]
> Probeer het nu! Klik op [ https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration ](https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration) om te zien de `common` tenants configuratie.

De `{tenant}` kan duren voordat een van de vier waarden:

| Waarde | Beschrijving |
| --- | --- |
| `common` |Gebruikers met zowel een persoonlijk Microsoft-account en een account voor werk- of schoolaccount van Azure Active Directory (Azure AD) kunnen aanmelden bij de toepassing. |
| `organizations` |Alleen gebruikers met een werk- of schoolaccounts van Azure AD kunnen zich aanmelden bij de toepassing. |
| `consumers` |Alleen gebruikers met een persoonlijk Microsoft-account kunnen aanmelden bij de toepassing. |
| `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` of `contoso.onmicrosoft.com` |Alleen gebruikers met een werk- of school-account van een specifieke Azure AD tenant kan zich aanmelden bij de toepassing. Ofwel de beschrijvende domeinnaam van de Azure AD-tenant of GUID-id van de tenant kan worden gebruikt. |

De metagegevens is een eenvoudige JavaScript Object Notation (JSON)-document. Zie het volgende fragment voor een voorbeeld. De inhoud van het codefragment worden volledig beschreven in de [OpenID Connect-specificatie](https://openid.net/specs/openid-connect-discovery-1_0.html#rfc.section.4.2).

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

Normaal gesproken zou u dit metagegevensdocument gebruiken om te configureren van een bibliotheek OpenID Connect of SDK. de bibliotheek gebruikt de metagegevens zijn werk te doen. Als u een vooraf gebouwde OpenID Connect-bibliotheek niet gebruikt, kunt u de stappen in de rest van dit artikel om uit te voeren aanmelden in een web-app met behulp van het v2.0-eindpunt volgen.

## <a name="send-the-sign-in-request"></a>De aanvraag voor aanmelding bij verzenden

Wanneer uw web-app moet de gebruiker te verifiëren, deze kunt instellen dat de gebruiker de `/authorize` eindpunt. Deze aanvraag is vergelijkbaar met de eerste zijde van de [OAuth 2.0-autorisatiecodestroom](v2-oauth2-auth-code-flow.md), met deze belangrijke verschillen:

* De aanvraag moet bevatten de `openid` in het bereik van de `scope` parameter.
* De `response_type` parameter moet bevatten `id_token`.
* De aanvraag moet bevatten de `nonce` parameter.

> [!IMPORTANT]
> Vragen om met succes is een ID-token, dat de app-registratie in de [registratieportal](https://apps.dev.microsoft.com) moet hebben de **[impliciete](v2-oauth2-implicit-grant-flow.md)** ingeschakeld voor de webclient. Als deze niet is ingeschakeld, een `unsupported_response` fout geretourneerd: "de opgegeven waarde voor de invoerparameter 'response_type' is niet toegestaan voor deze client. Verwachte waarde is "code" "

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
> Klik op de volgende koppeling voor het uitvoeren van deze aanvraag. Nadat u zich hebt aangemeld, kunt u uw browser wordt omgeleid naar https://localhost/myapp/, met een ID-token in de adresbalk. Let op: maakt gebruik van deze aanvraag `response_mode=fragment` (alleen ter demonstratie). Het is raadzaam dat u `response_mode=form_post`.
> <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=openid&response_mode=fragment&state=12345&nonce=678910" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/authorize...</a>

| Parameter | Voorwaarde | Beschrijving |
| --- | --- | --- |
| tenant |Vereist |U kunt de `{tenant}` waarde in het pad van de aanvraag om te bepalen wie kan zich aanmelden bij de toepassing. De toegestane waarden zijn `common`, `organizations`, `consumers`, en tenant-id's. Zie voor meer informatie, [protocol basisbeginselen](active-directory-v2-protocols.md#endpoints). |
| client_id |Vereist |De aanvraag-ID die de [Portal voor Appregistratie](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) toegewezen aan uw app. |
| response_type |Vereist |Moet bevatten `id_token` voor aanmelding OpenID Connect. Het kan ook andere omvatten `response_type` waarden, zoals `code`. |
| redirect_uri |Aanbevolen |De omleidings-URI van uw app, waarbij verificatiereacties kunnen worden verzonden en ontvangen door uw app. Het moet exact overeenkomen met een van de omleidings-URI's die u in de portal hebt geregistreerd, behalve dat het URL-codering moet worden. |
| scope |Vereist |Een door spaties gescheiden lijst met bereiken. Voor de OpenID Connect, moet deze het bereik bevatten `openid`, die wordt omgezet in de machtiging 'Aanmelden' in de gebruikersinterface voor toestemming. U kunt ook andere bereiken opnemen in deze aanvraag voor het aanvragen van toestemming. |
| nonce |Vereist |Een waarde die is opgenomen in de aanvraag, die worden gegenereerd door de app, die worden opgenomen in de resulterende waarde id_token als een claim. De app kunt controleren of deze waarde token opnieuw afspelen aanvallen te verkleinen. De waarde is doorgaans een willekeurige, unieke tekenreeks die kan worden gebruikt voor het identificeren van de oorsprong van de aanvraag. |
| response_mode |Aanbevolen |Hiermee geeft u de methode die moet worden gebruikt voor het verzenden van de resulterende autorisatiecode terug naar de app. Deze waarde kan `form_post` of `fragment` zijn. Voor webtoepassingen, wordt u aangeraden `response_mode=form_post`, om te controleren of de meest veilige overdracht van tokens aan uw toepassing. |
| state |Aanbevolen |Een waarde die is opgenomen in de aanvraag die ook in het token antwoord worden geretourneerd. Een tekenreeks van alle inhoud die u wilt dat kan zijn. Een willekeurig gegenereerde unieke waarde wordt meestal gebruikt voor het [cross-site-aanvraag kunnen worden vervalst aanvallen te voorkomen](https://tools.ietf.org/html/rfc6749#section-10.12). De status wordt ook gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat de verificatieaanvraag heeft plaatsgevonden, zoals de pagina of de weergave die de gebruiker was op. |
| prompt |Optioneel |Geeft het type tussenkomst van de gebruiker die is vereist. De enige geldige waarden op dit moment zijn `login`, `none`, en `consent`. De `prompt=login` claim zorgt ervoor dat de gebruiker zijn referenties invoeren voor deze aanvraag, die eenmalige aanmelding wordt genegeerd. De `prompt=none` claim is het tegenovergestelde. Deze claim zorgt ervoor dat de gebruiker niet wordt weergegeven met een interactieve prompt dan ook. Als de aanvraag kan niet op de achtergrond via eenmalige aanmelding worden voltooid, wordt er een fout geretourneerd in het v2.0-eindpunt. De `prompt=consent` claim wordt het dialoogvenster OAuth geactiveerd nadat de gebruiker zich aanmeldt. Het dialoogvenster waarin de gebruiker machtigingen verlenen voor de app. |
| login_hint |Optioneel |Gebruik deze parameter kunt u vooraf invullen van het veld gebruikersnaam en het e-adres van de aanmeldingspagina voor de gebruiker, als u bekend bent met de gebruikersnaam vooraf. Vaak apps deze parameter gebruiken tijdens de verificatie wordt uitgevoerd, nadat u hebt al de gebruikersnaam van een eerdere aanmelding uitgepakt met behulp van de `preferred_username` claim. |
| domain_hint |Optioneel |Deze waarde kan zijn `consumers` of `organizations`. Als opgenomen, wordt deze overgeslagen het detectieproces op basis van een e-mailbericht dat de gebruiker door op de v2.0-aanmelden pagina, voor een iets meer gestroomlijnde gebruikerservaring gaat. Vaak apps deze parameter gebruiken tijdens verificatie wordt uitgevoerd door te extraheren de `tid` claim van de ID-token. Als de `tid` claim waarde `9188040d-6c67-4c5b-b112-36a304b66dad` (de Microsoft-Account consument tenant), gebruikt u `domain_hint=consumers`. Gebruik anders `domain_hint=organizations`. |

Op dit moment wordt de gebruiker gevraagd zijn referenties invoeren en de verificatie voltooien. Het v2.0-eindpunt wordt gecontroleerd dat de gebruiker heeft ingestemd met de machtigingen die zijn aangegeven in de `scope` queryparameter. Als de gebruiker heeft niet ingestemd met een van deze machtigingen, vraagt het v2.0-eindpunt de gebruiker akkoord gaan met de vereiste machtigingen. U kunt meer lezen over [machtigingen en toestemming apps voor meerdere tenants](v2-permissions-and-consent.md).

Nadat de gebruiker wordt geverifieerd en verleent toestemming te geven, het v2.0-eindpunt retourneert een antwoord naar uw app op de aangegeven omleidings-URI met behulp van de methode die is opgegeven de `response_mode` parameter.

### <a name="successful-response"></a>Geslaagde reactie

Een geslaagde respons wanneer u `response_mode=form_post` ziet er als volgt:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&state=12345
```

| Parameter | Beschrijving |
| --- | --- |
| id_token |De ID-token dat de app worden aangevraagd. U kunt de `id_token` parameter om te controleren of de identiteit van de gebruiker en beginnen met een sessie met de gebruiker. Zie voor meer informatie over ID-tokens en de inhoud ervan, de [ `id_tokens` verwijzing](id-tokens.md). |
| state |Als een `state` parameter is opgenomen in de aanvraag, dezelfde waarde moet worden weergegeven in het antwoord. De app moet controleren of dat de provincie-waarden in de aanvraag en respons identiek zijn. |

### <a name="error-response"></a>Foutbericht

Foutberichten kunnen ook worden verzonden naar de omleidings-URI zodat ze kan worden verwerkt door de app. Reactie op een fout ziet er als volgt:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parameter | Beschrijving |
| --- | --- |
| error |Een tekenreeks voor de foutcode die u gebruiken kunt voor het classificeren van typen fouten die optreden en om te reageren op fouten. |
| error_description |Een bericht specifieke fout die u kan helpen de hoofdoorzaak van een verificatiefout identificeren. |

### <a name="error-codes-for-authorization-endpoint-errors"></a>Foutcodes voor endpoint-verificatiefouten

De volgende tabel worden foutcodes beschreven die kunnen worden geretourneerd in de `error` parameter van het foutbericht:

| Foutcode | Beschrijving | Clientactie |
| --- | --- | --- |
| invalid_request |Protocolfout, zoals een ontbrekende vereiste parameter. |Los en verzend de aanvraag opnieuw. Dit is een ontwikkeling-fout die meestal is aangetroffen tijdens de eerste test. |
| unauthorized_client |De clienttoepassing kan geen een autorisatiecode vragen. |Dit gebeurt meestal wanneer de clienttoepassing is niet geregistreerd in Azure AD of is niet toegevoegd aan Azure AD-tenant van de gebruiker. De toepassing kan de gebruiker met instructies voor het installeren van de toepassing en deze toevoegen aan Azure AD vragen. |
| ACCESS_DENIED |De resource-eigenaar geen toestemming. |De clienttoepassing kan de gebruiker die deze kan niet worden voortgezet, tenzij de gebruiker toestemming heeft melden. |
| unsupported_response_type |De autorisatie-server biedt geen ondersteuning voor het antwoord van het type in de aanvraag. |Los en verzend de aanvraag opnieuw. Dit is een ontwikkeling-fout die meestal is aangetroffen tijdens de eerste test. |
| server_error |De server heeft een onverwachte fout aangetroffen. |De aanvraag opnieuw. Deze fouten kunnen worden veroorzaakt door tijdelijke omstandigheden. De clienttoepassing mogelijk uitleggen aan de gebruiker dat de reactie is vertraagd vanwege een tijdelijke fout. |
| temporarily_unavailable |De server is tijdelijk bezet en kan de aanvraag te verwerken. |De aanvraag opnieuw. De clienttoepassing mogelijk uitleggen aan de gebruiker dat de reactie is vertraagd vanwege een tijdelijke situatie. |
| invalid_resource |De doelresource is ongeldig omdat deze niet bestaat, Azure AD kan deze niet vinden of deze is niet correct geconfigureerd. |Hiermee wordt aangegeven dat de resource als deze bestaat, niet is geconfigureerd in de tenant. De toepassing kan het bericht met instructies voor het installeren van de toepassing en toe te voegen aan Azure AD. |

## <a name="validate-the-id-token"></a>De ID-token te valideren

Alleen een id_token ontvangen is niet voldoende om te verifiëren van de gebruiker. u moet de handtekening van het id_token valideren en controleer of de claims in het token per vereisten van uw app. Maakt gebruik van het v2.0-eindpunt [JSON Web Tokens (JWTs)](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) en cryptografie met openbare sleutels voor het ondertekenen van tokens en controleren of ze geldig zijn.

U kunt kiezen om te valideren de `id_token` in client-code, maar een gebruikelijk is voor het verzenden van de `id_token` naar een back-endserver en er de validatie uit te voeren. Nadat u de handtekening van het id_token hebt gevalideerd, zijn er enkele claims u moet om te controleren. Zie de [ `id_token` verwijzing](id-tokens.md) voor meer informatie, met inbegrip van [valideren van Tokens](id-tokens.md#validating-an-idtoken) en [belangrijke informatie over de ondertekening van sleutelrollover](active-directory-signing-key-rollover.md). Het is raadzaam om gebruik van een bibliotheek voor het parseren en valideren van tokens: Er is ten minste één beschikbaar voor de meeste talen en platforms.
<!--TODO: Improve the information on this-->

U kunt ook om aanvullende claims, afhankelijk van uw scenario te valideren. Sommige algemene validaties zijn onder andere:

* Ervoor te zorgen dat de gebruiker/organisatie is geregistreerd voor de app.
* Ervoor te zorgen dat de gebruiker heeft juiste autorisatie/bevoegdheden
* Ervoor te zorgen dat een bepaalde sterkte van de verificatie is opgetreden, zoals meervoudige verificatie.

Nadat u hebt de id_token volledig gevalideerd, kunt u beginnen met een sessie met de gebruiker en de claims in de id_token gebruiken om informatie over de gebruiker in uw app te verkrijgen. Deze informatie kan worden gebruikt voor het weergeven, records, persoonlijke instellingen, enzovoort.

## <a name="send-a-sign-out-request"></a>Verzenden van een aanvragen voor afmelden

Wanneer u zich afmelden van de gebruiker van uw app wilt, is het niet voldoende om te wissen van cookies van uw app of anders wordt de gebruikerssessie te beëindigen. U moet de gebruiker ook omleiden naar het v2.0-eindpunt afmelden. Als u dit niet doet, vastgesteld de gebruiker aan uw app zonder hun referenties opnieuw in te voeren omdat er een geldige eenmalige aanmelding bij sessie met het v2.0-eindpunt.

U kunt de gebruiker omleiden de `end_session_endpoint` die worden vermeld in het metagegevensdocument voor OpenID Connect:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/logout?
post_logout_redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
```

| Parameter | Voorwaarde | Beschrijving |
| ----------------------- | ------------------------------- | ------------ |
| post_logout_redirect_uri | Aanbevolen | De URL die de gebruiker wordt omgeleid naar na het afmelden is. Als de parameter niet opgenomen is, wordt de gebruiker een algemeen bericht dat wordt gegenereerd door het v2.0-eindpunt weergegeven. Deze URL moet overeenkomen met een van de omleidings-URI's die zijn geregistreerd voor uw toepassing in de portal van de registratie van de app. |

## <a name="single-sign-out"></a>Eenmalige afmelding

Als u omleiden van de gebruiker de `end_session_endpoint`, het v2.0-eindpunt sessie vanuit de browser van de gebruiker worden gewist. Echter, de gebruiker kan nog steeds worden aangemeld bij andere toepassingen die gebruikmaken van Microsoft-accounts voor verificatie. Om in te schakelen die toepassingen aan te melden van de gebruiker om tegelijkertijd de v2.0 eindpunt een HTTP GET-aanvraag verzendt naar de geregistreerde `LogoutUrl` van alle toepassingen die de gebruiker die momenteel is aangemeld bij. Toepassingen moeten reageren op deze aanvraag door een sessie die u de gebruiker identificeert uit te schakelen en te retourneren een `200` antwoord. Als u ondersteunen eenmalige afmelding in uw toepassing wilt, moet u deze implementeren een `LogoutUrl` in de code van uw toepassing. U kunt instellen dat de `LogoutUrl` vanuit de portal van de registratie van de app.

## <a name="protocol-diagram-access-token-acquisition"></a>Diagram van protocol: toegang tot ophalen van tokens

Veel web-apps moeten niet alleen meldt u zich aan de gebruiker in, maar ook toegang tot een webservice namens de gebruiker met behulp van OAuth. In dit scenario combineert OpenID Connect voor verificatie van de gebruiker bij het ophalen van een autorisatiecode die u gebruiken kunt om toegangstokens als u van de OAuth-autorisatiecodestroom gebruikmaakt tegelijkertijd.

De volledige OpenID Connect aanmelden en token acquisition stroom lijkt op het volgende diagram. Wordt elke stap in de volgende secties van het artikel in detail beschreven.

![OpenID Connect-protocol: Token ophalen](./media/v2-protocols-oidc/convergence_scenarios_webapp_webapi.png)

## <a name="get-access-tokens"></a>Toegangstokens ophalen
Om te verkrijgen toegangstokens, wijzig de aanmeldingsaanvraag:

```
// Line breaks are for legibility only.

GET https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e        // Your registered Application ID
&response_type=id_token%20code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F       // Your registered redirect URI, URL encoded
&response_mode=form_post                              // 'form_post' or 'fragment'
&scope=openid%20                                      // Include both 'openid' and scopes that your app needs  
offline_access%20                                         
https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&state=12345                                          // Any value, provided by your app
&nonce=678910                                         // Any value, provided by your app
```

> [!TIP]
> Klik op de volgende koppeling voor het uitvoeren van deze aanvraag. Nadat u zich hebt aangemeld, uw browser wordt omgeleid naar https://localhost/myapp/, met een ID-token en een code in de adresbalk. Let op: maakt gebruik van deze aanvraag `response_mode=fragment` (alleen ter demonstratie). Het is raadzaam dat u `response_mode=form_post`.
> <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token%20code&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&response_mode=fragment&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&state=12345&nonce=678910" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/authorize...</a>
> 
> 

Door machtigingsbereiken te nemen in de aanvraag en met behulp van `response_type=id_token code`, het v2.0-eindpunt zorgt ervoor dat de gebruiker heeft ingestemd met de machtigingen die zijn aangegeven in de `scope` queryparameter. Retourneert een autorisatiecode aan uw app om te wisselen voor een toegangstoken.

### <a name="successful-response"></a>Geslaagde reactie

Een geslaagde respons van het gebruik van `response_mode=form_post` ziet er als volgt:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&state=12345
```

| Parameter | Beschrijving |
| --- | --- |
| id_token |De ID-token dat de app worden aangevraagd. U kunt de ID-token gebruiken om te controleren of de identiteit van de gebruiker en beginnen met een sessie met de gebruiker. U vindt meer informatie over ID-tokens en hun inhoud in de [ `id_tokens` verwijzing](id-tokens.md). |
| Code |De autorisatiecode die de app heeft aangevraagd. De app kan de autorisatiecode gebruiken om aan te vragen van een toegangstoken voor de doelresource. Er is een autorisatiecode zeer eenvoudige. Normaal gesproken verloopt een autorisatiecode over ongeveer tien minuten. |
| state |Als een parameter state is opgenomen in de aanvraag, dezelfde waarde moet worden weergegeven in het antwoord. De app moet controleren of dat de provincie-waarden in de aanvraag en respons identiek zijn. |

### <a name="error-response"></a>Foutbericht

Foutberichten kunnen ook worden verzonden naar de omleidings-URI zodat de app deze op de juiste wijze kan verwerken. Reactie op een fout ziet er als volgt:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parameter | Beschrijving |
| --- | --- |
| error |Een tekenreeks voor de foutcode die u gebruiken kunt voor het classificeren van typen fouten die optreden en om te reageren op fouten. |
| error_description |Een bericht specifieke fout die u kan helpen de hoofdoorzaak van een verificatiefout identificeren. |

Zie voor een beschrijving van de mogelijke foutcodes en aanbevolen clientantwoorden [foutcodes voor endpoint-verificatiefouten](#error-codes-for-authorization-endpoint-errors).

Wanneer u een autorisatiecode en een ID-token hebt, kunt u de gebruiker zich aanmelden en toegangstokens verkrijgen namens hen. De gebruiker in ondertekenen, moet u de ID-token valideren [precies zoals wordt beschreven](id-tokens.md#validating-an-idtoken). Toegangstokens, volg de stappen in [OAuth code flow-documentatie](v2-oauth2-auth-code-flow.md#request-an-access-token).
