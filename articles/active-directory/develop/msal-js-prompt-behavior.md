---
title: Vragen gedrag in interactieve aanvragen (Microsoft Authentication Library voor JavaScript) | Azure
description: Meer informatie over het gedrag van de vragen in interactieve aanroepen met behulp van de Microsoft Authentication Library voor JavaScript (MSAL.js) aan te passen.
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/24/2019
ms.author: nacanuma
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: dd0d736345f312f1a1d6f8f029b41429a3e5f0a7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65544272"
---
# <a name="prompt-behavior-in-msaljs-interactive-requests"></a>Gedrag vragen in MSAL.js interactieve aanvragen

Wanneer een gebruiker heeft gemaakt van een actief Azure AD-sessie met meerdere gebruikersaccounts, wordt de Azure AD-aanmeldingspagina standaard de gebruiker gevraagd een account selecteren voordat u doorgaat met het aanmelden. Een account selecteren als er slechts één geverifieerde sessie met Azure AD-ervaring zichtbaar niet voor gebruikers.

De MSAL.js-bibliotheek (te beginnen in v0.2.4) verzendt geen een aanvraagpromptparameter tijdens de interactieve aanvragen (`loginRedirect`, `loginPopup`, `acquireTokenRedirect` en `acquireTokenPopup`), en alle gedrag vragen daarmee niet wordt afgedwongen. Voor op de achtergrond token aanvragen met behulp van de `acquireTokenSilent` methode, MSAL.js geeft een prompt parameter is ingesteld op `none`.

U kunt het gedrag van de vragen voor de interactieve aanvragen door de instelling voor de parameter prompt in de parameters van de aanvraag doorgegeven aan de methoden op basis van uw toepassingsscenario, beheren. Bijvoorbeeld, als u wilt aanroepen van de ervaring van de selectie van account:

```javascript
var request = {
    scopes: ["user.read"],
    prompt: 'select_account',
}

userAgentApplication.loginRedirect(request);
```


De volgende vragen waarden kunnen worden doorgegeven bij het verifiëren met Azure AD:

**Aanmelding:** Deze waarde wordt de gebruiker referenties invoert op de verificatieaanvraag forceren.

**select_account:** Deze waarde wordt voorzien van de gebruiker een selectie ervaring met alle accounts in de sessie.

**toestemming geven:** Deze waarde roept de dialoog van de OAuth-toestemming waarmee gebruikers machtigen om de app.

**Geen:** Deze waarde zorgt ervoor dat de gebruiker een interactieve prompt niet te zien. Het verdient aanbeveling niet moeten worden doorgegeven van deze waarde aan interactieve methoden in MSAL.js omdat het kan onverwacht gedrag. Gebruik in plaats daarvan de `acquireTokenSilent` methode om te bereiken op de achtergrond aanroepen.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de `prompt` parameter in de [impliciete goedkeuring voor OAuth 2.0](v2-oauth2-implicit-grant-flow.md) protocol welke MSAL.js-bibliotheek wordt gebruikt.
