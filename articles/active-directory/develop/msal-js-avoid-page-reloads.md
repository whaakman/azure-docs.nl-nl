---
title: Vermijd van pagina laadt (Microsoft Authentication Library voor JavaScript) | Azure
description: Informatie over het voorkomen van pagina laadt te verkrijgen en vernieuwd tokens op de achtergrond met behulp van de Microsoft Authentication Library voor JavaScript (MSAL.js).
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
ms.openlocfilehash: 162811221e6dde89ad11f358b2ec8f32f3c82522
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66420466"
---
# <a name="avoid-page-reloads-when-acquiring-and-renewing-tokens-silently-using-msaljs"></a>Pagina laadt te verkrijgen en vernieuwd tokens op de achtergrond met behulp van MSAL.js voorkomen
Microsoft Authentication Library voor JavaScript (MSAL.js) maakt gebruik van verborgen `iframe` elementen voor het verkrijgen en tokens op de achtergrond op de achtergrond vernieuwen. Azure AD stuurt het token terug naar de geregistreerde redirect_uri opgegeven in het token aan te vragen (standaard is deze pagina van de hoofdmap van de app). Omdat het antwoord een 302 is, dit resulteert in de HTML-code die overeenkomt met de `redirect_uri` geladen de `iframe`. Meestal van de app `redirect_uri` is de pagina hoofdmap en dit ervoor zorgt dat deze opnieuw te laden.

In andere gevallen, als te navigeren naar pagina van de hoofdmap van de app moet worden geverifieerd, dit kan leiden tot geneste `iframe` elementen of `X-Frame-Options: deny` fout.

Aangezien MSAL.js de 302 dat is uitgegeven door Azure AD kan niet worden verwijderd en vereist voor het proces van het geretourneerde token is, niet het voorkomen dat de `redirect_uri` van geladen de `iframe`.

Om te voorkomen dat de hele app opnieuw laden of andere fouten als gevolg hiervan, volgt u deze oplossingen.

## <a name="specify-different-html-for-the-iframe"></a>Geef verschillende HTML-code voor het iframe

Stel de `redirect_uri` eigenschap op configuratie naar een eenvoudige pagina, waarvoor geen verificatie vereist is. U hebt om ervoor te zorgen dat deze met overeenkomt de `redirect_uri` geregistreerd in Azure portal. Dit heeft geen invloed op de ervaring van gebruiker aanmelden als de startpagina van MSAL worden opgeslagen wanneer de gebruiker begint de aanmeldingsprocedure en wordt omgeleid naar de exacte locatie nadat de aanmelding is voltooid.

## <a name="initialization-in-your-main-app-file"></a>Initialisatie in uw belangrijkste app-bestand

Als uw app is gestructureerd zodat er één, centrale Javascript-bestand dat de initialisatie van de app is, Routering en andere dingen definieert, kunt u voorwaardelijk laden uw app-modules op basis van of de app wordt geladen een `iframe` of niet. Bijvoorbeeld:

In de AngularJS: app.js

```javascript
// Check that the window is an iframe and not popup
if (window !== window.parent && !window.opener) {
angular.module('todoApp', ['ui.router', 'MsalAngular'])
    .config(['$httpProvider', 'msalAuthenticationServiceProvider','$locationProvider', function ($httpProvider, msalProvider,$locationProvider) {
        msalProvider.init(
            // msal configuration
        );

        $locationProvider.html5Mode(false).hashPrefix('');
    }]);
}
else {
    angular.module('todoApp', ['ui.router', 'MsalAngular'])
        .config(['$stateProvider', '$httpProvider', 'msalAuthenticationServiceProvider', '$locationProvider', function ($stateProvider, $httpProvider, msalProvider, $locationProvider) {
            $stateProvider.state("Home", {
                url: '/Home',
                controller: "homeCtrl",
                templateUrl: "/App/Views/Home.html",
            }).state("TodoList", {
                url: '/TodoList',
                controller: "todoListCtrl",
                templateUrl: "/App/Views/TodoList.html",
                requireLogin: true
            })

            $locationProvider.html5Mode(false).hashPrefix('');

            msalProvider.init(
                // msal configuration
            );
        }]);
}
```

In Angular: app.module.ts

```javascript
// Imports...
@NgModule({
  declarations: [
    AppComponent,
    MsalComponent,
    MainMenuComponent,
    AccountMenuComponent,
    OsNavComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    HttpClientModule,
    ServiceWorkerModule.register('ngsw-worker.js', { enabled: environment.production }),
    MsalModule.forRoot(environment.MsalConfig),
    SuiModule,
    PagesModule
  ],
  providers: [
    HttpServiceHelper,
    {provide: HTTP_INTERCEPTORS, useClass: MsalInterceptor, multi: true},
    AuthService
  ],
  entryComponents: [
    AppComponent,
    MsalComponent
  ]
})
export class AppModule {
  constructor() {
    console.log('APP Module Constructor!');
  }

  ngDoBootstrap(ref: ApplicationRef) {
    if (window !== window.parent && !window.opener)
    {
      console.log("Bootstrap: MSAL");
      ref.bootstrap(MsalComponent);
    }
    else
    {
    //this.router.resetConfig(RouterModule);
      console.log("Bootstrap: App");
      ref.bootstrap(AppComponent);
    }
  }
}
```

MsalComponent:

```javascript
import { Component} from '@angular/core';
import { MsalService } from '@azure/msal-angular';

// This component is used only to avoid Angular reload
// when doing acquireTokenSilent()

@Component({
  selector: 'app-root',
  template: '',
})
export class MsalComponent {
  constructor(private Msal: MsalService) {
  }
}
```

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [het bouwen van een toepassing met één pagina (SPA)](scenario-spa-overview.md) MSAL.js te gebruiken.