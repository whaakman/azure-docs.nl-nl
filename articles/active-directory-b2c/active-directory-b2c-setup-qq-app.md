---
title: 'Azure Active Directory B2C: Q configuratie | Microsoft Docs'
description: Registreren en aanmelden gebruikers met q-accounts in uw toepassingen die zijn beveiligd met Azure Active Directory B2C bieden.
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: mtillman
editor: parakhj
ms.assetid: 18c2cf94-8004-4de1-81c2-e45be65ce12d
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/26/2017
ms.author: parakhj
ms.openlocfilehash: d4cc26d4f206baf9137feae0825b1f9fa5a7c8d6
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-qq-accounts"></a>Azure Active Directory B2C: Zich kunnen registreren en aanmelden gebruikers bieden met q-accounts

> [!NOTE]
> Deze functie is in preview.
> 

## <a name="create-a-qq-application"></a>Een q-toepassing maken

Als u wilt gebruiken als een id-provider in Azure Active Directory (Azure AD) B2C q gebruikt, moet u een q-toepassing maken en geeft deze met de juiste parameters. U moet een account q om dit te doen. Als u niet hebt, kunt u één voor één [https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033](https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033).

### <a name="register-for-the-qq-developer-program"></a>Registreren voor het programma q-ontwikkelaars

1. Ga naar de [q-portal voor ontwikkelaars](http://open.qq.com) en meld u aan met de referenties van uw q-account.
2. Na het aanmelden, gaat u naar [http://open.qq.com/reg](http://open.qq.com/reg) zelf registreren als een ontwikkelaar.
3. Selecteer in het menu**个人**(afzonderlijke developer).
4. Voer de vereiste gegevens in het formulier en klik op**下一步**(volgende stap).
5. Voltooi de verificatieprocedure van de e-mailbericht.

> [!NOTE]
> U moet wacht een paar dagen na de registratie als ontwikkelaar worden goedgekeurd. 

### <a name="register-a-qq-application"></a>Een q-toepassing registreren

1. Ga naar [https://connect.qq.com/index.html](https://connect.qq.com/index.html).
2. Klik op**应用管理**(app management).
3. Klik op**创建应用**(app maken).
4. Voer de benodigde app-gegevens.
5. Klik op**创建应用**(app maken).
6. Geef de vereiste informatie.
7. Voor de**授权回调域**(callback URL) Voer `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp`. Bijvoorbeeld, als uw `tenant_name` is contoso.onmicrosoft.com, de URL moet worden ingesteld `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.
8. Klik op**创建应用**(app maken).
9. Klik op de bevestigingspagina op**应用管理**(app management) voor terugzending naar de pagina app-beheer.
10. Klik op**查看**(weergeven) naast de app die u zojuist hebt gemaakt.
11. Klik op**修改**(bewerken).
12. Kopiëren vanaf de bovenkant van de pagina de **APP-ID** en **APP-sleutel**.

## <a name="configure-qq-as-an-identity-provider-in-your-tenant"></a>Q configureren als een id-provider in uw tenant
1. Volg deze stappen voor [gaat u naar de blade B2C-functies](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) in de Azure portal.
2. Klik op de blade B2C-functies op **identiteitsproviders**.
3. Klik op **+Toevoegen** boven aan de blade.
4. Geef een beschrijvende **naam** voor de configuratie van de id-provider. Voer bijvoorbeeld 'Q'.
5. Klik op **identiteit providertype**, selecteer **q**, en klik op **OK**.
6. Klik op **instellen van deze id-provider**
7. Voer de **App-sleutel** die u eerder hebt gekopieerd als het **Client-ID**.
8. Voer de **App geheim** die u eerder hebt gekopieerd als het **Clientgeheim**.
9. Klik op **OK** en klik vervolgens op **maken** naar uw q-configuratie op te slaan.

