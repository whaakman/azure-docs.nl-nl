---
title: Azure Active Directory-Verificatiebibliotheken | Microsoft Docs
description: "De Azure AD Authentication Library (ADAL) kan client toepassingsontwikkelaars eenvoudig om gebruikers te verifiëren naar de cloud of on-premises Active Directory (AD) en vervolgens verkrijgen toegangstokens voor het beveiligen van API-aanroepen."
services: active-directory
documentationcenter: 
author: bryanla
manager: mtillman
editor: mbaldwin
ms.assetid: 2e4fc79a-0285-40be-8c77-65edee408a22
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/25/2017
ms.author: bryanla
ms.custom: aaddev
ms.openlocfilehash: f017e3d323b98660fdee902770652b3165e70e5e
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-authentication-libraries"></a>Azure Active Directory-Verificatiebibliotheken
De Azure Active Directory Authentication Library (ADAL) kunnen toepassingsontwikkelaars voor verificatie van gebruikers in de cloud of on-premises Active Directory (AD), en voor het beveiligen van API-aanroepen-tokens verkrijgen. ADAL gemakkelijker verificatie voor ontwikkelaars via functies, zoals:
 - Configureerbare tokencache dat winkels tokens en vernieuw tokens
 - automatisch token vernieuwen wanneer een toegangstoken is verlopen en een vernieuwingstoken is beschikbaar
 - ondersteuning voor asynchrone methodeaanroepen
 - en meer

> [!NOTE]
> Zoekt u de Azure AD v2.0-bibliotheken (MSAL)? Bekijk de [MSAL bibliotheek handleiding](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries). 
> 
> 

### <a name="client-libraries"></a>Clientbibliotheken

| Platform | Bibliotheek | Downloaden | Broncode | Voorbeeld | Naslaginformatie
| --- | --- | --- | --- | --- | --- |
| .NET-client, Windows Store UWP, Xamarin iOS en Android |ADAL .NET v3 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet) | [Bureaublad-app](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-dotnet) |[Naslaginformatie](https://docs.microsoft.com/dotnet/api/?view=identitymodelclientsad-3.13.9) | 
| .NET-client, Windows Store, Windows Phone 8.1 |ADAL .NET v2 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/2.28.4) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/releases/tag/v2.28.4) | [Bureaublad-app](https://github.com/AzureADQuickStarts/NativeClient-DotNet/releases/tag/v2.X) | | 
| Javascript |ADAL.js |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-js) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-js) |[App met één pagina](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) | |
| voor iOS, Mac OS |ADAL |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-objc/releases) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-objc) |[iOS-app](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-ios) | [Naslaginformatie](https://cocoapods.org/pods/ADAL)|
| Android |ADAL |[De centrale opslagplaats](http://search.maven.org/remotecontent?filepath=com/microsoft/aad/adal/) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-android) |[Android-app](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-android) | [JavaDocs](http://javadoc.io/doc/com.microsoft.aad/adal/)|
| Node.js |ADAL |[npm](https://www.npmjs.com/package/adal-node) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs) | | |
| Java |ADAL4J |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-java) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-java) |[Java-web-app](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-webapp-java) | |
| Python |ADAL |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-python) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-python) | | |

### <a name="server-libraries"></a>Server-bibliotheken 

