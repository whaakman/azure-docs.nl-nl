---
title: Verschillen tussen MSAL.js en ADAL.js | Azure
description: Meer informatie over de verschillen tussen Microsoft Authentication Library voor JavaScript (MSAL.js) en Azure AD Authentication Library voor JavaScript (ADAL.js) en hoe u kunt kiezen welke te gebruiken.
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/10/2019
ms.author: nacanuma
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 72da1130efc288c03229d990b75bc96c941265d7
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2019
ms.locfileid: "65544304"
---
# <a name="differences-between-msal-js-and-adal-js"></a>Verschillen tussen MSAL JS en ADAL JS

Zowel Microsoft Authentication Library voor JavaScript (MSAL.js) en Azure AD Authentication Library voor JavaScript (ADAL.js) worden gebruikt voor verificatie van Azure AD-entiteiten en aanvragen tokens van Azure AD. Tot nu toe heeft de meeste ontwikkelaars gewerkt met Azure AD voor ontwikkelaars (v1.0) voor verificatie van Azure AD-identiteiten (werk en school-accounts) door aan te vragen met behulp van ADAL. Nu kunt u MSAL.js gebruikt, u verifiëren een bredere set van Microsoft-identiteiten (Azure AD-identiteiten en Microsoft-accounts en sociale en lokale accounts via Azure AD B2C) via Microsoft identity-platform (v2.0).

In dit artikel wordt beschreven hoe u kiezen tussen de Microsoft Authentication Library voor JavaScript (MSAL.js) en Azure AD Authentication Library voor JavaScript (ADAL.js) en vergelijkt de twee bibliotheken.

## <a name="choosing-between-adaljs-and-msaljs"></a>Kiezen tussen ADAL.js en MSAL.js

Dit is de nieuwste generatie van Microsoft-verificatiebibliotheken in de meeste gevallen die u wilt gebruiken voor de Microsoft identity-platform en MSAL.js. Met behulp van MSAL.js, u aanschaft, tokens voor gebruikers aanmelden bij uw toepassing met Azure AD (werk en school-accounts), Microsoft (persoonlijk)-accounts (MSA) of Azure AD B2C.

Als u al bekend met het eindpunt (en ADAL.js) v1.0 bent, wilt u mogelijk lezen [wat is er anders bij het v2.0-eindpunt?](active-directory-v2-compare.md).

U moet echter wel ADAL.js gebruiken als uw toepassing moet aan te melden bij de gebruikers met eerdere versies van [Active Directory Federation Services (ADFS)](/windows-server/identity/active-directory-federation-services).

## <a name="key-differences-in-authentication-with-msaljs"></a>Belangrijke verschillen in de verificatie met MSAL.js

### <a name="core-api"></a>Core API

