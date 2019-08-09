---
title: Mobiele app die web-Api's aanroept-overzicht | Micro soft Identity-platform
description: Meer informatie over het bouwen van een mobiele app die web-Api's aanroept (overzicht)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.reviwer: brandwe
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: f67f063ab374dec8bac3905ea46f1dfa9d8ed1fb
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2019
ms.locfileid: "68852595"
---
# <a name="scenario-mobile-application-that-calls-web-apis"></a>Scenario: Mobiele toepassing die web-Api's aanroept

Lees alles wat u nodig hebt om een mobiele app te bouwen die web-Api's aanroept.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [Prerequisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="getting-started"></a>Aan de slag

Maak uw eerste mobiele toepassing en probeer een Snelstartgids uit te proberen.

> [!div class="nextstepaction"]
> [Snelstart: Een token verkrijgen en Microsoft Graph-API aanroepen vanuit een Android-app](./quickstart-v2-android.md)
>
> [Snelstart: Een token verkrijgen en Microsoft Graph-API aanroepen vanuit een iOS-app](./quickstart-v2-ios.md)
>
> [Snelstart: Een token verkrijgen en Microsoft Graph-API aanroepen vanuit een Xamarin iOS & Android-app](https://github.com/Azure-Samples/active-directory-xamarin-native-v2)

## <a name="overview"></a>Overzicht

Een persoonlijke, naadloze gebruikers ervaring is essentieel voor mobiele apps.  Met micro soft Identity platform kunnen mobiele ontwikkel aars die ervaring voor iOS-en Android-gebruikers maken. Uw toepassing kan gebruikers (Azure AD), persoonlijke Microsoft-account gebruikers en Azure AD B2C gebruikers Azure Active Directory aanmelden en tokens verkrijgen om namens u een web-API aan te roepen. Voor het implementeren van deze stromen gebruiken we micro soft Authentication Library (MSAL), waarmee de standaard [OAuth 2.0-autorisatie code stroom](v2-oauth2-auth-code-flow.md)wordt geïmplementeerd.

![Daemon-apps](./media/scenarios/mobile-app.svg)

Overwegingen voor mobiele apps:

- **Gebruikers ervaring is sleutel**: Gebruikers toestaan om de waarde van uw app te zien voordat hij of zij zich aanmeldt en alleen de vereiste machtigingen aanvragen.
- **Ondersteuning voor alle gebruikers configuraties**: Veel mobiele zakelijke gebruikers bevinden zich onder voorwaardelijke toegang en nalevings beleid voor apparaten. Zorg ervoor dat u deze belang rijke scenario's ondersteunt.
- **Eenmalige aanmelding (SSO) implementeren**: MSAL en micro soft Identity platform maken het inschakelen van eenmalige aanmelding eenvoudig via de browser van het apparaat of de Microsoft Authenticator (en Intune-bedrijfsportal op Android).

## <a name="specifics"></a>Opsporingsgegevens

Houd rekening met het volgende wanneer u een mobiele app op het micro soft Identity-platform bouwt:

- Afhankelijk van het platform is het mogelijk dat de gebruiker zich voor de eerste keer aanmeldt. IOS vereist bijvoorbeeld dat Apps gebruikers interactie weer geven bij gebruik van SSO de eerste keer door Microsoft Authenticator (en Intune-bedrijfsportal op Android).
- Op iOS en Android kan MSAL gebruikmaken van een externe browser (die mogelijk boven op uw app wordt weer gegeven) om gebruikers aan te melden. U kunt de configuratie aanpassen voor gebruik in-app-webweergaven.
- Gebruik nooit een geheim in een mobiele toepassing. Het is toegankelijk voor alle gebruikers.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Appregistratie](scenario-mobile-app-registration.md)
