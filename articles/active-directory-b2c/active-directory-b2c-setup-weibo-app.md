---
title: Instellen van zich kunnen registreren en aanmelden met een Weibo-account met behulp van Azure Active Directory B2C | Microsoft Docs
description: Meld u aan en meld u bieden aan klanten met een Weibo account in uw toepassingen met behulp van Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/09/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 2534e3e39d4a6dd1659dced5a1b0342798c0049e
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38969126"
---
# <a name="set-up-sign-up-and-sign-in-with-a-weibo-account-using-azure-active-directory-b2c"></a>Instellen van zich kunnen registreren en aanmelden met een Weibo-account met behulp van Azure Active Directory B2C

> [!NOTE]
> Deze functie is beschikbaar als preview-versie.
> 

## <a name="create-a-weibo-application"></a>Een Weibo-toepassing maken

Als u wilt een Weibo-account gebruiken als een id-provider in Azure Active Directory (Azure AD) B2C, moet u een toepassing maken in de tenant die aangeeft. Als u nog een Weibo-account hebt, kunt u krijgen via [ http://weibo.com/signup/signup.php?lang=en-us ](http://weibo.com/signup/signup.php?lang=en-us).

1. Aanmelden bij de [Weibo ontwikkelaarsportal](http://open.weibo.com/) met de referenties van uw Weibo-account.
2. Na de aanmelding, selecteert u uw weergavenaam in de rechterbovenhoek.
3. Selecteer in de vervolgkeuzelijst**编辑开发者信息**(informatie voor ontwikkelaars bewerken).
4. Voer de vereiste gegevens in en selecteer**提交**(verzenden).
5. Voltooi de verificatieprocedure van de e-mailbericht.
6. Ga naar de [identiteitverificatie pagina](http://open.weibo.com/developers/identity/edit).
7. Voer de vereiste gegevens in en selecteer**提交**(verzenden).

### <a name="register-a-weibo-application"></a>Een toepassing Weibo registreren

1. Ga naar de [nieuwe pagina voor de app registratie Weibo](http://open.weibo.com/apps/new).
2. Voer informatie over de benodigde toepassing.
3. Selecteer**创建**(maken).
4. Kopieer de waarden van **App-sleutel** en **Appgeheim**. U moet deze de id-provider toevoegen aan uw tenant.
5. De vereiste foto's uploaden en voer de benodigde gegevens.
6. Selecteer**保存以上信息**(opslaan).
7. Selecteer**高级信息**(geavanceerde informatie over).
8. Selecteer**编辑**(bewerken) naast het veld voor OAuth 2.0**授权设置**(Omleidings-URL).
9. Voer `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp` voor OAuth 2.0**授权设置**(Omleidings-URL). Bijvoorbeeld, als uw `tenant_name` is contoso.onmicrosoft.com, de URL die moet worden ingesteld `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.
10. Selecteer**提交**(verzenden).  

## <a name="configure-a-weibo-account-as-an-identity-provider"></a>Een account Weibo configureren als een id-provider

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) als globale beheerder van uw Azure AD B2C-tenant.
2. Zorg ervoor dat u de map met uw Azure AD B2C-tenant gebruikt door hiernaar over te schakelen rechtsboven in de Azure Portal. Selecteer de abonnementsgegevens en selecteer vervolgens **Schakelen tussen mappen**. 

    ![Overschakelen naar de Azure AD B2C-tenant](./media/active-directory-b2c-setup-weibo-app/switch-directories.png)

    Kies de map met uw tenant.

    ![Map selecteren](./media/active-directory-b2c-setup-weibo-app/select-directory.png)

3. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
4. Selecteer **id-providers**, en selecteer vervolgens **toevoegen**.
5. Geef een **naam**. Voer bijvoorbeeld *Weibo*.
6. Selecteer **type id-provider**, selecteer **Weibo (Preview)**, en klikt u op **OK**.
7. Selecteer **instellen van deze id-provider** en voer de App-sleutel die u eerder hebt genoteerd als de **Client-ID** en voert u de App-geheim die u hebt genoteerd als de **clientgeheim** van de Weibo-toepassing die u eerder hebt gemaakt.
8. Klik op **OK** en klik vervolgens op **maken** aan uw Weibo-configuratie op te slaan.
