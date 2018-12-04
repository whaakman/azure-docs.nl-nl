---
title: Instellen van zich kunnen registreren en aanmelden met een WeChat-account met behulp van Azure Active Directory B2C | Microsoft Docs
description: Meld u aan en meld u bieden aan klanten met een WeChat-account in uw toepassingen met behulp van Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 953868f90c4f761b1d02c314e0e1a4e04b8404d9
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52842608"
---
# <a name="set-up-sign-up-and-sign-in-with-a-wechat-account-using-azure-active-directory-b2c"></a>Instellen van zich kunnen registreren en aanmelden met een WeChat-account met behulp van Azure Active Directory B2C

> [!NOTE]
> Deze functie is beschikbaar als preview-versie.
> 

## <a name="create-a-wechat-application"></a>Een WeChat-toepassing maken

Als u wilt een WeChat-account gebruiken als een id-provider in Azure Active Directory (Azure AD) B2C, moet u een toepassing maken in de tenant die aangeeft. Als u nog een WeChat-account hebt, krijgt u informatie op [ https://kf.qq.com/faq/161220Brem2Q161220uUjERB.html ](https://kf.qq.com/faq/161220Brem2Q161220uUjERB.html).

### <a name="register-a-wechat-application"></a>Een WeChat-toepassing registreren

1. Aanmelden bij [ https://open.weixin.qq.com/ ](https://open.weixin.qq.com/) met uw WeChat-referenties.
2. Selecteer**管理中心**(center management).
3. Volg de stappen voor het registreren van een nieuwe toepassing.
4. Voer `https://your-tenant_name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` in**授权回调域**(URL voor terugbellen). Bijvoorbeeld, als de tenantnaam van uw contoso is, de URL die moet worden ingesteld `https://contoso.b2clogin.com/contoso.onmicrosoft.com/oauth2/authresp`.
5. Kopieer de **APP-ID** en **APP-sleutel**. U moet deze de id-provider toevoegen aan uw tenant.

## <a name="configure-wechat-as-an-identity-provider-in-your-tenant"></a>WeChat configureren als een id-provider in uw tenant

1. Meld u als globale beheerder van de Azure AD B2C-tenant aan bij [Azure Portal](https://portal.azure.com/).
2. Zorg ervoor dat u de map met uw Azure AD B2C-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map waarin uw tenant te kiezen.
3. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
4. Selecteer **id-providers**, en selecteer vervolgens **toevoegen**.
5. Geef een **naam**. Voer bijvoorbeeld *WeChat*.
6. Selecteer **type id-provider**, selecteer **WeChat (Preview)**, en klikt u op **OK**.
7. Selecteer **instellen van deze id-provider** en voert u de APP-ID die u eerder hebt genoteerd als de **Client-ID** en voer de APP-sleutel die u hebt genoteerd als de **clientgeheim** van de WeChat-toepassing die u eerder hebt gemaakt.
8. Klik op **OK** en klik vervolgens op **maken** aan uw WeChat-configuratie op te slaan.

