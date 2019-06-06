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
ms.openlocfilehash: 9d09436b9a2ac38e7b07a51f01d65769ed19d08e
ms.sourcegitcommit: 087ee51483b7180f9e897431e83f37b08ec890ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/31/2019
ms.locfileid: "66430821"
---
# <a name="public-client-and-confidential-client-applications"></a>Openbare clients en vertrouwelijke client-toepassingen
Microsoft Authentication Library (MSAL) definieert twee soorten clients: openbare en vertrouwelijke clients. De twee clienttypen worden onderscheiden door de mogelijkheid om te verifiëren veilig met de autorisatieserver en onderhouden van de vertrouwelijkheid van de referenties van de client. Daarentegen, zogenaamde maakt gebruik van Azure AD Authentication Library (ADAL) *verificatiecontext* (dit is een verbinding met Azure AD).

- **Vertrouwelijke clienttoepassingen** zijn apps die worden uitgevoerd op servers (web-apps, Web-API-apps of zelfs service/daemon-apps). Deze worden beschouwd als moeilijk toegang krijgen tot, en daarom kan een toepassingsgeheim te houden. Vertrouwelijke clients kunnen de configuratie van eenmalige geheimen bevatten. Elk exemplaar van de client heeft een afzonderlijke configuratie (met inbegrip van de client-ID en clientgeheim). Deze waarden zijn moeilijk voor eindgebruikers om op te halen. Een web-app is de meest voorkomende vertrouwelijke client. De client-ID is toegankelijk via de webbrowser, maar het geheim is doorgegeven alleen in het back-kanaal en nooit rechtstreeks worden blootgesteld.

    Vertrouwelijke client-apps: <BR>
    ![Web-app](media/msal-client-applications/web-app.png) ![Web-API](media/msal-client-applications/web-api.png) ![Daemon-service](media/msal-client-applications/daemon-service.png)

- **Openbare clienttoepassingen** zijn apps die worden uitgevoerd op apparaten of pc's of in een webbrowser. Ze zijn niet vertrouwd voor het veilig bewaren toepassingsgeheimen, zodat ze alleen toegang krijgen Web-API's namens de gebruiker tot. (Ze ondersteunen alleen openbare client stromen.) Openbare clients kunnen geen configuratie-time-geheimen, bevatten, zodat ze geen geheimen van de client.

    Openbare client-apps: <BR>
    ![Desktop-app](media/msal-client-applications/desktop-app.png) ![Browserless API](media/msal-client-applications/browserless-app.png) ![mobiele app](media/msal-client-applications/mobile-app.png)

> [!NOTE]
> In MSAL.js is er geen scheiding van openbare en vertrouwelijke client-apps.  MSAL.js staat voor client-apps als gebruiker agent gebaseerde apps, openbare clients waarbij de clientcode wordt uitgevoerd in een van de gebruikersagent, zoals een webbrowser. Deze clients geen geheimen worden opgeslagen omdat de context van de browser openlijk toegankelijk is.

## <a name="comparing-the-client-types"></a>Vergelijking van de clienttypen
Hier volgen enkele overeenkomsten en verschillen tussen openbare client en vertrouwelijke client apps:

- Beide soorten app onderhouden van een token cache van gebruiker en kunnen een token verkrijgen op de achtergrond (wanneer het token al in de cache van token is). Vertrouwelijke client-apps hebben ook een app-tokencache voor tokens die zijn bedoeld voor de app zelf.
- Beide typen app kunnen gebruikersaccounts beheren en ophalen van een account van de gebruiker tokencache, ophalen van een account van de id of een account te verwijderen.
- Openbare client-apps hebt vier manieren om een token (vier verificatiestromen) te verkrijgen. Vertrouwelijke client-apps kunnen op drie manieren om een token te verkrijgen (en een manier voor het berekenen van de URL van de id-provider eindpunt voor autorisatie). Zie voor meer informatie, [verkrijgen van tokens](msal-acquire-cache-tokens.md).

Als u bekend bent met ADAL, ziet u dat, in tegenstelling tot de ADAL-verificatiecontext in MSAL de client-ID (ook wel de *toepassings-ID* of *app-ID*) één keer bij het samenstellen van de toepassing wordt doorgegeven. Het hoeft niet opnieuw worden doorgegeven als de app een token verkrijgt. Dit geldt voor zowel voor openbare en vertrouwelijke client-apps. Constructors van vertrouwelijke client-apps worden ook clientreferenties doorgegeven: het geheim die ze met de id-provider delen.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over:
- [Opties voor toepassingsconfiguraties client](msal-client-application-configuration.md)
- [Clienttoepassingen instantiëren met behulp van MSAL.NET](msal-net-initializing-client-applications.md)
- [Clienttoepassingen instantiëren met behulp van MSAL.js](msal-js-initializing-client-applications.md)
