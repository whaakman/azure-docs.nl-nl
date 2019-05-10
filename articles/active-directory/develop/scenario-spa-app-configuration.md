---
title: Toepassing met één pagina (de configuratie van app-code) - Microsoft identity-platform
description: Informatie over het bouwen van een toepassing met één pagina (configuratie van de App-code)
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
ms.openlocfilehash: b71454fc553a0f81c26426a6a9588f15d5311e38
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65406423"
---
# <a name="single-page-application---code-configuration"></a>Toepassing van één pagina - code configureren

Informatie over het configureren van de code voor uw toepassing met één pagina (SPA).

## <a name="msal-libraries-supporting-implicit-flow"></a>MSAL bibliotheken ondersteunende de impliciete stroom

Microsoft identity-platform biedt MSAL.js-bibliotheek voor de ondersteuning van de impliciete stroom met behulp van de branche aanbevolen beveiligingsprocedures.  

De bibliotheken ondersteunende de impliciete stroom zijn:

| Bibliotheek met MSAL | Description |
|--------------|--------------|
| ![MSAL.js](media/sample-v2-code/logo_js.png) <br/> [MSAL.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)  | Plain JavaScript-bibliotheek voor gebruik in een client side web-app gebouwd met behulp van JavaScript- of beveiligd-WACHTWOORDVERIFICATIE frameworks zoals Angular, Vue.js, React.js, enzovoort. |
| ![MSAL Angular](media/sample-v2-code/logo_angular.png) <br/> [MSAL Angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) | -Wrapper van de kernbibliotheek MSAL.js te gebruiken in apps met één pagina die zijn gebouwd met de Angular framework vereenvoudigen. Deze bibliotheek is in preview en is [bekende problemen](https://github.com/AzureAD/microsoft-authentication-library-for-js/issues?q=is%3Aopen+is%3Aissue+label%3Aangular) met bepaalde versies van Angular en browsers. |

## <a name="application-code-configuration"></a>Toepassingsconfiguratie-code

De registratiegegevens van de toepassing wordt in de bibliotheek met MSAL doorgegeven als configuratie tijdens de initialisatie van de bibliotheek.

### <a name="javascript"></a>JavaScript

```javascript
// Configuration object constructed.
const config = {
    auth: {
        clientId: 'your_app_id',
        redirectUri: "your_app_redirect_uri" //defaults to application start page
    }
}

// create UserAgentApplication instance
const userAgentApplication = new UserAgentApplication(config);
```
Zie voor meer informatie over de beschikbare configuratieopties [tijdens de initialisatie van toepassingen met MSAL.js](msal-js-initializing-client-applications.md).

### <a name="angular"></a>Angular

```javascript
//In app.module.ts
@NgModule({
  imports: [ MsalModule.forRoot({
                clientId: 'your_app_id'
            })]
         })

  export class AppModule { }
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Aanmelden en afmelden](scenario-spa-sign-in.md)
