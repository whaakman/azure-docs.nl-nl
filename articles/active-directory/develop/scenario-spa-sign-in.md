---
title: Toepassing met één pagina (aanmelden) - Microsoft identity-platform
description: Informatie over het bouwen van een toepassing met één pagina (aanmelden)
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/06/2019
ms.author: nacanuma
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: fc9c46ae28960387e6f8efc1ade20afa1c77ef55
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65138790"
---
# <a name="single-page-application---sign-in"></a>Single-page toepassing - aanmelding

Informatie over het aanmelden toevoegen aan de code voor uw toepassing met één pagina.

Voordat u tokens voor toegang tot API's in uw toepassing krijgen kan, moet u de context van een geverifieerde gebruiker. U kunt gebruikers aanmelden bij uw toepassing in MSAL.js op twee manieren:

* [Meld u aan met een pop-upvenster](#sign-in-with-a-pop-up-window) met behulp van `loginPopup` methode
* [Meld u aan met omleiden](#sign-in-with-redirect) met behulp van `loginRedirect` methode

U kunt eventueel ook de bereiken van de API's die u de gebruiker moet om toestemming op het moment van aanmelding doorgeven.

> [!NOTE]
> Als uw toepassing al toegang tot een geverifieerde gebruikerscontext of een id heeft-token, kunt u de aanmeldings-stap overslaan en tokens rechtstreeks te verkrijgen. Zie voor meer informatie, [eenmalige aanmelding zonder aanmelding msal.js](msal-js-sso.md#sso-without-msaljs-login).

## <a name="choosing-between-a-pop-up-or-redirect-experience"></a>Kiezen tussen een pop-upvenster- of omleidings-ervaring

U kunt een combinatie van methoden met het pop-upvenster en de omleidings-niet gebruiken in uw toepassing. De keuze tussen een pop-upvenster- of omleidings-ervaring, is afhankelijk van de stroom van uw toepassing.

* Als u niet dat de gebruiker wilt verlaat de pagina van uw hoofdtoepassing tijdens de verificatie, is het raadzaam om de pop-methoden te gebruiken. Omdat de verificatie-omleiding in een pop-upvenster gebeurt, wordt de status van de hoofdtoepassing behouden.

* Er zijn bepaalde gevallen waarin u mogelijk de omleidings-methoden te gebruiken. Als gebruikers van uw toepassing hebt browser beperkingen of beleid waarbij windows pop-ups zijn uitgeschakeld, kunt u de omleidings-methoden. De omleidings-methoden gebruiken met Internet Explorer-browser, omdat er bepaalde zijn [bekende problemen met Internet Explorer](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser) bij het verwerken van pop-upvensters.

## <a name="sign-in-with-a-pop-up-window"></a>Meld u aan met een pop-upvenster

### <a name="javascript"></a>Javascript

```javascript
const loginRequest = {
    scopes: ["user.read", "user.write"]
}

userAgentApplication.loginPopup(loginRequest).then(function (loginResponse) {
    //login success
    let idToken = loginResponse.idToken;
}).catch(function (error) {
    //login failure
    console.log(error);
});
```

### <a name="angular"></a>Angular

De MSAL Angular-wrapper kunt u specifieke routes in uw toepassing te beveiligen door alleen toe te voegen de `MsalGuard` aan de definitie van de route. Deze guard roept de methode aan te melden wanneer die route wordt geopend.

```javascript
// In app.routes.ts
{ path: 'product', component: ProductComponent, canActivate : [MsalGuard],
    children: [
      { path: 'detail/:id', component: ProductDetailComponent  }
    ]
   },
  { path: 'myProfile' ,component: MsGraphComponent, canActivate : [MsalGuard] },
```

Inschakelen voor een ervaring pop-upvenster de `popUp` config-optie. U kunt ook de bereiken waarvoor toestemming als volgt doorgeven:

```javascript
//In app.module.ts
@NgModule({
  imports: [ MsalModule.forRoot({
                clientID: 'your_app_id',
                popUp: true,
                consentScopes: ["user.read", "user.write"]
            })]
         })
```

## <a name="sign-in-with-redirect"></a>Meld u aan met-omleiding

### <a name="javascript"></a>Javascript

De omleidings-methoden retourneren een promise vanwege de navigatie van de belangrijkste app niet. Om te verwerken en toegang tot de geretourneerde tokens, moet u succes- en callbacks registreren voordat u de omleidings-methoden aanroept.

```javascript
function authCallback(error, response) {
    //handle redirect response
}

userAgentApplication.handleRedirectCallback(authCallback);

const loginRequest = {
    scopes: ["user.read", "user.write"]
}

userAgentApplication.loginRedirect(loginRequest);
```

### <a name="angular"></a>Angular

Hier de code is hetzelfde als bij het aanmelden bij een sectie pop-upvenster hierboven wordt beschreven. De standaard-stroom is omleiden.

> [!NOTE]
> De ID-token bevat niet de toegestane bereiken en alleen vertegenwoordigt de geverifieerde gebruiker. De toegestane bereiken worden geretourneerd in het toegangstoken dat u, in de volgende stap aanschaft wordt.

## <a name="sign-out"></a>Afmelden

De MSAL-bibliotheek biedt een `logout` methode die de cache in de browser-opslag worden gewist en een afmelden aanvraag verzendt naar Azure AD. Na het afmelden, wordt hij omgeleid naar de startpagina van de toepassing standaard.

U kunt configureren dat de URI waarnaar wordt u omgeleid na teken uit door in te stellen de `postLogoutRedirectUri`. Deze URI moet ook worden geregistreerd als de URI Logout in de registratie van uw toepassing.

### <a name="javascript"></a>Javascript

```javascript
const config = {

    auth: {
        clientID: 'your_app_id',
        redirectUri: "your_app_redirect_uri", //defaults to application start page
        postLogoutRedirectUri: "your_app_logout_redirect_uri"
    }
}

const userAgentApplication = new UserAgentApplication(config);
userAgentApplication.logout();

```

### <a name="angular"></a>Angular

```javascript
//In app.module.ts
@NgModule({
  imports: [ MsalModule.forRoot({
                clientID: 'your_app_id',
                postLogoutRedirectUri: "your_app_logout_redirect_uri"
            })]
         })

// In app.component.ts
this.authService.logout();
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Ophalen van een token voor de app.](scenario-spa-acquire-token.md)
