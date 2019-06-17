---
title: Toepassing met één pagina (een token voor het aanroepen van een API ophalen) - Microsoft identity-platform
description: Informatie over het bouwen van een toepassing met één pagina (ophalen een token voor het aanroepen van een API)
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: nacanuma
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: f4c842db8a0874d3619e0dc59b90aa12226cb984
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65138819"
---
# <a name="single-page-application---acquire-a-token-to-call-an-api"></a>Toepassing van één pagina - een token voor het aanroepen van een API ophalen

Het patroon voor het verkrijgen van tokens voor API's met MSAL.js is om te proberen eerst een op de achtergrond token aan te vragen met de `acquireTokenSilent` methode. Wanneer deze methode wordt aangeroepen, wordt in de bibliotheek eerst de cache in de browser-opslag om te zien als een geldig token bestaat en wordt gecontroleerd. Wanneer er geen geldig token in de cache, verzendt deze een op de achtergrond tokenaanvraag naar Azure Active Directory (Azure AD) van een verborgen iframe. Deze methode kunt ook de bibliotheek vernieuwen van tokens. Zie voor meer informatie over de sessie voor eenmalige aanmelding en de waarden van de levensduur van tokens in Azure AD, [levensduur van token](active-directory-configurable-token-lifetimes.md).

De op de achtergrond token aanvragen voor Azure AD kunnen mislukken bij enkele redenen, zoals een verlopen Azure AD-sessie of een wachtwoordwijziging. In dat geval kunt u een van de interactieve methoden (die de gebruiker gevraagd) aanroepen voor het verkrijgen van tokens.

* [Token met een pop-upvenster verkrijgen](#acquire-token-with-a-pop-up-window) gebruiken `acquireTokenPopup`
* [Token met omleiding ophalen](#acquire-token-with-redirect) gebruiken `acquireTokenRedirect`

**Kiezen tussen een pop-upvenster- of omleidings-ervaring**

 U kunt een combinatie van methoden met het pop-upvenster en de omleidings-niet gebruiken in uw toepassing. De keuze tussen een pop-upvenster- of omleidings-ervaring, is afhankelijk van de stroom van uw toepassing.

* Als u niet dat de gebruiker wilt verlaat de pagina van uw hoofdtoepassing tijdens de verificatie, is het raadzaam om de pop-methoden te gebruiken. Omdat de verificatie-omleiding in een pop-upvenster gebeurt, wordt de status van de hoofdtoepassing behouden.

* Er zijn bepaalde gevallen waarin u mogelijk de omleidings-methoden te gebruiken. Als gebruikers van uw toepassing hebt browser beperkingen of beleid waarbij windows pop-ups zijn uitgeschakeld, kunt u de omleidings-methoden. Het is ook raadzaam de omleidings-methoden gebruiken met Internet Explorer-browser, omdat er bepaalde zijn [bekende problemen met Internet Explorer](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser) bij het verwerken van pop-upvensters.

U kunt de API-bereiken die u wilt dat het toegangstoken om op te nemen bij het bouwen van de aanvraag voor een toegangstoken instellen. Houd er rekening mee dat alle vereiste bereiken kan niet in het toegangstoken worden verleend en is afhankelijk van de gebruiker toestemming.

## <a name="acquire-token-with-a-pop-up-window"></a>Token met een pop-upvenster ophalen

### <a name="javascript"></a>Javascript

Het bovenstaande patroon met behulp van de methoden voor een pop-ervaring:

```javascript
const accessTokenRequest = {
    scopes: ["user.read"]
}

userAgentApplication.acquireTokenSilent(accessTokenRequest).then(function(accessTokenResponse) {
    // Acquire token silent success
    // call API with token
    let accessToken = accessTokenResponse.accessToken;
}).catch(function (error) {
    //Acquire token silent failure, send an interactive request.
    if (error.errorMessage.indexOf("interaction_required") !== -1) {
        userAgentApplication.acquireTokenPopup(accessTokenRequest).then(function(accessTokenResponse) {
            // Acquire token interactive success
        }).catch(function(error) {
            // Acquire token interactive failure
            console.log(error);
        });
    }
    console.log(error);
});
```

### <a name="angular"></a>Angular

De MSAL Angular-wrapper biedt het gemak van het toevoegen van de HTTP-interceptor `MsalInterceptor` die automatisch op de achtergrond toegangstokens verkrijgen en koppelt u ze aan de HTTP-aanvragen voor API's.

U kunt opgeven dat de bereiken voor API's in de `protectedResourceMap` config-optie die het MsalInterceptor vraagt automatisch tokens te verkrijgen.

```javascript
//In app.module.ts
@NgModule({
  imports: [ MsalModule.forRoot({
                clientID: 'your_app_id',
                protectedResourceMap: {"https://graph.microsoft.com/v1.0/me", ["user.read", "mail.send"]}
            })]
         })

providers: [ ProductService, {
        provide: HTTP_INTERCEPTORS,
        useClass: MsalInterceptor,
        multi: true
    }
   ],
```

Voor het slagen of mislukken van de op de achtergrond ophalen van tokens biedt MSAL Angular callbacks die u kunt zich abonneren. Het is ook belangrijk om te onthouden opzeggen.

```javascript
// In app.component.ts
 ngOnInit() {
    this.subscription=  this.broadcastService.subscribe("msal:acquireTokenFailure", (payload) => {
    });
}

ngOnDestroy() {
   this.broadcastService.getMSALSubject().next(1);
   if(this.subscription) {
     this.subscription.unsubscribe();
   }
 }
```

U kunt ook kunt u met behulp van de ophalen-token als methode, zoals beschreven in de kernbibliotheek MSAL.js ook expliciet verkrijgen.

## <a name="acquire-token-with-redirect"></a>Token met omleiding ophalen

### <a name="javascript"></a>Javascript

Het patroon is afwijkend hierboven, maar weergegeven met een omleidingsmethode voor het interactief token verkrijgen. Houd er rekening mee dat u de omleidings-callback registreren moet, zoals hierboven vermeld.

```javascript
function authCallback(error, response) {
    //handle redirect response
}

userAgentApplication.handleRedirectCallback(authCallback);

const accessTokenRequest: AuthenticationParameters = {
    scopes: ["user.read"]
}

userAgentApplication.acquireTokenSilent(accessTokenRequest).then(function(accessTokenResponse) {
    // Acquire token silent success
    // call API with token
    let accessToken = accessTokenResponse.accessToken;
}).catch(function (error) {
    //Acquire token silent failure, send an interactive request.
    console.log(error);
    if (error.errorMessage.indexOf("interaction_required") !== -1) {
        userAgentApplication.acquireTokenRedirect(accessTokenRequest);
    }
});
```

### <a name="angular"></a>Angular

Dit is hetzelfde als hierboven beschreven.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een Web-API aanroepen](scenario-spa-call-api.md)
