---
title: Kies de juiste federation-protocol voor uw toepassing met meerdere tenants
description: Richtlijnen voor onafhankelijke softwareleveranciers over de integratie met Azure Active Directory
services: active-directory
author: barbaraselden
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: conceptual
ms.workload: identity
ms.date: 05/22/2019
ms.author: baselden
ms.reviewer: jeeds
ms.collection: M365-identity-device-management
ms.openlocfilehash: 53b315b87200b37cda215a29a65be9babaf54f43
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67795198"
---
# <a name="choose-the-right-federation-protocol-for-your-multi-tenant-application"></a>Kies de juiste federation-protocol voor uw toepassing met meerdere tenants

Wanneer u uw software als een dienst (SaaS)-toepassing ontwikkelt, moet u de federation-protocol die het beste aan de behoeften van uw en uw klanten. Dit besluit is gebaseerd op het ontwikkelplatform en je dat wilt integreren met gegevens die beschikbaar zijn in Office 365 en Azure AD-ecosysteem van uw klanten.

Bekijk de volledige lijst van [protocollen die beschikbaar zijn voor SSO-integraties](what-is-single-sign-on.md) met Azure Active Directory.
De volgende tabel vergelijkt 
* Open verificatie (OAuth 2.0) 2.0
* Open ID Connect (OIDC)
* Security Assertion Markup Language (SAML)
* Web Services Federation (WSFed)

| Mogelijkheid| OAuth / OIDC| SAML / WSFed |
| - |-|-|
| Web gebaseerde eenmalige aanmelding| √| √ |
| Web gebaseerde eenmalige afmelding| √| √ |
| Op basis van een mobiele eenmalige aanmelding| √| √* |
| Op basis van een mobiele eenmalige afmelding| √| √* |
| Beleid voor voorwaardelijke toegang voor mobiele toepassingen| √| X |
| Naadloze MFA-ervaring voor mobiele toepassingen| √| X |
| Access Microsoft Graph| √| X |

\* Mogelijk, maar Microsoft biedt geen voorbeelden of richtlijnen.

## <a name="oauth-20-and-open-id-connect"></a>OAuth 2.0 en Open ID Connect

