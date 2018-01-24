---
title: Handleiding voor ontwikkelaars voor voorwaardelijke toegang van Azure Active Directory
description: Handleiding voor ontwikkelaars en scenario's voor voorwaardelijke toegang van Azure AD
services: active-directory
keywords: 
author: danieldobalian
manager: mtillman
editor: PatAltimore
ms.author: dadobali
ms.date: 07/19/2017
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.openlocfilehash: 8ad1ed1b8a178cb8906e6233e6bd78f50d01c50c
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="developer-guidance-for-azure-active-directory-conditional-access"></a>Handleiding voor ontwikkelaars voor voorwaardelijke toegang van Azure Active Directory

Azure Active Directory (AD) biedt verschillende manieren voor het beveiligen van uw app en het beveiligen van een service.  Een van deze unieke functies is voorwaardelijke toegang.  Voorwaardelijke toegang kan ontwikkelaars en enterprise-klanten te beschermen services in een groot aantal manieren:

* Multi-Factor Authentication
* Zodat alleen Intune ingeschreven apparaten hebben toegang tot specifieke services
* Beperken van de gebruikerslocaties en IP-adresbereiken

Zie voor meer informatie over de volledige functionaliteit van voorwaardelijke toegang [voorwaardelijke toegang in Azure Active Directory](../active-directory-conditional-access-azure-portal.md). 

In dit artikel richten we op wat voorwaardelijke toegang betekent dat ontwikkelaars bouwen van apps voor Azure AD.  Er wordt vanuit gegaan kennis van [één](active-directory-integrating-applications.md) en [multitenant](active-directory-devhowto-multi-tenant-overview.md) apps en [algemene patronen voor verificatie](active-directory-authentication-scenarios.md).

We je Duik in de impact van de toegang tot resources die u hebt geen controle over waarvoor beleid voor voorwaardelijke toegang toegepast.  Bovendien verkennen we de implicaties van voorwaardelijke toegang in de op namens-stroom, web-apps, toegang tot Microsoft Graph en het aanroepen van API's.

## <a name="how-does-conditional-access-impact-an-app"></a>Hoe beïnvloedt voorwaardelijke toegang tot een app

### <a name="app-types-impacted"></a>Beïnvloed App-typen

In de meest voorkomende gevallen voorwaardelijke toegang wordt het gedrag van een app niet gewijzigd of eventuele wijzigingen van de ontwikkelaar vereist.  Wanneer een app, indirect of op de achtergrond een token voor een service vraagt vereist een app alleen in bepaalde gevallen codewijzigingen voor het afhandelen van voorwaardelijke toegang 'uitdagingen'.  Het is mogelijk net zo eenvoudig als het uitvoeren van een aanvraag voor interactief aanmelden. 

De volgende scenario's vereist in het bijzonder code voor het verwerken van voorwaardelijke toegang 'uitdagingen': 

* Apps toegang tot Microsoft Graph
* Apps voor het uitvoeren van de op namens-stroom
* Apps toegang krijgen tot meerdere services/bronnen
* Apps van één pagina met ADAL.js
* Web-Apps voor het aanroepen van een resource

Voorwaardelijke toegang beleidsregels kunnen worden toegepast op de app, maar kunnen ook worden toegepast op een web-API uw app wordt geopend. Raadpleeg voor meer informatie over het configureren van beleid voor voorwaardelijke toegang, [aan de slag met Azure Active Directory voorwaardelijke toegang](../active-directory-conditional-access-azure-portal-get-started.md).

Afhankelijk van het scenario met een enterprise-klanten toepassen en beleidsregels voor voorwaardelijke toegang op elk gewenst moment verwijderen.  Om uw app om te blijven functioneren als een nieuw beleid wordt toegepast, moet u de verwerking van de 'uitdaging' implementeren. De volgende voorbeelden ziet challenge-verwerking. 

### <a name="conditional-access-examples"></a>Voorbeelden van voorwaardelijke toegang

Sommige scenario's vereist codewijzigingen voor het afhandelen van voorwaardelijke toegang, terwijl anderen werken zoals is.  Hier volgen enkele scenario's met behulp van voorwaardelijke toegang voor multi-factor Authentication waarmee enig inzicht in het verschil.

