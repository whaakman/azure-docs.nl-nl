---
title: 'Azure Active Directory B2C: Single-page-apps met behulp van de impliciete stroom | Microsoft Docs'
description: "Informatie over het bouwen van apps van één pagina rechtstreeks via impliciete OAuth 2.0-stroom met Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: mtillman
editor: parakhj
ms.assetid: a45cc74c-a37e-453f-b08b-af75855e0792
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: parakhj
ms.openlocfilehash: 2ce4aaac117920c1da0b8a29797169d536825c1a
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-ad-b2c-single-page-app-sign-in-by-using-oauth-20-implicit-flow"></a>Azure AD B2C: Single-page-app aanmelden met behulp van de impliciete OAuth 2.0-stroom

> [!NOTE]
> Deze functie is in preview.
> 

Veel moderne apps hebben een front-end app met één pagina die voornamelijk in JavaScript is geschreven. De app is vaak geschreven met behulp van een framework zoals AngularJS, Ember.js of Durandal. Apps van één pagina en andere JavaScript-apps die worden uitgevoerd in een browser voornamelijk hebt enkele aanvullende uitdagingen voor verificatie:

* De beveiligingskenmerken van deze apps zijn aanzienlijk anders dan traditionele webtoepassingen op basis van een server.
* Veel autorisatieservers en id-providers bieden geen ondersteuning voor cross-origin-resources (CORS) aanvragen om te delen.
* Volledige pagina browser omleidingen weg van de app kunnen aanzienlijk Invasief in de gebruikerservaring worden gemaakt.

