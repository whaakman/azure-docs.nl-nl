---
title: Client-toepassingen (Microsoft Authentication Library) | Azure
description: Meer informatie over openbare client en vertrouwelijke client toepassingen in de Microsoft Authentication Library (MSAL).
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/25/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9e0300ec0ef4ee67b06acb85514ae898bbd0a830
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2019
ms.locfileid: "65544290"
---
# <a name="public-client-and-confidential-client-applications"></a>Openbare clients en vertrouwelijke client-toepassingen
Microsoft Authentication Library (MSAL) definieert twee soorten clients: openbare en vertrouwelijke clients. De twee clienttypen worden onderscheiden door de mogelijkheid om te verifiëren veilig met de autorisatieserver en onderhouden van de vertrouwelijkheid van de referenties van de client.  Azure AD Authentication Library (ADAL) heeft daarentegen het concept van verificatiecontext (dit is een verbinding met Azure AD).

- **Vertrouwelijke clienttoepassingen** zijn toepassingen die worden uitgevoerd op servers (Web-Apps, Web-API of zelfs service/daemon-toepassingen). Deze worden beschouwd als moeilijk te openen en daarom kan een toepassingsgeheim te houden. Vertrouwelijke clients kunnen voor het opslaan van configuratie van eenmalige geheimen. Elk exemplaar van de client heeft een afzonderlijke configuratie (met inbegrip van clientId en geheim). Deze waarden zijn moeilijk voor eindgebruikers om op te halen. Een web-app is de meest voorkomende vertrouwelijke client. De client-ID is toegankelijk via de webbrowser, maar het geheim is doorgegeven alleen in het back-kanaal en nooit rechtstreeks worden blootgesteld.

    Vertrouwelijke client-apps: <BR>
    ![Web-app](media/msal-client-applications/web-app.png) ![Web-API](media/msal-client-applications/web-api.png) ![Daemon-service](media/msal-client-applications/daemon-service.png)

- **Openbare clienttoepassingen** zijn toepassingen die worden uitgevoerd op apparaten of desktopcomputers of in een webbrowser. Ze zijn niet vertrouwd voor de toepassingsgeheimen veilig te houden, en daarom alleen toegang tot Web-API's namens de gebruiker (ze alleen ondersteuning voor openbare client stromen). Openbare clients kunnen geen configuratie van eenmalige geheimen houdt, en als gevolg hiervan hebben geen clientgeheim.

    Openbare clienttoepassingen: <BR>
    ![Desktop-app](media/msal-client-applications/desktop-app.png) ![Browserless API](media/msal-client-applications/browserless-app.png) ![mobiele app](media/msal-client-applications/mobile-app.png)

> [!NOTE]
> In MSAL.js is er geen scheiding van openbare en vertrouwelijke client-apps.  MSAL.js staat voor client-apps als gebruiker agent-gebaseerde apps, een openbare client waarin de clientcode wordt uitgevoerd in een gebruikersagent, zoals een webbrowser.  Deze clients geen geheimen, opgeslagen omdat de context van de browser openlijk toegankelijk is.

## <a name="comparing-the-client-types"></a>Vergelijking van de clienttypen
Er zijn enkele commonalities en verschillen tussen openbare client en vertrouwelijke client toepassingen:

- Beide soorten toepassingen onderhouden van een gebruiker token cache en een token op de achtergrond (in gevallen waarin het token al in de cache van token is) kunnen ophalen. Vertrouwelijke clienttoepassingen hebben ook een app-tokencache voor tokens die voor de app zelf zijn.
- Beide kunnen gebruikersaccounts beheren en de accounts ophalen uit de cache van gebruiker-token, ophalen van een account van de id of een account te verwijderen.
- Openbare clienttoepassingen zijn vier manieren van het ophalen van een token (vier verificatiestromen), dat vertrouwelijke clienttoepassingen drie hebben (en een methode voor het berekenen van de URL van de id-provider eindpunt voor autorisatie). Zie voor meer informatie, scenario's en het verkrijgen van tokens.

Als u ADAL in het verleden hebt gebruikt, zult u merken dat, in tegenstelling tot de context van de ADAL-verificatie, in MSAL de client-ID (ook toepassings-ID of app-ID genoemd) één keer bij het samenstellen van de toepassing, en niet langer doorgegeven wordt moet worden herhaald een token te verkrijgen. Dit geldt zowel voor openbare en vertrouwelijke client-toepassingen. Constructors van vertrouwelijke client-toepassingen worden ook clientreferenties doorgegeven: het geheim die ze met de id-provider delen.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over:
- [Opties voor toepassingsconfiguraties client](msal-client-application-configuration.md)
- [Het instantiëren van clienttoepassingen met behulp van MSAL.NET](msal-net-initializing-client-applications.md).
- [Het instantiëren van clienttoepassingen met behulp van MSAL.js](msal-js-initializing-client-applications.md).
