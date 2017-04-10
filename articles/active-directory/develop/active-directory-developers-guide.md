---
title: Ontwikkelaarshandleiding voor Azure Active Directory | Microsoft Docs
description: Dit artikel bevat een uitgebreide handleiding voor bronnen voor ontwikkelaars voor Azure Active Directory.
services: active-directory
documentationcenter: dev-center-name
author: bryanla
manager: mbaldwin
editor: 
ms.assetid: 5c872c89-ef04-4f4c-98de-bc0c7460c7c2
ms.service: active-directory
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/09/2016
ms.author: mbaldwin
translationtype: Human Translation
ms.sourcegitcommit: 5cce99eff6ed75636399153a846654f56fb64a68
ms.openlocfilehash: 45ff2ff08cb36f9a4bf49ff04de149accf157e0e
ms.lasthandoff: 03/31/2017


---
# <a name="azure-active-directory-developers-guide"></a>Ontwikkelaarshandleiding voor Azure Active Directory
## <a name="overview"></a>Overzicht
Azure Active Directory (AD) is een IDMaaS-platform (Identity Management-as-a-Service) waarop ontwikkelaars efficiënt identiteitsbeheer kunnen integreren in hun toepassingen. De volgende artikelen bevatten overzichten van de implementatie en belangrijke functies van Azure AD. We raden u aan deze in de juiste volgorde te lezen. Ga naar [Aan de slag](#getting-started) als u direct aan de slag wilt gaan.

1. [De voordelen van Azure AD-integratie](active-directory-how-to-integrate.md): ontdek waarom de integratie met Azure AD de beste oplossing is voor beveiligde aanmelding en autorisatie.
2. [Scenario's voor Azure AD-verificatie](active-directory-authentication-scenarios.md): profiteer van vereenvoudigde verificatie in Azure AD om eenmalige aanmelding voor uw toepassing te bieden.
3. [Toepassingen integreren met Azure AD](active-directory-integrating-applications.md): meer informatie over het toevoegen, bijwerken en verwijderen van toepassingen in Azure AD en de huisstijlrichtlijnen voor geïntegreerde apps.
4. [Microsoft Graph](https://graph.microsoft.io/) en [Azure AD Graph API](active-directory-graph-api.md): geprogrammeerde toegang tot Azure AD via REST API-eindpunten. **Wij raden u ten zeerste aan om Microsoft Graph te gebruiken in plaats van Azure AD Graph API om toegang te krijgen tot Azure Active Directory-resources.** We richten ons momenteel op ontwikkelingen in Microsoft Graph. Voor Azure AD Graph API zijn geen verdere verbeteringen gepland. Er is maar een beperkt aantal scenario's waarvoor Azure AD Graph API nog steeds geschikt is. Zie het blogbericht [Mogelijk Graph of Azure AD Graph](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph) in het Office-ontwikkelaarscentrum voor meer informatie.
5. [Azure AD-verificatiebibliotheken](active-directory-authentication-libraries.md): u kunt eenvoudig gebruikers verifiëren door ervoor te zorgen dat zij toegangstokens verkrijgen via Azure AD-verificatiebibliotheken voor .NET, JavaScript, Objective-C, Android en meer.

## <a name="getting-started"></a>Aan de slag
Deze zelfstudies zijn geschikt voor meerdere platforms en hiermee kunt u snel leren te ontwikkelen met Azure Active Directory. Als vereiste moet u [een Azure Active Directory-tenant ophalen](active-directory-howto-tenant.md).

### <a name="mobile-and-pc-application-quick-start-guides"></a>Snelstartgidsen voor mobiele toepassingen en pc-toepassingen
| [![iOS](./media/active-directory-developers-guide/ios.png)](active-directory-devquickstarts-ios.md) | [![Android](./media/active-directory-developers-guide/android.png)](active-directory-devquickstarts-android.md) | [![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-dotnet.md) | [![Windows Universal](./media/active-directory-developers-guide/windows.png)](active-directory-devquickstarts-windowsstore.md) | 
|:---:|:---:|:---:|:---:|:---:|
| [iOS](active-directory-devquickstarts-ios.md) |[Android](active-directory-devquickstarts-android.md) |[.NET](active-directory-devquickstarts-dotnet.md) |[Windows</br>Universal](active-directory-devquickstarts-windowsstore.md) |

|[![Xamarin](./media/active-directory-developers-guide/xamarin.png)](active-directory-devquickstarts-xamarin.md) | [![Cordova](./media/active-directory-developers-guide/cordova.png)](active-directory-devquickstarts-cordova.md) | [![OAuth 2.0](./media/active-directory-developers-guide/oauth-2.png)](active-directory-protocols-oauth-code.md) |
|:---:|:---:|:---:|
|[Xamarin](active-directory-devquickstarts-xamarin.md) |[Cordova](active-directory-devquickstarts-cordova.md) |[Rechtstreeks integreren</br> met OAuth 2.0](active-directory-protocols-oauth-code.md) |

### <a name="web-application-quick-start-guides"></a>Snelstartgidsen voor webtoepassingen
| [![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-webapp-dotnet.md) | [![Java](./media/active-directory-developers-guide/java.png)](active-directory-devquickstarts-webapp-java.md) | [![AngularJS](./media/active-directory-developers-guide/angularjs.png)](active-directory-devquickstarts-angular.md) |
|:---:|:---:|:---:|
| [.NET](active-directory-devquickstarts-webapp-dotnet.md) |[Java](active-directory-devquickstarts-webapp-java.md) |[AngularJS](active-directory-devquickstarts-angular.md) |[JavaScript](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) |[Node.js](active-directory-devquickstarts-openidconnect-nodejs.md) |[Rechtstreeks integreren</br> met OpenID Connect](active-directory-protocols-openid-connect-code.md) |

| [![Javascript](./media/active-directory-developers-guide/javascript.png)](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) | [![Node.js](./media/active-directory-developers-guide/nodejs.png)](active-directory-devquickstarts-openidconnect-nodejs.md) | [![OpenID Connect](./media/active-directory-developers-guide/openid-connect.png)](active-directory-protocols-openid-connect-code.md) |
|:---:|:---:|:---:|
|[JavaScript](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) |[Node.js](active-directory-devquickstarts-openidconnect-nodejs.md) |[Rechtstreeks integreren</br> met OpenID Connect](active-directory-protocols-openid-connect-code.md) |

### <a name="web-api-quick-start-guides"></a>Snelstartgidsen voor web-API's
| [![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-webapi-dotnet.md) | [![Node.js](./media/active-directory-developers-guide/nodejs.png)](active-directory-devquickstarts-webapi-nodejs.md) |
|:---:|:---:|
| [.NET](active-directory-devquickstarts-webapi-dotnet.md) |[Node.js](active-directory-devquickstarts-webapi-nodejs.md) |

### <a name="microsoft-graph-and-azure-ad-graph-api-quick-start-guides"></a>Snelstartgidsen voor Microsoft Graph en Azure AD Graph API
| [![Microsoft Graph](./media/active-directory-developers-guide/msgraph.png)](https://developer.microsoft.com/graph/quick-start) | [![Azure AD Graph API](./media/active-directory-developers-guide/graph.png)](active-directory-graph-api-quickstart.md) |
|:---:|:---:|
| [Microsoft Graph](https://developer.microsoft.com/graph/quick-start) | [Azure AD Graph API](active-directory-graph-api-quickstart.md) |

## <a name="how-tos"></a>Procedures
In deze artikelen wordt beschreven hoe u bepaalde taken uitvoert met Azure Active Directory:

* [Een Azure AD-tenant verkrijgen](active-directory-howto-tenant.md)
* [Een Azure AD-gebruiker aanmelden met behulp van het patroon voor multitenant-toepassingen](active-directory-devhowto-multi-tenant-overview.md)
* [Een certificaat gebruiken in plaats van een geheim om een toepassings-id te verifiëren](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential/)
* Inschakelen van eenmalige aanmelding in verschillende apps met ADAL op [Android](active-directory-sso-android.md)- en [iOS](active-directory-sso-ios.md)-apparaten
* [Uw toepassing AppSource-gecertificeerd maken voor Azure AD](active-directory-devhowto-appsource-certified.md)
* [Uw toepassing weergeven in de Azure AD-toepassingsgalerie](active-directory-app-gallery-listing.md)
* [Web-apps voor Office 365 verzenden naar het verkoperdashboard](https://msdn.microsoft.com/office/office365/howto/submit-web-apps-seller-dashboard)
* [Een toepassing registreren met Azure Active Directory via Azure Portal](../active-directory-app-registration.md)
* [Inzicht krijgen in het Azure Active Directory-toepassingsmanifest](active-directory-application-manifest.md)
* [Inzicht krijgen in de huisstijlrichtlijnen voor de knoppen voor aanmelding en aankoop van apps in een clienttoepassing](active-directory-branding-guidelines.md)
* [Voorbeeld: apps maken waarmee gebruikers zich aanmelden met persoonlijke accounts en werk- of schoolaccounts](active-directory-appmodel-v2-overview.md)
* [Voorbeeld: apps maken waarmee gebruikers zich registreren en aanmelden](../../active-directory-b2c/active-directory-b2c-overview.md)
* [Voorbeeld: levensduur van tokens in Azure AD configureren](../active-directory-configurable-token-lifetimes.md) met behulp van PowerShell. Zie [Beleidsbewerkingen](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations) en de [Beleidsentiteit](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#policy-entity) voor meer informatie over het configureren via de Azure AD Graph API.

## <a name="reference"></a>Naslaginformatie
Deze artikelen bevatten naslaginformatie over REST API's en API's voor verificatiebibliotheken, protocollen, fouten, codevoorbeelden en eindpunten.  

### <a name="support"></a>Ondersteuning
* [Vragen met tags](http://stackoverflow.com/questions/tagged/azure-active-directory): vind Azure Active Directory-oplossingen over stackoverloop door te zoeken naar de tags [azure-active-directory](http://stackoverflow.com/questions/tagged/azure-active-directory) en [adal](http://stackoverflow.com/questions/tagged/adal).
* Zie de [woordenlijst voor Azure AD-ontwikkelaars](active-directory-dev-glossary.md) voor definities van de meest gebruikte termen die betrekking hebben op het ontwikkelen en integreren van toepassingen.

### <a name="code"></a>Code
* [Open source-bibliotheken van Azure Active Directory](http://github.com/AzureAD): de eenvoudigste manier om een bibliotheekbron te zoeken, is via onze [bibliotheeklijst](active-directory-authentication-libraries.md).
* [Azure Active Directory-voorbeelden](https://github.com/azure-samples?query=active-directory): de eenvoudigste manier om door de lijst met voorbeelden te bladeren, is via de [index met codevoorbeelden](active-directory-code-samples.md).
* [Active Directory Authentication Library (ADAL) voor .NET](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet)-documentatie is beschikbaar voor zowel [de meest recente primaire versie](https://docs.microsoft.com/active-directory/adal/microsoft.identitymodel.clients.activedirectory) als [de vorige primaire versie](https://docs.microsoft.com/active-directory/adal/v2/microsoft.identitymodel.clients.activedirectory).

### <a name="microsoft-graph-and-azure-ad-graph-api"></a>Microsoft Graph en Azure AD Graph API
> [!IMPORTANT]
> Wij raden u ten zeerste aan om [Microsoft Graph](https://graph.microsoft.io/) te gebruiken in plaats van Azure AD Graph API om toegang te krijgen tot Azure Active Directory-resources. We richten ons momenteel op ontwikkelingen in Microsoft Graph. Voor Azure AD Graph API zijn geen verdere verbeteringen gepland. Er is maar een beperkt aantal scenario's waarvoor Azure AD Graph API nog steeds geschikt is. Zie het blogbericht [Mogelijk Graph of Azure AD Graph](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph) in het Office-ontwikkelaarscentrum voor meer informatie.
> 
* [Microsoft Graph](https://graph.microsoft.io/): documentatie, naslaginformatie, voorbeelden en SDK's voor Microsoft Graph. 
* [Naslaginformatie over Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog): naslaginformatie over REST voor Azure Active Directory Graph API. 
* [Azure AD Graph API-machtigingsbereiken](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes): OAuth 2.0-machtigingsbereiken waarmee de toegang van een app tot directorygegevens in een tenant wordt beheerd.

### <a name="authentication-and-authorization-protocols"></a>Verificatie- en autorisatieprotocollen
* [Ondertekening van sleutelrollover in Azure AD](active-directory-signing-key-rollover.md): meer informatie over het proces voor ondertekening van de sleutelrollover in Azure AD en het bijwerken van de sleutel voor de meest voorkomende toepassingsscenario's.
* [OAuth 2.0-protocol: werken met de autorisatiecodetoekenning](active-directory-protocols-oauth-code.md): u kunt de autorisatiecodetoekenning van het OAuth 2.0-protocol gebruiken om toegang te verlenen aan webtoepassingen en web-API’s in uw Azure Active Directory-tenant.
* [OAuth 2.0-protocol: uitleg over de impliciete toekenning](active-directory-dev-understanding-oauth2-implicit-grant.md): meer informatie over de impliciete autorisatietoekenning en of deze geschikt is voor uw toepassing.
* [OAuth 2.0-protocol: service-naar-serviceaanroepen met behulp van clientreferenties](active-directory-protocols-oauth-service-to-service.md): de toekenning van clientreferenties in OAuth 2.0 stelt een webservice (een vertrouwelijke client) in staat om eigen referenties te gebruiken voor het verifiëren bij het aanroepen van een andere webservice, in plaats van een gebruiker te imiteren. In dit scenario is de client doorgaans een webservice in de middelste laag , een daemon-service of website.
* [OpenID Connect 1.0-protocol: aanmelding en verificatie](active-directory-protocols-openid-connect-code.md): met het OpenID Connect 1.0-protocol kunt u OAuth 2.0 uitbreiden voor gebruik als verificatieprotocol. Een clienttoepassing kan een id_token ontvangen voor het beheer van het aanmeldingsproces of de stroom van de autorisatiecode uitbreiden om zowel een id_token als autorisatiecode te ontvangen.
* [Naslaginformatie over het SAML 2.0-protocol](active-directory-saml-protocol-reference.md): met het SAML 2.0-protocol beschikken gebruikers in toepassingen over eenmalige aanmelding.
* [WS-Federation 1.2-protocol](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html): Azure Active Directory ondersteunt WS-Federation 1.2 vanaf de specificatie Webservices-federatie versie 1.2. Zie [Federation Metadata](active-directory-federation-metadata.md) (Federatieve metagegevens) voor meer informatie over het document met federatieve metagegevens.
* [Ondersteund token en ondersteunde claimtypen](active-directory-token-and-claims.md): in deze handleiding worden de claims in de tokens SAML 2.0 en JSON Web Tokens (JWT) beschreven en geëvalueerd.

## <a name="videos"></a>Video's
### <a name="build"></a>Ontwikkelen
In deze overzichtspresentaties over het ontwikkelen van apps met behulp van Azure Active Directory ziet u sprekers die rechtstreeks deel uitmaken van het technische team. De presentaties gaan over belangrijke onderwerpen, waaronder IDMaaS, verificatie, identiteitsfederatie en eenmalige aanmelding.

* [Microsoft Identity: plannen voor de toekomst](https://azure.microsoft.com/documentation/videos/build-2016-microsoft-identity-state-of-the-union-and-future-direction/)
* [Azure Active Directory: Identity Management als een service voor moderne toepassingen](https://azure.microsoft.com/documentation/videos/build-2015-azure-active-directory-identity-management-as-a-service-for-modern-applications/)
* [Moderne webtoepassingen ontwikkelen met Azure Active Directory](https://azure.microsoft.com/documentation/videos/build-2015-develop-modern-web-applications-with-azure-active-directory/)
* [Moderne systeemeigen toepassingen ontwikkelen met Azure Active Directory](https://azure.microsoft.com/documentation/videos/build-2015-develop-modern-native-applications-with-azure-active-directory/)

### <a name="azure-friday"></a>Azure Friday
[Azure Friday](https://azure.microsoft.com/documentation/videos/azure-friday/) is een videoserie die elke vrijdag wordt uitgezonden en waarin experts kort worden geïnterviewd (10-15 minuten) over diverse Azure-onderwerpen.  Gebruik de functie Servicefilter op de pagina om alle Azure Active Directory-video's te bekijken.

* [Azure Identity 101](https://azure.microsoft.com/documentation/videos/azure-identity-basics/)
* [Azure Identity 102](https://azure.microsoft.com/documentation/videos/azure-identity-creating-active-directory/)
* [Azure Identity 103](https://azure.microsoft.com/documentation/videos/azure-identity-application-to-authenticate/)

## <a name="social"></a>Sociaal
* [Active Directory-teamblog](http://blogs.technet.com/b/ad/): de nieuwste wereldwijde ontwikkelingen van Azure Active Directory.
* [Azure Active Directory Graph-teamblog](https://blogs.msdn.microsoft.com/aadgraphteam): specifieke Azure Active Directory-informatie voor Graph API.
* [Cloudidentiteit](http://www.cloudidentity.net): ideeën over Identity Management als een service van een belangrijke PM van Azure Active Directory.  
* [Azure Active Directory op Twitter](https://twitter.com/azuread): Azure Active Directory-aankondigingen van 140 tekens of minder.

## <a name="windows-server-on-premises-development"></a>On-premises ontwikkeling van Windows Server
Zie voor richtlijnen over het gebruik van de ontwikkeling van Windows Server en Active Directory Federation Services (ADFS):

* [AD FS-scenario's voor ontwikkelaars](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/ad-fs-scenarios-for-developers): biedt een overzicht van AD FS-onderdelen en hoe het werkt, met informatie over de ondersteunde verificatie/autorisatie-scenario's.
* [AD FS-overzichten](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/ad-fs-development): een lijst met overzichtsartikelen die stapsgewijze instructies bieden over het implementeren van de gerelateerde verificatie-/autorisatiestromen.

