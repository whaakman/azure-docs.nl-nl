---
title: 'Azure Active Directory v1.0: codevoorbeelden | Microsoft Docs'
description: Biedt een overzicht van de Azure Active Directory (v1.0 eindpunt) codevoorbeelden, geordend op scenario.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.assetid: a242a5ff-7300-40c2-ba83-fb6035707433
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: ryanwi
ms.reviewer: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: ca03c8d52de1580b755833dd990d1d7671f372ca
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67785702"
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
 -------- |  --------------------- | ------------------ 
![Deze afbeelding ziet u de JavaScript-logo](media/sample-v2-code/logo_js.png) | [javascript-singlepageapp](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) |
![Deze afbeelding ziet u de Angular JS-logo](media/sample-v2-code/logo_angular.png) | [angularjs-singlepageapp](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp) | [angularjs-singlepageapp-cors](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp-dotnet-webapi)

## <a name="web-applications"></a>Webtoepassingen

### <a name="web-applications-signing-in-users-calling-microsoft-graph-or-a-web-api-with-the-users-identity"></a>Web-Apps gebruikers aanmelden, aanroepen van Microsoft Graph of een Web-API met de identiteit van de gebruiker

De volgende voorbeelden ziet u Web-Apps ondertekenen van gebruikers. Sommige van deze toepassingen ook de Microsoft Graph of uw eigen Web-API, de naam van de aangemelde gebruiker niet aanroepen.

 Platform | Alleen gebruikers worden aangemeld | Aanroepen van Microsoft Graph of AAD Graph| Een andere ASP.NET of ASP.NET Core 2.0 Web-API-aanroepen
 -------- | ------------------- | --------------------- | -------------------------
