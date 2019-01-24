---
title: Token verwijzing in Azure Active Directory B2C | Microsoft Docs
description: De typen tokens die zijn uitgegeven in Azure Active Directory B2C
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 47cfd5820c80a0f53772f5424f674603acdaf18d
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54848934"
---
# <a name="azure-ad-b2c-token-reference"></a>Azure AD B2C: Tokenverwijzing

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C) verzendt verschillende typen beveiligingstokens tijdens de verwerking van elk [verificatiestroom](active-directory-b2c-apps.md). Dit document beschrijft de indeling, de beveiligingskenmerken en de inhoud van elk type token.

## <a name="types-of-tokens"></a>Typen tokens
Azure AD B2C ondersteunt de [OAuth 2.0-protocol voor autorisatie](active-directory-b2c-reference-protocols.md), welke maakt gebruik van beide toegang tot tokens en tokens te vernieuwen. Het biedt ook ondersteuning voor verificatie en aanmelding via [OpenID Connect](active-directory-b2c-reference-protocols.md), dat een derde type token introduceert: de ID-token. Elk van deze tokens wordt weergegeven als een bearer-token.

Een bearer-token is een lichtgewicht beveiligingstoken die de "bearer" toegang tot een beveiligde bron verleent. De houder is een partij die het token kan opleveren. Azure AD B2C een partij moeten eerst worden geverifieerd voordat het bearer-token kan ontvangen. Maar als de vereiste stappen zijn niet in gebruik voor het beveiligen van de token in overdracht en opslag, kan worden onderschept en die worden gebruikt door een onbedoelde partij. Sommige beveiligingstokens hebben een ingebouwd mechanisme voor zo wordt voorkomen dat onbevoegden gebruik ervan, maar bearer-tokens geen dit mechanisme. Zij moeten worden overgebracht in een beveiligd kanaal, zoals transport layer security (HTTPS).

Als een bearer-token buiten een beveiligd kanaal wordt verzonden, kunnen een schadelijke partij een man-in-the-middle-aanval kunt gebruiken om de token verkrijgen en gebruiken om ongeoorloofde toegang verlenen tot een beveiligde bron. Dezelfde beveiligingsprincipes van toepassing wanneer bearer-tokens worden opgeslagen of in de cache voor later gebruik opgeslagen. Altijd voor zorgen dat uw app worden verzonden en bearer-tokens worden opgeslagen op een veilige manier.

