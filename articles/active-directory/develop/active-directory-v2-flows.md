---
title: App-typen voor de Azure Active Directory v2.0-eindpunt | Microsoft Docs
description: De typen apps en scenario's ondersteund door de Azure Active Directory v2.0-eindpunt.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 494a06b8-0f9b-44e1-a7a2-d728cf2077ae
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: bd30776906b615262eb9f51220a0db457227e828
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39579204"
---
# <a name="app-types-for-the-azure-active-directory-v20-endpoint"></a>App-typen voor de Azure Active Directory v2.0-eindpunt

Het v2.0-eindpunt voor Azure Active Directory (Azure AD) biedt ondersteuning voor verificatie voor diverse moderne app-architecturen, allemaal op basis van standaardprotocollen [OAuth 2.0 of OpenID Connect](active-directory-v2-protocols.md). Dit artikel wordt beschreven welke typen apps die u bouwen kunt met behulp van Azure AD v2.0, ongeacht uw voorkeurstaal of het platform. De informatie in dit artikel is bedoeld om te begrijpen van geavanceerde scenario's voordat u [slag gaan met de code](active-directory-appmodel-v2-overview.md#getting-started).

> [!NOTE]
> Het v2.0-eindpunt biedt geen ondersteuning voor alle Azure Active Directory-scenario's en onderdelen. Om te bepalen of het v2.0-eindpunt moet worden gebruikt, lees meer over [v2.0 beperkingen](active-directory-v2-limitations.md).

## <a name="the-basics"></a>De basisbeginselen

