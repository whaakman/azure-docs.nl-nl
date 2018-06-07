---
title: Azure Active Directory-Verificatiebibliotheken | Microsoft Docs
description: De Azure AD Authentication Library (ADAL) kan client toepassingsontwikkelaars eenvoudig om gebruikers te verifiëren naar de cloud of on-premises Active Directory (AD) en vervolgens verkrijgen toegangstokens voor het beveiligen van API-aanroepen.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 2e4fc79a-0285-40be-8c77-65edee408a22
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/13/2018
ms.author: celested
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 17bf8a973c42b2940c8d09fa56627460743ed45f
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34594258"
---
# <a name="azure-active-directory-authentication-libraries"></a>Azure Active Directory-Verificatiebibliotheken

De Azure Active Directory Authentication Library (ADAL) v1.0 kunnen toepassingsontwikkelaars voor verificatie van gebruikers in de cloud of on-premises Active Directory (AD), en voor het beveiligen van API-aanroepen-tokens verkrijgen. ADAL gemakkelijker verificatie voor ontwikkelaars via functies, zoals:

- Configureerbare tokencache dat winkels tokens en vernieuw tokens
- automatisch token vernieuwen wanneer een toegangstoken is verlopen en een vernieuwingstoken is beschikbaar
- ondersteuning voor asynchrone methodeaanroepen

> [!NOTE]
> Zoekt u de Azure AD v2.0-bibliotheken (MSAL)? Bekijk de [MSAL bibliotheek handleiding](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries).
>
>

## <a name="microsoft-supported-client-libraries"></a>Microsoft ondersteunde clientbibliotheken

