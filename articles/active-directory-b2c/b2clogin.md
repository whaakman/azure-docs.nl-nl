---
title: Omleidings-Url's instellen op b2clogin.com-Azure Active Directory B2C | Microsoft Docs
description: Meer informatie over het gebruik van b2clogin.com in de omleidings-Url's voor Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 01/28/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 080c1933f88d9e824969a42212de2eacd0f62e14
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68927287"
---
# <a name="set-redirect-urls-to-b2clogincom-for-azure-active-directory-b2c"></a>Omleidings-Url's instellen op b2clogin.com voor Azure Active Directory B2C

Wanneer u een id-provider voor registratie instelt en u zich aanmeldt in uw Azure Active Directory (Azure AD) B2C-toepassing, moet u een omleidings-URL opgeven. In het verleden werd login.microsoftonline.com gebruikt, nu moet u b2clogin.com gebruiken.

> [!NOTE]
> U kunt Java script-client-side code (momenteel als preview-versie) gebruiken in b2clogin.com. Uw Java script-code wordt verwijderd van uw aangepaste pagina als u login.microsoftonline.com gebruikt. Aanvullende beveiligings beperkingen worden ook toegepast op login.microsoftonline.com, zoals HTML-formulier elementen uit uw aangepaste pagina verwijderen. 

Met b2clogin.com krijgt u extra voor delen, zoals:

- De ruimte die wordt gebruikt in de cookie-header door micro soft-Services is beperkt.
- Uw Url's bevatten niet langer een verwijzing naar micro soft. Bijvoorbeeld `https://your-tenant-name.b2clogin.com/tenant-id/oauth2/authresp`.

> [!NOTE]
> U kunt de Tenant naam en de Tenant-GUID als volgt gebruiken:
> * `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com`(Hiermee wordt nog steeds `onmicrosoft.com`verwezen naar)
> * `https://your-tenant-name.b2clogin.com/your-tenant-guid`(in dit geval is er helemaal geen verwijzing naar micro soft)
>
> U kunt echter geen _aangepast domein_ gebruiken voor uw Azure Active Directory B2C Tenant, bijvoorbeeld `https://your-tenant-name.b2clogin.com/your-custom-domain-name` _niet_ .

Houd rekening met deze instellingen die mogelijk moeten worden gewijzigd wanneer u b2clogin.com gebruikt:

- Stel de omleidings-Url's in uw ID-provider toepassingen in om b2clogin.com te gebruiken. 
- Stel uw Azure AD B2C-toepassing in voor het gebruik van b2clogin.com voor gebruikers stroom verwijzingen en Token-eind punten. 
- Als u MSAL gebruikt, moet u de eigenschap **ValidateAuthority** instellen op `false`.
- Zorg ervoor dat u de **toegestane oorsprong** wijzigt die u hebt gedefinieerd in de CORS-instellingen voor het aanpassen van de [gebruikers interface](active-directory-b2c-ui-customization-custom-dynamic.md).  

## <a name="change-redirect-urls"></a>Omleidings-Url's wijzigen

Als u b2clogin.com wilt gebruiken, zoekt u in de instellingen voor uw ID-provider toepassing naar en wijzigt u de lijst met vertrouwde Url's om terug te sturen naar Azure AD B2C.  Op dit moment hebt u het waarschijnlijk ingesteld om terug te sturen naar een login.microsoftonline.com-site. 

U moet de omleidings-URL wijzigen zodat `your-tenant-name.b2clogin.com` deze wordt geautoriseerd. Zorg ervoor dat u `your-tenant-name` vervangt door de naam van uw Azure AD B2C Tenant en `/te` verwijderen als deze bestaat in de URL. Er zijn een kleine variatie op deze URL voor elke id-provider. Controleer daarom de corresponderende pagina om de exacte URL op te halen.

In de volgende artikelen vindt u informatie over het instellen van id-providers:

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

Uw Azure AD B2C-toepassing verwijst waarschijnlijk `login.microsoftonline.com` naar verschillende locaties, zoals de verwijzingen naar uw gebruikers stroom en Token-eind punten.  Zorg ervoor dat uw autorisatie-eind punt, het eind punt van het token en de uitgever `your-tenant-name.b2clogin.com`zijn bijgewerkt voor gebruik.  

## <a name="set-the-validateauthority-property"></a>De eigenschap ValidateAuthority instellen

Als u MSAL gebruikt, stelt u de eigenschap **ValidateAuthority** in `false`op. Wanneer **ValidateAuthority** is ingesteld op `false`, worden omleidingen toegestaan voor b2clogin.com. 

In het volgende voor beeld ziet u hoe u de eigenschap kunt instellen:

In [MSAL voor .net](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet):

```CSharp
 ConfidentialClientApplication client = new ConfidentialClientApplication(...); // can also be PublicClientApplication
 client.ValidateAuthority = false;
```

En in [MSAL voor Java script](https://github.com/AzureAD/microsoft-authentication-library-for-js):

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
