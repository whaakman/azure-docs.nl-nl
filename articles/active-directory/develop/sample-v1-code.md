---
title: Azure Active Directory-codevoorbeelden | Microsoft Docs
description: Biedt een overzicht van de Azure Active Directory (v1.0 eindpunt) codevoorbeelden, geordend op scenario.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: mtillman
editor: ''
ms.assetid: a242a5ff-7300-40c2-ba83-fb6035707433
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: c6cf9681804f9576d6c13a52161843d64244b1e8
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55097687"
---
# <a name="azure-active-directory-code-samples-v10-endpoint"></a>Azure Active Directory-codevoorbeelden (v1.0 eindpunt)

[!INCLUDE [active-directory-develop-applies-v1-adal](../../../includes/active-directory-develop-applies-v1-adal.md)]

Microsoft Azure Active Directory (Azure AD) kunt u verificatie en autorisatie toevoegen aan uw web-apps en web-API's.

In deze sectie vindt u koppelingen naar voorbeelden die u gebruiken kunt voor meer informatie over het Azure AD v1.0-eindpunt. Deze voorbeelden laten zien hoe dit werkt samen met codefragmenten die u in uw toepassingen gebruiken kunt. Op de pagina van het voorbeeld van code vindt u gedetailleerde lezen-me onderwerpen die u bij de vereisten, installatie en configuratie helpen. En de code is opgenomen als u informatie over de kritieke secties.

> [!NOTE]
> Als u geïnteresseerd in Azure AD V2-codevoorbeelden bent, Zie [v2.0-codevoorbeelden per scenario](sample-v2-code.md).

