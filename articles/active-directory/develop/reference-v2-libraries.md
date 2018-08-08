---
title: Azure Active Directory v2.0-verificatiebibliotheken | Microsoft Docs
description: Compatibel-clientbibliotheken en server-middleware-bibliotheken en -gerelateerde bibliotheek, bron en koppelingen naar voorbeelden, voor het Azure Active Directory v2.0-eindpunt.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 19cec615-e51f-4141-9f8c-aaf38ff9f746
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/13/2018
ms.author: celested
ms.reviewer: jmprieur, saeeda
ms.custom: aaddev
ms.openlocfilehash: 1450cffca7a4cfa57856c75cdcc9e106958ea043
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39601673"
---
# <a name="azure-active-directory-v20-authentication-libraries"></a>Azure Active Directory v2.0-verificatiebibliotheken

De [v2.0-eindpunt voor Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-compare) ondersteunt de industriestandaard-protocollen voor OAuth 2.0 en OpenID Connect 1.0. Microsoft Authentication Library (MSAL) is ontworpen voor gebruik met de Azure AD v2.0-eindpunt. Het is ook mogelijk met gebruik van open source-bibliotheken die ondersteuning bieden voor OAuth 2.0 en OpenID Connect 1.0.

Het wordt aanbevolen dat u bibliotheken zijn, geschreven door protocol domein experts die een methodologie Security Development Lifecycle (SDL) zoals Volg [het gevolgd door Microsoft][Microsoft-SDL]. Als u te hand-code-ondersteuning voor de protocollen besluit, volgt u een methodologie, zoals SDL en betalen van Microsoft aandacht wordt besteed aan de beveiligingsoverwegingen in de specificaties standaarden voor elk protocol sluiten.

> [!NOTE]
> Zoek naar de Azure AD-v1.0-bibliotheken (ADAL)? Bekijk de [ADAL-bibliotheek handleiding](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-libraries).
>
>

## <a name="types-of-libraries"></a>Typen bibliotheken

Azure AD v2.0-eindpunt werkt met twee typen bibliotheken:

* **Clientbibliotheken**. Systeemeigen clients en servers gebruiken-clientbibliotheken toegangstokens voor het aanroepen van een resource, zoals Microsoft Graph.
* **Server-middleware bibliotheken**. Web-apps gebruiken server middleware-bibliotheken voor aanmelden van gebruikers. Web-API's gebruiken server middleware-bibliotheken voor het valideren van tokens die zijn verzonden door systeemeigen clients of andere servers.

## <a name="library-support"></a>Ondersteuning voor clientbibliotheek

