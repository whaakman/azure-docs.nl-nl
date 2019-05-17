---
title: Eenmalige aanmelding (Microsoft Authentication Library voor JavaScript) | Azure
description: Meer informatie over het bouwen van eenmalige aanmelding-ervaringen met behulp van de Microsoft Authentication Library voor JavaScript (MSAL.js).
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
ms.openlocfilehash: 9f1f102307256852ac92616c7fb707e0e2739e5d
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2019
ms.locfileid: "65544136"
---
# <a name="single-sign-on-with-msaljs"></a>Eenmalige aanmelding met MSAL.js

Eenmalige aanmelding (SSO) kunnen gebruikers eenmaal zijn referenties invoeren om te melden bij en zorg dat u een sessie die kan worden hergebruikt voor meerdere toepassingen zonder opnieuw te verifiëren. Dit biedt een naadloze ervaring voor de gebruiker en vermindert de herhaalde vragen om referenties.

Azure AD biedt mogelijkheden voor eenmalige aanmelding tot toepassingen door in te stellen van een sessiecookie wanneer de eerste keer door de gebruiker wordt geverifieerd. De bibliotheek MSAL.js zorgt ervoor dat toepassingen gebruikmaken van dit in een aantal manieren.

## <a name="sso-between-browser-tabs"></a>Eenmalige aanmelding tussen tabbladen in browser

Wanneer uw toepassing geopend in meerdere tabbladen is en u zich eerst bij de gebruiker op een tabblad aanmelden, wordt de gebruiker ook ondertekend in op de andere tabbladen zonder dat u wordt gevraagd. MSAL.js slaat de ID-token voor de gebruiker in de browser `localStorage` en wordt de gebruiker zich aanmeldt bij de toepassing op de andere geopende tabbladen.

Gebruikt standaard MSAL.js `sessionStorage` die staat niet toe dat de sessie worden gedeeld tussen tabbladen. Als u eenmalige aanmelding tussen tabbladen, zorg ervoor dat om in te stellen de `cacheLocation` in MSAL.js te `localStorage` zoals hieronder wordt weergegeven.

```javascript
const config = {
    auth: {
        clientId: “abcd-ef12-gh34-ikkl-ashdjhlhsdg”
    },
    cache: {
        cacheLocation: 'localStorage'
    }
}

const myMSALObj = new UserAgentApplication(config);
```

## <a name="sso-between-apps"></a>Eenmalige aanmelding tussen apps

Wanneer een gebruiker zich verifieert, wordt een sessiecookie ingesteld op de Azure AD-domein in de browser. MSAL.js, is afhankelijk van deze sessiecookie voor eenmalige aanmelding voor de gebruiker tussen verschillende toepassingen. MSAL.js worden ook de ID-tokens en tokens voor toegang van de gebruiker in de browser-opslag per toepassingsdomein opgeslagen. Als gevolg hiervan wordt het gedrag van eenmalige aanmelding varieert voor verschillende aanvragen:  

### <a name="applications-on-the-same-domain"></a>Toepassingen op hetzelfde domein

Wanneer toepassingen worden gehost in hetzelfde domein, wordt de gebruiker één keer zich kunt aanmelden bij een app en vervolgens geverifieerd naar de andere apps zonder ernaar te vragen. MSAL.js maakt gebruik van de tokens in de cache opgeslagen voor de gebruiker op het domein bieden van eenmalige aanmelding.

### <a name="applications-on-different-domain"></a>Toepassingen op een ander domein

Wanneer toepassingen worden gehost in verschillende domeinen bevinden, kunnen niet de tokens in de cache op een domein worden geopend door MSAL.js in domein B.

Dit betekent dat wanneer gebruikers bent aangemeld bij het domein een navigeren naar een toepassing op domein B, ze worden omgeleid of gevraagd of u met de Azure AD-pagina. Omdat Azure AD nog steeds de sessiecookie van de gebruiker heeft, wordt de gebruiker wordt aangemeld en moeten ze niet de referenties opnieuw invoeren. Als de gebruiker meerdere gebruikersaccounts in een sessie met Azure AD heeft, wordt de gebruiker gevraagd om op te halen van het relevante account zich aanmelden met.

### <a name="automatically-select-account-on-azure-ad"></a>Selecteer automatisch-account in Azure AD

In bepaalde gevallen is de toepassing toegang heeft tot de context van de verificatie van de gebruiker en wil de Azure AD-account selectie prompt voorkomen wanneer meerdere accounts worden aangemeld.  Dit kan een aantal verschillende manieren worden gedaan:

**Met behulp van de sessie-ID (SID)**

Sessie-ID is een [optionele claim](active-directory-optional-claims.md) die kan worden geconfigureerd in de ID-tokens. Deze claim kan de toepassing om te identificeren van de gebruiker Azure AD-sessie die onafhankelijk van de accountnaam van de gebruiker of gebruikersnaam. U kunt de beveiligings-id doorgeven in de parameters van de aanvraag naar de `acquireTokenSilent` aanroepen. Hierdoor kunnen Azure AD om de Accountselectie over te slaan. Beveiligings-id is gekoppeld aan de sessiecookie en wordt niet overschreden door de browser contexten.

