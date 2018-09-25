---
title: Web-API-apps in Azure Active Directory
description: Hierin wordt beschreven wat web API-Apps zijn en de basisprincipes van stroom protocol, registratie en verlopen van het token voor dit apptype.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: saeeda, jmprieur, andret
ms.custom: aaddev
ms.openlocfilehash: e4ed0db3a08937c3c8b51e2c8af5e566b59df4c4
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46970954"
---
# <a name="web-api"></a>Web-API

Web-API-apps zijn toepassingen die u nodig hebt om resources van een web-API. Er zijn twee identiteitstypen die de web-App gebruiken kunt om te verifiëren en de web-API aanroepen in dit scenario:

- **Toepassings-id** -in dit scenario maakt gebruik van OAuth 2.0-clientreferenties om te verifiëren als de toepassing en toegang tot de web-API. Bij het gebruik van een toepassings-id, de web-API kan alleen worden gedetecteerd dat de web-App wordt aangeroepen, als de web-API heeft niet informatie is ontvangen over de gebruiker. Als de toepassing informatie over de gebruiker ontvangt, wordt verzonden via het toepassingsprotocol en is het niet ondertekend door Azure AD. De web-API vertrouwt die de web-App de gebruiker geverifieerd. Dit patroon wordt een subsysteem van het vertrouwde genoemd om deze reden.
- **Gedelegeerde gebruikersidentiteit** -in dit scenario kan worden uitgevoerd op twee manieren: OpenID Connect en OAuth 2.0-autorisatiecode verlenen aan een vertrouwelijke client. De web-App verkrijgt een toegangstoken voor de gebruiker, blijkt dat de web-API die de gebruiker is geverifieerd aan de web-App en dat de web-App heeft kunnen verkrijgen van een gedelegeerde gebruikersidentiteit in voor het aanroepen van de web-API. Deze toegangstoken wordt verzonden in de aanvraag voor de web-API, die de gebruiker toestaat en retourneert de gewenste resource.

De toepassings-id en de gemachtigde gebruiker-ID-typen worden beschreven in de onderstaande stroom. Het belangrijkste verschil is dat de gedelegeerde gebruikersidentiteit eerst een autorisatiecode verkrijgen moet voordat de gebruiker kan zich aanmelden en toegang tot de web-API krijgen.

## <a name="diagram"></a>Diagram

![Diagram van de Web-API van Web Apps](./media/authentication-scenarios/web_app_to_web_api.png)

## <a name="protocol-flow"></a>Stroom protocol

### <a name="application-identity-with-oauth-20-client-credentials-grant"></a>Toepassings-id met referenties voor OAuth 2.0-client verlenen

1. Een gebruiker is aangemeld bij Azure AD in de web-App (Zie de **Web-apps** sectie voor meer informatie).
1. De web-App moet een toegangstoken verkrijgen, zodat het kan worden geverifieerd bij de web-API en opvragen van de gewenste resource. Wordt een aanvraag naar Azure AD-token-eindpunt met de referentie, toepassings-ID en de web-API-toepassing-ID-URI.
1. Azure AD verifieert de toepassing en retourneert een JWT-toegangstoken dat wordt gebruikt voor de web-API aanroepen.
1. Via HTTPS gebruikt de web-App het geretourneerde JWT-toegangstoken om toe te voegen van de JWT-tekenreeks met een aanduiding 'Bearer' in de autorisatie-header van de aanvraag naar de web-API. De web-API valideert de JWT-token, en als validatie gelukt is, geeft de gewenste resource.

### <a name="delegated-user-identity-with-openid-connect"></a>Gedelegeerde gebruikersidentiteit met OpenID Connect