| Platform | Bibliotheek | Downloaden | Broncode | Voorbeeld | Naslaginformatie
| --- | --- | --- | --- | --- | --- |
| .NET |OWIN voor AzureAD|[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.ActiveDirectory/) |[CodePlex](http://katanaproject.codeplex.com) |[MVC-App](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-webapp-dotnet) | |
| .NET |OWIN voor OpenIDConnect |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect) |[CodePlex](http://katanaproject.codeplex.com) |[Web-app](https://github.com/AzureADSamples/WebApp-OpenIDConnect-DotNet) | |
| Node.js |Azure AD-Passport |[npm](https://www.npmjs.com/package/passport-azure-ad) |[GitHub](https://github.com/AzureAD/passport-azure-ad) | [Web-API](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-webapi-nodejs)| |
| .NET |OWIN voor WS-Federation |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) |[CodePlex](http://katanaproject.codeplex.com) |[MVC-Web-App](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet) | |
| .NET |Identiteit Protocol-extensies voor .NET 4.5 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Protocol.Extensions) |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |
| .NET |JWT-Handler voor .NET 4.5 |[NuGet](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt) |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |

### <a name="v20-client-libraries-msal"></a>v2.0-clientbibliotheken (MSAL)

De [Azure AD v2.0-eindpunt](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-compare) combineert Azure AD en Microsoft-Accounts achter één eindpunt. Als u dit eindpunt, ontwikkelaars kunnen gebruikmaken van de [productie ondersteund preview MSAL bibliotheken](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries) in plaats van ADAL.

| Platform | Bibliotheek | Downloaden | Broncode | Voorbeeld | Naslaginformatie
| --- | --- | --- | --- | --- | --- |
| .NET-client, Windows Store UWP, Xamarin iOS en Android |MSAL voor .NET (preview) |[NuGet](https://www.nuget.org/packages/Microsoft.Identity.Client/1.1.0-preview) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) | [Bureaublad-app](~/articles/active-directory/develop/guidedsetups/active-directory-windesktop.md) |[Naslaginformatie](https://docs.microsoft.com/dotnet/api/?view=identityclient-1.1.0-preview) | 
| Javascript |MSAL voor JavaScript (preview) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-js) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-js) | [App met één pagina](~/articles/active-directory/develop/GuidedSetups/active-directory-javascriptspa.md) | [Naslaginformatie](https://htmlpreview.github.io/?https://raw.githubusercontent.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/docs/classes/_useragentapplication_.msal.useragentapplication.html) | 
| iOS |MSAL voor iOS (preview) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-objc) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-objc) | [iOS-app](~/articles/active-directory/develop/GuidedSetups/active-directory-ios.md) | [Naslaginformatie](https://azuread.github.io/docs/objc/) |
| Android |MSAL voor Android (preview) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-android) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-android) | [Android-app](~/articles/active-directory/develop/GuidedSetups/active-directory-android.md) | [Naslaginformatie](http://javadoc.io/doc/com.microsoft.identity.client/msal/0.1.1) |

## <a name="scenarios"></a>Scenario's

Hier volgen drie gangbare scenario's waarin ADAL voor het verifiëren van een client die toegang heeft tot een externe bron kan worden gebruikt:  

### <a name="authenticating-users-of-a-native-client-application-running-on-a-device"></a>Gebruikers van een systeemeigen clienttoepassing uitgevoerd op een apparaat te verifiëren 

In dit scenario heeft een ontwikkelaar een clienttoepassing WPF die nodig zijn voor toegang tot een externe bron die is beveiligd door Azure AD, zoals een web-API. Hij heeft een Azure-abonnement, weet hoe de downstream web-API aanroepen en kent van de Azure AD-tenant die gebruikmaakt van de web-API. Hij kan als gevolg hiervan ADAL gebruiken ter bevordering van de verificatie met Azure AD, door de verificatie-ervaring adal volledig delegeren of door expliciet afhandeling van gebruikersreferenties. ADAL maakt het gemakkelijk te verifiëren van de gebruiker, een toegangstoken en een vernieuwingstoken verkrijgen van Azure AD en gebruik vervolgens het toegangstoken om ervoor te aanvragen naar de web-API.

Zie voor een voorbeeld van code die u laat zien in dit scenario met verificatie met Azure AD [Native Client WPF-toepassing voor Web-API](https://github.com/azureadsamples/nativeclient-dotnet).

### <a name="authenticating-a-confidential-client-application-running-on-a-web-server"></a>Verificatie van een vertrouwelijk clienttoepassing uitvoert op een webserver

In dit scenario heeft een ontwikkelaar een toepassing die wordt uitgevoerd op een server die nodig zijn voor toegang tot een externe bron die is beveiligd door Azure AD, zoals een web-API. Hij een Azure-abonnement heeft, kunnen worden aanroepen van de downstream-service en kent dat de Azure AD-tenant de web-API wordt gebruikt. Hij kan als gevolg hiervan ADAL gebruiken om te vereenvoudigen, verificatie met Azure AD expliciet afhandeling van de referenties van de toepassing. ADAL zorgt ervoor dat er eenvoudig een token met behulp van de toepassing de clientreferenties ophalen uit Azure AD en dit token vervolgens gebruiken om ervoor te aanvragen naar de web-API. ADAL zorgt ook voor de levensduur van het toegangstoken beheren door deze cache en vernieuwen van deze indien nodig. Zie voor een voorbeeld van code die u in dit scenario laat zien [Daemon console toepassing naar de Web-API](https://github.com/AzureADSamples/Daemon-DotNet).

### <a name="authenticating-a-confidential-client-application-running-on-a-server-on-behalf-of-a-user"></a>Een vertrouwelijk clienttoepassing uitgevoerd op een server namens een gebruiker verifiëren 

In dit scenario heeft een ontwikkelaar een toepassing die wordt uitgevoerd op een server die nodig zijn voor toegang tot een externe bron die is beveiligd door Azure AD, zoals een web-API. De aanvraag moet ook worden gedaan namens een Azure AD-gebruiker. Hij heeft een Azure-abonnement, weet hoe de downstream web-API aanroepen en kent van de Azure AD tenant van de service wordt gebruikt. Als de gebruiker is geverifieerd aan de webtoepassing, kan de toepassing een autorisatiecode verkrijgen voor de gebruiker van Azure AD. De webtoepassing kunt vervolgens ADAL gebruiken om vernieuwen token namens een gebruiker met de autorisatie-code en client-referenties die zijn gekoppeld aan de toepassing van Azure AD te verkrijgen van een toegangstoken. Zodra de webtoepassing in bezit is van het toegangstoken is, kan de web-API aanroepen totdat het token is verlopen. Wanneer het token is verlopen, kan de webtoepassing gebruikmaken van ADAL voor een nieuw toegangstoken token ophalen met behulp van de vernieuwing die eerder is ontvangen. Zie voor een voorbeeld van code die u in dit scenario laat zien [Native client tot de Web-API voor Web-API](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof).

## <a name="see-also"></a>Zie ook

- [De ontwikkelaarshandleiding voor de Azure Active Directory](active-directory-developers-guide.md)
- [Verificatie-scenario's voor Azure Active directory](active-directory-authentication-scenarios.md)
- [Azure Active Directory-codevoorbeelden](active-directory-code-samples.md)
