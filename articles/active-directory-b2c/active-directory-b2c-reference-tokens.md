---
title: Token referentie - Azure AD B2C | Microsoft Docs
description: De typen tokens die zijn uitgegeven in Azure Active Directory B2C
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: mtillman
editor: parakhj
ms.assetid: 6df79878-65cb-4dfc-98bb-2b328055bc2e
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2017
ms.author: parakhj
ms.openlocfilehash: ce82fcc82cf411d1596fea56ff368d96eceeff38
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-ad-b2c-token-reference"></a>Azure AD B2C: Token verwijzing

Azure Active Directory B2C (Azure AD B2C) verzendt verschillende typen beveiligingstokens tijdens de verwerking van elk [authenticatiestroom](active-directory-b2c-apps.md). Dit document beschrijft de indeling, de beveiligingskenmerken en de inhoud van elk type token.

## <a name="types-of-tokens"></a>Typen tokens
Azure AD B2C ondersteunt de [OAuth 2.0-protocol voor autorisatie](active-directory-b2c-reference-protocols.md), welke maakt gebruik van beide toegang tot tokens en vernieuwen van tokens. Het biedt ook ondersteuning voor verificatie en meld u via [OpenID Connect](active-directory-b2c-reference-protocols.md), die een derde type token introduceert: het token ID. Elk van deze tokens wordt weergegeven als een bearer-token.

Een bearer-token is een lichtgewicht beveiligingstoken die de 'bearer' toegang tot een beveiligde bron verleent. De houder is een partij die het token kan opleveren. Azure AD B2C partij moet eerst worden geverifieerd voordat er een bearer-token kan ontvangen. Maar als de vereiste stappen niet genomen worden voor het beveiligen van het token in overdracht en opslag, kan deze kan worden onderschept en gebruikt door een onbedoelde partij. Sommige beveiligingstokens hebben een ingebouwde mechanisme om te voorkomen dat onbevoegden kunnen gebruiken, maar bearer-tokens hebben geen dit mechanisme. Zij moeten worden overgebracht in een beveiligd kanaal, zoals transport layer security (HTTPS).

Als een bearer-token buiten een beveiligd kanaal wordt verzonden, kunnen een kwaadwillende party een man-in-the-middle-aanval kunt gebruiken om het token verkrijgen en gebruiken om onbevoegde toegang te krijgen tot een beveiligde bron. De dezelfde beveiligings-principals van toepassing wanneer bearer-tokens worden opgeslagen of voor later gebruik opgeslagen. Altijd voor zorgen dat uw app verzendt en bearer-tokens worden opgeslagen op een veilige manier.

