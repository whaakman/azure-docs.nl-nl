---
title: Doorgeven van aangepaste status in verificatieaanvragen (Microsoft Authentication Library voor JavaScript) | Azure
description: Leer hoe u een waarde van de parameter aangepaste status doorgeven in met behulp van de Microsoft Authentication Library voor JavaScript (MSAL.js) verificatieaanvraag voor.
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
ms.date: 05/29/2019
ms.author: nacanuma
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: f29d84838ddb11ac359d7a04dbce8e39dd05ac01
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66420496"
---
# <a name="pass-custom-state-in-authentication-requests-using-msaljs"></a>Aangepaste status in aanvragen voor authenticatie met behulp van MSAL.js doorgeven
De *status* parameter, zoals gedefinieerd door de OAuth 2.0 is opgenomen in een verificatieaanvraag en wordt ook geretourneerd als in het token antwoord om te voorkomen dat de aanvraag voor cross-site kunnen worden vervalst aanvallen. Standaard Microsoft Authentication Library voor JavaScript (MSAL.js) geeft een willekeurig gegenereerde unieke *status* waarde voor de parameter in de verificatie-aanvragen.

De parameter state kan ook worden gebruikt voor de gecodeerde gegevens van de status van de app voordat omleidings. U kunt de status van de gebruiker in de app, zoals de pagina of de weergave die ze al had geopend, als invoer voor deze parameter doorgeven. De bibliotheek MSAL.js kunt u uw aangepaste status doorgeven als de parameter state in de `Request` object:

```javascript
// Request type
export type AuthenticationParameters = {
    scopes?: Array<string>;
    extraScopesToConsent?: Array<string>;
    prompt?: string;
    extraQueryParameters?: QPDict;
    claimsRequest?: string;
    authority?: string;
    state?: string;
    correlationId?: string;
    account?: Account;
    sid?: string;
    loginHint?: string;
};
```

Bijvoorbeeld:

```javascript
let loginRequest = {
    scopes: ["user.read", "user.write"],
    state: “page_url”
}

myMSALObj.loginPopup(loginRequest);
```

De status van de doorgegeven wordt toegevoegd aan de unieke GUID die is ingesteld door MSAL.js bij het verzenden van de aanvraag. Als het antwoord wordt geretourneerd, MSAL.js controleert op een status-overeenkomst en retourneert vervolgens het aangepaste doorgegeven in staat in de `Response` object als `accountState`.

```javascript
export type AuthResponse = {
    uniqueId: string;
    tenantId: string;
    tokenType: string;
    idToken: IdToken;
    accessToken: string;
    scopes: Array<string>;
    expiresOn: Date;
    account: Account;
    accountState: string;
};
```

Lees voor meer informatie over [het bouwen van een toepassing met één pagina (SPA)](scenario-spa-overview.md) MSAL.js te gebruiken.