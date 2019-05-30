---
title: Meer informatie over Microsoft Authentication Library (MSAL) | Azure
description: Microsoft Authentication Library (MSAL) kunnen ontwikkelaars van toepassingen kunnen verkrijgen van tokens om beveiligde Web-API's aanroepen. Deze Web-API's kunnen worden de Microsoft Graph, andere Microsoft-APIS, Web-API's van derden of uw eigen Web-API. MSAL biedt ondersteuning voor meerdere architecturen voor toepassingen en platforms.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/25/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4bd3e7d47b6e3083af6f388a5cd750da240a76b6
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66392891"
---
# <a name="overview-of-microsoft-authentication-library-msal"></a>Overzicht van Microsoft Authentication Library (MSAL)
Microsoft Authentication Library (MSAL) biedt ontwikkelaars de mogelijkheid aan te schaffen [tokens](developer-glossary.md#security-token) van de Microsoft identity-platform-eindpunt voor toegang tot beveiligde Web-API's. Deze Web-API's kunnen worden de Microsoft Graph, andere Microsoft-APIS, Web-API's van derden of uw eigen Web-API. MSAL is beschikbaar voor .NET, JavaScript, Android en iOS-, die ondersteuning bieden voor veel verschillende toepassingsarchitecturen en platforms.

MSAL biedt tal van manieren om op te halen van tokens, met een consistente API voor verschillende platforms. Met MSAL biedt de volgende voordelen:

* Niet nodig om rechtstreeks te gebruiken de OAuth-clientbibliotheken of de code op basis van het protocol in uw toepassing.
* Verkrijgt tokens namens een gebruiker of namens een toepassing (indien van toepassing op het platform).
* Onderhoudt een tokencache en vernieuwen van tokens voor u, wanneer ze zich dicht bij het verlopen. U moet geen token verloopt op uw eigen verwerken.
* Kunt u opgeven welke doelgroep u wilt dat uw toepassing aan te melden bij (uw organisatie, verschillende organisaties, werk en school en persoonlijke Microsoft-accounts, sociale identiteiten met Azure AD B2C, gebruikers in onafhankelijke en nationale clouds).
* Helpt u bij het instellen van uw toepassing vanuit-configuratiebestanden.
* Helpt u uw app op te lossen bij het blootstellen van bruikbare uitzonderingen, logboekregistratie en telemetrie.

## <a name="application-types-and-scenarios"></a>Soorten toepassingen en scenario 's
Met MSAL, een token kan worden verkregen uit een aantal soorten toepassingen: web-toepassingen, web-API's, apps van één pagina (JavaScript), mobiele en systeemeigen toepassingen, en daemons en servertoepassingen. 

MSAL kan worden gebruikt in veel toepassingen-scenario's, waaronder het volgende:

* [Toepassingen met één pagina (JavaScript)](scenario-spa-overview.md) 
* [Web-app aanmelden van gebruikers](scenario-web-app-sign-user-overview.md)
* [Web-App aanmelden met een gebruiker en het namens de gebruiker een web-API aanroepen](scenario-web-app-call-api-overview.md)
* [Een web-API beveiligen zodat alleen geverifieerde gebruikers toegang deze tot hebben](scenario-protected-web-api-overview.md)
* [Web-API aanroepen van een andere downstream web-API namens de aangemelde gebruiker](scenario-web-api-call-api-overview.md)
* [Bureaubladtoepassing aanroepen van een web-API namens de aangemelde gebruiker](scenario-desktop-overview.md)
* [Mobiele toepassing een Web-API aanroept namens de gebruiker die aangemeld interactief is](scenario-mobile-overview.md).
* [Web-API aanroept namens zichzelf bureaublad-service / daemontoepassing](scenario-daemon-overview.md)

## <a name="languages-and-frameworks"></a>Talen en frameworks

| Bibliotheek | Ondersteunde platforms en frameworks|
| --- | --- | 
| ![MSAL.NET](media/sample-v2-code/logo_NET.png) <br/>[MSAL.NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet)| .NET Framework, .NET Core, Xamarin Android, Xamarin iOS, Universal Windows Platform|
| ![MSAL.js](media/sample-v2-code/logo_js.png) <br/>[MSAL.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)| JavaScript/TypeScript-frameworks, zoals AngularJS, Ember.js of Durandal.js|
| ![MSAL voor Android](media/sample-v2-code/logo_Android.png) <br/>[MSAL voor Android (preview)](https://github.com/AzureAD/microsoft-authentication-library-for-android)|Android|
| ![MSAL voor iOS](media/sample-v2-code/logo_iOS.png) <br/>[MSAL.Objective-C (preview)](https://github.com/AzureAD/microsoft-authentication-library-for-objc)|iOS|

## <a name="differences-between-adal-and-msal"></a>Verschillen tussen ADAL en MSAL
Active Directory Authentication Library (ADAL) kan worden geïntegreerd met de Azure AD voor ontwikkelaars (v1.0)-eindpunt, waarbij MSAL kan worden geïntegreerd met het eindpunt van de Microsoft identity-platform (v2.0). Het eindpunt v1.0 biedt ondersteuning voor werkaccounts, maar geen persoonlijke accounts. Het v2.0-eindpunt is combineert persoonlijke Microsoft-accounts en werkaccounts in één verificatiesysteem. Daarnaast met MSAL kan ook krijgt u authenticaties voor Azure AD B2C.

Lees voor meer informatie over [vanaf ADAL.NET migreert naar MSAL.NET](msal-net-migration.md) en [vanaf ADAL.js migreert naar MSAL.js](msal-compare-msal-js-and-adal-js.md).

            