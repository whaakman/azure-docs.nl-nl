---
title: Inzicht in de stroom OpenID Connect verificatie code in Azure AD | Microsoft Docs
description: Dit artikel wordt beschreven hoe u met HTTP-berichten toestaan van toegang tot webtoepassingen en web-API's in uw tenant met behulp van Azure Active Directory en OpenID Connect.
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 29142f7e-d862-4076-9a1a-ecae5bcd9d9b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 1bb944997caa0c43354e82bf9b1a70e3e104a476
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# Toegang tot webtoepassingen die gebruikmaken van OpenID Connect en Azure Active Directory autoriseren
[OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html) is een eenvoudige identiteitlaag gebouwd op het OAuth 2.0-protocol. OAuth 2.0 definieert mechanismen voor het verkrijgen en gebruiken van **toegang tot tokens** voor toegang tot beveiligde bronnen, maar ze niet bepalen standaardmethoden om identiteit informatie te geven. OpenID Connect implementeert verificatie als een uitbreiding van het autorisatieproces OAuth 2.0. Biedt informatie over de gebruiker in de vorm van een `id_token` die de identiteit van de gebruiker wordt geverifieerd en basisprofiel informatie over de gebruiker.

Onze aanbeveling OpenID Connect worden is als u een webtoepassing die wordt gehost op een server en is toegankelijk via een browser bouwt.


[!INCLUDE [active-directory-protocols-getting-started](../../../includes/active-directory-protocols-getting-started.md)] 

## Verificatiestroom met OpenID Connect
De meest eenvoudige aanmelding-stroom bevat de volgende stappen: elk van deze hieronder in detail wordt beschreven.

![OpenId Connect Verificatiestroom](media/active-directory-protocols-openid-connect-code/active-directory-oauth-code-flow-web-app.png)

## Metagegevensdocument OpenID Connect

Beschrijving van een document met metagegevens die de meeste van de vereiste informatie voor een app uit te voeren aanmelden bevat OpenID Connect. Dit omvat gegevens zoals de URL's te gebruiken en de locatie van de openbare handtekeningsleutels van de service. Het metagegevensdocument OpenID Connect kan worden gevonden op:

```
https://login.microsoftonline.com/{tenant}/.well-known/openid-configuration
```
De metagegevens is een eenvoudige JSON JavaScript Object Notation ()-document. Zie het volgende fragment voor een voorbeeld. De inhoud van het codefragment is volledig worden beschreven in de [specificatie van het OpenID Connect](https://openid.net).

```
{
    "authorization_endpoint": "https://login.microsoftonline.com/common/oauth2/authorize",
    "token_endpoint": "https://login.microsoftonline.com/common/oauth2/token",
    "token_endpoint_auth_methods_supported":
    [
        "client_secret_post",
        "private_key_jwt"
    ],
    "jwks_uri": "https://login.microsoftonline.com/common/discovery/keys"
    
    ...
}
```

## Verzending van de aanvraag voor aanmelden
Wanneer uw webtoepassing moet de gebruiker te verifiëren, moet het door de gebruiker directe de `/authorize` eindpunt. Deze aanvraag is vergelijkbaar met de eerste zijde van de [OAuth 2.0 autorisatie Code stromen](active-directory-protocols-oauth-code.md), met enkele belangrijke verschillen:

* De aanvraag vergezeld gaan van het bereik `openid` in de `scope` parameter.
* De `response_type` vergezeld gaan van de parameter `id_token`.
* De aanvraag moet bevatten de `nonce` parameter.

Dus eruit als volgt een voorbeeld van een aanvraag:

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=form_post
&scope=openid
&state=12345
&nonce=7362CAEA-9CA5-4B43-9BA3-34D7C303EBA7
```

| Parameter |  | Beschrijving |
| --- | --- | --- |
| Tenant |Vereist |De `{tenant}` waarde in het pad van de aanvraag kan worden gebruikt om te bepalen wie bij de toepassing aanmelden kunt.  De toegestane waarden zijn tenant-id's, bijvoorbeeld `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` of `contoso.onmicrosoft.com` of `common` voor tenant-onafhankelijke tokens |
| client_id |Vereist |De toepassings-Id toegewezen aan uw app bij de registratie met Azure AD. U kunt dit vinden in de Azure Portal. Klik op **Azure Active Directory**, klikt u op **App registraties**, kies de toepassing en zoekt u de Id op de pagina van de toepassing. |
| response_type |Vereist |Moet bevatten `id_token` voor OpenID Connect aanmelden.  Het kan ook betekenen dat andere response_types zoals `code`. |
| Bereik |Vereist |Een door spaties gescheiden lijst met bereiken.  Voor het OpenID Connect, het bereik moet bevatten `openid`, die wordt omgezet in de machtiging 'Aanmelden u' in de gebruikersinterface van de toestemming.  U mogelijk ook andere bereiken opnemen in deze aanvraag voor het aanvragen van toestemming. |
| nonce |Vereist |Een waarde die is opgenomen in de aanvraag, die worden gegenereerd door de app, die is opgenomen in de resulterende `id_token` als een claim.  De app kunt vervolgens controleren of deze waarde om te beperken token replay-aanvallen.  De waarde is meestal een unieke tekenreeks van willekeurige of GUID die kan worden gebruikt voor het identificeren van de oorsprong van de aanvraag. |
| redirect_uri |Aanbevolen |De redirect_uri van uw app, waarbij verificatie reacties kunnen worden verzonden en ontvangen door uw app.  Er moet een van de redirect_uris die u in de portal hebt geregistreerd, behalve het url-codering moet exact overeenkomen. |
| response_mode |Aanbevolen |Hiermee geeft u de methode die moet worden gebruikt voor het verzenden van de resulterende authorization_code terug naar uw app.  Ondersteunde waarden zijn `form_post` voor *HTTP Formulierbericht* of `fragment` voor *URL-fragment*.  Voor webtoepassingen, wordt u aangeraden `response_mode=form_post` om te controleren of de meest beveiligde overdracht van tokens voor uw toepassing. |
| state |Aanbevolen |Een waarde die is opgenomen in de aanvraag die in het token antwoord wordt geretourneerd.  Een tekenreeks van inhoud die u wenst dat kan zijn.  Een willekeurig gegenereerde unieke waarde wordt doorgaans gebruikt voor [voorkomen van aanvraagvervalsing op meerdere sites aanvallen](http://tools.ietf.org/html/rfc6749#section-10.12).  De status wordt ook gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat het verificatieverzoek opgetreden, zoals de pagina of de weergave op. |
| prompt |Optioneel |Geeft het type van de interactie van de gebruiker die is vereist.  Op dit moment de enige geldige waarden zijn 'aanmelding', 'none', ' toestemming geven '.  `prompt=login`Hiermee wordt de gebruiker gevraagd hun referenties op die aanvraag, het negatief maken van eenmalige aanmelding.  `prompt=none`is het tegenovergestelde - zorgt ervoor dat de gebruiker niet vragen beeïndigen krijgt.  Als de aanvraag kan achtergrond via eenmalige aanmelding worden voltooid, wordt door het eindpunt een fout geretourneerd.  `prompt=consent`de OAuth-triggers toestemming dialoogvenster nadat de gebruiker zich aanmeldt, de gebruiker machtigen om de app wordt gevraagd. |
| login_hint |Optioneel |Kan worden gebruikt voor het veld gebruikersnaam, e-mailadres van de aanmeldingspagina voor de gebruiker vooraf worden ingevuld als u hun gebruikersnaam tevoren weten.  Vaak apps Gebruik deze parameter tijdens verificatie wordt uitgevoerd, de gebruikersnaam die al worden opgehaald uit een eerdere aanmelden met de `preferred_username` claim. |

Op dit punt wordt wordt de gebruiker gevraagd om hun referenties invoeren en de verificatie te voltooien.

### Voorbeeldreactie
Een voorbeeldantwoord kan nadat de gebruiker is geverifieerd, uitzien:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&state=12345
```

| Parameter | Beschrijving |
| --- | --- |
| id_token |De `id_token` die de app wordt aangevraagd. U kunt de `id_token` om te controleren van de identiteit van de gebruiker en beginnen met een sessie met de gebruiker. |
| state |Een waarde die is opgenomen in de aanvraag die ook in het token antwoord wordt geretourneerd. Een willekeurig gegenereerde unieke waarde wordt doorgaans gebruikt voor [voorkomen van aanvraagvervalsing op meerdere sites aanvallen](http://tools.ietf.org/html/rfc6749#section-10.12).  De status wordt ook gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat het verificatieverzoek opgetreden, zoals de pagina of de weergave op. |

### Foutbericht
Foutberichten kunnen ook worden verzonden naar de `redirect_uri` zodat de app ze op de juiste wijze kan verwerken:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parameter | Beschrijving |
| --- | --- |
| error |Een tekenreeks van de fout code die kan worden gebruikt voor het classificeren van typen fouten die optreden en kan worden gebruikt om te reageren op fouten. |
| error_description |Een specifieke foutbericht dat een ontwikkelaar kan helpen de hoofdoorzaak van een verificatiefout identificeren. |

#### Foutcodes voor autorisatie eindpunt fouten
De volgende tabel beschrijft de verschillende foutcodes die kunnen worden geretourneerd in de `error` parameter van het foutbericht.

| Foutcode | Beschrijving | Clientactie |
| --- | --- | --- |
| invalid_request |Protocolfout, zoals een ontbrekende vereiste parameter. |Herstel en verzend de aanvraag opnieuw. Dit is een fout voor ontwikkeling en meestal wordt onderschept tijdens de eerste test. |
| unauthorized_client |De clienttoepassing is niet toegestaan om aan te vragen een autorisatiecode. |Dit gebeurt meestal wanneer de clienttoepassing is niet geregistreerd in Azure AD of is niet toegevoegd aan Azure AD-tenant van de gebruiker. De toepassing kan het bericht met instructies voor het installeren van de toepassing en deze naar Azure AD toe te voegen. |
| ACCESS_DENIED |Resource-eigenaar toestemming geweigerd |De clienttoepassing kan de gebruiker die deze kan niet worden voortgezet tenzij de gebruiker akkoord gaat melden. |
| unsupported_response_type |De autorisatie-server biedt geen ondersteuning voor het antwoordtype in de aanvraag. |Herstel en verzend de aanvraag opnieuw. Dit is een fout voor ontwikkeling en meestal wordt onderschept tijdens de eerste test. |
| server_error |De server heeft een onverwachte fout aangetroffen. |De aanvraag opnieuw proberen. Deze fouten kunnen worden veroorzaakt door tijdelijke omstandigheden. Aan de gebruiker kan de clienttoepassing verklaren dat het antwoord is vertraagd vanwege een tijdelijke fout. |
| temporarily_unavailable |De server is tijdelijk te druk bezet om de aanvraag te verwerken. |De aanvraag opnieuw proberen. Aan de gebruiker kan de clienttoepassing verklaren dat het antwoord is vertraagd doordat een tijdelijke situatie. |
| invalid_resource |De doelresource is ongeldig omdat deze niet bestaat, Azure AD kan niet worden gevonden of is niet correct geconfigureerd. |Dit betekent dat de resource als deze bestaat, is niet geconfigureerd in de tenant. De toepassing kan het bericht met instructies voor het installeren van de toepassing en deze naar Azure AD toe te voegen. |

## De id_token valideren
Zojuist hebt ontvangen van een `id_token` is niet voldoende zijn voor het verifiëren van de gebruiker, moet u de handtekening valideren en controleer of de claims in de `id_token` per vereisten van uw app. Het Azure AD-eindpunt maakt gebruik van JSON Web Tokens (JWTs) en cryptografie met openbare sleutels voor het ondertekenen van tokens en controleren of ze geldig zijn.

U kunt kiezen om te valideren de `id_token` in client-code, maar een gangbare optie is om te verzenden de `id_token` naar een back-endserver en er de validatie uit te voeren. Zodra u hebt gecontroleerd dat de handtekening van de `id_token`, er zijn enkele claims die u nodig zijn om te controleren.

Mogelijk wilt u ook aanvullende verklaren afhankelijk van uw scenario. Sommige algemene validaties zijn onder andere:

* Gezorgd/organisatie van de gebruiker is aangemeld bij de app.
* De gebruiker ervoor heeft juiste autorisatie/bevoegdheden
* Een bepaalde sterkte van verificatie gezorgd is opgetreden, zoals multi-factor authentication-server.

Zodra u hebt gevalideerd de `id_token`, kunt u beginnen met een sessie met de gebruiker en gebruiken van de claims in de `id_token` om informatie over de gebruiker in uw app te verkrijgen. Deze informatie kan worden gebruikt voor weergave, registreert, autorisaties, enzovoort. Lees voor meer informatie over de typen tokens en claims [ondersteund Token en claimtypen](active-directory-token-and-claims.md).

## Afmelden aanvraag verzenden
Wanneer u wenst dat de gebruiker buiten de app te melden, is het niet voldoende om te wissen van cookies of anderszins einde van uw app de sessie met de gebruiker.  U moet ook omleiden door de gebruiker de `end_session_endpoint` voor afmelden.  Als u niet om dit te doen, zich de gebruiker om andere referenties voor uw app zonder hun referenties opnieuw invoeren omdat er een geldige eenmalige aanmelding sessie met het Azure AD-eindpunt.

U kunt gewoon omleiden door de gebruiker de `end_session_endpoint` vermeld in het document met OpenID Connect metagegevens:

```
GET https://login.microsoftonline.com/common/oauth2/logout?
post_logout_redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F

```

| Parameter |  | Beschrijving |
| --- | --- | --- |
| post_logout_redirect_uri |Aanbevolen |De URL waarnaar de gebruiker wordt omgeleid naar na geslaagde afmelden.  Als niet is opgenomen, wordt de gebruiker een algemeen foutbericht weergegeven. |

## Eenmalige afmelding
Wanneer het omleiden van de gebruiker de `end_session_endpoint`, Azure AD de gebruikerssessie vanuit de browser wordt gewist. Echter, de gebruiker mogelijk nog steeds zijn aangemeld bij andere toepassingen die Azure AD voor verificatie gebruiken. Om deze toepassingen die de gebruiker tegelijkertijd afmelden, Azure AD een HTTP GET-aanvraag verzendt naar de geregistreerde `LogoutUrl` van alle toepassingen die de gebruiker momenteel is aangemeld bij. Toepassingen op deze aanvraag moeten reageren door alle sessies waarin de gebruiker te wissen en retourneren een `200` antwoord.  Als u afmelden ondersteuning voor eenmalige aanmelding in uw toepassing wilt, moet u deze implementeren een `LogoutUrl` in de code van uw toepassing.  U kunt instellen de `LogoutUrl` vanuit de Azure-portal:

1. Navigeer naar de [Azure-Portal](https://portal.azure.com).
2. Kies uw Active Directory door te klikken op uw account in de rechterbovenhoek van de pagina.
3. Kies in het navigatievenster aan de linkerkant **Azure Active Directory**, en kies vervolgens **App registraties** en selecteer uw toepassing.
4. Klik op **eigenschappen** en zoek de **afmelding URL** in het tekstvak. 

## Token verkrijgen
Veel WebApps moeten niet alleen Meld u aan de gebruiker in, maar ook toegang tot een webservice namens een gebruiker met OAuth. Dit scenario combineert OpenID Connect voor de verificatie bij het ophalen van tegelijkertijd een `authorization_code` die kunnen worden gebruikt om op te halen `access_tokens` met behulp van OAuth Authorization Code stromen.

## Toegangstokens ophalen
Te verkrijgen toegangstokens, moet u de aanmeldingsverzoek van bovenaf wijzigen:

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e        // Your registered Application Id
&response_type=id_token+code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F       // Your registered Redirect Uri, url encoded
&response_mode=form_post                              // form_post', or 'fragment'
&scope=openid
&resource=https%3A%2F%2Fservice.contoso.com%2F                                     
&state=12345                                          // Any value, provided by your app
&nonce=678910                                         // Any value, provided by your app
```

Door te nemen machtigingsbereiken in de aanvraag en `response_type=code+id_token`, wordt de `authorize` eindpunt zorgt ervoor dat de gebruiker heeft ingestemd met de machtigingen die zijn aangegeven in de `scope` queryparameter en retourcode uw app een machtiging voor het uitwisselen van voor een toegangstoken.

### Geslaagde reactie
Een geslaagde reactie met `response_mode=form_post` lijkt:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&state=12345
```

| Parameter | Beschrijving |
| --- | --- |
| id_token |De `id_token` die de app wordt aangevraagd. U kunt de `id_token` om te controleren van de identiteit van de gebruiker en beginnen met een sessie met de gebruiker. |
| code |De authorization_code die de app wordt aangevraagd. De app kunt u de autorisatiecode gebruiken om aan te vragen van een toegangstoken voor de doelresource. Authorization_codes korte worden gehouden en normaal verlopen na ongeveer 10 minuten. |
| state |Als een parameter state is opgenomen in de aanvraag, moet dezelfde waarde weergegeven in het antwoord. De app dient te verifiëren dat de statuswaarden in de aanvraag en antwoord identiek zijn. |

### Foutbericht
Foutberichten kunnen ook worden verzonden naar de `redirect_uri` zodat de app ze op de juiste wijze kan verwerken:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parameter | Beschrijving |
| --- | --- |
| error |Een tekenreeks van de fout code die kan worden gebruikt voor het classificeren van typen fouten die optreden en kan worden gebruikt om te reageren op fouten. |
| error_description |Een specifieke foutbericht dat een ontwikkelaar kan helpen de hoofdoorzaak van een verificatiefout identificeren. |

Zie voor een beschrijving van de mogelijke foutcodes en hun aanbevolen clientactie [foutcodes voor autorisatie eindpunt fouten](#error-codes-for-authorization-endpoint-errors).

Als u toegang hebt verkregen van een vergunning `code` en een `id_token`, kunt u de gebruiker aanmelden en toegangstokens ophalen in hun naam.  De gebruiker in ondertekenen, moet u controleren de `id_token` precies zoals hierboven is beschreven. Als u toegangstokens, kunt u de stappen in de sectie "De autorisatiecode gebruiken om aan te vragen van een toegangstoken" van onze [OAuth-protocoldocumentatie](active-directory-protocols-oauth-code.md).