| Platform | Bibliotheek | Downloaden | Broncode | Voorbeeld | Referentie
| --- | --- | --- | --- | --- | --- |
| .NET-client, Windows Store UWP, Xamarin iOS en Android |ADAL .NET v3 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet) | [Bureaublad-app](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-dotnet) |[Naslaginformatie](https://docs.microsoft.com/dotnet/api/?view=identitymodelclientsad-3.13.9) |
| .NET-client, Windows Store, Windows Phone 8.1 |ADAL .NET v2 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/2.28.4) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/releases/tag/v2.28.4) | [Bureaublad-app](https://github.com/AzureADQuickStarts/NativeClient-DotNet/releases/tag/v2.X) | |
| Javascript |ADAL.js |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-js) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-js) |[App met één pagina](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) | |
| voor iOS, Mac OS |ADAL |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-objc/releases) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-objc) |[iOS-app](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-ios) | [Naslaginformatie](https://cocoadocs.org/docsets/ADAL/)|
| Android |ADAL |[De centrale opslagplaats](http://search.maven.org/remotecontent?filepath=com/microsoft/aad/adal/) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-android) |[Android-app](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-android) | [JavaDocs](http://javadoc.io/doc/com.microsoft.aad/adal/)|
| Node.js |ADAL |[npm](https://www.npmjs.com/package/adal-node) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs) | [Node.js-web-app](https://github.com/Azure-Samples/active-directory-node-webapp-openidconnect)| |
| Java |ADAL4J |[Maven](http://search.maven.org/#search%7Cga%7C1%7Ca%3Aadal4j%20g%3Acom.microsoft.azure) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-java) |[Java-web-app](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect) | |
| Python |ADAL |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-python) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-python) |[Python-web-app](https://github.com/Azure-Samples/active-directory-python-webapp-graphapi) |[Naslaginformatie](http://adal-python.readthedocs.io/) |

## <a name="microsoft-supported-server-libraries"></a>Ondersteunde Microsoft Server-bibliotheken

| Platform | Bibliotheek | Downloaden | Broncode | Voorbeeld | Referentie
| --- | --- | --- | --- | --- | --- |
| .NET |OWIN voor AzureAD|[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.ActiveDirectory/) |[CodePlex](http://katanaproject.codeplex.com) |[MVC-App](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-webapp-dotnet) | |
| .NET |OWIN voor OpenIDConnect |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect) |[CodePlex](http://katanaproject.codeplex.com) |[Web-app](https://github.com/AzureADSamples/WebApp-OpenIDConnect-DotNet) | |
| .NET |OWIN voor WS-Federation |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) |[CodePlex](http://katanaproject.codeplex.com) |[MVC-Web-App](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet) | |
| .NET |Identiteit Protocol-extensies voor .NET 4.5 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Protocol.Extensions) |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |
| .NET |JWT-Handler voor .NET 4.5 |[NuGet](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt) |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |
| Node.js |Azure AD-Passport |[npm](https://www.npmjs.com/package/passport-azure-ad) |[GitHub](https://github.com/AzureAD/passport-azure-ad) | [Web-API](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-webapi-nodejs)| |

## <a name="scenarios"></a>Scenario's

Hier volgen drie gangbare scenario's voor het gebruik van ADAL in een client die toegang heeft tot een externe resource:

### <a name="authenticating-users-of-a-native-client-application-running-on-a-device"></a>Gebruikers van een systeemeigen clienttoepassing uitgevoerd op een apparaat te verifiëren

In dit scenario heeft een ontwikkelaar een client voor mobiele of bureaubladtoepassingen die nodig zijn voor toegang tot een externe bron, zoals een web-API. De web-API aanroepen van anonieme niet toestaat en moet worden aangeroepen in de context van een geverifieerde gebruiker. De web-API is vooraf geconfigureerd voor het vertrouwen van de toegangstokens die zijn uitgegeven door een specifieke Azure AD-tenant. Azure AD is vooraf geconfigureerd voor het uitgeven van toegangstokens voor die bron. Om aan te roepen de web-API van de client, gebruikt de ontwikkelaar van de ADAL-verificatie met Azure AD mogelijk te maken. De veiligste manier gebruik van ADAL is voor het weergeven van de gebruikersinterface voor het verzamelen van referenties van gebruiker (als browservenster weergegeven).

ADAL kunt u gemakkelijk verifiëren van de gebruiker en roept u vervolgens de web-API met behulp van de toegang verkrijgen van een toegangstoken en een vernieuwingstoken van Azure AD-token.

Zie voor een voorbeeld van code die u laat zien in dit scenario met verificatie met Azure AD [Native Client WPF-toepassing voor Web-API](https://github.com/azureadsamples/nativeclient-dotnet).

### <a name="authenticating-a-confidential-client-application-running-on-a-web-server"></a>Verificatie van een vertrouwelijk clienttoepassing uitvoert op een webserver

In dit scenario heeft een ontwikkelaar een toepassing die wordt uitgevoerd op een server die nodig zijn voor toegang tot een externe bron, zoals een web-API. Het web dat API niet in anonieme staat aanroept, zodat deze moet worden aangeroepen vanuit een gemachtigde service. De web-API is vooraf geconfigureerd voor het vertrouwen van de toegangstokens die zijn uitgegeven door een specifieke Azure AD-tenant. Azure AD is vooraf geconfigureerd om toegangstokens voor die bron naar een service met de referenties van de client (client-ID en geheim). ADAL vergemakkelijkt de verificatie van de service met Azure AD retourneren van een toegangstoken dat kan worden gebruikt voor de web-API aanroepen. ADAL zorgt ook voor de levensduur van het toegangstoken beheren door deze cache en vernieuwen van deze indien nodig. Zie voor een voorbeeld van code die u in dit scenario laat zien [Daemon console toepassing naar de Web-API](https://github.com/AzureADSamples/Daemon-DotNet).

### <a name="authenticating-a-confidential-client-application-running-on-a-server-on-behalf-of-a-user"></a>Een vertrouwelijk clienttoepassing uitgevoerd op een server namens een gebruiker verifiëren

In dit scenario heeft een ontwikkelaar een webtoepassing die wordt uitgevoerd op een server die nodig zijn voor toegang tot een externe bron, zoals een web-API. De web-API toestaat aanroepen niet anonieme, zodat deze moet worden aangeroepen vanuit een gemachtigde service namens een geverifieerde gebruiker. De web-API is vooraf geconfigureerd om te vertrouwen toegangstokens die zijn uitgegeven door een specifieke Azure AD-tenant en Azure AD is vooraf geconfigureerd om toegangstokens voor die bron naar een service met de clientreferenties. Als de gebruiker is geverifieerd in de webtoepassing, kan de toepassing een autorisatiecode verkrijgen voor de gebruiker van Azure AD. De webtoepassing kunt vervolgens ADAL gebruiken om vernieuwen token namens een gebruiker met de autorisatie-code en client-referenties die zijn gekoppeld aan de toepassing van Azure AD te verkrijgen van een toegangstoken. Zodra de webtoepassing in bezit is van het toegangstoken is, kan de web-API aanroepen totdat het token is verlopen. Wanneer het token is verlopen, kan de webtoepassing gebruikmaken van ADAL voor een nieuw toegangstoken token ophalen met behulp van de vernieuwing die eerder is ontvangen. Zie voor een voorbeeld van code die u in dit scenario laat zien [Native client tot de Web-API voor Web-API](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof).

## <a name="see-also"></a>Zie ook

- [De ontwikkelaarshandleiding voor de Azure Active Directory](active-directory-developers-guide.md)
- [Verificatie-scenario's voor Azure Active directory](active-directory-authentication-scenarios.md)
- [Azure Active Directory-codevoorbeelden](active-directory-code-samples.md)
