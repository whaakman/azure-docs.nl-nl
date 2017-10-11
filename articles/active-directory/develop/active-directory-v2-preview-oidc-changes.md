---
title: Wijzigingen in de Azure AD v2.0-eindpunt | Microsoft Docs
description: Een beschrijving van wijzigingen die worden uitgevoerd in de app model v2.0 openbare preview-protocollen.
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: e6c5b891-0b5d-47dc-8184-5d0ef6a1a006
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/16/2016
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: ae73833a68db14804dc40eaf07ff7d3effaa9052
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="important-updates-to-the-v20-authentication-protocols"></a>Belangrijke Updates voor de verificatieprotocollen v2.0
Ontwikkelaars aandacht! De volgende twee weken brengen we enkele updates voor onze v2.0-verificatieprotocollen die u kunnen betekenen dat de wijzigingen voor alle apps die u hebt geschreven in onze preview-periode op te splitsen.  

## <a name="who-does-this-affect"></a>Wie heeft dit invloed op?
Alle apps die zijn geschreven voor het gebruik van het v2.0 geconvergeerde eindpunt voor authenticatie

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
```

Meer informatie over het v2.0-eindpunt vindt [hier](active-directory-appmodel-v2-overview.md).

Als u een app met het v2.0-eindpunt met de codering rechtstreeks aan het protocol v2.0 hebt gebouwd, moet met behulp van een van onze middlewares OpenID Connect of OAuth web of met andere 3e partij bibliotheken voor het uitvoeren van verificatie, u worden voorbereid voor het testen van uw projecten en wijzigingen aanbrengen indien nodig.

## <a name="who-doesnt-this-affect"></a>Wie heeft niet dit invloed op?
Alle apps die zijn geschreven voor het eindpunt productie Azure AD-verificatie

```
https://login.microsoftonline.com/common/oauth2/authorize
```

Dit protocol is ingesteld in steen en worden geen wijzigingen in de problemen.

Bovendien, als uw app **alleen** onze ADAL-bibliotheek gebruikt voor het uitvoeren van verificatie, u hebt geen wijzigen.  ADAL heeft uw app uit de wijzigingen worden afgeschermd.  

## <a name="what-are-the-changes"></a>Wat zijn de wijzigingen?
### <a name="removing-the-x5t-value-from-jwt-headers"></a>De waarde x5t verwijderen van JWT-headers
Het v2.0-eindpunt maakt gebruik van JWT-tokens uitgebreid, die een gedeelte van de parameters header met relevante metagegevens over het token bevatten.  Als u de koptekst van een van onze huidige JWTs decoderen, zou u ongeveer vinden:

```
{ 
    "type": "JWT",
    "alg": "RS256",
    "x5t": "MnC_VZcATfM5pOYiJHMba9goEKY",
    "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

Eigenschappen van de 'x5t' en 'kid' identificeren waar de openbare sleutel die moet worden gebruikt voor het valideren van de handtekening van het token, opgehaald uit het eindpunt van de metagegevens OpenID Connect.

De wijziging we hier maken is het verwijderen van de eigenschap 'x5t'.  U kunt doorgaan met het gebruik van dezelfde mechanismen om tokens te valideren, maar verstandig alleen op de eigenschap 'kid' voor het ophalen van de juiste openbare sleutel, zoals opgegeven in het OpenID Connect-protocol. 

> [!IMPORTANT]
> **Uw job: Zorg ervoor dat uw app is niet afhankelijk van de aanwezigheid van de x5t-waarde.**
> 
> 

### <a name="removing-profileinfo"></a>Profile_info verwijderen
Voorheen het v2.0-eindpunt is is een base64-gecodeerd JSON-object in retourneren token antwoorden aangeroepen `profile_info`.  Bij het aanvragen van een toegangstoken van het v2.0-eindpunt met een verzoek om te verzenden:

```
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

Het antwoord eruit als de volgende JSON-object:

```
{ 
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https://outlook.office.com/mail.read",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "profile_info": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

De `profile_info` waarde bevat informatie over de gebruiker die heeft aangemeld bij de app - hun weergavenaam, de voornaam, achternaam, e-mailadres, id, enzovoort.  Voornamelijk om de `profile_info` is gebruikt voor het opslaan van token en op het scherm.

Nu worden verwijderd de `profile_info` waarde – maar maak je geen zorgen, bieden we nog steeds deze informatie voor ontwikkelaars op een iets andere plaats.  In plaats van `profile_info`, het v2.0-eindpunt resulteert nu een `id_token` in elk token antwoord:

```
{ 
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https://outlook.office.com/mail.read",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

U kunt decoderen en parseren van de id_token om op te halen van de dezelfde informatie die u hebt ontvangen van profile_info.  De id_token is een JSON Web Token (JWT), met inhoud zoals opgegeven door het OpenID Connect.  De code hiervoor dus moet vergelijkbaar: u hoeft alleen het middelste segment (de tekst) van de id_token uitpakken en decoderen van base64 deze toegang krijgt tot het JSON-object.

De volgende twee weken moet u de code uw app om de gebruikersgegevens van de opgehaald uit de `id_token` of `profile_info`; afhankelijk van wat aanwezig is.  Op die manier wanneer de wijziging wordt aangebracht uw app kan de overgang van naadloos verwerken `profile_info` naar `id_token` zonder onderbreking.

> [!IMPORTANT]
> **Uw job: Zorg ervoor dat uw app is niet afhankelijk van de aanwezigheid van de `profile_info` waarde.**
> 
> 

### <a name="removing-idtokenexpiresin"></a>Id_token_expires_in verwijderen
Net als bij `profile_info`, worden ook verwijderd de `id_token_expires_in` parameter van antwoorden.  Voorheen het v2.0-eindpunt zou waarde retourneren voor `id_token_expires_in` samen met elke id_token antwoord, bijvoorbeeld in een antwoord autoriseren:

```
https://myapp.com?id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...&id_token_expires_in=3599...
```

Of in een token antwoord:

```
{ 
    "token_type": "Bearer",
    "id_token_expires_in": 3599,
    "scope": "openid",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "profile_info": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

De `id_token_expires_in` waarde duidt dit op het aantal seconden dat de id_token blijft geldig voor.  Nu we verwijdert de `id_token_expires_in` volledig waarde.  In plaats daarvan kunt u de standaard OpenID Connect `nbf` en `exp` controleren de geldigheid van een id_token-claims.  Zie de [v2.0 tokenverwijzing](active-directory-v2-tokens.md) voor meer informatie over deze claims.

> [!IMPORTANT]
> **Uw job: Zorg ervoor dat uw app is niet afhankelijk van de aanwezigheid van de `id_token_expires_in` waarde.**
> 
> 

### <a name="changing-the-claims-returned-by-scopeopenid"></a>Het wijzigen van de claims geretourneerd door een bereik openid =
Deze wijziging wordt de belangrijkste – in feite, heeft dit gevolgen voor vrijwel elke app die gebruikmaakt van het v2.0-eindpunt.  Veel toepassingen aanvragen verzenden naar het v2.0-eindpunt met behulp de `openid` bereik, zoals:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid offline_access https://outlook.office.com/mail.read
```

Vandaag, wanneer de gebruiker toestemming voor verleent de `openid` bereik, uw app een schat aan informatie over de gebruiker in de resulterende id_token ontvangt.  Deze claims kunnen opnemen hun naam, voorkeur gebruikersnaam, e-mailadres, object-ID en meer.

In deze update wordt de informatie wijzigt die de `openid` bereik kan uw apptoegang tot, voor betere comform met de specificatie van het OpenID Connect.  De `openid` bereik wordt alleen toe zodat uw app voor het ondertekenen van de gebruiker in en ontvangen van een app-specifiek-id voor de gebruiker in de `sub` claimen van de id_token.  De claims in een id_token met alleen de `openid` bereik verleend worden persoonlijke gegevens.  Voorbeeld id_token claims zijn:

```
{ 
    "aud": "580e250c-8f26-49d0-bee8-1c078add1609",
    "iss": "https://login.microsoftonline.com/b9410318-09af-49c2-b0c3-653adc1f376e/v2.0 ",
    "iat": 1449520283,
    "nbf": 1449520283,
    "exp": 1449524183,
    "nonce": "12345",
    "sub": "MF4f-ggWMEji12KynJUNQZphaUTvLcQug5jdF2nl01Q",
    "tid": "b9410318-09af-49c2-b0c3-653adc1f376e",
    "ver": "2.0",
}
```

Als u ophalen van persoonsgegevens (PII) over de gebruiker in uw app wilt, moet uw app om aanvullende machtigingen van de gebruiker.  Ondersteuning voor twee nieuwe scopes worden geïntroduceerd van het OpenID Connect-specificatie – de `email` en `profile` scopes – waarmee u kunt doen.

* De `email` bereik is zeer eenvoudig: uw app-toegang tot het primaire e-mailadres van de gebruiker via de `email` claim in de id_token.  Houd er rekening mee dat de `email` claim altijd worden niet aanwezig in id_tokens – het alleen worden opgenomen als beschikbaar in het gebruikersprofiel.
* De `profile` bereik kan uw apptoegang tot alle andere basisinformatie over de gebruiker – de naam, de voorkeur username, object-ID, enzovoort.

Hiermee kunt u uw app op een wijze minimale openbaarmaking code – vraagt u de gebruiker voor de set gegevens die uw app vereist zijn ingesteld met de taak uit te voeren.  Als u doorgaan met het ophalen van de volledige set van gebruikersgegevens die uw app ontvangt momenteel wilt, moet u alle drie bereiken opnemen in uw autorisatieaanvragen:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid profile email offline_access https://outlook.office.com/mail.read
```

Uw app kunt beginnen met het verzenden van de `email` en `profile` onmiddellijk bereiken en het v2.0-eindpunt accepteert deze twee scopes en beginnen met het aanvragen van de machtigingen van gebruikers indien nodig.  Echter, de wijziging in de interpretatie van de `openid` bereik wordt pas van kracht een paar weken.

> [!IMPORTANT]
> **Uw job: Voeg de `profile` en `email` scopes als uw app informatie over de gebruiker vereist.**  Houd er rekening mee dat ADAL beide machtigingen in aanvragen standaard bevatten zal. 
> 
> 

### <a name="removing-the-issuer-trailing-slash"></a>Verwijderen van de verlener afsluitende slash.
Voorheen werd door duurde de waarde van de certificaatverlener die wordt weergegeven in de tokens van het v2.0-eindpunt het formulier

```
https://login.microsoftonline.com/{some-guid}/v2.0/
```

De guid is waar de tenantId van de Azure AD-tenant die het token heeft afgegeven.  Met deze wijzigingen, wordt de waarde van de verlener pas

```
https://login.microsoftonline.com/{some-guid}/v2.0 
```

in beide tokens en in het OpenID Connect discovery-document.

> [!IMPORTANT]
> **Uw job: Zorg ervoor dat uw app in de waarde van de verlener zowel met als zonder een afsluitende slash accepteert tijdens de validatie van de verlener.**
> 
> 

## <a name="why-change"></a>Waarom wijzigen?
Er is de primaire reden voor de introductie van deze wijzigingen te voldoen aan de standaard OpenID Connect-specificatie.  We hopen verschillen tussen integreren met Microsoft identity services en andere services identiteit in de branche te beperken door OpenID Connect compatibel, dat.  We willen kunnen ontwikkelaars hun favoriete open-source-verificatiebibliotheken gebruiken zonder dat voor het wijzigen van de bibliotheken om Microsoft verschillen te ondersteunen.

## <a name="what-can-you-do"></a>Wat kunt u doen?
Vanaf vandaag, kunt u beginnen met het maken van alle wijzigingen die hierboven worden beschreven.  U moet onmiddellijk het volgende doen:

1. **Verwijder eventuele afhankelijkheden op de `x5t` headerparameter.**
2. **De overgang van de juiste manier verwerken `profile_info` naar `id_token` in antwoorden van een token.**
3. **Verwijder eventuele afhankelijkheden op de `id_token_expires_in` antwoord-parameter.**
4. **Voeg de `profile` en `email` scopes aan uw app als uw app basisgebruiker informatie nodig heeft.**
5. **Waarden van de verlener in tokens zowel met als zonder een afsluitende slash accepteren.**

Onze [v2.0 protocoldocumentatie](active-directory-v2-protocols.md) al is bijgewerkt met deze wijzigingen, zodat u deze als referentie gebruiken kunt bij het bijwerken van uw code.

Als u meer vragen over het bereik van de wijzigingen hebt, aarzel niet om te bereiken ons op Twitter op @AzureAD.

## <a name="how-often-will-protocol-changes-occur"></a>Hoe vaak wijzigingen protocol plaatsvindt?
We voorziet niet alle verder op te splitsen wijzigingen in de verificatieprotocollen.  We bundelt deze wijzigingen in een versie die u hebt geen te gaan via dit soort updateproces snel opnieuw elk gewenst moment opzettelijk.  Natuurlijk, zullen wij functies toevoegen aan de geconvergeerde v2.0 authentication-service die u van profiteren kunt, maar deze wijzigingen moet additief zijn en niet-bestaande code einde.

Tot slot willen we spreken Hartelijk dank voor dingen uitprobeert tijdens de preview-periode.  Inzicht en ervaringen van onze vroege gebruikers zijn waardevol zijn tot nu toe, en we hopen dat u hebt uw mening en ideeën delen.

Gelukkig coderen!

De deling van de Microsoft-identiteitsbeheer

