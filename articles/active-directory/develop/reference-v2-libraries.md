---
title: Verificatiebibliotheken van Microsoft identity-platform | Microsoft Docs
description: Compatibel-clientbibliotheken en server-middleware-bibliotheken en -gerelateerde bibliotheek, bron en koppelingen naar voorbeelden, voor het eindpunt van de Microsoft identity-platform.
services: active-directory
documentationcenter: ''
author: negoe
manager: CelesteDG
editor: ''
ms.assetid: 19cec615-e51f-4141-9f8c-aaf38ff9f746
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: negoe
ms.reviewer: jmprieur, saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 19718dc884685f6c1e1a588eea6a5de36e446fb1
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2019
ms.locfileid: "65545538"
---
# <a name="microsoft-identity-platform-authentication-libraries"></a>Verificatiebibliotheken van Microsoft identity-platform

De [Microsoft identity platform endpoint](active-directory-v2-compare.md) ondersteunt de industriestandaard-protocollen voor OAuth 2.0 en OpenID Connect 1.0. Microsoft Authentication Library (MSAL) is ontworpen voor gebruik met het eindpunt van de Microsoft identity-platform. Het is ook mogelijk met gebruik van open source-bibliotheken die ondersteuning bieden voor OAuth 2.0 en OpenID Connect 1.0.

Het wordt aanbevolen dat u bibliotheken zijn, geschreven door protocol domein experts die een methodologie Security Development Lifecycle (SDL) zoals Volg [het gevolgd door Microsoft][Microsoft-SDL]. Als u hand-code voor de protocollen besluit, moet u een methodologie volgen, zoals SDL en betalen van Microsoft aandacht wordt besteed aan de beveiligingsoverwegingen in de specificaties standaarden voor elk protocol sluiten.

> [!NOTE]
> Zoek naar de Azure AD authentication library (ADAL)? Bekijk de [ADAL-bibliotheek handleiding](active-directory-authentication-libraries.md).

## <a name="types-of-libraries"></a>Typen bibliotheken

Eindpunt voor Microsoft identity-platform werkt met twee typen bibliotheken:

* **Clientbibliotheken**: Systeemeigen clients en servers gebruiken-clientbibliotheken toegangstokens voor het aanroepen van een resource, zoals Microsoft Graph.
* **Server-middleware bibliotheken**: Web-apps gebruiken server middleware-bibliotheken voor aanmelden van gebruikers. Web-API's gebruiken server middleware-bibliotheken voor het valideren van tokens die zijn verzonden door systeemeigen clients of andere servers.

## <a name="library-support"></a>Ondersteuning voor clientbibliotheek

Bibliotheken zijn er twee categorieën voor ondersteuning:

* **Microsoft ondersteund**: Microsoft biedt oplossingen voor deze bibliotheken en SDL heeft gedaan vanwege de zorgvuldigheid op deze bibliotheken.
* **Compatibel**: Microsoft heeft deze bibliotheken in algemene scenario's getest en hebt bevestigd dat ze met het eindpunt van de Microsoft identity-platform werken. Microsoft biedt geen oplossingen voor deze bibliotheken en een overzicht van deze bibliotheken niet heeft gedaan. Problemen en functie-aanvragen moeten worden omgeleid naar de open source-project van de bibliotheek.

Zie de volgende secties in dit artikel voor een lijst met bibliotheken die met het eindpunt van de Microsoft identity-platform werken.

## <a name="microsoft-supported-client-libraries"></a>Ondersteunde Microsoft-clientbibliotheken

Clientbibliotheken voor verificatie worden gebruikt voor het verkrijgen van een token voor het aanroepen van een beveiligde Web-API

