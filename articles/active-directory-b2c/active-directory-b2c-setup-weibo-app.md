---
title: Weibo configuratie in Azure Active Directory B2C | Microsoft Docs
description: Meld u aan en meld u bieden voor gebruikers met een Weibo account in uw toepassingen die zijn beveiligd met Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 3/26/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: bfd7dde290bd040f8457e6d095fdf896e802764b
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37444786"
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-weibo-accounts"></a>Azure Active Directory B2C: Bieden zich kunnen registreren en aanmelden voor consumenten met Weibo-accounts

> [!NOTE]
> Deze functie is beschikbaar als preview-versie. Gebruik deze id-provider niet in uw productieomgeving.
> 

## <a name="create-a-weibo-application"></a>Een Weibo-toepassing maken

Voor het gebruik van Weibo als id-provider in Azure Active Directory (Azure AD) B2C, moet u een Weibo-toepassing maken en geven met de juiste parameters. U moet een account Weibo om dit te doen. Als u niet hebt, krijgt u één voor één [ http://weibo.com/signup/signup.php?lang=en-us ](http://weibo.com/signup/signup.php?lang=en-us).

### <a name="register-for-the-weibo-developer-program"></a>Meld u aan voor het Ontwikkelaarsprogramma Weibo

1. Ga naar de [Weibo ontwikkelaarsportal](http://open.weibo.com/) en meld u aan met de referenties van uw Weibo-account.
2. Na het aanmelden, klik op de weergavenaam van uw in de rechterbovenhoek.
3. Selecteer in de vervolgkeuzelijst**编辑开发者信息**(informatie voor ontwikkelaars bewerken).
4. Voer de vereiste gegevens in het formulier en klik op**提交**(verzenden).
5. Voltooi de verificatieprocedure van de e-mailbericht.
6. Ga naar de [identiteitverificatie pagina](http://open.weibo.com/developers/identity/edit).
7. Voer de vereiste gegevens in het formulier en klik op**提交**(verzenden).

### <a name="register-a-weibo-application"></a>Een toepassing Weibo registreren

1. Ga naar de [nieuwe pagina voor de app registratie Weibo](http://open.weibo.com/apps/new).
2. Voer informatie over de benodigde toepassing.
3. Klik op**创建**(maken).
4. Kopieer de waarden van **App-sleutel** en **Appgeheim**. U moet dit later opnieuw.
5. De vereiste foto's uploaden en voer de benodigde gegevens.
6. Klik op**保存以上信息**(opslaan).
7. Klik op**高级信息**(geavanceerde informatie over).
8. Klik op**编辑**(bewerken) naast het veld voor OAuth 2.0**授权设置**(Omleidings-URL).
9. Voer `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp` voor OAuth 2.0**授权设置**(Omleidings-URL). Bijvoorbeeld, als uw `tenant_name` is contoso.onmicrosoft.com, de URL die moet worden ingesteld `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.
10. Klik op**提交**(verzenden).  

## <a name="configure-weibo-as-an-identity-provider-in-your-tenant"></a>Weibo configureren als een id-provider in uw tenant
1. Volg deze stappen om [gaat u naar de blade B2C-functies](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) in Azure portal.
2. Klik op de blade B2C-functies **id-providers**.
3. Klik op **+Toevoegen** boven aan de blade.
4. Geef een beschrijvende **naam** voor de configuratie van de id-provider. Voer bijvoorbeeld 'Weibo'.
5. Klik op **type id-provider**, selecteer **Weibo**, en klikt u op **OK**.
6. Klik op **deze id-provider instellen**
7. Voer de **App-sleutel** die u eerder hebt gekopieerd als de **Client-ID**.
8. Voer de **Appgeheim** die u eerder hebt gekopieerd als de **Clientgeheim**.
9. Klik op **OK** en klik vervolgens op **maken** aan uw Weibo-configuratie op te slaan.