Zie voor aanvullende beveiligingsoverwegingen op bearer-tokens [RFC 6750 sectie 5](https://tools.ietf.org/html/rfc6750).

Veel van de tokens die problemen met Azure AD B2C worden geïmplementeerd als JSON-webtokens (JWTs). Een JWT is een compacte, URL-veilige manier van het overbrengen van gegevens tussen twee partijen. JWTs bevatten informatie bekend als claims. Dit zijn asserties van informatie over het bearer- en het onderwerp van het token. De claims in JWTs zijn JSON-objecten die zijn gecodeerd en voor de overdracht is geserialiseerd. Omdat de JWTs dat is uitgegeven door Azure AD B2C zijn ondertekend, maar niet versleuteld, kunt u eenvoudig controleren op de inhoud van een JWT voor foutopsporing kunt uitvoeren. Er zijn verschillende hulpprogramma's beschikbaar die kan dit doet, met inbegrip van [jwt.ms](https://jwt.ms). Raadpleeg voor meer informatie over JWTs [JWT-specificaties](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

### <a name="id-tokens"></a>Id-tokens

Een ID-token is een soort beveiligingstoken dat uw app van de Azure AD B2C ontvangt `/authorize` en `/token` eindpunten. ID-tokens worden weergegeven als [JWTs](#types-of-tokens), en bevatten claims die u gebruiken kunt om gebruikers in uw app te identificeren. Wanneer ID-tokens worden verkregen via de `/authorize` eindpunt, ze worden gedaan met behulp van de [impliciete stroom](active-directory-b2c-reference-spa.md), die vaak wordt gebruikt voor gebruikers die zich in voor javascript op basis van webtoepassingen. Wanneer ID-tokens worden verkregen via de `/token` eindpunt, ze worden gedaan met behulp van de [vertrouwelijke codestroom](active-directory-b2c-reference-oidc.md), die het token dat is verborgen in de browser houdt. Hiermee wordt het token voor het veilig in HTTP-aanvragen voor de communicatie tussen de twee onderdelen van de dezelfde toepassing of service worden verzonden. U kunt de claims in een ID-token dat wens naar. Ze worden vaak gebruikt om gegevens weer te geven of beslissingen over toegangsbeheer in een app.  

ID-tokens zijn ondertekend, maar ze zijn niet versleuteld. Wanneer uw app of API ontvangt een ID-token, moet het [valideert de handtekening](#token-validation) om te bewijzen dat het token authentiek is. Uw app of API moet ook valideren voor een aantal claims in het token om te bewijzen dat deze geldig is. Afhankelijk van de scenariovereisten, de claims die zijn gevalideerd door een app kunnen verschillen, maar uw app moet uitvoeren [algemene claim validaties](#token-validation) in elk scenario.

#### <a name="sample-id-token"></a>Voorbeeld-id-token
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

Een toegangstoken is ook een vorm van een beveiligingstoken dat uw app van de Azure AD B2C ontvangt `/authorize` en `/token` eindpunten. Toegangstokens worden ook weergegeven als [JWTs](#types-of-tokens), en bevatten claims die u gebruiken kunt voor het identificeren van de toegekende machtigingen aan uw API's. Toegangstokens zijn ondertekend, maar ze zijn niet versleuteld. Toegangstokens moeten worden gebruikt voor toegang tot API's en resource-servers. Meer informatie over het [toegangstokens gebruiken](active-directory-b2c-access-tokens.md). 

Wanneer uw API een toegangstoken ontvangt, moet het [valideert de handtekening](#token-validation) om te bewijzen dat het token authentiek is. Uw API moet ook een aantal claims in het token om te bewijzen dat deze geldig is gevalideerd. Afhankelijk van de scenariovereisten, de claims die zijn gevalideerd door een app kunnen verschillen, maar uw app moet uitvoeren [algemene claim validaties](#token-validation) in elk scenario.

### <a name="claims-in-id-and-access-tokens"></a>Claims in het ID- en toegangstokens

Wanneer u Azure AD B2C gebruikt, hebt u heel nauwkeurig bepalen de inhoud van uw tokens. U kunt configureren [gebruikersstromen](active-directory-b2c-reference-policies.md) en aangepaste beleidsregels voor het verzenden van bepaalde verzamelingen van gebruikersgegevens in de claims die uw app nodig voor de bewerkingen heeft. Deze claims standaardeigenschappen zoals van de gebruiker kunnen bevatten `displayName` en `emailAddress`. Ze kunnen ook bevatten [aangepaste gebruikerskenmerken](active-directory-b2c-reference-custom-attr.md) die u kunt definiëren in uw B2C-directory. Elke-ID en access token die u ontvangt een bepaalde set claims met betrekking tot beveiliging bevat. Uw toepassingen kunnen deze claims gebruiken om veilige verificatie van gebruikers en aanvragen.

Houd er rekening mee dat de claims in het ID-tokens worden niet geretourneerd in een bepaalde volgorde. Bovendien kunnen u nieuwe claims geïntroduceerd in de ID-tokens op elk gewenst moment. Uw app moet niet verbroken als nieuwe claims worden geïntroduceerd. Hier volgen de claims die u verwacht te bestaan in het ID- en toegangstokens dat is uitgegeven door Azure AD B2C. Aanvullende claims worden bepaald door het beleid. Probeer te inspecteren van de claims in het voorbeeld-ID-token door te plakken in oefening [jwt.ms](https://jwt.ms). Meer informatie vindt u de [OpenID Connect-specificatie](https://openid.net/specs/openid-connect-core-1_0.html).

| Name | Claim | Voorbeeldwaarde | Description |
| --- | --- | --- | --- |
| Doelgroep |`aud` |`90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6` |Een claim doelgroep identificeert de beoogde ontvanger van het token. Voor Azure AD B2C is de doelgroep van uw app toepassings-ID, zoals toegewezen aan uw app in de portal voor app-registratie. Uw app moet deze waarde te valideren en het token af te wijzen als komt niet overeen met. Doelgroep is gelijk aan de resource. |
| Verlener |`iss` |`https://{tenantname}.b2clogin.com/775527ff-9a37-4307-8b3d-cc311f58d925/v2.0/` |Deze claim identificeert de beveiligingstokenservice (STS) die wordt gemaakt en het token retourneert. Het identificeert ook de Azure AD-directory waarin de gebruiker is geverifieerd. Uw app moet de claim van verlener om ervoor te zorgen dat het token afkomstig van het Azure Active Directory v2.0-eindpunt is te valideren. |
| Uitgegeven op |`iat` |`1438535543` |Deze claim is de tijd waarop het token is uitgegeven, in epoche-tijd weergegeven. |
| Vervaltijd |`exp` |`1438539443` |Verlooptijd van de claim is de tijd waarop het token ongeldig is, wordt weergegeven in epoche-tijd. Uw app moet deze claim gebruiken om de geldigheid van de levensduur van tokens. |
| Niet voor |`nbf` |`1438535543` |Deze claim wordt de tijd waarop het token geldig, in epoche-tijd weergegeven wordt. Dit is meestal hetzelfde als de tijd die het token is uitgegeven. Uw app moet deze claim gebruiken om de geldigheid van de levensduur van tokens. |
| Versie |`ver` |`1.0` |Dit is de versie van de ID-token, zoals gedefinieerd door Azure AD. |
| Code-hash |`c_hash` |`SGCPtt01wxwfgnYZy2VJtQ` |Een hash van de code is opgenomen in een ID-token dat alleen wanneer het token is uitgegeven, samen met een OAuth 2.0-autorisatiecode. Een code-hash kan worden gebruikt om te valideren de echtheid van een autorisatiecode. Zie voor meer informatie over het uitvoeren van deze validatie de [OpenID Connect-specificatie](https://openid.net/specs/openid-connect-core-1_0.html).  |
| Access token-hash |`at_hash` |`SGCPtt01wxwfgnYZy2VJtQ` |Een access token-hash is opgenomen in een ID-token dat alleen wanneer het token is uitgegeven, samen met een OAuth 2.0-toegangstoken. Een access token-hash kan worden gebruikt om te valideren de echtheid van een toegangstoken. Zie voor meer informatie over het uitvoeren van deze validatie de [OpenID Connect-specificatie](https://openid.net/specs/openid-connect-core-1_0.html)  |
| nonce |`nonce` |`12345` |Een nonce is een strategie gebruikt voor het beperken van token opnieuw afspelen aanvallen. Uw app een nonce kunt opgeven in een autorisatieaanvraag met behulp van de `nonce` queryparameter. De waarde die u in de aanvraag opgeeft wordt verzonden wijzigen de `nonce` claimen van een ID-token dat alleen. Dit kan uw app om te controleren of de waarde op basis van de waarde die deze in de aanvraag, waarmee de sessie van de app aan een bepaalde ID-token dat koppelt is opgegeven. Deze validatie tijdens de validatie van tokens proces-ID moet worden uitgevoerd in uw app. |
| Onderwerp |`sub` |`884408e1-2918-4cz0-b12d-3aa027d7563b` |Dit is de principal waarover het token worden bevestigd met gegevens, zoals de gebruiker van een app. Deze waarde is onveranderbaar en kan niet worden toegewezen of opnieuw gebruikt. Het kan worden gebruikt voor autorisatie controles veilig uitvoeren, zoals wanneer het token wordt gebruikt voor toegang tot een resource. Standaard wordt de claim onderwerp gevuld met de object-ID van de gebruiker in de map. Zie voor meer informatie, [Azure Active Directory B2C: Token-, sessie en configuratie voor eenmalige aanmelding](active-directory-b2c-token-session-sso.md). |
| Authentication context class reference |`acr` |Niet van toepassing |Momenteel niet gebruikt, behalve in het geval van oudere beleid. Zie voor meer informatie, [Azure Active Directory B2C: Token-, sessie en configuratie voor eenmalige aanmelding](active-directory-b2c-token-session-sso.md). |
| Framework vertrouwensbeleid |`tfp` |`b2c_1_sign_in` |Dit is de naam van het beleid dat is gebruikt om de ID-token te verkrijgen. |
| Verificatie-tijd |`auth_time` |`1438535543` |Deze claim is de tijd waarop een laatste opgegeven gebruikersreferenties, in epoche-tijd weergegeven. |

### <a name="refresh-tokens"></a>Vernieuwen van tokens
Vernieuwen van tokens zijn beveiligingstokens die uw app gebruiken kunt om nieuwe ID-tokens verkrijgen en toegangstokens in een stroom voor OAuth 2.0. Ze uw app op de lange termijn toegang bieden tot bronnen namens gebruikers zonder interactie met de gebruikers.

Voor het ontvangen van een vernieuwing token een token antwoord wordt uw app moet aanvragen de `offline_access` bereik. Voor meer informatie over de `offline_access` scope, raadpleeg dan de [naslaginformatie over Azure AD B2C-protocol](active-directory-b2c-reference-protocols.md).

Vernieuwen van tokens, en altijd zullen zijn, helemaal ondoorzichtig aan uw app. Ze kunnen zijn uitgegeven door Azure AD worden gecontroleerd en geïnterpreteerd alleen door Azure AD. Ze zijn lange levensduur hebben, maar uw app moet niet worden geschreven met de verwachting dat een vernieuwingstoken voor een bepaalde periode duurt. Vernieuwingstokens kunnen op elk moment voor een aantal redenen zijn ongeldig. De enige manier om uw app om te weten of een vernieuwingstoken geldig is, is om te je deze door een token aan te vragen naar Azure AD.

Wanneer u een vernieuwingstoken voor een nieuw token inwisselt (en als uw app heeft gekregen de `offline_access` bereik), ontvangt u een nieuwe vernieuwingstoken in het token antwoord. U moet de zojuist uitgegeven vernieuwingstoken opslaan. Deze moet het vernieuwingstoken dat u eerder hebt gebruikt in de aanvraag vervangen. Dit helpt te garanderen dat uw vernieuwingstokens geldig voor zo lang mogelijk blijven.

## <a name="token-validation"></a>Validatie van tokens
Voor het valideren van een token, Controleer uw app de handtekening en de claims van het token.

Aantal open-source-bibliotheken zijn beschikbaar voor het valideren van JWTs, afhankelijk van de taal van uw voorkeur. U wordt aangeraden dat u deze opties verkennen in plaats van uw eigen validatielogica implementeren. De informatie in deze handleiding kunt u informatie over het correct gebruik deze bibliotheken.

### <a name="validate-the-signature"></a>Valideert de handtekening
Een JWT bevat drie segmenten, gescheiden door het `.` teken. Het eerste segment is het *header*, de tweede is het *hoofdtekst*, en de derde is de *handtekening*. Het segment handtekening kan worden gebruikt om te valideren de echtheid van het token, zodat deze kan worden vertrouwd door uw app.

Azure AD B2C-tokens zijn ondertekend met behulp van asymmetrische codering van industriestandaard-algoritmen, zoals RSA-256. De header van het token bevat informatie over de sleutel en -versleuteling-methode gebruikt voor het ondertekenen van het token:

```
{
        "typ": "JWT",
        "alg": "RS256",
        "kid": "GvnPApfWMdLRi8PDmisFn7bprKg"
}
```

De `alg` claim geeft aan dat de algoritme die is gebruikt voor het ondertekenen van het token. De `kid` claim geeft aan dat de bepaalde openbare sleutel die is gebruikt voor het ondertekenen van het token.

Op elk moment kan Azure AD een token ondertekenen met behulp van een van een bepaalde set van paren van openbare en persoonlijke sleutels. Azure AD draait het mogelijke set sleutels periodiek, zodat uw app voor een correcte verwerking die automatisch moet worden geschreven. Een redelijke frequentie om te controleren op updates voor de openbare sleutels die worden gebruikt door Azure AD is elke 24 uur.

Azure AD B2C is een eindpunt van de metagegevens OpenID Connect. Hiermee kunt apps voor het ophalen van informatie over Azure AD B2C tijdens runtime. Deze informatie omvat eindpunten, de inhoud van tokens en voor token-ondertekening van sleutels. Uw B2C-map bevat een JSON-metagegevensdocument voor elk beleid. Bijvoorbeeld, het document met metagegevens voor de `b2c_1_sign_in` beleid in `fabrikamb2c.onmicrosoft.com` bevindt zich in:

```
https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in
```

`fabrikamb2c.onmicrosoft.com` de B2C-directory gebruikt voor het verifiëren van de gebruiker is en `b2c_1_sign_in` is het beleid voor het verkrijgen van het token. Om te bepalen welk beleid is gebruikt voor het ondertekenen van een token (en waar u om op te halen de metagegevens), hebt u twee opties. Ten eerste de naam van het beleid is opgenomen in de `acr` claim in het token. U kunt claims uit de hoofdtekst van het JWT parseren met base 64-decodering van de instantie en bij het deserialiseren van de JSON-tekenreeks die het resultaat. De `acr` claim is de naam van het beleid dat is gebruikt voor het uitgeven van het token.  De andere mogelijkheid is het coderen van het beleid in de waarde van de `state` parameter wanneer u de aanvraag uitgeven, en vervolgens worden ontsleuteld om te controleren welk beleid is gebruikt. Een van beide methoden is geldig.

De metadata-document is een JSON-object dat handig stukjes informatie bevat. Het gaat hierbij om de locatie van de eindpunten voor het uitvoeren van de OpenID Connect-verificatie vereist. Ook `jwks_uri`, waardoor de locatie van de set met openbare sleutels die worden gebruikt voor het ondertekenen van tokens. Dat locatie is die hier beschikbaar zijn, maar het is raadzaam om op te halen van de locatie dynamisch met behulp van het document met metagegevens en parseren `jwks_uri`:

```
https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in
```

Het JSON-document zich op de volgende URL bevat alle openbare sleutels informatie wordt gebruikt op een bepaald moment. Uw app kan gebruiken de `kid` claim in de JWT-header de openbare sleutel in de JSON-document dat wordt gebruikt voor het ondertekenen van een bepaalde token selecteren. Deze kan vervolgens handtekeningvalidatie uitvoeren met behulp van de juiste openbare sleutel en het opgegeven algoritme.

Een beschrijving van het uitvoeren van handtekeningvalidatie valt buiten het bereik van dit document. Aantal open-source-bibliotheken zijn beschikbaar voor hulp bij dit als u deze nodig hebt.

### <a name="validate-the-claims"></a>Valideren van de claims
Wanneer uw app of API ontvangt een ID-token, moet deze ook verschillende controles op basis van de claims in het ID-token uitvoeren. Deze omvatten, maar zijn niet beperkt tot:

* De **doelgroep** claim: Hiermee wordt gecontroleerd dat de ID-token dat is bedoeld om u te krijgen tot uw app.
* De **niet vóór** en **verlooptijd** claims: Deze controleren of de ID-token is niet verlopen.
* De **verlener** claim: Hiermee wordt gecontroleerd of het token aan uw app is uitgegeven door Azure AD.
* De **nonce**: Dit is een strategie voor het token opnieuw afspelen netwerkaanval.

Voor een volledige lijst van validaties moet worden uitgevoerd voor uw app, raadpleegt u de [OpenID Connect-specificatie](https://openid.net). Details van de verwachte waarden voor deze claims worden opgenomen in de voorgaande [token sectie](#types-of-tokens).  

## <a name="token-lifetimes"></a>Levensduur van tokens
De levensduur van de volgende tokens zijn bedoeld om uw kennis verder. Ze kunnen u helpen bij het ontwikkelen en fouten opsporen in apps. Houd er rekening mee dat uw apps niet worden geschreven in u kunt verwachten van een van deze levensduur constant blijft. Ze kunnen en verandert. Meer informatie over de [aanpassing van de levensduur van tokens](active-directory-b2c-token-session-sso.md) in Azure AD B2C.

| Token | Levensduur | Description |
| --- | --- | --- |
| Id-tokens |Eén uur |ID-tokens zijn doorgaans geldig voor een uur. Uw web-app kunt u deze levensduur gebruiken voor het onderhouden van een eigen sessies met gebruikers (aanbevolen). U kunt ook de levensduur van een andere sessie. Als uw app een nieuwe ID-token ophalen moet, deze alleen toegang nodig heeft om een nieuwe aanvraag voor aanmelding met Azure AD. Als een gebruiker een ongeldig browsersessie met Azure AD heeft, kan die gebruiker niet worden vereist referenties opnieuw invoeren. |
| Vernieuwen van tokens |Tot 14 dagen |Een enkele vernieuwingstoken is geldig voor maximaal 14 dagen. Een vernieuwingstoken kan echter worden ongeldig op elk gewenst moment voor een aantal oorzaken hebben. Uw app moet nog steeds gebruik van een vernieuwingstoken totdat de aanvraag is mislukt, of uw app vervangt het vernieuwingstoken dat door een nieuwe. Een vernieuwingstoken kan ook worden ongeldig als 90 dagen zijn verstreken nadat de gebruiker referenties voor het laatst ingevoerd. |
| Verificatiecodes |Vijf minuten |Verificatiecodes zijn opzettelijk tijdelijke. Ze moeten worden ingewisseld onmiddellijk voor toegangstokens, ID-tokens of vernieuwingstokens wanneer ze worden ontvangen. |

