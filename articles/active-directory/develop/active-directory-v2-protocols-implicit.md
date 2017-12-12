---
title: "Beveiligen van één pagina van toepassingen via de Azure AD v2.0 impliciete stroom | Microsoft Docs"
description: "Gebouw webtoepassingen die gebruikmaken van Azure AD v2.0 uitvoering van de impliciete stroom voor apps van één pagina."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mtillman
editor: 
ms.assetid: 3605931f-dc24-4910-bb50-5375defec6a8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 7ecc9de6a9eb910ac4c31290710530555441890d
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="v20-protocols---spas-using-the-implicit-flow"></a>v2.0 protocollen - kuuroorden met behulp van de impliciete stroom
Met het v2.0-eindpunt kunt u gebruikers zich bij uw apps op één pagina met persoonlijke en zakelijke/school accounts van Microsoft.  Één pagina en andere JavaScript-apps die worden uitgevoerd voornamelijk in een browser gezicht interessante enkele uitdagingen bij de verificatie:

* De beveiligingskenmerken van deze apps zijn aanzienlijk anders dan traditionele server op basis van webtoepassingen.
* Veel autorisatieservers & id-providers bieden geen ondersteuning voor CORS-aanvragen.
* Volledige pagina browser wordt omgeleid weg van de app worden met name Invasief in de gebruikerservaring.

