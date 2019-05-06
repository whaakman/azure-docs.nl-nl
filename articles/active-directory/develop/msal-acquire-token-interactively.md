---
title: Beheren van tokens (Microsoft Authentication Library) | Azure
description: Meer informatie over het ophalen en opslaan in cache met behulp van de Microsoft Authentication Library (MSAL).
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: celested
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/24/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0d32b56b28d9ce7425e782fc10fa9ffb67047ce0
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65139503"
---
# <a name="acquiring-and-caching-tokens-using-msal"></a>Ophalen en opslaan in cache tokens met MSAL
[Toegangstokens](access-tokens.md) zorgen ervoor dat clients veilig aanroepen van web-API's die zijn beveiligd door Azure. Er zijn veel manieren om een token met behulp van Microsoft Authentication Library (MSAL) te verkrijgen. Enkele manieren vereisen gebruikersinteracties via een webbrowser. Sommige nodig geen voor alle gebruikersinteracties. In het algemeen is de manier om een token te verkrijgen is afhankelijk van of de toepassing een openbare client-toepassing (desktop- of mobiele app) of een vertrouwelijke client-toepassing (Web-App, Web-API of daemon-toepassing, zoals een Windows-service is).

MSAL slaat een token nadat deze is aangeschaft.  Toepassingscode moet proberen een token op de achtergrond (uit de cache), eerst ophalen voor het ophalen van een token op een andere manier.

U kunt ook de tokencache wordt bereikt door het verwijderen van de accounts uit de cache wissen. De sessiecookie die zich in de browser, maar wordt niet verwijderd.

## <a name="scopes-when-acquiring-tokens"></a>Bereiken bij het verkrijgen van tokens
[Scopes](v2-permissions-and-consent.md) zijn de machtigingen die een web-API beschikbaar maakt voor clienttoepassingen om toegang aan te vragen. Clienttoepassingen aanvragen van de gebruiker toestemming voor deze bereiken bij het maken van verificatieaanvragen voor het ophalen van tokens voor toegang tot de web-API's. MSAL kunt u tokens voor toegang tot Azure AD voor ontwikkelaars (v1.0) en Microsoft identity-platform (v2.0) API's. v2.0-protocol maakt gebruik van bereiken in plaats van de resource in de aanvragen. Lees voor meer informatie, [vergelijking v1.0 en v2.0](active-directory-v2-compare.md). Het v2.0-eindpunt retourneert op basis van de web-API van configuratie van de token versie accepteert, het toegangstoken en MSAL.

Een aantal MSAL verkrijgen token als methode vereist een *scopes* parameter. Deze parameter is een eenvoudige lijst met tekenreeksen die de gewenste machtigingen en bronnen die worden aangevraagd declareren. Bekende bereiken zijn de [machtigingen voor Microsoft Graph](/graph/permissions-reference).

Het is ook mogelijk in MSAL voor toegang tot v1.0 resources. Lees voor meer informatie, [bereiken voor een toepassing v1.0](msal-v1-app-scopes.md).

### <a name="request-specific-scopes-for-a-web-api"></a>Aanvragen van specifieke bereiken voor een web-API
Wanneer de toepassing nodig heeft om aan te vragen van tokens met specifieke machtigingen voor een resource-API, moet u om door te geven van de bereiken met de ID-URI van de API-app in de onderstaande indeling:  *&lt;app ID URI&gt; / &lt;bereik&gt;*

Bijvoorbeeld, bereiken voor Microsoft Graph API: `https://graph.microsoft.com/User.Read`

Of, bijvoorbeeld bereiken voor een aangepaste web-API: `api://abscdefgh-1234-abcd-efgh-1234567890/api.read`

Voor de Microsoft Graph API, alleen een waarde voor het bereik `user.read` wordt toegewezen aan `https://graph.microsoft.com/User.Read` formatteren en door elkaar kunnen worden gebruikt.

> [!NOTE]
> Bepaalde web-API's, zoals Azure Resource Manager-API (https://management.core.windows.net/) verwacht een afsluitende slash (/) claim in de doelgroep (aud) van het toegangstoken. In dit geval is het belangrijk om door te geven van het bereik als https://management.core.windows.net//user_impersonation (Let op de dubbele slash), voor het token geldig te zijn in de API.