| Platform | Tapewisselaar | Downloaden | Broncode | Voorbeeld | Referentie | Conceptuele doc-bestand | Roadmap |
| --- | --- | --- | --- | --- | --- | --- | ---|
| ![JavaScript](media/sample-v2-code/logo_js.png) | MSAL.js  | [NPM](https://www.npmjs.com/package/msal) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angularjs/README.md) |  [App van één pagina](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2) | [Naslaginformatie](https://htmlpreview.github.io/?https://raw.githubusercontent.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-core/docs/classes/_useragentapplication_.useragentapplication.html) | [Conceptuele docs](msal-overview.md)| [Roadmap](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki#roadmap)
|![Angular JS](media/sample-v2-code/logo_angular.png) | MSAL Angular JS | [NPM](https://www.npmjs.com/package/@azure/msal-angularjs) | [GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angularjs/README.md) |  |  | |
![Angular](media/sample-v2-code/logo_angular.png) | MSAL Angular(Preview) | [NPM](https://www.npmjs.com/package/@azure/msal-angular) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) | | | |
| ![.NET Framework](media/sample-v2-code/logo_NET.png) ![UWP](media/sample-v2-code/logo_windows.png) ![Xamarin](media/sample-v2-code/logo_xamarin.png) | MSAL .NET  |[NuGet](https://www.nuget.org/packages/Microsoft.Identity.Client) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) | [Desktop-app](guidedsetups/active-directory-mobileanddesktopapp-windowsdesktop-intro.md) | [MSAL.NET](https://docs.microsoft.com/dotnet/api/microsoft.identity.client?view=azure-dotnet-preview) |[wiki](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki#conceptual-documentation) | [Roadmap](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki#roadmap)
| ![Python](media/sample-v2-code/logo_python.png) | MSAL Python (Preview) | [PyPI](https://pypi.org/project/msal) | [GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-python) | [Voorbeelden](https://github.com/AzureAD/microsoft-authentication-library-for-python/tree/dev/sample) | [ReadTheDocs](https://msal-python.rtfd.io/) | [wiki](https://github.com/AzureAD/microsoft-authentication-library-for-python/wiki) | [Roadmap](https://github.com/AzureAD/microsoft-authentication-library-for-python/wiki/Roadmap)
| ![Java](media/sample-v2-code/logo_java.png) | MSAL Java (Preview) | [Maven](https://mvnrepository.com/artifact/com.microsoft.azure/msal4j) | [GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-java) | [Voorbeelden](https://github.com/AzureAD/microsoft-authentication-library-for-java/tree/dev/src/samples) | | | [Roadmap](https://github.com/AzureAD/microsoft-authentication-library-for-java/wiki)
| ![iOS / Objective-C of swift.](media/sample-v2-code/logo_iOS.png) | MSAL obj_c (Preview) | [GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-objc) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-objc) | [iOS-app](https://github.com/Azure-Samples/active-directory-msal-ios-swift) |  |
|![Android / Java](media/sample-v2-code/logo_Android.png) | MSAL (Preview) | [Centrale opslagplaats](https://repo1.maven.org/maven2/com/microsoft/identity/client/msal/) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-android) | [Android-app](quickstart-v2-android.md) | [JavaDocs](https://javadoc.io/doc/com.microsoft.identity.client/msal) | | |

## <a name="microsoft-supported-server-middleware-libraries"></a>Microsoft ondersteund server middleware-bibliotheken

Middleware-bibliotheken worden gebruikt voor het beveiligen van webtoepassingen en Web-API's. Voor web-app of Web-API die zijn geschreven met ASP.NET of ASP.NET Core, het middleware-bibliotheken worden gebruikt door ASP.NET / ASP.NET Core

| Platform | Tapewisselaar | Downloaden | Broncode | Voorbeeld | Referentie
| --- | --- | --- | --- | --- | --- |
| ![.NET](media/sample-v2-code/logo_NET.png) ![.NET Core](media/sample-v2-code/logo_NETcore.png) | ASP.NET Security |[NuGet](https://www.nuget.org/packages/Microsoft.AspNet.Mvc/) |[GitHub](https://github.com/aspnet/AspNetCore) |[MVC-app](quickstart-v2-aspnet-webapp.md) |[ASP.NET-API-verwijzing](https://docs.microsoft.com/dotnet/api/?view=aspnetcore-2.0) |
| ![.NET](media/sample-v2-code/logo_NET.png)| IdentityModel extensies voor .NET| |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | [MVC-app](quickstart-v2-aspnet-webapp.md) |[Naslaginformatie](https://docs.microsoft.com/dotnet/api/overview/azure/activedirectory/client?view=azure-dotnet) |
| ![Node.js](media/sample-v2-code/logo_nodejs.png) | Azure AD-Passport |[NPM](https://www.npmjs.com/package/passport-azure-ad) |[GitHub](https://github.com/AzureAD/passport-azure-ad) | [Web-app](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs) | |

## <a name="compatible-client-libraries"></a>Compatibel-clientbibliotheken

| Platform | De naam van bibliotheek | Geteste versie | Broncode | Voorbeeld |
|:---:|:---:|:---:|:---:|:---:|
|![JavaScript](media/sample-v2-code/logo_js.png)|[Hello.js](https://adodson.com/hello.js/) | 1.13.5 |[Hello.js](https://github.com/MrSwitch/hello.js) |[SPA](https://github.com/Azure-Samples/active-directory-javascript-graphapi-web-v2) |
| ![Java](media/sample-v2-code/logo_java.png) | [Notulist Java](https://github.com/scribejava/scribejava) | [Versie 3.2.0](https://github.com/scribejava/scribejava/releases/tag/scribejava-3.2.0) | [ScribeJava](https://github.com/scribejava/scribejava/) | |
| ![Java](media/sample-v2-code/logo_java.png) | [Bibliotheek Gluu OpenID Connect](https://github.com/GluuFederation/oxAuth) | [Versie 3.0.2](https://github.com/GluuFederation/oxAuth/releases/tag/3.0.2) | [Bibliotheek Gluu OpenID Connect](https://github.com/GluuFederation/oxAuth) | |
| ![Python](media/sample-v2-code/logo_python.png) | [Requests-OAuthlib](https://github.com/requests/requests-oauthlib) | [Versie 1.2.0](https://github.com/requests/requests-oauthlib/releases/tag/v1.2.0) | [Requests-OAuthlib](https://github.com/requests/requests-oauthlib) | |
| ![Node.js](media/sample-v2-code/logo_nodejs.png) | [openid-client](https://github.com/panva/node-openid-client) | [Versie 2.4.5](https://github.com/panva/node-openid-client/releases/tag/v2.4.5) | [openid-client](https://github.com/panva/node-openid-client) | |
| ![PHP](media/sample-v2-code/logo_php.png) | [De PHP-competitie oauth2-client](https://github.com/thephpleague/oauth2-client) | [Versie 1.4.2](https://github.com/thephpleague/oauth2-client/releases/tag/1.4.2) | [oauth2-client](https://github.com/thephpleague/oauth2-client/) | |
| ![Ruby](media/sample-v2-code/logo_ruby.png) |[OmniAuth](https://github.com/omniauth/omniauth/wiki) |omniauth:1.3.1<br />omniauth-oauth2:1.4.0 |[OmniAuth](https://github.com/omniauth/omniauth)<br />[OmniAuth OAuth2](https://github.com/intridea/omniauth-oauth2) |  |
| ![iOS](media/sample-v2-code/logo_iOS.png) ![Android](media/sample-v2-code/logo_Android.png) | [React Native App Auth](https://github.com/FormidableLabs/react-native-app-auth) | [Versie 4.2.0](https://github.com/FormidableLabs/react-native-app-auth/releases/tag/v4.2.0) | [React Native App Auth](https://github.com/FormidableLabs/react-native-app-auth) | |

Voor elke standaarden-compatibele bibliotheek kunt u het eindpunt van Microsoft identity-platform, dus het is belangrijk te weten waar u voor ondersteuning.

* Neem contact op met de eigenaar van de bibliotheek voor problemen en nieuwe functieaanvragen in de code van de bibliotheek.
* Neem contact op met Microsoft voor problemen en nieuwe functieaanvragen in de implementatie van het servicezijde-protocol.
* [Een functieaanvraag bestand](https://feedback.azure.com/forums/169401-azure-active-directory) voor aanvullende functies u graag zou willen zien in het protocol.
* [Maak een ondersteuningsaanvraag](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) als u een probleem vindt waarbij het eindpunt van de Microsoft identity-platform is niet compatibel zijn met OAuth 2.0 of OpenID Connect 1.0.

## <a name="related-content"></a>Gerelateerde inhoud

Zie voor meer informatie over het eindpunt van de Microsoft identity-platform, de [overzicht van Microsoft identity-platform][AAD-App-Model-V2-Overview].

<!--Image references-->

<!--Reference style links -->
[AAD-App-Model-V2-Overview]: v2-overview.md
[ClientLib-NET-Lib]: https://www.nuget.org/packages/Microsoft.Identity.Client
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

[Microsoft-SDL]: https://www.microsoft.com/sdl/default.aspx
[ServerLib-Net4-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/
[ServerLib-Net4-Owin-Oidc-Repo]: https://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oidc-Sample]: active-directory-v2-devquickstarts-dotnet-web.md
[ServerLib-Net4-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OAuth/
[ServerLib-Net4-Owin-Oauth-Repo]: https://katanaproject.codeplex.com/
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
