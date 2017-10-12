---
title: Azure Active Directory voor ontwikkelaars | Microsoft Docs
description: Dit artikel geeft een overzicht van het aanmelden bij werk- en schoolaccounts van Microsoft met Azure Active Directory.
services: active-directory
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 5c872c89-ef04-4f4c-98de-bc0c7460c7c2
ms.service: active-directory
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: dce813b885d492343428428056a2e8aada27b461
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-active-directory-for-developers"></a>Azure Active Directory voor ontwikkelaars
Azure Active Directory is een identiteitsservice in de cloud waarmee ontwikkelaars ervoor kunnen zorgen dat elke gebruiker met een werk- of schoolaccount ondersteund door Microsoft zich veilig kan aanmelden.  In deze documentatie wordt beschreven hoe u Azure AD-ondersteuning aan uw toepassing kunt toevoegen met de standaardverificatieprotocollen OAuth en OpenID Connect.

| | |
| --- | --- |
|[Basisprincipes van Auth](active-directory-authentication-scenarios.md) | Een inleiding tot verificatie met Azure AD |
|[Soorten toepassingen](active-directory-authentication-scenarios.md#application-types-and-scenarios) | Een overzicht van de verificatiescenario's die worden ondersteund door Azure AD |                                
                                                                              
## <a name="get-started"></a>Aan de slag
In deze stapsgewijze installaties leert u hoe u onze verificatiebibliotheken kunt gebruiken om Azure Active Directory-gebruikers aan te melden.

|  |  |  |  |
| --- | --- | --- | --- |
| <center>![Mobiele en bureaubladapps](./media/active-directory-developers-guide/NativeApp_Icon.png)<br />Mobiele en bureaubladapps</center> | [Overzicht](active-directory-authentication-scenarios.md#native-application-to-web-api)<br /><br />[iOS](active-directory-devquickstarts-ios.md)<br /><br />[Android](active-directory-devquickstarts-android.md) | [.NET](active-directory-devquickstarts-dotnet.md)<br /><br />[Windows](active-directory-devquickstarts-windowsstore.md)<br /><br />[Xamarin](active-directory-devquickstarts-xamarin.md) | [Cordova](active-directory-devquickstarts-cordova.md)<br /><br />[OAuth 2.0](active-directory-protocols-oauth-code.md) |
| <center>![Web Apps](./media/active-directory-developers-guide/Web_app.png)<br />Web Apps</center> | [Overzicht](active-directory-authentication-scenarios.md#web-browser-to-web-application)<br /><br />[ASP.NET](active-directory-devquickstarts-webapp-dotnet.md)<br /><br />[Java](active-directory-devquickstarts-webapp-java.md) | [NodeJS](active-directory-devquickstarts-openidconnect-nodejs.md)<br /><br />[OpenID Connect 1.0](active-directory-protocols-openid-connect-code.md) |  |
| <center>![Apps met één pagina](./media/active-directory-developers-guide/SPA.png)<br />Apps met één pagina</center> | [Overzicht](active-directory-authentication-scenarios.md#single-page-application-spa)<br /><br />[AngularJS](active-directory-devquickstarts-angular.md)<br /><br />[JavaScript](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) |  |  |
| <center>![Web-API's](./media/active-directory-developers-guide/Web_API.png)<br />Web-API's</center> | [Overzicht](active-directory-authentication-scenarios.md#web-application-to-web-api)<br /><br />[ASP.NET](active-directory-devquickstarts-webapi-dotnet.md)<br /><br />[NodeJS](active-directory-devquickstarts-webapi-nodejs.md) | &nbsp; |
| <center>![Service naar service](./media/active-directory-developers-guide/Service_App.png)<br />Service naar service</center> | [Overzicht](active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api)<br /><br />[.NET](active-directory-code-samples.md#server-or-daemon-application-to-web-api)<br /><br />[Referenties voor OAuth 2.0-client](active-directory-protocols-oauth-service-to-service.md) |  |

## <a name="guides"></a>Handleidingen
In deze artikelen wordt beschreven hoe u algemene taken kunt uitvoeren met Azure Active Directory.

|                                                                           |  |
|---------------------------------------------------------------------------| --- |
|[Appregistratie](active-directory-integrating-applications.md)           | Een app registreren in Azure AD |
|[Apps met meerdere tenants](active-directory-devhowto-multi-tenant-overview.md)    | Aanmelden bij een Microsoft-werkaccount |
|[OAuth en OpenID Connect](active-directory-protocols-openid-connect-code.md)| Gebruikers aanmelden en web-API's aanroepen via onze moderne auth-protocollen |
|[Meer handleidingen...](active-directory-developers-guide-index.md#guides)        |     |

## <a name="reference"></a>Naslaginformatie
Deze artikelen bieden gedetailleerde informatie over de API's, protocolberichten en termen die worden gebruikt in Azure Active Directory.

|                                                                                   | |
| ----------------------------------------------------------------------------------| --- |
| [Verificatiebibliotheken (ADAL)](active-directory-authentication-libraries.md)   | Een overzicht van de bibliotheken en SDK's die worden geleverd door Azure AD |
| [Codevoorbeelden](active-directory-code-samples.md)                                  | Een lijst met alle Azure AD-codevoorbeelden |
| [Woordenlijst](active-directory-dev-glossary.md)                                      | Termen en definities van woorden die in deze documenten worden gebruikt |
| [Meer referentiemateriaal...](active-directory-developers-guide-index.md#reference)|     |

## <a name="help--support"></a>Help en ondersteuning
Dit zijn de aanbevolen plaatsen voor hulp bij het ontwikkelen met Azure Active Directory.

|  |  
|---|
|[Stack Overflow's tags voor `azure-active-directory` en `adal`](http://stackoverflow.com/questions/tagged/azure-active-directory+or+adal)      |
|[Feedback op Azure Active Directory](https://feedback.azure.com/forums/169401-azure-active-directory/category/164757-developer-experiences)|
| [Microsoft Dev Chat Uitproberen (gratis gedurende beperkte tijd)](http://aka.ms/devchat) |

<br />

> [!NOTE]
> Als u persoonlijke Microsoft-accounts wilt aanmelden, kunt u overwegen het [Azure AD v2.0-eindpunt](active-directory-appmodel-v2-overview.md) te gebruiken.  Het Azure AD v2.0-eindpunt combineert persoonlijke Microsoft-accounts en Microsoft-werkaccounts (vanuit Azure AD) in één verificatiesysteem.
