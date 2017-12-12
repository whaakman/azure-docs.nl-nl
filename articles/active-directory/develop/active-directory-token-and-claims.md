---
title: Meer informatie over de verschillende token en claimtypen die worden ondersteund door Azure AD | Microsoft Docs
description: Een handleiding voor meer informatie over en evaluatie van de claims in de door Azure Active Directory (AAD) uitgegeven tokens van SAML 2.0 en JSON Web Tokens (JWT)
documentationcenter: na
author: dstrockis
services: active-directory
manager: mtillman
editor: 
ms.assetid: 166aa18e-1746-4c5e-b382-68338af921e2
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 3104b47d7ff8585142674b0ee545012f1e291ddd
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-ad-token-reference"></a>Azure AD-tokenverwijzing
Azure Active Directory (Azure AD) verzendt verschillende typen beveiligingstokens bij de verwerking van elke verificatiestroom. Dit document beschrijft de indeling, de beveiligingskenmerken en de inhoud van elk type token.

## <a name="types-of-tokens"></a>Typen tokens
Azure AD-ondersteunt de [OAuth 2.0-protocol voor autorisatie](active-directory-protocols-oauth-code.md), welke maakt gebruik van zowel access_tokens als refresh_tokens.  Het biedt ook ondersteuning voor verificatie en meld u via [OpenID Connect](active-directory-protocols-openid-connect-code.md), die een derde type token, de id_token introduceert.  Elk van deze tokens wordt weergegeven als 'bearer-token'.

