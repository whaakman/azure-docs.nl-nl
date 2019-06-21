---
title: Microsoft identity-platform-codevoorbeelden | Microsoft Docs
description: Biedt een overzicht van de beschikbare Microsoft identity-platform (v2.0-eindpunt)-codevoorbeelden, geordend op scenario.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: a242a5ff-7300-40c2-ba83-fb6035707433
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/26/2018
ms.author: ryanwi
ms.reviewer: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 326b69bffa0cd5728b939a91cce4fab3f3a329f7
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67272672"
---
# <a name="microsoft-identity-platform-code-samples-v20-endpoint"></a>Codevoorbeelden voor Microsoft identity-platform (v2.0-eindpunt)

[!INCLUDE [active-directory-develop-applies-v2-msal](../../../includes/active-directory-develop-applies-v2-msal.md)]

U kunt Microsoft identity-platform te gebruiken:

- Verificatie en autorisatie toevoegen aan uw webtoepassingen en web-API's.
- Vereisen dat een toegangstoken voor toegang tot een beveiligde web-API.

Dit artikel worden kort en verstrekt u koppelingen naar voorbeelden van het eindpunt van de Microsoft identity-platform. In deze voorbeelden ziet u hoe deze wordt uitgevoerd en bieden ook codefragmenten die u in uw toepassingen gebruiken kunt. Op de pagina van het voorbeeld van code vindt u gedetailleerde Leesmij-onderwerpen die u bij de vereisten, installatie en configuratie helpen. Opmerkingen in de code te begrijpen van de kritieke secties.

> [!NOTE]
> Als u geïnteresseerd in v1.0 voorbeelden bent, Zie [Azure AD-codevoorbeelden (v1.0 eindpunt)](sample-v1-code.md).

Zie voor meer informatie over het algemeen scenario voor elk Voorbeeldtype, [App-typen voor het eindpunt van de Microsoft identity-platform](v2-app-types.md).

