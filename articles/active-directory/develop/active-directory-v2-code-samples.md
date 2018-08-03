---
title: Azure Active Directory-codevoorbeelden | Microsoft Docs
description: Biedt een overzicht van de beschikbare Azure Active Directory (V2-eindpunt) codevoorbeelden, geordend op scenario.
services: active-directory
documentationcenter: dev-center-name
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: a242a5ff-7300-40c2-ba83-fb6035707433
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/26/2018
ms.author: celested
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 51a50693a97a011d00d055eff8d3a0846512bce5
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39441405"
---
# <a name="azure-active-directory-code-samples-v2-endpoint"></a>Azure Active Directory-codevoorbeelden (V2-eindpunt)

U kunt Microsoft Azure Active Directory (Azure AD) om te gebruiken:

- Verificatie en autorisatie toevoegen aan uw webtoepassingen en web-API's.
- Vereisen dat een toegangstoken voor toegang tot een beveiligde web-API.

Dit artikel worden kort en verstrekt u koppelingen naar voorbeelden van het Azure AD V2-eindpunt. Deze voorbeelden laten zien hoe dit werkt, samen met codefragmenten die u in uw toepassingen gebruiken kunt. Op de pagina van het voorbeeld van code vindt u gedetailleerde Leesmij-onderwerpen die aan de vereisten, installatie, en instellen. Opmerkingen in de code, zijn er om te begrijpen van de kritieke secties.

> [!NOTE]
> Als u geïnteresseerd in V1-voorbeelden bent, Zie [Azure AD-codevoorbeelden (V1-eindpunt)](active-directory-code-samples.md).

Zie voor meer informatie over het algemeen scenario voor elk Voorbeeldtype, [App-typen voor de Azure Active Directory v2.0-eindpunt](active-directory-v2-flows.md).

