---
title: Azure Active Directory-codevoorbeelden | Microsoft Docs
description: Een index van Azure Active Directory-codevoorbeelden, onderverdeeld op basis van scenario.
services: active-directory
documentationcenter: dev-center-name
author: msmbaldwin
manager: mtillman
editor: 
ms.assetid: a242a5ff-7300-40c2-ba83-fb6035707433
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/19/2017
ms.author: mbaldwin
ms.custom: aaddev
ms.openlocfilehash: ba8356a018605331dc4b9cc9ff401929cd8793bf
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-code-samples"></a>Azure Active Directory-codevoorbeelden
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Verificatie en autorisatie toevoegen aan uw webtoepassingen en web-API's kunt u Microsoft Azure Active Directory (Azure AD). Deze sectie een koppeling naar de voorbeelden die u laten zien hoe deze wordt uitgevoerd en codefragmenten die u in uw toepassingen gebruiken kunt. In de voorbeeldpagina code vindt u gedetailleerde lezen-mij onderwerpen die u bij de vereisten, installatie en configuratie helpen. En de code is om te begrijpen van de kritieke secties toegelicht.

Zie inzicht in het eenvoudige scenario voor elk Voorbeeldtype verificatie scenario's voor Azure AD.

Bijdragen aan onze voorbeelden op GitHub: [Microsoft Azure Active Directory-voorbeelden en documentatie](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="web-browser-to-web-application"></a>Webbrowser webtoepassing
Deze voorbeelden laten zien hoe een webtoepassing die zorgt ervoor de browser van de gebruiker dat aanmelden bij Azure AD te schrijven.

| Taal/Platform | Voorbeeld | Beschrijving |
| --- | --- | --- |
| C# / .NET |[WebApp-OpenIDConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) |Gebruik OpenID Connect (ASP.Net OpenID Connect OWIN middleware) om te verifiëren van gebruikers van een Azure AD-tenant. |
| C# / .NET |[WebApp-MultiTenant-OpenIdConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect) |Een multitenant .NET MVC-webtoepassing die OpenID Connect (middleware ASP.Net OpenID Connect OWIN) gebruikt om gebruikers te verifiëren van meerdere Azure AD-tenants. |
| C# / .NET |[WebApp-WSFederation-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-wsfederation) |WS-Federation (middleware ASP.Net WS-Federation OWIN) gebruiken om gebruikers te verifiëren van een Azure AD-tenant. |

## <a name="single-page-application-spa"></a>Toepassing van één pagina (SPA)
Dit voorbeeld wordt het schrijven van een toepassing één pagina die wordt beveiligd met Azure AD.  

| Taal/Platform | Voorbeeld | Beschrijving |
| --- | --- | --- |
| JavaScript, C# / .NET |[SinglePageApp DotNet](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp) |ADAL voor JavaScript en Azure AD gebruiken voor het beveiligen van een AngularJS op basis van één pagina app geïmplementeerd met een ASP.NET-web-API back-end. |

## <a name="native-application-to-web-api"></a>Systeemeigen toepassing aan Web-API
Deze codevoorbeelden laten zien hoe native clienttoepassingen ontwikkelen die aanroepen van web-API's die zijn beveiligd door Azure AD. Ze gebruiken [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md) en [OAuth 2.0 in Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx).

| Taal/Platform | Voorbeeld | Beschrijving |
| --- | --- | --- |
| Javascript |[NativeClient-MultiTarget-Cordova](https://github.com/Azure-Samples/active-directory-cordova-multitarget) |Gebruik de ADAL Apache Cordova-invoegtoepassing voor het bouwen van een Apache Cordova-app die een web-API aanroept en Azure AD gebruikt voor verificatie. |
| C# / .NET |[NativeClient DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-desktop) |Een .NET WPF-toepassing die een web-API-die aanroepen is beveiligd met Azure AD. |
| C# / .NET |[NativeClient WindowsStore](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) |Een Windows Store-toepassing die een web-API-die aanroepen is beveiligd met Azure AD. |
| C# / .NET |[NativeClient-WebAPI-MultiTenant-WindowsStore](https://github.com/Azure-Samples/active-directory-dotnet-webapi-multitenant-windows-store) |Een Windows Store-toepassing voor het aanroepen van een multitenant-web-API die is beveiligd met Azure AD. |
| C# / .NET |[WebAPI-OnBehalfOf-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof) |Een native client-toepassing die een web-API die een token te handelen namens de oorspronkelijke gebruiker ontvangt, en gebruikt vervolgens het token naar een andere web-API aanroepen aanroept. |
| C# / .NET |[NativeClient WindowsPhone8.1](https://github.com/Azure-Samples/active-directory-dotnet-windowsphone-8.1) |Een Windows Store-toepassing voor Windows Phone 8.1 die een web-API-die aanroepen wordt beveiligd door Azure AD. |
| ObjC |[NativeClient iOS](https://github.com/Azure-Samples/active-directory-ios) |Een iOS-toepassing die een web-API-die aanroepen vereist Azure AD voor verificatie. |
| C# / .NET |[WebAPI-ManuallyValidateJwt-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapi-manual-jwt-validation) |Een systeemeigen clienttoepassing die bevat de logica voor het verwerken van een JWT-token in een web-API, in plaats van OWIN middleware. |
| C# / Xamarin |[NativeClient Xamarin.android](https://github.com/Azure-Samples/active-directory-xamarin-android) |Een Xamarin binding aan de systeemeigen Azure AD Authentication Library (ADAL) voor de Android-bibliotheek. |
| C# / Xamarin |[IOS-Xamarin-NativeClient](https://github.com/Azure-Samples/active-directory-xamarin-ios) |Een Xamarin binding aan de systeemeigen Azure AD Authentication Library (ADAL) voor iOS. |
| C# / Xamarin |[NativeClient-MultiTarget-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-multitarget) |Een Xamarin-project dat gericht is op vijf platforms en een web-API-aanroepen die wordt beveiligd door Azure AD. |
| C# / .NET |[NativeClient Headless DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-headless) |Een systeemeigen toepassing die niet-interactieve verificatie uitvoert en een web-API-aanroepen die wordt beveiligd door Azure AD. |

## <a name="web-application-to-web-api"></a>Webtoepassing aan Web-API
Deze codevoorbeelden tonen hoe gebruiken [OAuth 2.0 in Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx) om webtoepassingen te bouwen die aanroep-web-API's die zijn beveiligd door Azure AD.

| Taal/Platform | Voorbeeld | Beschrijving |
| --- | --- | --- |
| C# / .NET |[WebApp WebAPI-OpenIDConnect DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-openidconnect) |Een web-API met aangemelde gebruikersmachtigingen aanroepen. |
| C# / .NET |[WebApp-WebAPI-OAuth2-AppIdentity-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-appidentity) |Met de machtigingen van de toepassing een web-API-aanroep. |
| C# / .NET |[WebApp-WebAPI-OAuth2-UserIdentity-Dotnet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity) |Toevoegen van de autorisatie met [OAuth 2.0 in Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx) naar een bestaande webtoepassing zodat een web-API kan aanroepen. |
| Javascript |[WebAPI Nodejs](https://github.com/Azure-Samples/active-directory-node-webapi) |Instellen van een REST-API-service die geïntegreerd met Azure AD voor API-beveiliging. Bevat een Node.js-server met een Web-API. |
| C# / .NET |[WebApp-WebAPI-MultiTenant-OpenIdConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-multitenant-openidconnect) |Een multitenant MVC-webtoepassing die gebruikmaakt van OpenID Connect (ASP.Net OpenID Connect OWIN middleware) om gebruikers te verifiëren van een Azure AD-tenant. Maakt gebruik van een autorisatiecode de Graph API aanroepen. |

## <a name="server-or-daemon-application-to-web-api"></a>Server of daemontoepassing aan Web-API
Deze codevoorbeelden laten zien hoe u een daemon of server toepassing bouwt die resources van een web-API met behulp van krijgt [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md) en [OAuth 2.0 in Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx).

| Taal/Platform | Voorbeeld | Beschrijving |
| --- | --- | --- |
| C# / .NET |[Daemon DotNet](https://github.com/Azure-Samples/active-directory-dotnet-daemon) |Een consoletoepassing aanroept een web-API. De client-referentie is een wachtwoord. |
| C# / .NET |[Daemon-CertificateCredential-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential) |Een consoletoepassing die een web-API aanroept. De client-referentie is een certificaat. |

## <a name="calling-azure-ad-graph-api"></a>Azure AD Graph API aanroepen
Deze voorbeeldcode laten zien hoe toepassingen ontwikkelen die roept u de Azure AD Graph API om te lezen en schrijven van Active directory-gegevens.

| Taal/Platform | Voorbeeld | Beschrijving |
| --- | --- | --- |
| Java |[GraphAPI-Java-Web-App](https://github.com/Azure-Samples/active-directory-java-graphapi-web) |Een webtoepassing die gebruikmaakt van de Graph API voor toegang tot gegevens van Azure AD-directory. |
| PHP |[WebApp-GraphAPI-PHP](https://github.com/Azure-Samples/active-directory-php-graphapi-web) |Een webtoepassing die gebruikmaakt van de Graph API voor toegang tot gegevens van Azure AD-directory. |
| C# / .NET |[WebApp-GraphAPI-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-web) |Een webtoepassing die gebruikmaakt van de Graph API voor toegang tot gegevens van Azure AD-directory. |
| C# / .NET |[ConsoleApp-GraphAPI-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-console) |Deze console-app toont de algemene lezen en schrijven aanroepen naar de Graph API en laat zien hoe de licentietoewijzing gebruiker uitvoeren en het bijwerken van een gebruiker een foto van miniatuurformaat en koppelingen. |
| C# / .NET |[ConsoleApp GraphAPI-DiffQuery DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-diffquery) |Een consoletoepassing die gebruikmaakt van de differentiële query in de Graph API periodieke wijzigingen in gebruikersobjecten in een Azure AD-tenant ophalen. |
| C# / .NET |[WebApp GraphAPI-DirectoryExtensions DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-directoryextensions-web) |Een MVC-toepassing gebruikt Graph API's voor het genereren van een organigram eenvoudige bedrijf. |
| PHP |[WebApp GraphAPI-DirectoryExtensions PHP](https://github.com/Azure-Samples/active-directory-php-graphapi-directoryextensions-web) |Een PHP-toepassing die de Graph-API voor het registreren van een uitbreiding en vervolgens lezen, bijwerken en verwijderen van de waarden in het kenmerk toestelnummer aanroept. |

## <a name="authorization"></a>Autorisatie
Deze codevoorbeelden laten zien hoe Azure AD gebruiken voor autorisatie.

| Taal/Platform | Voorbeeld | Beschrijving |
| --- | --- | --- |
| C# / .NET |[WebApp-GroupClaims-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-groupclaims) |Uitvoeren op rollen gebaseerd-toegangsbeheer (RBAC) met behulp van Azure Active Directory-groepclaims in een toepassing die is geïntegreerd met Azure AD. |
| C# / .NET |[WebApp-RoleClaims-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) |Uitvoeren op rollen gebaseerd toegangsbeheer (RBAC) op met behulp van de rollen van de Azure Active Directory-toepassing in een toepassing die is geïntegreerd met Azure AD. |

## <a name="legacy-walkthroughs"></a>Verouderde scenario 's
Deze scenario's enigszins oudere technologie gebruiken, maar nog steeds mogelijk van belang.

| Taal/Platform | Voorbeeld | Beschrijving |
| --- | --- | --- |
| C# / .NET |[Op basis van rollen en op basis van ACL-verificatie in een Microsoft Azure AD-toepassing](http://go.microsoft.com/fwlink/?LinkId=331694) |Op basis van rollen autorisatie (RBAC) en op basis van ACL uitvoeren in een toepassing die is geïntegreerd met Azure AD. |
| C# / .NET |[AAL - app Windows Store met REST-service - verificatie](http://go.microsoft.com/fwlink/?LinkId=330605) |Gebruik [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md) (voorheen AAL) voor Windows Store Beta verificatiefuncties gebruiker toevoegen aan een Windows Store-app. |
| C# / .NET |[ADAL - systeemeigen App REST-service - verificatie met AAD via het bladerdialoogvenster](http://go.microsoft.com/fwlink/?LinkId=259814) |Gebruik [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md) verificatiefuncties gebruiker toevoegen aan een WPF-client. |
| C# / .NET |[ADAL - systeemeigen App REST-service - verificatie met ACS via het bladerdialoogvenster](http://code.msdn.microsoft.com/AAL-Native-App-to-REST-de57f2cc) |Gebruik [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md) en [Access Control Service 2.0 (ACS)](http://msdn.microsoft.com/library/azure/hh147631.aspx) verificatiefuncties gebruiker toevoegen aan een WPF-client. |
| C# / .NET |[ADAL - verificatie van de Server naar Server](http://go.microsoft.com/fwlink/?LinkId=259816) |Gebruik [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md) voor het beveiligen van de serviceaanroepen vanuit een kant serverproces naar een MVC4 Web API REST-service. |
| C# / .NET |[Toevoegen van aanmelding aan uw webtoepassing met Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) |Configureer een .NET-toepassing uit te voeren web eenmalige aanmelding op basis van uw Azure AD-directory enterprise. |
| C# / .NET |[Ontwikkeling van Multitenant-webtoepassingen met Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect) |Gebruik Azure AD toevoegen aan de eenmalige aanmelding en een directory toegangsmogelijkheden van een .NET-toepassing in meerdere organisaties werken. |
| JAVA |[Java-voorbeeld-App voor Azure AD Graph API](http://go.microsoft.com/fwlink/?LinkId=263969) |De Graph API gebruiken voor toegang tot directory-gegevens van Azure AD. |
| PHP |[PHP-voorbeeld-App voor Azure AD Graph API](http://code.msdn.microsoft.com/PHP-Sample-App-For-Windows-228c6ddb) |De Graph API gebruiken voor toegang tot directory-gegevens van Azure AD. |
| C# / .NET |[Voorbeeld-App voor Azure AD Graph API](http://go.microsoft.com/fwlink/?LinkID=262648) |De Graph API gebruiken voor toegang tot directory-gegevens van Azure AD. |
| C# / .NET |[Voorbeeld-App voor Azure AD Graph differentiële Query](http://go.microsoft.com/fwlink/?LinkId=275398) |Gebruik de differentiële query in de Graph API periodieke wijzigingen in gebruikersobjecten in een Azure AD-tenant ophalen. |
| C# / .NET |[Voorbeeld-App voor het integreren van Multitenant Cloudtoepassing voor Azure AD](http://go.microsoft.com/fwlink/?LinkId=275397) |Een toepassing met meerdere tenants integreren met Azure AD. |
| C# / .NET |[Beveiligen van een Windows Store-toepassing en de REST-webservice met behulp van Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) |Een eenvoudige web-API-bron- en een Windows Store-clienttoepassing die gebruikmaken van Azure AD maken en de [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md). |
| C# / .NET |[Met behulp van de grafiek API query uitvoeren op Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-web) |Configureer een Microsoft .NET-toepassing voor het gebruik van de Azure AD Graph API toegang tot gegevens van een Azure AD-directory-tenant. |

## <a name="see-also"></a>Zie ook
##### <a name="other-resources"></a>Meer informatie
[Ontwikkelaarshandleiding Azure Active Directory](active-directory-developers-guide.md)

[Azure AD Graph API conceptuele en verwijzingen](https://msdn.microsoft.com/library/azure/hh974476.aspx)

[Azure AD Graph API Hulpbibliotheek](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)