Zie voor extra beveiligingsoverwegingen op bearer-tokens [RFC 6750 sectie 5](http://tools.ietf.org/html/rfc6750).

Veel van de tokens die Azure AD B2C uitgeeft worden geïmplementeerd als JSON webtokens (JWTs). Een JWT is een compacte, URL-veilige methode voor het overbrengen van gegevens tussen twee partijen. JWTs bevatten informatie die wel claims. Dit zijn asserties van informatie over de bearer en het onderwerp van het token. De claims in JWTs zijn JSON-objecten die zijn gecodeerd en geserialiseerd voor verzending. Omdat de JWTs uitgegeven door Azure AD B2C zijn ondertekend, maar niet versleuteld, kunt u eenvoudig de inhoud van een JWT voor foutopsporing van het inspecteren. Er zijn verschillende hulpprogramma's beschikbaar die hiervoor, met inbegrip van [jwt.ms](https://jwt.ms). Raadpleeg voor meer informatie over JWTs [JWT-specificaties](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

### <a name="id-tokens"></a>ID-tokens

Een token ID is een vorm van het beveiligingstoken dat uw app van de Azure AD B2C ontvangt `/authorize` en `/token` eindpunten. ID-tokens worden weergegeven als [JWTs](#types-of-tokens), en bevatten claims die u gebruiken kunt om gebruikers in uw app te identificeren. Wanneer de ID-tokens zijn verkregen van de `/authorize` eindpunt, ze klaar bent met behulp van de [impliciete stroom](active-directory-b2c-reference-spa.md), die vaak wordt gebruikt voor gebruikers die zich in voor javascript op basis van webtoepassingen. Wanneer de ID-tokens zijn verkregen van de `/token` eindpunt, ze klaar bent met behulp van de [code vertrouwelijk stroom](active-directory-b2c-reference-oidc.md), die het token dat is verborgen in de browser houdt. Hierdoor kan het token veilig in HTTP-aanvragen voor de communicatie tussen de twee onderdelen van de dezelfde toepassing of service worden verzonden. Zoals u dat wilt, kunt u de claims in een token ID. Ze worden vaak gebruikt om gegevens van het account weer te geven of om access control beslissingen te nemen in een app.  

ID-tokens zijn ondertekend, maar ze niet op dit moment zijn gecodeerd. Wanneer uw app of API ontvangt een token ID, moet deze [valideert de handtekening](#token-validation) om te bewijzen dat het token authentiek is. Uw app of API moet ook valideren voor een paar claims in het token om te bewijzen dat deze geldig is. Afhankelijk van de scenariovereisten de claims gevalideerd door een app kunnen variëren, maar uw app moet uitvoeren sommige [algemene claim validaties](#token-validation) in elk scenario.

#### <a name="sample-id-token"></a>Voorbeeldtoken-ID
```
// Line breaks for display purposes only

eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6IklkVG9rZW5TaWduaW5nS2V5Q29udGFpbmVyIn0.
eyJleHAiOjE0NDIzNjAwMzQsIm5iZiI6MTQ0MjM1NjQzNCwidmVyIjoiMS4wIiwiaXNzIjoiaHR0cHM6Ly9s
b2dpbi5taWNyb3NvZnRvbmxpbmUuY29tLzc3NTUyN2ZmLTlhMzctNDMwNy04YjNkLWNjMzExZjU4ZDkyNS92
Mi4wLyIsImFjciI6ImIyY18xX3NpZ25faW5fc3RvY2siLCJzdWIiOiJOb3Qgc3VwcG9ydGVkIGN1cnJlbnRs
eS4gVXNlIG9pZCBjbGFpbS4iLCJhdWQiOiI5MGMwZmU2My1iY2YyLTQ0ZDUtOGZiNy1iOGJiYzBiMjlkYzYi
LCJpYXQiOjE0NDIzNTY0MzQsImF1dGhfdGltZSI6MTQ0MjM1NjQzNCwiaWRwIjoiZmFjZWJvb2suY29tIn0.
h-uiKcrT882pSKUtWCpj-_3b3vPs3bOWsESAhPMrL-iIIacKc6_uZrWxaWvIYkLra5czBcGKWrYwrAC8ZvQe
DJWZ50WXQrZYODEW1OUwzaD_I1f1HE0c2uvaWdGXBpDEVdsD3ExKaFlKGjFR2V7F-fPThkVDdKmkUDQX3bVc
yyj2V2nlCQ9jd7aGnokTPfLfpOjuIrTsAdPcGpe5hfSEuwYDmqOJjGs9Jp1f-eSNEiCDQOaTBSvr479L5ptP
XWeQZyX2SypN05Rjr05bjZh3j70ZUimiocfJzjibeoDCaQTz907yAg91WYuFOrQxb-5BaUoR7K-O7vxr2M-_
CQhoFA

```

### <a name="access-tokens"></a>Toegangstokens

Een toegangstoken is ook een vorm van een beveiligingstoken dat uw app van de Azure AD B2C ontvangt `/authorize` en `/token` eindpunten. Toegangstokens worden ook weergegeven als [JWTs](#types-of-tokens), en bevatten claims die u kunt de machtigingen hebben om uw API's te identificeren. Toegangstokens zijn ondertekend, maar ze niet op dit moment zijn gecodeerd. Toegangstokens moeten worden gebruikt voor toegang tot API's en resource-servers. Meer informatie over het [toegangstokens gebruiken](active-directory-b2c-access-tokens.md). 

Wanneer uw API een toegangstoken ontvangt, moet deze [valideert de handtekening](#token-validation) om te bewijzen dat het token authentiek is. Uw API moet ook valideren voor een paar claims in het token om te bewijzen dat deze geldig is. Afhankelijk van de scenariovereisten de claims gevalideerd door een app kunnen variëren, maar uw app moet uitvoeren sommige [algemene claim validaties](#token-validation) in elk scenario.

### <a name="claims-in-id-and-access-tokens"></a>Claims in tokens-ID en -toegang

Wanneer u Azure AD B2C gebruikt, hebt u fijnmazig controle over de inhoud van uw tokens. U kunt configureren [beleid](active-directory-b2c-reference-policies.md) te verzenden van bepaalde verzamelingen gebruikersgegevens in claims die uw app is vereist voor bewerkingen. Deze claims standaardeigenschappen zoals van de gebruiker kunnen opnemen `displayName` en `emailAddress`. Ze kunnen ook [aangepaste gebruikerskenmerken](active-directory-b2c-reference-custom-attr.md) die u kunt definiëren in uw B2C-directory. Elke-ID en -toegang token die u ontvangt een bepaalde set claims die betrekking hebben op beveiliging bevat. Uw toepassingen kunnen gebruikmaken van deze claims voor het veilig verifiëren van gebruikers en aanvragen.

Houd er rekening mee dat de claims in de ID-tokens niet in een bepaalde volgorde worden geretourneerd. Bovendien kunnen u nieuwe claims geïntroduceerd in de ID-tokens op elk gewenst moment. Uw app beter niet verbreken wanneer nieuwe claims binnenkomen. Hier volgen de claims die u verwacht te aanwezig zijn in-ID en toegang tokens die zijn uitgegeven door Azure AD B2C. Aanvullende claims worden bepaald door het beleid. Probeer de claims in het voorbeeld-ID-token te bekijken door te plakken in verdient het aanbeveling [jwt.ms](https://jwt.ms). Meer informatie vindt u in de [specificatie van het OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html).

| Naam | Claim | Voorbeeldwaarde | Beschrijving |
| --- | --- | --- | --- |
| Doelgroep |`aud` |`90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6` |Een claim doelgroep identificeert de beoogde ontvanger van het token. Voor Azure AD B2C is de doelgroep van uw app-toepassings-ID die is toegewezen aan uw app in de app-portal voor wachtwoordregistratie. Uw app moet deze waarde niet valideren en weigeren van het token als komt niet overeen met. |
| Certificaatverlener |`iss` |`https://login.microsoftonline.com/775527ff-9a37-4307-8b3d-cc311f58d925/v2.0/` |Deze claim identificeert de beveiligingstokenservice (STS) die wordt gemaakt en retourneert het token. Ook worden de Azure AD-directory waarin de gebruiker werd geverifieerd. Uw app moet de claim verlener om ervoor te zorgen dat het token afkomstig zijn van het Azure Active Directory v2.0-eindpunt te valideren. |
| verleend aan |`iat` |`1438535543` |Deze claim is de tijd waarop het token is uitgegeven, epoche tijd. |
| verlooptijd |`exp` |`1438539443` |Verlooptijd van de claim is de tijd waarop het token ongeldig is, weergegeven in epoche tijd. Uw app moet deze claim gebruiken om de geldigheid van de levensduur van tokens. |
| niet voor |`nbf` |`1438535543` |Deze claim is het tijdstip waarop het token geldige, dat wordt vertegenwoordigd in epoche tijd wordt. Dit is meestal hetzelfde zijn als de tijd die het token is uitgegeven. Uw app moet deze claim gebruiken om de geldigheid van de levensduur van tokens. |
| Versie |`ver` |`1.0` |Dit is de versie van het token ID, zoals gedefinieerd door Azure AD. |
| code-hash |`c_hash` |`SGCPtt01wxwfgnYZy2VJtQ` |Een code-hash is opgenomen in een token ID alleen wanneer het token samen met een OAuth 2.0-autorisatiecode wordt uitgegeven. Een hash van de code kan worden gebruikt om de echtheid van een autorisatiecode te valideren. Zie voor meer informatie over het uitvoeren van deze validatie de [specificatie van het OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html).  |
| token hash toegang |`at_hash` |`SGCPtt01wxwfgnYZy2VJtQ` |Een token hash van de toegang is opgenomen in een token ID alleen wanneer het token is uitgegeven samen met een OAuth 2.0-toegangstoken. Een token hash toegang kan worden gebruikt om de echtheid van een toegangstoken te valideren. Zie voor meer informatie over het uitvoeren van deze validatie de [specificatie van het OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html)  |
| nonce |`nonce` |`12345` |Een nonce is een strategie gebruikt voor het beperken van token replay-aanvallen. Uw app een nonce kunt opgeven in een aanvraag voor verificatie met behulp van de `nonce` queryparameter. De waarde die u in de aanvraag opgeeft zal worden verzonden zijn de `nonce` van een token ID alleen claim. Hierdoor kan uw app om te controleren of de waarde met de waarde die het opgegeven op de aanvraag die sessie van de app aan een opgegeven ID-token koppelt. Uw app moet deze validatie tijdens de validatie van token ID uitvoeren. |
| Onderwerp |`sub` |`884408e1-2918-4cz0-b12d-3aa027d7563b` |Dit is de principal waarover het token asserts informatie, zoals de gebruiker van een app. Deze waarde is onveranderbaar en kan niet worden toegewezen of opnieuw gebruikt. Het kan controles uit te voeren autorisatie veilig, zoals wanneer het token wordt gebruikt voor toegang tot een bron worden gebruikt. Standaard wordt de claim onderwerp ingevuld met de object-ID van de gebruiker in de map. Zie voor meer informatie, [Azure Active Directory B2C: Token, sessie en configuratie voor één aanmelding](active-directory-b2c-token-session-sso.md). |
| Authentication context class reference |`acr` |Niet van toepassing |Momenteel niet gebruikt, behalve in het geval van een oudere beleidsregels. Zie voor meer informatie, [Azure Active Directory B2C: Token, sessie en configuratie voor één aanmelding](active-directory-b2c-token-session-sso.md). |
| Framework vertrouwensbeleid |`tfp` |`b2c_1_sign_in` |Dit is de naam van het beleid dat is gebruikt voor het verkrijgen van het token ID. |
| Verificatie-tijd |`auth_time` |`1438535543` |Deze claim is het tijdstip waarop een laatste opgegeven gebruikersreferenties, epoche tijd. |

### <a name="refresh-tokens"></a>Vernieuwen van tokens
Vernieuwen van tokens zijn beveiligingstokens die uw app gebruiken kunt voor de nieuwe ID-tokens verkrijgen en toegang tot tokens in een OAuth 2.0-stroom. Ze uw app op lange termijn toegang bieden tot bronnen namens gebruikers zonder tussenkomst met deze gebruikers.

Voor het ontvangen van een vernieuwing token een token antwoord wordt uw app moet aanvragen de `offline_acesss` bereik. Voor meer informatie over de `offline_access` bereik, raadpleeg dan de [naslaginformatie over Azure AD B2C-protocol](active-directory-b2c-reference-protocols.md).

Vernieuwen van tokens, en altijd zullen zijn, mat aan uw app. Ze kunnen zijn uitgegeven door Azure AD en worden gecontroleerd en geïnterpreteerd alleen door Azure AD. Ze zijn lange levensduur hebben, maar uw app niet worden geschreven met de verwachting die een vernieuwingstoken voor een bepaalde periode duurt. Vernieuwen van tokens kunnen op elk moment om een groot aantal redenen zijn ongeldig. De enige manier om uw app weten of een vernieuwingstoken geldig is, is als u wilt gebruikmaken van het door het maken van een tokenaanvraag naar Azure AD.

Wanneer u een vernieuwingstoken voor een nieuw token inwisselen (en als uw app heeft gekregen de `offline_access` bereik), ontvangt u een nieuw vernieuwingstoken in het token antwoord. Het van nieuwe uitgegeven vernieuwingstoken op te slaan. Het moet vernieuwingstoken dat u eerder hebt gebruikt in de aanvraag vervangen. Dit helpt te garanderen dat uw vernieuwen van tokens geldig voor zo lang mogelijk blijven.

## <a name="token-validation"></a>Validatie van tokens
Voor het valideren van een token moet het controleren van uw app in de handtekening en de claims van het token.

Veel open-source bibliotheken zijn beschikbaar voor het valideren van JWTs, afhankelijk van uw voorkeurstaal. U wordt aangeraden dat u deze opties verkennen in plaats van uw eigen validatielogica implementeren. De informatie in deze handleiding kunt u informatie over het correct gebruik die bibliotheken.

### <a name="validate-the-signature"></a>Valideert de handtekening
Een JWT bevat drie segmenten, gescheiden door het `.` teken. Het eerste segment is het *header*, de tweede is het *hoofdtekst*, en het derde is de *handtekening*. Het segment handtekening kan worden gebruikt voor het valideren van de echtheid van het token, zodat deze kan worden vertrouwd door uw app.

Azure AD B2C-tokens zijn ondertekend met behulp van industriestandaard asymmetrische algoritmen, zoals RSA 256. De header van het token bevat informatie over de sleutel en versleuteling methode gebruikt voor het ondertekenen van het token:

```
{
        "typ": "JWT",
        "alg": "RS256",
        "kid": "GvnPApfWMdLRi8PDmisFn7bprKg"
}
```

De `alg` claim geeft aan dat de algoritme die is gebruikt voor het ondertekenen van het token. De `kid` claim geeft aan dat de bepaalde openbare sleutel die is gebruikt voor het ondertekenen van het token.

Azure AD kunt een token ondertekenen met behulp van een van een bepaalde set paren van openbare en persoonlijke sleutels op een bepaald moment. Azure AD draait regelmatig de mogelijke reeks sleutels zodat uw app automatisch afhandelen van de belangrijkste wijzigingen worden geschreven. Een redelijke frequentie om te controleren op updates voor de openbare sleutels die worden gebruikt door Azure AD wordt elke 24 uur.

Azure AD B2C heeft een eindpunt met OpenID Connect metagegevens. Hiermee kunt apps voor het ophalen van informatie over Azure AD B2C tijdens runtime. Deze informatie omvat eindpunten, inhoud van tokens en token-ondertekening sleutels. Uw B2C-directory bevat een JSON-metagegevens-document voor elk beleid. Het metagegevensdocument voor de `b2c_1_sign_in` beleid in `fabrikamb2c.onmicrosoft.com` bevindt zich op:

```
https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in
```

`fabrikamb2c.onmicrosoft.com`is de B2C-directory gebruikt voor het verifiëren van de gebruiker en `b2c_1_sign_in` is het beleid voor het verkrijgen van het token. Om te bepalen welk beleid is gebruikt voor het ondertekenen van een token (en verder op te halen de metagegevens), hebt u twee opties. Eerst de naam van het beleid is opgenomen in de `acr` claim in het token. U kunt claims buiten de hoofdtekst van het JWT parseren met base 64-decoderen van de hoofdtekst en bij het deserialiseren van de JSON-tekenreeks die het resultaat. De `acr` claim is de naam van het beleid dat is gebruikt voor het uitgeven van het token.  De andere optie is voor het coderen van het beleid in de waarde van de `state` parameter wanneer u de aanvraag en vervolgens worden ontsleuteld om te bepalen welk beleid wordt gebruikt. Beide methoden is geldig.

Het metagegevensdocument is een JSON-object dat nuttig stukjes informatie bevat. Het gaat hierbij om de locatie van de eindpunten vereist OpenID Connect validatie uit te voeren. Ook `jwks_uri`, waardoor de locatie van de set van openbare sleutels die worden gebruikt voor het ondertekenen van tokens. Dat locatie hier is opgegeven, maar het is raadzaam de locatie dynamisch ophalen met behulp van het metagegevensdocument en parseren `jwks_uri`:

```
https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in
```

Het JSON-document dat zich bevindt op deze URL bevat alle openbare gegevens van de sleutel op een bepaald moment in gebruik. Uw app kunt gebruiken de `kid` claim in de header JWT de openbare sleutel in het JSON-document dat wordt gebruikt voor het ondertekenen van een bepaalde token selecteren. Validatie van handtekening kan vervolgens uitvoeren met behulp van de juiste openbare sleutel en het algoritme aangegeven.

Een beschrijving van het uitvoeren van de handtekeningvalidatie is buiten het bereik van dit document. Veel open-source bibliotheken zijn beschikbaar voor hulp bij dit als u deze nodig hebt.

### <a name="validate-the-claims"></a>Valideren van de claims
Wanneer uw app of API ontvangt een token ID, moet deze ook verschillende controles op basis van de claims in het token ID uitvoeren. Deze omvatten, maar zijn niet beperkt tot:

* De **doelgroep** claim: Hiermee wordt gecontroleerd of het token ID is bedoeld om te krijgen tot uw app.
* De **niet voor** en **verlooptijd** claims: deze controleren of het token ID is niet verlopen.
* De **verlener** claim: Hiermee wordt gecontroleerd of het token aan uw app is uitgegeven door Azure AD.
* De **nonce**: dit is een strategie voor tokenreplay aanval risicobeperking.

Voor een volledige lijst van validaties uw app moet worden uitgevoerd, raadpleegt u de [specificatie van het OpenID Connect](https://openid.net). Details van de verwachte waarden voor deze claims worden opgenomen in de voorgaande [token sectie](#types-of-tokens).  

## <a name="token-lifetimes"></a>Levensduur van token
De levensduur van de volgende tokens zijn bedoeld om uw kennis meer. Ze kunnen u helpen bij het ontwikkelen en foutopsporing van apps. Houd er rekening mee dat uw apps niet kunt verwachten van een van deze levensduur ongewijzigd moeten worden geschreven. Ze kunnen en wordt gewijzigd. Meer informatie over de [aanpassing van de levensduur van token](active-directory-b2c-token-session-sso.md) in Azure AD B2C.

| Token | Levensduur | Beschrijving |
| --- | --- | --- |
| ID-tokens |Een uur |ID-tokens zijn doorgaans geldig voor een uur. Uw web-app kunt deze levensduur gebruiken om de eigen sessies onderhouden met gebruikers (aanbevolen). U kunt ook de levensduur van een andere sessie. Als uw app een nieuwe ID-token ophalen moet, moet deze gewoon een nieuwe aanvraag voor aanmelden bij Azure AD maken. Als een gebruiker een ongeldig browsersessie met Azure AD heeft, wordt die gebruiker mogelijk niet vereist referenties opnieuw invoeren. |
| Vernieuwen van tokens |14 dagen |Een enkele vernieuwingstoken is geldig voor een maximum van 14 dagen. Echter kan een vernieuwingstoken op elk gewenst moment voor een aantal redenen ongeldig worden. Uw app moet blijven proberen te gebruiken een vernieuwingstoken totdat de aanvraag is mislukt, of uw app vervangen door het vernieuwingstoken een nieuw. Een vernieuwingstoken kan ook ongeldig als 90 dagen zijn verstreken nadat de gebruiker referenties voor het laatst ingevoerd. |
| Autorisatiecodes |Vijf minuten |Autorisatiecodes zijn opzettelijk tijdelijke. Ze moeten worden ingewisseld onmiddellijk voor toegangstokens, ID-tokens of vernieuwen van tokens wanneer ze worden ontvangen. |

