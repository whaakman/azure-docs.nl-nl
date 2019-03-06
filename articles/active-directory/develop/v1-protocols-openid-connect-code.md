---
title: Inzicht in de codestroom OpenID Connect voor verificatie in Azure AD | Microsoft Docs
description: In dit artikel wordt beschreven hoe u HTTP-berichten gebruiken om toegang tot webtoepassingen en web-API's in uw tenant met behulp van Azure Active Directory en OpenID Connect te verlenen.
services: active-directory
documentationcenter: .net
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 29142f7e-d862-4076-9a1a-ecae5bcd9d9b
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/4/2019
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 281e1109964ac64853b8b82525579b7ff4de0d2f
ms.sourcegitcommit: 94305d8ee91f217ec98039fde2ac4326761fea22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57406402"
---
# <a name="authorize-access-to-web-applications-using-openid-connect-and-azure-active-directory"></a>Toegang verlenen aan webtoepassingen met OpenID Connect en Azure Active Directory

[OpenID Connect](https://openid.net/specs/openid-connect-core-1_0.html) is een eenvoudige identiteitslaag gebaseerd op het OAuth 2.0-protocol. OAuth 2.0 definieert mechanismen voor het verkrijgen en gebruiken van [ **toegangstokens** ](access-tokens.md) voor toegang tot beveiligde bronnen, maar ze niet bepalen standaardmethoden voor gegevens van identiteit. OpenID Connect, wordt verificatie geïmplementeerd als een uitbreiding van het autorisatieproces van OAuth 2.0. Het biedt informatie over de gebruiker in de vorm van een [ `id_token` ](id-tokens.md) die controleert of de identiteit van de gebruiker en basisprofielgegevens informatie over de gebruiker.

OpenID Connect is onze aanbeveling als u een webtoepassing die wordt gehost op een server en toegankelijk via een browser bouwt.


[!INCLUDE [active-directory-protocols-getting-started](../../../includes/active-directory-protocols-getting-started.md)] 

## <a name="authentication-flow-using-openid-connect"></a>Verificatiestroom waarbij OpenID Connect wordt gebruikt

De meest eenvoudige stroom aanmelden bevat de volgende stappen uit: elk van deze wordt beschreven in de hieronder beschreven.

![OpenId Connect-Verificatiestroom](./media/v1-protocols-openid-connect-code/active-directory-oauth-code-flow-web-app.png)

## <a name="openid-connect-metadata-document"></a>OpenID Connect-document met metagegevens

OpenID Connect beschrijving van een document met metagegevens die de meeste van de informatie die nodig zijn voor een app uit te voeren aanmelden bevat. Dit omvat gegevens zoals de URL's te gebruiken en de locatie van de openbare ondersteuningssleutels van de service. Het metagegevensdocument voor OpenID Connect kan worden gevonden op:

```
https://login.microsoftonline.com/{tenant}/.well-known/openid-configuration
```
De metagegevens is een eenvoudige JavaScript Object Notation (JSON)-document. Zie het volgende fragment voor een voorbeeld. De inhoud van het codefragment worden volledig beschreven in de [OpenID Connect-specificatie](https://openid.net). Houd er rekening mee dat die tenant in plaats van `common` in plaats van {tenant} bovenstaande zal leiden tot tenant-specifieke URI's in de JSON-object geretourneerd.

```
{
    "authorization_endpoint": "https://login.microsoftonline.com/common/oauth2/authorize",
    "token_endpoint": "https://login.microsoftonline.com/common/oauth2/token",
    "token_endpoint_auth_methods_supported":
    [
        "client_secret_post",
        "private_key_jwt",
        "client_secret_basic"
    ],
    "jwks_uri": "https://login.microsoftonline.com/common/discovery/keys"
    "userinfo_endpoint":"https://login.microsoftonline.com/{tenant}/openid/userinfo",
    ...
}
```

## <a name="send-the-sign-in-request"></a>De aanvraag voor aanmelding bij verzenden

Wanneer uw web-App moet de gebruiker te verifiëren, moet het de gebruiker directe de `/authorize` eindpunt. Deze aanvraag is vergelijkbaar met de eerste zijde van de [OAuth 2.0 machtiging Code Flow](v1-protocols-oauth-code.md), met enkele belangrijke verschillen:

* De aanvraag moet de scope bevatten `openid` in de `scope` parameter.
* De `response_type` parameter moet bevatten `id_token`.
* De aanvraag moet bevatten de `nonce` parameter.

Een voorbeeld van een aanvraag zou dit er als volgt:

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token
&redirect_uri=http%3A%2F%2Flocalhost%3a12345
&response_mode=form_post
&scope=openid
&state=12345
&nonce=7362CAEA-9CA5-4B43-9BA3-34D7C303EBA7
```

| Parameter |  | Description |
| --- | --- | --- |
| tenant |vereist |De `{tenant}` waarde in het pad van de aanvraag kan worden gebruikt om te bepalen wie zich bij de toepassing aanmelden kan. De toegestane waarden zijn tenant-id's, bijvoorbeeld `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` of `contoso.onmicrosoft.com` of `common` voor tenant-onafhankelijke tokens |
| client_id |vereist |De toepassings-Id die aan uw app wordt toegewezen wanneer u deze hebt geregistreerd bij Azure AD. U kunt dit vinden in de Azure Portal. Klik op **Azure Active Directory**, klikt u op **App-registraties**, kiest u de toepassing en zoekt u de Id op de pagina van de toepassing. |
| response_type |vereist |Moet bevatten `id_token` voor aanmelding OpenID Connect. Het kan ook andere response_types zoals `code` of `token`. |
| scope | Aanbevolen | De OpenID Connect-specificatie is vereist voor het bereik `openid`, die wordt omgezet in de machtiging 'Aanmelden' in de gebruikersinterface voor toestemming. Deze en andere OIDC-scopes op het eindpunt v1.0 worden genegeerd, maar is nog steeds een aanbevolen procedure voor standaarden-compatibele clients. |
| nonce |vereist |Een waarde die is opgenomen in de aanvraag, die worden gegenereerd door de app, die is opgenomen in de resulterende `id_token` als een claim. De app kunt vervolgens controleren of deze waarde token opnieuw afspelen aanvallen te verkleinen. De waarde is doorgaans een willekeurige, unieke tekenreeks of GUID die kan worden gebruikt voor het identificeren van de oorsprong van de aanvraag. |
| redirect_uri | Aanbevolen |De redirect_uri van uw app, waarbij verificatiereacties kunnen worden verzonden en ontvangen door uw app. Het moet een van de redirect_uris die u in de portal hebt geregistreerd, behalve het url-codering moet exact overeenkomen. Als ontbreekt, wordt de gebruikersagent worden verzonden naar een van de omleidings-URI's in willekeurige volgorde voor de app is geregistreerd. |
| response_mode |optioneel |Hiermee geeft u de methode die moet worden gebruikt voor het verzenden van de resulterende authorization_code terug naar de app. Ondersteunde waarden zijn `form_post` voor *HTTP formulier post* en `fragment` voor *URL-fragment*. Voor webtoepassingen, wordt u aangeraden `response_mode=form_post` om te controleren of de meest veilige overdracht van tokens aan uw toepassing. De standaardwaarde voor een stroom met inbegrip van een id_token `fragment`.|
| state |Aanbevolen |Een waarde die is opgenomen in de aanvraag die in het token antwoord wordt geretourneerd. Een tekenreeks van de inhoud die u wenst dat kan zijn. Een willekeurig gegenereerde unieke waarde wordt meestal gebruikt voor [cross-site-aanvraag kunnen worden vervalst aanvallen](https://tools.ietf.org/html/rfc6749#section-10.12). De status wordt ook gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat de verificatieaanvraag heeft plaatsgevonden, zoals de pagina of de weergave die ze al had geopend. |
| prompt |optioneel |Geeft het type tussenkomst van de gebruiker die is vereist. Op dit moment de enige geldige waarden zijn 'aanmelding', 'none', ' toestemming geven '. `prompt=login` Hiermee wordt de gebruiker zijn referenties invoeren voor deze aanvraag, zodat eenmalige aanmelding. `prompt=none` is het tegenovergestelde - Hiermee zorgt u ervoor dat de gebruiker niet wordt weergegeven met een interactieve prompt dan ook. Als de aanvraag kan niet op de achtergrond via eenmalige aanmelding worden voltooid, wordt in het eindpunt een fout geretourneerd. `prompt=consent` de OAuth-triggers toestemming dialoogvenster nadat de gebruiker zich heeft aangemeld, waarin de gebruiker wordt om de app-machtigingen te verlenen. |
| login_hint |optioneel |Kan worden gebruikt om het veld gebruikersnaam, e-mailadres van de aanmeldingspagina voor de gebruiker vooraf worden ingevuld als u hun gebruikersnaam tevoren weten. Vaak apps Gebruik deze parameter tijdens verificatie wordt uitgevoerd, de gebruikersnaam die al worden geëxtraheerd uit een vorige aanmelden met behulp van de `preferred_username` claim. |

Op dit moment wordt de gebruiker gevraagd zijn referenties invoeren en de verificatie voltooien.

### <a name="sample-response"></a>Voorbeeldantwoord

Een voorbeeldantwoord nadat de gebruiker is geverifieerd, kan er als volgt uit:

```
POST / HTTP/1.1
Host: localhost:12345
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&state=12345
```

| Parameter | Description |
| --- | --- |
| id_token |De `id_token` die de app heeft aangevraagd. U kunt de `id_token` om te controleren of de identiteit van de gebruiker en beginnen met een sessie met de gebruiker. |
| state |Een waarde die is opgenomen in de aanvraag die ook in het token antwoord wordt geretourneerd. Een willekeurig gegenereerde unieke waarde wordt meestal gebruikt voor [cross-site-aanvraag kunnen worden vervalst aanvallen](https://tools.ietf.org/html/rfc6749#section-10.12). De status wordt ook gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat de verificatieaanvraag heeft plaatsgevonden, zoals de pagina of de weergave die ze al had geopend. |

### <a name="error-response"></a>Foutbericht

Foutberichten kunnen ook worden verzonden naar de `redirect_uri` zodat de app deze op de juiste wijze kan verwerken:

```
POST / HTTP/1.1
Host: localhost:12345
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parameter | Description |
| --- | --- |
| error |Een tekenreeks voor de foutcode die kan worden gebruikt voor het classificeren van typen fouten die optreden en kan worden gebruikt om te reageren op fouten. |
| error_description |Een specifieke foutbericht dat een ontwikkelaar kan helpen de hoofdoorzaak van een verificatiefout identificeren. |

#### <a name="error-codes-for-authorization-endpoint-errors"></a>Foutcodes voor endpoint-verificatiefouten

De volgende tabel beschrijft de verschillende foutcodes die kunnen worden geretourneerd in de `error` parameter van het foutbericht.

| Foutcode | Description | Clientactie |
| --- | --- | --- |
| invalid_request |Protocolfout in, zoals een ontbrekende vereiste parameter. |Los en verzend de aanvraag opnieuw. Dit is een fout, ontwikkeling en is meestal aangetroffen tijdens de eerste test. |
| unauthorized_client |De clienttoepassing is niet toegestaan om aan te vragen van een autorisatiecode. |Dit gebeurt meestal wanneer de clienttoepassing is niet geregistreerd in Azure AD of is niet toegevoegd aan Azure AD-tenant van de gebruiker. De toepassing kan het bericht met instructies voor het installeren van de toepassing en toe te voegen aan Azure AD. |
| access_denied |Resource-eigenaar toestemming geweigerd |De clienttoepassing kan de gebruiker die deze kan niet worden voortgezet, tenzij de gebruiker toestemming heeft melden. |
| unsupported_response_type |De autorisatie-server biedt geen ondersteuning voor het antwoord van het type in de aanvraag. |Los en verzend de aanvraag opnieuw. Dit is een fout, ontwikkeling en is meestal aangetroffen tijdens de eerste test. |
| server_error |De server heeft een onverwachte fout aangetroffen. |De aanvraag opnieuw. Deze fouten kunnen worden veroorzaakt door tijdelijke omstandigheden. De clienttoepassing mogelijk uitleggen aan de gebruiker dat de reactie is vertraagd vanwege een tijdelijke fout. |
| temporarily_unavailable |De server is tijdelijk bezet en kan de aanvraag te verwerken. |De aanvraag opnieuw. De clienttoepassing mogelijk uitleggen aan de gebruiker dat de reactie is vertraagd vanwege een tijdelijke situatie. |
| invalid_resource |De doelresource is ongeldig omdat deze niet bestaat, Azure AD kan deze niet vinden of deze is niet correct geconfigureerd. |Dit geeft aan dat de resource als deze bestaat, is niet geconfigureerd in de tenant. De toepassing kan het bericht met instructies voor het installeren van de toepassing en toe te voegen aan Azure AD. |

## <a name="validate-the-idtoken"></a>De id_token valideren

Zojuist hebt ontvangen van een `id_token` is niet voldoende zijn voor het verifiëren van de gebruiker, moet u valideert de handtekening en controleer of de claims in de `id_token` per vereisten van uw app. De Azure AD-eindpunt maakt gebruik van JSON Web Tokens (JWTs) en cryptografie met openbare sleutels voor het ondertekenen van tokens en controleren of ze geldig zijn.

U kunt kiezen om te valideren de `id_token` in client-code, maar een gebruikelijk is voor het verzenden van de `id_token` naar een back-endserver en er de validatie uit te voeren. 

U kunt ook om aanvullende claims, afhankelijk van uw scenario te valideren. Sommige algemene validaties zijn onder andere:

* Ervoor te zorgen dat de gebruiker/organisatie is geregistreerd voor de app.
* Ervoor te zorgen dat de gebruiker heeft juiste autorisatie/bevoegdheden, met behulp van de `wids` of `roles` claims. 
* Ervoor te zorgen dat een bepaalde sterkte van de verificatie is opgetreden, zoals meervoudige verificatie.

Nadat u hebt gevalideerd de `id_token`, kunt u beginnen met een sessie met de gebruiker en de claims in de `id_token` om informatie over de gebruiker in uw app te verkrijgen. Deze informatie kan worden gebruikt voor het weergeven, records, persoonlijke instellingen, enzovoort. Voor meer informatie over `id_tokens` en claims, lezen [AAD id_tokens](id-tokens.md).

## <a name="send-a-sign-out-request"></a>Verzenden van een aanvragen voor afmelden

Als u de gebruiker buiten de app aanmelden wilt, is het niet voldoende om te wissen van de cookies of anderszins einde van uw app de sessie met de gebruiker. U moet ook omleiden van de gebruiker de `end_session_endpoint` voor afmelden. Als u niet om dit te doen, zich de gebruiker aan uw app verifiëren zonder hun referenties opnieuw in te voeren, omdat ze een geldig aanmeldings-sessie met de Azure AD-eindpunt.

U kunt gewoon de gebruiker omleiden de `end_session_endpoint` die worden vermeld in het metagegevensdocument voor OpenID Connect:

```
GET https://login.microsoftonline.com/common/oauth2/logout?
post_logout_redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F

```

| Parameter |  | Description |
| --- | --- | --- |
| post_logout_redirect_uri |Aanbevolen |De URL die de gebruiker moet worden omgeleid naar na geslaagde afmelden. Als niet is opgenomen, wordt de gebruiker een algemeen foutbericht weergegeven. |

## <a name="single-sign-out"></a>Eenmalige afmelding

Als u omleiden van de gebruiker de `end_session_endpoint`, Azure AD wordt de sessie van de browser van de gebruiker gewist. Echter, de gebruiker kan nog steeds worden aangemeld bij andere toepassingen die Azure AD voor verificatie gebruiken. Om in te schakelen die toepassingen naar de gebruiker tegelijk afmelden, Azure AD een HTTP GET-aanvraag verzendt naar de geregistreerde `LogoutUrl` van alle toepassingen die de gebruiker die momenteel is aangemeld bij. Toepassingen moeten reageren op deze aanvraag door een sessie die u de gebruiker identificeert uit te schakelen en te retourneren een `200` antwoord. Als u uitzoomen ondersteuning van eenmalige aanmelding in uw toepassing wilt, moet u deze implementeren een `LogoutUrl` in de code van uw toepassing. U kunt instellen dat de `LogoutUrl` vanuit Azure portal:

1. Navigeer naar de [Azure-Portal](https://portal.azure.com).
2. Kies uw Active Directory door te klikken op uw account in de rechterbovenhoek van de pagina.
3. Kies in het navigatievenster links **Azure Active Directory**, en kies vervolgens **App-registraties** en selecteer uw toepassing.
4. Klik op **instellingen**, klikt u vervolgens **eigenschappen** en zoek de **afmeldings-URL van** in het tekstvak. 

## <a name="token-acquisition"></a>Ophalen van tokens
Veel web-apps moeten niet alleen Meld u aan de gebruiker in, maar ook toegang tot een webservice die namens met OAuth. In dit scenario combineert OpenID Connect voor verificatie van de gebruiker bij het ophalen van tegelijkertijd een `authorization_code` die kunnen worden gebruikt om op te halen `access_tokens` met behulp van de [OAuth Authorization Code Flow](v1-protocols-oauth-code.md#use-the-authorization-code-to-request-an-access-token).

## <a name="get-access-tokens"></a>Toegangstokens ophalen
Om toegangstokens verkrijgen, moet u de aanmeldingsaanvraag van bovenstaande wijzigen:

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e        // Your registered Application Id
&response_type=id_token+code
&redirect_uri=http%3A%2F%2Flocalhost%3a12345          // Your registered Redirect Uri, url encoded
&response_mode=form_post                              // `form_post' or 'fragment'
&scope=openid
&resource=https%3A%2F%2Fservice.contoso.com%2F        // The identifier of the protected resource (web API) that your application needs access to
&state=12345                                          // Any value, provided by your app
&nonce=678910                                         // Any value, provided by your app
```

Door te nemen van de machtigingsbereiken in de aanvraag en met behulp van `response_type=code+id_token`, wordt de `authorize` eindpunt zorgt ervoor dat de gebruiker heeft ingestemd met de machtigingen die zijn aangegeven in de `scope` queryparameter en uw app een autorisatiecode uitwisselen voor retourneren een toegangstoken.

### <a name="successful-response"></a>Geslaagde reactie

Een geslaagde respons met `response_mode=form_post` ziet eruit als:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&state=12345
```

| Parameter | Description |
| --- | --- |
| id_token |De `id_token` die de app heeft aangevraagd. U kunt de `id_token` om te controleren of de identiteit van de gebruiker en beginnen met een sessie met de gebruiker. |
| code |De authorization_code die de app heeft aangevraagd. De app kan de autorisatiecode gebruiken om aan te vragen van een toegangstoken voor de doelresource. Authorization_codes zijn korte duur en normaal verlopen na ongeveer 10 minuten. |
| state |Als een parameter state is opgenomen in de aanvraag, dezelfde waarde moet worden weergegeven in het antwoord. De app moet controleren of dat de provincie-waarden in de aanvraag en respons identiek zijn. |

### <a name="error-response"></a>Foutbericht

Foutberichten kunnen ook worden verzonden naar de `redirect_uri` zodat de app deze op de juiste wijze kan verwerken:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parameter | Description |
| --- | --- |
| error |Een tekenreeks voor de foutcode die kan worden gebruikt voor het classificeren van typen fouten die optreden en kan worden gebruikt om te reageren op fouten. |
| error_description |Een specifieke foutbericht dat een ontwikkelaar kan helpen de hoofdoorzaak van een verificatiefout identificeren. |

Zie voor een beschrijving van de mogelijke foutcodes en de aanbevolen clientactie [foutcodes voor endpoint-verificatiefouten](#error-codes-for-authorization-endpoint-errors).

Nadat u akkoord bent gegaan met een autorisatie `code` en een `id_token`, u kunt de gebruiker zich aanmeldt en [toegangstokens](access-tokens.md) namens hen. De gebruiker in ondertekenen, moet u valideren de `id_token` exact zoals hierboven is beschreven. Als u toegangstokens, u kunt de stappen beschreven in de sectie "De autorisatiecode gebruiken om aan te vragen van een toegangstoken" van onze [OAuth code flow-documentatie](v1-protocols-oauth-code.md#use-the-authorization-code-to-request-an-access-token).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over de [toegangstokens](access-tokens.md).
* Meer informatie over de [ `id_token` en claims](id-tokens.md).
