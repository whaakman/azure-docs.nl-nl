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
ms.openlocfilehash: eb0acd1534bab11eb57a7aa0e695f192b5999ed2
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65076493"
---
# <a name="scenario-mobile-application-that-calls-web-apis"></a>Scenario: Mobiele toepassing of aanroepen van web-API 's

Meer informatie over alles die wat u nodig voor het bouwen van een mobiele app die web-API's aanroept.

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

Bij het bouwen van een mobiele app, is het essentieel een persoonlijke, naadloze gebruikerservaring.  Microsoft identity-platform kan ontwikkelaars van mobiele hiervoor exact voor iOS en Android-gebruikers. Uw toepassing kunt aanmelden met Azure AD, persoonlijke Microsoft-account en Azure AD B2C-gebruikers en tokens voor het aanroepen van een web-API in hun naam te verkrijgen. Voor het implementeren van deze stromen, gebruiken we Microsoft Authentication Library (MSAL) waarmee de industrienorm [OAuth 2.0-autorisatiecodestroom](v2-oauth2-auth-code-flow.md).

![Daemon-apps](./media/scenarios/mobile-app.svg)

Overwegingen voor mobiele Apps:

- ***Gebruikerservaring is essentieel***: Toestaan dat gebruikers de waarde van uw app zien voordat waarin wordt gevraagd voor aanmelding bij en alleen de benodigde machtigingen aanvragen.
- ***Ondersteuning voor alle gebruikersconfiguraties***: Er zijn veel mobiele zakelijke gebruikers onder voorwaardelijke toegang en nalevingsbeleid voor apparaten. Zorg ervoor dat deze belangrijke scenario's ondersteunen.
- ***Implementeren van eenmalige aanmelding (SSO)***: MSAL en Microsoft identity-platform kunnen inschakelen van eenmalige aanmelding eenvoudig via de browser van het apparaat of de Microsoft Authenticator (en Intune-bedrijfsportal voor Android).

## <a name="specifics"></a>Specifieke informatie

Bij het bouwen van een mobiele app van Microsoft identity-platform, heeft de end-to-end-ervaring enkele overwegingen:

- Afhankelijk van het platform, zonder tussenkomst van aanmelden niet mogelijk op het eerste teken in. iOS-, vereist bijvoorbeeld dat voor apps om weer te geven tussenkomst van de gebruiker bij het ophalen van eenmalige aanmelding van de eerste keer door de Microsoft Authenticator (en de Intune-bedrijfsportal voor Android).
- Op iOS en Android, kan MSAL een externe browsers (die kunnen worden weergegeven boven op uw app) gebruiken voor het aanmelden van gebruikers. Dit kan worden aangepast voor het gebruik van webweergaven van het in-app in plaats daarvan.
- Gebruik nooit een geheim in een mobiele toepassing, is toegankelijk voor alle gebruikers.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Appregistratie](scenario-mobile-app-registration.md)