1. Een gebruiker is aangemeld bij een web-App met behulp van Azure AD (Zie de [Web Browser webtoepassing](#web-browser-to-web-application) hierboven). Als de gebruiker van de web-App heeft nog niet toegestaan dat zodat de webtoepassing naar de web-API aanroepen namens, wordt de gebruiker moet toestemming geven. De machtigingen die vereist is door de toepassing wordt weergegeven, en als een van deze machtigingen op administrator-niveau, een normale gebruiker in de map niet mogelijk om in te stemmen. Dit proces toestemming geldt alleen voor toepassingen met meerdere tenants, niet één tenant-toepassingen, zoals de toepassing beschikt al over de vereiste machtigingen. Wanneer de gebruiker is aangemeld, heeft een ID-token met informatie over de gebruiker, evenals een autorisatiecode ontvangen in de web-App.
1. Met behulp van de autorisatiecode dat is uitgegeven door Azure AD, verzendt de web-App een aanvraag naar Azure AD-token-eindpunt met de autorisatiecode, informatie over de clienttoepassing (toepassings-ID en omleidings-URI) en de gewenste resource (toepassings-ID De URI voor de web-API).
1. De autorisatiecode en informatie over de web-App en web-API worden gevalideerd door Azure AD. Na de validatie is geslaagd, Azure AD twee tokens retourneert: een JWT-toegangstoken en een JWT-vernieuwingstoken.
1. Via HTTPS gebruikt de web-App het geretourneerde JWT-toegangstoken om toe te voegen van de JWT-tekenreeks met een aanduiding 'Bearer' in de autorisatie-header van de aanvraag naar de web-API. De web-API valideert de JWT-token, en als validatie gelukt is, geeft de gewenste resource.

### <a name="delegated-user-identity-with-oauth-20-authorization-code-grant"></a>Gedelegeerde gebruikersidentiteit met OAuth 2.0-autorisatiecode verlenen

1. Een gebruiker is al aangemeld bij een webtoepassing, waarvan verificatiemechanisme onafhankelijk van Azure AD is.
1. De web-App is een autorisatiecode voor een toegangstoken verkrijgen, zodat deze Hiermee wordt een aanvraag via de browser naar Azure AD autorisatie-eindpunt, bieden de toepassings-ID en omleidings-URI voor de web-App na een geslaagde authenticatie vereist. De gebruiker zich aanmeldt bij Azure AD.
1. Als de gebruiker van de web-App heeft nog niet toegestaan dat zodat de webtoepassing naar de web-API aanroepen namens, wordt de gebruiker moet toestemming geven. De machtigingen die vereist is door de toepassing wordt weergegeven, en als een van deze machtigingen op administrator-niveau, een normale gebruiker in de map niet mogelijk om in te stemmen. Deze toestemming is van toepassing op zowel één als multitenant-toepassing. In het geval één tenant, kan een beheerder beheerder toestemming voor toestemming uitvoeren namens de gebruikers. Dit kan worden gedaan met behulp van de `Grant Permissions` knop in de [Azure-portal](https://portal.azure.com). 
1. Nadat de gebruiker heeft ingestemd, ontvangt de webtoepassing de autorisatiecode die nodig zijn voor een toegangstoken verkrijgen.
1. Met behulp van de autorisatiecode dat is uitgegeven door Azure AD, verzendt de web-App een aanvraag naar Azure AD-token-eindpunt met de autorisatiecode, informatie over de clienttoepassing (toepassings-ID en omleidings-URI) en de gewenste resource (toepassings-ID De URI voor de web-API).
1. De autorisatiecode en informatie over de web-App en web-API worden gevalideerd door Azure AD. Na de validatie is geslaagd, Azure AD twee tokens retourneert: een JWT-toegangstoken en een JWT-vernieuwingstoken.
1. Via HTTPS gebruikt de web-App het geretourneerde JWT-toegangstoken om toe te voegen van de JWT-tekenreeks met een aanduiding 'Bearer' in de autorisatie-header van de aanvraag naar de web-API. De web-API valideert de JWT-token, en als validatie gelukt is, geeft de gewenste resource.

## <a name="code-samples"></a>Codevoorbeelden

Zie de codevoorbeelden voor Web-App naar Web-API-scenario's. En kom regelmatig terug--nieuwe voorbeelden worden regelmatig toegevoegd. Web [toepassing naar Web-API](sample-v1-code.md#web-applications-signing-in-users-calling-microsoft-graph-or-a-web-api-with-the-users-identity).

## <a name="app-registration"></a>App-registratie

Zie voor het registreren van een toepassing met de Azure AD v1.0-eindpunt, [een app registreren bij de Azure AD-eindpunt v1.0](quickstart-v1-add-azure-ad-app.md).

* Één tenant - voor de toepassings-id en gemachtigde gebruiker identiteit gevallen, de web-App en de web-API moet worden geregistreerd in dezelfde map in Azure AD. De web-API kan worden geconfigureerd als een reeks machtigingen die worden gebruikt voor het beperken van toegang tot de bronnen van de web-App beschikbaar wilt maken. Als een gemachtigde gebruiker-ID-type wordt gebruikt, de web-App dient te selecteren van de gewenste machtigingen van de **machtigingen voor andere toepassingen** vervolgkeuzelijst in de Azure-portal. Deze stap is niet vereist als het toepassingstype identiteit wordt gebruikt.
* Multitenant-eerst de webtoepassing is geconfigureerd om aan te geven van de machtigingen die nodig is voor functioneel. Deze lijst met vereiste machtigingen in een dialoogvenster weergegeven wanneer een gebruiker of beheerder in de doelmap toestemming geeft voor de toepassing, waardoor het beschikbaar is voor hun organisatie. Sommige toepassingen vereisen alleen machtigingen op gebruikersniveau, waar een gebruiker in de organisatie toestemming kunt geven. Andere toepassingen vereist beheerdersrechten, die een gebruiker in de organisatie kan niet met instemmen zijn. Alleen een directory-beheerder kan toestemming geven voor toepassingen waarvoor dit niveau van machtigingen. Wanneer de gebruiker of beheerder hiermee akkoord gaat, de web-App en de web-API zijn beide geregistreerd in de directory.

## <a name="token-expiration"></a>Geldigheidsduur van het token

Wanneer de web-App de autorisatiecode gebruikt om te profiteren van een JWT-token, ontvangt deze ook een JWT-vernieuwingstoken. Wanneer het toegangstoken is verlopen, kan het vernieuwingstoken dat kan worden gebruikt voor het verifiëren van de gebruiker zonder dat ze zich opnieuw aanmelden. Dit vernieuwingstoken wordt vervolgens gebruikt voor verificatie van de gebruiker, wat resulteert in een nieuw toegangstoken en een vernieuwingstoken.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over andere [toepassingstypen en scenario's](app-types.md)
- Meer informatie over de Azure AD [de basisbeginselen van verificatie](authentication-scenarios.md)