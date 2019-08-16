---
title: Omleidings-Url's instellen op b2clogin.com-Azure Active Directory B2C
description: Meer informatie over het gebruik van b2clogin.com in de omleidings-Url's voor Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/17/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: dbc366daac89f44d4b084081590124f81ff9cc9c
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2019
ms.locfileid: "69533741"
---
# <a name="set-redirect-urls-to-b2clogincom-for-azure-active-directory-b2c"></a>Omleidings-Url's instellen op b2clogin.com voor Azure Active Directory B2C

Wanneer u een id-provider voor registratie instelt en u zich aanmeldt in uw Azure Active Directory B2C-toepassing (Azure AD B2C), moet u een omleidings-URL opgeven. U moet niet langer verwijzen naar *login.microsoftonline.com* in uw toepassingen en api's. Gebruik in plaats daarvan *b2clogin.com* voor alle nieuwe toepassingen en migreer bestaande toepassingen van *login.microsoftonline.com* naar *b2clogin.com*.

## <a name="benefits-of-b2clogincom"></a>Voor delen van b2clogin.com

Wanneer u *b2clogin.com* als omleidings-URL gebruikt:

* De ruimte die wordt gebruikt in de cookie-header door micro soft-Services is beperkt.
* De omleidings-Url's hoeven niet langer een verwijzing naar micro soft te bevatten.
* Java script-client-side-code wordt ondersteund (momenteel in [Preview](user-flow-javascript-overview.md)) op aangepaste pagina's. Vanwege beveiligings beperkingen worden java script-code en HTML-formulier elementen verwijderd uit aangepaste pagina's als u *login.microsoftonline.com*gebruikt.

## <a name="overview-of-required-changes"></a>Overzicht van vereiste wijzigingen

Er zijn verschillende wijzigingen die u mogelijk moet maken om uw toepassingen te migreren naar *b2clogin.com*:

* Wijzig de omleidings-URL in de toepassingen van uw identiteits provider om naar *b2clogin.com*te verwijzen.
* Werk uw Azure AD B2C-toepassingen bij om *b2clogin.com* in hun gebruikers stroom en Token-eindpunt verwijzingen te gebruiken.
* Werk alle **toegestane oorsprongen** bij die u hebt gedefinieerd in de CORS-instellingen voor het aanpassen van de [gebruikers interface](active-directory-b2c-ui-customization-custom-dynamic.md).

## <a name="change-identity-provider-redirect-urls"></a>Omleidings-Url's voor de ID-provider wijzigen

Wijzig op elke website van de identiteits provider waar u een toepassing hebt gemaakt, alle vertrouwde url's om om `your-tenant-name.b2clogin.com` te leiden in plaats van *login.microsoftonline.com*.

Er zijn twee indelingen die u kunt gebruiken voor uw b2clogin.com-omleidings-Url's. De eerste biedt het voor deel dat ' micro soft ' nergens in de URL wordt weer gegeven met behulp van de Tenant-ID (een GUID) in plaats van de domein naam van de Tenant:

```
https://{your-tenant-name}.b2clogin.com/{your-tenant-id}/oauth2/authresp
```

De tweede optie maakt gebruik van de domein naam van uw Tenant `your-tenant-name.onmicrosoft.com`in de vorm van. Bijvoorbeeld:

```
https://{your-tenant-name}.b2clogin.com/{your-tenant-name}.onmicrosoft.com/oauth2/authresp
```

Voor beide indelingen:

* Vervang `{your-tenant-name}` met de naam van uw Azure AD B2C-tenant.
* Verwijderen `/te` als deze bestaat in de URL.

## <a name="update-your-applications-and-apis"></a>Uw toepassingen en Api's bijwerken

De code in uw Azure AD B2C-toepassingen en api's kunnen op verschillende locaties `login.microsoftonline.com` worden gebruikt. Uw code kan bijvoorbeeld verwijzingen bevatten naar gebruikers stromen en Token-eind punten. Werk het volgende bij in plaats `your-tenant-name.b2clogin.com`daarvan:

* Autorisatie-eindpunt
* Tokeneindpunt
* Uitgever van het token

Zo zou het CA-eind punt voor het aanmeldings-en aanmeldings beleid van Contoso nu het volgende zijn:

```
https://contosob2c.b2clogin.com/00000000-0000-0000-0000-000000000000/B2C_1_signupsignin1
```

## <a name="microsoft-authentication-library-msal"></a>Microsoft Authentication Library (MSAL)

### <a name="validateauthority-property"></a>Eigenschap ValidateAuthority

Als u [MSAL.net][msal-dotnet] v2 of eerder gebruikt, stelt u de eigenschap `false` ValidateAuthority in op client instantiëren om omleidingen naar *b2clogin.com*toe te staan. Deze instelling is niet vereist voor MSAL.NET v3 en hoger.

```CSharp
ConfidentialClientApplication client = new ConfidentialClientApplication(...); // Can also be PublicClientApplication
client.ValidateAuthority = false; // MSAL.NET v2 and earlier **ONLY**
```

Als u [MSAL voor Java script][msal-js]gebruikt:

```JavaScript
this.clientApplication = new UserAgentApplication(
  env.auth.clientId,
  env.auth.loginAuthority,
  this.authCallback.bind(this),
  {
    validateAuthority: false
  }
);
```

<!-- LINKS - External -->
[msal-dotnet]: https://github.com/AzureAD/microsoft-authentication-library-for-dotnet
[msal-dotnet-b2c]: https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/AAD-B2C-specifics
[msal-js]: https://github.com/AzureAD/microsoft-authentication-library-for-js
[msal-js-b2c]: ../active-directory/develop/msal-b2c-overview.md