![Deze afbeelding ziet u de ASP.NET-logo](media/sample-v2-code/logo_NETcore.png)</p>ASP.NET Core 2.0 | [dotnet-webapp-openidconnect-aspnetcore](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore) | [webapp-webapi-multitenant-openidconnect-aspnetcore](https://github.com/Azure-Samples/active-directory-webapp-webapi-multitenant-openidconnect-aspnetcore/) </p>(AAD Graph) | [dotnet-webapp-webapi-openidconnect-aspnetcore](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore)
![Deze afbeelding ziet u de ASP.NET-logo](media/sample-v2-code/logo_NETframework.png)</p> ASP.NET 4.5 | [webApp-openidconnect-dotnet](quickstart-v1-aspnet-webapp.md) </p> [webapp-WSFederation-dotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-wsfederation) </p> [dotnet-webapp-webapi-oauth2-useridentity](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity) | [dotnet-webapp-multitenant-openidconnect](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect)</p> (AAD Graph) |
![Deze afbeelding ziet u het Python-logo](media/sample-v2-code/logo_python.png) | | [python-webapp-graphapi](https://github.com/Azure-Samples/active-directory-python-webapp-graphapi)  |
![Deze afbeelding ziet u de Java-logboek](media/sample-v2-code/logo_java.png)  | | [java-webapp-openidconnect](https://github.com/azure-samples/active-directory-java-webapp-openidconnect)  |
![Deze afbeelding ziet u de PHP-logo](media/sample-v2-code/logo_php.png) | | [php-graphapi-web](https://github.com/Azure-Samples/active-directory-php-graphapi-web)  |

### <a name="web-applications-demonstrating-role-based-access-control-authorization"></a>Web-Apps aan te tonen op rollen gebaseerd toegangsbeheer (autorisatie)

De volgende voorbeelden laten zien hoe voor het implementeren van op rollen gebaseerd toegangsbeheer (RBAC). RBAC wordt gebruikt om de machtigingen van bepaalde functies in een webtoepassing voor bepaalde gebruikers te beperken. De gebruikers zijn gemachtigd, afhankelijk van of ze deel uitmaken van een **Azure AD-groep** of een bepaalde toepassing **rol**.

Platform | Voorbeeld |
 -------- | ------------------- |
![Deze afbeelding ziet u de ASP.NET-logo](media/sample-v2-code/logo_NETframework.png)</p> ASP.NET 4.5 | [dotnet-webapp-groupclaims](https://github.com/Azure-Samples/active-directory-dotnet-webapp-groupclaims) </p>  [dotnet-webapp-roleclaims](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) | Een .NET 4.5 MVC-web-app die gebruikmaakt van Azure AD **rollen** voor autorisatie

## <a name="desktop-and-mobile-public-client-applications-calling-microsoft-graph-or-a-web-api"></a>Desktop- en mobiele openbare clienttoepassingen aanroepen van Microsoft Graph of een Web-API

De volgende voorbeelden ziet u openbare clienttoepassingen (-Desktop/pmobile toepassingen) die toegang hebben tot de Microsoft Graph of een Web-API van de naam van een gebruiker. Afhankelijk van de apparaten en platforms, kunnen toepassingen gebruikers aanmelden op verschillende manieren (stromen/verleent):

- Interactief
- Op de achtergrond (met geïntegreerde Windows-verificatie op Windows of gebruikersnaam en wachtwoord)
- Door het overdragen van de interactieve aanmelding met een ander apparaat (apparaat codestroom gebruikt op apparaten die niet-webbesturingselementen)

Clienttoepassing | Platform | Stroom/verlenen | Aanroepen van Microsoft Graph | Een ASP.NET- of ASP.NET Core 2.x Web-API-aanroepen
------------------ | -------- | ---------- | -------------------- | -------------------------
Desktop (WPF)           | ![Deze afbeelding ziet u de .NET /C# logo](media/sample-v2-code/logo_NET.png)  | Interactief | Onderdeel van [dotnet-native-multitarget](https://github.com/azure-samples/active-directory-dotnet-native-multitarget) | [Dotnet-native-desktop](https://github.com/Azure-Samples/active-directory-dotnet-native-desktop) </p> [dotnet-native-aspnetcore](https://azure.microsoft.com/resources/samples/active-directory-dotnet-native-aspnetcore/)</p> [dotnet-webapi-manual-jwt-validation](https://github.com/azure-samples/active-directory-dotnet-webapi-manual-jwt-validation)
Mobile (UWP)            | .![Deze afbeelding ziet u de .NET /C#/UWP](media/sample-v2-code/logo_Windows.png)   | Interactief | [dotnet-native-uwp-wam](https://github.com/azure-samples/active-directory-dotnet-native-uwp-wam) </p> In dit voorbeeld wordt [WAM](/windows/uwp/security/web-account-manager), niet [ADAL.NET](https://aka.ms/adalnet)|  [DotNet-windows-store](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) (UWP-toepassing met behulp van ADAL.NET om aan te roepen van één tenant Web-API) </p> [DotNet-webapi-multitenant-windows-store](https://github.com/Azure-Samples/active-directory-dotnet-webapi-multitenant-windows-store) (UWP-toepassing met behulp van ADAL.NET om aan te roepen een multitenant-Web-API)|
Mobile (Android, iOS, UWP)   | ![Deze afbeelding ziet u de .NET /C# (Xamarin)](media/sample-v2-code/logo_xamarin.png) | Interactief | [dotnet-native-multitarget](https://github.com/azure-samples/active-directory-dotnet-native-multitarget) |
Mobile (Android)           | ![Deze afbeelding ziet u de Android-logo](media/sample-v2-code/logo_Android.png) | Interactief |   [android](https://github.com/Azure-Samples/active-directory-android) |
Mobile (iOS)           | ![Deze afbeelding ziet u iOS / Objective-C of Swift.](media/sample-v2-code/logo_iOS.png) | Interactief |   [nativeClient-iOS](https://github.com/azureadquickstarts/nativeclient-ios) |
Bureaublad (Console)          | ![Deze afbeelding ziet u de .NET /C# logo](media/sample-v2-code/logo_NET.png) | Gebruikersnaam / wachtwoord </p>  Geïntegreerde Windows-verificatie | | [DotNet native headless](https://github.com/azure-samples/active-directory-dotnet-native-headless)
Bureaublad (Console)          | ![Deze afbeelding ziet u de Java-logo](media/sample-v2-code/logo_Java.png) | Gebruikersnaam / wachtwoord | | [Java native headless](https://github.com/Azure-Samples/active-directory-java-native-headless)
Bureaublad (Console)           | ![Deze afbeelding ziet u de .NET Core /C# logo](media/sample-v2-code/logo_NETcore.png) | De stroom van apparaat | | [dotnet-deviceprofile](https://github.com/Azure-Samples/active-directory-dotnet-deviceprofile)

## <a name="daemon-applications-accessing-web-apis-with-the-applications-identity"></a>Daemon-toepassingen (toegang tot web-API's met de identiteit van de toepassing)

De volgende voorbeelden tonen desktop of web-toepassingen die toegang hebben tot de Microsoft Graph of een web-API zonder gebruiker (met de toepassings-id).

Clienttoepassing | Platform | Stroom/verlenen | Een ASP.NET- of ASP.NET Core 2.0-Web-API wordt aangeroepen
------------------ | -------- | ---------- | -------------------- 
Daemon-app (Console)          | ![Deze afbeelding ziet u de .NET-logo](media/sample-v2-code/logo_NETframework.png) | De referenties van de client met app-geheim of certificaat | [dotnet-daemon](https://github.com/azure-samples/active-directory-dotnet-daemon)</p> [dotnet-daemon-certificate-credential](https://github.com/azure-samples/active-directory-dotnet-daemon-certificate-credential)
Daemon-app (Console)         | ![Deze afbeelding ziet u de .NET-logo](media/sample-v2-code/logo_NETcore.png) | De referenties van de client met certificaat| [dotnetcore-daemon-certificate-credential](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-certificate-credential)
ASP.NET Web App  | ![Deze afbeelding ziet u de .NET-logo](media/sample-v2-code/logo_NETframework.png) | Clientreferenties | [dotnet-webapp-webapi-oauth2-appidentity](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-appidentity)

## <a name="web-apis"></a>Web-API's

### <a name="web-api-protected-by-azure-active-directory"></a>Web-API die wordt beveiligd door Azure Active Directory

Het volgende voorbeeld toont het beveiligen van een node.js-web-API met Azure AD.

In de vorige secties van dit artikel vindt u ook andere voorbeelden ter illustratie van een clienttoepassing **aanroepen** een ASP.NET- of ASP.NET Core **Web-API**. Deze voorbeelden niet opnieuw worden vermeld in deze sectie, maar vindt u deze in de laatste kolom van de tabellen boven of onder

| Platform | Voorbeeld |
|--------|-------------------|
| ![Deze afbeelding ziet u de Node.js-logo](media/sample-v2-code/logo_nodejs.png)  | [node-webapi](https://github.com/Azure-Samples/active-directory-node-webapi) |

### <a name="web-api-calling-microsoft-graph-or-another-web-api"></a>Web-API aanroepen van Microsoft Graph of een andere Web-API

De volgende voorbeelden ziet een web-API die een andere web-API aanroept. Het tweede voorbeeld laat zien hoe voorwaardelijke toegang worden verwerkt.

| Platform |  Aanroepen van Microsoft Graph | Een andere ASP.NET of ASP.NET Core 2.0 Web-API-aanroepen |
| -------- |  --------------------- | ------------------------- |
| ![Deze afbeelding ziet u de ASP.NET-logo](media/sample-v2-code/logo_NETframework.png)</p> ASP.NET 4.5 | [dotnet-webapi-onbehalfof](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof) </p> [dotnet-webapi-onbehalfof-ca](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof-ca) | [dotnet-webapi-onbehalfof](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof) </p> [dotnet-webapi-onbehalfof-ca](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof-ca) |

## <a name="other-microsoft-graph-samples"></a>Andere voorbeelden van Microsoft Graph

Zie voor voorbeelden en zelfstudies die laten zien van verschillende gebruikspatronen voor de Microsoft Graph API, met inbegrip van verificatie met Azure AD [Microsoft Graph-Communityvoorbeelden en zelfstudies](https://github.com/microsoftgraph/msgraph-community-samples).

## <a name="see-also"></a>Zie ook

[Ontwikkelaarshandleiding voor Azure Active Directory](v1-overview.md)

[Azure Active Directory-verificatie-bibliotheken](active-directory-authentication-libraries.md)

[Azure AD Graph API conceptuele en naslaginformatie](https://msdn.microsoft.com/library/azure/hh974476.aspx)

[Azure AD Graph API-Helper-bibliotheek](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)
