---
title: Mobiele app, die aanroepen van web-API's - overzicht | Microsoft identity-platform
description: Informatie over het bouwen van een mobiele app of aanroepen van web-API's (overzicht)
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: dadobali
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: d9978905f8ae0719f6a77ca2351f2c43bbf94ee0
ms.sourcegitcommit: f013c433b18de2788bf09b98926c7136b15d36f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/13/2019
ms.locfileid: "65550372"
---
# <a name="scenario-mobile-application-that-calls-web-apis"></a>Scenario: Mobiele toepassing of aanroepen van web-API 's

Meer informatie over alles die wat u nodig om te weten een mobiele App die web-API's aanroept.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [Prerequisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="getting-started"></a>Aan de slag

Uw eerste mobiele toepassing maken en gebruik een snelstartgids.

> [!div class="nextstepaction"]
> [Snelstart: Een token verkrijgen en Microsoft Graph API aanroepen vanuit een Android-app](./quickstart-v2-android.md)
>
> [Snelstart: Een token verkrijgen en Microsoft Graph API aanroepen vanuit een iOS-app](./quickstart-v2-ios.md)
>
> [Snelstart: Een token verkrijgen en Microsoft Graph API aanroepen vanuit een Xamarin iOS- en Android-app](https://github.com/Azure-Samples/active-directory-xamarin-native-v2)

## <a name="overview"></a>Overzicht

Een persoonlijke, naadloze gebruikerservaring is het essentieel is voor mobiele apps.  Microsoft identity-platform kan mobiele ontwikkelaars die ervaring voor iOS en Android-gebruikers maken. Uw toepassing kan melden bij Azure Active Directory (Azure AD), persoonlijke Microsoft-accountgebruikers en Azure AD B2C-gebruikers en tokens voor het aanroepen van een web-API namens hen verkrijgen. Voor het implementeren van deze stromen, gebruiken we Microsoft Authentication Library (MSAL), waarmee de industrienorm [OAuth 2.0-autorisatiecodestroom](v2-oauth2-auth-code-flow.md).

![Daemon-apps](./media/scenarios/mobile-app.svg)

Overwegingen voor mobiele apps:

- **Gebruikerservaring is essentieel**: Toestaan dat gebruikers de waarde van uw app zien voordat waarin wordt gevraagd voor aanmelding bij en alleen de vereiste machtigingen aanvragen.
- **Ondersteuning voor alle gebruikersconfiguraties**: Er zijn veel mobiele zakelijke gebruikers onder voorwaardelijke toegang en nalevingsbeleid voor apparaten. Zorg ervoor dat deze belangrijke scenario's ondersteunen.
- **Implementeren van eenmalige aanmelding (SSO)**: MSAL en Microsoft identity-platform kunnen inschakelen van eenmalige aanmelding eenvoudig via de browser van het apparaat of de Microsoft Authenticator (en Intune-bedrijfsportal voor Android).

## <a name="specifics"></a>Specifieke informatie

Houd er rekening mee met de volgende overwegingen wanneer u een mobiele app in Microsoft identity-platform maken:

- Afhankelijk van het platform, sommige tussenkomst van de gebruiker mogelijk vereist zijn de eerste keer dat gebruikers zich aanmelden. Voor moet iOS bijvoorbeeld apps om weer te geven tussenkomst van de gebruiker bij het gebruik van eenmalige aanmelding voor het eerst via de Microsoft Authenticator (en de Intune-bedrijfsportal voor Android).
- Op iOS en Android kan MSAL een externe browser (die mogelijk worden weergegeven boven op uw app) gebruiken voor het aanmelden van gebruikers. U kunt de configuratie voor het gebruik van webweergaven van het in-app in plaats daarvan kunt aanpassen.
- Gebruik nooit een geheim in een mobiele toepassing. Deze is toegankelijk voor alle gebruikers.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Appregistratie](scenario-mobile-app-registration.md)
