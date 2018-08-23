---
title: Azure Active Directory-Verificatiebibliotheken | Microsoft Docs
description: De Azure AD Authentication Library (ADAL) kan client toepassingsontwikkelaars gemakkelijk om gebruikers te verifiëren naar de cloud of on-premises Active Directory (AD) en vervolgens toegangstokens voor het beveiligen van API-aanroepen te verkrijgen.
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
ms.openlocfilehash: 6cf01bfe0b88150f7d2b200031c96ffb703ade0a
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42054498"
---
# <a name="azure-active-directory-authentication-libraries"></a>Azure Active Directory-Verificatiebibliotheken

De Azure Active Directory Authentication Library (ADAL) v1.0 kunnen toepassingsontwikkelaars voor verificatie van gebruikers naar de cloud of on-premises Active Directory (AD), en verkrijgen van tokens voor het beveiligen van API-aanroepen. ADAL vergemakkelijkt verificatie voor ontwikkelaars via functies, zoals:

- Configureerbare tokencache dat winkels toegang krijgen tokens tot en tokens vernieuwen
- Automatische token vernieuwen wanneer een toegangstoken is verlopen en een vernieuwingstoken is beschikbaar
- Ondersteuning voor asynchrone methodeaanroepen

> [!NOTE]
> Zoek naar de Azure AD v2.0-bibliotheken (MSAL)? Bekijk de [MSAL bibliotheek handleiding](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries).
>
>

## <a name="microsoft-supported-client-libraries"></a>Ondersteunde Microsoft-clientbibliotheken

