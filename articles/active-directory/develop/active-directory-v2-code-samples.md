---
title: Azure Active Directory-codevoorbeelden | Microsoft Docs
description: Biedt een overzicht van de beschikbare Azure Active Directory (eindpunt V2)-codevoorbeelden, onderverdeeld op basis van scenario.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: mtillman
editor: ''
ms.assetid: a242a5ff-7300-40c2-ba83-fb6035707433
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/26/2018
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 39c2c5adef72db830ef226228017065c1b70f496
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="azure-active-directory-code-samples-v2-endpoint"></a>Azure Active Directory-codevoorbeelden (V2-eindpunt)

U kunt Microsoft Azure Active Directory (Azure AD):

- Verificatie en autorisatie toevoegen aan uw webtoepassingen en web-API's.
- Vereist een toegangstoken voor toegang tot een beveiligd web-API.

Dit artikel beschrijft kort en bevat koppelingen naar voorbeelden voor het Azure AD-V2-eindpunt. Deze voorbeelden laten zien hoe deze wordt uitgevoerd, samen met codefragmenten die u in uw toepassingen gebruiken kunt. In de voorbeeldpagina code vindt u gedetailleerde Leesmij-onderwerpen die helpen bij de vereisten voor installatie, en ingesteld. Opmerkingen in de code zijn er om te begrijpen van de kritieke secties.

> [!NOTE]
> Als u geïnteresseerd in de voorbeelden V1 bent, Zie [Azure AD-codevoorbeelden (V1-eindpunt)](active-directory-code-samples.md).

Zie inzicht in het eenvoudige scenario voor elk Voorbeeldtype [App-typen voor de Azure Active Directory v2.0-eindpunt](active-directory-v2-flows.md).