Ter ondersteuning van deze toepassingen, Azure Active Directory B2C (Azure AD B2C) maakt gebruik van de impliciete OAuth 2.0-stroom. De OAuth 2.0 autorisatie impliciete grant-stroom wordt beschreven in [sectie 4.2 van de OAuth 2.0-specificatie](http://tools.ietf.org/html/rfc6749). In de impliciete flow, ontvangt de app tokens rechtstreeks vanuit de Azure Active Directory (Azure AD) eindpunt, zonder eventuele exchange server-naar-server te autoriseren. Alle verificatielogica en sessie wordt verwerkt volledig in de JavaScript-client zonder aanvullende paginaomleidingen plaatsen.

Azure AD B2C breidt de standaard impliciete OAuth 2.0-stroom aan meer dan een eenvoudige verificatie en autorisatie. Azure AD B2C introduceert de [Beleidsparameter](active-directory-b2c-reference-policies.md). Met de Beleidsparameter, kunt u OAuth 2.0 gebruikerservaringen zoals toevoegen aan uw app registreren, aanmelden en Profielbeheer. In dit artikel wordt beschreven hoe u de impliciete stroom en Azure AD gebruiken voor het implementeren van elk van deze ervaring in uw toepassingen met één pagina. Als u aan de slag wilt, Bekijk onze [Node.js](https://github.com/Azure-Samples/active-directory-b2c-javascript-singlepageapp-nodejs-webapi) en [Microsoft .NET](https://github.com/Azure-Samples/active-directory-b2c-javascript-singlepageapp-dotnet-webapi) voorbeelden.

In het voorbeeld HTTP-aanvragen in dit artikel, gebruiken we onze voorbeeld Azure AD B2C-directory **fabrikamb2c.onmicrosoft.com**. We gebruiken ook onze eigen voorbeeldtoepassing en het beleid. U kunt de aanvragen zelf met behulp van deze waarden, of kunt u deze vervangen door uw eigen waarden.
Meer informatie over hoe [ophalen van uw eigen Azure AD B2C-directory, toepassingen en beleidsregels](#use-your-own-b2c-tenant).


## <a name="protocol-diagram"></a>Protocol-diagram

De impliciete aanmelden stroom lijkt op de volgende afbeelding. Elke stap wordt in detail later in dit artikel beschreven.

![OpenID Connect banen](../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

## <a name="send-authentication-requests"></a>Verificatieaanvragen verzenden
Wanneer uw web-app moet de gebruiker verifiëren en uitvoeren van een beleid, stuurt deze door de gebruiker de `/authorize` eindpunt. Dit is het interactieve gedeelte van de stroom waarin de gebruiker wordt optreden, afhankelijk van het beleid. De gebruiker ontvangt een ID-token van het Azure AD-eindpunt.

In deze aanvraag is de client geeft aan de `scope` parameter de machtigingen die nodig zijn voor het verkrijgen van de gebruiker. In de `p` parameter, betekent dit het beleid uit te voeren. De volgende drie voorbeelden (met regeleinden voor leesbaarheid) elk gebruik een ander beleid. Voor een idee krijgen hoe elke aanvraag werkt, probeer de aanvraag in een browser te plakken en het uitvoeren.

### <a name="use-a-sign-in-policy"></a>Een beleid voor aanmelden gebruiken
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=id_token+token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=fragment
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_in
```

### <a name="use-a-sign-up-policy"></a>Een registratiebeleid gebruiken
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=id_token+token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=fragment
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_up
```

### <a name="use-an-edit-profile-policy"></a>Gebruik van een beleid voor het profiel bewerken
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=id_token+token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=fragment
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_edit_profile
```

| Parameter | Vereist? | Beschrijving |
| --- | --- | --- |
| client_id |Vereist |De toepassings-ID die is toegewezen aan uw app in de [Azure-portal](https://portal.azure.com). |
| response_type |Vereist |Moet bevatten `id_token` voor OpenID Connect aanmelden. Kan het ook het antwoordtype bevatten `token`. Als u `token`, uw app kunt onmiddellijk een toegangstoken van het eindpunt autoriseren ontvangen zonder het maken van een tweede aanvraag naar het geautoriseerde eindpunt.  Als u de `token` antwoordtype, de `scope` parameter moet een scope die welke resource aangeeft voor het uitgeven van het token voor bevatten. |
| redirect_uri |Aanbevolen |De omleidings-URI van uw app, waarbij verificatie reacties kunnen worden verzonden en ontvangen door uw app. Deze moet exact overeenkomen met een van de omleidings-URI's die u hebt geregistreerd in de portal, behalve dat het URL-codering moet worden. |
| response_mode |Aanbevolen |Hiermee geeft u de methode om te gebruiken voor het verzenden van het resulterende token terug naar uw app.  Gebruik voor impliciete stromen `fragment`. |
| Bereik |Vereist |Een door spaties gescheiden lijst met bereiken. Een waarde op één scope geeft u aan Azure AD zowel de machtigingen die worden aangevraagd. De `openid` bereik geeft aan dat een machtiging voor het aanmelden van de gebruiker en gegevens over de gebruiker in de vorm van een ID-tokens verkrijgen. (Bespreken we dit later in het artikel.) De `offline_access` bereik is optioneel voor web-apps. Hiermee wordt aangegeven dat uw app een vernieuwingstoken voor de lange levensduur hebben toegang tot bronnen moet. |
| state |Aanbevolen |Een waarde die is opgenomen in de aanvraag die ook in het token antwoord wordt geretourneerd. Het kan zijn dat een tekenreeks van inhoud die u wilt gebruiken. Normaal gesproken wordt een willekeurig gegenereerde unieke waarde gebruikt, om aanvraagvervalsing op meerdere sites aanvallen te voorkomen. De status wordt ook gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat de verificatieaanvraag heeft plaatsgevonden, zoals de pagina die ze op waren. |
| nonce |Vereist |Een waarde die is opgenomen in de aanvraag (gegenereerd door de app) die is opgenomen in het resulterende token ID als een claim. De app kunt vervolgens controleren of deze waarde om te beperken token replay-aanvallen. De waarde is meestal een willekeurige, unieke tekenreeks die kan worden gebruikt voor het identificeren van de oorsprong van de aanvraag. |
| P |Vereist |Het beleid uit te voeren. Dit is de naam van een beleid dat is gemaakt in uw Azure AD B2C-tenant. De waarde van de beleid-naam moet beginnen met **b2c\_1\_**. Zie voor meer informatie [ingebouwde beleid van Azure AD B2C](active-directory-b2c-reference-policies.md). |
| prompt |Optioneel |Het type van de interactie van de gebruiker die vereist zijn. De enige geldige waarde is momenteel `login`. Dit zorgt ervoor dat gebruikers hun referenties invoeren voor deze aanvraag. Eenmalige aanmelding wordt pas van kracht. |

Op dit moment wordt de gebruiker gevraagd om de werkstroom van het beleid te voltooien. Dit kan de gebruiker invoeren van hun gebruikersnaam en wachtwoord, aanmelden met een sociale identiteit aanmelden voor de map of andere aantal stappen inhouden. Acties van de gebruiker is afhankelijk van hoe het beleid wordt gedefinieerd.

Nadat de gebruiker het beleid voltooit, Azure AD retourneert een antwoord aan uw app op de waarde die u gebruikt voor `redirect_uri`. Gebruikt de methode die is opgegeven in de `response_mode` parameter. Het antwoord is exact hetzelfde zijn voor elk van de gebruiker actie scenario's, onafhankelijk van het beleid dat is uitgevoerd.

### <a name="successful-response"></a>Geslaagde reactie
Een geslaagde reactie die gebruikmaakt van `response_mode=fragment` en `response_type=id_token+token` met regeleinden voor de leesbaarheid van de volgende lijkt:

```
GET https://aadb2cplayground.azurewebsites.net/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&token_type=Bearer
&expires_in=3599
&scope="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
&id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=arbitrary_data_you_sent_earlier
```

| Parameter | Beschrijving |
| --- | --- |
| access_token |Het toegangstoken dat de app wordt aangevraagd.  Het toegangstoken moet niet worden gedecodeerd of anders geïnspecteerd. Het kan worden behandeld als één tekenreeks. |
| token_type |De waarde van het type token. Het enige type dat ondersteuning biedt voor Azure AD is Bearer. |
| expires_in |De lengte van de tijd die het toegangstoken ongeldig (in seconden is). |
| Bereik |De bereiken die de token geldig is. U kunt bereiken die in cache tokens gebruiken voor later gebruik. |
| id_token |Het token ID die de app wordt aangevraagd. U kunt het token ID gebruiken om te controleren van de identiteit van de gebruiker en beginnen met een sessie met de gebruiker. Zie voor meer informatie over het ID-tokens en de inhoud ervan, de [Azure AD B2C tokenverwijzing](active-directory-b2c-reference-tokens.md). |
| state |Als een `state` parameter is opgenomen in de aanvraag dezelfde waarde moet worden weergegeven in het antwoord. De app moet controleren of de `state` waarden in de aanvraag en antwoord identiek zijn. |

### <a name="error-response"></a>Foutbericht
Foutberichten kunnen ook worden verzonden naar de omleidings-URI zodat de app ze op de juiste wijze kan verwerken:

```
GET https://aadb2cplayground.azurewebsites.net/#
error=access_denied
&error_description=the+user+canceled+the+authentication
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parameter | Beschrijving |
| --- | --- |
| error |Een code fouttekenreeks gebruikt om te classificeren typen fouten die optreden. Ook kunt u de foutcode voor foutafhandeling. |
| error_description |Een specifiek foutbericht die u kan helpen de hoofdoorzaak van een verificatiefout identificeren. |
| state |Zie de volledige beschrijving in de voorgaande tabel. Als een `state` parameter is opgenomen in de aanvraag dezelfde waarde moet worden weergegeven in het antwoord. De app moet controleren of de `state` waarden in de aanvraag en antwoord identiek zijn.|

## <a name="validate-the-id-token"></a>Het token ID valideren
Ontvangen van een token ID is niet voldoende om de gebruiker te verifiëren. U moet de handtekening van het token ID valideren en controleer of de claims in het token per vereisten van uw app. Maakt gebruik van Azure AD B2C [JSON Web Tokens (JWTs)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) en cryptografie met openbare sleutels voor het ondertekenen van tokens en controleren of ze geldig zijn.

Veel open source-bibliotheken zijn beschikbaar voor het valideren van JWTs, afhankelijk van de taal die u wilt gebruiken. U kunt verkennen beschikbaar open source-bibliotheken in plaats van uw eigen validatielogica implementeren. U kunt de informatie in dit artikel kunt u informatie over het correct gebruik die bibliotheken.

Azure AD B2C heeft een eindpunt met OpenID Connect metagegevens. Een app kunt gebruiken van het eindpunt voor het ophalen van informatie over Azure AD B2C tijdens runtime. Deze informatie omvat eindpunten, inhoud van tokens en token-ondertekening sleutels. Er is een JSON-metagegevens-document voor elk beleid in uw Azure AD B2C-tenant. Bijvoorbeeld, bevindt het metagegevensdocument voor het beleid b2c_1_sign_in in de tenant fabrikamb2c.onmicrosoft.com zich op:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in`

Een van de eigenschappen van dit document configuratie is de `jwks_uri`. De waarde voor hetzelfde beleid zou zijn:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in`

Om te bepalen welk beleid is gebruikt voor het ondertekenen van een token ID (en waar u de metagegevens van ophalen), hebt u twee opties. Eerst de naam van het beleid is opgenomen in de `acr` claim in `id_token`. Zie voor informatie over het parseren van de claims van een token ID, de [Azure AD B2C tokenverwijzing](active-directory-b2c-reference-tokens.md). De andere optie is voor het coderen van het beleid in de waarde van de `state` parameter wanneer u de aanvraag verzendt. Vervolgens worden gedecodeerd de `state` parameter om te bepalen welk beleid is gebruikt. Beide methoden is geldig.

Nadat u het metagegevensdocument van het eindpunt van de metagegevens OpenID Connect hebt aangeschaft, kunt u de openbare sleutels van RSA-256 (dat zich op dit eindpunt) om de handtekening van het token ID te valideren. Er is mogelijk meerdere sleutels die worden vermeld op dit eindpunt op elk moment kunt herkennen aan een `kid`. De koptekst van `id_token` bevat ook een `kid` claim. Hiermee wordt aangegeven welke van deze sleutels is gebruikt voor het ondertekenen van het token ID. Voor meer informatie, waaronder informatie over [valideren van tokens](active-directory-b2c-reference-tokens.md#token-validation), Zie de [Azure AD B2C tokenverwijzing](active-directory-b2c-reference-tokens.md).
<!--TODO: Improve the information on this-->

Valideren van de handtekening van het token ID vereisen meerdere claims verificatie. Bijvoorbeeld:

* Valideer de `nonce` claim om token replayaanvallen te voorkomen. De waarde moet zijn opgegeven in de aanvraag aanmelden.
* Valideer de `aud` claim om ervoor te zorgen dat het ID-token is uitgegeven voor uw app. De waarde moet de toepassings-ID van uw app.
* Valideer de `iat` en `exp` claims om ervoor te zorgen dat het token ID niet is verlopen.

Verschillende meer validaties die moeten worden uitgevoerd zijn beschreven in de [OpenID Connect Core Spec](http://openid.net/specs/openid-connect-core-1_0.html). U kunt ook aanvullende verklaren, afhankelijk van uw scenario. Sommige algemene validaties zijn onder andere:

* Ervoor te zorgen dat de gebruiker of organisatie heeft aangemeld voor de app.
* Ervoor te zorgen dat de gebruiker heeft de juiste autorisatie en bevoegdheden.
* Ervoor te zorgen dat een bepaalde sterkte van verificatie heeft plaatsgevonden, zoals zoals met behulp van Azure multi-factor Authentication.

Zie voor meer informatie over de claims in een token ID, de [Azure AD B2C tokenverwijzing](active-directory-b2c-reference-tokens.md).

Nadat u hebt het token ID volledig gevalideerd, kunt u beginnen met een sessie met de gebruiker. Gebruik in uw app de claims in het token ID verkrijgen van informatie over de gebruiker. Deze informatie kan worden gebruikt voor weergave, registreert en autorisatie.

## <a name="get-access-tokens"></a>Toegangstokens ophalen
Als het enige dat uw web-apps moet doen uitvoeren beleidsregels, kunt u de volgende secties overslaan. De informatie in de volgende secties zijn alleen van toepassing op web-apps die moeten geverifieerde aanroepen maken naar een web-API en die worden beveiligd door Azure AD B2C.

Nu dat u hebt de gebruiker aangemeld bij uw app met één pagina, kunt u toegangstokens ophalen voor het aanroepen van web API's die zijn beveiligd door Azure AD. Zelfs als u al een token hebt ontvangen met behulp van de `token` antwoordtype, kunt u deze methode aan te schaffen tokens voor aanvullende bronnen zonder het omleiden van de gebruiker zich opnieuw aanmeldt.

In een typische web-app-stroom u doet dit door een aanvraag naar de `/token` eindpunt.  Echter, het eindpunt ondersteunt geen CORS-aanvragen, zodat het AJAX-aanroepen ophalen en vernieuwen van tokens kan niet worden gebruikt. In plaats daarvan kunt u de impliciete stroom in een verborgen iframe-element van HTML-nieuwe tokens krijgen voor andere web-API's. Hier volgt een voorbeeld: met regeleinden beter leesbaar:

```

https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&scope=https%3A%2F%2Fapi.contoso.com%2Ftasks.read
&response_mode=fragment
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&prompt=none
&domain_hint=organizations
&login_hint=myuser@mycompany.com
&p=b2c_1_sign_in
```

| Parameter | Vereist? | Beschrijving |
| --- | --- | --- |
| client_id |Vereist |De toepassings-ID die is toegewezen aan uw app in de [Azure-portal](https://portal.azure.com). |
| response_type |Vereist |Moet bevatten `id_token` voor OpenID Connect aanmelden.  Het kan ook betekenen dat het antwoordtype `token`. Als u `token` hier uw app kan onmiddellijk ontvangen een toegangstoken van het eindpunt autoriseren, zonder dat een tweede aanvraag naar het geautoriseerde eindpunt. Als u de `token` antwoordtype, de `scope` parameter moet een scope die welke resource aangeeft voor het uitgeven van het token voor bevatten. |
| redirect_uri |Aanbevolen |De omleidings-URI van uw app, waarbij verificatie reacties kunnen worden verzonden en ontvangen door uw app. Deze moet exact overeenkomen met een van de omleidings-URI's die u in de portal hebt geregistreerd, behalve dat het URL-codering moet worden. |
| Bereik |Vereist |Een door spaties gescheiden lijst met bereiken.  Voor het ophalen van tokens, zijn alle scopes die u nodig voor de bedoelde bron hebt. |
| response_mode |Aanbevolen |Hiermee geeft u de methode die wordt gebruikt voor het verzenden van het resulterende token terug naar uw app.  Kan `query`, `form_post`, of `fragment`. |
| state |Aanbevolen |Een waarde die is opgenomen in de aanvraag die in het token antwoord wordt geretourneerd.  Het kan zijn dat een tekenreeks van inhoud die u wilt gebruiken.  Normaal gesproken wordt een willekeurig gegenereerde unieke waarde gebruikt, om aanvraagvervalsing op meerdere sites aanvallen te voorkomen.  De status wordt ook gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat de verificatieaanvraag heeft plaatsgevonden. De pagina of de weergave moet de gebruiker is bijvoorbeeld op. |
| nonce |Vereist |Een waarde die is opgenomen in de aanvraag, die worden gegenereerd door de app, die is opgenomen in het resulterende token ID als een claim.  De app kunt vervolgens controleren of deze waarde om te beperken token replay-aanvallen. De waarde is meestal een willekeurige, unieke tekenreeks die aangeeft van de oorsprong van de aanvraag. |
| prompt |Vereist |Gebruiken om te vernieuwen en tokens verkrijgen in een verborgen iframe, `prompt=none` om ervoor te zorgen dat de iframe niet op de aanmeldingspagina hangen komt en wordt onmiddellijk geretourneerd. |
| login_hint |Vereist |Als u wilt vernieuwen en tokens verkrijgen in een verborgen iframe, de gebruikersnaam van de gebruiker opnemen in deze hint onderscheid maken tussen meerdere sessies van de gebruiker mogelijk op een bepaald moment. U kunt de gebruikersnaam extraheren uit een eerdere aanmelden met behulp van de `preferred_username` claim. |
| domain_hint |Vereist |Kan `consumers` of `organizations`.  Voor het vernieuwen en ophalen van tokens in een verborgen iframe, moet u opnemen de `domain_hint` waarde in de aanvraag.  Pak de `tid` claim uit het token ID van een eerdere aanmelden om te bepalen welke waarde om te gebruiken.  Als de `tid` claim waarde is `9188040d-6c67-4c5b-b112-36a304b66dad`, gebruik `domain_hint=consumers`.  Gebruik anders `domain_hint=organizations`. |

Door in te stellen de `prompt=none` parameter, deze aanvraag ofwel slaagt of mislukt onmiddellijk en retourneert aan uw toepassing.  Een geslaagde reactie wordt verzonden naar uw app op de aangegeven omleidings-URI, met behulp van de methode die is opgegeven in de `response_mode` parameter.

### <a name="successful-response"></a>Geslaagde reactie
Een geslaagde reactie met behulp van `response_mode=fragment` ziet er als volgt:

```
GET https://aadb2cplayground.azurewebsites.net/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=arbitrary_data_you_sent_earlier
&token_type=Bearer
&expires_in=3599
&scope=https%3A%2F%2Fapi.contoso.com%2Ftasks.read
```

| Parameter | Beschrijving |
| --- | --- |
| access_token |Het token dat de app wordt aangevraagd. |
| token_type |Het type token wordt altijd Bearer zijn. |
| state |Als een `state` parameter is opgenomen in de aanvraag dezelfde waarde moet worden weergegeven in het antwoord. De app moet controleren of de `state` waarden in de aanvraag en antwoord identiek zijn. |
| expires_in |Hoe lang het toegangstoken is ongeldig (in seconden). |
| Bereik |De bereiken die het toegangstoken is geldig voor. |

### <a name="error-response"></a>Foutbericht
Foutberichten kunnen ook worden verzonden naar de omleidings-URI zodat de app ze op de juiste wijze kan verwerken.  Voor `prompt=none`, een verwachte fout ziet er als volgt:

```
GET https://aadb2cplayground.azurewebsites.net/#
error=user_authentication_required
&error_description=the+request+could+not+be+completed+silently
```

| Parameter | Beschrijving |
| --- | --- |
| error |Een tekenreeks van de fout code die kan worden gebruikt voor het classificeren van typen fouten die optreden. U kunt ook de tekenreeks gebruiken om te reageren op fouten. |
| error_description |Een specifiek foutbericht die u kan helpen de hoofdoorzaak van een verificatiefout identificeren. |

Als u deze fout in de aanvraag iframe ontvangt, moet de gebruiker interactief aanmelden opnieuw voor het ophalen van een nieuw token. U kunt dit op een manier die zinvol is voor uw toepassing verwerkt.

## <a name="refresh-tokens"></a>Vernieuwen van tokens
ID-tokens en toegangstokens verlopen na een korte periode. Uw app moet worden voorbereid voor deze tokens periodiek vernieuwen.  Uitvoeren als u wilt vernieuwen beide typen token, dezelfde verborgen iframe aanvraag wordt gebruikt in een eerdere bijvoorbeeld met behulp van de `prompt=none` -parameter voor het beheren van Azure AD-stappen.  Voor het ontvangen van een nieuwe `id_token` waarde: Zorg ervoor dat u `response_type=id_token` en `scope=openid`, en een `nonce` parameter.

## <a name="send-a-sign-out-request"></a>Afmelden aanvraag verzenden
Als u wilt dat de gebruiker buiten de app te melden, moet u de gebruiker omgeleid naar Azure AD om af te melden. Als u dit doet, kan de gebruiker mogelijk om andere referenties voor uw app zonder hun referenties opnieuw invoeren. Dit komt doordat er een geldige eenmalige aanmelding sessie met Azure AD.

U kunt gewoon omleiden door de gebruiker de `end_session_endpoint` die worden vermeld in de dezelfde OpenID Connect metagegevensdocument dat wordt beschreven in [valideren van het token ID](#validate-the-id-token). Bijvoorbeeld:

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/logout?
p=b2c_1_sign_in
&post_logout_redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
```

| Parameter | Vereist? | Beschrijving |
| --- | --- | --- |
| P |Vereist |Het beleid moet worden gebruikt voor het ondertekenen van de gebruiker buiten uw toepassing. |
| post_logout_redirect_uri |Aanbevolen |De URL die de gebruiker wordt omgeleid naar na geslaagde afmelding plaatsvindt. Als dit niet opgenomen is, wordt Azure AD B2C een algemeen bericht voor de gebruiker. |

> [!NOTE]
> Doorsturen van de gebruiker de `end_session_endpoint` enkele van de status van de gebruiker één aanmelding met Azure AD B2C wordt gewist. Echter ondertekenen niet het de gebruiker buiten de sociale identiteit provider gebruikerssessie. Als de gebruiker selecteert dezelfde provider identificeren tijdens een volgende aanmelden, wordt de gebruiker is geverifieerd, zonder dat hun referenties invoeren. Als een gebruiker wil te melden bij uw Azure AD B2C-toepassing, betekent het niet noodzakelijkerwijs dat ze willen volledig afmelden bij hun account Facebook, bijvoorbeeld. Echter, voor lokale accounts de gebruikerssessie wordt beëindigd goed.
> 
> 

## <a name="use-your-own-azure-ad-b2c-tenant"></a>Gebruik uw eigen Azure AD B2C-tenant
Om te proberen deze aanvragen zelf, moet u de volgende drie stappen uitvoeren. Vervang de voorbeeldwaarden die we in dit artikel met uw eigen waarden gebruiken:

1. [Een Azure AD B2C-tenant maken](active-directory-b2c-get-started.md). Gebruik de naam van uw tenant in de aanvragen.
2. [Maken van een toepassing](active-directory-b2c-app-registration.md) verkrijgen van een toepassings-ID en een `redirect_uri` waarde. Een web-app of web-API opnemen in uw app. U kunt desgewenst een toepassingsgeheim maken.
3. [Maken van uw beleid](active-directory-b2c-reference-policies.md) om op te halen van de beleidsnamen van uw.

## <a name="samples"></a>Voorbeelden

* [Een app één pagina maken met behulp van Node.js](https://github.com/Azure-Samples/active-directory-b2c-javascript-singlepageapp-nodejs-webapi)
* [Een app één pagina maken met behulp van .NET](https://github.com/Azure-Samples/active-directory-b2c-javascript-singlepageapp-dotnet-webapi)