Omdat u een standards-compatibele bibliotheek kiezen kunt wanneer u het v2.0-eindpunt is het belangrijk te weten waar u voor ondersteuning. Neem contact op met de eigenaar van de bibliotheek voor problemen en functieaanvragen in de code van de bibliotheek. Neem contact op met Microsoft voor problemen en functieaanvragen in de implementatie van het servicezijde-protocol. [Een functieaanvraag bestand](https://feedback.azure.com/forums/169401-azure-active-directory) voor aanvullende functies u graag zou willen zien in het protocol. [Maak een ondersteuningsaanvraag](https://docs.microsoft.com/en-us/azure/azure-supportability/how-to-create-azure-support-request) als u een probleem vindt waarbij de Azure AD v2.0-eindpunt is niet compatibel zijn met OAuth 2.0 of OpenID Connect 1.0.

Bibliotheken zijn er twee categorieën voor ondersteuning:

* **Microsoft ondersteund**. Microsoft biedt oplossingen voor deze bibliotheken en SDL heeft gedaan vanwege de zorgvuldigheid op deze bibliotheken.
* **Compatibel**. Microsoft heeft deze bibliotheken in algemene scenario's getest en hebt bevestigd dat ze met het v2.0-eindpunt werken. Microsoft biedt geen oplossingen voor deze bibliotheken en een overzicht van deze bibliotheken niet heeft gedaan. Problemen en functie-aanvragen moeten worden omgeleid naar de open source-project van de bibliotheek.

Zie de volgende secties in dit artikel voor een lijst met bibliotheken die met het v2.0-eindpunt werken.

## <a name="microsoft-supported-client-libraries"></a>Ondersteunde Microsoft-clientbibliotheken

> [!IMPORTANT]
> De MSAL-preview-bibliotheken zijn geschikt voor gebruik in een productieomgeving. Microsoft biedt de dezelfde productie-ondersteuning voor deze bibliotheken als de huidige productie-bibliotheken (ADAL). Tijdens de Preview-versie, wijzigingen aan de MSAL-API, indeling van de interne cache en andere mechanismen van deze bibliotheken zonder voorafgaande kennisgeving, die u moet uitvoeren samen met oplossingen voor problemen of verbeterde functies te verwachten. Dit kan van invloed op uw toepassing. Een wijziging in de cache-indeling is bijvoorbeeld mogelijk uw gebruikers zich opnieuw aanmelden. Een wijziging in de API moet u mogelijk om bij te werken van uw code. Wanneer de algemene beschikbaarheid (GA)-versie beschikbaar is, alle toepassingen met behulp van een preview-versie van de bibliotheek moeten bijwerken binnen zes maanden of ze werken mogelijk niet meer.

| Platform | Bibliotheek | Downloaden | Broncode | Voorbeeld | Referentie
| --- | --- | --- | --- | --- | --- |
| .NET-client, Windows Store, UWP of Xamarin iOS en Android | MSAL .NET (Preview) |[NuGet](https://www.nuget.org/packages/Microsoft.Identity.Client) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) | [Desktop-App](guidedsetups/active-directory-mobileanddesktopapp-windowsdesktop-intro.md) |  |
| Javascript | MSAL.js (Preview) | [GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-js) | [GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-js) | [App met één pagina](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2) |  |
| iOS-, Mac OS | MSAL (Preview) | [GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-objc) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-objc) | [iOS App](https://github.com/Azure-Samples/active-directory-msal-ios-swift) |  |
| Android | MSAL (Preview) | [De centrale opslagplaats](https://repo1.maven.org/maven2/com/microsoft/identity/client/msal/) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-android) | [Android-App](guidedsetups/active-directory-mobileanddesktopapp-android-intro.md) | [JavaDocs](http://javadoc.io/doc/com.microsoft.identity.client/msal) |

## <a name="microsoft-supported-server-middleware-libraries"></a>Microsoft ondersteund server middleware-bibliotheken

| Platform | Bibliotheek | Downloaden | Broncode | Voorbeeld | Referentie
| --- | --- | --- | --- | --- | --- |
| .NET 4.x | OpenID Connect OWIN-middleware |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect) |[GitHub](https://github.com/aspnet/AspNetKatana/) |[MVC-App](guidedsetups/active-directory-serversidewebapp-aspnetwebappowin-intro.md) | |
| .NET 4.x | OWIN OAuth Bearer-middleware voor AzureAD |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.ActiveDirectory/) |[GitHub](https://github.com/aspnet/AspNetKatana/) |  | |
| .NET 4.x | JWT-Handler voor .NET 4.5 | [NuGet](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt/4.0.4.403061554) | [GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |
| .NET Core | ASP.NET-middleware OpenID Connect |[Microsoft.AspNetCore.Authentication.OpenIdConnect (NuGet)][ServerLib-NetCore-Owin-Oidc-Lib] |[ASP.NET-beveiliging (GitHub)][ServerLib-NetCore-Owin-Oidc-Repo] |[MVC-app](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-v2) |
| .NET Core | ASP.NET OAuth Bearer middleware |[Microsoft.AspNetCore.Authentication.OAuth (NuGet)][ServerLib-NetCore-Owin-Oauth-Lib] |[ASP.NET-beveiliging (GitHub)][ServerLib-NetCore-Owin-Oauth-Repo] |  |
| .NET Core | JWT-Handler voor .NET Core  |[NuGet](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt) |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |
| Node.js |Azure AD-Passport |[npm](https://www.npmjs.com/package/passport-azure-ad) |[GitHub](https://github.com/AzureAD/passport-azure-ad) | [Web-app](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs)| |

## <a name="compatible-client-libraries"></a>Compatibel-clientbibliotheken

| Platform | De naam van bibliotheek | Geteste versie | Broncode | Voorbeeld |
|:---:|:---:|:---:|:---:|:---:|
| Android |[OIDCAndroidLib](https://github.com/kalemontes/OIDCAndroidLib/) |0.2.1 |[OIDCAndroidLib](https://github.com/kalemontes/OIDCAndroidLib) |[Voorbeeld van de systeemeigen app](active-directory-v2-devquickstarts-android.md) |
| iOS |[NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) |1.2.8 |[NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) |[Voorbeeld van de systeemeigen app](active-directory-v2-devquickstarts-ios.md) |
| Javascript |[Hello.js](https://adodson.com/hello.js/) |1.13.5 |[Hello.js](https://github.com/MrSwitch/hello.js) |[SPA](https://github.com/Azure-Samples/active-directory-javascript-graphapi-web-v2) |
| Java | [Notulist Java scribejava](https://github.com/scribejava/scribejava) | [Versie 3.2.0](https://github.com/scribejava/scribejava/releases/tag/scribejava-3.2.0) | [ScribeJava](https://github.com/scribejava/scribejava/) | |
| PHP | [De PHP-competitie oauth2-client](https://github.com/thephpleague/oauth2-client) | [Versie 1.4.2](https://github.com/thephpleague/oauth2-client/releases/tag/1.4.2) | [oauth2-client](https://github.com/thephpleague/oauth2-client/) | |
| Ruby |[OmniAuth](https://github.com/omniauth/omniauth/wiki) |omniauth:1.3.1</br>omniauth-oauth2:1.4.0 |[OmniAuth](https://github.com/omniauth/omniauth)</br>[OmniAuth OAuth2](https://github.com/intridea/omniauth-oauth2) |  |

## <a name="related-content"></a>Gerelateerde inhoud
Zie voor meer informatie over de Azure AD v2.0-eindpunt, de [overzicht van Azure AD app model v2.0][AAD-App-Model-V2-Overview].

<!--Image references-->

<!--Reference style links -->
[AAD-App-Model-V2-Overview]: ../active-directory-appmodel-v2-overview.md
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