U kunt ook bijdragen aan de voorbeelden op GitHub. Voor meer informatie Zie [Microsoft Azure Active Directory-voorbeelden en documentatie](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="desktop-and-mobile-public-client-apps"></a>Desktop en mobiel openbare client-apps

De volgende voorbeelden ziet openbare client toepassingen (bureaublad/mobiele toepassingen) die toegang Microsoft Graph of een Web-API van de naam van een gebruiker tot.

clienttoepassing | Platform | Stroom/verlenen | Aanroepen Microsoft Graph | Een ASP.NET Core 2.0 Web-API aanroept
------------------ | -------- | ---------- | -------------------- | -------------------------
Bureaublad (WPF)      | .NET / C#  | Interactief | [DotNet-desktop-msgraph-v2](http://github.com/azure-samples/active-directory-dotnet-desktop-msgraph-v2) <p/> [DotNet-admin-beperkt-scopes-v2](https://github.com/azure-samples/active-directory-dotnet-admin-restricted-scopes-v2) | [DotNet native-aspnetcore v2](https://GitHub.com/azure-samples/active-directory-dotnet-native-aspnetcore-v2)
Mobile (UWP)   | .NET / C# (UWP) | Interactief | [DotNet-native-uwp-v2](https://github.com/azure-samples/active-directory-dotnet-native-uwp-v2) |
Mobile (Android, iOS, UWP)   | .NET / C# (Xamarin) | Interactief | [xamarin-native-v2](https://Github.com/azure-samples/active-directory-xamarin-native-v2) |
Mobile (iOS)       | iOS / Objective C of swift. | Interactief | [IOS-swift-native-v2](https://github.com/azure-samples/active-directory-ios-swift-native-v2) <p/> [IOS-native-nxoauth2-v2](https://github.com/azure-samples/active-directory-ios-native-nxoauth2-v2) |
Mobile (Android)   | Android / Java | Interactief |   [android-native-v2](https://github.com/azure-samples/active-directory-android-native-v2 ) |

## <a name="web-applications"></a>Web-apps

De volgende voorbeelden laten zien webtoepassingen die gebruikers aanmelden, Microsoft Graph aanroepen of met de identiteit van de gebruiker een web-API aanroepen.

 Platform | Alleen gebruikers worden aangemeld | Gebruikers worden aangemeld en roept Microsoft Graph 
 -------- | ------------------- | --------------------------------- 
ASP.NET 4.x | [appmodelv2 webapp-openIDConnect dotNet](https://GitHub.com/AzureAdQuickstarts/AppModelv2-WebApp-OpenIDConnect-DotNet) <p/> [DotNet webapp-openidconnect v2](https://GitHub.com/azure-samples/active-directory-dotnet-webapp-openidconnect-v2)  |              [ASPNET-connect-rest-sample](https://github.com/microsoftgraph/aspnet-connect-rest-sample)   
ASP.NET Core 2.0 | [aspnetcore webapp-openidconnect v2](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2) |              [aspnetcore-connect-voorbeeld](https://github.com/microsoftgraph/aspnetcore-connect-sample)   
Node.js      |                   | [AppModelv2 WebApp-OpenIDConnect nodejs](https://github.com/azureadquickstarts/appmodelv2-webapp-openidconnect-nodejs)     
Ruby      |                   | [Ruby-connect-rest-sample](https://github.com/microsoftgraph/ruby-connect-rest-sample)     

## <a name="daemon-applications"></a>Daemon-toepassingen

De volgende voorbeelden ziet bureaublad of web-toepassingen die toegang Microsoft Graph of een web-API met de toepassings-id (Er is geen gebruiker tot).

clienttoepassing | Platform | Stroom/verlenen | Aanroepen Microsoft Graph 
------------------ | -------- | ---------- | -------------------- 
Web-app | .NET / C#  | Clientreferenties | [DotNet-daemon-v2](https://github.com/azure-samples/active-directory-dotnet-daemon-v2) 

## <a name="single-page-applications-spa"></a>Toepassingen met één pagina (SPA)

Dit voorbeeld wordt het schrijven van een toepassing één pagina die wordt beveiligd met Azure AD.

 Platform |  Aanroepen Microsoft Graph 
 -------- |  --------------------- 
JavaScript (msal.js)  | [JavaScript-graphapi-v2](https://github.com/azure-samples/active-directory-javascript-graphapi-v2) 
JavaScript (msal.js + AngularJS) | [hoekvormige-connect-rest-sample](https://github.com/microsoftgraph/angular-connect-rest-sample) 
JavaScript (Hello.JS)  | [JavaScript-graphapi-web-v2](https://github.com/azure-samples/active-directory-javascript-graphapi-web-v2) 
JavaScript (hello.js + Hoekvormige 4) | [angular4-connect-voorbeeld](https://github.com/microsoftgraph/angular4-connect-sample) 

## <a name="web-apis"></a>Web-API's

### <a name="web-api-protected-by-azure-ad"></a>Web-API die zijn beveiligd door Azure AD

Het volgende voorbeeld laat zien hoe een web-API met de Azure AD-V2-eindpunt te beveiligen.

Platform | Voorbeeld | Beschrijving
 -------- | ------------------- | ---------------------
Node.js | [DotNet native-aspnetcore v2](https://GitHub.com/azure-samples/active-directory-dotnet-native-aspnetcore-v2) | Een ASP.NET Core Web API-aanroepen vanuit een WPF-toepassing met behulp van Azure AD-V2.

### <a name="web-api-calling-microsoft-graph-or-another-web-api"></a>Web-API aanroepen van Microsoft Graph of een andere Web-API

Dit voorbeeld is nog niet beschikbaar.

## <a name="other-microsoft-graph-samples"></a>Andere voorbeelden Microsoft Graph

Voor meer informatie over [voorbeelden](https://github.com/microsoftgraph/msgraph-community-samples/tree/master/samples#aspnet) en zelfstudies die laten verschillende gebruikspatronen voor Microsoft Graph API zien, zoals verificatie met Azure AD, Zie [Microsoft Graph-Community-voorbeelden en zelfstudies](https://github.com/microsoftgraph/msgraph-community-samples).

## <a name="see-also"></a>Zie ook

[Ontwikkelaarshandleiding voor Azure Active Directory](active-directory-developers-guide.md)

[Azure AD Graph API conceptuele en verwijzingen](https://msdn.microsoft.com/library/azure/hh974476.aspx)

[Azure AD Graph API Hulpbibliotheek](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)
