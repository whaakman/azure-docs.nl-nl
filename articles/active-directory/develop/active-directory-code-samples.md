---
title: Azure Active Directory-codevoorbeelden | Microsoft Docs
description: Biedt een overzicht van Azure Active Directory (v1-eindpunt) codevoorbeelden, onderverdeeld op basis van scenario.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: mtillman
editor: ''
ms.assetid: a242a5ff-7300-40c2-ba83-fb6035707433
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/24/2018
ms.author: celested
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 299e0d4fa53f9b8a2aef2fc881b136aa41aacfe4
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2018
---
# <a name="azure-active-directory-code-samples-v1-endpoint"></a>Azure Active Directory-codevoorbeelden (V1-eindpunt)

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Verificatie en autorisatie toevoegen aan uw webtoepassingen en web-API's kunt u Microsoft Azure Active Directory (Azure AD).

Deze sectie vindt u koppelingen naar voorbeelden die kunt u meer informatie over het Azure AD-V1-eindpunt. Deze voorbeelden laten zien hoe deze samen met codefragmenten die u in uw toepassingen gebruiken kunt wordt uitgevoerd. In de voorbeeldpagina code vindt u gedetailleerde lezen-mij onderwerpen die u bij de vereisten, installatie en configuratie helpen. En de code is om te begrijpen van de kritieke secties toegelicht.

> [!NOTE]
> Als u geïnteresseerd in Azure AD-V2-codevoorbeelden bent, Zie [v2.0-codevoorbeelden door scenario](active-directory-v2-code-samples.md).

