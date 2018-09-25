---
title: Azure Active Directory v2.0-verificatiebibliotheken | Microsoft Docs
description: Compatibel-clientbibliotheken en server-middleware-bibliotheken en -gerelateerde bibliotheek, bron en koppelingen naar voorbeelden, voor het Azure Active Directory v2.0-eindpunt.
services: active-directory
documentationcenter: ''
author: negoe
manager: mtillman
editor: ''
ms.assetid: 19cec615-e51f-4141-9f8c-aaf38ff9f746
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/14/2018
ms.author: negoe
ms.reviewer: jmprieur, saeeda
ms.custom: aaddev
ms.openlocfilehash: 220627e6903abb643cde63cbc9b85402360ce3f3
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46999760"
---
# <a name="azure-active-directory-v20-authentication-libraries"></a>Azure Active Directory v2.0-verificatiebibliotheken

De [v2.0-eindpunt voor Azure Active Directory (Azure AD)](active-directory-v2-compare.md) ondersteunt de industriestandaard-protocollen voor OAuth 2.0 en OpenID Connect 1.0. Microsoft Authentication Library (MSAL) is ontworpen voor gebruik met de Azure AD v2.0-eindpunt. Het is ook mogelijk met gebruik van open source-bibliotheken die ondersteuning bieden voor OAuth 2.0 en OpenID Connect 1.0.

Het wordt aanbevolen dat u bibliotheken zijn, geschreven door protocol domein experts die een methodologie Security Development Lifecycle (SDL) zoals Volg [het gevolgd door Microsoft][Microsoft-SDL]. Als u hand-code voor de protocollen besluit, moet u een methodologie volgen, zoals SDL en betalen van Microsoft aandacht wordt besteed aan de beveiligingsoverwegingen in de specificaties standaarden voor elk protocol sluiten.

> [!NOTE]
> Zoek naar de Azure AD v1.0 library (ADAL)? Bekijk de [ADAL-bibliotheek handleiding](active-directory-authentication-libraries.md).

## <a name="types-of-libraries"></a>Typen bibliotheken

Azure AD v2.0-eindpunt werkt met twee typen bibliotheken:

* **Clientbibliotheken**: systeemeigen clients en servers-clientbibliotheken gebruiken om toegangstokens voor het aanroepen van een resource, zoals Microsoft Graph.
* **Server-middleware bibliotheken**: Web-apps gebruiken server middleware-bibliotheken voor aanmelden van gebruikers. Web-API's gebruiken server middleware-bibliotheken voor het valideren van tokens die zijn verzonden door systeemeigen clients of andere servers.

## <a name="library-support"></a>Ondersteuning voor clientbibliotheek

Bibliotheken zijn er twee categorieën voor ondersteuning:

* **Microsoft ondersteund**: Microsoft biedt oplossingen voor deze bibliotheken en SDL heeft gedaan vanwege de zorgvuldigheid op deze bibliotheken.
* **Compatibel**: Microsoft heeft deze bibliotheken in algemene scenario's getest en hebt bevestigd dat ze met het v2.0-eindpunt werken. Microsoft biedt geen oplossingen voor deze bibliotheken en een overzicht van deze bibliotheken niet heeft gedaan. Problemen en functie-aanvragen moeten worden omgeleid naar de open source-project van de bibliotheek.

Zie de volgende secties in dit artikel voor een lijst met bibliotheken die met het v2.0-eindpunt werken.

## <a name="microsoft-supported-client-libraries"></a>Ondersteunde Microsoft-clientbibliotheken

Clientbibliotheken voor verificatie worden gebruikt voor het verkrijgen van een token voor het aanroepen van een beveiligde Web-API

