---
title: Toepassing met één pagina (aanmelden)-micro soft Identity-platform
description: Meer informatie over het maken van een toepassing met één pagina (aanmelden)
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
ms.openlocfilehash: 7bf614a5523e78fc72918db973ef8d738a171fff
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2019
ms.locfileid: "69031785"
---
# <a name="single-page-application---sign-in"></a>Toepassing met één pagina-aanmelden

Meer informatie over het toevoegen van een aanmelding aan de code voor uw toepassing met één pagina.

Voordat u tokens kunt ophalen voor toegang tot Api's in uw toepassing, hebt u een geverifieerde gebruikers context nodig. U kunt op twee manieren gebruikers aan uw toepassing aanmelden in MSAL. js:

* [Meld u aan met een pop-](#sign-in-with-a-pop-up-window) upvenster `loginPopup` met behulp van de methode
* [Meld u aan met](#sign-in-with-redirect) de `loginRedirect` methode Redirect using

U kunt eventueel ook de scopes door geven van de Api's waarvoor u de gebruiker om toestemming moet vragen op het moment dat u zich aanmeldt.

> [!NOTE]
> Als uw toepassing al toegang heeft tot een geverifieerde gebruikers context of id-token, kunt u de aanmeldings stap overs Laan en de tokens rechtstreeks ophalen. Zie [SSO zonder msal. js login](msal-js-sso.md#sso-without-msaljs-login)(Engelstalig) voor meer informatie.

## <a name="choosing-between-a-pop-up-or-redirect-experience"></a>Kiezen tussen een pop-up-of omleidings ervaring

U kunt geen combi natie van zowel de pop-up-als omleidings methoden in uw toepassing gebruiken. De keuze tussen een pop-up-of omleidings ervaring is afhankelijk van uw toepassings stroom.

* Als u niet wilt dat de gebruiker tijdens de verificatie naar de hoofd pagina van de toepassing navigeert, is het raadzaam om de pop-upmethode te gebruiken. Omdat de verificatie omleiding plaatsvindt in een pop-upvenster, blijft de status van de hoofd toepassing behouden.

* Er zijn bepaalde gevallen waarin u mogelijk de omleidings methoden moet gebruiken. Als gebruikers van uw toepassing browser beperkingen of-beleid hebben waarin pop-ups Windows zijn uitgeschakeld, kunt u de omleidings methoden gebruiken. Gebruik de omleidings methoden met Internet Explorer browser omdat er bepaalde [bekende problemen met Internet Explorer optreden](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser) bij het verwerken van pop-upvensters.

## <a name="sign-in-with-a-pop-up-window"></a>Aanmelden met een pop-upvenster

### <a name="javascript"></a>JavaScript

```javascript
const loginRequest = {
    scopes: ["https://graph.microsoft.com/User.ReadWrite"]
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

Met de MSAL-hoek wrapper kunt u specifieke routes in uw toepassing beveiligen door simpelweg `MsalGuard` de definitie aan de route toe te voegen. Deze Guard roept de-methode aan om u aan te melden wanneer de route wordt geopend.

```javascript
// In app.routes.ts
{ path: 'product', component: ProductComponent, canActivate : [MsalGuard],
    children: [
      { path: 'detail/:id', component: ProductDetailComponent  }
    ]
   },
  { path: 'myProfile' ,component: MsGraphComponent, canActivate : [MsalGuard] },
```

Schakel de `popUp` configuratie optie in voor een pop-upvenster. U kunt ook de volgende bereiken door geven die toestemming nodig heeft:

```javascript
//In app.module.ts
@NgModule({
  imports: [ MsalModule.forRoot({
                clientID: 'your_app_id',
                popUp: true,
                consentScopes: ["https://graph.microsoft.com/User.ReadWrite"]
            })]
         })
```

## <a name="sign-in-with-redirect"></a>Aanmelden met omleiding

### <a name="javascript"></a>JavaScript

De omleidings methoden retour neren geen belofte als gevolg van de navigatie weg van de hoofd-app. Als u de geretourneerde tokens wilt verwerken en openen, moet u geslaagde en fout-Call backs registreren voordat u de omleidings methoden aanroept.

```javascript
function authCallback(error, response) {
    //handle redirect response
}

userAgentApplication.handleRedirectCallback(authCallback);

const loginRequest = {
    scopes: ["https://graph.microsoft.com/User.ReadWrite"]
}

userAgentApplication.loginRedirect(loginRequest);
```

### <a name="angular"></a>Angular

De code die hier wordt beschreven, is hetzelfde als hierboven vermeld in de sectie aanmelden met een pop-upvenster. De standaard stroom wordt omgeleid.

> [!NOTE]
> Het ID-token bevat niet de toegestane bereiken en vertegenwoordigt alleen de geverifieerde gebruiker. De toegestane bereiken worden geretourneerd in het toegangs token dat u in de volgende stap gaat ophalen.

## <a name="sign-out"></a>Afmelden

De MSAL-bibliotheek biedt `logout` een methode waarmee de cache in de browser opslag wordt gewist en een afmeldings aanvraag wordt verzonden naar Azure AD. Nadat u zich hebt afgemeld, wordt de standaard instelling voor de start pagina van de toepassing omgeleid.

U kunt de URI zo configureren dat deze wordt omgeleid na afmelden door het `postLogoutRedirectUri`in te stellen. Deze URI moet ook worden geregistreerd als de afmeldings-URI in de registratie van uw toepassing.

### <a name="javascript"></a>JavaScript

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
> [Een token voor de app ophalen](scenario-spa-acquire-token.md)
