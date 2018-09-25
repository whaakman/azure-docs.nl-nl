---
title: Met b2clogin.com | Microsoft Docs
description: Meer informatie over het gebruik van b2clogin.com in plaats van login.microsoftonline.com.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/29/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: c24582fce44006d9a3972d73078aa8cb0d212c11
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47053841"
---
# <a name="using-b2clogincom"></a>Met behulp van B2Clogin.com

Voortaan kunt bent we alle klanten stimuleert `<YourDirectoryName>.b2clogin.com` en we niet meer ondersteund `login.microsoftonline.com`. B2Clogin.com biedt extra voordelen, zoals:
* U kunt niet meer dezelfde cookie delen met andere Microsoft-services.
* Kunt u alle verwijzingen naar Microsoft in uw URL verwijderen (u kunt vervangen `<YourDirectoryName>.onmicrosoft.com` met uw directory-ID). Bijvoorbeeld: `https://<YourDirectoryName>.b2clogin.com/tfp/<YourDirectoryID>/<policyname>/v2.0/.well-known/openid-configuration`.

Dit is wat u moet doen als u wilt migreren via op b2clogin.com

* De omleidings-URI's voor uw apps van sociale id-provider wijzigen
* Uw toepassing B2Clogin.com in plaats van bewerken `login.microsoftonline.com` voor beleid verwijst naar en token-eindpunten.
* Als u MSAL gebruikt, moet u instellen `ValidateAuthority=false`.  

##<a name="redirect-uris-for-social-identity-providers"></a>Omleidings-URI's voor sociale id-providers

Als u de id-providers van sociale account instellen in uw directory hebt, moet u moet wijzigingen aanbrengen in hun toepassingen.  Er is een parameter voor de toepassing voor met afzonderlijke sociale providers die bevat een lijst met vertrouwde URL's terug te keren naar Azure AD B2C.  Op dit moment hebt u waarschijnlijk het ingesteld op wordt terug omgeleid naar enkele `login.microsoftonline.com` site, moet u deze URL wijzigen zodat `YourDirectoryName.b2clogin.com` is een geautoriseerde omleidings-URI.  Zorg ervoor dat u de `/te` ook.  Er zijn kleine verschillen naar deze URL voor elke id-provider zodat de bijbehorende pagina om de exacte URL controleren.  

| Identiteitsprovider |
|-------------------|
|[Microsoft-account](active-directory-b2c-setup-msa-app.md)|
|[Facebook](active-directory-b2c-setup-fb-app.md)|
|[Google](active-directory-b2c-setup-goog-app.md)|
|[Amazon](active-directory-b2c-setup-amzn-app.md)|
|[LinkedIn](active-directory-b2c-setup-li-app.md)|
|[Twitter](active-directory-b2c-setup-twitter-app.md)|
|[GitHub](active-directory-b2c-setup-github-app.md)|
|[Weibo](active-directory-b2c-setup-weibo-app.md)|
|[QQ](active-directory-b2c-setup-qq-app.md)|
|[WeChat](active-directory-b2c-setup-wechat-app.md)|
|[Azure AD](active-directory-b2c-setup-oidc-azure-active-directory.md)|
|[Aangepaste OIDC](active-directory-b2c-setup-oidc-idp.md)|

##<a name="update-your-application-references"></a>De referenties van uw toepassing bijwerken

Uw toepassing waarschijnlijk verwijst naar `login.microsoftonline.com` op verschillende plaatsen, zoals uw beleid verwijst naar en token-eindpunten.  Zorg ervoor dat de autorisatie-eindpunt, token-eindpunt en uitgever zijn bijgewerkt.  

##<a name="set-validateauthorityfalse-in-msal"></a>Stel `ValidateAuthority=false` in MSAL

Als u MSAL gebruikt, moet u om in te stellen `ValidateAuthority=false`.  Zie voor meer informatie, [deze documentatie](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.clientapplicationbase?view=azure-dotnet).