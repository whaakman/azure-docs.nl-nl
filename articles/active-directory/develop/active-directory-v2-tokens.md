---
title: Azure Active Directory-v2.0 tokens verwijzing | Microsoft Docs
description: De typen tokens en claims die door het Azure AD v2.0-eindpunt
services: active-directory
documentationcenter: 
author: dstrockis
manager: mtillman
editor: 
ms.assetid: dc58c282-9684-4b38-b151-f3e079f034fd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 01994e067bd7ce0343f12ec3334a91bd062251a8
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-v20-tokens-reference"></a>Overzicht van Azure Active Directory v2.0-tokens
Het v2.0-eindpunt voor Azure Active Directory (Azure AD) verzendt verschillende typen beveiligingstokens in elk [authenticatiestroom](active-directory-v2-flows.md). Deze verwijzing beschrijft de indeling, de beveiligingskenmerken en de inhoud van elk type token.

> [!NOTE]
> Het v2.0-eindpunt biedt geen ondersteuning voor alle Azure Active Directory-scenario's en onderdelen. Meer informatie over om te bepalen of het v2.0-eindpunt moet worden gebruikt, [v2.0 beperkingen](active-directory-v2-limitations.md).
>
>

## <a name="types-of-tokens"></a>Typen tokens
Het v2.0-eindpunt ondersteunt de [OAuth 2.0-protocol voor autorisatie](active-directory-v2-protocols.md), welke maakt gebruik van tokens en vernieuw tokens. Het v2.0-eindpunt biedt ook ondersteuning voor verificatie en meld u via [OpenID Connect](active-directory-v2-protocols.md). OpenID Connect introduceert een derde type token, het token ID. Elk van deze tokens wordt weergegeven als een *bearer* token.

