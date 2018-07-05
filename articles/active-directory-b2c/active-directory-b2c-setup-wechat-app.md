---
title: WeChat-configuratie in Azure Active Directory B2C | Microsoft Docs
description: Meld u aan en meld u bieden voor gebruikers met een WeChat-account in uw toepassingen die zijn beveiligd met Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 3/26/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: a18d41a4f45b147790a17664156659d282e710d4
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37445932"
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-wechat-accounts"></a>Azure Active Directory B2C: Bieden zich kunnen registreren en aanmelden voor consumenten met WeChat-accounts

> [!NOTE]
> Deze functie is beschikbaar als preview-versie.
> 

## <a name="create-a-wechat-application"></a>Een WeChat-toepassing maken

Voor het gebruik van WeChat als id-provider in Azure Active Directory (Azure AD) B2C, moet u een WeChat-toepassing maken en geven met de juiste parameters. U moet een WeChat-account om dit te doen. Als u niet hebt, kunt u één voor het aanmelden via een van de mobiele apps of met behulp van het nummer van uw q kunt ophalen. Hierna krijgen uw account is geregistreerd bij de WeChat-Ontwikkelaarsprogramma. U vindt meer informatie [hier](http://kf.qq.com/faq/161220Brem2Q161220uUjERB.html).

### <a name="register-a-wechat-application"></a>Een WeChat-toepassing registreren

1. Ga naar [https://open.weixin.qq.com/](https://open.weixin.qq.com/) en meld u aan.
2. Klik op**管理中心**(center management).
3. Volg de stappen die nodig zijn voor het registreren van een nieuwe toepassing.
4. Voor**授权回调域**(URL voor terugbellen), voer `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp`. Bijvoorbeeld, als uw `tenant_name` is contoso.onmicrosoft.com, de URL die moet worden ingesteld `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.
5. Zoeken en kopieer de **APP-ID** en **APP-sleutel**. U moet deze later.

## <a name="configure-wechat-as-an-identity-provider-in-your-tenant"></a>WeChat configureren als een id-provider in uw tenant
1. Volg deze stappen om [gaat u naar de blade B2C-functies](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) in Azure portal.
2. Klik op de blade B2C-functies **id-providers**.
3. Klik op **+Toevoegen** boven aan de blade.
4. Geef een beschrijvende **naam** voor de configuratie van de id-provider. Voer bijvoorbeeld 'WeChat'.
5. Klik op **type id-provider**, selecteer **WeChat**, en klikt u op **OK**.
6. Klik op **deze id-provider instellen**
7. Voer de **App-sleutel** die u eerder hebt gekopieerd als de **Client-ID**.
8. Voer de **Appgeheim** die u eerder hebt gekopieerd als de **Clientgeheim**.
9. Klik op **OK** en klik vervolgens op **maken** aan uw WeChat-configuratie op te slaan.