U kunt ook bijdragen aan de voorbeelden op GitHub. Voor meer informatie Zie [Microsoft Azure Active Directory-voorbeelden en documentatie](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="single-page-applications"></a>Toepassingen met één pagina

Deze voorbeelden laten zien hoe het schrijven van een toepassing één pagina die wordt beveiligd met Microsoft identity-platform. Deze voorbeelden gebruik een van de versies van het MSAL.js.

| Platform | Description | Koppeling |
| -------- | --------------------- | -------- |
| ![JavaScript](media/sample-v2-code/logo_js.png) [JavaScript (msal.js)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-core) | Aanroepen van Microsoft Graph |[javascript-graphapi-web-v2](https://github.com/Azure-Samples/active-directory-javascript-graphapi-web-v2) |
| ![JavaScript](media/sample-v2-code/logo_js.png) [JavaScript (msal.js)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-core) | B2C-aanroepen |[b2c-javascript-msal-singlepageapp](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp) |
| ![JavaScript](media/sample-v2-code/logo_js.png) [JavaScript (msal.js)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-core) | Aanroepen van eigen web-API |[javascript-singlepageapp-dotnet-webapi-v2](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2) |
| ![Angular JS](media/sample-v2-code/logo_angular.png) [JavaScript (MSAL AngularJS)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angularjs)| Aanroepen van Microsoft Graph  | [MsalAngularjsDemoApp](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-angularjs/samples/MsalAngularjsDemoApp)
| ![Angular](media/sample-v2-code/logo_angular.png) [JavaScript (MSAL Angular)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular)| Aanroepen van Microsoft Graph  | [MSALAngularDemoApp](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-angular/samples/MSALAngularDemoApp) |

## <a name="web-applications"></a>Webtoepassingen

De volgende voorbeelden ziet u webtoepassingen die aanmelden van gebruikers. Aantal voorbeelden demonstreren ook de toepassing aanroepen van Microsoft Graph of uw eigen web-API met de identiteit van de gebruiker.

| Platform | Alleen gebruikers worden aangemeld | Gebruikers worden aangemeld en Microsoft Graph aanroepen |
| -------- | ------------------- | --------------------------------- |
| ![ASP.NET Core](media/sample-v2-code/logo_NETcore.png)</p>ASP.NET Core 2.2 | [Zelfstudie voor ASP.NET Core Web-App-gebruikers zich aanmeldt](https://aka.ms/aspnetcore-webapp-sign-in) | Hetzelfde voorbeeld de [ASP.NET Core Web-App roept Microsoft Graph](https://aka.ms/aspnetcore-webapp-call-msgraph) fase |
| ![ASP.NET](media/sample-v2-code/logo_NETframework.png)</p> ASP.NET | [ASP.NET-snelstartgids](https://github.com/AzureAdQuickstarts/AppModelv2-WebApp-OpenIDConnect-DotNet) </p> [dotnet-webapp-openidconnect-v2](https://github.com/azure-samples/active-directory-dotnet-webapp-openidconnect-v2)  |  [dotnet-admin-restricted-scopes-v2](https://github.com/azure-samples/active-directory-dotnet-admin-restricted-scopes-v2) </p> |[msgraph-training-aspnetmvcapp](https://github.com/microsoftgraph/msgraph-training-aspnetmvcapp)
| ![Node.js](media/sample-v2-code/logo_nodejs.png)  |                   | [Snelstartgids voor node.js](https://github.com/azureadquickstarts/appmodelv2-webapp-openidconnect-nodejs) |
| ![Ruby](media/sample-v2-code/logo_ruby.png) |                   | [msgraph-training-rubyrailsapp](https://github.com/microsoftgraph/msgraph-training-rubyrailsapp) |

## <a name="desktop-and-mobile-public-client-apps"></a>Desktop- en mobiele openbare client-apps

De volgende voorbeelden tonen openbare client toepassingen (desktop- of mobiele toepassingen) die toegang hebben tot de Microsoft Graph API of uw eigen web-API van de naam van een gebruiker. Deze clienttoepassingen gebruikt Microsoft Authentication Library (MSAL).

| Clienttoepassing | Platform | Stroom/verlenen | Aanroepen van Microsoft Graph | Een ASP.NET Core 2.0-web-API-aanroepen |
| ------------------ | -------- |  ----------| ---------- | ------------------------- |
| Desktop (WPF)      | ![.NET/C#](media/sample-v2-code/logo_NET.png) | [interactief](msal-authentication-flows.md#interactive)| [dotnet-desktop-msgraph-v2](https://github.com/azure-samples/active-directory-dotnet-desktop-msgraph-v2) | [dotnet-native-aspnetcore-v2](https://aka.ms/msidentity-aspnetcore-webapi) |
| Bureaublad (Console)   | ![.NET / C# (Desktop)](media/sample-v2-code/logo_NET.png) | [Geïntegreerde Windows-verificatie](msal-authentication-flows.md#integrated-windows-authentication) | [dotnet-iwa-v2](https://github.com/azure-samples/active-directory-dotnet-iwa-v2) |  |
| Bureaublad (Console)   | ![.NET / C# (Desktop)](media/sample-v2-code/logo_NETcore.png) | [Gebruikersnaam en wachtwoord](msal-authentication-flows.md#usernamepassword) |[dotnetcore-up-v2](https://github.com/azure-samples/active-directory-dotnetcore-console-up-v2) |  |
| Mobile (Android, iOS, UWP)   | ![.NET / C# (Xamarin)](media/sample-v2-code/logo_xamarin.png) | [interactief](msal-authentication-flows.md#interactive) |[xamarin-native-v2](https://github.com/azure-samples/active-directory-xamarin-native-v2) |  |
| Mobile (iOS)       | ![iOS / Objective-C of swift.](media/sample-v2-code/logo_iOS.png) | [interactief](msal-authentication-flows.md#interactive) |[ios-swift-native-v2](https://github.com/azure-samples/active-directory-ios-swift-native-v2) </p> [ios-native-nxoauth2-v2](https://github.com/azure-samples/active-directory-ios-native-nxoauth2-v2) |  |
| Mobile (Android)   | ![Android / Java](media/sample-v2-code/logo_Android.png) | [interactief](msal-authentication-flows.md#interactive) |  [android-native-v2](https://github.com/azure-samples/active-directory-android-native-v2 ) |  |

## <a name="daemon-applications"></a>Daemon-toepassingen

De volgende voorbeelden ziet een toepassing die toegang heeft tot de Microsoft Graph-API met een eigen identiteit (waarbij geen gebruiker).

| Clienttoepassing | Platform | Stroom/verlenen | Aanroepen van Microsoft Graph |
| ------------------ | -------- | ---------- | -------------------- |
| Console | ![.NET Core](media/sample-v2-code/logo_NETcore.png)</p> ASP.NET  | [Clientreferenties](msal-authentication-flows.md#client-credentials) | [dotnetcore-daemon-v2](https://github.com/azure-samples/active-directory-dotnetcore-daemon-v2) |
| Web-app | ![ASP.NET](media/sample-v2-code/logo_NETframework.png)</p> ASP.NET  | [Clientreferenties](msal-authentication-flows.md#client-credentials) | [dotnet-daemon-v2](https://github.com/azure-samples/active-directory-dotnet-daemon-v2) |

## <a name="headless-applications"></a>' Headless '-toepassingen

Het volgende voorbeeld toont een openbare client-toepassing die wordt uitgevoerd op een apparaat zonder een webbrowser. De app is een opdrachtregelprogramma, een app die wordt uitgevoerd op Linux- of Mac- of een IoT-toepassing. Het voorbeeld is uitgerust met een app toegang tot de Microsoft Graph-API, de naam van een gebruiker die zich aanmeldt interactief op een ander apparaat (zoals een mobiele telefoon). Deze clienttoepassing maakt gebruik van Microsoft Authentication Library (MSAL).

| Clienttoepassing | Platform | Stroom/verlenen | Aanroepen van Microsoft Graph |
| ------------------ | -------- |  ----------| ---------- |
| Bureaublad (Console)   | ![.NET / C# (Desktop)](media/sample-v2-code/logo_NETcore.png) | [De stroom van apparaat](msal-authentication-flows.md#device-code) |[dotnetcore-devicecodeflow-v2](https://github.com/azure-samples/active-directory-dotnetcore-devicecodeflow-v2) |

## <a name="web-apis"></a>Web-API's

De volgende voorbeelden laten zien voor het beveiligen van een web-API met het eindpunt van de Microsoft identity-platform, en hoe u een downstream-API aanroepen vanuit de web-API.

| Platform | Voorbeeld |
| -------- | ------------------- |
| ![ASP.NET Core](media/sample-v2-code/logo_NETcore.png)</p>ASP.NET Core 2.2 | ASP.NET Core web-API (service) van [dotnet-native-aspnetcore-v2](https://aka.ms/msidentity-aspnetcore-webapi-calls-msgraph)  |
| ![ASP.NET](media/sample-v2-code/logo_NET.png)</p>ASP.NET MVC | Web API (service) of [ms-identity-aspnet-webapi-onbehalfof](https://github.com/Azure-Samples/ms-identity-aspnet-webapi-onbehalfof) |

## <a name="other-microsoft-graph-samples"></a>Andere voorbeelden van Microsoft Graph

Voor meer informatie over [voorbeelden](https://github.com/microsoftgraph/msgraph-community-samples/tree/master/samples#aspnet) en zelfstudies die laten zien van verschillende gebruikspatronen voor de Microsoft Graph API, met inbegrip van verificatie met Azure AD, Zie [Microsoft Graph-Community voorbeelden en zelfstudies](https://github.com/microsoftgraph/msgraph-community-samples).

## <a name="see-also"></a>Zie ook

- [Azure Active Directory (v1.0) developer's guide](v1-overview.md)
- [Azure AD Graph API conceptuele en naslaginformatie](https://msdn.microsoft.com/library/azure/hh974476.aspx)
- [Azure AD Graph API-Helper-bibliotheek](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)
