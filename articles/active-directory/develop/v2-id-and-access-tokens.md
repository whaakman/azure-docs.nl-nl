---
title: Azure Active Directory v2.0 tokens referentie | Microsoft Docs
description: De typen tokens en claims die zijn gegenereerd door de Azure AD v2.0-eindpunt
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: dc58c282-9684-4b38-b151-f3e079f034fd
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: 7fcc39aed1599c1fa928736453082cbce6d7e27e
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39581407"
---
# <a name="azure-active-directory-v20-tokens-reference"></a>Referentie voor Azure Active Directory v2.0-tokens
De Azure Active Directory (Azure AD) v2.0-eindpunt verzendt verschillende typen beveiligingstokens in elk [verificatiestroom](active-directory-v2-flows.md). Deze referentie beschrijving van de indeling, de beveiligingskenmerken en de inhoud van elk type token.

> [!NOTE]
> Het v2.0-eindpunt biedt geen ondersteuning voor alle Azure Active Directory-scenario's en onderdelen. Om te bepalen of het v2.0-eindpunt moet worden gebruikt, lees meer over [v2.0 beperkingen](active-directory-v2-limitations.md).
>
>

## <a name="types-of-tokens"></a>Typen tokens
Het v2.0-eindpunt biedt ondersteuning voor de [OAuth 2.0-protocol voor autorisatie](active-directory-v2-protocols.md), welke maakt gebruik van toegangstokens en vernieuwingstokens. Het v2.0-eindpunt biedt ook ondersteuning voor verificatie en aanmelding via [OpenID Connect](active-directory-v2-protocols.md). OpenID Connect, introduceert een derde type token, de ID-token. Elk van deze tokens wordt weergegeven als een *bearer* token.