### <a name="request-dynamic-scopes-for-incremental-consent"></a>Dynamische bereiken van de aanvraag voor incrementele toestemming
Bij het bouwen van toepassingen die gebruikmaken van v1.0, moest u de volledige set machtigingen (statische bereiken) vereist voor de toepassing voor de gebruiker om toestemming op het moment van de aanmelding registreren. In versie 2.0, kunt u extra machtigingen aanvragen indien nodig met de scope-parameter. Deze heten dynamische bereiken en toestaan dat de gebruiker incrementele toestemming te bereiken.

U kunt bijvoorbeeld in eerste instantie Meld u aan de gebruiker en ze elk soort toegang weigeren. Later, kunt u ze geven de mogelijkheid om te lezen van de agenda van de gebruiker door het aanvragen van het bereik van de kalender in het token als methode ophalen en toestemming van de gebruiker.

Bijvoorbeeld: `https://graph.microsoft.com/User.Read` en `https://graph.microsoft.com/Calendar.Read`

## <a name="acquiring-tokens-silently-from-the-cache"></a>Verkrijgen van tokens op de achtergrond (uit de cache)
MSAL onderhoudt een tokencache (of twee caches voor toepassingen van vertrouwelijke client) en een token opgeslagen nadat deze is aangeschaft.  In veel gevallen bij het ophalen van een token op de achtergrond wordt een ander token ophalen met meer bereiken op basis van een token in de cache. Het is ook geschikt voor een token vernieuwen als dit het ophalen van verloopt bijna (zoals de tokencache ook een vernieuwingstoken bevat).

### <a name="recommended-call-pattern-for-public-client-applications"></a>De aanroep van de aanbevolen patroon voor openbare-clienttoepassingen
Toepassingscode moet ophalen van een token op de achtergrond (uit de cache), eerst proberen.  Als de aanroep van de methode een 'UI is vereist' fout of uitzondering retourneert, probeert u het ophalen van een token op een andere manier. 

Er zijn echter twee stromen waarvoor u **beter niet** proberen een token op de achtergrond ophalen:

- [clientreferenties die stroom](msal-authentication-flows.md#client-credentials), die geen token cache van de gebruiker, maar een tokencache toepassing gebruikt. Deze methode zorgt dat deze toepassing-tokencache controleren voordat een aanvraag wordt verzonden naar de STS.
- [autorisatiecodestroom](msal-authentication-flows.md#authorization-code) in Web-Apps, zoals deze wordt ingewisseld een code die de toepassing hebt verkregen door ondertekening in de gebruiker is, en dat zij toestemming voor meer bereiken. Omdat een code wordt doorgegeven als een parameter en niet op een account, de methode kan niet zoeken in de cache voor het inwisselen van de code die is vereist, toch een oproep naar de service.

### <a name="recommended-call-pattern-in-web-apps-using-the-authorization-code-flow"></a>Aanbevolen aanroep patroon in Web Apps met behulp van de stroom autorisatiecode 
Voor webtoepassingen die gebruikmaken van de [OpenID Connect-autorisatiecodestroom](v2-protocols-oidc.md), de aanbevolen patroon in de controllers is:

- Exemplaar maken van een vertrouwelijke client-toepassing met een token cache met aangepaste serialisatie. 
- Het token met behulp van de stroom van de autorisatiecode ophalen

## <a name="acquiring-tokens"></a>Verkrijgen van tokens
Over het algemeen de methode van het ophalen van een token is afhankelijk van of het een openbare client of een vertrouwelijke client-toepassing is.

### <a name="public-client-applications"></a>Openbare clienttoepassingen
Voor openbare-clienttoepassingen (desktop- of mobiele app), u:
- Vaak tokens verkrijgen interactief, zorg dat de gebruiker zich aanmeldt via een gebruikersinterface of de pop-upvenster.
- Kan [op de achtergrond een token verkrijgen voor de aangemelde gebruiker](msal-authentication-flows.md#integrated-windows-authentication) met behulp van geïntegreerde Windows-verificatie (IWA/Kerberos) als de desktop-toepassing wordt uitgevoerd op een Windows-computer lid is van een domein of naar Azure.
- Kan [ophalen een token met een gebruikersnaam en wachtwoord](msal-authentication-flows.md#usernamepassword) in .NET framework-bureaubladclient toepassingen, maar dit wordt niet aanbevolen. Gebruik geen gebruikersnaam en wachtwoord in vertrouwelijke client-toepassingen.
- Een token via kunt verkrijgen de [apparaat codestroom](msal-authentication-flows.md#device-code) in toepassingen die worden uitgevoerd op apparaten die niet beschikken over een webbrowser. De gebruiker wordt geleverd met een URL en een code, die vervolgens gaat naar een webbrowser op een ander apparaat en de code opgeeft en zich aanmeldt.  Vervolgens Azure AD stuurt een token terug naar het apparaat in browser.

### <a name="confidential-client-applications"></a>Vertrouwelijke client-toepassingen 
Voor vertrouwelijke client-toepassingen (Web-App, Web-API of daemon-toepassing, zoals een Windows-service), u:
- Tokens verkrijgen **voor de toepassing zelf** en niet voor een gebruiker, met behulp van de [clientreferenties die stroom](msal-authentication-flows.md#client-credentials). Dit kan worden gebruikt voor het synchroniseren hulpprogramma's, of hulpprogramma's waarmee gebruikers in het algemeen worden verwerkt en niet op een specifieke gebruiker. 
- Gebruik de [op namens-stroom](msal-authentication-flows.md#on-behalf-of) voor een web-API voor het aanroepen van een API namens de gebruiker. De toepassing wordt aangeduid met de clientreferenties van de om te kunnen verkrijgen van een token op basis van een gebruiker-verklaring (SAML bijvoorbeeld of een JWT-token). Deze stroom wordt gebruikt door toepassingen die toegang moeten krijgen tot resources van een bepaalde gebruiker in de service-naar-service aanroepen.
- Verkrijgen met behulp van de [autorisatiecodestroom](msal-authentication-flows.md#authorization-code) in web-apps nadat de gebruiker zich aanmeldt bij via de autorisatie aanvraag-URL. OpenID Connect-toepassing wordt doorgaans gebruikt dit mechanisme waarmee de gebruiker aanmelden met behulp van Open ID connect en toegang tot web-API's namens de gebruiker.


## <a name="authentication-results"></a>Verificatie-resultaten 
Wanneer de client een toegangstoken vraagt, retourneert Azure AD ook een verificatieresultaat waarin een aantal metagegevens over het toegangstoken. Deze informatie omvat de verlooptijd van het toegangstoken en de bereiken waarvoor deze geldig is. Deze gegevens kan uw app doen van intelligente cachebewerkingen van toegangstokens zonder te parseren van het toegangstoken zelf.  Resultaat van de verificatie wordt aangegeven:

- De [toegangstoken](access-tokens.md) voor de web-API voor toegang tot resources. Dit is een tekenreeks, meestal een met base64 gecodeerde JWT maar de client moet nooit zoeken binnen het toegangstoken. De indeling wordt niet gegarandeerd blijft stabiel en deze kan worden versleuteld voor de resource. Gebruikers schrijven van code, afhankelijk van de token toegang tot inhoud op de client is een van de grootste bronnen van fouten en client logische onderbrekingen.
- De [ID-token](id-tokens.md) voor de gebruiker (dit is een JWT).
- Het token verlopen, wordt de datum en tijd wanneer het token is verlopen.
- De tenant-ID bevat de tenant waarin de gebruiker is gevonden. Gastgebruikers (Azure AD B2B-scenario's) is de tenant-ID de Gast-tenant, niet de unieke tenant. Wanneer het token wordt doorgegeven in de naam van een gebruiker, bevat het verificatieresultaat ook informatie over deze gebruiker. Voor vertrouwelijke client stromen waar tokens worden aangevraagd zonder gebruiker (voor de toepassing), is deze gebruikersgegevens null.
- De bereiken waarvoor het token is uitgegeven.
- De unieke ID voor de gebruiker.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [afhandeling van fouten en uitzonderingen](msal-handling-exceptions.md). 
