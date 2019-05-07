---
title: Initialiseren van de client-toepassingen (Microsoft Authentication Library voor JavaScript) | Azure
description: Meer informatie over het initialiseren van clienttoepassingen met behulp van de Microsoft Authentication Library voor JavaScript (MSAL.js).
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: celested
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/12/2019
ms.author: nacanuma
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7990566ca9cd93e79b8356cfd15fda03a7469695
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65138301"
---
# <a name="initialize-client-applications-using-msaljs"></a>Client-toepassingen met behulp van MSAL.js initialiseren
In dit artikel wordt beschreven hoe u bij het initialiseren van Microsoft Authentication Library voor JavaScript (MSAL.js) een exemplaar van een gebruikersagent-toepassing. De toepassing van de gebruikersagent is een vorm van openbare clienttoepassing waarin de clientcode wordt uitgevoerd in een gebruikersagent, zoals een webbrowser. Deze clients geen geheimen, opgeslagen omdat de context van de browser openlijk toegankelijk is. Lees voor meer informatie over de client toepassingstypen en opties voor toepassingsconfiguraties de [overzicht](msal-client-applications.md).

## <a name="prerequisites"></a>Vereisten
Voordat u het initialiseren van een toepassing, moet u eerst [registreren bij Azure portal](scenario-spa-app-registration.md) zodat uw app kan worden geïntegreerd met het Microsoft identity-platform. Na de registratie moet u mogelijk de volgende informatie (die kan worden gevonden in de Azure-portal):

- De client-ID (een tekenreeks die een GUID voor uw toepassing)
- De URL van de id-provider (met de naam van het exemplaar) en de doelgroep aanmelden voor uw toepassing. Deze twee parameters bekend staan als de instantie.
- De tenant-ID als u een line-of-business-toepassing uitsluitend voor uw organisatie (ook met de naam één tenant toepassing ontwikkelt).
- Voor web-apps hebt u ook instellen de redirectUri waarop de id-provider retourneert aan uw toepassing met de beveiligingstokens.

## <a name="initializing-applications"></a>Tijdens de initialisatie van toepassingen

U kunt als volgt MSAL.js gebruiken in een gewone JavaScript/Typescript-toepassing. MSAL verificatiecontext initialiseren door instantiëren `UserAgentApplication` met een configuratieobject. De minimale vereiste configuratie MSAL.js initialiseren is de clientID van uw toepassing die u uit de portal voor appregistratie ophalen moet.

Voor het omleiden van verificatiemethoden met stromen (`loginRedirect` en `acquireTokenRedirect`), moet u expliciet een retouraanroep voor voltooid of fout via registreren `handleRedirectCallback()` methode. Dit is nodig omdat omleiden stromen beloften niet retourneren als de methoden met een pop-ervaring.

```javascript
// Configuration object constructed
const config = {
    auth: {
        clientId: “abcd-ef12-gh34-ikkl-ashdjhlhsdg”
    }
}

// create UserAgentApplication instance
const myMSALObj = new UserAgentApplication(config);

function authCallback(error, response) {
    //handle redirect response
}

// (optional when using redirect methods) register redirect call back for Success or Error
myMSALObj.handleRedirectCallback(authCallback);
```

MSAL.js is ontworpen om een enkele instantie en de configuratie van de `UserAgentApplication` om weer te geven van een verificatie met eenmalige-context. Meerdere exemplaren worden niet aanbevolen omdat ze conflicterende vermeldingen in de cache en het gedrag in de browser veroorzaken.

## <a name="configuration-options"></a>Configuratie-opties

MSAL.js heeft een configuratie-object dat hieronder biedt een groepering van configureerbare opties die beschikbaar zijn voor het maken van een exemplaar van `UserAgentApplication`.

```javascript
type storage = "localStorage" | "sessionStorage";

// Protocol Support
export type AuthOptions = {
    clientId: string;
    authority?: string;
    validateAuthority?: boolean;
    redirectUri?: string | (() => string);
    postLogoutRedirectUri?: string | (() => string);
    navigateToLoginRequestUrl?: boolean;
};

// Cache Support
export type CacheOptions = {
    cacheLocation?: CacheLocation;
    storeAuthStateInCookie?: boolean;
};

// Library support
export type SystemOptions = {
    logger?: Logger;
    loadFrameTimeout?: number;
    tokenRenewalOffsetSeconds?: number;
};

// Developer App Environment Support
export type FrameworkOptions = {
    isAngular?: boolean;
    unprotectedResources?: Array<string>;
    protectedResourceMap?: Map<string, Array<string>>;
};

// Configuration Object
export type Configuration = {
    auth: AuthOptions,
    cache?: CacheOptions,
    system?: SystemOptions,
    framework?: FrameworkOptions
};
```

Hieronder ziet u het totale aantal configuratieopties die momenteel worden ondersteund in de config-object:

- **clientID**: Vereist. De clientID van uw toepassing, moet u dit vanaf de portal voor appregistratie.

