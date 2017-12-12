---
title: App-typen voor de Azure Active Directory v2.0-eindpunt | Microsoft Docs
description: De typen apps en scenario's ondersteund door het Azure Active Directory v2.0-eindpunt.
services: active-directory
documentationcenter: 
author: dstrockis
manager: mtillman
editor: 
ms.assetid: 494a06b8-0f9b-44e1-a7a2-d728cf2077ae
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: b0344c1da626a8b4679a632db239fc9ded9d5ce6
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="app-types-for-the-azure-active-directory-v20-endpoint"></a>App-typen voor de Azure Active Directory v2.0-eindpunt
Het Azure Active Directory (Azure AD) v2.0-eindpunt ondersteunt verificatie voor diverse moderne app-architecturen allemaal op basis van industriestandaard-protocollen [OAuth 2.0- of OpenID Connect](active-directory-v2-protocols.md). Dit artikel wordt beschreven welke typen apps die u maken kunt met behulp van Azure AD v2.0, ongeacht uw voorkeurstaal of platform. De informatie in dit artikel is bedoeld om te begrijpen geavanceerde scenario's voordat u [beginnen met werken met de code](active-directory-appmodel-v2-overview.md#getting-started).

> [!NOTE]
> Het v2.0-eindpunt biedt geen ondersteuning voor alle Azure Active Directory-scenario's en onderdelen. Meer informatie over om te bepalen of het v2.0-eindpunt moet worden gebruikt, [v2.0 beperkingen](active-directory-v2-limitations.md).
> 
> 