| Platform | Bibliotheek | Downloaden | Broncode | Voorbeeld | Referentie
| --- | --- | --- | --- | --- | --- |
| .NET-client, Windows Store, UWP of Xamarin iOS en Android |ADAL .NET v3 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet) | [Desktop-app](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-dotnet) |[Naslaginformatie](https://docs.microsoft.com/dotnet/api/?view=identitymodelclientsad-3.13.9) |
| .NET-client, Windows Store, Windows Phone 8.1 |ADAL .NET v2 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/2.28.4) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/releases/tag/v2.28.4) | [Desktop-app](https://github.com/AzureADQuickStarts/NativeClient-DotNet/releases/tag/v2.X) | |
| Javascript |ADAL.js |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-js) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-js) |[App met één pagina](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) | |
| iOS-, Mac OS |ADAL |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-objc/releases) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-objc) |[iOS-app](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-ios) | [Naslaginformatie](http://cocoadocs.org/docsets/ADAL/2.5.1/)|
| Android |ADAL |[De centrale opslagplaats](http://search.maven.org/remotecontent?filepath=com/microsoft/aad/adal/) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-android) |[Android-app](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-android) | [JavaDocs](http://javadoc.io/doc/com.microsoft.aad/adal/)|
| Node.js |ADAL |[npm](https://www.npmjs.com/package/adal-node) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs) | [Node.js-web-app](https://github.com/Azure-Samples/active-directory-node-webapp-openidconnect)|[Naslaginformatie](https://docs.microsoft.com/javascript/api/adal-node/?view=azure-node-latest) |
| Java |ADAL4J |[Maven](http://search.maven.org/#search%7Cga%7C1%7Ca%3Aadal4j%20g%3Acom.microsoft.azure) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-java) |[Java-web-app](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect) |[Naslaginformatie](http://javadoc.io/doc/com.microsoft.azure/adal4j) |
| Python |ADAL |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-python) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-python) |[Python-web-app](https://github.com/Azure-Samples/active-directory-python-webapp-graphapi) |[Naslaginformatie](http://adal-python.readthedocs.io/) |

## <a name="microsoft-supported-server-libraries"></a>Ondersteunde Microsoft Server-bibliotheken

| Platform | Bibliotheek | Downloaden | Broncode | Voorbeeld | Referentie
| --- | --- | --- | --- | --- | --- |
| .NET |OWIN voor AzureAD|[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.ActiveDirectory/) |[CodePlex](http://katanaproject.codeplex.com) |[MVC-App](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-webapp-dotnet) | |
| .NET |OWIN voor OpenIDConnect |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect) |[CodePlex](http://katanaproject.codeplex.com) |[Web-app](https://github.com/AzureADSamples/WebApp-OpenIDConnect-DotNet) | |
| .NET |OWIN voor de WS-Federation |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) |[CodePlex](http://katanaproject.codeplex.com) |[MVC-Web-App](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet) | |
| .NET |Identiteit Protocol Extensions for .NET 4.5 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Protocol.Extensions) |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |
| .NET |JWT-Handler voor .NET 4.5 |[NuGet](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt) |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |
| Node.js |Azure AD-Passport |[npm](https://www.npmjs.com/package/passport-azure-ad) |[GitHub](https://github.com/AzureAD/passport-azure-ad) | [Web-API](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-webapi-nodejs)| |

## <a name="scenarios"></a>Scenario's

Hier volgen drie gangbare scenario's voor het gebruik van ADAL in een client die toegang heeft tot een externe resource:

### <a name="authenticating-users-of-a-native-client-application-running-on-a-device"></a>Verifiëren van gebruikers van een systeemeigen client-toepassing die wordt uitgevoerd op een apparaat

In dit scenario kan heeft een ontwikkelaar een mobiele client of een bureaubladtoepassing die toegang nodig heeft tot een externe bron, zoals een web-API. De web-API is niet toegestaan voor anonieme aanroepen en moet worden aangeroepen in de context van een geverifieerde gebruiker. De web-API is vooraf geconfigureerd voor het vertrouwen toegangstokens die zijn uitgegeven door een specifieke Azure AD-tenant. Azure AD is vooraf geconfigureerd voor het uitgeven van tokens voor toegang voor die bron. Voor het aanroepen van de web-API van de client, de ontwikkelaar gebruikmaakt van ADAL om verificatie met Azure AD mogelijk te maken. De veiligste manier het gebruik van ADAL is dat deze de gebruikersinterface voor het verzamelen van gebruikersreferenties (weergegeven als browservenster).

ADAL kunt u eenvoudig om te verifiëren van de gebruiker, een toegangstoken en een vernieuwingstoken verkrijgen van Azure AD en roep vervolgens de web-API met behulp van de access token.

Zie voor een voorbeeld waarin wordt gedemonstreerd in dit scenario met behulp van verificatie met Azure AD [systeemeigen Client WPF-toepassing voor de Web-API](https://github.com/azureadsamples/nativeclient-dotnet).

### <a name="authenticating-a-confidential-client-application-running-on-a-web-server"></a>Verificatie van een vertrouwelijke client-toepassing die wordt uitgevoerd op een webserver

In dit scenario kan heeft een ontwikkelaar een toepassing die wordt uitgevoerd op een server die toegang nodig heeft tot een externe bron, zoals een web-API. De web die API is niet toegestaan als anonieme aanroept, zodat deze moet worden aangeroepen vanuit een gemachtigde service. De web-API is vooraf geconfigureerd voor het vertrouwen toegangstokens die zijn uitgegeven door een specifieke Azure AD-tenant. Azure AD is vooraf geconfigureerd voor het verzenden van toegangstokens voor die bron naar een service met clientreferenties (client-ID en geheim). ADAL vereenvoudigt het uitvoeren van verificatie van de service met Azure AD retourneren van een toegangstoken dat kan worden gebruikt om de web-API aanroepen. ADAL verwerkt ook de levensduur van het toegangstoken beheren door ze en vernieuwen van deze indien nodig. Zie voor een codevoorbeeld die u in dit scenario laat zien, [Daemon console toepassing tot Web-API](https://github.com/AzureADSamples/Daemon-DotNet).

### <a name="authenticating-a-confidential-client-application-running-on-a-server-on-behalf-of-a-user"></a>Verificatie van een vertrouwelijke client-toepassing die wordt uitgevoerd op een server namens een gebruiker

In dit scenario heeft een ontwikkelaar een webtoepassing die wordt uitgevoerd op een server die toegang nodig heeft tot een externe bron, zoals een web-API. De web-API is niet toegestaan voor anonieme oproepen, dus deze moet worden aangeroepen vanuit een gemachtigde service namens een geverifieerde gebruiker. De web-API is vooraf geconfigureerd dat is uitgegeven door een specifieke Azure Active Directory-toegangstokens vertrouwen tenant en Azure AD is vooraf geconfigureerd voor het verzenden van toegangstokens voor die bron naar een service met clientreferenties. Zodra de gebruiker is geverifieerd in de web-App, kan de toepassing een autorisatiecode verkrijgen voor de gebruiker van Azure AD. De web-App kunt vervolgens ADAL gebruiken om te verkrijgen van een toegangstoken en vernieuwen van het token namens een gebruiker met de autorisatie-code en client-referenties die zijn gekoppeld aan de toepassing van Azure AD. Zodra de web-App in het bezit is van het toegangstoken is, kan de web-API aanroepen totdat het token is verlopen. Wanneer het token is verlopen, kan de web-App gebruikmaken van ADAL om een nieuw toegangstoken ophalen met behulp van het vernieuwen van de token die eerder is ontvangen. Zie voor een codevoorbeeld die u in dit scenario laat zien, [Native client naar de Web-API voor Web-API](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof).

## <a name="see-also"></a>Zie ook

- [De Azure Active Directory developer's guide](azure-ad-developers-guide.md)
- [Verificatiescenario's voor Azure Active directory](authentication-scenarios.md)
- [Azure Active Directory-codevoorbeelden](sample-v1-code.md)
