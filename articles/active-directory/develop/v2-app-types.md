---
title: Soorten toepassingen voor Microsoft identity-platform | Azure
description: De typen apps en scenario's die worden ondersteund door het eindpunt van Microsoft identity-platform (v2.0).
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 494a06b8-0f9b-44e1-a7a2-d728cf2077ae
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/06/2019
ms.author: celested
ms.reviewer: saeeda, jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 562b45f30bf057feba38a716e040ab2efc15da90
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60297093"
---
# <a name="application-types-for-microsoft-identity-platform"></a>Soorten toepassingen voor Microsoft identity-platform

Het eindpunt van de Microsoft identity-platform (v2.0) biedt ondersteuning voor verificatie voor diverse moderne app-architecturen, allemaal op basis van standaardprotocollen [OAuth 2.0 of OpenID Connect](active-directory-v2-protocols.md). Dit artikel wordt beschreven welke typen apps die u bouwen kunt met behulp van Microsoft identity-platform, ongeacht uw voorkeurstaal of het platform. De informatie is bedoeld om u inzicht in geavanceerde scenario's voordat u te helpen [slag gaan met de code](v2-overview.md#getting-started).

> [!NOTE]
> Het eindpunt van de Microsoft identity-platform biedt geen ondersteuning voor alle Azure Active Directory (Azure AD)-scenario's en onderdelen. Meer informatie over om te bepalen of moet u het eindpunt van de Microsoft identity-platform, [beperkingen van het Microsoft identity platform](active-directory-v2-limitations.md).

## <a name="the-basics"></a>De basisbeginselen

