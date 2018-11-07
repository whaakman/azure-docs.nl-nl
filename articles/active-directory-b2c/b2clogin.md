---
title: Set Omleidings-URL's op b2clogin.com voor Azure Active Directory B2C | Microsoft Docs
description: Meer informatie over het gebruik van b2clogin.com in uw Omleidings-URL's voor Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 10/22/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 00fde1145b038f3bf82e3dbc5e1dc967e1865ad6
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51234917"
---
# <a name="set-redirect-urls-to-b2clogincom-for-azure-active-directory-b2c"></a>Set Omleidings-URL's op b2clogin.com voor Azure Active Directory B2C

Bij het instellen van een id-provider voor registratie en aanmelding in uw Azure Active Directory (Azure AD) B2C-toepassing, moet u een Omleidings-URL opgeven. Login.microsoftonline.com is in het verleden gebruikt, nu u b2clogin.com moet worden gebruikt.

Met b2clogin.com biedt extra voordelen, zoals:

- Cookies worden niet langer worden gedeeld met andere Microsoft-services.
- De URL's zijn niet langer een verwijzing naar Microsoft. Bijvoorbeeld `https://your-tenant-name.b2clogin.com/tfp/your-tenant-ID/policyname/v2.0/.well-known/openid-configuration`.

Houd rekening met deze instellingen die nodig hebt mogelijk bij het gebruik van b2clogin.com wijzigen:

- Instellen van de omleidings-URL's in uw toepassingen identity provider b2clogin.com gebruiken. 
- Uw Azure AD B2C-toepassing met b2clogin.com voor beleid verwijst naar en token-eindpunten instellen. 
- Als u MSAL gebruikt, moet u instellen de **ValidateAuthority** eigenschap `false`.
- Zorg ervoor dat u een wijzigen **oorsprongen toegestaan** die u hebt gedefinieerd in de CORS-instellingen voor [aanpassingen van de gebruiker-gebruikersinterface](active-directory-b2c-ui-customization-custom-dynamic.md).  

## <a name="change-redirect-urls"></a>Wijziging Omleidings-URL's

Voor het gebruik van b2clogin.com, in de instellingen voor de toepassing van uw id-provider, zoekt en wijzigen van de lijst met vertrouwde URL's terug te keren naar Azure AD B2C.  Op dit moment hebt u waarschijnlijk het ingesteld op wordt terug omgeleid naar de site van sommige login.microsoftonline.com. 

U moet de omleidings-URL wijzigen zodat `your-tenant-name.b2clogin.com` is geautoriseerd. Vervang `your-tenant-name` met de naam van uw Azure AD B2C-tenant en verwijder `/te` als deze in de URL bestaat. Er zijn kleine verschillen naar deze URL voor elke id-provider zodat de bijbehorende pagina om de exacte URL controleren.

U kunt configuratie-informatie voor id-providers vinden in de volgende artikelen:

- [Microsoft-account](active-directory-b2c-setup-msa-app.md)
- [Facebook](active-directory-b2c-setup-fb-app.md)
- [Google](active-directory-b2c-setup-goog-app.md)
- [Amazon](active-directory-b2c-setup-amzn-app.md)
- [LinkedIn](active-directory-b2c-setup-li-app.md)
- [Twitter](active-directory-b2c-setup-twitter-app.md)
- [GitHub](active-directory-b2c-setup-github-app.md)
- [Weibo](active-directory-b2c-setup-weibo-app.md)
- [QQ](active-directory-b2c-setup-qq-app.md)
- [WeChat](active-directory-b2c-setup-wechat-app.md)
- [Azure AD](active-directory-b2c-setup-oidc-azure-active-directory.md)
- [Aangepaste OIDC](active-directory-b2c-setup-oidc-idp.md)

## <a name="update-your-application"></a>Uw toepassing bijwerken

Uw Azure AD B2C-toepassing waarschijnlijk verwijst naar `login.microsoftonline.com` op verschillende plaatsen, zoals uw beleid verwijst naar en token-eindpunten.  Zorg ervoor dat de autorisatie-eindpunt, token-eindpunt en uitgever zijn bijgewerkt voor het gebruik van `your-tenant-name.b2clogin.com`.  

## <a name="set-the-validateauthority-property"></a>Stel de eigenschap ValidateAuthority

Als u MSAL, stelt u de **ValidateAuthority** naar `false`. Het volgende voorbeeld ziet u hoe u de eigenschap kunt instellen:

In [MSAL voor .net](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet):

```CSharp
 ConfidentialClientApplication client = new ConfidentialClientApplication(...); // can also be PublicClientApplication
 client.ValidateAuthority = false;
```

En in [MSAL voor Javascript](https://github.com/AzureAD/microsoft-authentication-library-for-js):

```Javascript
this.clientApplication = new UserAgentApplication(
  env.auth.clientId,
  env.auth.loginAuthority,
  this.authCallback.bind(this),
  {
    validateAuthority: false
  }
);
```
