---
title: Service-naar-service-apps in Azure Active Directory
description: Hierin wordt beschreven welke service-naar-service-toepassingen en de basisprincipes van stroom protocol, registratie en verlopen van het token voor dit apptype.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: saeeda, jmprieur, andret
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: e0ced89ce97d5f22270d9968fdeb0ddb3fad1e4e
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56194985"
---
# <a name="service-to-service-apps"></a>Service-naar-service-apps

Service-naar-service-toepassingen kunnen een daemon of server-toepassing die resources ophalen uit een web-API moet worden. Er zijn twee subregio's die betrekking hebben op deze sectie:

- Een daemon uit die nodig zijn om aan te roepen een web-API, die is gebouwd op type referenties toekenning van OAuth 2.0-client

    In dit scenario is het belangrijk om te begrijpen van een aantal dingen. Eerst is tussenkomst van de gebruiker niet mogelijk is met een daemon-toepassing waarvoor u de toepassing een eigen identiteit hebben. Een voorbeeld van een daemontoepassing is een batch-taak, of de service van een besturingssysteem op de achtergrond uitgevoerd. Dit type toepassing vraagt een toegangstoken met behulp van de toepassings-id en presenteert de toepassings-ID, de referentie (wachtwoord of certificaat) en de toepassing-ID-URI naar Azure AD. Na een geslaagde authenticatie ontvangt de daemon voor een toegangstoken van Azure AD, dat vervolgens wordt gebruikt voor het aanroepen van de web-API.

- Een servertoepassing (zoals een web-API) die nodig heeft om aan te roepen een web-API, die is gebouwd op namens van OAuth 2.0-concept-specificatie

    Stel dat een gebruiker is geverifieerd op een systeemeigen toepassing, en deze systeemeigen toepassing moet een web-API aanroepen in dit scenario. Azure AD geeft een JWT-toegangstoken voor het aanroepen van de web-API. Als de web-API een andere downstream web-API aanroepen moet, kan deze de stroom op-andere gebruikers-of gebruik het overdragen van de identiteit van de gebruiker en worden geverifieerd bij de tweede laag web-API.

## <a name="diagram"></a>Diagram

![Daemon of servertoepassing naar Web-API-diagram](./media/authentication-scenarios/daemon_server_app_to_web_api.png)

## <a name="dprotocol-flow"></a>DProtocol stroom

### <a name="application-identity-with-oauth-20-client-credentials-grant"></a>Toepassings-id met referenties voor OAuth 2.0-client verlenen

1. De servertoepassing moet eerst worden geverifieerd bij Azure AD als zelf, zonder menselijke tussenkomst, zoals een dialoogvenster voor interactieve aanmelding. Wordt een aanvraag naar Azure AD-token-eindpunt met de referenties, toepassings-ID en toepassings-ID-URI.
1. Azure AD verifieert de toepassing en retourneert een JWT-toegangstoken dat wordt gebruikt voor de web-API aanroepen.
1. Via HTTPS gebruikt de web-App het geretourneerde JWT-toegangstoken om toe te voegen van de JWT-tekenreeks met een aanduiding 'Bearer' in de autorisatie-header van de aanvraag naar de web-API. De web-API valideert de JWT-token, en als validatie gelukt is, geeft de gewenste resource.

### <a name="delegated-user-identity-with-oauth-20-on-behalf-of-draft-specification"></a>Gedelegeerde gebruikersidentiteit met OAuth 2.0 namens concept-specificatie

De stroom die hierna wordt ervan uitgegaan dat een gebruiker is geverifieerd op een andere toepassing (zoals een systeemeigen toepassing) en de identiteit van de gebruiker is gebruikt voor het verkrijgen van een toegangstoken uit aan de web-API van de eerste laag.

1. Het toegangstoken verzendt de systeemeigen toepassing naar de web-API van de eerste laag.
1. De eerste laag web-API verzendt een aanvraag naar Azure AD-token-eindpunt biedt de toepassings-ID en referenties, evenals het toegangstoken van de gebruiker. Bovendien de aanvraag is verzonden met een on_behalf_of parameter die aangeeft van de web API nieuwe tokens voor het aanroepen van een downstream web-API namens de oorspronkelijke gebruiker wordt aangevraagd.
1. Azure AD verifieert dat de eerste laag web-API heeft machtigingen voor toegang tot de tweede laag web-API en valideert de aanvraag, retourneren een JWT-toegangstoken en vernieuwen van een JWT-token naar de web-API voor de eerste laag.
1. Via HTTPS roept de eerste laag web-API voor de web-API van de tweede laag vervolgens door de token tekenreeks in de autorisatie-header in de aanvraag toe te voegen. De eerste laag web-API kunt doorgaan met het aanroepen van de tweede laag web-API, zolang het toegangstoken en vernieuwen van tokens geldig zijn.

## <a name="code-samples"></a>Codevoorbeelden

Zie de codevoorbeelden voor Daemon of servertoepassing naar Web-API-scenario's. En controleer regelmatig back-wanneer er nieuwe voorbeelden worden regelmatig toegevoegd. [Server- of Daemon-toepassing naar Web-API](sample-v1-code.md#daemon-applications-accessing-web-apis-with-the-applications-identity)

## <a name="app-registration"></a>App-registratie

* Één tenant - voor de toepassings-id en de gemachtigde gebruiker identiteit gevallen de daemon of server-toepassing moet zijn geregistreerd in dezelfde map in Azure AD. De web-API kan worden geconfigureerd als een reeks machtigingen die worden gebruikt voor het beperken van de daemon of van de server toegang tot de resources beschikbaar wilt maken. Als een gemachtigde gebruiker-ID-type wordt gebruikt, moet de servertoepassing Selecteer de gewenste machtigingen in de vervolgkeuzelijst 'Machtigingen voor andere toepassingen' in de Azure-portal. Deze stap is niet vereist als het toepassingstype identiteit wordt gebruikt.
* Multitenant- en de toepassing daemon of de server is geconfigureerd om aan te geven van de machtigingen die nodig is voor functioneel. Deze lijst met vereiste machtigingen in een dialoogvenster weergegeven wanneer een gebruiker of beheerder in de doelmap toestemming geeft voor de toepassing, waardoor het beschikbaar is voor hun organisatie. Sommige toepassingen vereisen alleen machtigingen op gebruikersniveau, waar een gebruiker in de organisatie toestemming kunt geven. Andere toepassingen vereist beheerdersrechten, die een gebruiker in de organisatie kan niet met instemmen zijn. Alleen een directory-beheerder kan toestemming geven voor toepassingen waarvoor dit niveau van machtigingen. Wanneer de gebruiker of beheerder hiermee akkoord gaat, worden beide van de web-API's in de directory geregistreerd.

## <a name="token-expiration"></a>Geldigheidsduur van het token

Wanneer de eerste toepassing gebruikmaakt van de autorisatiecode ophalen een toegang JWT-token, ontvangt deze ook een JWT-vernieuwingstoken. Wanneer het toegangstoken is verlopen, kan het vernieuwingstoken dat kan worden gebruikt voor het opnieuw verifiëren zonder te vragen om referenties. Dit vernieuwingstoken wordt vervolgens gebruikt voor verificatie van de gebruiker, wat resulteert in een nieuw toegangstoken en een vernieuwingstoken.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over andere [toepassingstypen en scenario's](app-types.md)
- Meer informatie over de Azure AD [de basisbeginselen van verificatie](authentication-scenarios.md)
