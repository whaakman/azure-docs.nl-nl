---
title: Q-configuratie in Azure Active Directory B2C | Microsoft Docs
description: Geef zich kunnen registreren en aanmelden voor consumenten met q-accounts in uw toepassingen die zijn beveiligd met Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 3/26/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 1f9a0f56158f08dd3b22078f111c9ec6911b726c
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37444426"
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-qq-accounts"></a>Azure Active Directory B2C: Bieden zich kunnen registreren en aanmelden voor consumenten met q-accounts

> [!NOTE]
> Deze functie is beschikbaar als preview-versie.
> 

## <a name="create-a-qq-application"></a>Een q-toepassing maken

Voor het gebruik van q als id-provider in Azure Active Directory (Azure AD) B2C, moet u een q-toepassing maken en geven met de juiste parameters. U moet een q-account om dit te doen. Als u niet hebt, krijgt u één voor één [ https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033 ](https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033).

### <a name="register-for-the-qq-developer-program"></a>Meld u aan voor het Ontwikkelaarsprogramma q

1. Ga naar de [q-ontwikkelaarsportal](http://open.qq.com) en meld u aan met de referenties van uw q-account.
2. Na het aanmelden, gaat u naar [ http://open.qq.com/reg ](http://open.qq.com/reg) naar uzelf te registreren als een ontwikkelaar.
3. Selecteer in het menu**个人**(individuele ontwikkelaars).
4. Voer de vereiste gegevens in het formulier en klik op**下一步**(volgende stap).
5. Voltooi de verificatieprocedure van de e-mailbericht.

> [!NOTE]
> U moet wacht een paar dagen na de registratie als ontwikkelaar worden goedgekeurd. 

### <a name="register-a-qq-application"></a>Een q-toepassing registreren

1. Ga naar [https://connect.qq.com/index.html](https://connect.qq.com/index.html).
2. Klik op**应用管理**(app-beheer).
3. Klik op**创建应用**(app maken).
4. Voer de vereiste app-gegevens.
5. Klik op**创建应用**(app maken).
6. Voer vereiste gegevens in.
7. Voor de**授权回调域**(URL voor terugbellen) Voer `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp`. Bijvoorbeeld, als uw `tenant_name` is contoso.onmicrosoft.com, de URL die moet worden ingesteld `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.
8. Klik op**创建应用**(app maken).
9. Klik op de bevestigingspagina op**应用管理**(app management) om terug te keren naar de pagina app-beheer.
10. Klik op**查看**(weergeven) naast de app die u zojuist hebt gemaakt.
11. Klik op**修改**(bewerken).
12. Kopieer vanaf de bovenkant van de pagina, de **APP-ID** en **APP-sleutel**.

## <a name="configure-qq-as-an-identity-provider-in-your-tenant"></a>Q configureren als een id-provider in uw tenant
1. Volg deze stappen om [gaat u naar de blade B2C-functies](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) in Azure portal.
2. Klik op de blade B2C-functies **id-providers**.
3. Klik op **+Toevoegen** boven aan de blade.
4. Geef een beschrijvende **naam** voor de configuratie van de id-provider. Voer bijvoorbeeld 'Q'.
5. Klik op **type id-provider**, selecteer **q**, en klikt u op **OK**.
6. Klik op **deze id-provider instellen**
7. Voer de **App-sleutel** die u eerder hebt gekopieerd als de **Client-ID**.
8. Voer de **Appgeheim** die u eerder hebt gekopieerd als de **Clientgeheim**.
9. Klik op **OK** en klik vervolgens op **maken** aan uw q-configuratie op te slaan.