* U maakt een één-tenant iOS-app en toepassen van beleid voor voorwaardelijke toegang.  De app een gebruiker zich aanmeldt en geen toegang vragen tot een API.  Wanneer de gebruiker zich aanmeldt, wordt het beleid wordt automatisch geactiveerd en wordt de gebruiker moet multi-factor authentication (MFA) uitvoeren. 
* U maakt een multitenant-web-app die gebruikmaakt van Microsoft Graph voor toegang tot Exchange, andere services.  Een enterprise-klanten die deze app aanneemt stelt een beleid in Exchange.  Als de web-app een token voor MS Graph vraagt, wordt de app wordt niet gevraagd om te voldoen aan het beleid.  De eindgebruiker is aangemeld met een geldige-tokens. Wanneer de app probeert te gebruiken van dit token tegen Microsoft Graph voor toegang tot Exchange-gegevens, een claims 'uitdaging' wordt geretourneerd naar de web-app via de ```WWW-Authenticate``` header.  De app kan vervolgens worden gebruikt de ```claims``` in een nieuwe aanvraag en wordt de eindgebruiker gevraagd om te voldoen aan de voorwaarden. 
* U kunt een eigen app die gebruikmaakt van een service voor de middelste laag voor toegang tot een downstream-API wilt maken.  Een klant enterprise op het gebruik van deze app bedrijf geldt een beleid voor de downstream-API.  Wanneer een eindgebruiker zich aanmeldt, wordt de systeemeigen app vraagt om toegang tot de middelste laag en stuurt het token.  De middelste laag wordt uitgevoerd op namens-stroom voor het aanvragen van toegang tot de downstream-API.  Op dit moment wordt een claims 'uitdaging' geverifieerd op de middelste laag. De uitdaging waar wordt de middelste laag teruggestuurd naar de systeemeigen app, die behoeften te voldoen aan het beleid voor voorwaardelijke toegang.

### <a name="complying-with-a-conditional-access-policy"></a>Voldoen aan beleid voor voorwaardelijke toegang

Beleid voor voorwaardelijke toegang wordt voor verschillende andere app topologieën geëvalueerd wanneer de sessie tot stand is gebracht.  Als een beleid voor voorwaardelijke toegang is van invloed op de granulatie van apps en services, afhankelijk het punt waarop deze wordt aangeroepen van geheugenlatentie het scenario dat u probeert te bereiken.

Wanneer uw app probeert te krijgen tot een service met een beleid voor voorwaardelijke toegang, ondervinden het moeilijk voorwaardelijke toegang.  Deze uitdaging wordt gecodeerd in de `claims` parameter die wordt geleverd in een antwoord van Azure AD of Microsoft Graph.  Hier volgt een voorbeeld van deze uitdaging parameter: 

```
claims={"access_token":{"polids":{"essential":true,"Values":["<GUID>"]}}}
```

Ontwikkelaars kunnen de deze uitdaging en voegt deze naar een nieuwe aanvraag naar Azure AD.  Deze status wordt doorgegeven, wordt geen actie nodig om te voldoen aan het beleid voor voorwaardelijke toegang van de eindgebruiker gevraagd. Details van de fout en het uitpakken van de parameter worden in de volgende scenario's beschreven. 

## <a name="scenarios"></a>Scenario's

### <a name="prerequisites"></a>Vereisten