U moet zich registreren voor elke app die gebruikmaakt van het v2.0-eindpunt in de [Portal voor Appregistratie Microsoft](https://apps.dev.microsoft.com). Het registratieproces voor de app worden verzameld en deze waarden voor uw app worden toegewezen:

* Een **toepassings-ID** die uw app uniek wordt aangeduid
* Een **omleidings-URI** die u kunt gebruiken om te leiden antwoorden naar uw app terug
* Enkele andere scenariospecifieke waarden

Informatie voor meer informatie over hoe u [een app registreren](quickstart-v2-register-an-app.md).

Nadat de app is geregistreerd, wordt de status van de app communiceert met Azure AD door verzoeken te sturen naar de Azure AD v2.0-eindpunt. We bieden open-source frameworks en bibliotheken die de details van deze aanvragen verwerken. U hebt ook de mogelijkheid voor het implementeren van de verificatielogica voor zelf met het maken van aanvragen voor deze eindpunten:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```
<!-- TODO: Need a page for libraries to link to -->

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

U kunt meer informatie over de typen tokens en claims die beschikbaar zijn voor een app in de [v2.0 tokens verwijzing](v2-id-and-access-tokens.md).

In de web server-apps duurt de verificatiestroom van de aanmelding bij deze stappen op hoog niveau:

![Web-app-verificatiestroom](../../media/active-directory-v2-flows/convergence_scenarios_webapp.png)

U kunt controleren of de identiteit van de gebruiker door het valideren van de ID-token met een openbare ondertekeningssleutel die is ontvangen van het v2.0-eindpunt. Een sessiecookie is ingesteld, die kan worden gebruikt om de gebruiker op de volgende pagina-aanvragen te identificeren.

Als u wilt zien van dit scenario werkt, probeert u een van de web-app aanmelden codevoorbeelden in onze v2.0 [aan de slag](active-directory-appmodel-v2-overview.md#getting-started) sectie.

Naast eenvoudige aanmelding moet een web-app-server mogelijk toegang tot een andere webservice, zoals een REST-API. In dit geval de web server-app maken in een gecombineerde OpenID Connect en OAuth 2.0-stroom via de [OAuth 2.0-autorisatiecodestroom](active-directory-v2-protocols.md). Lees voor meer informatie over dit scenario [aan de slag met web-apps en Web-API's](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md).

## <a name="web-apis"></a>Web-API's
U kunt het v2.0-eindpunt gebruiken om webservices, zoals de RESTful Web-API van uw app te beveiligen. In plaats van ID-tokens en sessiecookies gebruikt een Web-API een OAuth 2.0-toegangstoken om de gegevens te beveiligen en om binnenkomende aanvragen te verifiëren. De aanroeper van een Web-API voegt een toegangstoken in de autorisatie-header van een HTTP-aanvraag, als volgt:

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

De Web-API gebruikt het toegangstoken om de identiteit van de API-aanroeper te verifiëren en om informatie over de aanroeper van claims die zijn gecodeerd in het toegangstoken. Zie voor meer informatie over de typen tokens en claims die beschikbaar voor een app zijn, de [v2.0 tokens verwijzing](v2-id-and-access-tokens.md).

Een Web-API kunt bieden gebruikers de mogelijkheid om te melden voor beveiligingsmeldingen of opt-out voor specifieke functionaliteit of gegevens bij het blootstellen van machtigingen, ook wel bekend als [scopes](v2-permissions-and-consent.md). Voor een aanroepende app aan te schaffen machtiging voor een bereik, de gebruiker moet toestemming geven aan het bereik tijdens een stroom. Het v2.0-eindpunt wordt de gebruiker om toestemming wordt gevraagd en vervolgens registreert machtigingen in alle toegangstokens die de Web-API ontvangt. De Web-API valideert de toegangstokens te geven deze ontvangt op elke aanroep en autorisatie controleert.

Een Web-API kan de toegangstokens ontvangen van alle typen apps, waaronder web server-apps, desktop en mobiele apps, apps van één pagina, daemons serverzijde en aan andere Web-API's. De stroom op hoog niveau voor een Web-API ziet er als volgt:

![Web-API-verificatiestroom](/media/v2-protocols-oidc/convergence_scenarios_webapi.png)

Bekijk voor meer informatie over het beveiligen van een Web-API met behulp van OAuth2-toegangstokens, de Web-API-codevoorbeelden in onze [aan de slag](active-directory-appmodel-v2-overview.md#getting-started) sectie.

In veel gevallen web-API's moet ook uitgaande aanvragen versturen naar andere downstream web-API's die zijn beveiligd door Azure Active Directory. Om dit te doen, web-API's kan profiteren van Azure AD **op namens** stroom, waarmee de web-API voor het uitwisselen van een binnenkomende toegangstoken voor een andere toegangstoken moet worden gebruikt in uitgaande aanvragen. Het v2.0-eindpunt van namens de stroom wordt beschreven in [hier details](v2-oauth2-on-behalf-of-flow.md).

## <a name="mobile-and-native-apps"></a>Mobiele en systeemeigen apps
Apparaat geïnstalleerde apps, zoals mobiele en bureaublad-apps, is vaak nodig hebt voor toegang tot back-endservices of Web-API's die gegevens opslaan en uitvoeren van functies namens een gebruiker. Deze apps kunnen toevoegen aan- en autorisatie aan back-end-services met behulp van de [OAuth 2.0-autorisatiecodestroom](v2-oauth2-auth-code-flow.md).

In deze stroom ontvangt de app een autorisatiecode van het v2.0-eindpunt wanneer de gebruiker zich aanmeldt. De autorisatiecode vertegenwoordigt de machtiging van de app om aan te roepen back-end-services namens de gebruiker die is aangemeld. De app kan de autorisatiecode op de achtergrond voor een OAuth 2.0-toegangstoken en een vernieuwingstoken uitwisselen. De app kunt gebruikt het toegangstoken voor verificatie bij Web-API's in de HTTP-aanvragen en het vernieuwingstoken te gebruiken om nieuwe toegangstokens oudere access tokens verlopen.

![Systeemeigen app verificatiestroom](../../media/active-directory-v2-flows/convergence_scenarios_native.png)

## <a name="single-page-apps-javascript"></a>Apps van één pagina (JavaScript)
Veel moderne apps hebben een app met één pagina front-end die voornamelijk in JavaScript is geschreven. Vaak wordt geschreven met behulp van een framework zoals AngularJS, Ember.js of Durandal.js. De Azure AD v2.0-eindpunt biedt ondersteuning voor deze apps met behulp van de [OAuth 2.0-impliciete stroom](v2-oauth2-implicit-grant-flow.md).

In deze stroom ontvangt de app tokens rechtstreeks vanuit het v2.0-eindpunt, zonder de uitwisseling van een server-naar-server machtigen. Alle verificatielogica en sessie verwerking wordt volledig in de JavaScript-client, zonder extra paginaomleidingen plaatsen.

![Impliciete verificatiestroom](../../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

Als u wilt zien van dit scenario werkt, probeert u een van de app met één pagina codevoorbeelden in onze [aan de slag](active-directory-appmodel-v2-overview.md#getting-started) sectie.

## <a name="daemons-and-server-side-apps"></a>Daemons en serverzijde apps
Apps die langlopende processen hebben of die werken zonder interactie met een gebruiker moeten ook een manier om toegang te krijgen tot beveiligde bronnen, zoals Web-API's. Deze apps kunnen verifiëren en tokens verkrijgen met behulp van de identiteit van de app, in plaats van een gebruiker toegewezen identiteit, met de OAuth 2.0-clientreferentiestroom.

In deze stroom, wordt de app communiceert rechtstreeks met de `/token` eindpunt om op te halen van eindpunten:

![Verificatiestroom daemon-app](../../media/active-directory-v2-flows/convergence_scenarios_daemon.png)

Zie voor het bouwen van een daemon-app, de [clientreferenties documentatie](v2-oauth2-client-creds-grant-flow.md), of probeer een [.NET-voorbeeld-app](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2).
