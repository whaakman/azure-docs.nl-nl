---
title: Overzicht van tokens - Azure Active Directory B2C | Microsoft Docs
description: Meer informatie over de tokens die worden gebruikt in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 11361bc6ab75e873e1b4081dcfc6492abc093b54
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59680261"
---
# <a name="overview-of-tokens-in-azure-active-directory-b2c"></a>Overzicht van de tokens in Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD) B2C verzendt verschillende typen beveiligingstokens tijdens de verwerking van elk [verificatiestroom](active-directory-b2c-apps.md). Dit document beschrijft de indeling, de beveiligingskenmerken en de inhoud van elk type token.

## <a name="token-types"></a>Typen van token

Azure AD B2C ondersteunt de [OAuth 2.0 en OpenID Connect-protocollen](active-directory-b2c-reference-protocols.md), welke maakt gebruik van tokens voor verificatie en beveiligde toegang tot bronnen. Alle tokens die worden gebruikt in Azure AD B2C zijn [JSON webtokens (JWTs)](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) die asserties van informatie over het bearer- en het onderwerp van het token bevatten.

De volgende tokens worden gebruikt in de communicatie met Azure AD B2C:

- *ID-token* -een JWT met claims die u gebruiken kunt om gebruikers in uw toepassing te identificeren. Dit token wordt veilig in HTTP-aanvragen voor de communicatie tussen de twee onderdelen van de dezelfde toepassing of service verzonden. U kunt de claims in een ID-token dat wens naar. Ze worden vaak gebruikt om gegevens weer te geven of beslissingen over toegangsbeheer in een toepassing. ID-tokens zijn ondertekend, maar ze zijn niet versleuteld. Wanneer uw toepassing of de API ontvangt een ID-token, moet de handtekening om te bewijzen dat het token authentiek is worden gevalideerd. Uw toepassing of de API moet ook valideren voor een aantal claims in het token om te bewijzen dat deze geldig is. De claims die zijn gevalideerd door een toepassing kunnen variëren, afhankelijk van de scenariovereisten, maar uw toepassing moet enkele algemene claim validaties in elk scenario uitvoeren.
- *Toegangstoken* -een JWT met claims die u gebruiken kunt voor het identificeren van de toegekende machtigingen aan uw API's. Toegangstokens zijn ondertekend, maar ze worden niet versleuteld. Toegangstokens worden gebruikt voor toegang tot API's en resource-servers.  Wanneer uw API een toegangstoken ontvangt, moet de handtekening om te bewijzen dat het token authentiek is worden gevalideerd. Uw API moet ook een aantal claims in het token om te bewijzen dat deze geldig is gevalideerd. De claims die zijn gevalideerd door een toepassing kunnen variëren, afhankelijk van de scenariovereisten, maar uw toepassing moet enkele algemene claim validaties in elk scenario uitvoeren.
- *Token vernieuwen* -vernieuwingstokens worden gebruikt voor het verkrijgen van de nieuwe ID-tokens en tokens in een stroom voor OAuth 2.0 openen. Ze opgeven uw toepassing op de lange termijn toegang tot resources namens gebruikers zonder interactie met die gebruikers. Vernieuwen van tokens zijn niet transparant voor uw toepassing. Ze kunnen zijn uitgegeven door Azure AD B2C en worden gecontroleerd en geïnterpreteerd alleen door Azure AD B2C. Ze zijn lange levensduur hebben, maar uw toepassing mag niet worden geschreven met de verwachting dat een vernieuwingstoken voor een bepaalde periode duurt. Vernieuwingstokens kunnen op elk moment voor een aantal redenen zijn ongeldig. De enige manier om uw toepassing om te weten of een vernieuwingstoken geldig is, is om te je deze door een aanvraag voor een toegangstoken aan Azure AD B2C. Wanneer u een vernieuwingstoken voor een nieuw token inwisselen, ontvangt u een nieuwe vernieuwingstoken in het token antwoord. Sla de nieuwe vernieuwingstoken. Het vernieuwingstoken dat u eerder hebt gebruikt in de aanvraag wordt vervangen. Deze actie kunt garanderen dat uw vernieuwingstokens geldig voor zo lang mogelijk blijven. 