Voorwaardelijke toegang van Azure AD is een functie die is opgenomen in [Azure AD Premium](../active-directory-whatis.md#choose-an-edition).  U kunt meer informatie over het licentievereisten in de [niet-gelicentieerde gebruiksrapport](../active-directory-conditional-access-unlicensed-usage-report.md).  Ontwikkelaars kunnen worden gekoppeld aan de [Microsoft Developer Network](https://msdn.microsoft.com/dn308572.aspx), waaronder een gratis abonnement op de Enterprise Mobility Suite waaronder Azure AD Premium.

### <a name="considerations-for-specific-scenarios"></a>Overwegingen voor het specifieke scenario 's

De volgende informatie is alleen van toepassing in deze scenario's voor voorwaardelijke toegang:

* Apps toegang tot Microsoft Graph
* Apps voor het uitvoeren van de op namens-stroom
* Apps toegang krijgen tot meerdere services/bronnen
* Apps van één pagina met ADAL.js

In de volgende secties je krijgt in algemene scenario's waarin complexere zijn.  De kern van het besturingssysteem principe is voorwaardelijk beleid wordt geëvalueerd op het moment dat het token is aangevraagd voor de service die toegepast is, tenzij het wordt gebruikt door Microsoft Graph beleid voor voorwaardelijke toegang.

## <a name="scenario-app-accessing-microsoft-graph"></a>Scenario: App toegang tot Microsoft Graph

In dit scenario doorlopen we het geval wanneer toegang tot de aanvragen van een web apps naar Microsoft Graph. Beleid voor voorwaardelijke toegang kan in dit geval worden toegewezen aan SharePoint, Exchange of een andere service die wordt geopend als een werklast via Microsoft Graph.  In dit voorbeeld gaan we ervan uit dat er is een beleid voor voorwaardelijke toegang op Sharepoint Online.

![App-toegang tot Microsoft Graph stroomdiagram](media/active-directory-conditional-access-developer/app-accessing-microsoft-graph-scenario.png)

De app verzoekt eerst autorisatie voor Microsoft Graph waarvoor toegang tot een downstream werkbelasting zonder voorwaardelijke toegang is vereist.  De aanvraag kan worden uitgevoerd zonder een beleid wordt aangeroepen en de app-tokens ontvangen voor Microsoft Graph.  De app kan het toegangstoken op dit moment gebruiken in een bearer-aanvraag voor het eindpunt aangevraagd. Nu nodig de app toegang heeft tot een Sharepoint Online-eindpunt van Microsoft Graph, bijvoorbeeld:`https://graph.microsoft.com/v1.0/me/mySite`

De app heeft al een geldig token voor Microsoft Graph, zodat deze de nieuwe aanvraag uitvoeren kan zonder een nieuw token worden verleend. Deze aanvraag is mislukt en claims moeilijk is uitgegeven door Microsoft Graph in de vorm van een HTTP 403-verboden met een ```WWW-Authenticate``` uitdaging.
Hier volgt een voorbeeld van het antwoord: 

```
HTTP 403; Forbidden 
error=insufficient_claims
www-authenticate="Bearer realm="", authorization_uri="https://login.windows.net/common/oauth2/authorize", client_id="<GUID>", error=insufficient_claims, claims={"access_token":{"polids":{"essential":true,"values":["<GUID>"]}}}"
```

De uitdaging van claims is opgenomen in de ```WWW-Authenticate``` koptekst, die het uitpakken van de claims-parameter voor de volgende aanvraag kan worden geparseerd.  Zodra deze wordt toegevoegd aan de nieuwe aanvraag, Azure AD, kent het beleid voor voorwaardelijke toegang evalueren als u de gebruiker zich aanmeldt en de app is nu in overeenstemming met het beleid voor voorwaardelijke toegang.  De aanvraag voor het Sharepoint Online-eindpunt herhalende slaagt.

De ```WWW-Authenticate``` header heeft een unieke structuur en is geen trivial parseren om de waarden ophalen.  Hier volgt een korte methode om u te helpen.

```csharp
        /// <summary>
        /// This method extracts the claims value from the 403 error response from MS Graph. 
        /// </summary>
        /// <param name="wwwAuthHeader"></param>
        /// <returns>Value of the claims entry. This should be considered an opaque string. 
        /// Returns null if the wwwAuthheader does not contain the claims value. </returns>
        private String extractClaims(String wwwAuthHeader)
        {
            String ClaimsKey = "claims=";
            String ClaimsSubstring = "";
            if (wwwAuthHeader.Contains(ClaimsKey))
            {
                int Index = wwwAuthHeader.IndexOf(ClaimsKey);
                ClaimsSubstring = wwwAuthHeader.Substring(Index, wwwAuthHeader.Length - Index);
                string ClaimsChallenge;
                if (Regex.Match(ClaimsSubstring, @"}$").Success)
                {
                    ClaimsChallenge = ClaimsSubstring.Split('=')[1];
                }
                else
                {
                    ClaimsChallenge = ClaimsSubstring.Substring(0, ClaimsSubstring.IndexOf("},") + 1);
                }
                return ClaimsChallenge;
            }
            return null; 
        }
```

Voor de codevoorbeelden die laten zien hoe u de uitdaging claims verwerken, raadpleegt u de [codevoorbeeld-op-andere gebruikers-of](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof-ca) voor ADAL .NET.

## <a name="scenario-app-performing-the-on-behalf-of-flow"></a>Scenario: App uitvoeren van de op namens-stroom

In dit scenario doorlopen we het geval waarbij een systeemeigen app een/API-webservice roept.  Op zijn beurt deze service biedt [de ' op namens-' stroom](active-directory-authentication-scenarios.md#application-types-and-scenarios) een downstream service aanroepen.  In ons geval we onze beleid voor voorwaardelijke toegang hebt toegepast op de downstream-service (Web API 2) en een systeemeigen app in plaats van een server/daemon-app gebruikt. 

![Uitvoeren van het stroomdiagram op-andere gebruikers-of App](media/active-directory-conditional-access-developer/app-performing-on-behalf-of-scenario.png)

De eerste tokenaanvraag voor Web-API-1 wordt de eindgebruiker voor multi-factor authentication niet gevraagd als Web-API 1 de downstream-API niet altijd mogelijk bereikt.  Wanneer de Web-API 1 wil een token op-andere gebruikers-of de gebruiker een aanvraag voor Web API 2, wordt het verzoek mislukken omdat de gebruiker niet is aangemeld met multi-factor authentication-server.

Azure AD retourneert een HTTP-antwoord met enkele interessante gegevens: 

> [!NOTE]
> In dit exemplaar is een foutbeschrijving multi-factor authentication-server, maar er is een breed scala aan `interaction_required` mogelijk die betrekking hebben op voorwaardelijke toegang.  

```
HTTP 400; Bad Request 
error=interaction_required
error_description=AADSTS50076: Due to a configuration change made by your administrator, or because you moved to a new location, you must use multi-factor authentication to access '<Web API 2 App/Client ID>'.
claims={"access_token":{"polids":{"essential":true,"Values":["<GUID>"]}}}
```

In onze Web API-1 wordt de fout catch `error=interaction_required`, en de `claims` voor de bureaublad-app.  Op dat moment de bureaublad-app kunt maken een nieuwe `acquireToken()` aanroepen en toevoegen de `claims`uitdaging als een extra queryreeksparameter opgeven.  Deze nieuwe aanvraag moet de gebruiker multi-factor Authentication en stuurt deze nieuwe token terug naar de Web-API-1 en voltooien van de op namens-stroom.

Als u wilt uitproberen dit scenario, Zie onze [.NET codevoorbeeld](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof-ca).  Dit laat zien hoe de uitdaging claims van Web-API 1 doorgeven aan de systeemeigen app en een nieuwe aanvraag in de client-app opstelt. 

## <a name="scenario-app-accessing-multiple-services"></a>Scenario: App toegang tot meerdere services

In dit scenario doorlopen we het geval waarbij een web-app heeft toegang tot twee services waarbij een van de toegewezen beleid voor voorwaardelijke toegang heeft.  Afhankelijk van de logische app bestaat een pad waarin uw app geen toegang krijgen tot beide webservices vereist.  In dit scenario speelt een belangrijke rol in de gebruikerservaring is de volgorde waarin u een token aanvragen.

Stel hebben we webservice A en B en webservice B heeft het beleid voor voorwaardelijke toegang toegepast.  Tijdens de eerste interactieve auth-aanvraag toestemming voor beide services vereist, wordt het beleid voor voorwaardelijke toegang niet in alle gevallen vereist.  Als de app een token voor webservice B vraagt, het beleid wordt aangeroepen en volgende aanvragen voor webservice A slaagt ook als volgt.

![App-toegang tot meerdere services stroomdiagram](media/active-directory-conditional-access-developer/app-accessing-multiple-services-scenario.png)

U kunt ook als de app een token in eerste instantie voor web service A vraagt, wordt de eindgebruiker geen gebruikgemaakt van het beleid voor voorwaardelijke toegang.  Hierdoor kan de ontwikkelaar van de app voor het beheren van de gebruikerservaring en beleid voor voorwaardelijke toegang kan worden aangeroepen in alle gevallen niet afdwingen. Het geval is lastig is als de app vervolgens een token voor webservice B. vraagt Op dit moment wordt de eindgebruiker moet voldoen aan het beleid voor voorwaardelijke toegang.  Wanneer de app probeert te `acquireToken`, kan dit de volgende fout (weergegeven in het volgende diagram) genereren: 

```
HTTP 400; Bad Request
error=interaction_required
error_description=AADSTS50076: Due to a configuration change made by your administrator, or because you moved to a new location, you must use multi-factor authentication to access '<Web API App/Client ID>'.
claims={"access_token":{"polids":{"essential":true,"Values":["<GUID>"]}}}
``` 

![App-toegang tot meerdere aanvragen van een nieuw token-services](media/active-directory-conditional-access-developer/app-accessing-multiple-services-new-token.png)

Als de app van de ADAL-bibliotheek gebruikmaakt, wordt het token verkrijgen van een mislukt altijd interactief geprobeerd.  Wanneer deze interactieve aanvraag optreedt, heeft de eindgebruiker de mogelijkheid om te voldoen aan de voorwaardelijke toegang.  Dit is de waarde true tenzij de aanvraag is een `AcquireTokenSilentAsync` of `PromptBehavior.Never` in dat geval de app moet om uit te voeren een interactieve ```AcquireToken``` aanvraag voor het gebruik van het einde van de mogelijkheid om te voldoen aan het beleid bieden. 

## <a name="scenario-single-page-app-spa-using-adaljs"></a>Scenario: Één pagina App (SPA) met behulp van ADAL.js

In dit scenario doorlopen we het geval wanneer we een app met één pagina (SPA hebben) ADAL.js met een voorwaardelijke toegang verlenen tot beveiligde web-API niet aanroepen.  Dit is een eenvoudige architectuur, maar sommige nuances die worden gehouden moet bij het ontwikkelen van rond voorwaardelijke toegang heeft.

In ADAL.js, zijn er enkele functies die tokens verkrijgen: `login()`, `acquireToken(...)`, `acquireTokenPopup(…)`, en `acquireTokenRedirect(…)`. 

* `login()`verkrijgt van een token ID via een aanvraag voor interactief aanmelden, maar biedt toegangstokens voor elke service (met inbegrip van een web-API van voorwaardelijke toegang verlenen tot beveiligde) niet ophalen.  
* `acquireToken(…)`vervolgens kan achtergrond verkrijgen van een toegangstoken dat wil zeggen dat UI wordt niet weergegeven in elk geval worden gebruikt.  
* `acquireTokenPopup(…)`en `acquireTokenRedirect(…)` zijn beide gebruikt voor het interactief aanvragen van een token voor een resource wat betekent dat ze altijd aanmelden UI niet weergeven.

Wanneer een app een toegangstoken een Web-API aan te roepen moet, wordt geprobeerd een `acquireToken(…)`.  De token sessie is verlopen of als we nodig om te voldoen aan beleid voor voorwaardelijke toegang, wordt de *acquireToken* functie mislukt en de app gebruikt `acquireTokenPopup()` of `acquireTokenRedirect()`.

![Één pagina app met behulp van ADAL stroomdiagram](media/active-directory-conditional-access-developer/spa-using-adal-scenario.png)

We doorlopen een voorbeeld met ons scenario voorwaardelijke toegang.  De eindgebruiker gewoon bevindt zich op de site en beschikt niet over een sessie.  We voeren een `login()` aanroepen, een ID zonder multi-factor authentication-token ophalen.  De gebruiker treffers vervolgens een knop die de app om gegevens van aanvragen van een web-API vereist.  De app wordt geprobeerd een `acquireToken()` aanroep maar is mislukt omdat de gebruiker multi-factor authentication-server nog en behoeften te voldoen aan het beleid voor voorwaardelijke toegang niet uitgevoerd.

Azure AD verzendt terug van de volgende HTTP-antwoord: 

```
HTTP 400; Bad Request 
error=interaction_required
error_description=AADSTS50076: Due to a configuration change made by your administrator, or because you moved to a new location, you must use multi-factor authentication to access '<Web API App/Client ID>'.
```

Onze app catch moet de `error=interaction_required`.  De toepassing kan vervolgens worden gebruikt een `acquireTokenPopup()` of `acquireTokenRedirect()` op dezelfde resource.  De gebruiker moet een multi-factor Authentication. Nadat de gebruiker is voltooid voor de multi-factor authentication, kan de app een nieuw toegangstoken voor de aangevraagde bron is uitgegeven.

Als u wilt uitproberen dit scenario, Zie onze [JS SPA op-andere gebruikers-of codevoorbeeld](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof-ca).  Deze voorbeeldcode maakt gebruik van het beleid voor voorwaardelijke toegang en de web-API die u eerder hebt geregistreerd bij een SPA JS ter illustratie van dit scenario. Er wordt weergegeven hoe goed de uitdaging claims verwerken en ophalen van een toegangstoken die kan worden gebruikt voor uw Web-API. U kunt ook de algemene afhandeling [Angular.js codevoorbeeld](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp) voor hulp bij een Hoekvormige SPA


## <a name="see-also"></a>Zie ook

* Zie voor meer informatie over de mogelijkheden, [voorwaardelijke toegang in Azure Active Directory](../active-directory-conditional-access-azure-portal.md).
* Zie voor meer Azure AD-codevoorbeelden [Github-Repo-van-codevoorbeelden](https://github.com/azure-samples?utf8=%E2%9C%93&q=active-directory). 
* Zie voor meer informatie over de ADAL SDK van en toegang naslagdocumentatie over de [bibliotheek handleiding](active-directory-authentication-libraries.md).
* Zie voor meer informatie over scenario's met meerdere tenants, [aanmelden van gebruikers met behulp van het patroon multitenant](active-directory-devhowto-multi-tenant-overview.md).
