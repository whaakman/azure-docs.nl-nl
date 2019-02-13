---
title: Richtlijnen voor ontwikkelaars voor voorwaardelijke toegang van Azure Active Directory
description: Richtlijnen voor ontwikkelaars en scenario's voor voorwaardelijke toegang voor Azure AD
services: active-directory
keywords: ''
author: CelesteDG
manager: mtillman
ms.author: celested
ms.reviewer: dadobali
ms.date: 09/24/2018
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2be77cdc4a5ad38a7d8c125fd95256e77cd92019
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56202941"
---
# <a name="developer-guidance-for-azure-active-directory-conditional-access"></a>Richtlijnen voor ontwikkelaars voor voorwaardelijke toegang van Azure Active Directory

De functie voor voorwaardelijke toegang in Azure Active Directory (Azure AD) biedt verschillende manieren die u gebruiken kunt voor het beveiligen van uw app en het beveiligen van een service. Voorwaardelijke toegang kan ontwikkelaars en enterprise-klanten services in een groot aantal manieren beveiligen:

* Multi-Factor Authentication
* Zodat alleen Intune ingeschreven apparaten toegang krijgen tot specifieke services
* Beperken van de gebruikerslocaties en IP-bereiken

Zie voor meer informatie over de volledige functionaliteit van voorwaardelijke toegang, [voorwaardelijke toegang in Azure Active Directory](../active-directory-conditional-access-azure-portal.md).

Dit artikel wordt beschreven hoe u voorwaardelijke toegang kunt gebruiken voor het bouwen van apps voor Azure AD-ontwikkelaars, en u leert ook over de impact van de toegang tot bronnen die u hebt geen controle over die mogelijk beleid voor voorwaardelijke toegang toegepast. Het artikel behandelt ook de gevolgen van voorwaardelijke toegang in de stroom op-andere gebruikers-of web-apps, toegang tot Microsoft Graph en het aanroepen van API's.

Kennis van [één](quickstart-v1-integrate-apps-with-azure-ad.md) en [multitenant](howto-convert-app-to-be-multi-tenant.md) apps en [algemene verificatiepatronen](authentication-scenarios.md) wordt uitgegaan.

## <a name="how-does-conditional-access-impact-an-app"></a>Hoe voorwaardelijke toegang van invloed zijn op een app?

### <a name="app-types-impacted"></a>Beïnvloed App-typen

In de meest voorkomende gevallen voor voorwaardelijke toegang niet verandert het gedrag van een app of vereist wijzigingen van de ontwikkelaar. Wanneer een app op de achtergrond of indirect vraagt om een token voor een service, vereist een app alleen in bepaalde gevallen wijzigingen in de code voor het afhandelen van voorwaardelijke toegang 'uitdagingen'. Kan het zijn net zo eenvoudig als het uitvoeren van een aanvraag voor interactief aanmelden.

Met name vereisen de volgende scenario's code voor het verwerken van voorwaardelijke toegang 'uitdagingen':

* Apps die toegang tot Microsoft Graph
* Apps uitvoeren van de stroom op-andere gebruikers-of
* Apps die toegang tot meerdere services/resources
* Apps van één pagina met behulp van ADAL.js
* Web Apps aanroepen van een resource

Voorwaardelijk beleid kunnen worden toegepast op de app, maar kunnen ook worden toegepast op een web-API uw app wordt geopend. Zie voor meer informatie over het configureren van beleid voor voorwaardelijke toegang, [Quick Start: MFA vereisen voor specifieke apps met voorwaardelijke toegang van Azure Active Directory](../conditional-access/app-based-mfa.md).

Afhankelijk van het scenario, een zakelijke klant toepassen en beleid voor voorwaardelijke toegang op elk gewenst moment verwijderen. In volgorde voor uw app blijven werken wanneer een nieuw beleid wordt toegepast, moet u voor het implementeren van de verwerking 'challenge'. De volgende voorbeelden ziet u challenge-verwerking.

### <a name="conditional-access-examples"></a>Voorbeelden van voorwaardelijke toegang

Sommige scenario's vereist wijzigingen in de code voor het afhandelen van voorwaardelijke toegang, terwijl anderen werken. Hier volgen enkele scenario's voor voorwaardelijke toegang met meervoudige verificatie waarbij inzicht in het verschil biedt te doen.