```javascript
var request = {
    scopes: ["user.read"],
    sid: sid
}

userAgentApplication.acquireTokenSilent(request).then(function(response) {
        const token = response.accessToken
    }
).catch(function (error) {  
        //handle error
});
```

> [!Note]
> Beveiligings-id kan alleen worden gebruikt met verificatie op de achtergrond van aanvragen van `acquireTokenSilent` in MSAL.js aanroepen.
U vindt de stappen voor het configureren van optionele claims in het toepassingsmanifest [hier](active-directory-optional-claims.md).

**Met behulp van aanmelding Hint**

Als u geen SID claim geconfigureerd of nodig hebt om het account wordt gevraagd een selectie in interactieve authentication-oproepen over te slaan, kunt u doen door op te geven een `login_hint` in de aanvraagparameters en eventueel een `domain_hint` als `extraQueryParameters` in de MSAL.js interactieve methoden (`loginPopup`, `loginRedirect`, `acquireTokenPopup` en `acquireTokenRedirect`). Bijvoorbeeld:

```javascript
var request = {
    scopes: ["user.read"],
    loginHint: preferred_username,
    extraQueryParameters: {domain_hint: 'organizations'}
}

userAgentApplication.loginRedirect(request);
```

U kunt de waarden voor login_hint en domain_hint krijgen door het lezen van de claims geretourneerd in het ID-token voor de gebruiker.

* **loginHint** moet worden ingesteld op de `preferred_username` claim in het ID-token.

* **domain_hint** alleen is vereist om te worden doorgegeven bij het gebruik van de/Common instantie. De domeinhint wordt bepaald door de tenant ID(tid).  Als de `tid` claim in het ID-token is `9188040d-6c67-4c5b-b112-36a304b66dad` consumenten is. Anders wordt deze organisaties.

Lezen [hier](v2-oauth2-implicit-grant-flow.md) voor meer informatie over de waarden voor de hint van aanmelding en een geheugensteun voor domein.

> [!Note]
> U kunt geen beveiligings-id en login_hint doorgeven op hetzelfde moment. Dit leidt foutbericht.

## <a name="sso-without-msaljs-login"></a>Eenmalige aanmelding zonder MSAL.js aanmelding

Standaard vereist MSAL.js dat een aanmeldingsmethode wordt aangeroepen voor het maken van de context van een gebruiker voor het ophalen van tokens voor API's. Aangezien aanmeldingsmethoden interactieve, ziet de gebruiker een prompt.

Er zijn bepaalde gevallen waarin toepassingen toegang tot de context van de geverifieerde gebruiker hebben of de ID-token dat door middel van verificatie in een andere toepassing heeft gestart en u wilt gebruikmaken van eenmalige aanmelding te verkrijgen van tokens zonder het eerst aanmelden via MSAL.js.

Een voorbeeld hiervan is: Een gebruiker is aangemeld bij een bovenliggende-webtoepassing die als host fungeert voor een andere JavaScript-toepassing die wordt uitgevoerd als een invoegtoepassing of -invoegtoepassing.

De SSO-ervaring in dit scenario kan als volgt worden bereikt:

Geeft de `sid` indien beschikbaar (of `login_hint` en eventueel `domain_hint`) als parameters voor de MSAL.js aanvragen `acquireTokenSilent` aanroepen als volgt:

```javascript
var request = {
    scopes: ["user.read"],
    loginHint: preferred_username,
    extraQueryParameters: {domain_hint: 'organizations'}
}

userAgentApplication.acquireTokenSilent(request).then(function(response) {
        const token = response.accessToken
    }
).catch(function (error) {  
        //handle error
});
```

## <a name="sso-in-adaljs-to-msaljs-update"></a>Eenmalige aanmelding in ADAL.js MSAL.js update

MSAL.js zorgt voor functiepariteit met ADAL.js voor scenario's Azure AD-verificatie. De migratie van ADAL.js naar MSAL.js om eenvoudig te maken en om te voorkomen dat u wordt gevraagd uw gebruikers zich opnieuw aanmelden, de bibliotheek leest de ID-token van de gebruikerssessie ADAL.js cache en naadloos meldt zich aan de gebruiker in MSAL.js.  

Als u wilt profiteren van het gedrag van eenmalige aanmelding (SSO) bij het bijwerken van ADAL.js, moet u ervoor dat de bibliotheken `localStorage` voor het opslaan van tokens. Stel de `cacheLocation` naar `localStorage` in de configuratie van zowel de MSAL.js en de ADAL.js tijdens de initialisatie als volgt te werk:


```javascript
// In ADAL.js
window.config = {
   clientId: 'g075edef-0efa-453b-997b-de1337c29185',
   cacheLocation: 'localStorage'
};

var authContext = new AuthenticationContext(config);


// In latest MSAL.js version
const config = {
    auth: {
        clientId: “abcd-ef12-gh34-ikkl-ashdjhlhsdg”
    },
    cache: {
        cacheLocation: 'localStorage'
    }
}

const myMSALObj = new UserAgentApplication(config);
```

Nadat deze is geconfigureerd, worden MSAL.js kan de status van de cache van de geverifieerde gebruiker in ADAL.js lezen en dat bieden van eenmalige aanmelding in MSAL.js te gebruiken.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de [eenmalige aanmelding-sessie en levensduur van vernieuwingstoken](active-directory-configurable-token-lifetimes.md) waarden in Azure AD.