Een bearer-token is een lichtgewicht beveiligingstoken die de 'bearer' toegang tot een beveiligde bron verleent. In dit opzicht is het 'bearer' een partij die het token kan opleveren. Hoewel verificatie met Azure AD is vereist om een bearer-token ontvangen, moeten stappen voor het beveiligen van het token, om te voorkomen dat wordt onderschept door een onbedoelde partij worden genomen. Omdat bearer-tokens niet over een ingebouwde mechanisme om te voorkomen dat onbevoegden kunnen gebruiken, moeten zij worden overgebracht in een beveiligd kanaal zoals transport layer security (HTTPS). Als een bearer-token in het wissen wordt verzonden, kan een man-in de middelste aanval worden gebruikt voor het token verkrijgen en onbevoegde toegang te krijgen tot een beveiligde bron. De dezelfde beveiligings-principals van toepassing wanneer caching bearer-tokens voor later gebruik of op te slaan. Altijd voor zorgen dat uw app verzendt en bearer-tokens worden opgeslagen op een veilige manier. Zie voor meer beveiligingsoverwegingen op bearer-tokens [RFC 6750 sectie 5](http://tools.ietf.org/html/rfc6750).

Veel van de tokens die zijn uitgegeven door Azure AD worden geïmplementeerd als JSON Web Tokens of JWTs.  Een JWT is een compacte, URL-veilige methode voor het overbrengen van gegevens tussen twee partijen.  De informatie in JWTs worden aangeduid als 'claims' of asserties van informatie over de bearer en het onderwerp van het token.  De claims in JWTs zijn JSON-objecten gecodeerd en geserialiseerd voor verzending.  Aangezien de JWTs uitgegeven door Azure AD zijn ondertekend, maar niet versleuteld, controleert u eenvoudig de inhoud van een JWT voor foutopsporing.  Er zijn verschillende hulpprogramma's beschikbaar om te doen, zoals [jwt.ms](https://jwt.ms/). Voor meer informatie over JWTs, u kunt verwijzen naar de [JWT-specificatie](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

## <a name="idtokens"></a>Id_tokens
Id_tokens zijn een vorm van aanmelden beveiligingstoken dat uw app ontvangt bij het uitvoeren van verificatie met behulp van [OpenID Connect](active-directory-protocols-openid-connect-code.md).  Ze worden weergegeven als [JWTs](#types-of-tokens), en bevatten claims die u gebruiken kunt voor het ondertekenen van de gebruiker in uw app.  U kunt de claims kunt gebruiken in een id_token zoals naar eigen inzicht: vaak ze worden gebruikt voor het weergeven van accountgegevens of beslissingen toegang besturingselement in een app.

Id_tokens zijn ondertekend, maar niet op dit moment worden versleuteld.  Wanneer uw app een id_token ontvangt, moet deze [valideert de handtekening](#validating-tokens) te bewijzen dat de echtheid van het token en enkele claims in het token om te bewijzen van de geldigheid te valideren.  De claims gevalideerd door een app, is afhankelijk van de scenariovereisten voor, maar er zijn een aantal [algemene claim validaties](#validating-tokens) die uw app moet worden uitgevoerd in elk scenario.

Zie de volgende sectie voor informatie over id_tokens claims, evenals een id_token voorbeeld.  Houd er rekening mee dat de claims in id_tokens niet in een bepaalde volgorde worden geretourneerd.  Bovendien, nieuwe claims kunnen worden opgenomen in id_tokens op elk punt in tijd: uw app beter niet verbreken wanneer nieuwe claims binnenkomen.  De volgende lijst bevat de claims die uw app kan op betrouwbare wijze op het moment van schrijven van dit interpreteren.  Indien nodig, nog meer details kunt u vinden in de [specificatie van het OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html).

#### <a name="sample-idtoken"></a>Voorbeeld id_token
```
eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83ZmU4MTQ0Ny1kYTU3LTQzODUtYmVjYi02ZGU1N2YyMTQ3N2UvIiwiaWF0IjoxMzg4NDQwODYzLCJuYmYiOjEzODg0NDA4NjMsImV4cCI6MTM4ODQ0NDc2MywidmVyIjoiMS4wIiwidGlkIjoiN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlIiwib2lkIjoiNjgzODlhZTItNjJmYS00YjE4LTkxZmUtNTNkZDEwOWQ3NGY1IiwidXBuIjoiZnJhbmttQGNvbnRvc28uY29tIiwidW5pcXVlX25hbWUiOiJmcmFua21AY29udG9zby5jb20iLCJzdWIiOiJKV3ZZZENXUGhobHBTMVpzZjd5WVV4U2hVd3RVbTV5elBtd18talgzZkhZIiwiZmFtaWx5X25hbWUiOiJNaWxsZXIiLCJnaXZlbl9uYW1lIjoiRnJhbmsifQ.
```

> [!TIP]
> Probeer de claims in het voorbeeld id_token bekijken door te plakken in verdient het aanbeveling [jwt.ms](https://jwt.ms/).
> 
> 

#### <a name="claims-in-idtokens"></a>Claims in id_tokens
> [!div class="mx-codeBreakAll"]
| JWT Claim | Naam | Beschrijving |
| --- | --- | --- |
| `appid` |Toepassings-id |De toepassing die het token wordt gebruikt voor toegang tot een bron te identificeren. De toepassing kan fungeren als zelf of namens een gebruiker. De toepassings-ID vertegenwoordigt doorgaans een application-object, maar het kan ook een service-principal-object vertegenwoordigen in Azure AD. <br><br> **Voorbeeldwaarde JWT**: <br> `"appid":"15CB020F-3984-482A-864D-1D92265E8268"` |
| `aud` |Doelgroep |De beoogde ontvanger van het token. De toepassing die de token ontvangt moet verifiëren dat de waarde van de doelgroep juist is en tot tokens die zijn bedoeld voor een andere doelgroep negeren. <br><br> **Voorbeeldwaarde SAML**: <br> `<AudienceRestriction>`<br>`<Audience>`<br>`https://contoso.com`<br>`</Audience>`<br>`</AudienceRestriction>` <br><br> **Voorbeeldwaarde JWT**: <br> `"aud":"https://contoso.com"` |
| `appidacr` |Application Authentication Context Class Reference |Hiermee wordt aangegeven hoe de client is geverifieerd. Voor een openbare-client heeft de waarde 0 is. Als de client-ID en clientgeheim zijn gebruikt, is de waarde 1. <br><br> **Voorbeeldwaarde JWT**: <br> `"appidacr": "0"` |
| `acr` |Authentication Context Class Reference |Hiermee wordt aangegeven hoe de certificaathouder is geverifieerd, in plaats van de client in de Application Authentication Context Class Reference claim. Een waarde van '0' geeft aan dat de verificatie van de eindgebruiker niet voldeed aan de vereisten van de ISO/IEC 29115. <br><br> **Voorbeeldwaarde JWT**: <br> `"acr": "0"` |
| Verificatie Instant |Registreert de datum en tijd waarop de verificatie heeft plaatsgevonden. <br><br> **Voorbeeldwaarde SAML**: <br> `<AuthnStatement AuthnInstant="2011-12-29T05:35:22.000Z">` | |
| `amr` |Verificatiemethode |Geeft aan hoe het onderwerp van het token is geverifieerd. <br><br> **Voorbeeldwaarde SAML**: <br> `<AuthnContextClassRef>`<br>`http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod/password`<br>`</AuthnContextClassRef>` <br><br> **Voorbeeldwaarde JWT**:`“amr”: ["pwd"]` |
| `given_name` |Voornaam |De eerste biedt of als u ' ' naam van de gebruiker, zoals ingesteld op het gebruikersobject Azure AD. <br><br> **Voorbeeldwaarde SAML**: <br> `<Attribute Name=”http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname”>`<br>`<AttributeValue>Frank<AttributeValue>` <br><br> **Voorbeeldwaarde JWT**: <br> `"given_name": "Frank"` |
| `groups` |Groepen |Biedt de object-id's die de groepslidmaatschappen van de certificaathouder vertegenwoordigen. Deze waarden zijn uniek (Zie de Object-ID) en veilig kunnen worden gebruikt voor het beheren van toegang tot, zoals het afdwingen van de autorisatie voor toegang tot een resource. De groepen die zijn opgenomen in de claim groepen zijn geconfigureerd op basis van per toepassing, via de eigenschap 'groupMembershipClaims' van het toepassingsmanifest. Een waarde van null alle groepen uitsluit, een waarde van 'Beveiligingsgroep' bevat alleen uitgevoerd voor Active Directory-beveiligingsgroep lidmaatschappen en een waarde 'Alle' wordt beveiligingsgroepen en distributielijsten van Office 365. <br><br> **Voorbeeldwaarde SAML**: <br> `<Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/groups">`<br>`<AttributeValue>07dd8a60-bf6d-4e17-8844-230b77145381</AttributeValue>` <br><br> **Voorbeeldwaarde JWT**: <br> `“groups”: ["0e129f5b-6b0a-4944-982d-f776045632af", … ]` |
| `idp` |Id-provider |Registreert de id-provider die het onderwerp van het token wordt geverifieerd. Deze waarde is gelijk aan de waarde van de verlener claim tenzij het gebruikersaccount bevindt zich in een andere tenant dan de verlener. <br><br> **Voorbeeldwaarde SAML**: <br> `<Attribute Name=” http://schemas.microsoft.com/identity/claims/identityprovider”>`<br>`<AttributeValue>https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/<AttributeValue>` <br><br> **Voorbeeldwaarde JWT**: <br> `"idp":”https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/”` |
| `iat` |IssuedAt |Slaat de tijd waarop het token is uitgegeven. Dit wordt vaak gebruikt voor het meten van token versheid. <br><br> **Voorbeeldwaarde SAML**: <br> `<Assertion ID="_d5ec7a9b-8d8f-4b44-8c94-9812612142be" IssueInstant="2014-01-06T20:20:23.085Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">` <br><br> **Voorbeeldwaarde JWT**: <br> `"iat": 1390234181` |
| `iss` |Certificaatverlener |Identificeert de beveiligingstokenservice (STS) die wordt gemaakt en retourneert het token. In de tokens die Azure AD retourneert, wordt de verlener sts.windows.net. De GUID in de claimwaarde van de verlener is de tenant-ID van de Azure AD-directory. De tenant-ID is een niet-wijzigbaar en betrouwbare id van de map. <br><br> **Voorbeeldwaarde SAML**: <br> `<Issuer>https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/</Issuer>` <br><br> **Voorbeeldwaarde JWT**: <br>  `"iss":”https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/”` |
| `family_name` |Achternaam |Biedt de laatste naam, achternaam of familienaam van de gebruiker, zoals gedefinieerd in de Azure AD-gebruiker-object. <br><br> **Voorbeeldwaarde SAML**: <br> `<Attribute Name=” http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname”>`<br>`<AttributeValue>Miller<AttributeValue>` <br><br> **Voorbeeldwaarde JWT**: <br> `"family_name": "Miller"` |
| `unique_name` |Naam |Biedt een menselijke leesbare waarde die aangeeft van het onderwerp van het token. Deze waarde is niet noodzakelijkerwijs uniek zijn binnen een tenant en is ontworpen om te worden gebruikt alleen ter informatie weergegeven. <br><br> **Voorbeeldwaarde SAML**: <br> `<Attribute Name=”http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name”>`<br>`<AttributeValue>frankm@contoso.com<AttributeValue>` <br><br> **Voorbeeldwaarde JWT**: <br> `"unique_name": "frankm@contoso.com"` |
| `oid` |Object-id |Bevat een unieke id van een object in Azure AD. Deze waarde is onveranderbaar en kan niet worden toegewezen of opnieuw gebruikt. De object-ID gebruiken om een object in query's naar Azure AD te identificeren. <br><br> **Voorbeeldwaarde SAML**: <br> `<Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">`<br>`<AttributeValue>528b2ac2-aa9c-45e1-88d4-959b53bc7dd0<AttributeValue>` <br><br> **Voorbeeldwaarde JWT**: <br> `"oid":"528b2ac2-aa9c-45e1-88d4-959b53bc7dd0"` |
| `roles` |Rollen |Hiermee geeft u alle rollen van de toepassing die het onderwerp heeft gekregen zowel direct als indirect via een groepslidmaatschap en kan worden gebruikt voor op rollen gebaseerde toegangsbeheer afdwingen. Toepassingsrollen zijn gedefinieerd op basis van per toepassing via de `appRoles` eigenschap van het toepassingsmanifest. De `value` eigenschap van de toepassingsrol van elke is de waarde die wordt weergegeven in de claim rollen. <br><br> **Voorbeeldwaarde SAML**: <br> `<Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/role">`<br>`<AttributeValue>Admin</AttributeValue>` <br><br> **Voorbeeldwaarde JWT**: <br> `“roles”: ["Admin", … ]` |
| `scp` |Bereik |Hiermee geeft u de imitatie-machtigingen te krijgen tot de clienttoepassing. Standaard de machtiging is `user_impersonation`. De eigenaar van de beveiligde bron kunt u aanvullende waarden registreren in Azure AD. <br><br> **Voorbeeldwaarde JWT**: <br> `"scp": "user_impersonation"` |
| `sub` |Onderwerp |Identificeert de principal waarover het token asserts informatie, zoals de gebruiker van een toepassing. Deze waarde is onveranderbaar en kan niet opnieuw worden toegewezen of opnieuw worden gebruikt, dus kan worden gebruikt controles uit te voeren autorisatie veilig. Omdat het onderwerp altijd aanwezig in de tokens problemen met de Azure AD is, raden wij u deze waarde in een algemeen autorisatie-systeem. <br> `SubjectConfirmation`is niet een claim. Hierin wordt beschreven hoe het onderwerp van het token is gecontroleerd. `Bearer`Hiermee wordt aangegeven dat het onderwerp wordt bevestigd door hun bezit van het token. <br><br> **Voorbeeldwaarde SAML**: <br> `<Subject>`<br>`<NameID>S40rgb3XjhFTv6EQTETkEzcgVmToHKRkZUIsJlmLdVc</NameID>`<br>`<SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />`<br>`</Subject>` <br><br> **Voorbeeldwaarde JWT**: <br> `"sub":"92d0312b-26b9-4887-a338-7b00fb3c5eab"` |
| `tid` |Tenant-id |Een niet-wijzigbaar, herbruikbare id waarmee de directory-tenant die het token heeft uitgegeven. U kunt deze waarde gebruiken voor toegang tot directoryresources in een multitenant-toepassing tenantspecifieke. U kunt deze waarde bijvoorbeeld gebruiken voor het identificeren van de tenant in een aanroep van de Graph API. <br><br> **Voorbeeldwaarde SAML**: <br> `<Attribute Name=”http://schemas.microsoft.com/identity/claims/tenantid”>`<br>`<AttributeValue>cbb1a5ac-f33b-45fa-9bf5-f37db0fed422<AttributeValue>` <br><br> **Voorbeeldwaarde JWT**: <br> `"tid":"cbb1a5ac-f33b-45fa-9bf5-f37db0fed422"` |
| `nbf`, `exp` |Levensduur van token |Definieert het tijdsinterval waarin een token geldig is. De service die het token valideert dient te verifiëren dat de huidige datum valt binnen de levensduur van tokens, anders die wordt het token moet afwijzen. De service kan leiden tot vijf minuten buiten het bereik van de levensduur van token ter compensatie van eventuele verschillen in kloktijd ('tijd tijdverschil') tussen Azure AD en de service. <br><br> **Voorbeeldwaarde SAML**: <br> `<Conditions`<br>`NotBefore="2013-03-18T21:32:51.261Z"`<br>`NotOnOrAfter="2013-03-18T22:32:51.261Z"`<br>`>` <br><br> **Voorbeeldwaarde JWT**: <br> `"nbf":1363289634, "exp":1363293234` |
| `upn` |User principal name |Slaat de naam van de user principal.<br><br> **Voorbeeldwaarde JWT**: <br> `"upn": frankm@contoso.com` |
| `ver` |Versie |Slaat het versienummer van het token. <br><br> **Voorbeeldwaarde JWT**: <br> `"ver": "1.0"` |

## <a name="access-tokens"></a>Toegangstokens
Heeft geverifieerd wordt Azure AD een toegangstoken dat kan worden gebruikt voor toegang tot beveiligde bronnen. Het toegangstoken is een base 64 gecodeerde JSON Web Token (JWT) en de inhoud ervan kunnen worden gecontroleerd via een decoder worden uitgevoerd.

Als uw app alleen *gebruikt* toegangstokens om toegang te krijgen tot API's, kunnen (en moeten) behandelen toegangstokens als volledig ondoorzichtig: ze zijn alleen tekenreeksen die uw app aan resources in HTTP-aanvragen doorgeven kan.

Wanneer u een toegangstoken aanvraagt, wordt ook bepaalde metagegevens over het toegangstoken voor het gebruiken van uw app in Azure AD geretourneerd.  Deze informatie omvat de verlooptijd van het toegangstoken en de bereiken waarvoor geldig is.  Dit kan het toegangstoken zelf uw app om uit te voeren Intelligente caching van toegangstokens zonder parseren openen.

Als uw app een API die is beveiligd met Azure AD dat toegangstokens in HTTP-aanvragen is verwacht, moet u uitvoeren validatie en de controle van de tokens die u ontvangt. Validatie van het toegangstoken voordat u deze gebruikt voor toegang tot resources moet worden uitgevoerd in uw app. Zie voor meer informatie over validatie [valideren van Tokens](#validating-tokens).  
Zie voor meer informatie over hoe u kunt dit doen met .NET [beveiligen van een Web-API met behulp van Azure AD-Bearer-tokens](active-directory-devquickstarts-webapi-dotnet.md).

## <a name="refresh-tokens"></a>Vernieuwen van tokens

Vernieuwen van tokens zijn beveiligingstokens die uw app gebruiken kunt om nieuwe in een stroom OAuth 2.0-toegangstokens te verkrijgen.  Kunt u uw app te bereiken op lange termijn toegang tot bronnen namens een gebruiker zonder tussenkomst door de gebruiker.

Vernieuwen van tokens zijn meerdere resource.  Dat wil zeggen worden dat een vernieuwingstoken dat is ontvangen tijdens een tokenaanvraag voor één resource kan ingewisseld voor toegangstokens tot een andere resource. Stel hiervoor de `resource` parameter in de aanvraag voor de betreffende resource.

Vernieuwen van tokens zijn volledig ondoorzichtig aan uw app. Ze zijn lange levensduur hebben, maar niet uw app kunt verwachten dat een vernieuwingstoken voor een periode duurt worden geschreven.  Vernieuwen van tokens kunnen op elk moment in de tijd om een groot aantal redenen zijn ongeldig.  De enige manier om uw app weten of een vernieuwingstoken geldig is, is als u wilt gebruikmaken van het door het maken van een tokenaanvraag naar Azure AD-token-eindpunt.

Wanneer u een vernieuwingstoken voor een nieuw toegangstoken inwisselen, ontvangt u een nieuw vernieuwingstoken in het token antwoord.  U moet het nieuw verleende vernieuwingstoken opslaan vervangen van het abonnement dat u in de aanvraag gebruikt.  Dit wordt gegarandeerd dat het vernieuwen van tokens geldig voor zo lang mogelijk blijven.

## <a name="validating-tokens"></a>Valideren van tokens

Om een id_token of een access_token valideren, moet uw app in de handtekening van het token zowel de claims valideren. Om te valideren toegangstokens, moet uw app ook valideren de uitgever, de doelgroep en de ondertekening tokens. Deze moeten worden gevalideerd met de waarden in het OpenID discovery-document. Bijvoorbeeld: de tenant onafhankelijke versie van het document bevindt zich op [https://login.microsoftonline.com/common/.well-known/openid-configuration](https://login.microsoftonline.com/common/.well-known/openid-configuration). Azure AD-middleware beschikt over ingebouwde mogelijkheden voor het valideren van de toegangstokens en vindt u op onze [voorbeelden](https://docs.microsoft.com/azure/active-directory/active-directory-code-samples) te zoeken in de taal van uw keuze. Zie voor meer informatie over het valideren van een JWT-token expliciet de [handmatige JWT Validatievoorbeeld](https://github.com/Azure-Samples/active-directory-dotnet-webapi-manual-jwt-validation).  

We bieden bibliotheken en codevoorbeelden die laten zien hoe eenvoudig afhandelen token validatie - de onderstaande informatie is alleen beschikbaar voor degenen die het onderliggende proces begrijpt wilt.  Er zijn ook enkele van derden open-source bibliotheken beschikbaar voor de validatie van JWT - er is ten minste één optie voor bijna elk platform en er taal. Zie voor meer informatie over Azure AD-verificatiebibliotheken en codevoorbeelden [Azure AD-verificatiebibliotheken](active-directory-authentication-libraries.md).

#### <a name="validating-the-signature"></a>De handtekening wordt gevalideerd

Een JWT bevat drie segmenten die worden gescheiden door het `.` teken.  Het eerste segment wordt ook wel de **header**, de tweede als de **hoofdtekst**, en de derde als de **handtekening**.  Het segment handtekening kan worden gebruikt voor het valideren van de echtheid van het token, zodat deze kan worden vertrouwd door uw app.

Tokens die zijn uitgegeven door Azure AD bent aangemeld met behulp van de branche standaard versleuteling voor asymmetrische algoritmen, zoals RSA 256. De koptekst van het JWT bevat informatie over de sleutel en versleuteling methode gebruikt voor het ondertekenen van het token:

```
{
  "typ": "JWT",
  "alg": "RS256",
  "x5t": "kriMPdmBvx68skT8-mPAB3BseeA"
}
```

De `alg` claim geeft aan dat de algoritme die is gebruikt voor het ondertekenen van het token, terwijl de `x5t` claim geeft aan dat de bepaalde openbare sleutel die is gebruikt voor het ondertekenen van het token.

Op elk gewenst moment in de tijd, Azure AD Meld u aan een id_token met een van een bepaalde set paren van openbare en persoonlijke sleutels. Azure AD draait de mogelijke reeks sleutels op periodieke basis, zodat uw app automatisch afhandelen van de belangrijkste wijzigingen worden geschreven.  Een redelijke frequentie om te controleren op updates voor de openbare sleutels die worden gebruikt door Azure AD wordt elke 24 uur.

U kunt de ondertekening sleutelgegevens nodig zijn voor het valideren van de handtekening met behulp van het metagegevensdocument OpenID Connect zich bevindt op verkrijgen:

```
https://login.microsoftonline.com/common/.well-known/openid-configuration
```

> [!TIP]
> Probeer deze URL in een browser.
> 
> 

Dit metagegevensdocument is een nuttig stukjes informatie, zoals de locatie van de verschillende eindpunten vereist voor het OpenID Connect verificatie uitvoeren met JSON-object.  

Dit omvat ook een `jwks_uri`, waardoor de locatie van de set van openbare sleutels die worden gebruikt voor het ondertekenen van tokens.  Het JSON-document vinden op de `jwks_uri` bevat alle gegevens van de openbare sleutel in gebruik op dat moment bepaald.  Uw app kunt gebruiken de `kid` claim in de header JWT te selecteren welke openbare sleutel in dit document is gebruikt voor het ondertekenen van een bepaalde token.  Validatie van handtekening met behulp van de juiste openbare sleutel en het opgegeven algoritme kan vervolgens uitvoeren.

Het valideren van de handtekening is buiten het bereik van dit document: Er zijn veel open-source bibliotheken beschikbaar voor u doen indien nodig te helpen.

#### <a name="validating-the-claims"></a>Valideren van de claims

Wanneer uw app een token (een id_token bij het aanmelden van gebruiker of een toegangstoken als een bearer-token in de HTTP-aanvraag ontvangt) moet deze ook een aantal controles op basis van de claims uitvoeren in het token.  Deze omvatten, maar zijn niet beperkt tot:

* De **doelgroep** claim - om te controleren of het token is bedoeld om te krijgen tot uw app.
* De **niet voor** en **verlooptijd** claims - om te controleren of het token niet is verlopen.
* De **verlener** claim - om te controleren of het token inderdaad aan uw app door Azure AD uitgegeven is.
* De **Nonce** : als u wilt een token replay-aanval te verhelpen.
* en nog veel meer...

Voor een volledige lijst van de claim validaties uw app moet worden uitgevoerd voor de ID-Tokens, raadpleegt u de [specificatie van het OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation). Details van de verwachte waarden voor deze claims worden opgenomen in de voorgaande [id_token sectie](#id-tokens) sectie.

## <a name="sample-tokens"></a>Voorbeeld-Tokens

Deze sectie vindt voorbeelden van SAML- en JWT-tokens die Azure AD als resultaat geeft. Deze voorbeelden kunnen u zien van de claims in de context.

### <a name="saml-token"></a>SAML-Token

Dit is een voorbeeld van een typische SAML-token.

    <?xml version="1.0" encoding="UTF-8"?>
    <t:RequestSecurityTokenResponse xmlns:t="http://schemas.xmlsoap.org/ws/2005/02/trust">
      <t:Lifetime>
        <wsu:Created xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">2014-12-24T05:15:47.060Z</wsu:Created>
        <wsu:Expires xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">2014-12-24T06:15:47.060Z</wsu:Expires>
      </t:Lifetime>
      <wsp:AppliesTo xmlns:wsp="http://schemas.xmlsoap.org/ws/2004/09/policy">
        <EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
          <Address>https://contoso.onmicrosoft.com/MyWebApp</Address>
        </EndpointReference>
      </wsp:AppliesTo>
      <t:RequestedSecurityToken>
        <Assertion xmlns="urn:oasis:names:tc:SAML:2.0:assertion" ID="_3ef08993-846b-41de-99df-b7f3ff77671b" IssueInstant="2014-12-24T05:20:47.060Z" Version="2.0">
          <Issuer>https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/</Issuer>
          <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
            <ds:SignedInfo>
              <ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
              <ds:SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256" />
              <ds:Reference URI="#_3ef08993-846b-41de-99df-b7f3ff77671b">
                <ds:Transforms>
                  <ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature" />
                  <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
                </ds:Transforms>
                <ds:DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256" />
                <ds:DigestValue>cV1J580U1pD24hEyGuAxrbtgROVyghCqI32UkER/nDY=</ds:DigestValue>
              </ds:Reference>
            </ds:SignedInfo>
            <ds:SignatureValue>j+zPf6mti8Rq4Kyw2NU2nnu0pbJU1z5bR/zDaKaO7FCTdmjUzAvIVfF8pspVR6CbzcYM3HOAmLhuWmBkAAk6qQUBmKsw+XlmF/pB/ivJFdgZSLrtlBs1P/WBV3t04x6fRW4FcIDzh8KhctzJZfS5wGCfYw95er7WJxJi0nU41d7j5HRDidBoXgP755jQu2ZER7wOYZr6ff+ha+/Aj3UMw+8ZtC+WCJC3yyENHDAnp2RfgdElJal68enn668fk8pBDjKDGzbNBO6qBgFPaBT65YvE/tkEmrUxdWkmUKv3y7JWzUYNMD9oUlut93UTyTAIGOs5fvP9ZfK2vNeMVJW7Xg==</ds:SignatureValue>
            <KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
              <X509Data>
                <X509Certificate>MIIDPjCCAabcAwIBAgIQsRiM0jheFZhKk49YD0SK1TAJBgUrDgMCHQUAMC0xKzApBgNVBAMTImFjY291bnRzLmFjY2Vzc2NvbnRyb2wud2luZG93cy5uZXQwHhcNMTQwMTAxMDcwMDAwWhcNMTYwMTAxMDcwMDAwWjAtMSswKQYDVQQDEyJhY2NvdW50cy5hY2Nlc3Njb250cm9sLndpbmRvd3MubmV0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAkSCWg6q9iYxvJE2NIhSyOiKvqoWCO2GFipgH0sTSAs5FalHQosk9ZNTztX0ywS/AHsBeQPqYygfYVJL6/EgzVuwRk5txr9e3n1uml94fLyq/AXbwo9yAduf4dCHTP8CWR1dnDR+Qnz/4PYlWVEuuHHONOw/blbfdMjhY+C/BYM2E3pRxbohBb3x//CfueV7ddz2LYiH3wjz0QS/7kjPiNCsXcNyKQEOTkbHFi3mu0u13SQwNddhcynd/GTgWN8A+6SN1r4hzpjFKFLbZnBt77ACSiYx+IHK4Mp+NaVEi5wQtSsjQtI++XsokxRDqYLwus1I1SihgbV/STTg5enufuwIDAQABo2IwYDBeBgNVHQEEVzBVgBDLebM6bK3BjWGqIBrBNFeNoS8wLTErMCkGA1UEAxMiYWNjb3VudHMuYWNjZXNzY29udHJvbC53aW5kb3dzLm5ldIIQsRiM0jheFZhKk49YD0SK1TAJBgUrDgMCHQUAA4IBAQCJ4JApryF77EKC4zF5bUaBLQHQ1PNtA1uMDbdNVGKCmSp8M65b8h0NwlIjGGGy/unK8P6jWFdm5IlZ0YPTOgzcRZguXDPj7ajyvlVEQ2K2ICvTYiRQqrOhEhZMSSZsTKXFVwNfW6ADDkN3bvVOVbtpty+nBY5UqnI7xbcoHLZ4wYD251uj5+lo13YLnsVrmQ16NCBYq2nQFNPuNJw6t3XUbwBHXpF46aLT1/eGf/7Xx6iy8yPJX4DyrpFTutDz882RWofGEO5t4Cw+zZg70dJ/hH/ODYRMorfXEW+8uKmXMKmX2wyxMKvfiPbTy5LmAU8Jvjs2tLg4rOBcXWLAIarZ</X509Certificate>
              </X509Data>
            </KeyInfo>
          </ds:Signature>
          <Subject>
            <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent">m_H3naDei2LNxUmEcWd0BZlNi_jVET1pMLR6iQSuYmo</NameID>
            <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />
          </Subject>
          <Conditions NotBefore="2014-12-24T05:15:47.060Z" NotOnOrAfter="2014-12-24T06:15:47.060Z">
            <AudienceRestriction>
              <Audience>https://contoso.onmicrosoft.com/MyWebApp</Audience>
            </AudienceRestriction>
          </Conditions>
          <AttributeStatement>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
              <AttributeValue>a1addde8-e4f9-4571-ad93-3059e3750d23</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/tenantid">
              <AttributeValue>b9411234-09af-49c2-b0c3-653adc1f376e</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
              <AttributeValue>sample.admin@contoso.onmicrosoft.com</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname">
              <AttributeValue>Admin</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname">
              <AttributeValue>Sample</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/groups">
              <AttributeValue>5581e43f-6096-41d4-8ffa-04e560bab39d</AttributeValue>
              <AttributeValue>07dd8a89-bf6d-4e81-8844-230b77145381</AttributeValue>
              <AttributeValue>0e129f4g-6b0a-4944-982d-f776000632af</AttributeValue>
              <AttributeValue>3ee07328-52ef-4739-a89b-109708c22fb5</AttributeValue>
              <AttributeValue>329k14b3-1851-4b94-947f-9a4dacb595f4</AttributeValue>
              <AttributeValue>6e32c650-9b0a-4491-b429-6c60d2ca9a42</AttributeValue>
              <AttributeValue>f3a169a7-9a58-4e8f-9d47-b70029v07424</AttributeValue>
              <AttributeValue>8e2c86b2-b1ad-476d-9574-544d155aa6ff</AttributeValue>
              <AttributeValue>1bf80264-ff24-4866-b22c-6212e5b9a847</AttributeValue>
              <AttributeValue>4075f9c3-072d-4c32-b542-03e6bc678f3e</AttributeValue>
              <AttributeValue>76f80527-f2cd-46f4-8c52-8jvd8bc749b1</AttributeValue>
              <AttributeValue>0ba31460-44d0-42b5-b90c-47b3fcc48e35</AttributeValue>
              <AttributeValue>edd41703-8652-4948-94a7-2d917bba7667</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/identityprovider">
              <AttributeValue>https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/</AttributeValue>
            </Attribute>
          </AttributeStatement>
          <AuthnStatement AuthnInstant="2014-12-23T18:51:11.000Z">
            <AuthnContext>
              <AuthnContextClassRef>urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
            </AuthnContext>
          </AuthnStatement>
        </Assertion>
      </t:RequestedSecurityToken>
      <t:RequestedAttachedReference>
        <SecurityTokenReference xmlns="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd" xmlns:d3p1="http://docs.oasis-open.org/wss/oasis-wss-wssecurity-secext-1.1.xsd" d3p1:TokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0">
          <KeyIdentifier ValueType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLID">_3ef08993-846b-41de-99df-b7f3ff77671b</KeyIdentifier>
        </SecurityTokenReference>
      </t:RequestedAttachedReference>
      <t:RequestedUnattachedReference>
        <SecurityTokenReference xmlns="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd" xmlns:d3p1="http://docs.oasis-open.org/wss/oasis-wss-wssecurity-secext-1.1.xsd" d3p1:TokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0">
          <KeyIdentifier ValueType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLID">_3ef08993-846b-41de-99df-b7f3ff77671b</KeyIdentifier>
        </SecurityTokenReference>
      </t:RequestedUnattachedReference>
      <t:TokenType>http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0</t:TokenType>
      <t:RequestType>http://schemas.xmlsoap.org/ws/2005/02/trust/Issue</t:RequestType>
      <t:KeyType>http://schemas.xmlsoap.org/ws/2005/05/identity/NoProofKey</t:KeyType>
    </t:RequestSecurityTokenResponse>

### <a name="jwt-token---user-impersonation"></a>JWT-Token - Gebruikersimitaties
Dit is een voorbeeld van een typische JSON web token (JWT) gebruikt in een grant-autorisatiecodestroom.
Naast claims, bevat het token een versienummer in **ver** en **appidacr**, authentication context class reference, waarmee wordt aangegeven hoe de client is geverifieerd. Voor een openbare-client heeft de waarde 0 is. Als een client-ID of een clientgeheim gebruikt, is de waarde 1.

    {
     typ: "JWT",
     alg: "RS256",
     x5t: "kriMPdmBvx68skT8-mPAB3BseeA"
    }.
    {
     aud: "https://contoso.onmicrosoft.com/scratchservice",
     iss: "https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/",
     iat: 1416968588,
     nbf: 1416968588,
     exp: 1416972488,
     ver: "1.0",
     tid: "b9411234-09af-49c2-b0c3-653adc1f376e",
     amr: [
      "pwd"
     ],
     roles: [
      "Admin"
     ],
     oid: "6526e123-0ff9-4fec-ae64-a8d5a77cf287",
     upn: "sample.user@contoso.onmicrosoft.com",
     unique_name: "sample.user@contoso.onmicrosoft.com",
     sub: "yf8C5e_VRkR1egGxJSDt5_olDFay6L5ilBA81hZhQEI",
     family_name: "User",
     given_name: "Sample",
     groups: [
      "0e129f6b-6b0a-4944-982d-f776000632af",
      "323b13b3-1851-4b94-947f-9a4dacb595f4",
      "6e32c250-9b0a-4491-b429-6c60d2ca9a42",
      "f3a161a7-9a58-4e8f-9d47-b70022a07424",
      "8d4c81b2-b1ad-476d-9574-544d155aa6ff",
      "1bf80164-ff24-4866-b19c-6212e5b9a847",
      "76f80127-f2cd-46f4-8c52-8edd8bc749b1",
      "0ba27160-44d0-42b5-b90c-47b3fcc48e35"
     ],
     appid: "b075ddef-0efa-123b-997b-de1337c29185",
     appidacr: "1",
     scp: "user_impersonation",
     acr: "1"
    }.

## <a name="related-content"></a>Gerelateerde inhoud
* Zie de Azure AD Graph [beleid operations](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations) en de [beleid entiteit](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#policy-entity), voor meer informatie over het beheren van de levensduur van token beleid via de Azure AD Graph API.
* Zie voor meer informatie over en voorbeelden voor het beheren van beleid via PowerShell-cmdlets, inclusief voorbeelden, [configureerbare token levensduur in Azure AD](../active-directory-configurable-token-lifetimes.md). 