- **Instantie**: Optioneel. Een URL die wijzen op een map die u MSAL kunt tokens van aanvragen. Standaardwaarde: `https://login.microsoftonline.com/common`.
    * In Azure AD, is van het formulier https://&lt;exemplaar&gt;/&lt;doelgroep&gt;, waarbij &lt;exemplaar&gt; is het id-provider-domein (bijvoorbeeld `https://login.microsoftonline.com`) en &lt;doelgroep&gt; is een id voor de doelgroep aanmelden. Dit kan de volgende waarden zijn:
        * `https://login.microsoftonline.com/<tenant>`-tenant is een domein dat is gekoppeld aan de tenant, zoals contoso.onmicrosoft.com, of de GUID die de `TenantID` eigenschap van de map die alleen aan te melden bij de gebruikers van een specifieke organisatie wordt gebruikt.
        * `https://login.microsoftonline.com/common`-Gebruikt voor aanmelding bij gebruikers met een werk- en schoolaccounts of een persoonlijk Microsoft-account.
        * `https://login.microsoftonline.com/organizations/`-Gebruikt voor aanmelding bij gebruikers met een werk-en schoolaccounts.
        * `https://login.microsoftonline.com/consumers/` -Wordt gebruikt voor het aanmelden van gebruikers met alleen persoonlijke Microsoft-account (live).
    * In Azure AD B2C, is van het formulier `https://<instance>/tfp/<tenant>/<policyName>/`, waar exemplaar van het domein van Azure AD B2C, is de tenant is de naam van de Azure AD B2C-tenant, policyName is de naam van de B2C-beleid toe te passen.


- **validateAuthority**: Optioneel.  Valideer de verlener van tokens. De standaardwaarde is `true`. Voor B2C-toepassingen, omdat de waarde van de instantie is bekend en per beleid afwijken kan, de autoriteit validatie werkt niet en moet worden ingesteld op `false`.

- **redirectUri**: Optioneel.  De omleidings-URI van uw app, waarbij verificatiereacties kunnen worden verzonden en ontvangen door uw app. Het moet exact overeenkomen met een van de omleidings-URI's die u in de portal hebt geregistreerd, behalve dat het URL-codering moet worden. Standaard ingesteld op `window.location.href`.

- **postLogoutRedirectUri**: Optioneel.  De gebruiker wordt omgeleid `postLogoutRedirectUri` na het afmelden. De standaardwaarde is `redirectUri`.

- **navigateToLoginRequestUrl**: Optioneel. De mogelijkheid om standaardnavigatie naar pagina start na aanmelding uitschakelen. De standaardinstelling is true. Dit wordt alleen gebruikt voor omleiding stromen.

- **cacheLocation**: Optioneel.  Hiermee stelt u browser opslag op een `localStorage` of `sessionStorage`. De standaardwaarde is `sessionStorage`.

- **storeAuthStateInCookie**: Optioneel.  Met deze markering is geïntroduceerd in MSAL.js v0.2.2 als een oplossing voor de [lus verificatieproblemen](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#1-issues-due-to-security-zones) op Microsoft Internet Explorer en Microsoft Edge. De vlag inschakelen `storeAuthStateInCookie` om op te lossen waar, om te profiteren van dit. Als deze optie is ingeschakeld, slaat MSAL.js de aanvraagstatus verificatie is vereist voor de validatie van het auth-stromen in de browsercookies. Standaard deze vlag is ingesteld op `false`.

- **logger**: Optioneel.  Een Logger-object met een retouraanroep-instantie die kan worden geboden door de ontwikkelaar te gebruiken en logboeken op een aangepaste manier publiceren. Zie voor meer informatie over de logger-object doorgeven, [logboekregistratie met msal.js](msal-logging.md).

- **loadFrameTimeout**: Optioneel.  Er is een time-out opgetreden voor het aantal milliseconden van inactiviteit voordat een antwoord van de vernieuwing van het token uit Azure AD moet worden beschouwd. De standaardwaarde is 6 seconden.

- **tokenRenewalOffsetSeconds**: Optioneel. Het aantal milliseconden dat Hiermee stelt u het venster van offset die nodig zijn om de token vóór de verloopdatum te verlengen. De standaardwaarde is 300 milliseconden.

Deze zijn alleen van toepassing op worden doorgegeven van de MSAL Angular-wrapper-bibliotheek:
- **unprotectedResources**: Optioneel.  Matrix van URI's van niet-beveiligde resources. MSAL wordt een token niet koppelen aan uitgaande aanvragen waarvoor deze URI. Standaard ingesteld op `null`.

- **protectedResourceMap**: Optioneel.  Dit is toewijzing van resources aan bereiken door MSAL gebruikt voor het automatisch koppelen van toegangstokens in web API-aanroepen. Één toegangstoken wordt verkregen voor de resource. Zodat u kunt een specifieke resourcepad als volgt toewijzen: {"https://graph.microsoft.com/v1.0/me", ["user.read"]}, of de app-URL van de resource als: {"https://graph.microsoft.com/", ["user.read", "mail.send"]}. Dit is vereist voor CORS-aanroepen. Standaard ingesteld op `null`.