## <a name="the-basics"></a>De basisbeginselen
U moet registreren elke app die gebruikmaakt van het v2.0-eindpunt in de [Portal voor registratie van Microsoft-toepassing](https://apps.dev.microsoft.com). Het registratieproces app verzamelt en wijst deze waarden voor uw app:

* Een **toepassings-ID** die wordt aangeduid uw app
* Een **omleidings-URI** die u kunt gebruiken om te leiden antwoorden terug naar uw app
* Enkele andere scenariospecifieke waarden

Informatie voor meer informatie over hoe [een app registreren](active-directory-v2-app-registration.md).

Nadat de app is geregistreerd, communiceert de app met Azure AD door verzoeken te sturen naar de Azure AD v2.0-eindpunt. We bieden open source frameworks en bibliotheken waarmee de details van deze aanvragen worden verwerkt. U hebt ook de optie voor het implementeren van de verificatielogica zelf aanvragen voor deze eindpunten te maken:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```
<!-- TODO: Need a page for libraries to link to -->

## <a name="web-apps"></a>Web-apps
Voor web-apps (.NET, PHP, Java, Ruby, Python, knooppunt) dat de gebruiker via een browser gebruikt, kunt u [OpenID Connect](active-directory-v2-protocols.md) voor gebruikersaanmelding. In het OpenID Connect ontvangt de web-app een token ID. Een token ID is een beveiligingstoken dat de identiteit van de gebruiker wordt geverifieerd en bevat informatie over de gebruiker in de vorm van claims:

```
// Partial raw ID token
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImtyaU1QZG1Cd...

// Partial content of a decoded ID token
{
    "name": "John Smith",
    "email": "john.smith@gmail.com",
    "oid": "d9674823-dffc-4e3f-a6eb-62fe4bd48a58"
    ...
}
```

U kunt meer informatie over de typen tokens en claims die beschikbaar zijn voor een app in de [v2.0 tokens verwijzing](active-directory-v2-tokens.md).

In de web server-apps duurt de authenticatiestroom-in deze stappen op hoog niveau:

![Authenticatiestroom voor web-app](../../media/active-directory-v2-flows/convergence_scenarios_webapp.png)

U kunt controleren of de identiteit van de gebruiker door het valideren van het token ID met een openbare ondertekeningssleutel die is ontvangen van het v2.0-eindpunt. Een sessiecookie is ingesteld, die kan worden gebruikt om de gebruiker op de volgende pagina-aanvragen te identificeren.

Overzicht van dit scenario werkt, probeer een van de web-app aanmelden codevoorbeelden die in onze v2.0 [aan de slag](active-directory-appmodel-v2-overview.md#getting-started) sectie.

Naast eenvoudige aanmelden moet een webserver-app mogelijk toegang tot een andere webservice, zoals een REST-API. In dit geval de app web server alleen mogelijk in een gecombineerde OpenID Connect en OAuth 2.0-stroom met behulp van de [OAuth 2.0-autorisatiecodestroom](active-directory-v2-protocols.md). Lees voor meer informatie over dit scenario over [aan de slag met web-apps en Web-API's](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md).

## <a name="web-apis"></a>Web-API's
U kunt het v2.0-eindpunt om webservices, zoals de RESTful Web-API van uw app te beveiligen. Een Web-API gebruikt een toegangstoken OAuth 2.0 in plaats van de ID-tokens en sessiecookies om de gegevens te beveiligen en om inkomende aanvragen te verifiëren. De aanroeper van een Web-API voegt een toegangstoken in de autorisatie-header van een HTTP-verzoek als volgt:

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

De Web-API gebruikt het toegangstoken om de identiteit van de API-aanroeper te verifiëren en informatie over de aanroeper van claims die zijn gecodeerd in het toegangstoken niet uitpakken. Zie voor meer informatie over de typen tokens en claims die beschikbaar voor een app zijn, de [v2.0 tokens verwijzing](active-directory-v2-tokens.md).

Een Web-API gebruikers kunt geven de macht opt-in-of specifieke functionaliteit of gegevens afmelden bij het blootstellen van machtigingen, ook wel bekend als [scopes](active-directory-v2-scopes.md). Voor een app aanroepen te verkrijgen van de machtiging voor een scope, de gebruiker moet toestemming geven aan het bereik tijdens een stroom. Het v2.0-eindpunt wordt de gebruiker om toestemming wordt gevraagd en vervolgens registreert machtigingen in alle toegangstokens die ontvangt van de Web-API. De Web-API valideert de toegangstokens dit op elke aanroep ontvangt en autorisatie controles uitgevoerd.

Een Web-API kan toegangstokens ontvangen van alle typen apps, met inbegrip van server-web-apps, bureaublad en mobiele apps, apps van één pagina, daemons aan serverzijde en zelfs andere Web-API's. De stroom op hoog niveau voor een Web-API ziet er als volgt:

![Web-API-authenticatiestroom](../../media/active-directory-v2-flows/convergence_scenarios_webapi.png)

Bekijk voor meer informatie over hoe u een Web-API beveiligen met behulp van OAuth2-toegangstokens, de codevoorbeelden van Web-API in onze [aan de slag](active-directory-appmodel-v2-overview.md#getting-started) sectie.

In veel gevallen moet de web-API's ook uitgaande aanvragen in andere downstream web-API's die zijn beveiligd door Azure Active Directory aanbrengen.  Om dit te doen, web-API's kan profiteren van Azure AD **op namens** stroom, waardoor de web-API voor het uitwisselen van een binnenkomende toegangstoken voor een andere toegangstoken moet worden gebruikt in uitgaande aanvragen.  Het v2.0-eindpunt van namens stroom wordt beschreven in [details hier](active-directory-v2-protocols-oauth-on-behalf-of.md).

## <a name="mobile-and-native-apps"></a>Mobiele en systeemeigen apps
Apparaat geïnstalleerd apps, zoals mobiele en bureaublad-apps, nodig vaak toegang tot back-endservices of Web-API's die gegevens opslaan en uitvoeren van functies namens een gebruiker. Deze apps kunnen toevoegen aan- en autorisatie aan back-end-services met behulp van de [OAuth 2.0-autorisatiecodestroom](active-directory-v2-protocols-oauth-code.md).

In deze stroom ontvangt de app een autorisatiecode van het v2.0-eindpunt wanneer de gebruiker zich aanmeldt. De autorisatiecode vertegenwoordigt de machtiging van de app aan te roepen back-end-services namens de gebruiker die is aangemeld. De app kan de autorisatiecode op de achtergrond voor een toegangstoken OAuth 2.0 en een vernieuwingstoken uitwisselen. De app kunt gebruiken van het toegangstoken voor verificatie bij de Web-API's in HTTP-aanvragen en het vernieuwingstoken gebruiken om nieuwe toegangstokens oudere toegangstokens verloopt.

![Systeemeigen app verificatiestroom](../../media/active-directory-v2-flows/convergence_scenarios_native.png)

## <a name="single-page-apps-javascript"></a>Apps met één pagina (JavaScript)
Veel moderne apps hebben een front-end app met één pagina die voornamelijk in JavaScript is geschreven. Vaak wordt geschreven met behulp van een framework zoals AngularJS, Ember.js of Durandal.js. Het Azure AD v2.0-eindpunt ondersteunt deze apps met behulp van de [impliciete OAuth 2.0-stroom](active-directory-v2-protocols-implicit.md).

In deze stroom voert de app-tokens ontvangen rechtstreeks vanuit de v2.0 eindpunt, zonder eventuele uitwisselingen server-naar-server te autoriseren. Alle verificatielogica en sessie afhandeling van vindt volledig in de JavaScript-client, zonder extra paginaomleidingen plaatsen.

![Impliciete verificatiestroom](../../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

Overzicht van dit scenario werkt, probeer een van de codevoorbeelden van één pagina app onze [aan de slag](active-directory-appmodel-v2-overview.md#getting-started) sectie.

## <a name="daemons-and-server-side-apps"></a>Daemons en serverzijde apps
Apps die langlopende processen hebben of die werken zonder interactie met een gebruiker moeten ook een manier om toegang te krijgen tot beveiligde bronnen, zoals Web-API's. Deze apps kunnen verifiëren en tokens verkrijgen met behulp van de identiteit van de app in plaats van een gebruiker identiteit met het OAuth 2.0-clientreferentiestroom overgedragen.

In deze stroom voert de app communiceert rechtstreeks met de `/token` eindpunt eindpunten verkrijgen:

![Verificatiestroom daemon-app](../../media/active-directory-v2-flows/convergence_scenarios_daemon.png)

Een daemon-app bouwen, Zie de documentatie van de client-referenties in onze [aan de slag](active-directory-appmodel-v2-overview.md#getting-started) sectie of probeer een [voorbeeld-app voor .NET](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2).