* U een één tenant iOS-app bouwen en toepassen van beleid voor voorwaardelijke toegang. De app zich in een gebruiker en toegang tot een API niet vragen. Wanneer de gebruiker zich aanmeldt, wordt het beleid wordt automatisch geactiveerd en moet de gebruiker multi-factor authentication (MFA) uitvoeren.
* U bouwt een multitenant-web-app die gebruikmaakt van Microsoft Graph voor toegang tot Exchange, onder andere services. Een enterprise-klant bent die deze app neemt Hiermee stelt u een beleid voor Exchange. Wanneer de web-app vraagt om een token voor MS Graph, wordt de app wordt niet gevraagd om te voldoen aan het beleid. De gebruiker is aangemeld met een geldige-tokens. Wanneer de app probeert dit token op basis van de Microsoft Graph gebruiken voor toegang tot Exchange-gegevens, een claims "uitdaging' wordt geretourneerd naar de web-app via de ```WWW-Authenticate``` header. De app kan vervolgens worden gebruikt de ```claims``` in een nieuwe aanvraag en de eindgebruiker gevraagd om te voldoen aan de voorwaarden.
* U bouwt een systeemeigen app die gebruikmaakt van een middelste laag service voor toegang tot een downstream-API. Een enterprise-klant bent van het bedrijf met behulp van deze app is een beleid van toepassing op de downstream-API. Wanneer een eindgebruiker zich aanmeldt, wordt de systeemeigen app toegang tot de middelste laag en wordt het token verzonden. De middelste laag wordt uitgevoerd op namens-stroom voor het aanvragen van toegang tot de downstream-API. Op dit moment wordt een claims "uitdaging' weergegeven voor de middelste laag. De uitdaging wordt voor de middelste laag teruggestuurd naar de systeemeigen app, die moet overeenkomen met het beleid voor voorwaardelijke toegang.

### <a name="complying-with-a-conditional-access-policy"></a>Voldoen aan beleid voor voorwaardelijke toegang

Beleid voor voorwaardelijke toegang wordt voor topologieën met meerdere verschillende app geëvalueerd wanneer de sessie tot stand is gebracht. Als een beleid voor voorwaardelijke toegang is van invloed op de granulatie van apps en services, het punt waarop de toepassing wordt aangeroepen, is afhankelijk van sterk op het scenario dat u probeert te bereiken.

Wanneer uw app probeert te krijgen tot een service met een beleid voor voorwaardelijke toegang, ondervinden het een uitdaging voor voorwaardelijke toegang. Deze uitdaging wordt gecodeerd in de `claims` parameter die wordt geleverd in een reactie van Azure AD of Microsoft Graph. Hier volgt een voorbeeld van deze parameter challenge:

```
claims={"access_token":{"polids":{"essential":true,"Values":["<GUID>"]}}}
```

Ontwikkelaars kunnen nemen deze uitdaging en voegt u deze naar een nieuwe aanvraag naar Azure AD. Deze status wordt doorgegeven, vraagt de gebruiker een actie die nodig zijn om te voldoen aan het beleid voor voorwaardelijke toegang uit te voeren. Details van de fout en het uitpakken van de parameter worden in de volgende scenario's beschreven.

## <a name="scenarios"></a>Scenario's

### <a name="prerequisites"></a>Vereisten