U moet zich registreren voor elke app die gebruikmaakt van het eindpunt van de Microsoft identity-platform in de nieuwe [portal voor App-registraties](https://go.microsoft.com/fwlink/?linkid=2083908). Het registratieproces voor de app worden verzameld en deze waarden voor uw app worden toegewezen:

* Een **(client) toepassings-ID** die uw app uniek wordt aangeduid
* Een **omleidings-URI** die u kunt gebruiken om te leiden antwoorden naar uw app terug
* Enkele andere scenariospecifieke waarden, zoals ondersteund accounttypen

Informatie voor meer informatie over hoe u [een app registreren](quickstart-register-app.md).

Nadat de app is geregistreerd, wordt de status van de app communiceert met Microsoft identity-platform door verzoeken te sturen naar het eindpunt. We bieden open-source frameworks en bibliotheken die de details van deze aanvragen verwerken. U hebt ook de mogelijkheid voor het implementeren van de verificatielogica voor zelf met het maken van aanvragen voor deze eindpunten:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

## <a name="single-page-apps-javascript"></a>Apps van één pagina (JavaScript)

Veel moderne apps hebben een app met één pagina front-end die voornamelijk in JavaScript is geschreven. Vaak wordt geschreven met behulp van een framework zoals AngularJS, Ember.js of Durandal.js. Het eindpunt van de Microsoft identity-platform biedt ondersteuning voor deze apps met behulp van de [OAuth 2.0-impliciete stroom](v2-oauth2-implicit-grant-flow.md).

In deze stroom ontvangt de app tokens rechtstreeks vanuit het Microsoft identity-platform autorisatie-eindpunt, zonder alle server-naar-server worden uitgewisseld. Alle verificatielogica en sessie verwerking wordt volledig in de JavaScript-client, zonder extra paginaomleidingen plaatsen.

![Impliciete verificatiestroom](./media/v2-app-types/convergence-scenarios-implicit.svg)

Als u wilt zien van dit scenario werkt, probeert u een van de app met één pagina codevoorbeelden in de [identiteitsplatform van Microsoft aan de slag](v2-overview.md#getting-started) sectie.

## <a name="web-apps"></a>Web-apps

Voor web-apps (.NET, PHP, Java, Ruby, Python, knooppunt) die de gebruiker toegang heeft tot via een browser, kunt u [OpenID Connect](active-directory-v2-protocols.md) voor aanmelden van gebruikers. In de OpenID Connect ontvangt de web-app een ID-token. Een ID-token is een beveiligingstoken dat de identiteit van de gebruiker controleert en bevat informatie over de gebruiker in de vorm van claims:

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

Meer informatie over de verschillende typen tokens die worden gebruikt in het eindpunt van de Microsoft identity-platform zijn beschikbaar in de [toegangstoken](access-tokens.md) verwijzing en [id_token verwijzing](id-tokens.md)

In de web server-apps duurt de verificatiestroom van de aanmelding bij deze stappen op hoog niveau:

![Web-app-verificatiestroom](./media/v2-app-types/convergence-scenarios-webapp.svg)

U kunt controleren of de identiteit van de gebruiker door het valideren van de ID-token met een openbare ondertekeningssleutel die is ontvangen van het eindpunt van de Microsoft identity-platform. Een sessiecookie is ingesteld, die kan worden gebruikt om de gebruiker op de volgende pagina-aanvragen te identificeren.

Als u wilt zien van dit scenario werkt, probeert u een van de web-app aanmelden codevoorbeelden in de [identiteitsplatform van Microsoft aan de slag](v2-overview.md#getting-started) sectie.

Naast eenvoudige aanmelding moet een web-app-server mogelijk toegang tot een andere webservice, zoals een REST-API. In dit geval de web server-app maken in een gecombineerde OpenID Connect en OAuth 2.0-stroom via de [OAuth 2.0-autorisatiecodestroom](active-directory-v2-protocols.md). Lees voor meer informatie over dit scenario [aan de slag met web-apps en Web-API's](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md).

## <a name="web-apis"></a>Web-API's

U kunt het eindpunt van de Microsoft identity-platform gebruiken om webservices, zoals de RESTful Web-API van uw app te beveiligen. In plaats van ID-tokens en sessiecookies gebruikt een Web-API een OAuth 2.0-toegangstoken om de gegevens te beveiligen en om binnenkomende aanvragen te verifiëren. De aanroeper van een Web-API voegt een toegangstoken in de autorisatie-header van een HTTP-aanvraag, als volgt:

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

De Web-API gebruikt het toegangstoken om de identiteit van de API-aanroeper te verifiëren en om informatie over de aanroeper van claims die zijn gecodeerd in het toegangstoken. Meer informatie over de verschillende typen tokens die worden gebruikt in het eindpunt van de Microsoft identity-platform zijn beschikbaar in de [toegangstoken](access-tokens.md) verwijzing en [id_token verwijzing](id-tokens.md)

Een Web-API kunt bieden gebruikers de mogelijkheid om te melden voor beveiligingsmeldingen of opt-out voor specifieke functionaliteit of gegevens bij het blootstellen van machtigingen, ook wel bekend als [scopes](v2-permissions-and-consent.md). Voor een aanroepende app aan te schaffen machtiging voor een bereik, de gebruiker moet toestemming geven aan het bereik tijdens een stroom. Het eindpunt van de Microsoft identity-platform wordt de gebruiker om toestemming wordt gevraagd en legt vervolgens machtigingen in alle toegangstokens die de Web-API ontvangt. De Web-API valideert de toegangstokens te geven deze ontvangt op elke aanroep en autorisatie controleert.

Een Web-API kan de toegangstokens ontvangen van alle typen apps, waaronder web server-apps, desktop en mobiele apps, apps van één pagina, daemons serverzijde en aan andere Web-API's. De stroom op hoog niveau voor een Web-API ziet er als volgt:

![Web-API-verificatiestroom](./media/v2-app-types/convergence-scenarios-webapi.svg)

Bekijk voor meer informatie over het beveiligen van een Web-API met behulp van OAuth2-toegangstokens, de Web-API-codevoorbeelden in de [identiteitsplatform van Microsoft aan de slag](v2-overview.md#getting-started) sectie.

In veel gevallen web-API's moet ook uitgaande aanvragen versturen naar andere downstream web-API's die zijn beveiligd door Microsoft identity-platform. Om dit te doen, web-API's kan profiteren van de **op-andere gebruikers-Of** stroom, waarmee de web-API voor het uitwisselen van een binnenkomende toegangstoken voor een andere toegangstoken moet worden gebruikt in uitgaande aanvragen. Zie voor meer informatie, [identiteitsplatform van Microsoft en OAuth 2.0 namens-stroom](v2-oauth2-on-behalf-of-flow.md).

## <a name="mobile-and-native-apps"></a>Mobiele en systeemeigen apps

Apparaat geïnstalleerde apps, zoals mobiele en bureaublad-apps, is vaak nodig hebt voor toegang tot back-endservices of Web-API's die gegevens opslaan en uitvoeren van functies namens een gebruiker. Deze apps kunnen toevoegen aan- en autorisatie aan back-end-services met behulp van de [OAuth 2.0-autorisatiecodestroom](v2-oauth2-auth-code-flow.md).

In deze stroom ontvangt de app een autorisatiecode van het eindpunt van de Microsoft identity platform wanneer de gebruiker zich aanmeldt. De autorisatiecode vertegenwoordigt de machtiging van de app om aan te roepen back-end-services namens de gebruiker die is aangemeld. De app kan de autorisatiecode op de achtergrond voor een OAuth 2.0-toegangstoken en een vernieuwingstoken uitwisselen. De app kunt gebruikt het toegangstoken voor verificatie bij Web-API's in de HTTP-aanvragen en het vernieuwingstoken te gebruiken om nieuwe toegangstokens oudere access tokens verlopen.

![Systeemeigen app verificatiestroom](./media/v2-app-types/convergence-scenarios-native.svg)

## <a name="daemons-and-server-side-apps"></a>Daemons en serverzijde apps

Apps die langlopende processen hebben of die werken zonder interactie met een gebruiker moeten ook een manier om toegang te krijgen tot beveiligde bronnen, zoals Web-API's. Deze apps kunnen verifiëren en tokens verkrijgen met behulp van de identiteit van de app, in plaats van een gebruiker toegewezen identiteit, met de OAuth 2.0-clientreferentiestroom. U kunt de app identiteit bevestigen met een clientgeheim of een certificaat. Zie voor meer informatie, [verifiëren met Microsoft identity-platform in daemon-apps met certificaten](https://azure.microsoft.com/resources/samples/active-directory-dotnet-daemon-certificate-credential/).

In deze stroom, wordt de app communiceert rechtstreeks met de `/token` eindpunt om toegang te krijgen:

![Verificatiestroom daemon-app](./media/v2-app-types/convergence-scenarios-daemon.svg)

Zie voor het bouwen van een daemon-app, de [clientreferenties documentatie](v2-oauth2-client-creds-grant-flow.md), of probeer een [.NET-voorbeeld-app](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2).