Voor deze toepassingen (nadenkt: AngularJS, Ember.js, React.js, enzovoort) de impliciete OAuth 2.0-Grant-stroom biedt ondersteuning voor Azure AD.  De impliciete stroom wordt beschreven in de [OAuth 2.0-specificatie](http://tools.ietf.org/html/rfc6749#section-4.2).  Het belangrijkste voordeel is dat de app om op te halen van tokens van Azure AD kunnen zonder dat u een back-endserver uitvoert credential exchange.  Hiermee wordt de app aanmelden van de gebruiker, sessie en ophalen van tokens, voor andere web-API's vanuit de client alle JavaScript-code.  Er zijn enkele belangrijke beveiligingsoverwegingen rekening te houden wanneer u de impliciete stroom - specifiek ongeveer [client](http://tools.ietf.org/html/rfc6749#section-10.3) en [gebruikersimitaties](http://tools.ietf.org/html/rfc6749#section-10.3).

Als u de impliciete stroom en Azure AD authentication toevoegen aan uw app van JavaScript gebruiken wilt, raden wij aan gebruik van onze JavaScript-bibliotheek van open-source [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js).  Er zijn enkele AngularJS-zelfstudies beschikbaar [hier](active-directory-appmodel-v2-overview.md#getting-started) waarmee u kunt aan de slag.  

Als u liever niet te gebruiken van een bibliotheek in uw app met één pagina en verzend protocolberichten, volgt u de volgende algemene stappen uit.

> [!NOTE]
> Niet alle Azure Active Directory-scenario's en functies worden ondersteund door het v2.0-eindpunt.  Meer informatie over om te bepalen of moet u het v2.0-eindpunt, [v2.0 beperkingen](active-directory-v2-limitations.md).
> 
> 

## <a name="protocol-diagram"></a>Protocol-diagram
Het hele impliciete teken in stroom uitziet - elk van de stappen hieronder in detail worden beschreven.

![OpenId Connect banen](../../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

## <a name="send-the-sign-in-request"></a>Verzending van de aanvraag voor aanmelden
U kunt verzenden om te ondertekenen in eerste instantie de gebruiker in uw app, een [OpenID Connect](active-directory-v2-protocols-oidc.md) autorisatieaanvraag en get een `id_token` van het v2.0-eindpunt:

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token+token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=openid%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&response_mode=fragment
&state=12345
&nonce=678910
```

> [!TIP]
> Klik in de onderstaande koppeling voor het uitvoeren van deze aanvraag. Na het aanmelden, uw browser wordt omgeleid naar `https://localhost/myapp/` met een `id_token` in de adresbalk.
> <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token+token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=openid%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment&state=12345&nonce=678910" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/Authorize...</a>
> 
> 

| Parameter |  | Beschrijving |
| --- | --- | --- |
| Tenant |Vereist |De `{tenant}` waarde in het pad van de aanvraag kan worden gebruikt om te bepalen wie bij de toepassing aanmelden kunt.  De toegestane waarden zijn `common`, `organizations`, `consumers`, en het tenant-id's.  Zie voor meer details [protocol basisbeginselen](active-directory-v2-protocols.md#endpoints). |
| client_id |Vereist |De toepassings-Id die de portal voor wachtwoordregistratie ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) toegewezen van uw app. |
| response_type |Vereist |Moet bevatten `id_token` voor OpenID Connect aanmelden.  Het kan ook betekenen dat de response_type `token`. Met behulp van `token` hier, krijgt uw app ontvangt een toegangstoken onmiddellijk uit het geautoriseerde eindpunt zonder een tweede indienen naar het geautoriseerde eindpunt.  Als u de `token` response_type, de `scope` parameter moet een scope die aangeeft welke resource voor het uitgeven van het token voor bevatten. |
| redirect_uri |Aanbevolen |De redirect_uri van uw app, waarbij verificatie reacties kunnen worden verzonden en ontvangen door uw app.  Er moet een van de redirect_uris die u in de portal hebt geregistreerd, behalve het url-codering moet exact overeenkomen. |
| Bereik |Vereist |Een door spaties gescheiden lijst met bereiken.  Voor het OpenID Connect, het bereik moet bevatten `openid`, die wordt omgezet in de machtiging 'Aanmelden u' in de gebruikersinterface van de toestemming.  (Optioneel) u kunt ook om op te nemen de `email` of `profile` [scopes](active-directory-v2-scopes.md) voor toegang tot extra gebruikersgegevens.  U mogelijk ook andere bereiken opnemen in deze aanvraag voor het aanvragen van toestemming voor verschillende bronnen. |
| response_mode |Aanbevolen |Hiermee geeft u de methode die moet worden gebruikt voor het verzenden van het resulterende token terug naar uw app.  Moet `fragment` voor de impliciete stroom. |
| state |Aanbevolen |Een waarde die is opgenomen in de aanvraag die ook in het token antwoord worden geretourneerd.  Een tekenreeks van inhoud die u wenst dat kan zijn.  Een willekeurig gegenereerde unieke waarde wordt doorgaans gebruikt voor [voorkomen van aanvraagvervalsing op meerdere sites aanvallen](http://tools.ietf.org/html/rfc6749#section-10.12).  De status wordt ook gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat het verificatieverzoek opgetreden, zoals de pagina of de weergave op. |
| nonce |Vereist |Een waarde die is opgenomen in de aanvraag, die worden gegenereerd door de app, die wordt opgenomen in de resulterende id_token als een claim.  De app kunt vervolgens controleren of deze waarde om te beperken token replay-aanvallen.  De waarde is doorgaans een willekeurige, unieke tekenreeks die kan worden gebruikt voor het identificeren van de oorsprong van de aanvraag. |
| prompt |optioneel |Geeft het type van de interactie van de gebruiker die is vereist.  De enige geldige waarden op dit moment zijn 'aanmelding', 'none', ' toestemming geven '.  `prompt=login`wordt de gebruiker dwingen te hun referenties invoeren voor deze aanvraag, het negatief maken van eenmalige aanmelding.  `prompt=none`is het tegenovergestelde - dan gegarandeerd dat de gebruiker niet vragen beeïndigen krijgt.  Als de aanvraag kan achtergrond via eenmalige aanmelding worden voltooid, wordt het v2.0-eindpunt een fout geretourneerd.  `prompt=consent`activeert de toestemming OAuth dialoogvenster nadat de gebruiker zich aanmeldt, de gebruiker machtigen om de app wordt gevraagd. |
| login_hint |optioneel |Kan worden gebruikt voor het vervullen van het veld gebruikersnaam, e-mailadres van de aanmeldingspagina voor de gebruiker vooraf als u hun gebruikersnaam tevoren weet.  Vaak apps Gebruik deze parameter tijdens hernieuwde verificatie de gebruikersnaam die al worden opgehaald uit een eerdere aanmelden met de `preferred_username` claim. |
| domain_hint |optioneel |Een van `consumers` of `organizations`.  Als opgenomen, wordt het proces detectie op basis van e-mailbericht wordt overgeslagen die gebruiker doorloopt op de v2.0 aanmeldingspagina wordt, wat leidt tot een iets meer gestroomlijnde gebruikerservaring.  Vaak apps wordt met deze parameter gebruikt tijdens de hervalidatie door het uitpakken van de `tid` opvragen bij de id_token.  Als de `tid` claim waarde is `9188040d-6c67-4c5b-b112-36a304b66dad`, moet u `domain_hint=consumers`.  Gebruik anders `domain_hint=organizations`. |

Op dit moment wordt de gebruiker wordt gevraagd hun referenties invoeren en de authenticatie voltooien.  Het v2.0-eindpunt er ook voor zorgen dat de gebruiker heeft ingestemd met de machtigingen die zijn aangegeven in de `scope` queryparameter.  Als de gebruiker niet aan een van deze machtigingen heeft ingestemd, wordt deze vraagt de gebruiker toe te staan de vereiste machtigingen.  Details van [machtigingen, toestemming en multitenant apps vindt u hier](active-directory-v2-scopes.md).

Als de gebruiker wordt geverifieerd en toestemming verleent, het v2.0-eindpunt resulteert in een antwoord aan uw app op de aangegeven `redirect_uri`, via de methode die is opgegeven in de `response_mode` parameter.

#### <a name="successful-response"></a>Geslaagde reactie
Een geslaagde reactie met `response_mode=fragment` en `response_type=id_token+token` met regeleinden voor de leesbaarheid van de volgende lijkt:

```
GET https://localhost/myapp/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&token_type=Bearer
&expires_in=3599
&scope=https%3a%2f%2fgraph.microsoft.com%2fmail.read 
&id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
```

| Parameter | Beschrijving |
| --- | --- |
| access_token |Opgenomen als `response_type` bevat `token`. Het toegangstoken dat de app is aangevraagd, in dit geval voor Microsoft Graph.  Het toegangstoken moet niet worden gedecodeerd of anders gecontroleerd, kan deze worden behandeld als één tekenreeks. |
| token_type |Opgenomen als `response_type` bevat `token`.  Altijd `Bearer`. |
| expires_in |Opgenomen als `response_type` bevat `token`.  Geeft het aantal seconden dat het token geldig is voor cachedoeleinden. |
| Bereik |Opgenomen als `response_type` bevat `token`.  Hiermee geeft u de een of meer bereiken waarvoor de access_token geldig zijn. |
| id_token |De id_token die de app wordt aangevraagd. U kunt de id_token gebruiken om te controleren van de identiteit van de gebruiker en beginnen met een sessie met de gebruiker.  Meer informatie over id_tokens en de inhoud ervan is opgenomen in de [v2.0-eindpunt tokenverwijzing](active-directory-v2-tokens.md). |
| state |Als een parameter state is opgenomen in de aanvraag, moet dezelfde waarde weergegeven in het antwoord. De app dient te verifiëren dat de statuswaarden in de aanvraag en antwoord identiek zijn. |

#### <a name="error-response"></a>Foutbericht
Foutberichten kunnen ook worden verzonden naar de `redirect_uri` zodat de app ze op de juiste wijze kan verwerken:

```
GET https://localhost/myapp/#
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parameter | Beschrijving |
| --- | --- |
| error |Een tekenreeks van de fout code die kan worden gebruikt voor het classificeren van typen fouten die optreden en kan worden gebruikt om te reageren op fouten. |
| error_description |Een specifieke foutbericht dat een ontwikkelaar kan helpen de hoofdoorzaak van een verificatiefout identificeren. |

## <a name="validate-the-idtoken"></a>De id_token valideren
Zojuist hebt ontvangen van een id_token is niet voldoende om te verifiëren van de gebruiker. u moet de id_token handtekening valideren en controleer of de claims in het token per vereisten van uw app.  Maakt gebruik van het v2.0-eindpunt [JSON Web Tokens (JWTs)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) en cryptografie met openbare sleutels voor het ondertekenen van tokens en controleren of ze geldig zijn.

U kunt kiezen om te valideren de `id_token` in client-code, maar een gangbare optie is om te verzenden de `id_token` naar een back-endserver en er de validatie uit te voeren.  Zodra u hebt de handtekening van de id_token gevalideerd, zijn er enkele claims, u moet om te controleren.  Zie de [v2.0 tokenverwijzing](active-directory-v2-tokens.md) voor meer informatie, met inbegrip van [valideren van Tokens](active-directory-v2-tokens.md#validating-tokens) en [belangrijke informatie over het ondertekenen van sleutel Rollover](active-directory-v2-tokens.md#validating-tokens).  U wordt aangeraden die gebruikmaken van een bibliotheek geparseerd en valideren van tokens: Er is ten minste één beschikbaar voor de meeste talen en platforms.
<!--TODO: Improve the information on this-->

Mogelijk wilt u ook aanvullende verklaren afhankelijk van uw scenario.  Sommige algemene validaties zijn onder andere:

* Gezorgd/organisatie van de gebruiker is aangemeld bij de app.
* De gebruiker ervoor heeft juiste autorisatie/bevoegdheden
* Een bepaalde sterkte van verificatie gezorgd is opgetreden, zoals multi-factor authentication-server.

Zie voor meer informatie over de claims in een id_token de [v2.0-eindpunt tokenverwijzing](active-directory-v2-tokens.md).

Zodra u hebt de id_token volledig gevalideerd, kunt u beginnen met een sessie met de gebruiker en de claims in de id_token gebruiken om informatie over de gebruiker in uw app te verkrijgen.  Deze informatie kan worden gebruikt voor weergave, registreert, autorisaties, enzovoort.

## <a name="get-access-tokens"></a>Toegangstokens ophalen
Nu dat u hebt de gebruiker zich in uw app met één pagina, kunt u toegangstokens ophalen voor het aanroepen van web API's die zijn beveiligd door Azure AD, zoals de [Microsoft Graph](https://graph.microsoft.io).  Zelfs als u al hebt ontvangen een token met de `token` response_type, kunt u deze methode verkrijgen van tokens, voor aanvullende bronnen zonder dat het omleiden van de gebruiker zich opnieuw aanmeldt.

In de normale OpenID Connect/OAuth-stroom zou u dit doen door een aanvraag naar het v2.0 `/token` eindpunt.  Echter, het v2.0-eindpunt ondersteunt geen CORS-aanvragen, zodat het AJAX-aanroepen ophalen en vernieuwen van tokens buiten de vraag valt.  In plaats daarvan kunt u de impliciete stroom in een verborgen iframe nieuwe tokens krijgen voor andere web-API's: 

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment
&state=12345&nonce=678910
&prompt=none
&domain_hint=organizations
&login_hint=myuser@mycompany.com
```

> [!TIP]
> Kopiëren en plakken probeert de onderstaande aanvraag in een browsertabblad! (Vergeet niet ter vervanging van de `domain_hint` en de `login_hint` waarden met de juiste waarden voor uw gebruiker)
> 
> 

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment&state=12345&nonce=678910&prompt=none&domain_hint={{consumers-or-organizations}}&login_hint={{your-username}}
```

| Parameter |  | Beschrijving |
| --- | --- | --- |
| Tenant |Vereist |De `{tenant}` waarde in het pad van de aanvraag kan worden gebruikt om te bepalen wie bij de toepassing aanmelden kunt.  De toegestane waarden zijn `common`, `organizations`, `consumers`, en het tenant-id's.  Zie voor meer details [protocol basisbeginselen](active-directory-v2-protocols.md#endpoints). |
| client_id |Vereist |De toepassings-Id die de portal voor wachtwoordregistratie ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) toegewezen van uw app. |
| response_type |Vereist |Moet bevatten `id_token` voor OpenID Connect aanmelden.  Het kan ook betekenen dat andere response_types zoals `code`. |
| redirect_uri |Aanbevolen |De redirect_uri van uw app, waarbij verificatie reacties kunnen worden verzonden en ontvangen door uw app.  Er moet een van de redirect_uris die u in de portal hebt geregistreerd, behalve het url-codering moet exact overeenkomen. |
| Bereik |Vereist |Een door spaties gescheiden lijst met bereiken.  Voor het ophalen van tokens, omvatten alle [scopes](active-directory-v2-scopes.md) u nodig hebt voor de bron van belang. |
| response_mode |Aanbevolen |Hiermee geeft u de methode die moet worden gebruikt voor het verzenden van het resulterende token terug naar uw app.  Een van `query`, `form_post`, of `fragment`. |
| state |Aanbevolen |Een waarde die is opgenomen in de aanvraag die ook in het token antwoord worden geretourneerd.  Een tekenreeks van inhoud die u wenst dat kan zijn.  Een willekeurig gegenereerde unieke waarde wordt doorgaans gebruikt voor het voorkomen van aanvraagvervalsing op meerdere sites aanvallen.  De status wordt ook gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat het verificatieverzoek opgetreden, zoals de pagina of de weergave op. |
| nonce |Vereist |Een waarde die is opgenomen in de aanvraag, die worden gegenereerd door de app, die wordt opgenomen in de resulterende id_token als een claim.  De app kunt vervolgens controleren of deze waarde om te beperken token replay-aanvallen.  De waarde is doorgaans een willekeurige, unieke tekenreeks die kan worden gebruikt voor het identificeren van de oorsprong van de aanvraag. |
| prompt |Vereist |Voor het vernieuwen van & ophalen van tokens in een verborgen iframe, moet u `prompt=none` om ervoor te zorgen dat de iframe niet op de aanmeldingspagina v2.0 vastloopt nu en wordt onmiddellijk geretourneerd. |
| login_hint |Vereist |Voor het vernieuwen van & ophalen van tokens in een verborgen iframe, moet u de gebruikersnaam van de gebruiker opnemen in deze hint om onderscheid maken tussen meerdere sessies die deze gebruikers op een bepaald tijdstip hebben. U kunt de gebruikersnaam extraheren uit een eerdere aanmelden met de `preferred_username` claim. |
| domain_hint |Vereist |Een van `consumers` of `organizations`.  Voor het vernieuwen van & ophalen van tokens in een verborgen iframe, moet u de domain_hint opnemen in de aanvraag.  U moet pakt de `tid` opvragen bij de id_token van een vorige aanmelden om te bepalen welke waarde om te gebruiken.  Als de `tid` claim waarde is `9188040d-6c67-4c5b-b112-36a304b66dad`, moet u `domain_hint=consumers`.  Gebruik anders `domain_hint=organizations`. |

Dank aan de `prompt=none` parameter, deze aanvraag ofwel slagen of mislukken onmiddellijk en terugkeren naar de toepassing.  Een geslaagde reactie wordt verzonden naar uw app op de aangegeven `redirect_uri`, via de methode die is opgegeven in de `response_mode` parameter.

#### <a name="successful-response"></a>Geslaagde reactie
Een geslaagde reactie met `response_mode=fragment` lijkt:

```
GET https://localhost/myapp/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
&token_type=Bearer
&expires_in=3599
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read
```

| Parameter | Beschrijving |
| --- | --- |
| access_token |Het token dat de app wordt aangevraagd. |
| token_type |Altijd `Bearer`. |
| state |Als een parameter state is opgenomen in de aanvraag, moet dezelfde waarde weergegeven in het antwoord. De app dient te verifiëren dat de statuswaarden in de aanvraag en antwoord identiek zijn. |
| expires_in |Hoe lang het toegangstoken is ongeldig (in seconden). |
| Bereik |De bereiken die het toegangstoken is geldig voor. |

#### <a name="error-response"></a>Foutbericht
Foutberichten kunnen ook worden verzonden naar de `redirect_uri` zodat de app ze op de juiste wijze kan verwerken.  In het geval van `prompt=none`, een verwachte fout is:

```
GET https://localhost/myapp/#
error=user_authentication_required
&error_description=the+request+could+not+be+completed+silently
```

| Parameter | Beschrijving |
| --- | --- |
| error |Een tekenreeks van de fout code die kan worden gebruikt voor het classificeren van typen fouten die optreden en kan worden gebruikt om te reageren op fouten. |
| error_description |Een specifieke foutbericht dat een ontwikkelaar kan helpen de hoofdoorzaak van een verificatiefout identificeren. |

Als u deze fout in de aanvraag iframe ontvangt, moet de gebruiker interactief aanmelden opnieuw voor het ophalen van een nieuw token.  U kunt kiezen om deze situatie in een manier die het zinvol is voor uw toepassing te verwerken.

## <a name="refreshing-tokens"></a>Vernieuwen van tokens
Beide `id_token`s en `access_token`s verloopt na een korte periode, zodat uw app moet worden voorbereid om te vernieuwen deze tokens regelmatig.  Voor beide typen token vernieuwt, kunt u uitvoeren dezelfde verborgen iframe aanvraag boven het gebruik van de `prompt=none` parameter Azure AD-gedrag te bepalen.  Als u wilt ontvangen van een nieuwe `id_token`, zorg ervoor dat u `response_type=id_token` en `scope=openid`, evenals een `nonce` parameter.

## <a name="send-a-sign-out-request"></a>Een afmelden aanvraag verzenden
De OpenIdConnect `end_session_endpoint` , kan uw app een aanvraag te verzenden naar het v2.0-eindpunt een gebruikerssessie beëindigen en cookies die zijn ingesteld door het v2.0-eindpunt te wissen.  Volledig ondertekenen van een gebruiker uit een webtoepassing, moet uw app een eigen sessie met de gebruiker beëindigen (meestal met een token cache wissen of verwijderen van cookies) en leid de browser:

```
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/logout?post_logout_redirect_uri=https://localhost/myapp/
```

| Parameter |  | Beschrijving |
| --- | --- | --- |
| Tenant |Vereist |De `{tenant}` waarde in het pad van de aanvraag kan worden gebruikt om te bepalen wie bij de toepassing aanmelden kunt.  De toegestane waarden zijn `common`, `organizations`, `consumers`, en het tenant-id's.  Zie voor meer details [protocol basisbeginselen](active-directory-v2-protocols.md#endpoints). |
| post_logout_redirect_uri | Aanbevolen | De URL waarnaar de gebruiker moet worden geretourneerd aan nadat de afmelding is voltooid. Deze waarde moet overeenkomen met een van de omleiding die URI 's geregistreerd voor de toepassing. Als niet is opgenomen, kan de gebruiker wordt een algemeen bericht door het v2.0-eindpunt worden weergegeven. |