Voorwaardelijke toegang van Azure AD is een functie die is opgenomen [Azure AD Premium](https://docs.microsoft.com/azure/active-directory/active-directory-whatis). U kunt meer informatie over de licentievereisten in de [rapport met niet-gelicentieerde gebruik](../active-directory-conditional-access-unlicensed-usage-report.md). Ontwikkelaars kunnen deelnemen aan de [Microsoft Developer Network](https://msdn.microsoft.com/dn308572.aspx), waaronder een gratis abonnement voor Enterprise Mobility Suite, waaronder Azure AD Premium.

### <a name="considerations-for-specific-scenarios"></a>Overwegingen voor het specifieke scenario 's

De volgende informatie is alleen van toepassing in deze scenario's voor voorwaardelijke toegang:

* Apps die toegang tot Microsoft Graph
* Apps uitvoeren van de stroom op-andere gebruikers-of
* Apps die toegang tot meerdere services/resources
* Apps van één pagina met behulp van ADAL.js

De volgende secties worden besproken algemene scenario's die, hoe complexer. De belangrijkste operationele principe is voorwaardelijke toegang beleidsregels worden geëvalueerd op het moment dat het token is aangevraagd voor de service een beleid voor voorwaardelijke toegang toegepast heeft, tenzij het wordt benaderd via Microsoft Graph.

## <a name="scenario-app-accessing-microsoft-graph"></a>Scenario: App-toegang tot Microsoft Graph

In dit scenario leert u hoe de toegang tot Microsoft Graph voor het aanvragen van een web-app. Het beleid voor voorwaardelijke toegang kan in dit geval worden toegewezen aan SharePoint, Exchange, of een andere service die wordt gebruikt als een werkbelasting via Microsoft Graph. In dit voorbeeld gaan we wordt ervan uitgegaan dat er is een beleid voor voorwaardelijke toegang voor SharePoint Online.

![App-toegang tot Microsoft Graph-stroomdiagram](./media/conditional-access-dev-guide/app-accessing-microsoft-graph-scenario.png)

De app de eerste keer aanvraagt autorisatie voor Microsoft Graph waarvoor toegang tot een downstream workload zonder voorwaardelijke toegang. De aanvraag is geslaagd zonder het aanroepen van een beleid en de app ontvangt tokens voor Microsoft Graph. De app kan het toegangstoken op dit moment gebruiken in een bearer-aanvraag voor het eindpunt dat is aangevraagd. De app moet nu toegang tot een SharePoint Online-eindpunt van Microsoft Graph, bijvoorbeeld: `https://graph.microsoft.com/v1.0/me/mySite`

De app heeft al een geldig token voor Microsoft Graph, zodat deze de nieuwe aanvraag uitvoeren kunt zonder een nieuw token wordt uitgegeven. Deze aanvraag is mislukt en een uitdaging claims is uitgegeven door Microsoft Graph in de vorm van een HTTP 403-verboden met een ```WWW-Authenticate``` uitdaging.

Hier volgt een voorbeeld van het antwoord:

```
HTTP 403; Forbidden
error=insufficient_claims
www-authenticate="Bearer realm="", authorization_uri="https://login.windows.net/common/oauth2/authorize", client_id="<GUID>", error=insufficient_claims, claims={"access_token":{"polids":{"essential":true,"values":["<GUID>"]}}}"
```

De uitdaging claims bevindt zich in de ```WWW-Authenticate``` koptekst, die kan worden geparseerd om op te halen van de claims-parameter voor de volgende aanvraag. Zodra deze wordt toegevoegd aan de nieuwe aanvraag, Azure AD kent om te evalueren van het beleid voor voorwaardelijke toegang bij de aanmelding bij de gebruiker en de app is nu in overeenstemming met het beleid voor voorwaardelijke toegang. De aanvraag voor het eindpunt van de SharePoint Online herhalende slaagt.

De ```WWW-Authenticate``` header heeft een unieke structuur en is geen pretje om parseren om de waarden ophalen. Hier volgt een korte methode om u te helpen.

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

Voor codevoorbeelden die laten hoe u zien voor het afhandelen van de claims-uitdaging, raadpleegt u de [codevoorbeeld-op-andere gebruikers-of](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof-ca) voor ADAL .NET.

## <a name="scenario-app-performing-the-on-behalf-of-flow"></a>Scenario: Uitvoeren van de stroom op-andere gebruikers-of App

In dit scenario doorlopen we het geval waarin een systeemeigen app een webservice /-API-aanroepen. Op zijn beurt biedt deze service [hij 'op-andere gebruikers-of' stroom voor het aanroepen van een downstream-service. In ons geval we ons beleid voor voorwaardelijke toegang hebt toegepast op de downstream-service (Web API 2) en worden met behulp van een systeemeigen app in plaats van een server/daemon-app.

![Uitvoeren van het diagram van de stroom op-andere gebruikers-of App](./media/conditional-access-dev-guide/app-performing-on-behalf-of-scenario.png)

De eerste tokenaanvraag voor Web-API-1 wordt de eindgebruiker voor multi-factor authentication niet gevraagd, zoals Web-API 1 niet altijd de downstream-API bereikt. Zodra de Web-API 1 probeert om aan te vragen van een token op-andere gebruikers-of de gebruiker voor Web API 2, wordt de aanvraag mislukt, omdat de gebruiker niet is aangemeld met meervoudige verificatie.

Azure AD wordt een HTTP-antwoord met enkele interessante gegevens geretourneerd:

> [!NOTE]
> In dit exemplaar is een beschrijving van de fout met meervoudige verificatie, maar er is een breed scala aan `interaction_required` mogelijk die betrekking hebben op voorwaardelijke toegang.

```
HTTP 400; Bad Request
error=interaction_required
error_description=AADSTS50076: Due to a configuration change made by your administrator, or because you moved to a new location, you must use multi-factor authentication to access '<Web API 2 App/Client ID>'.
claims={"access_token":{"polids":{"essential":true,"Values":["<GUID>"]}}}
```

In de Web-API-1, wordt de fout catch `error=interaction_required`, en terugsturen van de `claims` uitdaging voor de desktop-app. Op dat moment de desktop-app kunt u een nieuwe `acquireToken()` aanroepen en voegen de `claims`uitdaging als een extra queryreeks-parameter. Deze nieuwe aanvraag moet de gebruiker multi-factor Authentication en het verzenden van dit nieuwe token terug naar de Web-API-1 en de stroom op-andere gebruikers-of voltooien.

Als u wilt uitproberen in dit scenario, Zie onze [.NET-codevoorbeeld dat](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof-ca). Hierin wordt beschreven hoe u de claims-uitdaging van Web-API 1 doorgeven aan de systeemeigen app en een nieuwe aanvraag in de client-app maken.

## <a name="scenario-app-accessing-multiple-services"></a>Scenario: App-toegang tot meerdere services

In dit scenario doorlopen we het geval waarbij een web-app toegang heeft tot twee services waarbij een van de toegewezen beleid voor voorwaardelijke toegang heeft. Afhankelijk van uw logische app bestaat een pad op waarin uw app geen toegang nodig is op beide webservices. De volgorde waarin u een token aanvragen speelt in dit scenario is een belangrijke rol in de ervaring van eindgebruikers.

Stel we hebben webservice A en B en B-webservice heeft ons beleid voor voorwaardelijke toegang toegepast. Tijdens de initiële interactieve auth-aanvraag toestemming voor beide services vereist, wordt het beleid voor voorwaardelijke toegang niet in alle gevallen vereist. Als de app een token voor de webservice B vraagt, het beleid wordt aangeroepen en de volgende aanvragen voor de webservice A slaagt ook als volgt.

![App-toegang tot meerdere services stroomdiagram](./media/conditional-access-dev-guide/app-accessing-multiple-services-scenario.png)

U kunt ook als de app wordt in eerste instantie een token voor web service A aanvraagt, wordt de eindgebruiker geen gebruikgemaakt van het beleid voor voorwaardelijke toegang. Hiermee wordt de app-ontwikkelaar om te bepalen de eindgebruiker optreden en niet afdwingen dat het beleid voor voorwaardelijke toegang in alle gevallen worden aangeroepen. De lastigste case is als de app vervolgens een token voor webservice B. vraagt De eindgebruiker moet op dit moment om te voldoen aan het beleid voor voorwaardelijke toegang. Wanneer de app probeert te `acquireToken`, kan deze de volgende fout (weergegeven in het volgende diagram) genereren:

```
HTTP 400; Bad Request
error=interaction_required
error_description=AADSTS50076: Due to a configuration change made by your administrator, or because you moved to a new location, you must use multi-factor authentication to access '<Web API App/Client ID>'.
claims={"access_token":{"polids":{"essential":true,"Values":["<GUID>"]}}}
```

![App-toegang tot meerdere aanvragen van een nieuw token-services](./media/conditional-access-dev-guide/app-accessing-multiple-services-new-token.png)

Als de app van de ADAL-bibliotheek gebruikmaakt, een fout opgetreden bij het verkrijgen van het token wordt altijd opnieuw uitgevoerd interactief. Wanneer deze interactieve aanvraag optreedt, heeft de gebruiker de mogelijkheid om te voldoen aan de voorwaardelijke toegang. Dit is van toepassing, tenzij de aanvraag is een `AcquireTokenSilentAsync` of `PromptBehavior.Never` in dat geval de app nodig heeft om uit te voeren een interactieve ```AcquireToken``` aanvraag voor het gebruik end geven de mogelijkheid om te voldoen aan het beleid.

## <a name="scenario-single-page-app-spa-using-adaljs"></a>Scenario: App met één pagina (SPA) met behulp van ADAL.js

In dit scenario helpen wij bij het geval wanneer we een app met één pagina (SPA) hebben, met behulp van ADAL.js om aan te roepen een voorwaardelijke toegang tot beveiligde web-API. Dit is een eenvoudige architectuur, maar sommige aspecten die worden gehouden moeten bij het ontwikkelen van voorwaardelijke toegang heeft.

In ADAL.js, er zijn enkele functies die tokens verkrijgen: `login()`, `acquireToken(...)`, `acquireTokenPopup(…)`, en `acquireTokenRedirect(…)`.

* `login()` verkrijgt van een ID-token dat door een aanvraag voor interactief aanmelden, maar wordt de toegangstokens voor elke service (met inbegrip van een voorwaardelijke toegang tot beveiligde web-API) niet ophalen.
* `acquireToken(…)` Klik kan op de achtergrond verkrijgen van een toegangstoken wat betekent dat de gebruikersinterface wordt niet weergegeven in elk geval worden gebruikt.
* `acquireTokenPopup(…)` en `acquireTokenRedirect(…)` worden zowel gebruikt voor het interactief aanvragen van een token voor een resource wat betekent dat ze altijd UI voor aanmelden worden weergegeven.

Wanneer een app een toegangstoken moet voor het aanroepen van een Web-API, wordt geprobeerd een `acquireToken(…)`. Als de sessie van het token is verlopen of als we moeten voldoen aan beleid voor voorwaardelijke toegang, dan zal de *acquireToken* functie mislukt en de app gebruikt `acquireTokenPopup()` of `acquireTokenRedirect()`.

![App met één pagina met behulp van ADAL stroomdiagram](./media/conditional-access-dev-guide/spa-using-adal-scenario.png)

We nemen een voorbeeld met een scenario voor onze voorwaardelijke toegang. De eindgebruiker heeft alleen bevindt zich op de site en een sessie. We voeren een `login()` bel, een ID zonder multi-factor authentication-token ophalen. De gebruiker vervolgens komt binnen via een knop die vereist dat de app gegevens van aanvragen van een web-API. De app probeert uit te voeren een `acquireToken()` aanroep mislukt, maar omdat de gebruiker is niet uitgevoerd met meervoudige verificatie nog en behoeften om te voldoen aan het beleid voor voorwaardelijke toegang.

Azure AD stuurt terug de volgende HTTP-antwoord:

```
HTTP 400; Bad Request
error=interaction_required
error_description=AADSTS50076: Due to a configuration change made by your administrator, or because you moved to a new location, you must use multi-factor authentication to access '<Web API App/Client ID>'.
```

Onze app nodig heeft om af te vangen de `error=interaction_required`. De toepassing kan vervolgens worden gebruikt een `acquireTokenPopup()` of `acquireTokenRedirect()` op dezelfde resource. De gebruiker wordt gedwongen om te doen van een multi-factor authentication. Nadat de gebruiker is voltooid de multi-factor authentication, kan de app een nieuwe toegangstoken voor de aangevraagde resource is uitgegeven.

Als u wilt uitproberen in dit scenario, Zie onze [JS beveiligd-wachtwoordverificatie-op-andere gebruikers-of codevoorbeeld](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof-ca). Dit codevoorbeeld maakt gebruik van het beleid voor voorwaardelijke toegang en de web-API die u eerder hebt geregistreerd bij een JS SPA ter illustratie van dit scenario. Het laat zien hoe goed de uitdaging claims verwerken en een toegangstoken die kunnen worden gebruikt voor uw Web-API. U kunt ook de algemene afhandeling [Angular.js-codevoorbeeld](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp) voor hulp bij een Angular beveiligd-WACHTWOORDVERIFICATIE


## <a name="see-also"></a>Zie ook

* Zie voor meer informatie over de mogelijkheden, [voorwaardelijke toegang in Azure Active Directory](../active-directory-conditional-access-azure-portal.md).
* Zie voor meer Azure AD-codevoorbeelden, [GitHub-opslagplaats van codevoorbeelden](https://github.com/azure-samples?utf8=%E2%9C%93&q=active-directory).
* Zie voor meer informatie over de ADAL SDK van en toegang tot de naslagdocumentatie [bibliotheek handleiding](active-directory-authentication-libraries.md).
* Zie voor meer informatie over scenario's met meerdere tenants, [aanmelden van gebruikers met behulp van het patroon voor multitenant](howto-convert-app-to-be-multi-tenant.md).