## <a name="endpoints"></a>Eindpunten

Een [toepassing geregistreerd](tutorial-register-applications.md) tokens ontvangt en communiceert met Azure AD B2C door het verzenden van aanvragen naar deze eindpunten:

- `https://{tenant}.b2clogin.com/{tenant}.onmicrosoft.com/oauth2/v2.0/authorize`
- `https://{tenant}.b2clogin.com/{tenant}.onmicrosoft.com/oauth2/v2.0/token`

Beveiligingstokens die uw toepassing van Azure AD B2C ontvangt kunnen afkomstig zijn uit de `/authorize` of `/token` eindpunten. Wanneer ID-tokens worden verkregen via de `/authorize` eindpunt, ze worden gedaan met behulp van de [impliciete stroom](active-directory-b2c-reference-spa.md), die vaak wordt gebruikt voor gebruikers die zich in op basis van de javascript web-apps. Wanneer ID-tokens worden verkregen via de `/token` eindpunt, ze worden gedaan met behulp van de [vertrouwelijke codestroom](active-directory-b2c-reference-oidc.md), die het token dat is verborgen in de browser houdt.

## <a name="claims"></a>Claims

Wanneer u Azure AD B2C gebruikt, hebt u heel nauwkeurig bepalen de inhoud van uw tokens. U kunt configureren [gebruikersstromen](active-directory-b2c-reference-policies.md) en [aangepast beleid](active-directory-b2c-overview-custom.md) voor het verzenden van bepaalde verzamelingen van gebruikersgegevens in de claims die vereist voor uw toepassing zijn. Deze claims kunnen bevatten standaardeigenschappen als **displayName** en **emailAddress**. Uw toepassingen kunnen deze claims gebruiken om veilige verificatie van gebruikers en aanvragen. 

De claims in het ID-tokens worden niet geretourneerd in een bepaalde volgorde. Nieuwe claims kunnen worden ingevoerd in het ID-tokens op elk gewenst moment. Uw toepassing mag niet worden verbroken wanneer nieuwe claims binnenkomen. U kunt ook opnemen [aangepaste gebruikerskenmerken](active-directory-b2c-reference-custom-attr.md) in de claims.

De volgende tabel bevat de claims die u kunt verwachten in de ID-tokens en tokens die zijn uitgegeven door Azure AD B2C toegang.

