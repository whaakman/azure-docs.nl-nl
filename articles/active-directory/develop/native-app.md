---
title: Systeemeigen apps in Azure Active Directory
description: Hierin wordt beschreven welke systeemeigen apps en de basisbeginselen van stroom protocol, registratie en verlopen van het token voor dit apptype.
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
ms.openlocfilehash: af3c75364346a1e2e91c0c6d942df5c47b16ffed
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55096650"
---
# <a name="native-apps"></a>Systeemeigen apps

Systeemeigen apps zijn toepassingen die namens een gebruiker een web-API aanroepen. In dit scenario is gebaseerd op het machtigingstype van OAuth 2.0 machtiging code met een openbare client, zoals beschreven in sectie 4.1 van de [OAuth 2.0-specificatie](https://tools.ietf.org/html/rfc6749). De systeemeigen toepassing verkrijgt een toegangstoken voor de gebruiker met behulp van de OAuth 2.0-protocol. Deze toegangstoken wordt verzonden in de aanvraag naar de web-API, die de gebruiker toestaat en retourneert de gewenste resource.

## <a name="diagram"></a>Diagram

![Systeemeigen toepassing voor de Web-API-Diagram](./media/authentication-scenarios/native_app_to_web_api.png)

## <a name="protocol-flow"></a>Stroom protocol

Als u van de AD-Verificatiebibliotheken gebruikmaakt, worden de meeste van de hieronder beschreven informatie voor het protocol voor u, zoals de browser pop-upvenster token opslaan in cache en de verwerking van vernieuwingstokens verwerkt.

1. Met behulp van een browser pop, dat de systeemeigen toepassing een aanvraag doet bij het autorisatie-eindpunt in Azure AD. Deze aanvraag bevat de toepassings-ID en de omleidings-URI van de systeemeigen toepassing, zoals wordt weergegeven in de Azure-portal en de toepassings-ID-URI voor de web-API. Als de gebruiker nog niet is aangemeld, wordt ze gevraagd zich opnieuw aanmelden
1. Azure AD verifieert de gebruiker. Als het is een multitenant-toepassing en toestemming vereist voor het gebruik van de toepassing is, de gebruiker moet toestemming geven als ze nog niet hebt gedaan. Na de toestemming verlenen en bij een geslaagde verificatie, problemen met Azure AD een autorisatie code-antwoord terug naar de clienttoepassing omleidings-URI.
1. Wanneer Azure AD een autorisatie code-antwoord terug naar de omleidings-URI geeft, wordt de clienttoepassing stopt tussenkomst van de browser en extraheert de autorisatiecode uit het antwoord. Met deze autorisatiecode, verzendt de clienttoepassing een aanvraag naar Azure AD-token-eindpunt heeft met de autorisatiecode, details over de clienttoepassing (toepassings-ID en omleidings-URI) en de gewenste resource (toepassings-ID-URI voor de Web-API).
1. De autorisatiecode en informatie over de client- en web-API worden gevalideerd door Azure AD. Na de validatie is geslaagd, Azure AD twee tokens retourneert: een JWT-toegangstoken en een JWT-vernieuwingstoken. Bovendien Azure AD als resultaat geeft algemene informatie over de gebruiker, zoals hun weergave naam en het tenant-ID.
1. Via HTTPS gebruikt de clienttoepassing het geretourneerde JWT-toegangstoken om toe te voegen van de JWT-tekenreeks met een aanduiding 'Bearer' in de autorisatie-header van de aanvraag naar de web-API. De web-API valideert de JWT-token, en als validatie gelukt is, geeft de gewenste resource.
1. Wanneer het toegangstoken is verlopen, is de clienttoepassing ontvangt een foutbericht weergegeven dat de gebruiker moet opnieuw worden geverifieerd. Als de toepassing een ongeldig vernieuwingstoken heeft, kan deze worden gebruikt om een nieuw toegangstoken verkrijgen zonder tussenkomst van de gebruiker zich opnieuw aanmelden. Als het vernieuwingstoken is verlopen, moet de toepassing interactief verifiëren van de gebruiker opnieuw.

> [!NOTE]
> Het vernieuwingstoken dat is uitgegeven door Azure AD kan worden gebruikt voor toegang tot meerdere resources. Bijvoorbeeld, hebt u een clienttoepassing die gemachtigd is voor het aanroepen van twee web-API's, kan het vernieuwingstoken dat kan worden gebruikt voor een toegangstoken aan de andere web-API ook ophalen.

## <a name="code-samples"></a>Codevoorbeelden

Zie de codevoorbeelden voor systeemeigen toepassing voor de Web-API-scenario's. En kom regelmatig terug--we nieuwe voorbeelden vaak toevoegen. [Systeemeigen toepassing voor de Web-API](sample-v1-code.md#desktop-and-mobile-public-client-applications-calling-microsoft-graph-or-a-web-api).

## <a name="app-registration"></a>App-registratie

Zie voor het registreren van een toepassing met de Azure AD v1.0-eindpunt, [een app registreren bij de Azure AD-eindpunt v1.0](quickstart-v1-add-azure-ad-app.md).

* Één tenant - de systeemeigen toepassing en de web-API moet worden geregistreerd in dezelfde map in Azure AD. De web-API kan worden geconfigureerd als een reeks machtigingen die worden gebruikt voor het beperken van toegang tot de bronnen van de systeemeigen toepassing beschikbaar wilt maken. De clienttoepassing vervolgens selecteert de gewenste machtigingen in het menu van de vervolgkeuzelijst 'Machtigingen voor andere toepassingen' in de Azure-portal.
* Multitenant - eerst de systeemeigen toepassing slechts geregistreerd in de ontwikkelaar of in de map van de uitgever. Ten tweede is de systeemeigen toepassing geconfigureerd om aan te geven van de machtigingen die nodig is voor functioneel. Deze lijst met vereiste machtigingen in een dialoogvenster weergegeven wanneer een gebruiker of beheerder in de doelmap toestemming geeft voor de toepassing, waardoor het beschikbaar is voor hun organisatie. Sommige toepassingen vereisen alleen machtigingen op gebruikersniveau, waar een gebruiker in de organisatie toestemming kunt geven. Andere toepassingen vereist beheerdersrechten, die een gebruiker in de organisatie kan niet met instemmen zijn. Alleen een directory-beheerder kan toestemming geven voor toepassingen waarvoor dit niveau van machtigingen. Wanneer de gebruiker of beheerder hiermee akkoord gaat, worden alleen de web-API is geregistreerd in de directory. 

## <a name="token-expiration"></a>Geldigheidsduur van het token

Wanneer de systeemeigen toepassing maakt gebruik van de autorisatiecode ophalen een toegang JWT-token, ontvangt deze ook een JWT-vernieuwingstoken. Wanneer het toegangstoken is verlopen, kan het vernieuwingstoken dat kan worden gebruikt voor het opnieuw verifiëren zonder dat ze zich opnieuw aanmelden. Dit vernieuwingstoken wordt vervolgens gebruikt voor verificatie van de gebruiker, wat resulteert in een nieuw toegangstoken en een vernieuwingstoken.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over andere [toepassingstypen en scenario's](app-types.md)
- Meer informatie over de Azure AD [de basisbeginselen van verificatie](authentication-scenarios.md)