Een bearer-token is een lichtgewicht beveiligingstoken waarmee het bearer-toegang tot een beveiligde resource wordt verleend. De houder is een partij die het token kan opleveren. Hoewel een partij moet worden geverifieerd met Azure AD voor het ontvangen van het bearer-token als stappen niet genomen worden voor het beveiligen van het token tijdens de overdracht en opslag, kunt u het probleem is onderschept en die worden gebruikt door een onbedoelde partij. Sommige beveiligingstokens hebben een ingebouwd mechanisme om te voorkomen dat onbevoegden kunnen gebruiken, maar bearer-tokens niet. Bearer-tokens moeten in een beveiligd kanaal, zoals transport layer security (HTTPS) worden getransporteerd. Als een bearer-token is verzonden zonder dat dit type beveiliging, een schadelijke partij kan worden gebruikt voor het gebruik van een 'man-in-the-middle-aanval' de token ophalen en deze gebruiken voor niet-geautoriseerde toegang tot een beveiligde bron. Dezelfde beveiligingsprincipes van toepassing wanneer op te slaan of bearer-tokens voor later gebruik opslaan in cache. Altijd voor zorgen dat uw app veilig worden verzonden en bearer-tokens worden opgeslagen. Zie voor meer beveiligingsoverwegingen voor het bearer-tokens, [RFC 6750 sectie 5](http://tools.ietf.org/html/rfc6750).

Veel van de tokens die zijn uitgegeven door het v2.0-eindpunt worden geïmplementeerd als JSON-Webtokens (JWTs). Een JWT is een compacte, URL-veilige manier om informatie overgedragen tussen twee partijen. De informatie in een JWT heet een *claim*. Het is een verklaring van informatie over het bearer en het onderwerp van het token. De claims in een JWT zijn JavaScript Object Notation (JSON)-objecten die zijn gecodeerd en voor de overdracht is geserialiseerd. Omdat de JWTs dat is uitgegeven door het v2.0-eindpunt zijn ondertekend, maar niet versleuteld, kunt u eenvoudig de inhoud van een JWT voor foutopsporing controleren. Zie voor meer informatie over JWTs, de [JWT-specificatie](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

### <a name="id-tokens"></a>ID-tokens
Een ID-token is een vorm van beveiliging voor aanmelden-token dat uw app ontvangt wanneer deze verificatie met behulp van uitvoert [OpenID Connect](active-directory-v2-protocols.md). ID-tokens worden weergegeven als [JWTs](#types-of-tokens), en bevatten claims die u kunt de gebruiker zich aanmeldt bij uw app. U kunt de claims in een ID-token op verschillende manieren gebruiken. Normaal gesproken beheerders ID-tokens gebruiken om gegevens weer te geven of beslissingen over toegangsbeheer in een app. Het v2.0-eindpunt geeft slechts één type ID-token, met een consistente set claims, ongeacht het type van de gebruiker die is aangemeld. De indeling en inhoud van de ID-tokens zijn hetzelfde voor persoonlijke Microsoft-accountgebruikers en werk-of schoolaccount.

ID-tokens zijn op dit moment is ondertekend, maar niet versleuteld. Wanneer uw app een ID-token ontvangt, moet het [valideert de handtekening](#validating-tokens) te bewijzen dat de echtheid van het token en een aantal claims in het token om te bewijzen dat de geldigheid ervan te valideren. De claims die zijn gevalideerd door een app variëren, afhankelijk van de scenariovereisten, maar uw app moet uitvoeren [algemene claim validaties](#validating-tokens) in elk scenario.

We geven u de volledige details over claims in het ID-tokens in de volgende secties, naast een ID-token dat voorbeeld. Houd er rekening mee dat claims in het ID-tokens worden niet geretourneerd in een specifieke volgorde. Nieuwe claims kunnen ook worden opgenomen in de ID-tokens op elk gewenst moment. Uw app moet niet afbreken wanneer nieuwe claims worden geïntroduceerd. De volgende lijst bevat de claims die uw app op dit moment op betrouwbare wijze interpreteren. U vindt meer informatie in de [OpenID Connect-specificatie](http://openid.net/specs/openid-connect-core-1_0.html).

#### <a name="sample-id-token"></a>Voorbeeld-id-token
```
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiI2NzMxZGU3Ni0xNGE2LTQ5YWUtOTdiYy02ZWJhNjkxNDM5MWUiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vYjk0MTk4MTgtMDlhZi00OWMyLWIwYzMtNjUzYWRjMWYzNzZlL3YyLjAiLCJpYXQiOjE0NTIyODUzMzEsIm5iZiI6MTQ1MjI4NTMzMSwiZXhwIjoxNDUyMjg5MjMxLCJuYW1lIjoiQmFiZSBSdXRoIiwibm9uY2UiOiIxMjM0NSIsIm9pZCI6ImExZGJkZGU4LWU0ZjktNDU3MS1hZDkzLTMwNTllMzc1MGQyMyIsInByZWZlcnJlZF91c2VybmFtZSI6InRoZWdyZWF0YmFtYmlub0BueXkub25taWNyb3NvZnQuY29tIiwic3ViIjoiTUY0Zi1nZ1dNRWppMTJLeW5KVU5RWnBoYVVUdkxjUXVnNWpkRjJubDAxUSIsInRpZCI6ImI5NDE5ODE4LTA5YWYtNDljMi1iMGMzLTY1M2FkYzFmMzc2ZSIsInZlciI6IjIuMCJ9.p_rYdrtJ1oCmgDBggNHB9O38KTnLCMGbMDODdirdmZbmJcTHiZDdtTc-hguu3krhbtOsoYM2HJeZM3Wsbp_YcfSKDY--X_NobMNsxbT7bqZHxDnA2jTMyrmt5v2EKUnEeVtSiJXyO3JWUq9R0dO-m4o9_8jGP6zHtR62zLaotTBYHmgeKpZgTFB9WtUq8DVdyMn_HSvQEfz-LWqckbcTwM_9RNKoGRVk38KChVJo4z5LkksYRarDo8QgQ7xEKmYmPvRr_I7gvM2bmlZQds2OeqWLB1NSNbFZqyFOCgYn3bAQ-nEQSKwBaA36jYGPOVG2r2Qv1uKcpSOxzxaQybzYpQ
```

> [!TIP]
> Plak de voorbeeld-ID-token in om te controleren van de claims in het voorbeeld-ID-token, verdient het aanbeveling [jwt.ms](http://jwt.ms/).
>
>

#### <a name="claims-in-id-tokens"></a>Claims in het ID-tokens
| Naam | Claim | Voorbeeldwaarde | Beschrijving |
| --- | --- | --- | --- |
| Doelgroep |`aud` |`6731de76-14a6-49ae-97bc-6eba6914391e` |Hiermee geeft u de beoogde ontvanger van het token. In het ID-tokens is de doelgroep van uw app toepassings-ID, toegewezen aan uw app in de Portal voor Appregistratie Microsoft. Uw app moet deze waarde te valideren en het token te negeren als de waarde komt niet overeen met. |
| Verlener |`iss` |`https://login.microsoftonline.com/b9419818-09af-49c2-b0c3-653adc1f376e/v2.0 ` |Identificeert de beveiligingstokenservice (STS) die wordt gemaakt en retourneert het token en de Azure AD-tenant waarin de gebruiker is geverifieerd. Uw app moet de claim van verlener om ervoor te zorgen dat het token afkomstig van het v2.0-eindpunt is te valideren. Deze moet de GUID-gedeelte van de claim ook gebruiken om het beperken van de set van tenants die kunnen zich aanmelden bij de app. De GUID die wordt aangegeven dat de gebruiker een consument gebruiker vanuit een Microsoft-account is `9188040d-6c67-4c5b-b112-36a304b66dad`. |
| Uitgegeven op |`iat` |`1452285331` |De tijd waarop het token is uitgegeven, weergegeven in epoche-tijd. |
| Verlooptijd |`exp` |`1452289231` |De tijd waarop het token ongeldig is, wordt weergegeven in epoche-tijd. Uw app moet deze claim gebruiken om de geldigheid van de levensduur van tokens. |
| Niet voor |`nbf` |`1452285331` |De tijd waarop het token geldig is, wordt weergegeven in epoche-tijd. Dit is meestal hetzelfde als de uitgifte-tijd. Uw app moet deze claim gebruiken om de geldigheid van de levensduur van tokens. |
| versie |`ver` |`2.0` |De versie van de ID-token, zoals gedefinieerd door Azure AD. De waarde voor het v2.0-eindpunt heeft `2.0`. |
| tenant-id |`tid` |`b9419818-09af-49c2-b0c3-653adc1f376e` |Een GUID die de Azure AD-tenant die door de gebruiker van vertegenwoordigt. De GUID is voor werk- en schoolaccounts accounts, de onveranderbare tenant-ID van de organisatie die de gebruiker behoort. De waarde voor persoonlijke accounts heeft `9188040d-6c67-4c5b-b112-36a304b66dad`. De `profile` bereik is vereist voor het ontvangen van deze claim. |
| Code-hash |`c_hash` |`SGCPtt01wxwfgnYZy2VJtQ` |De hash van de code is opgenomen in de ID-tokens, alleen wanneer de ID-token dat is uitgegeven met een OAuth 2.0-autorisatiecode. Het kan worden gebruikt om te valideren de echtheid van een autorisatiecode. Zie voor meer informatie over het uitvoeren van deze validatie de [OpenID Connect-specificatie](http://openid.net/specs/openid-connect-core-1_0.html). |
| Access token-hash |`at_hash` |`SGCPtt01wxwfgnYZy2VJtQ` |Toegang tot de token-hash is opgenomen in de ID tokens alleen wanneer de ID-token dat is uitgegeven met een OAuth 2.0-toegangstoken. Het kan worden gebruikt om te valideren de echtheid van een toegangstoken. Zie voor meer informatie over het uitvoeren van deze validatie de [OpenID Connect-specificatie](http://openid.net/specs/openid-connect-core-1_0.html). |
| nonce |`nonce` |`12345` |De nonce is een strategie voor het oplossen van token opnieuw afspelen aanvallen. Uw app een nonce kunt opgeven in een autorisatieaanvraag met behulp van de `nonce` queryparameter. De waarde die u in de aanvraag opgeeft wordt verzonden in het ID-token `nonce` claim, ongewijzigd. Uw app kunt controleren of de waarde op basis van de waarde die deze in de aanvraag, waarmee de sessie van de app aan een specifieke ID-token dat koppelt is opgegeven. Deze validatie tijdens de validatie van tokens proces-ID moet worden uitgevoerd in uw app. |
| naam |`name` |`Babe Ruth` |De van naamclaim biedt een leesbare waarde die aangeeft in het onderwerp van het token. De waarde kan niet worden gegarandeerd uniek te zijn, is het veranderlijke en is ontworpen om alleen worden gebruikt voor weer te geven. De `profile` bereik is vereist voor het ontvangen van deze claim. |
| e-mailen |`email` |`thegreatbambino@nyy.onmicrosoft.com` |Het primaire e-mailadres dat is gekoppeld aan het gebruikersaccount dat, als er een bestaat. De waarde ervan is veranderlijke en na verloop van tijd veranderen. De `email` bereik is vereist voor het ontvangen van deze claim. |
| gewenste gebruikersnaam |`preferred_username` |`thegreatbambino@nyy.onmicrosoft.com` |De primaire gebruikersnaam die de gebruiker in het v2.0-eindpunt vertegenwoordigt. Het is mogelijk een e-mailadres, telefoonnummer of een algemene gebruikersnaam zonder een indeling die is opgegeven. De waarde ervan is veranderlijke en na verloop van tijd veranderen. Omdat dit veranderlijke, moet deze waarde niet worden gebruikt om autorisatie beslissingen te nemen. De `profile` bereik is vereist voor het ontvangen van deze claim. |
| Onderwerp |`sub` |`MF4f-ggWMEji12KynJUNQZphaUTvLcQug5jdF2nl01Q` | De principal waarover het token worden bevestigd met gegevens, zoals de gebruiker van een app. Deze waarde is onveranderbaar en kan niet worden toegewezen of opnieuw gebruikt. Deze controles uit te voeren autorisatie veilig, zoals wanneer het token wordt gebruikt voor toegang tot een bron kan worden gebruikt en kan worden gebruikt als een sleutel in databasetabellen. Omdat het onderwerp altijd is aanwezig zijn in de tokens dat problemen met Azure AD, wordt u aangeraden deze waarde in een autorisatiesysteem voor algemeen gebruik. Het onderwerp is echter een pairwise id - het is uniek is voor een bepaalde toepassing-ID. Als een enkele gebruiker zich in twee verschillende apps met behulp van de twee andere client-id's, krijgen die apps dus twee verschillende waarden voor de claim onderwerp. Dit kan wel of niet kan worden gewenst is afhankelijk van uw architectuur en privacy-vereisten. |
| Object-ID |`oid` |`a1dbdde8-e4f9-4571-ad93-3059e3750d23` | De onveranderbare id voor een object in het Microsoft-identiteitssysteem, in dit geval een gebruikersaccount. Het kan ook worden gebruikt voor het uitvoeren van controles autorisatie veilig en als een sleutel in databasetabellen. Deze ID is uniek voor de gebruiker voor toepassingen: twee verschillende toepassingen die zich in dezelfde gebruiker ontvangt de dezelfde waarde in de `oid` claim. Dit betekent dat deze kan worden gebruikt bij het maken van query's bij Microsoft online services, zoals de Microsoft Graph. De Microsoft Graph retourneert deze ID als de `id` eigenschap voor een bepaalde gebruikersaccount. Omdat de `oid` kunnen meerdere apps correleren van gebruikers, de `profile` bereik is vereist voor het ontvangen van deze claim. Houd er rekening mee dat als een enkele gebruiker in meerdere tenants bestaat, de gebruiker een ander object-ID in elke tenant bevat: deze worden beschouwd als andere accounts, zelfs als de gebruiker meldt zich aan bij elk account met dezelfde referenties. |

### <a name="access-tokens"></a>Toegangstokens

Het v2.0-eindpunt kunt apps van derden die zijn geregistreerd bij Azure AD om uit te geven van de toegangstokens voor beveiligde resources, zoals Web-API's. Zie voor meer informatie over het instellen van een toepassing om uit te geven toegangstokens [over het registreren van een app met het v2.0-eindpunt](quickstart-v2-register-an-app.md). Bij het registreren van de toepassing met het v2.0-eindpunt, de ontwikkelaar toegangsniveaus, met de naam kunt opgeven **scopes**, voor welke access tokens kunnen worden verleend. Bijvoorbeeld, de **calendars.read** bereik dat is gedefinieerd in de Microsoft Graph API verleent machtiging voor het lezen van de agenda van de gebruiker. Wanneer uw toepassing een toegangstoken van het v2.0-eindpunt ontvangt, moet u de handtekening van het token, uitgever, doelgroep, verlooptijd en eventuele andere claims valideren, afhankelijk van uw scenario. 

Wanneer u een toegangstoken van het v2.0-eindpunt aanvraagt, retourneert het v2.0-eindpunt ook metagegevens over het toegangstoken voor uw app te gebruiken. Deze informatie omvat de verlooptijd van het toegangstoken en de bereiken waarvoor deze geldig is. Uw app gebruikmaakt van deze metagegevens intelligente cachebewerkingen uitvoeren van toegangstokens zonder te parseren, open het toegangstoken zelf.

### <a name="refresh-tokens"></a>Vernieuwen van tokens
Vernieuwen van tokens zijn beveiligingstokens die uw app gebruiken kunt om nieuwe toegangstokens in een stroom voor OAuth 2.0. Uw app kan vernieuwen van tokens gebruiken om te bereiken op de lange termijn toegang tot resources namens een gebruiker zonder interactie met de gebruiker.

Vernieuwen van tokens zijn meerdere resource. Een vernieuwingstoken dat is ontvangen tijdens een tokenaanvraag voor één resource kan worden ingewisseld voor tokens voor toegang tot een geheel andere resource.

Als u wilt vernieuwen in een token antwoord ontvangen, uw app moet aanvragen en worden verleend de `offline_access` bereik. Voor meer informatie over de `offline_access` scope, raadpleegt u de [toestemming en scopes](v2-permissions-and-consent.md) artikel.

Vernieuwen van tokens, en altijd zullen zijn, helemaal ondoorzichtig aan uw app. Ze kunnen zijn uitgegeven door de Azure AD v2.0-eindpunt en alleen worden geïnspecteerd en geïnterpreteerd door het v2.0-eindpunt. Ze zijn lange levensduur hebben, maar uw app moet niet worden geschreven om te verwachten dat een vernieuwingstoken voor een bepaalde periode duurt. Vernieuwen van tokens is ongeldig op elk moment om diverse redenen - voor meer informatie, Zie [token intrekken](v1-id-and-access-tokens.md#token-revocation). De enige manier om uw app om te weten of een vernieuwingstoken geldig is, is om te je deze door een token aan te vragen naar het v2.0-eindpunt.

Wanneer u een vernieuwingstoken voor een nieuw toegangstoken inwisselt (en als uw app is verleend de `offline_access` bereik), ontvangt u een nieuwe vernieuwingstoken in het token antwoord. Sla de zojuist uitgegeven vernieuwingstoken, ter vervanging van de versie die u hebt gebruikt in de aanvraag. Dit zorgt ervoor dat uw vernieuwingstokens geldig voor zo lang mogelijk blijven.

## <a name="validating-tokens"></a>Valideren van tokens
Het token alleen validatie van dat uw apps hoeft uit te voeren is op dit moment ID-tokens te valideren. Voor het valideren van een ID-token, moet het valideren van uw app in zowel de ID-token handtekening en de claims in het ID-token.

<!-- TODO: Link --> Microsoft biedt de bibliotheken en codevoorbeelden die u te laten zien hoe eenvoudig omgaan met validatie van tokens. In de volgende secties beschrijven we het onderliggende proces. Er zijn verschillende open source-bibliotheken van derden beschikbaar voor validatie van JWT. Er is ten minste één tapewisselaar-optie voor bijna elk platform en taal.

### <a name="validate-the-signature"></a>Valideert de handtekening
Een JWT bevat drie segmenten die worden gescheiden door het `.` teken. Het eerste segment wordt ook wel de *header*, het tweede segment is het *hoofdtekst*, en het derde segment is het *handtekening*. Het segment handtekening kan worden gebruikt voor het valideren van de authenticiteit van de ID-token, zodat deze kan worden vertrouwd door uw app.

ID-tokens zijn ondertekend met behulp van asymmetrische codering van industriestandaard-algoritmen, zoals RSA-256. De koptekst van de ID-token bevat informatie over de sleutel en -versleuteling-methode gebruikt voor het ondertekenen van het token. Bijvoorbeeld:

```
{
  "typ": "JWT",
  "alg": "RS256",
  "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

De `alg` claim geeft aan dat de algoritme die is gebruikt voor het ondertekenen van het token. De `kid` claim geeft aan dat de openbare sleutel die is gebruikt voor het ondertekenen van het token.

ID- en toegangstokens ondertekent het v2.0-eindpunt met behulp van een van een specifieke set paren van openbare en persoonlijke sleutels. De mogelijke set sleutels, draait het v2.0-eindpunt periodiek, zodat uw app voor een correcte verwerking die automatisch moet worden geschreven. Een redelijke frequentie om te controleren op updates voor de openbare sleutels die worden gebruikt door het v2.0-eindpunt is elke 24 uur.

U kunt het ondertekenen van belangrijke gegevens die u nodig hebt voor het valideren van de handtekening met behulp van de OpenID Connect-metagegevensdocument dat zich bevindt in verkrijgen:

```
https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration
```

> [!TIP]
> Probeer de URL in een browser.

Dit metagegevensdocument is een JSON-object dat handig stukjes informatie, zoals de locatie van de verschillende eindpunten vereist voor de OpenID Connect-verificatie is. Het document bevat ook een *jwks_uri*, waardoor de locatie van de set met openbare sleutels die worden gebruikt voor het ondertekenen van tokens. Het JSON-document dat zich bevindt in de jwks_uri heeft alle belangrijke informatie over de openbare die momenteel in gebruik. Uw app kan gebruiken de `kid` claim in de JWT-header te selecteren welke openbare sleutel in dit document is gebruikt voor het ondertekenen van een token. Vervolgens wordt er handtekeningvalidatie uitgevoerd met behulp van de juiste openbare sleutel en het opgegeven algoritme.

> [!NOTE]
> De `x5t` claim is afgeschaft in het v2.0-eindpunt. Wordt u aangeraden de `kid` claim uw token te valideren.

Het valideren van de handtekening is buiten het bereik van dit document. Veel open source-bibliotheken zijn beschikbaar voor u hierbij helpen.

### <a name="validate-the-claims"></a>Valideren van de claims
Wanneer uw app een ID-token bij het aanmelden van gebruikers ontvangt, moet deze ook een aantal controles op basis van de claims in het ID-token uitvoeren. Deze omvatten, maar zijn niet beperkt tot:

* **doelgroep** claim, om te controleren dat de ID-token dat is bedoeld om u te krijgen tot uw app
* **niet voor** en **verlooptijd** claims, om te controleren of de ID-token is niet verlopen
* **verlener** claim, om te controleren of het token aan uw app is uitgegeven door het v2.0-eindpunt
* **nonce**, als een token opnieuw afspelen netwerkaanval

Zie voor een volledige lijst van de claim validaties die uw app moet worden uitgevoerd, de [OpenID Connect-specificatie](http://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation).

Details van de verwachte waarden voor deze claims worden opgenomen in de [ID-tokens](# ID tokens) sectie.

## <a name="token-lifetimes"></a>Levensduur van tokens
We bieden de volgende levensduur van tokens voor uitsluitend ter informatie. De informatie kan u helpen bij het ontwikkelen en fouten opsporen in apps. Uw apps moeten niet worden geschreven u kunt verwachten van een van deze levensduur constant blijft. Kan er een levensduur van token en op elk gewenst moment worden gewijzigd.

| Token | Levensduur | Beschrijving |
| --- | --- | --- |
| ID-tokens (werk of school-accounts) |1 uur |ID-tokens zijn doorgaans geldig 1 uur. Uw web-app deze dezelfde levensduur kunt gebruiken voor het onderhouden van een eigen sessie met de gebruiker (aanbevolen) kunt, maar u de levensduur van een geheel andere sessie. Als uw app een nieuwe ID-token ophalen moet, moet u een nieuwe aanmeldingsaanvraag naar het v2.0 eindpunt voor autorisatie. Als de gebruiker een ongeldig browsersessie met het v2.0-eindpunt heeft, kan de gebruiker niet opnieuw in te voeren hun referenties worden vereist. |
| ID-tokens (persoonlijke accounts) |24 uur |ID-tokens voor persoonlijke accounts zijn doorgaans geldig gedurende 24 uur. Uw web-app deze dezelfde levensduur kunt gebruiken voor het onderhouden van een eigen sessie met de gebruiker (aanbevolen) kunt, maar u de levensduur van een geheel andere sessie. Als uw app een nieuwe ID-token ophalen moet, moet u een nieuwe aanmeldingsaanvraag naar het v2.0 eindpunt voor autorisatie. Als de gebruiker een ongeldig browsersessie met het v2.0-eindpunt heeft, kan de gebruiker niet opnieuw in te voeren hun referenties worden vereist. |
| Toegangstokens (werk of school-accounts) |1 uur |Vermeld in het token antwoorden als onderdeel van de metagegevens van de token. |
| Toegangstokens (persoonlijke accounts) |1 uur |Vermeld in het token antwoorden als onderdeel van de metagegevens van de token. Toegangstokens die zijn uitgegeven namens persoonlijke accounts kunnen worden geconfigureerd voor de levensduur van een andere, maar 1 uur is normaal. |
| Vernieuwen van tokens (werk of school-account) |Tot 14 dagen |Een enkele vernieuwingstoken is geldig voor maximaal 14 dagen. Het vernieuwingstoken dat kan worden in ongeldige op elk gewenst moment om verschillende redenen, zodat uw app proberen blijven moet te gebruiken een vernieuwingstoken totdat deze is mislukt, of uw app wordt vervangen door een nieuwe vernieuwingstoken. Er wordt een vernieuwingstoken ongeldig als het is 90 dagen geleden hun referenties door de gebruiker heeft ingevoerd. |
| Vernieuwen van tokens (persoonlijke accounts) |Tot 1 jaar |Een enkele vernieuwingstoken is geldig voor een maximum van 1 jaar. Het vernieuwingstoken dat kan worden in ongeldige op elk gewenst moment om verschillende redenen, zodat uw app proberen een vernieuwingstoken gebruiken blijven moet totdat deze is mislukt. |
| Autorisatiecodes (werk of school-accounts) |10 minuten |Verificatiecodes zijn opzettelijk analyserapporten en onmiddellijk moeten worden ingewisseld voor toegangstokens en vernieuwingstokens wanneer de tokens worden ontvangen. |
| Autorisatiecodes (persoonlijke accounts) |5 minuten |Verificatiecodes zijn opzettelijk analyserapporten en onmiddellijk moeten worden ingewisseld voor toegangstokens en vernieuwingstokens wanneer de tokens worden ontvangen. Er worden autorisatiecodes die namens persoonlijke accounts worden uitgegeven voor eenmalig gebruik. |