Zie inzicht in het eenvoudige scenario voor elk Voorbeeldtype [verificatie scenario's voor Azure AD](active-directory-authentication-scenarios.md).

U kunt ook bijdragen aan onze voorbeelden op GitHub. Voor meer informatie Zie [Microsoft Azure Active Directory-voorbeelden en documentatie](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="desktop-and-mobile-public-client-applications-calling-microsoft-graph-or-a-web-api"></a>Desktop en mobiel openbare clienttoepassingen aanroepen van Microsoft Graph of een Web-API

De volgende voorbeelden ziet openbare client toepassingen (bureaublad/mobiele toepassingen) die toegang Microsoft Graph of een Web-API van de naam van een gebruiker tot.

clienttoepassing | Platform | Stroom/verlenen | Aanroepen Microsoft Graph | Een ASP.NET- of ASP.NET Core 2.0 Web API-aanroepen
------------------ | -------- | ---------- | -------------------- | -------------------------
Bureaublad (WPF)           | .NET / C# | Interactief | [DotNet-native-multitarget](https://github.com/azure-samples/active-directory-dotnet-native-multitarget) | [DotNet-native-bureaublad](https://github.com/Azure-Samples/active-directory-dotnet-native-desktop) </p> [DotNet-native-aspnetcore](https://azure.microsoft.com/resources/samples/active-directory-dotnet-native-aspnetcore/)</p> [DotNet-webapi-handmatige-jwt-validatie](https://github.com/azure-samples/active-directory-dotnet-webapi-manual-jwt-validation)
Mobile (UWP)            | .NET / C#  | Interactief | [DotNet-native-uwp-wam](https://github.com/azure-samples/active-directory-dotnet-native-uwp-wam) |  [DotNet-windows-store](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) (één tenant Web-API) </p> [DotNet-webapi-multitenant-windows-store](https://github.com/Azure-Samples/active-directory-dotnet-webapi-multitenant-windows-store) (multitenant-Web-API)|
Mobile (Android, iOS, UWP)   | .NET / C# (Xamarin) | Interactief | [DotNet-native-multitarget](https://github.com/azure-samples/active-directory-dotnet-native-multitarget) |
Mobile (Android)           | Android/Java | Interactief |   [Android](https://github.com/Azure-Samples/active-directory-android) |
Mobile (iOS)           | iOS/Objective-C | Interactief |   [nativeClient iOS](https://github.com/azureadquickstarts/nativeclient-ios) |
Bureaublad (Console)          | .NET / C# | Gebruikersnaam / wachtwoord </p> Geïntegreerde Windows-verificatie | | [DotNet systeemeigen headless](https://github.com/azure-samples/active-directory-dotnet-native-headless)
Bureaublad (Console)           | .NET core / C# | Het profiel van apparaat | | [DotNet deviceprofile](https://github.com/Azure-Samples/active-directory-dotnet-deviceprofile)

## <a name="web-applications"></a>Webtoepassingen

### <a name="web-applications-signing-in-users-calling-microsoft-graph-or-a-web-api-with-the-users-identity"></a>Webtoepassingen gebruikers aanmelden, het aanroepen van Microsoft Graph of een Web-API met de identiteit van de gebruiker

 Platform | Alleen gebruikers worden aangemeld | Aanroepen Microsoft Graph of AAD-grafiek| Een andere ASP.NET of ASP.NET Core 2.0 Web API-aanroepen
 -------- | ------------------- | --------------------- | -------------------------
ASP.NET 4.5 | [webApp-openidconnect-dotnet](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-aspnetwebapp-v1) </p> [WebApp-WSFederation-dotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-wsfederation) | [DotNet-webapp-multitenant-openidconnect](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect) (AAD grafiek) |
ASP.NET Core 2.0 | [DotNet webapp-openidconnect aspnetcore](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore) | [WebApp-webapi-multitenant-openidconnect-aspnetcore](https://github.com/Azure-Samples/active-directory-webapp-webapi-multitenant-openidconnect-aspnetcore/) (AAD grafiek) | [DotNet-webapp-webapi-openidconnect-aspnetcore](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore)
ASP.NET 4.5 | [DotNet-webapp-webapi-oauth2-useridentity](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity) | |
Python | | [Python-Web-App-graphapi](https://github.com/Azure-Samples/active-directory-python-webapp-graphapi)  |
Java | | [Java-Web-App-openidconnect](https://github.com/azure-samples/active-directory-java-webapp-openidconnect)  |
PHP | | [PHP-graphapi-web](https://github.com/Azure-Samples/active-directory-php-graphapi-web)  |

### <a name="web-applications-demonstrating-role-based-access-control-authorization"></a>Webtoepassingen demonstratie van op rollen gebaseerde toegangsbeheer (autorisatie)

De volgende voorbeelden laten zien hoe op rollen gebaseerde toegangsbeheer implementeren, die wordt gebruikt om de machtigingen van bepaalde functies van een webtoepassing tot bepaalde gebruikers te beperken. De gebruikers zijn gemachtigd, afhankelijk van of ze bij een Azure AD-groep of rol horen.

Platform | Voorbeeld | Beschrijving
 -------- | ------------------- | ---------------------
ASP.NET 4.5 | [DotNet-webapp-groupclaims](https://github.com/Azure-Samples/active-directory-dotnet-webapp-groupclaims) | Een .NET 4.5 MVC-web-app die gebruikmaakt van Azure AD **groepen** voor autorisatie
ASP.NET 4.5 | [DotNet-webapp-roleclaims](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) | Een .NET 4.5 MVC-web-app die gebruikmaakt van Azure AD **rollen** voor autorisatie

## <a name="daemon-applications-accessing-web-apis-with-the-applications-identity"></a>Daemon-toepassingen (toegang tot Web-API's met de identiteit van de toepassing)

De volgende voorbeelden ziet bureaublad of web-toepassingen die toegang Microsoft Graph of een web-API met geen enkele gebruiker (met de toepassings-id tot).

clienttoepassing | Platform | Stroom/verlenen | Aanroepen Microsoft Graph | Een ASP.NET- of ASP.NET Core 2.0 Web API-aanroepen
------------------ | -------- | ---------- | -------------------- | -------------------------
Daemon-app (Console)          | .NET / C#  | De referenties van de client met de app geheim of certificaat | | [DotNet-daemon](https://github.com/azure-samples/active-directory-dotnet-daemon)</p> [DotNet-daemon-certificaat-credential](https://github.com/azure-samples/active-directory-dotnet-daemon-certificate-credential)
Daemon-app (Console)         | .NET core / C# | De referenties van de client met certificaat| | [dotnetcore-daemon-certificaat-credential](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-certificate-credential)
Bureaublad            | Java | Clientreferenties |   [Java systeemeigen headless](https://github.com/azure-samples/active-directory-java-native-headless) |
ASP.NET-Web-App  | .NET / C# | Clientreferenties |    | [DotNet-webapp-webapi-oauth2-appidentity](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-appidentity)

## <a name="web-apis"></a>Web-API's

### <a name="web-api-protected-by-azure-active-directory"></a>Web-API die zijn beveiligd door Azure Active Directory

Het volgende voorbeeld laat zien hoe een node.js-web-API met Azure AD te beveiligen.

Platform | Voorbeeld | Beschrijving
 -------- | ------------------- | ---------------------
Node.js | [knooppunt webapi](https://github.com/Azure-Samples/active-directory-node-webapi) |  NodeJS web-API die is beveiligd met Azure AD en OAuth 2.0-toegangstokens.

### <a name="web-api-calling-microsoft-graph-or-another-web-api"></a>Web-API aanroepen van Microsoft Graph of een andere Web-API

De volgende voorbeelden ziet u een web-API die een andere web-API aanroept. Het tweede voorbeeld laat zien hoe voorwaardelijke toegang verwerken.

 Platform |  Aanroepen Microsoft Graph | Een andere ASP.NET of ASP.NET Core 2.0 Web API-aanroepen
 -------- |  --------------------- | -------------------------
ASP.NET 4.5 | [DotNet-webapi-onbehalfof](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof) |[DotNet-webapi-onbehalfof](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof)
ASP.NET 4.5 | [DotNet-webapi-onbehalfof-ca](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof-ca) |[DotNet-webapi-onbehalfof-ca](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof-ca)

## <a name="single-page-applications"></a>Toepassingen met één pagina

Dit voorbeeld wordt het schrijven van een toepassing één pagina die wordt beveiligd met Azure AD.

 Platform |  Aanroepen Microsoft Graph | Een eigen API-aanroepen
 -------- |  --------------------- | -------------------------
JavaScript (hoek) / ASP.NET 4.x |  | [angularjs singlepageapp](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp)

## <a name="other-microsoft-graph-samples"></a>Andere voorbeelden Microsoft Graph

Zie voor voorbeelden en zelfstudies die laten verschillende gebruikspatronen voor Microsoft Graph API zien, zoals verificatie met Azure AD [Microsoft Graph Community voorbeelden en zelfstudies](https://github.com/microsoftgraph/msgraph-community-samples).

## <a name="see-also"></a>Zie ook

[Ontwikkelaarshandleiding Azure Active Directory](active-directory-developers-guide.md)

[Azure AD Graph API conceptuele en verwijzingen](https://msdn.microsoft.com/library/azure/hh974476.aspx)

[Azure AD Graph API Hulpbibliotheek](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)
