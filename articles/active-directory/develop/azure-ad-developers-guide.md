---
title: Azure Active Directory voor ontwikkelaars | Microsoft Docs
description: Dit artikel geeft een overzicht van het aanmelden bij werk- en schoolaccounts van Microsoft met Azure Active Directory.
services: active-directory
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 5c872c89-ef04-4f4c-98de-bc0c7460c7c2
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/30/2018
ms.author: celested
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 6c3148f6ca9e538d2fed3467ba5d2129e123c7fd
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39502281"
---
# <a name="azure-active-directory-for-developers"></a>Azure Active Directory voor ontwikkelaars

Azure Active Directory (Azure AD) is een identiteitsservice in de cloud waarmee ontwikkelaars apps kunnen maken die ervoor zorgen dat elke gebruiker met een Microsoft-werkaccount of -schoolaccount zich veilig kan aanmelden. Azure AD biedt ondersteuning voor ontwikkelaars die Line-Of-Business-apps met één tenant maken en voor ontwikkelaars die apps met meerdere tenants willen ontwikkelen. Naast eenvoudige aanmelding maakt Azure AD ook mogelijk dat apps Microsoft-API's zoals [Microsoft Graph](https://developer.microsoft.com/en-us/graph/docs/concepts/overview) en aangepaste API's die zijn gemaakt op het Azure AD-platform aanroepen. In deze documentatie wordt beschreven hoe u Azure AD-ondersteuning aan uw app kunt toevoegen met standaardprotocollen zoals OAuth 2.0 en OpenID Connect.

> [!NOTE]
> De meeste inhoud op deze pagina is gericht op het Azure AD v1.0-eindpunt, dat alleen ondersteuning biedt voor Microsoft-werkaccounts en -schoolaccounts. Raadpleeg de informatie over het [Azure AD v2.0-eindpunt](active-directory-appmodel-v2-overview.md) als u persoonlijke Microsoft-accounts of Microsoft-accounts van klanten wilt aanmelden. Het Azure AD v2.0-eindpunt biedt een uniforme ontwikkeling voor apps met aanmelding voor zowel gebruikers met Azure AD-accounts (werk en school) als gebruikers met persoonlijke Microsoft-accounts.

| | |
| --- | --- |
|[De basisbeginselen van verificatie](authentication-scenarios.md) | Een inleiding tot verificatie met Azure AD. |
|[Soorten toepassingen](authentication-scenarios.md#application-types-and-scenarios) | Een overzicht van de verificatiescenario's die worden ondersteund door Azure AD. |      
| | |

## <a name="get-started"></a>Aan de slag
De volgende handleidingen begeleiden u bij het maken van een app op het platform van uw voorkeur met behulp van de Azure AD Authentication Library (ADAL) SDK. Raadpleeg onze documentatie over het [Azure AD v2.0-eindpunt](active-directory-appmodel-v2-overview.md) als u informatie zoekt over het gebruik van de Microsoft Authentication Library (MSAL).

|  |  |  |  |
| --- | --- | --- | --- |
| <center>![Mobiele en bureaubladapps](./media/azure-ad-developers-guide/NativeApp_Icon.png)<br />Mobiele en bureaubladapps</center> | [Overzicht](authentication-scenarios.md#native-application-to-web-api)<br /><br />[iOS](quickstart-v1-ios.md)<br /><br />[Android](quickstart-v1-android.md) | [.NET (WPF)](quickstart-v1-dotnet.md)<br /><br />[Xamarin](quickstart-v1-xamarin.md) |
| <center>![Web-apps](./media/azure-ad-developers-guide/Web_app.png)<br />Web-apps</center> | [Overzicht](authentication-scenarios.md#web-browser-to-web-application)<br /><br />[ASP.NET](quickstart-v1-aspnet-webapp.md)<br /><br />[Java](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect) | [Python](https://github.com/Azure-Samples/active-directory-python-webapp-graphapi)<br/><br/> [Node.js](quickstart-v1-openid-connect-code.md) |
| <center>![Apps met één pagina](./media/azure-ad-developers-guide/SPA.png)<br />Apps met één pagina</center> | [Overzicht](authentication-scenarios.md#single-page-application-spa)<br /><br />[AngularJS](active-directory-devquickstarts-angular.md)<br /><br />[JavaScript](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) |  |
| <center>![Web-API's](./media/azure-ad-developers-guide/Web_API.png)<br />Web-API's</center> | [Overzicht](authentication-scenarios.md#web-application-to-web-api)<br /><br />[ASP.NET](active-directory-devquickstarts-webapi-dotnet.md)<br /><br />[Node.js](active-directory-devquickstarts-webapi-nodejs.md) | &nbsp; |
| <center>![Service naar service](./media/azure-ad-developers-guide/Service_App.png)<br />Service naar service</center> | [Overzicht](authentication-scenarios.md#daemon-or-server-application-to-web-api)<br /><br />[.NET](active-directory-code-samples.md#daemon-applications-accessing-web-apis-with-the-applications-identity)|  |
|  |  |  |  |  |

## <a name="how-to-guides"></a>Handleidingen
Deze handleidingen behandelen een paar van de meest voorkomende taken in Azure AD.

|                                                                           |  |
|---------------------------------------------------------------------------| --- |
|[Een toepassing registreren](quickstart-v1-integrate-apps-with-azure-ad.md)           | Een toepassing registreren in Azure AD. |
|[Toepassingen voor meerdere tenants](active-directory-devhowto-multi-tenant-overview.md)    | Aanmelden bij een Microsoft-werkaccount. |
|[OAuth- en OpenID Connect-protocollen](active-directory-protocols-openid-connect-code.md)| Het aanmelden van gebruikers en aanroepen van web-API's met de Microsoft-verificatieprotocollen. |
|  |  |

## <a name="reference-topics"></a>Onderwerpen met naslaginformatie
De volgende artikelen bieden gedetailleerde informatie over API's, protocolberichten en termen die worden gebruikt in Azure AD.

|                                                                                   | |
| ----------------------------------------------------------------------------------| --- |
| [Verificatiebibliotheken (ADAL)](active-directory-authentication-libraries.md)   | Een overzicht van de bibliotheken en SDK's die worden geleverd door Azure AD. |
| [Codevoorbeelden](active-directory-code-samples.md)                                  | Een lijst met alle Azure AD-codevoorbeelden. |
| [Woordenlijst](active-directory-dev-glossary.md)                                      | Termen en definities van woorden die in deze documenten worden gebruikt. |
|  |  |


[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