Een bearer-token is een lichtgewicht beveiligingstoken die de bearer toegang tot een beveiligde bron verleent. De houder is een partij die het token kan opleveren. Hoewel een partij moet worden geverifieerd met Azure AD voor het ontvangen van de bearer-token als stappen niet genomen worden voor het beveiligen van het token tijdens de overdracht en opslag, kunt u het probleem is onderschept en gebruikt door een onbedoelde partij. Sommige beveiligingstokens hebben een ingebouwde mechanisme om te voorkomen dat onbevoegden kunnen gebruiken, maar bearer-tokens niet. Bearer-tokens moeten in een beveiligd kanaal zoals transport layer security (HTTPS) worden getransporteerd. Als een bearer-token is verzonden zonder dat dit type beveiliging, een schadelijke party gebruiken een 'man-in-the-middle-aanval' aan het token verkrijgen en deze gebruiken voor toegang door onbevoegden tot een beveiligde bron. De dezelfde beveiligings-principals van toepassing wanneer caching bearer-tokens voor later gebruik of op te slaan. Altijd voor zorgen dat uw app veilig worden verzonden en bearer-tokens slaat. Zie voor meer beveiligingsoverwegingen voor bearer-tokens, [RFC 6750 sectie 5](http://tools.ietf.org/html/rfc6750).

Veel van de tokens die zijn uitgegeven door het v2.0-eindpunt worden geïmplementeerd als JSON Web Tokens (JWTs). Een JWT is een compacte, URL-veilige manier informatie over te brengen tussen twee partijen. De informatie in een JWT wordt aangeroepen een *claim*. Het is een bewering van informatie over de bearer en het onderwerp van het token. De claims in een JWT zijn JSON JavaScript Object Notation ()-objecten die zijn gecodeerd en geserialiseerd voor verzending. Omdat de JWTs uitgegeven door het v2.0-eindpunt zijn ondertekend, maar niet versleuteld, controleert u eenvoudig de inhoud van een JWT voor foutopsporing. Zie voor meer informatie over JWTs de [JWT-specificatie](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

### <a name="id-tokens"></a>ID-tokens
Een token ID is een vorm van aanmelden beveiligingstoken dat uw app ontvangt wanneer wordt verificatie uitgevoerd via [OpenID Connect](active-directory-v2-protocols.md). ID-tokens worden weergegeven als [JWTs](#types-of-tokens), en bevatten claims die u gebruiken kunt voor de gebruiker zich aanmeldt bij uw app. U kunt de claims in een token ID op verschillende manieren gebruiken. Gewoonlijk admins ID-tokens gebruiken om gegevens van het account weer te geven of om access control beslissingen te nemen in een app. Het v2.0-eindpunt problemen die slechts één type token ID, waarvoor een consistente set claims, ongeacht het type van de gebruiker die is aangemeld. De indeling en de inhoud van het ID-tokens zijn hetzelfde voor persoonlijke Microsoft-accountgebruikers en voor werk of school-accounts.

ID-tokens worden op dit moment is ondertekend, maar niet versleuteld. Wanneer uw app een token ID ontvangt, moet deze [valideert de handtekening](#validating-tokens) te bewijzen dat de echtheid van het token en enkele claims in het token om te bewijzen van de geldigheid te valideren. De claims gevalideerd door een app, is afhankelijk van de scenariovereisten voor, maar uw app moet uitvoeren sommige [algemene claim validaties](#validating-tokens) in elk scenario.

We bieden u de volledige details over claims in de ID-tokens in de volgende secties, naast een voorbeeldtoken-ID. Houd er rekening mee dat de claims in de ID-tokens niet in een bepaalde volgorde worden geretourneerd. Nieuwe claims kunnen ook worden opgenomen in de ID-tokens op elk gewenst moment. Uw app beter niet verbreken wanneer nieuwe claims zijn geïntroduceerd. De volgende lijst bevat de claims die uw app momenteel betrouwbaar herkent. U vindt meer informatie in de [specificatie van het OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html).

#### <a name="sample-id-token"></a>Voorbeeldtoken-ID
```
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiI2NzMxZGU3Ni0xNGE2LTQ5YWUtOTdiYy02ZWJhNjkxNDM5MWUiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vYjk0MTk4MTgtMDlhZi00OWMyLWIwYzMtNjUzYWRjMWYzNzZlL3YyLjAiLCJpYXQiOjE0NTIyODUzMzEsIm5iZiI6MTQ1MjI4NTMzMSwiZXhwIjoxNDUyMjg5MjMxLCJuYW1lIjoiQmFiZSBSdXRoIiwibm9uY2UiOiIxMjM0NSIsIm9pZCI6ImExZGJkZGU4LWU0ZjktNDU3MS1hZDkzLTMwNTllMzc1MGQyMyIsInByZWZlcnJlZF91c2VybmFtZSI6InRoZWdyZWF0YmFtYmlub0BueXkub25taWNyb3NvZnQuY29tIiwic3ViIjoiTUY0Zi1nZ1dNRWppMTJLeW5KVU5RWnBoYVVUdkxjUXVnNWpkRjJubDAxUSIsInRpZCI6ImI5NDE5ODE4LTA5YWYtNDljMi1iMGMzLTY1M2FkYzFmMzc2ZSIsInZlciI6IjIuMCJ9.p_rYdrtJ1oCmgDBggNHB9O38KTnLCMGbMDODdirdmZbmJcTHiZDdtTc-hguu3krhbtOsoYM2HJeZM3Wsbp_YcfSKDY--X_NobMNsxbT7bqZHxDnA2jTMyrmt5v2EKUnEeVtSiJXyO3JWUq9R0dO-m4o9_8jGP6zHtR62zLaotTBYHmgeKpZgTFB9WtUq8DVdyMn_HSvQEfz-LWqckbcTwM_9RNKoGRVk38KChVJo4z5LkksYRarDo8QgQ7xEKmYmPvRr_I7gvM2bmlZQds2OeqWLB1NSNbFZqyFOCgYn3bAQ-nEQSKwBaA36jYGPOVG2r2Qv1uKcpSOxzxaQybzYpQ
```

> [!TIP]
> Plak de voorbeeldtoken-ID in om te controleren van de claims in het voorbeeld-ID-token, verdient het aanbeveling [calebb.net](http://calebb.net/).
>
>

#### <a name="claims-in-id-tokens"></a>Claims in de ID-tokens
| Naam | Claim | Voorbeeldwaarde | Beschrijving |
| --- | --- | --- | --- |
| doelgroep |`aud` |`6731de76-14a6-49ae-97bc-6eba6914391e` |Identificeert de beoogde ontvanger van het token. In de ID-tokens is de doelgroep van uw app toepassings-ID, toegewezen aan uw app in de Registratieportal voor Microsoft-toepassing. Uw app moet deze waarde niet valideren en weigeren van het token als de waarde komt niet overeen met. |
| certificaatverlener |`iss` |`https://login.microsoftonline.com/b9419818-09af-49c2-b0c3-653adc1f376e/v2.0 ` |Identificeert de beveiligingstokenservice (STS) die wordt gemaakt en retourneert het token en de Azure AD-tenant waarin de gebruiker werd geverifieerd. Uw app moet de claim verlener om ervoor te zorgen dat het token afkomstig zijn van het v2.0-eindpunt te valideren. Het moet ook de GUID-gedeelte van de claim en Beperk het aantal tenants die bij de app aanmelden zich gebruiken. De GUID die aangeeft dat de gebruiker een consumer gebruiker uit een Microsoft-account is `9188040d-6c67-4c5b-b112-36a304b66dad`. |
| verleend aan |`iat` |`1452285331` |De tijd waarop het token is uitgegeven, weergegeven in de epoche tijd. |
| verlooptijd |`exp` |`1452289231` |De tijd waarop het token ongeldig is, weergegeven in de epoche tijd. Uw app moet deze claim gebruiken om de geldigheid van de levensduur van tokens. |
| niet voor |`nbf` |`1452285331` |Het tijdstip waarop het token geldig is, wordt weergegeven in de epoche tijd. Het is doorgaans hetzelfde zijn als de uitgifte-tijd. Uw app moet deze claim gebruiken om de geldigheid van de levensduur van tokens. |
| versie |`ver` |`2.0` |De versie van het token ID, zoals gedefinieerd door Azure AD. De waarde voor het v2.0-eindpunt heeft `2.0`. |
| Tenant-ID |`tid` |`b9419818-09af-49c2-b0c3-653adc1f376e` |Een GUID die de Azure AD-tenant waarvan de gebruiker van vertegenwoordigt. De GUID is voor werk- en schoolaccounts-accounts, niet-wijzigbaar tenant-ID van de organisatie die de gebruiker behoort. De waarde voor persoonlijke accounts heeft `9188040d-6c67-4c5b-b112-36a304b66dad`. De `profile` bereik is vereist voor deze claim wordt ontvangen. |
| code-hash |`c_hash` |`SGCPtt01wxwfgnYZy2VJtQ` |De hash van de code is opgenomen in de ID-tokens, alleen wanneer het token ID is uitgegeven door een OAuth 2.0-autorisatie-code. Het kan worden gebruikt om de echtheid van een autorisatiecode te valideren. Zie voor meer informatie over het uitvoeren van deze validatie de [specificatie van het OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html). |
| token hash toegang |`at_hash` |`SGCPtt01wxwfgnYZy2VJtQ` |De toegang token hash is opgenomen in de ID tokens alleen wanneer het token ID uitgegeven met een OAuth 2.0-toegangstoken. Het kan worden gebruikt om de echtheid van een toegangstoken te valideren. Zie voor meer informatie over het uitvoeren van deze validatie de [specificatie van het OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html). |
| nonce |`nonce` |`12345` |De nonce is een strategie voor beperkende token replay-aanvallen. Uw app een nonce kunt opgeven in een aanvraag voor verificatie met behulp van de `nonce` queryparameter. De waarde die u in de aanvraag opgeeft is verzonden in het ID-token `nonce` claim, ongewijzigd. Uw app kunt controleren of de waarde met de waarde die het opgegeven op de aanvraag die sessie van de app aan een specifieke ID-token koppelt. Uw app moet deze validatie tijdens de validatie van token ID uitvoeren. |
| naam |`name` |`Babe Ruth` |De van naamclaim biedt een leesbare waarde die aangeeft van het onderwerp van het token. De waarde kan niet worden gegarandeerd uniek zijn en veranderlijke is ontworpen om te worden gebruikt alleen ter informatie weergegeven. De `profile` bereik is vereist voor deze claim wordt ontvangen. |
| e-mailen |`email` |`thegreatbambino@nyy.onmicrosoft.com` |Het primaire e-mailadres dat is gekoppeld aan het gebruikersaccount, indien aanwezig. De waarde ervan is veranderlijke en na verloop van tijd kan wijzigen. De `email` bereik is vereist voor deze claim wordt ontvangen. |
| voorkeur gebruikersnaam |`preferred_username` |`thegreatbambino@nyy.onmicrosoft.com` |De primaire gebruikersnaam die de gebruiker in het v2.0-eindpunt vertegenwoordigt. Het is mogelijk een e-mailadres, telefoonnummer of een algemene gebruikersnaam zonder een specifieke indeling. De waarde ervan is veranderlijke en na verloop van tijd kan wijzigen. Aangezien het veranderlijke, moet deze waarde niet worden gebruikt om autorisatiebeslissingen te nemen. De `profile` bereik is vereist voor deze claim wordt ontvangen. |
| Onderwerp |`sub` |`MF4f-ggWMEji12KynJUNQZphaUTvLcQug5jdF2nl01Q` | De principal waarover het token asserts informatie, zoals de gebruiker van een app. Deze waarde is onveranderbaar en kan niet worden toegewezen of opnieuw gebruikt. Deze controles uit te voeren autorisatie veilig, zoals wanneer het token wordt gebruikt voor toegang tot een bron kan worden gebruikt en kan worden gebruikt als een sleutel in databasetabellen. Omdat het onderwerp altijd aanwezig zijn op de tokens dat problemen met Azure AD, wordt u aangeraden deze waarde in een algemeen autorisatiesysteem. Het onderwerp is echter een pairwise id - deze uniek is voor een bepaalde toepassing-ID.  Daarom als één gebruiker zich bij twee verschillende apps met behulp van twee andere client-ID aanmeldt, ontvangt deze apps twee verschillende waarden voor de claim onderwerp.  Dit kan of kan niet worden gewenst afhankelijk van uw architectuur en privacy-vereisten. |
| object-ID |`oid` |`a1dbdde8-e4f9-4571-ad93-3059e3750d23` | De onveranderbare id voor een object in de Microsoft identity-systeem, in dit geval een gebruikersaccount.  Het kan ook worden gebruikt voor het uitvoeren van autorisatie controles veilig en als een sleutel in databasetabellen. Deze ID is uniek voor de gebruiker alle toepassingen - twee verschillende toepassingen dezelfde gebruiker aanmelden ontvangt dezelfde waarde in de `oid` claim.  Dit betekent dat deze kan worden gebruikt bij het maken van query's bij Microsoft online services, zoals Microsoft Graph.  Microsoft Graph retourneert deze ID als de `id` eigenschap voor een bepaald gebruikersaccount.  Omdat de `oid` meerdere apps met elkaar correleren gebruikers, kan de `profile` bereik is vereist voor deze claim wordt ontvangen. Houd er rekening mee dat als een enkele gebruiker in meerdere tenants bestaat, de gebruiker een ander object-ID in elke tenant bevat-deze worden beschouwd als andere accounts, zelfs als de gebruiker zich bij elk account met dezelfde aanmeldingsgegevens aanmeldt. |

### <a name="access-tokens"></a>Toegangstokens
Toegangstokens die zijn uitgegeven door het v2.0-eindpunt kunnen op dit moment wordt alleen door Microsoft-Services worden verbruikt. Uw apps nodig niet voor het uitvoeren van de validatie of inspectie van toegangstokens voor een van de momenteel ondersteunde scenario's. U kunt toegangstokens behandelen als een mat. Ze zijn alleen tekenreeksen die uw app naar Microsoft in HTTP-aanvragen doorgeven kunt.

In de nabije toekomst vindt het v2.0-eindpunt de mogelijkheid voor uw app toegangstokens ontvangen van andere clients. Op dat moment wordt de informatie in dit naslagonderwerp worden bijgewerkt met de informatie die u nodig hebt voor uw app validatie van servertoegang op token en andere vergelijkbare taken uit te voeren.

Wanneer u een toegangstoken van het v2.0-eindpunt aanvraagt, retourneert het v2.0-eindpunt ook metagegevens over het toegangstoken voor uw app te gebruiken. Deze informatie omvat de verlooptijd van het toegangstoken en de bereiken waarvoor geldig is. Uw app gebruikmaakt van deze metagegevens uitvoeren Intelligente caching van toegangstokens zonder parseren open het toegangstoken zelf.

### <a name="refresh-tokens"></a>Vernieuwen van tokens
Vernieuwen van tokens zijn beveiligingstokens die uw app gebruiken kunt om nieuwe toegangstokens ophalen in een OAuth 2.0-stroom. Uw app kunt vernieuwen van tokens gebruiken als u de lange termijn toegang tot bronnen namens een gebruiker zonder interactie met de gebruiker.

Vernieuwen van tokens zijn meerdere resource. Een vernieuwingstoken dat is ontvangen tijdens een tokenaanvraag voor één resource kan worden ingewisseld voor toegangstokens tot een andere resource.

Als u wilt vernieuwen in een token antwoord ontvangen, uw app moet aanvragen en worden verleend de `offline_acesss` bereik. Voor meer informatie over de `offline_access` bereik, raadpleegt u de [toestemming en scopes](active-directory-v2-scopes.md) artikel.

Vernieuwen van tokens, en altijd zullen zijn, mat aan uw app. Ze kunnen zijn uitgegeven door het Azure AD v2.0-eindpunt en alleen worden gecontroleerd en geïnterpreteerd door het v2.0-eindpunt. Ze zijn lange levensduur hebben, maar niet uw app kunt verwachten dat een vernieuwingstoken voor een periode duurt worden geschreven. Vernieuwen van tokens kunnen op elk moment om verschillende redenen zijn ongeldig. De enige manier om uw app weten of een vernieuwingstoken geldig is, is als u wilt gebruikmaken van het door het maken van een tokenaanvraag door naar het v2.0-eindpunt.

Wanneer u een vernieuwingstoken voor een nieuw toegangstoken inwisselen (en als uw app had gekregen de `offline_access` bereik), ontvangt u een nieuw vernieuwingstoken in het token antwoord. Sla het van nieuwe uitgegeven vernieuwingstoken, ter vervanging van het abonnement dat u in de aanvraag gebruikt. Dit zorgt ervoor dat het vernieuwen van tokens geldig voor zo lang mogelijk blijven.

## <a name="validating-tokens"></a>Valideren van tokens
De validatie van het type alleen tokens die uw apps hoeft moeten uit te voeren is op dit moment ID-tokens te valideren. Voor het valideren van een token ID, moet uw app in zowel de handtekening van het token ID als de claims in het token ID valideren.

<!-- TODO: Link -->
Microsoft biedt bibliotheken en codevoorbeelden ziet u hoe eenvoudig verwerken validatie van tokens. In de volgende secties beschrijven we het onderliggende proces. Er zijn verschillende van derden open source-bibliotheken beschikbaar voor de validatie van JWT. Er is ten minste één tapewisselaar optie voor bijna elk platform- en taalinstellingen.

### <a name="validate-the-signature"></a>Valideert de handtekening
Een JWT bevat drie segmenten die worden gescheiden door het `.` teken. Het eerste segment wordt ook wel de *header*, het tweede segment is het *hoofdtekst*, en het derde segment is het *handtekening*. Het segment handtekening kan worden gebruikt voor het valideren van de echtheid van het token ID, zodat deze kan worden vertrouwd door uw app.

ID-tokens zijn ondertekend met behulp van industriestandaard asymmetrische algoritmen, zoals RSA 256. De kop van het token ID heeft informatie over de sleutel en versleuteling methode gebruikt voor het ondertekenen van het token. Bijvoorbeeld:

```
{
  "typ": "JWT",
  "alg": "RS256",
  "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

De `alg` claim geeft aan dat de algoritme die is gebruikt voor het ondertekenen van het token. De `kid` claim geeft aan dat de openbare sleutel die is gebruikt voor het ondertekenen van het token.

Op elk gewenst moment kan het v2.0-eindpunt een token ID ondertekenen met behulp van een van een specifieke set paren van openbare en persoonlijke sleutels. Het v2.0-eindpunt draait periodiek de mogelijke set van sleutels, zodat uw app automatisch afhandelen van de belangrijkste wijzigingen worden geschreven. Een redelijke frequentie om te controleren op updates voor de openbare sleutels die worden gebruikt door het v2.0-eindpunt is elke 24 uur.

U kunt het ondertekenen van belangrijke gegevens die u nodig hebt voor het valideren van de handtekening met behulp van het metagegevensdocument OpenID Connect zich bevindt op verkrijgen:

```
https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration
```

> [!TIP]
> Probeer de URL in een browser.
>
>

Dit metagegevensdocument is een JSON-object dat nuttig stukjes informatie, zoals de locatie van de verschillende eindpunten vereist voor OpenID Connect-verificatie is.  Het document bevat ook een *jwks_uri*, waardoor de locatie van de set van openbare sleutels die worden gebruikt voor het ondertekenen van tokens. Het JSON-document vinden op de jwks_uri heeft alle de openbare sleutelinformatie die momenteel in gebruik is. Uw app kunt gebruiken de `kid` claim in de header JWT te selecteren welke openbare sleutel in dit document is gebruikt voor het ondertekenen van een token. Vervolgens wordt er handtekeningvalidatie uitgevoerd met behulp van de juiste openbare sleutel en het algoritme aangegeven.

Het valideren van de handtekening is buiten het bereik van dit document. Veel open source-bibliotheken zijn beschikbaar voor u hierbij helpen.

### <a name="validate-the-claims"></a>Valideren van de claims
Wanneer uw app een token ID van de gebruiker zich aanmelden ontvangt, moet deze ook een aantal controles op basis van de claims in het token ID uitvoeren. Deze omvatten, maar zijn niet beperkt tot:

* **doelgroep** claim om te controleren of het token ID is bedoeld om te krijgen tot uw app
* **niet voor** en **verlooptijd** claims, om te controleren of het token ID is niet verlopen
* **certificaatverlener** claim om te controleren of het token aan uw app is uitgegeven door het v2.0-eindpunt
* **nonce**, als een token replay-aanvallen risicobeperking

Zie voor een volledige lijst van claim validaties die uw app moet worden uitgevoerd, de [specificatie van het OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation).

Details van de verwachte waarden voor deze claims worden opgenomen in de [ID-tokens](# ID tokens) sectie.

## <a name="token-lifetimes"></a>Levensduur van token
We bieden de volgende token levensduur uitsluitend ter informatie. De informatie die kan u helpen bij het ontwikkelen en foutopsporing van apps. Uw apps worden niet kunnen verwachten van een van deze levensduur ongewijzigd geschreven. Token kan levensduur en op elk gewenst moment wordt gewijzigd.

| Token | Levensduur | Beschrijving |
| --- | --- | --- |
| ID-tokens (werk of school-accounts) |1 uur |ID-tokens zijn meestal geldig 1 uur. Uw web-app kan deze dezelfde levensduur via een eigen sessie bijhouden met de gebruiker (aanbevolen) of u kunt de levensduur van een andere sessie. Als uw app een nieuwe ID-token ophalen moet, moet deze Maak een nieuwe aanmelding aanvraag naar het v2.0 eindpunt autoriseren. Als de gebruiker een ongeldig browsersessie met het v2.0-eindpunt heeft, is de gebruiker mogelijk niet vereist voor het hun referenties opnieuw invoeren. |
| ID-tokens (persoonlijke accounts) |24 uur |ID-tokens voor persoonlijke accounts zijn meestal geldig 24 uur. Uw web-app kan deze dezelfde levensduur via een eigen sessie bijhouden met de gebruiker (aanbevolen) of u kunt de levensduur van een andere sessie. Als uw app een nieuwe ID-token ophalen moet, moet deze Maak een nieuwe aanmelding aanvraag naar het v2.0 eindpunt autoriseren. Als de gebruiker een ongeldig browsersessie met het v2.0-eindpunt heeft, is de gebruiker mogelijk niet vereist voor het hun referenties opnieuw invoeren. |
| Toegangstokens (werk of school-accounts) |1 uur |In token antwoorden aangeduid als onderdeel van de metagegevens van de token. |
| Toegangstokens (persoonlijke accounts) |1 uur |In token antwoorden aangeduid als onderdeel van de metagegevens van de token. Toegangstokens die zijn uitgegeven namens persoonlijke accounts kunnen worden geconfigureerd voor de levensduur van een andere, maar 1 uur is standaard. |
| Vernieuwen van tokens (werk of school-account) |14 dagen |Een enkele vernieuwingstoken is geldig voor een maximum van 14 dagen. Echter, het vernieuwingstoken ongeldig worden op elk gewenst moment om verschillende redenen, zodat uw app proberen blijven moet te gebruiken een vernieuwingstoken totdat deze is mislukt, of uw app wordt vervangen door een nieuwe vernieuwingstoken. Er wordt een vernieuwingstoken ongeldig als het al 90 dagen geleden hun referenties door de gebruiker heeft ingevoerd. |
| Vernieuwen van tokens (persoonlijke accounts) |Maximaal 1 jaar |Een enkele vernieuwingstoken is geldig voor een maximum van 1 jaar. Echter, het vernieuwingstoken ongeldig worden op elk gewenst moment om verschillende redenen, zodat uw app geprobeerd een vernieuwingstoken gebruiken worden moet totdat deze is mislukt. |
| Autorisatiecodes (werk of school-accounts) |10 minuten |Autorisatiecodes zijn opzettelijk tijdelijke en onmiddellijk moeten worden ingewisseld voor toegangstokens en vernieuwen van tokens wanneer de tokens worden ontvangen. |
| Autorisatiecodes (persoonlijke accounts) |5 minuten |Autorisatiecodes zijn opzettelijk tijdelijke en onmiddellijk moeten worden ingewisseld voor toegangstokens en vernieuwen van tokens wanneer de tokens worden ontvangen. Er zijn autorisatiecodes die namens persoonlijke accounts worden verleend voor eenmalig gebruik. |
