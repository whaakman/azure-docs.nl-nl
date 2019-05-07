---
title: Overzicht van Microsoft identity-platform (v2.0) - Azure
description: Meer informatie over de Microsoft identity-platform (v2.0) eindpunt en het platform.
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
ms.openlocfilehash: f6aa0af1ff5a8600f43ab92a5f2534a8e3ff253c
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65067671"
---
# <a name="microsoft-identity-platform-v20-overview"></a>Overzicht van Microsoft identity-platform (v2.0)

Het Microsoft Identity Platform bouwt voort op de identiteitsservice en het ontwikkelaarsplatform van Azure Active Directory (Azure AD). Deze service kunnen ontwikkelaars toepassingen ontwikkelen die zich in alle Microsoft-identiteit en ophalen van tokens voor het aanroepen van Microsoft APIs, zoals Microsoft Graph of API's waarmee ontwikkelaars hebt gebouwd. Het Microsoft identity-platform bestaat uit:

- **OAuth 2.0 en OpenID Connect-standaard-compatibele verificatieservice** die biedt ontwikkelaars de mogelijkheid om te verifiëren van een Microsoft-identiteit, met inbegrip van:
  - Werk- of schoolaccount accounts (ingericht via Azure AD)
  - Persoonlijke Microsoft-accounts (zoals Skype, Xbox en Outlook.com)
  - Lokale of sociale accounts (via Azure AD B2C)
- **Open source-bibliotheken**: Microsoft Authentication Libraries (MSAL) en ondersteuning voor andere standaarden-compatibele bibliotheken
- **Application management-portal**: Een registratie en configuratie van ervaring gebouwd in Azure portal, samen met alle uw andere Azure beheermogelijkheden.
- **Toepassingsconfiguratie API en PowerShell**: waarmee programmatische configuratie van uw toepassingen via REST-API (Microsoft Graph en Azure Active Directory Graph 1.6) en PowerShell, zodat u uw DevOps-taken kunt automatiseren.
- **Inhoud voor ontwikkelaars**: conceptuele en verwijzen naar documentatie, voorbeelden van de Quick Start, codevoorbeelden, zelfstudies en handleidingen.

Microsoft identity-platform biedt voor ontwikkelaars, naadloze integratie met innovaties in de ruimte identiteit en beveiliging, zoals de configuratie-verificatie, meer bevoegdheden verificatie en voorwaardelijke toegang.  U hoeft niet te implementeren van een dergelijke functionaliteit: toepassingen die zijn geïntegreerd met het Microsoft identity-platform systeemeigen te profiteren van dergelijke innovaties.

U kunt met Microsoft identity-platform, één keer code schrijven en bereik elke gebruiker. U kunt één keer een app bouwen en deze werkt op vele platformen gebruiken, of een app maken die als een client, evenals een resourcetoepassing (API fungeert).

## <a name="getting-started"></a>Aan de slag

Werken met identiteit beschikt niet over moeten worden, moeilijk. Kies een scenario waarin op u van toepassing, elk pad scenario heeft een snelstartgids en een overzichtspagina waarmee u omhoog en binnen enkele minuten:

- [Bouw een app met één pagina](scenario-spa-overview.md)
- [Bouw een web-app waarmee gebruikers worden aangemeld](scenario-web-app-sign-user-overview.md)
- [Bouw een web-app die web-API's aanroepen](scenario-web-app-call-api-overview.md)
- [Een beveiligde web-API bouwen](scenario-protected-web-api-overview.md)
- [Een web-API die aanroepen van web-API's bouwen](scenario-web-api-call-api-overview.md)
- [Een desktop-app bouwen](scenario-desktop-overview.md)
- [Een daemon-app bouwen](scenario-daemon-overview.md)
- [Bouw een mobiele app](scenario-mobile-overview.md)

<!--- We are making updates to the application scenarios chart. This is placeholder text for it.

The following chart outlines the full set of authentication scenarios and best practices – use it as a reference when integrating the Microsoft identity platform before shipping your app.

[![Application scenarios in Microsoft identity platform](./media/v2-overview/application-scenarios-identity-platform.png)](./media/v2-overview/application-scenarios-identity-platform.svg#lightbox)

--->

## <a name="next-steps"></a>Volgende stappen

Als u meer informatie over verificatie op basis van core wilt, raden we dat u begint met de volgende onderwerpen:

- [De basisbeginselen van verificatie](authentication-scenarios.md)
- [Toepassing en service-principals](app-objects-and-service-principals.md)
- [doelgroepen](v2-supported-account-types.md)
- [Machtigingen en toestemming](v2-permissions-and-consent.md)
- [ID-tokens](id-tokens.md) en [toegangstokens](access-tokens.md)

Een toepassing boordevol gegevens die worden aangeroepen bouwen [Microsoft Graph](https://docs.microsoft.com/graph/overview).

Wanneer u bent klaar om te starten van uw app in een **productieomgeving**, deze aanbevolen procedures:

- [Logboekregistratie inschakelen](msal-logging.md) in uw toepassing.
- Schakel telemetrie in uw toepassing.
- Schakel [proxy's en aanpassen van HTTP-clients](msal-net-provide-httpclient.md).
- Uw integratie testen door de [Microsoft identity-platform-integratie controlelijst](identity-platform-integration-checklist.md).

## <a name="learn-more"></a>Meer informatie

Als u had planningsstap voor het bouwen van een klantgerichte-toepassing die zich aanmeldt sociale en lokale identiteiten, kijk dan eens de [overzicht van Azure AD B2C](https://docs.microsoft.com/azure/active-directory-b2c/tutorial-add-identity-providers).