| Platform | Bibliotheek | Downloaden | Broncode | Voorbeeld | Referentie | Conceptuele doc-bestand | Roadmap |
| --- | --- | --- | --- | --- | --- | --- | ---| ---|
| ![Javascript](media/sample-v2-code/logo_js.png) | MSAL.js (Preview) | [NPM](https://www.npmjs.com/package/msal) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angularjs/README.md) |  [App van één pagina](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2) |  | [Wiki](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki)|
|![Angular JS](media/sample-v2-code/logo_angular.png) | Angular JS MSAL | [NPM](https://www.npmjs.com/package/@azure/msal-angularjs) | [GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angularjs/README.md) |  |  | |
![Angular](media/sample-v2-code/logo_angular.png) | MSAL Angular(Preview) | [NPM](https://www.npmjs.com/package/@azure/msal-angular) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) | | | |
| ![.NET Framework](media/sample-v2-code/logo_NET.png) ![UWP](media/sample-v2-code/logo_windows.png) ![Xamarin](media/sample-v2-code/logo_xamarin.png) | MSAL .NET (Preview) |[NuGet](https://www.nuget.org/packages/Microsoft.Identity.Client) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) | [Desktop-app](guidedsetups/active-directory-mobileanddesktopapp-windowsdesktop-intro.md) | [MSAL.NET](https://docs.microsoft.com/dotnet/api/microsoft.identity.client?view=azure-dotnet-preview) |[Wiki](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki#conceptual-documentation) | [Roadmap](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki#roadmap)
| ![iOS / Objective-C of swift.](media/sample-v2-code/logo_iOS.png) | MSAL obj_c (Preview) | [GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-objc) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-objc) | [iOS-app](https://github.com/Azure-Samples/active-directory-msal-ios-swift) |  |
|![Android / Java](media/sample-v2-code/logo_Android.png) | MSAL (Preview) | [ Centrale opslagplaats](https://repo1.maven.org/maven2/com/microsoft/identity/client/msal/) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-android) | [Android-app](quickstart-v2-android.md) | [JavaDocs](http://javadoc.io/doc/com.microsoft.identity.client/msal) | | |

## <a name="microsoft-supported-server-middleware-libraries"></a>Microsoft ondersteund server middleware-bibliotheken

Middleware-bibliotheken worden gebruikt voor het beveiligen van webtoepassingen en Web-API's. Voor web-app of Web-API die zijn geschreven met ASP.NET of ASP.NET Core, het middleware-bibliotheken worden gebruikt door ASP.NET / ASP.NET Core

| Platform | Bibliotheek | Downloaden | Broncode | Voorbeeld | Referentie
| --- | --- | --- | --- | --- | --- |
| ![.NET](media/sample-v2-code/logo_NET.png) ![.NET Core](media/sample-v2-code/logo_NETcore.png) | ASP.NET-beveiliging |[NuGet](https://www.nuget.org/packages/Microsoft.AspNet.Mvc/) |[ASP.NET-beveiliging (GitHub)](https://github.com/aspnet/Security) |[MVC-app](quickstart-v2-aspnet-webapp.md) |[ASP.NET-API-verwijzing](https://docs.microsoft.com/dotnet/api/?view=aspnetcore-2.0) |
| ![.NET](media/sample-v2-code/logo_NET.png)| IdentityModel extensies voor .NET| |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | [MVC-app](quickstart-v2-aspnet-webapp.md) |[Naslaginformatie](https://docs.microsoft.com/dotnet/api/overview/azure/activedirectory/client?view=azure-dotnet) |
| ![Node.js](media/sample-v2-code/logo_nodejs.png) | Azure AD-Passport |[NPM](https://www.npmjs.com/package/passport-azure-ad) |[GitHub](https://github.com/AzureAD/passport-azure-ad) | [Web-app](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs) | |

## <a name="compatible-client-libraries"></a>Compatibel-clientbibliotheken

| Platform | De naam van bibliotheek | Geteste versie | Broncode | Voorbeeld |
|:---:|:---:|:---:|:---:|:---:|
|![Javascript](media/sample-v2-code/logo_js.png)|[Hello.js](https://adodson.com/hello.js/) |1.13.5 |[Hello.js](https://github.com/MrSwitch/hello.js) |[SPA](https://github.com/Azure-Samples/active-directory-javascript-graphapi-web-v2) |
| ![Java](media/sample-v2-code/logo_java.png) | [Notulist Java](https://github.com/scribejava/scribejava) | [Versie 3.2.0](https://github.com/scribejava/scribejava/releases/tag/scribejava-3.2.0) | [ScribeJava](https://github.com/scribejava/scribejava/) | |
| ![PHP](media/sample-v2-code/logo_php.png) | [De PHP-competitie oauth2-client](https://github.com/thephpleague/oauth2-client) | [Versie 1.4.2](https://github.com/thephpleague/oauth2-client/releases/tag/1.4.2) | [oauth2-client](https://github.com/thephpleague/oauth2-client/) | |
| ![Ruby](media/sample-v2-code/logo_ruby.png) |[OmniAuth](https://github.com/omniauth/omniauth/wiki) |omniauth:1.3.1</br>omniauth-oauth2:1.4.0 |[OmniAuth](https://github.com/omniauth/omniauth)</br>[OmniAuth OAuth2](https://github.com/intridea/omniauth-oauth2) |  |
![iOS](media/sample-v2-code/logo_iOS.png) |[NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) |1.2.8 |[NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) |[Voorbeeld van de systeemeigen app](active-directory-v2-devquickstarts-ios.md) |

Voor elke standaarden-compatibele bibliotheek kunt u het v2.0-eindpunt, het is dus belangrijk te weten waar u voor ondersteuning.

* Neem contact op met de eigenaar van de bibliotheek voor problemen en nieuwe functieaanvragen in de code van de bibliotheek.
* Neem contact op met Microsoft voor problemen en nieuwe functieaanvragen in de implementatie van het servicezijde-protocol.
* [Een functieaanvraag bestand](https://feedback.azure.com/forums/169401-azure-active-directory) voor aanvullende functies u graag zou willen zien in het protocol.
* [Maak een ondersteuningsaanvraag](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) als u een probleem vindt waarbij de Azure AD v2.0-eindpunt is niet compatibel zijn met OAuth 2.0 of OpenID Connect 1.0.

## <a name="related-content"></a>Gerelateerde inhoud

Zie voor meer informatie over de Azure AD v2.0-eindpunt, de [overzicht van Azure AD app model v2.0][AAD-App-Model-V2-Overview].

<!--Image references-->

<!--Reference style links -->
[AAD-App-Model-V2-Overview]: v2-overview.md
[ClientLib-NET-Lib]: http://www.nuget.org/packages/Microsoft.Identity.Client
[ClientLib-NET-Repo]: https://github.com/AzureAD/microsoft-authentication-library-for-dotnet
[ClientLib-NET-Sample]: active-directory-v2-devquickstarts-wpf.md
[ClientLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ClientLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad
[ClientLib-Node-Sample]:/
[ClientLib-Iosmac-Lib]:/
[ClientLib-Iosmac-Repo]:/
[ClientLib-Iosmac-Sample]:/
[ClientLib-Android-Lib]:/
[ClientLib-Android-Repo]:/
[ClientLib-Android-Sample]:/
[ClientLib-Js-Lib]:/
[ClientLib-Js-Repo]:/
[ClientLib-Js-Sample]:/

[Microsoft-SDL]: http://www.microsoft.com/sdl/default.aspx
[ServerLib-Net4-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/
[ServerLib-Net4-Owin-Oidc-Repo]: http://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oidc-Sample]: active-directory-v2-devquickstarts-dotnet-web.md
[ServerLib-Net4-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OAuth/
[ServerLib-Net4-Owin-Oauth-Repo]: http://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oauth-Sample]: https://azure.microsoft.com/documentation/articles/active-directory-v2-devquickstarts-dotnet-api/
[ServerLib-Net-Jwt-Lib]: https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt
[ServerLib-Net-Jwt-Repo]: https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet
[ServerLib-Net-Jwt-Sample]:/
[ServerLib-NetCore-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OpenIdConnect/
[ServerLib-NetCore-Owin-Oidc-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oidc-Sample]: https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-v2
[ServerLib-NetCore-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OAuth/
[ServerLib-NetCore-Owin-Oauth-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oauth-Sample]:/
[ServerLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ServerLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad/
[ServerLib-Node-Sample]: https://azure.microsoft.com/documentation/articles/active-directory-v2-devquickstarts-node-web/