Zie voor meer informatie over het algemeen scenario voor elk Voorbeeldtype, [Verificatiescenario's voor Azure AD](authentication-scenarios.md).

U kunt ook bijdragen aan onze voorbeelden op GitHub. Voor meer informatie Zie [Microsoft Azure Active Directory-voorbeelden en documentatie](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="single-page-applications"></a>Toepassingen met één pagina

In dit voorbeeld laat zien hoe het schrijven van een toepassing één pagina die wordt beveiligd met Azure AD.

 Platform | Een eigen API-aanroepen | Een andere Web-API-aanroepen
 -------- |  --------------------- | ------------------ | ----------------
![Javascript](media/sample-v2-code/logo_js.png) | [javascript-singlepageapp](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) |
![Angular JS](media/sample-v2-code/logo_angular.png) | [angularjs-singlepageapp](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp) | [angularjs-singlepageapp-cors](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp-dotnet-webapi)

## <a name="web-applications"></a>Webtoepassingen

### <a name="web-applications-signing-in-users-calling-microsoft-graph-or-a-web-api-with-the-users-identity"></a>Web-Apps gebruikers aanmelden, aanroepen van Microsoft Graph of een Web-API met de identiteit van de gebruiker

De volgende voorbeelden ziet u Web-Apps ondertekenen van gebruikers. Sommige van deze toepassingen ook de Microsoft Graph of uw eigen Web-API, de naam van de aangemelde gebruiker niet aanroepen.

 Platform | Alleen gebruikers worden aangemeld | Aanroepen van Microsoft Graph of AAD Graph| Een andere ASP.NET of ASP.NET Core 2.0 Web-API-aanroepen
 -------- | ------------------- | --------------------- | -------------------------
![ASP.NET](media/sample-v2-code/logo_NETcore.png)<p/>ASP.NET Core 2.0 | [dotnet-webapp-openidconnect-aspnetcore](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore) | [webapp-webapi-multitenant-openidconnect-aspnetcore](https://github.com/Azure-Samples/active-directory-webapp-webapi-multitenant-openidconnect-aspnetcore/) <p/>(AAD Graph) | [dotnet-webapp-webapi-openidconnect-aspnetcore](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore)
![ASP.NET 4.5](media/sample-v2-code/logo_NETframework.png)<p/> ASP.NET 4.5 | [webApp-openidconnect-dotnet](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-aspnetwebapp-v1) <p/> [webapp-WSFederation-dotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-wsfederation) <p/> [dotnet-webapp-webapi-oauth2-useridentity](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity) | [dotnet-webapp-multitenant-openidconnect](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect)<p/> (AAD Graph) |
![Python](media/sample-v2-code/logo_python.png) | | [python-webapp-graphapi](https://github.com/Azure-Samples/active-directory-python-webapp-graphapi)  |
![Java](media/sample-v2-code/logo_java.png)  | | [java-webapp-openidconnect](https://github.com/azure-samples/active-directory-java-webapp-openidconnect)  |
![PHP](media/sample-v2-code/logo_php.png) | | [php-graphapi-web](https://github.com/Azure-Samples/active-directory-php-graphapi-web)  |

### <a name="web-applications-demonstrating-role-based-access-control-authorization"></a>Web-Apps aan te tonen op rollen gebaseerd toegangsbeheer (autorisatie)

De volgende voorbeelden laten zien hoe voor het implementeren van op rollen gebaseerd toegangsbeheer (RBAC). RBAC wordt gebruikt om de machtigingen van bepaalde functies in een webtoepassing voor bepaalde gebruikers te beperken. De gebruikers zijn gemachtigd, afhankelijk van of ze deel uitmaken van een **Azure AD-groep** of een bepaalde toepassing **rol**.

Platform | Voorbeeld
 -------- | -------------------
![ASP.NET 4.5](media/sample-v2-code/logo_NETframework.png)<p/> ASP.NET 4.5 | [dotnet-webapp-groupclaims](https://github.com/Azure-Samples/active-directory-dotnet-webapp-groupclaims) <p/>  [dotnet-webapp-roleclaims](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) | Een .NET 4.5 MVC-web-app die gebruikmaakt van Azure AD **rollen** voor autorisatie

## <a name="desktop-and-mobile-public-client-applications-calling-microsoft-graph-or-a-web-api"></a>Desktop- en mobiele openbare clienttoepassingen aanroepen van Microsoft Graph of een Web-API

De volgende voorbeelden ziet u openbare clienttoepassingen (desktop/mobiele toepassingen) die toegang hebben tot de Microsoft Graph of een Web-API van de naam van een gebruiker. Afhankelijk van de apparaten en platforms, kunnen toepassingen gebruikers aanmelden op verschillende manieren (stromen/verleent): 

- interactief,
- op de achtergrond (met geïntegreerde Windows-verificatie op Windows of gebruikersnaam en wachtwoord) 
- of zelfs door over te dragen de interactieve aanmelding met een ander apparaat (apparaat codestroom gebruikt op apparaten die niet-webbesturingselementen).

Clienttoepassing | Platform | Stroom/verlenen | Aanroepen van Microsoft Graph | Een ASP.NET- of ASP.NET Core 2.x Web-API-aanroepen
------------------ | -------- | ---------- | -------------------- | -------------------------
Desktop (WPF)           | ![.NET/C#](media/sample-v2-code/logo_NET.png)  | Interactief | Onderdeel van [dotnet-native-multitarget](https://github.com/azure-samples/active-directory-dotnet-native-multitarget) | [Dotnet-native-desktop](https://github.com/Azure-Samples/active-directory-dotnet-native-desktop) </p> [dotnet-native-aspnetcore](https://azure.microsoft.com/resources/samples/active-directory-dotnet-native-aspnetcore/)</p> [dotnet-webapi-manual-jwt-validation](https://github.com/azure-samples/active-directory-dotnet-webapi-manual-jwt-validation)
Mobile (UWP)            | .![.NET/C#/UWP](media/sample-v2-code/logo_Windows.png)   | Interactief | [dotnet-native-uwp-wam](https://github.com/azure-samples/active-directory-dotnet-native-uwp-wam) </p> In dit voorbeeld wordt [WAM](https://docs.microsoft.com/windows/uwp/security/web-account-manager), niet [ADAL.NET](https://aka.ms/adalnet)|  [DotNet-windows-store](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) (UWP-toepassing met behulp van ADAL.NET om aan te roepen van één tenant Web-API) </p> [DotNet-webapi-multitenant-windows-store](https://github.com/Azure-Samples/active-directory-dotnet-webapi-multitenant-windows-store) (UWP-toepassing met behulp van ADAL.NET om aan te roepen een multitenant-Web-API)|
Mobile (Android, iOS, UWP)   | ![.NET / C# (Xamarin)](media/sample-v2-code/logo_xamarin.png) | Interactief | [dotnet-native-multitarget](https://github.com/azure-samples/active-directory-dotnet-native-multitarget) |
Mobile (Android)           | ![Android / Java](media/sample-v2-code/logo_Android.png) | Interactief |   [android](https://github.com/Azure-Samples/active-directory-android) |
Mobile (iOS)           | ![iOS / Objective-C of swift.](media/sample-v2-code/logo_iOS.png) | Interactief |   [nativeClient-iOS](https://github.com/azureadquickstarts/nativeclient-ios) |
Bureaublad (Console)          | ![.NET/C#](media/sample-v2-code/logo_NET.png) | Gebruikersnaam / wachtwoord </p>  Geïntegreerde Windows-verificatie | | [DotNet native headless](https://github.com/azure-samples/active-directory-dotnet-native-headless)
Bureaublad (Console)          | ![Java-console](media/sample-v2-code/logo_Java.png) | Gebruikersnaam / wachtwoord | | [Java native headless](https://github.com/Azure-Samples/active-directory-java-native-headless)
Bureaublad (Console)           | ![.NET Core/C#](media/sample-v2-code/logo_NETcore.png) | De stroom van apparaat | | [dotnet-deviceprofile](https://github.com/Azure-Samples/active-directory-dotnet-deviceprofile)

## <a name="daemon-applications-accessing-web-apis-with-the-applications-identity"></a>Daemon-toepassingen (toegang tot Web-API's met de identiteit van de toepassing)

De volgende voorbeelden tonen desktop of web-toepassingen die toegang hebben tot de Microsoft Graph of een web-API zonder gebruiker (met de toepassings-id).

Clienttoepassing | Platform | Stroom/verlenen | Een ASP.NET- of ASP.NET Core 2.0-Web-API wordt aangeroepen
------------------ | -------- | ---------- | -------------------- | -------------------------
Daemon-app (Console)          | ![.NET](media/sample-v2-code/logo_NETframework.png) | De referenties van de client met app-geheim of certificaat | [dotnet-daemon](https://github.com/azure-samples/active-directory-dotnet-daemon)</p> [dotnet-daemon-certificate-credential](https://github.com/azure-samples/active-directory-dotnet-daemon-certificate-credential)
Daemon-app (Console)         | ![.NET](media/sample-v2-code/logo_NETcore.png) | De referenties van de client met certificaat| [dotnetcore-daemon-certificate-credential](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-certificate-credential)
ASP.NET Web App  | ![.NET](media/sample-v2-code/logo_NETframework.png) | Clientreferenties | [dotnet-webapp-webapi-oauth2-appidentity](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-appidentity)

## <a name="web-apis"></a>Web-API's

### <a name="web-api-protected-by-azure-active-directory"></a>Web-API die wordt beveiligd door Azure Active Directory

Het volgende voorbeeld toont het beveiligen van een node.js-web-API met Azure AD.

In de vorige secties van dit artikel vindt u ook andere voorbeelden ter illustratie van een clienttoepassing **aanroepen** een ASP.NET- of ASP.NET Core **Web-API**. Deze voorbeelden niet opnieuw worden vermeld in deze sectie, maar vindt u deze in de laatste kolom van de tabellen boven of onder

Platform | Voorbeeld
 -------- | -------------------
![PHP](media/sample-v2-code/logo_nodejs.png)  | [node-webapi](https://github.com/Azure-Samples/active-directory-node-webapi)

### <a name="web-api-calling-microsoft-graph-or-another-web-api"></a>Web-API aanroepen van Microsoft Graph of een andere Web-API

De volgende voorbeelden ziet een web-API die een andere web-API aanroept. Het tweede voorbeeld laat zien hoe voorwaardelijke toegang worden verwerkt.

 Platform |  Aanroepen van Microsoft Graph | Een andere ASP.NET of ASP.NET Core 2.0 Web-API-aanroepen
 -------- |  --------------------- | -------------------------
![ASP.NET 4.5](media/sample-v2-code/logo_NETframework.png)<p/> ASP.NET 4.5 | [dotnet-webapi-onbehalfof](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof) <p/> [dotnet-webapi-onbehalfof-ca](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof-ca) |[dotnet-webapi-onbehalfof](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof) <p/> [dotnet-webapi-onbehalfof-ca](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof-ca)

## <a name="other-microsoft-graph-samples"></a>Andere voorbeelden van Microsoft Graph

Zie voor voorbeelden en zelfstudies die laten zien van verschillende gebruikspatronen voor de Microsoft Graph API, met inbegrip van verificatie met Azure AD [Microsoft Graph-Communityvoorbeelden en zelfstudies](https://github.com/microsoftgraph/msgraph-community-samples).

## <a name="see-also"></a>Zie ook

[Ontwikkelaarshandleiding voor Azure Active Directory](v1-overview.md)

[Azure Active Directory-verificatie-bibliotheken](active-directory-authentication-libraries.md)

[Azure AD Graph API conceptuele en naslaginformatie](https://msdn.microsoft.com/library/azure/hh974476.aspx)

[Azure AD Graph API-Helper-bibliotheek](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)
