---
title: Azure AD v2 iOS Getting Started - Test | Microsoft Docs
description: Hoe iOS (Swift)-toepassingen met een API waarvoor toegangstokens door Azure Active Directory-v2-eindpunt kunnen aanroepen
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.openlocfilehash: 4a88096d2b0a23708acdbc1798eac528599b4f71
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
## <a name="test-querying-the-microsoft-graph-api-from-your-ios-application"></a>Opvragen van de Microsoft Graph-API van uw iOS-toepassing testen

Druk op `Command`  +  `R` de code uitvoeren in de simulator.

![Schermopname van een steekproef](media/active-directory-mobileanddesktopapp-ios-test/iostestscreenshot.png)

Wanneer u klaar bent om te testen, tikt u op *'Microsoft Graph API aanroepen'* en wordt u gevraagd uw gebruikersnaam en wachtwoord typen.

### <a name="consent"></a>Toestemming
De eerste keer dat u zich aanmeldt bij uw toepassing worden weergegeven met een scherm toestemming vergelijkbaar met de waarin u wilt accepteren expliciet hieronder:

![Toestemming scherm](media/active-directory-mobileanddesktopapp-ios-test/iosconsentscreen.png)

### <a name="expected-results"></a>Verwachte resultaten
Ziet u informatie over gebruikersprofielen geretourneerd door de Microsoft Graph-API-aanroep in de *logboekregistratie* sectie.

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Meer informatie over bereikwaarden en gedelegeerde machtigingen

De Microsoft Graph API vereist de `user.read` bereik van het gebruikersprofiel lezen. Deze scope wordt automatisch toegevoegd standaard in elke toepassing die wordt geregistreerd op onze portal voor wachtwoordregistratie. Sommige andere API's voor Microsoft Graph evenals aangepaste API's voor uw back-endserver moet mogelijk extra scopes. Bijvoorbeeld: voor het bereik van Microsoft Graph `Calendars.Read` is vereist voor een lijst met agenda's van de gebruiker. Voor toegang tot de agenda van de gebruiker in een context van een toepassing, moet u toevoegen de `Calendars.Read` overgedragen machtigingen voor de toepassingsregistratie-informatie en voeg vervolgens de `Calendars.Read` bereik voor de `acquireTokenSilent` aanroepen. De gebruiker kan gevraagd om extra toestemmingen als u het aantal scopes verhogen.

<!--end-collapse-->



