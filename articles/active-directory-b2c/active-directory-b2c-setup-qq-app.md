---
title: Instellen van zich kunnen registreren en aanmelden met een q-account met behulp van Azure Active Directory B2C | Microsoft Docs
description: Meld u aan en meld u aan klanten met accounts in uw toepassingen met behulp van Azure Active Directory B2C q bieden.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 7abe86d49ec62460f4bfe039cbd935efe21caba8
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "44716329"
---
# <a name="set-up-sign-up-and-sign-in-with-a-qq-account-using-azure-active-directory-b2c"></a>Instellen van zich kunnen registreren en aanmelden met een q-account met behulp van Azure Active Directory B2C

> [!NOTE]
> Deze functie is beschikbaar als preview-versie.
> 

## <a name="create-a-qq-application"></a>Een q-toepassing maken

Voor het gebruik van een q-account als id-provider in Azure Active Directory (Azure AD) B2C, moet u een toepassing maken in de tenant die aangeeft. Als u nog een q-account hebt, kunt u krijgen via [ https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033 ](https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033).

### <a name="register-for-the-qq-developer-program"></a>Meld u aan voor het Ontwikkelaarsprogramma q

1. Aanmelden bij de [q-ontwikkelaarsportal](http://open.qq.com) met de referenties van uw q-account.
2. Na het aanmelden, gaat u naar [ http://open.qq.com/reg ](http://open.qq.com/reg) naar uzelf te registreren als een ontwikkelaar.
3. Selecteer**个人**(individuele ontwikkelaars).
4. Voer de vereiste gegevens in en selecteer**下一步**(volgende stap).
5. Voltooi de verificatieprocedure van de e-mailbericht. U moet wacht een paar dagen na de registratie als ontwikkelaar worden goedgekeurd. 

### <a name="register-a-qq-application"></a>Een q-toepassing registreren

1. Ga naar [https://connect.qq.com/index.html](https://connect.qq.com/index.html).
2. Selecteer**应用管理**(app-beheer).
5. Selecteer**创建应用**(app maken) en voer de vereiste informatie.
7. Voer `https://your-tenant-name.b2clogin.com/your-tenant-name}.onmicrosoft.com/oauth2/authresp` in**授权回调域**(URL voor terugbellen). Bijvoorbeeld, als uw `tenant_name` is contoso, de URL die moet worden ingesteld `https://contoso.b2clogin.com/contoso.onmicrosoft.com/oauth2/authresp`.
8. Selecteer**创建应用**(app maken).
9. Selecteer op de bevestigingspagina**应用管理**(app management) om terug te keren naar de pagina app-beheer.
10. Selecteer**查看**(weergeven) naast de app die u hebt gemaakt.
11. Selecteer**修改**(bewerken).
12. Kopieer de **APP-ID** en **APP-sleutel**. U moet beide van deze waarden in de id-provider toevoegen aan uw tenant.

## <a name="configure-qq-as-an-identity-provider"></a>Q configureren als een id-provider

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) als globale beheerder van uw Azure AD B2C-tenant.
2. Zorg ervoor dat u de map met uw Azure AD B2C-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map waarin uw tenant te kiezen.  

    ![Overschakelen naar de Azure AD B2C-tenant](./media/active-directory-b2c-setup-qq-app/switch-directories.png)

3. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
4. Selecteer **id-providers**, en selecteer vervolgens **toevoegen**.
5. Geef een **naam**. Voer bijvoorbeeld *q*.
6. Selecteer **type id-provider**, selecteer **q (Preview)**, en klikt u op **OK**.
7. Selecteer **instellen van deze id-provider** en voert u de APP-ID die u eerder hebt genoteerd als de **Client-ID** en voer de APP-sleutel die u hebt genoteerd als de **clientgeheim** van de q de toepassing die u eerder hebt gemaakt.
8. Klik op **OK** en klik vervolgens op **maken** aan uw q-configuratie op te slaan.

