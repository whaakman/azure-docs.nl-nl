---
title: Ontwikkeling van Microsoft identity-platform - Azure
description: Meer informatie over Microsoft identity-platform, een evolutie van het Azure Active Directory (Azure AD) identity-service en developer-platform.
services: active-directory
documentationcenter: dev-center-name
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: celested
ms.reviewer: agirling, saeeda, benv
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: a0b912b6f3fe42c724468347f9b3a7f0b4efa054
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65067950"
---
# <a name="evolution-of-microsoft-identity-platform"></a>Ontwikkeling van Microsoft identity-platform

Het Microsoft Identity Platform bouwt voort op de identiteitsservice en het ontwikkelaarsplatform van Azure Active Directory (Azure AD). Deze service kunnen ontwikkelaars toepassingen ontwikkelen die aanmelden van gebruikers, het ophalen van tokens voor het aanroepen van API's, zoals Microsoft Graph of API's waarmee ontwikkelaars hebt gemaakt. Het bestaat uit een authentication-service, open source-bibliotheken, registratie van toepassingen en -configuratie (via een portal voor ontwikkelaars en API-toepassing), volledige ontwikkelaarsdocumentatie, Quick Start-voorbeelden, codevoorbeelden, handleidingen, zelfstudies en de inhoud van andere ontwikkelaars. Het Microsoft Identity Platform biedt ondersteuning voor standaardprotocollen als OAuth 2.0 en OpenID Connect.

Tot nu toe hebt de meeste ontwikkelaars gewerkt met de Azure AD v1.0-platform om te verifiëren werk en schoolaccounts (ingericht door Azure AD) door het aanvragen van tokens van het eindpunt van de v1.0 Azure AD met behulp van Azure AD Authentication Library (ADAL) voor Azure-portal registratie van toepassingen en configuratie, en Azure AD Graph-API voor programmatische toepassingsconfiguratie.

Met Microsoft identity-platform (v2.0), moet u uw bereik uitbreiden voor deze soorten gebruikers:

- Werk- en schoolaccounts accounts (Azure AD ingericht-accounts)
- Persoonlijke accounts (zoals Outlook.com of Hotmail.com)
- Uw klanten die hun eigen e-mailadres of sociale id (zoals LinkedIn, Facebook, Google) via de Azure AD B2C-aanbieding

Met de geïntegreerde Microsoft identity-platform, kunt u één keer code schrijven en een Microsoft-identiteit verifiëren in uw toepassing. Er is een volledig ondersteunde open-source-bibliotheek met de naam Microsoft Authentication Library (MSAL) voor verschillende platforms. MSAL is eenvoudig te gebruiken, wordt er goede eenmalige aanmelding (SSO) voor uw gebruikers, kunt u hoge betrouwbaarheid en prestaties, en is ontwikkeld met behulp van Microsoft Secure Development Lifecycle (SDL). Bij het aanroepen van API's, kunt u uw toepassing om te profiteren van incrementele toestemming te geven, zodat u kunt de aanvraag voor goedkeuring van meer Invasief bereiken uitstellen totdat het gebruik van de toepassing dit garandeert tijdens runtime kunt configureren.

U kunt de Azure-portal gebruiken om te registreren en uw toepassing configureren en gebruiken van de Microsoft Graph-API voor programmatische toepassingsconfiguratie.

Uw toepassing in uw eigen tempo bijwerken. Toepassingen die zijn gebouwd met ADAL-bibliotheken worden nog steeds ondersteund. Gemengde toepassing portfolio, die bestaan uit toepassingen die zijn ontwikkeld met ADAL en toepassingen die zijn gebouwd met MSAL-bibliotheken, worden ook ondersteund. Dit betekent dat toepassingen met behulp van de meest recente ADAL en de meest recente MSAL SSO wordt geleverd in de portfolio, geleverd door de gedeelde tokencache tussen deze bibliotheken. Toepassingen die zijn gewijzigd van ADAL in MSAL handhaaft gebruiker aanmelden status na de upgrade.

## <a name="microsoft-identity-platform-experience"></a>Gebruikerservaring met het Microsoft Identity Platform

In het volgende schema wordt de gebruikerservaring met Microsoft Identity weergegeven op hoog niveau, inclusief de registratie van apps, SDK’s, eindpunten en ondersteunde identiteiten.

![Het Microsoft Identity Platform vandaag](./media/about-microsoft-identity-platform/about-microsoft-identity-platform.svg)

### <a name="app-registration-experience"></a>App-registratie-ervaring

De Azure-portal **[App-registraties](https://go.microsoft.com/fwlink/?linkid=2083908)** ervaring is de één portal-ervaring voor het beheren van alle toepassingen die u hebt geïntegreerd met Microsoft identity-platform. Als u de Portal voor Appregistratie gebruikt al, moet u beginnen met behulp van de Azure portal-app-registratie-ervaring in plaats daarvan.

Voor integratie met Azure AD B2C (bij het verifiëren van sociale of lokale identiteiten), moet u uw toepassing registreren in een B2C-tenant. Deze ervaring is ook onderdeel van de Azure-portal.

De **application-API in Microsoft Graph** is momenteel in preview. Gebruik deze API via een programma configureren van uw toepassingen die zijn geïntegreerd met Microsoft identity-platform voor het verifiëren van een Microsoft-identiteit. Echter, totdat deze API algemene beschikbaarheid wordt bereikt, moet u de API van Azure AD Graph 1.6 en het toepassingsmanifest.

### <a name="msal-libraries"></a>MSAL-bibliotheken

De MSAL-bibliotheek kunt u toepassingen bouwen die alle Microsoft-identiteit verifiëren. De MSAL-bibliotheken in .NET zijn algemeen beschikbaar. MSAL-bibliotheken voor JavaScript, iOS en Android zijn in preview en geschikt zijn voor gebruik in een productieomgeving. We bieden de dezelfde productie-ondersteuning voor MSAL-bibliotheken in preview als wij doen voor versies van MSAL en ADAL die algemeen beschikbaar.

U kunt ook de MSAL-bibliotheken aan uw toepassing integreren met Azure AD B2C.

Server-side-bibliotheken voor het bouwen van web-apps en web-API's zijn algemeen beschikbaar: [ASP.NET](https://docs.microsoft.com/aspnet/overview) en [ASP.NET Core](https://docs.microsoft.com/aspnet/core/?view=aspnetcore-2.2)

### <a name="microsoft-identity-platform-endpoint"></a>Microsoft identity-platform-eindpunt

Microsoft identity-platform (v2.0)-eindpunt is nu gecertificeerd OIDC. Het werkt met de Microsoft Authentication Libraries (MSAL) of andere standaarden-compatibele bibliotheek. Mens leesbaar scopes, in overeenstemming met het industriële standaarden geïmplementeerd.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over v1.0 en v2.0.

* [Overzicht van Microsoft identity-platform (v2.0)](v2-overview.md)
* [Azure Active Directory voor ontwikkelaars (v1.0)-overzicht](v1-overview.md)