OAuth 2.0 is een [industriestandaard](https://oauth.net/2/) protocol voor autorisatie. OIDC (OpenID Connect) is een [industriestandaard](https://openid.net/connect/) identiteitslaag van verificatie gebaseerd op het Oath-2.0-protocol.

### <a name="benefits"></a>Voordelen

Microsoft adviseert OIDC-/ OAuth 2.0 omdat ze verificatie en autorisatie die zijn ingebouwd in de protocollen. Met SAML, moet u bovendien autorisatie implementeren.

De autorisatie die inherent zijn aan deze protocollen kan uw toepassing te openen en te integreren met uitgebreide gebruiker en de bedrijfsgegevens via de Microsoft Graph API.

Met behulp van OAuth 2.0 en OIDC vereenvoudigt de eindgebruikerservaring voor uw klanten bij het kiezen van eenmalige aanmelding voor uw toepassing. U kunt eenvoudig de benodigde machtiging-sets definiëren die vervolgens automatisch worden weergegeven aan de beheerder of gebruiker stemt ermee in dat.

Bovendien kan maakt gebruik van deze protocollen uw klanten voor voorwaardelijke toegang en het MFA-beleid gebruiken voor het beheren van toegang tot de toepassingen. Microsoft biedt de bibliotheken en [codevoorbeelden voor meerdere technologieplatformen](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Samples) om te helpen uw ontwikkeling.  

### <a name="implementation"></a>Implementatie

U kunt uw toepassing registreren bij Microsoft Identity, dit een OAuth 2.0-provider is. U kunt uw toepassing op basis van OAuth 2.0 ook registreren met andere id-Provider die u integreren wilt met. 

Zie voor meer informatie over het registreren van uw toepassing en deze protocollen implementeren voor eenmalige aanmelding tot web-apps [toegang verlenen aan webtoepassingen met OpenID Connect en Azure Active Directory](../develop/sample-v2-code.md).  Voor informatie over hoe u deze protocollen implementeren voor eenmalige aanmelding in mobiele apps, Zie het volgende: 

* [Android](../develop/quickstart-v2-android.md)

* [iOS](../develop/quickstart-v2-ios.md)

* [Universeel Windows-platform](../develop/quickstart-v2-uwp.md)

## <a name="saml-20-and-wsfed"></a>SAML 2.0 en WSFed

Security Assertion Markup Language (SAML) wordt meestal gebruikt voor webtoepassingen. Zie [hoe Azure het SAML-protocol gebruikt](../develop/active-directory-saml-protocol-reference.md) voor een overzicht. 

Web Services Federation (WSFed) is een [industriestandaard](http://docs.oasis-open.org/wsfed/federation/v1.2/ws-federation.html) doorgaans gebruikt voor webtoepassingen die zijn ontwikkeld met behulp van het .net-platform.

### <a name="benefits"></a>Voordelen

SAML 2.0 is een goed ontwikkelde standard- en de meeste technologie platforms ondersteuning voor open source-bibliotheken voor SAML 2.0. U kunt uw klanten bieden een interface voor het configureren van SAML SSO-beheer. Ze kunnen SAML SSO configureren voor Microsoft Azure AD en andere id-provider die ondersteuning biedt voor SAML-2

### <a name="trade-offs"></a>Trade-offs

Wanneer u SAML 2.0 of WSFed protocollen voor mobiele toepassingen, worden bepaalde beleidsregels voor voorwaardelijke toegang, met inbegrip van multi-factor Authentication (MFA) een gedegradeerde ervaring hebben. Ook als u toegang wilt tot de Microsoft Graph, moet u voor het implementeren van autorisatie via OAuth 2.0 voor het genereren van tokens die nodig zijn. 

### <a name="implementation"></a>Implementatie

Microsoft niet bibliotheken bieden voor SAML-implementatie of specifieke bibliotheken aanbevolen. Er zijn veel open source-bibliotheken beschikbaar.

## <a name="sso-and-using-microsoft-graph-rest-api"></a>Eenmalige aanmelding en met behulp van Microsoft Graph Rest-API 

Microsoft Graph is de gegevensinfrastructuur voor alle Microsoft 365, met inbegrip van Office 365, Windows 10 en Enterprise Mobility en Security en aanvullende producten, zoals Dynamics 365. Dit omvat de belangrijkste schema's van de entiteiten, zoals gebruikers, groepen, agenda, E-mail, bestanden en meer, die productiviteit van de gebruiker. Microsoft Graph biedt drie interfaces voor ontwikkelaars een op REST API gebaseerde, Microsoft Graph-gegevens verbinden en Connectors waarmee ontwikkelaars hun eigen gegevens toevoegen in Microsoft Graph.  

Met behulp van een van de bovenstaande protocollen voor eenmalige aanmelding van uw toepassing toegang biedt tot de uitgebreide gegevens beschikbaar zijn via de Microsoft Graph REST-API. Hierdoor kunnen uw klanten meer waarde halen uit hun investering in Microsoft 365. Uw toepassing kan bijvoorbeeld de Microsoft Graph API om te integreren met Office 365-instantie voor uw klanten en surface gebruikers Microsoft Office en SharePoint-items in uw toepassing aanroepen. 

Als u van Open ID Connect gebruikmaakt om te verifiëren, is uw ontwikkeling biedt naadloze omdat u deze OAuth2 gebruikt, kan de basis van Open ID Connect, om te verkrijgen van tokens kan worden gebruikt voor het aanroepen van Microsoft Graph-API's. Als uw toepassing van SAML of WSFed gebruikmaakt, moet u aanvullende code toevoegen in uw toepassing aan deze OAuth2 te verkrijgen van de tokens die zijn vereist voor het aanroepen van Microsoft Graph-API's. 

## <a name="next-steps"></a>Volgende stappen

[SSO inschakelen voor uw toepassing met meerdere tenants](isv-sso-content.md)

[Documentatie voor uw toepassing met meerdere tenants maken](isv-create-sso-documentation.md)