* Maakt gebruik van ADAL.js [AuthenticationContext](https://github.com/AzureAD/azure-activedirectory-library-for-js/wiki/Config-authentication-context#authenticationcontext) als de weergave van een exemplaar van van uw toepassing verbinding met de autorisatie-server of de id-provider via een URL-instantie. Daarentegen MSAL.js API is ontworpen rond gebruiker agent-clienttoepassing (een vorm van openbare clienttoepassing waarin de clientcode wordt uitgevoerd in een gebruikersagent, zoals een webbrowser). Het biedt de `UserAgentApplication` klasse vertegenwoordigt een exemplaar van de context van verificatie van de toepassing met de autorisatie-server. Zie voor meer informatie, [geïnitialiseerd met behulp van MSAL.js](msal-js-initializing-client-applications.md).

* In ADAL.js, methoden voor het verkrijgen van tokens zijn gekoppeld aan een enkele instantie is ingesteld de `AuthenticationContext`. In MSAL.js, kunnen het token aanvragen ophalen, nemen de waarden van de andere instantie van dan wat is ingesteld in de `UserAgentApplication`. Hiermee wordt MSAL.js te verwerven en tokens afzonderlijk cache voor meerdere tenants en gebruikersaccounts in dezelfde toepassing.

* De methode voor het verkrijgen en tokens op de achtergrond vernieuwen zonder dat gebruikers wordt gevraagd de naam `acquireToken` in ADAL.js. In MSAL.js, deze methode met de naam `acquireTokenSilent` moet meer beschrijvende van deze functionaliteit.

### <a name="authority-value-common"></a>Instantie-waarde `common`

In v1.0, met behulp van de `https://login.microsoftonline.com/common` instantie, kunnen gebruikers zich aanmelden met een Azure AD-account (voor elke organisatie).

In versie 2.0, met behulp van de `https://login.microsoftonline.com/common` -instantie, kunnen gebruikers zich aanmelden met een Azure AD-organisatie-account of een persoonlijk Microsoft-account (MSA). Als u wilt de aanmelding beperken tot alleen Azure AD-accounts (hetzelfde gedrag als met ADAL.js), moet u gebruiken `https://login.microsoftonline.com/organizations`. Zie voor meer informatie, de `authority` optie config in [geïnitialiseerd met behulp van MSAL.js](msal-js-initializing-client-applications.md).

### <a name="scopes-for-acquiring-tokens"></a>Bereiken voor het verkrijgen van tokens
* Bereik in plaats van de resourceparameter in verificatieaanvragen te verkrijgen van tokens

    v2.0-protocol maakt gebruik van bereiken in plaats van de resource in de aanvragen. Met andere woorden, wanneer uw toepassing nodig heeft om aan te vragen van tokens met machtigingen voor een resource, zoals MS Graph, is het verschil in waarden worden doorgegeven aan de bibliotheek-methoden als volgt:

    V1.0: bron =https://graph.microsoft.com

    v2.0: bereik = https://graph.microsoft.com/User.Read

    U kunt aanvragen bereiken voor elke resource API met behulp van de URI van de API in deze indeling: appidURI of het bereik voor een voorbeeld: https://mytenant.onmicrosoft.com/myapi/api.read

    Alleen voor de MS Graph API, een waarde voor het bereik `user.read` wordt toegewezen aan https://graph.microsoft.com/User.Read en door elkaar kunnen worden gebruikt.

    ```javascript
    var request = {
        scopes = ["User.Read"];
    };

    acquireTokenPopup(request);   
    ```

* Dynamische bereiken voor incrementele toestemming.

    Bij het bouwen van toepassingen die gebruikmaken van v1.0, die u nodig zijn voor het registreren van de volledige set machtigingen (statische bereiken) vereist voor de toepassing voor de gebruiker toe te staan op het moment van de aanmelding. In versie 2.0, kunt u de scope-parameter om aan te vragen van de machtigingen op het moment dat u wilt dat ze. Dit zijn dynamische scopes genoemd. Hierdoor kan de gebruiker incrementele toestemming te bereiken. Dus als aan het begin wilt u alleen dat de gebruiker zich aanmeldt bij uw toepassing en u elk soort toegang hoeft, u kunt doen. Als later u de mogelijkheid om te lezen van de agenda van de gebruiker moet, kunt u vervolgens de scope agenda in de acquireToken-methoden aanvragen en ophalen van de gebruiker toestemming. Bijvoorbeeld:

    ```javascript
    var request = {
        scopes = ["https://graph.microsoft.com/User.Read", "https://graph.microsoft.com/Calendar.Read"];
    };

    acquireTokenPopup(request);   
    ```

* Bereiken voor V1.0 API 's

    Bij het ophalen van tokens voor V1.0 API's met behulp van MSAL.js, kunt u de statische bereiken geregistreerd op de API door toe te voegen aanvragen `.default` naar de URI van de App-ID van de API als bereik. Bijvoorbeeld:

    ```javascript
    var request = {
        scopes = [ appidURI + "/.default"];
    };

    acquireTokenPopup(request);
    ```

## <a name="next-steps"></a>Volgende stappen
Raadpleeg voor meer informatie, [vergelijking v1.0 en v2.0](active-directory-v2-compare.md).