| Name | Claim | Voorbeeldwaarde | Description |
| ---- | ----- | ------------- | ----------- |
| Doelgroep | `aud` | `90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6` | Hiermee geeft u de beoogde ontvanger van het token. Voor Azure AD B2C is de doelgroep de toepassings-ID. Uw toepassing moet deze waarde te valideren en het token te negeren als deze niet overeenkomt met. Doelgroep is gelijk aan de resource. |
| Verlener | `iss` |`https://{tenant}.b2clogin.com/775527ff-9a37-4307-8b3d-cc311f58d925/v2.0/` | Identificeert de beveiligingstokenservice (STS) die wordt gemaakt en het token retourneert. Het identificeert ook de map waarin de gebruiker is geverifieerd. Uw toepassing moet de claim van verlener om ervoor te zorgen dat het token afkomstig van het juiste eindpunt is valideren. |
| Uitgegeven op | `iat` | `1438535543` | De tijd waarop het token is uitgegeven, weergegeven in epoche-tijd. |
| Vervaltijd | `exp` | `1438539443` | De tijd waarop het token ongeldig is, wordt weergegeven in epoche-tijd. Deze claim moet in uw toepassing gebruiken om te controleren of de geldigheid van de levensduur van tokens. |
| Niet voor | `nbf` | `1438535543` | De tijd waarop het token geldig is, wordt weergegeven in epoche-tijd. Deze tijd is meestal hetzelfde als de tijd die het token is uitgegeven. Deze claim moet in uw toepassing gebruiken om te controleren of de geldigheid van de levensduur van tokens. |
| Versie | `ver` | `1.0` | De versie van de ID-token, zoals gedefinieerd door Azure AD B2C. |
| Code-hash | `c_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | Een code-hash is opgenomen in een ID-token dat alleen wanneer het token is uitgegeven, samen met een OAuth 2.0-autorisatiecode. Een code-hash kan worden gebruikt om te valideren de echtheid van een autorisatiecode. Zie voor meer informatie over het uitvoeren van deze validatie de [OpenID Connect-specificatie](https://openid.net/specs/openid-connect-core-1_0.html).  |
| Access token-hash | `at_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | Een access token-hash opgenomen in een ID-token dat alleen wanneer het token is uitgegeven, samen met een OAuth 2.0-toegangstoken. Een access token-hash kan worden gebruikt om te valideren de echtheid van een toegangstoken. Zie voor meer informatie over het uitvoeren van deze validatie de [OpenID Connect-specificatie](https://openid.net/specs/openid-connect-core-1_0.html)  |
| nonce | `nonce` | `12345` | Een nonce is een strategie gebruikt voor het beperken van token opnieuw afspelen aanvallen. Uw toepassing een nonce kunt opgeven in een autorisatieaanvraag met behulp van de `nonce` queryparameter. De waarde die u in de aanvraag opgeeft is verzonden niet gewijzigd de `nonce` claimen van een ID-token dat alleen. Deze claim kan uw toepassing om te controleren of de waarde op basis van de waarde die is opgegeven in de aanvraag. Uw toepassing moeten deze validatie tijdens de validatie van tokens proces-ID worden uitgevoerd. |
| Onderwerp | `sub` | `884408e1-2918-4cz0-b12d-3aa027d7563b` | De principal waarover het token worden bevestigd met gegevens, zoals de gebruiker van een toepassing. Deze waarde is onveranderbaar en kan niet worden toegewezen of opnieuw gebruikt. Het kan worden gebruikt voor autorisatie controles veilig uitvoeren, zoals wanneer het token wordt gebruikt voor toegang tot een resource. Standaard wordt de claim onderwerp gevuld met de object-ID van de gebruiker in de map. |
| Authentication context class reference | `acr` | Niet van toepassing | Alleen gebruikt met oudere beleid. |
| Framework vertrouwensbeleid | `tfp` | `b2c_1_signupsignin1` | De naam van het beleid dat is gebruikt om de ID-token te verkrijgen. |
| Verificatie-tijd | `auth_time` | `1438535543` | De tijd die een gebruiker laatste referenties ingevoerd weergegeven in epoche-tijd. |
| Bereik | `scp` | `Read`| De machtigingen verleend aan de resource voor een toegangstoken. Meerdere toegekende machtigingen worden gescheiden door een spatie. |
| Geautoriseerde partij | `azp` | `975251ed-e4f5-4efd-abcb-5f1a8f566ab7` | De **toepassings-ID** van de clienttoepassing die de aanvraag wordt gestart. |

## <a name="configuration"></a>Configuratie

De volgende eigenschappen worden gebruikt om [beheren van de levensduur van beveiligingstokens](configure-tokens.md) gegenereerd door Azure AD B2C:

- **Toegang en ID-token levensduur (minuten)** - de levensduur van het OAuth 2.0-bearer-token gebruikt voor toegang tot een beveiligde resource. De standaardwaarde is 60 minuten. De (inclusieve) minimumwaarde is 5 minuten. Het maximum (inclusief) is 1440 minuten.

- **Levensduur van vernieuwingstoken (dagen)** : de maximale periode waarbinnen een vernieuwingstoken kan worden gebruikt om een nieuwe toegang of de ID-token verkrijgen. De periode geldt ook voor een nieuwe vernieuwingstoken ophalen als uw toepassing is verleend de `offline_access` bereik. De standaardwaarde is 14 dagen. De minimale (inclusief) is één dag. Het maximum (inclusief) is 90 dagen.

- **Levensduur van sliding window token vernieuwen (dagen)** - na deze tijd is verstreken periode de gebruiker wordt gedwongen om te verifiëren, ongeacht de geldigheidsperiode van de meest recente vernieuwingstoken verkregen door de toepassing. Kan alleen worden opgegeven als de switch is ingesteld op **gebonden**. Het moet groter zijn dan of gelijk zijn aan de **levensduur van vernieuwingstoken (dagen)** waarde. Als de switch is ingesteld op **Unbounded**, u niet een bepaalde waarde opgeven. De standaardwaarde is 90 dagen. De minimale (inclusief) is één dag. Het maximum (inclusief) is 365 dagen.

De volgende gevallen gebruik worden ingeschakeld met behulp van deze eigenschappen:

- Toestaan dat een gebruiker aangemeld bij een mobiele toepassing blijven voor onbepaalde tijd, zolang de gebruiker op de toepassing voortdurend actief is. U kunt instellen **Refresh token levensduur van sliding window (dagen)** naar **Unbounded** in uw gebruikersstroom aanmelden.
- Voldoen aan uw toonaangevende beveiligings- en nalevingsvereisten door in te stellen van de levensduur van de juiste toegang-tokens.

Deze instellingen zijn niet beschikbaar voor wachtwoord opnieuw instellen van gebruikersstromen. 

## <a name="compatibility"></a>Compatibiliteit

De volgende eigenschappen worden gebruikt om [tokencompatibiliteit beheren](configure-tokens.md):

- **Claim van verlener (iss)** -deze eigenschap geeft u de Azure AD B2C-tenant die het token is uitgegeven. De standaardwaarde is `https://<domain>/{B2C tenant GUID}/v2.0/`. De waarde van `https://<domain>/tfp/{B2C tenant GUID}/{Policy ID}/v2.0/` id's bevat voor zowel de Azure AD B2C-tenant en de gebruikersstroom die is gebruikt in het token aan te vragen. Als uw toepassing of de tapewisselaar moet Azure AD B2C om te voldoen aan de [OpenID Connect Discovery 1.0-specificatie](https://openid.net/specs/openid-connect-discovery-1_0.html), gebruikt deze waarde.

- **Onderwerp (sub) claim** -deze eigenschap geeft u de entiteit die het token worden bevestigd met informatie. De standaardwaarde is **ObjectID**, die vult de `sub` claim in het token met de object-ID van de gebruiker. De waarde van **niet ondersteund** wordt alleen aangeboden voor achterwaartse compatibiliteit. Het wordt aanbevolen dat u naar overschakelt **ObjectID** zodra u zich kunt.

- **Claim voor beleids-ID** -deze eigenschap geeft het claimtype waarin de naam van het beleid wordt gebruikt in de aanvraag van het token is gevuld. De standaardwaarde is `tfp`. De waarde van `acr` wordt alleen aangeboden voor achterwaartse compatibiliteit.

## <a name="pass-through"></a>Pass-through

Wanneer een gebruikersbeleving wordt gestart, Azure AD B2C, een toegangstoken ontvangt van een id-provider. Azure AD B2C gebruikt dit token voor het ophalen van informatie over de gebruiker. U [Schakel een claim in de gebruikersstroom](idp-pass-through-user-flow.md) of [definiëren van een claim in het aangepaste beleid](idp-pass-through-custom.md) om het token door naar de toepassingen die u in Azure AD B2C registreert. Uw toepassing moet gebruikmaken van een [v2 gebruikersstroom](user-flow-versions.md) om te profiteren van het token wordt doorgegeven als een claim.

Azure AD B2C ondersteunt momenteel alleen het doorgeven van het toegangstoken van OAuth 2.0-identiteitsproviders, waaronder Facebook en Google. Voor alle andere id-providers, wordt de claim leeg geretourneerd. 

## <a name="validation"></a>Validatie

Voor het valideren van een token, moet uw toepassing controleren de handtekening en de claims van het token. Aantal open source-bibliotheken zijn beschikbaar voor het valideren van JWTs, afhankelijk van de taal van uw voorkeur. Het verdient aanbeveling dat u deze opties verkennen in plaats van uw eigen validatielogica implementeren.

### <a name="validate-signature"></a>Handtekening valideren

Een JWT bevat drie segmenten, een *header*, een *hoofdtekst*, en een *handtekening*. Het segment handtekening kan worden gebruikt om te valideren de echtheid van het token, zodat deze kan worden vertrouwd door uw toepassing. Azure AD B2C-tokens zijn ondertekend met behulp van asymmetrische codering van industriestandaard-algoritmen, zoals RSA-256. 

De header van het token bevat informatie over de sleutel en -versleuteling-methode gebruikt voor het ondertekenen van het token:

```
{
        "typ": "JWT",
        "alg": "RS256",
        "kid": "GvnPApfWMdLRi8PDmisFn7bprKg"
}
```

De waarde van de **alg** claim is het algoritme dat is gebruikt voor het ondertekenen van het token. De waarde van de **kind** claim wordt de openbare sleutel die is gebruikt voor het ondertekenen van het token. Azure AD B2C kunt een token ondertekenen met behulp van een van een set van paren van openbare en persoonlijke sleutels op een bepaald moment. Azure AD B2C wordt periodiek de mogelijke set sleutels draait. Uw toepassing moet worden geschreven om af te handelen die belangrijke wijzigingen automatisch. Een redelijke frequentie om te controleren op updates voor de openbare sleutels die worden gebruikt door Azure AD B2C wordt elke 24 uur.

Azure AD B2C is een eindpunt van de metagegevens OpenID Connect. Dit eindpunt, toepassingen, kunnen aanvragen informatie over Azure AD B2C tijdens runtime. Deze informatie omvat eindpunten, de inhoud van tokens en voor token-ondertekening van sleutels. Uw Azure AD B2C-tenant bevat een JSON-metagegevensdocument voor elk beleid. De metadata-document is een JSON-object dat handig stukjes informatie bevat. De metagegevens bevat **jwks_uri**, waardoor de locatie van de set met openbare sleutels die worden gebruikt voor het ondertekenen van tokens. Aanbevolen voor het ophalen van de locatie dynamisch met behulp van het document met metagegevens en parseren van die locatie is die hier beschikbaar zijn, maar de **jwks_uri**:

```
https://contoso.b2clogin.com/contoso.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_signupsignin1
```
Het JSON-document zich op de volgende URL bevat alle openbare sleutels informatie wordt gebruikt op een bepaald moment. Uw app kan gebruiken de `kid` claim in de JWT-header de openbare sleutel in de JSON-document dat wordt gebruikt voor het ondertekenen van een bepaalde token selecteren. Deze kan vervolgens handtekeningvalidatie uitvoeren met behulp van de juiste openbare sleutel en het opgegeven algoritme.

Het document met metagegevens voor de `B2C_1_signupsignin1` beleid in de `contoso.onmicrosoft.com` tenant bevindt zich in:

```
https://contoso.b2clogin.com/contoso.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_signupsignin1
```

Om te bepalen welk beleid is gebruikt voor het ondertekenen van een token (en waar u om aan te vragen van de metagegevens), hebt u twee opties. Ten eerste de naam van het beleid is opgenomen in de `acr` claim in het token. U kunt claims uit de hoofdtekst van het JWT parseren met base 64-decodering van de instantie en bij het deserialiseren van de JSON-tekenreeks die het resultaat. De `acr` claim is de naam van het beleid dat is gebruikt voor het uitgeven van het token. De andere mogelijkheid is het coderen van het beleid in de waarde van de `state` parameter wanneer u de aanvraag uitgeven, en vervolgens worden ontsleuteld om te controleren welk beleid is gebruikt. Een van beide methoden is geldig.

Een beschrijving van het uitvoeren van handtekeningvalidatie valt buiten het bereik van dit document. Er zijn veel open source-bibliotheken beschikbaar waarmee u een token te valideren.

### <a name="validate-claims"></a>Valideren van claims

Wanneer uw toepassingen of API ontvangt een ID-token, moet deze ook verschillende controles op basis van de claims in het ID-token uitvoeren. De volgende claims moeten worden gecontroleerd:

- **doelgroep** -controleert of dat de ID-token dat is bedoeld om u te krijgen tot uw toepassing.
- **niet voor** en **verlooptijd** -controleert of dat de ID-token dat nog niet is verlopen.
- **verlener** -controleert of het token is uitgegeven voor uw toepassing door Azure AD B2C.
- **nonce** -een strategie voor het token opnieuw afspelen netwerkaanval.

Voor een volledige lijst van validaties uw toepassing moeten worden uitgevoerd, raadpleegt u de [OpenID Connect-specificatie](https://openid.net).  

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [toegangstokens gebruiken](active-directory-b2c-access-tokens.md).

