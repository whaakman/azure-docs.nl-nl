---
title: Instellen van zich kunnen registreren en aanmelden met een WeChat-account met behulp van Azure Active Directory B2C | Microsoft Docs
description: Meld u aan en meld u bieden aan klanten met een WeChat-account in uw toepassingen met behulp van Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/09/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: e88187c5035abc28ca9deecaf8517e8a21e38d1d
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37952331"
---
# <a name="set-up-sign-up-and-sign-in-with-a-wechat-account-using-azure-active-directory-b2c"></a>Instellen van zich kunnen registreren en aanmelden met een WeChat-account met behulp van Azure Active Directory B2C

> [!NOTE]
> Deze functie is beschikbaar als preview-versie.
> 

## <a name="create-a-wechat-application"></a>Een WeChat-toepassing maken

Als u wilt een WeChat-account gebruiken als een id-provider in Azure Active Directory (Azure AD) B2C, moet u een toepassing maken in de tenant die aangeeft. Als u nog een WeChat-account hebt, krijgt u informatie op [ http://kf.qq.com/faq/161220Brem2Q161220uUjERB.html ](http://kf.qq.com/faq/161220Brem2Q161220uUjERB.html).

### <a name="register-a-wechat-application"></a>Een WeChat-toepassing registreren

1. Aanmelden bij [ https://open.weixin.qq.com/ ](https://open.weixin.qq.com/) met uw WeChat-referenties.
2. Selecteer**管理中心**(center management).
3. Volg de stappen voor het registreren van een nieuwe toepassing.
4. Voer `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp` in**授权回调域**(URL voor terugbellen). Bijvoorbeeld, als uw `tenant_name` is contoso.onmicrosoft.com, de URL die moet worden ingesteld `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.
5. Kopieer de **APP-ID** en **APP-sleutel**. U moet deze de id-provider toevoegen aan uw tenant.

## <a name="configure-wechat-as-an-identity-provider-in-your-tenant"></a>WeChat configureren als een id-provider in uw tenant

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) als globale beheerder van uw Azure AD B2C-tenant.
2. Zorg ervoor dat u de map met uw Azure AD B2C-tenant gebruikt door hiernaar over te schakelen rechtsboven in de Azure Portal. Selecteer de abonnementsgegevens en selecteer vervolgens **Schakelen tussen mappen**. 

    ![Overschakelen naar de Azure AD B2C-tenant](./media/active-directory-b2c-setup-wechat-app/switch-directories.png)

    Kies de map met uw tenant.

    ![Map selecteren](./media/active-directory-b2c-setup-wechat-app/select-directory.png)

3. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
4. Selecteer **id-providers**, en selecteer vervolgens **toevoegen**.
5. Geef een **naam**. Voer bijvoorbeeld *WeChat*.
6. Selecteer **type id-provider**, selecteer **WeChat (Preview)**, en klikt u op **OK**.
7. Selecteer **instellen van deze id-provider** en voert u de APP-ID die u eerder hebt genoteerd als de **Client-ID** en voer de APP-sleutel die u hebt genoteerd als de **clientgeheim** van de WeChat-toepassing die u eerder hebt gemaakt.
8. Klik op **OK** en klik vervolgens op **maken** aan uw WeChat-configuratie op te slaan.

