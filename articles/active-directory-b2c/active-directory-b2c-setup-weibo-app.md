---
title: 'Azure Active Directory B2C: Weibo configuratie | Microsoft Docs'
description: Registreren en aanmelden gebruikers met een account in uw toepassingen die zijn beveiligd met Azure Active Directory B2C Weibo bieden.
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: mtillman
editor: parakhj
ms.assetid: 1860de34-94cb-4ceb-851e-102f930f7230
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/26/2017
ms.author: parakhj
ms.openlocfilehash: 320d7c57035d3b3ecdb4e03d80141a58cf429461
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-weibo-accounts"></a>Azure Active Directory B2C: Zich kunnen registreren en aanmelden gebruikers bieden met Weibo-accounts

> [!NOTE]
> Deze functie is in preview. Gebruik deze id-provider niet in uw productieomgeving.
> 

## <a name="create-a-weibo-application"></a>Een toepassing Weibo maken

Als u wilt gebruiken als een id-provider in Azure Active Directory (Azure AD) B2C Weibo gebruikt, moet u een Weibo-toepassing maken en geeft deze met de juiste parameters. U moet een account Weibo om dit te doen. Als u niet hebt, kunt u één voor één [http://weibo.com/signup/signup.php?lang=en-us](http://weibo.com/signup/signup.php?lang=en-us).

### <a name="register-for-the-weibo-developer-program"></a>Registreren voor de Ontwikkelaarsprogramma Weibo

1. Ga naar de [Weibo ontwikkelaarsportal](http://open.weibo.com/) en meld u aan met de referenties van uw Weibo-account.
2. Na het aanmelden, klik op de weergavenaam van uw in de rechterbovenhoek.
3. Selecteer in de vervolgkeuzelijst**编辑开发者信息**(informatie voor ontwikkelaars bewerken).
4. Voer de vereiste gegevens in het formulier en klik op**提交**(verzenden).
5. Voltooi de verificatieprocedure van de e-mailbericht.
6. Ga naar de [identiteitverificatie pagina](http://open.weibo.com/developers/identity/edit).
7. Voer de vereiste gegevens in het formulier en klik op**提交**(verzenden).

### <a name="register-a-weibo-application"></a>Een toepassing Weibo registreren

1. Ga naar de [app de registratiepagina van nieuwe Weibo](http://open.weibo.com/apps/new).
2. Geef informatie over de benodigde toepassing.
3. Klik op**创建**(maken).
4. Kopieer de waarden van **App-sleutel** en **App geheim**. U nodig deze later.
5. De vereiste foto's uploaden en voer de benodigde informatie.
6. Klik op**保存以上信息**(opslaan).
7. Klik op**高级信息**(Geavanceerd informatie).
8. Klik op**编辑**(bewerken) naast het veld voor OAuth2.0**授权设置**(Omleidings-URL).
9. Voer `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp` voor OAuth2.0**授权设置**(Omleidings-URL). Bijvoorbeeld, als uw `tenant_name` is contoso.onmicrosoft.com, de URL moet worden ingesteld `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.
10. Klik op**提交**(verzenden).  

## <a name="configure-weibo-as-an-identity-provider-in-your-tenant"></a>Weibo configureren als een id-provider in uw tenant
1. Volg deze stappen voor [gaat u naar de blade B2C-functies](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) in de Azure portal.
2. Klik op de blade B2C-functies op **identiteitsproviders**.
3. Klik op **+Toevoegen** boven aan de blade.
4. Geef een beschrijvende **naam** voor de configuratie van de id-provider. Voer bijvoorbeeld 'Weibo'.
5. Klik op **identiteit providertype**, selecteer **Weibo**, en klik op **OK**.
6. Klik op **instellen van deze id-provider**
7. Voer de **App-sleutel** die u eerder hebt gekopieerd als het **Client-ID**.
8. Voer de **App geheim** die u eerder hebt gekopieerd als het **Clientgeheim**.
9. Klik op **OK** en klik vervolgens op **maken** naar uw Weibo-configuratie op te slaan.