U kunt ook bijdragen aan de voorbeelden op GitHub. Voor meer informatie Zie [Microsoft Azure Active Directory-voorbeelden en documentatie](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="desktop-and-mobile-public-client-apps"></a>Desktop- en mobiele openbare client-apps

De volgende voorbeelden tonen openbare client toepassingen (desktop/mobiele toepassingen) die toegang hebben tot de Microsoft Graph of een Web-API van de naam van een gebruiker.

Clienttoepassing | Platform | Stroom/verlenen | Aanroepen van Microsoft Graph | Een ASP.NET Core 2.0-Web-API-aanroepen
------------------ | -------- | ---------- | -------------------- | -------------------------
(WPF) Desktop      | .NET / C#  | Interactief | [DotNet-desktop-msgraph-v2](http://github.com/azure-samples/active-directory-dotnet-desktop-msgraph-v2) <p/> [DotNet-admin-beperkt-scopes-v2](https://github.com/azure-samples/active-directory-dotnet-admin-restricted-scopes-v2) | [DotNet-native-aspnetcore-v2](https://GitHub.com/azure-samples/active-directory-dotnet-native-aspnetcore-v2)
Mobile (UWP)   | .NET / C# (UWP) | Interactief | [DotNet-native-uwp-v2](https://github.com/azure-samples/active-directory-dotnet-native-uwp-v2) |
Mobile (Android, iOS, UWP)   | .NET / C# (Xamarin) | Interactief | [xamarin-native-v2](https://Github.com/azure-samples/active-directory-xamarin-native-v2) |
Mobile (iOS)       | iOS / Objective-C of swift. | Interactief | [IOS swift-native v2](https://github.com/azure-samples/active-directory-ios-swift-native-v2) <p/> [IOS-native-nxoauth2-v2](https://github.com/azure-samples/active-directory-ios-native-nxoauth2-v2) |
Mobile (Android)   | Android / Java | Interactief |   [android-native-v2](https://github.com/azure-samples/active-directory-android-native-v2 ) |

## <a name="web-applications"></a>Webtoepassingen

De volgende voorbeelden ziet u web-apps die gebruikers aanmelden, Microsoft Graph aanroepen of een web-API met de identiteit van de gebruiker aanroepen.

 Platform | Alleen gebruikers worden aangemeld | Gebruikers worden aangemeld en Microsoft Graph aanroepen 
 -------- | ------------------- | --------------------------------- 
ASP.NET 4.x | [appmodelv2-Web-App-openIDConnect-dotNet](https://GitHub.com/AzureAdQuickstarts/AppModelv2-WebApp-OpenIDConnect-DotNet) <p/> [DotNet-Web-App-openidconnect-v2](https://GitHub.com/azure-samples/active-directory-dotnet-webapp-openidconnect-v2)  |              [ASPNET-verbinding maken-rest-voorbeeld](https://github.com/microsoftgraph/aspnet-connect-rest-sample)   
ASP.NET Core 2.0 | [aspnetcore-Web-App-openidconnect-v2](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2) |              [aspnetcore-verbinding maken-voorbeeld](https://github.com/microsoftgraph/aspnetcore-connect-sample)   
Node.js      |                   | [AppModelv2-Web-App-OpenIDConnect-nodejs](https://github.com/azureadquickstarts/appmodelv2-webapp-openidconnect-nodejs)     
Ruby      |                   | [Ruby-verbinding maken-rest-voorbeeld](https://github.com/microsoftgraph/ruby-connect-rest-sample)     

## <a name="daemon-applications"></a>Daemon-toepassingen

De volgende voorbeelden tonen desktop of web-toepassingen die toegang hebben tot de Microsoft Graph of een web-API met de toepassings-id (Er is geen gebruiker).

Clienttoepassing | Platform | Stroom/verlenen | Aanroepen van Microsoft Graph 
------------------ | -------- | ---------- | -------------------- 
Web-app | .NET / C#  | Clientreferenties | [DotNet-daemon-v2](https://github.com/azure-samples/active-directory-dotnet-daemon-v2) 

## <a name="single-page-applications-spa"></a>Toepassingen met één pagina (SPA)

In dit voorbeeld laat zien hoe het schrijven van een toepassing één pagina die wordt beveiligd met Azure AD.

 Platform |  Aanroepen van Microsoft Graph 
 -------- |  --------------------- 
JavaScript (msal.js)  | [JavaScript-graphapi-v2](https://github.com/azure-samples/active-directory-javascript-graphapi-v2) 
JavaScript (msal.js + AngularJS) | [angular-verbinding maken-rest-voorbeeld](https://github.com/microsoftgraph/angular-connect-rest-sample) 
JavaScript (Hello.JS)  | [JavaScript-graphapi-web-v2](https://github.com/azure-samples/active-directory-javascript-graphapi-web-v2) 
JavaScript (hello.js + Angular 4) | [angular4-verbinding maken-voorbeeld](https://github.com/microsoftgraph/angular4-connect-sample) 

## <a name="web-apis"></a>Web-API's

### <a name="web-api-protected-by-azure-ad"></a>Web-API die wordt beveiligd door Azure AD

Het volgende voorbeeld toont het beveiligen van een web-API met de Azure AD V2-eindpunt.

Platform | Voorbeeld | Beschrijving
 -------- | ------------------- | ---------------------
Node.js | [DotNet-native-aspnetcore-v2](https://GitHub.com/azure-samples/active-directory-dotnet-native-aspnetcore-v2) | Een ASP.NET Core Web-API-aanroepen vanuit een WPF-toepassing met behulp van Azure AD V2.

### <a name="web-api-calling-microsoft-graph-or-another-web-api"></a>Web-API aanroepen van Microsoft Graph of een andere Web-API

In dit voorbeeld is nog niet beschikbaar.

## <a name="other-microsoft-graph-samples"></a>Andere voorbeelden van Microsoft Graph

Voor meer informatie over [voorbeelden](https://github.com/microsoftgraph/msgraph-community-samples/tree/master/samples#aspnet) en zelfstudies die laten zien van verschillende gebruikspatronen voor de Microsoft Graph API, met inbegrip van verificatie met Azure AD, Zie [Microsoft Graph-Community voorbeelden en zelfstudies](https://github.com/microsoftgraph/msgraph-community-samples).

## <a name="see-also"></a>Zie ook

[Ontwikkelaarshandleiding voor Azure Active Directory](azure-ad-developers-guide.md)

[Azure AD Graph API conceptuele en naslaginformatie](https://msdn.microsoft.com/library/azure/hh974476.aspx)

[Azure AD Graph API-Helper-bibliotheek](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)
