---
title: Instellen van zich kunnen registreren en aanmelden met een Twitter-account met behulp van Azure Active Directory B2C | Microsoft Docs
description: Meld u aan en meld u bieden aan klanten met een Twitter-account in uw toepassingen met behulp van Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/09/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 6d8e9245e95c08aad69cd05f338b6260e554469b
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43337787"
---
# <a name="set-up-sign-up-and-sign-in-with-a-twitter-account-using-azure-active-directory-b2c"></a>Instellen van zich kunnen registreren en aanmelden met een Twitter-account met behulp van Azure Active Directory B2C

## <a name="create-a-twitter-application"></a>Een Twitter-toepassing maken

Voor het gebruik van een Twitter-account als id-provider in Azure Active Directory (Azure AD) B2C, moet u een toepassing maken in de tenant die aangeeft. Als u nog een Twitter-account hebt, kunt u krijgen via [ https://twitter.com/signup ](https://twitter.com/signup).

1. Aanmelden bij de [Twitter Apps](https://apps.twitter.com/) met uw Twitter-referenties.
2. Selecteer **nieuwe App maken**.
3. Voer de **naam**, **beschrijving**, en **Website**.
4. Voer `https://{tenant}.b2clogin.com/te/{tenant}.onmicrosoft.com/{policyId}/oauth1/authresp` in **Callback-URL's**. Vervang **{tenant}** met de naam van uw tenant (bijvoorbeeld contosob2c) en **{policyId}** met uw beleids-ID (bijvoorbeeld b2c_1_policy). U moet een URL voor terugbellen voor alle beleidsregels die gebruikmaken van de Twitter-account toevoegen. 
5. Ga akkoord met de **Developer-overeenkomst** en selecteer **uw Twitter-toepassing maken**.
7. Selecteer de **Keys and Access Tokens** tabblad.
8. Kopieer de waarde van **Consumer Key** en **Consumer Secret**. U moet beide een Twitter-account worden geconfigureerd als een id-provider in uw tenant.

## <a name="configure-twitter-as-an-identity-provider-in-your-tenant"></a>Configureren van Twitter als id-provider in uw tenant

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) als globale beheerder van uw Azure AD B2C-tenant.
2. Zorg ervoor dat u de map met uw Azure AD B2C-tenant gebruikt door hiernaar over te schakelen rechtsboven in de Azure Portal. Selecteer de abonnementsgegevens en selecteer vervolgens **Schakelen tussen mappen**. 

    ![Overschakelen naar de Azure AD B2C-tenant](./media/active-directory-b2c-setup-twitter-app/switch-directories.png)

    Kies de map met uw tenant.

    ![Map selecteren](./media/active-directory-b2c-setup-twitter-app/select-directory.png)

3. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
4. Selecteer **id-providers**, en selecteer vervolgens **toevoegen**.
5. Geef een **naam**. Voer bijvoorbeeld *Twitter*.
6. Selecteer **type id-provider**, selecteer **Twitter**, en klikt u op **OK**.
7. Selecteer **instellen van deze id-provider** en voer de consument-sleutel voor de **Client-ID** en de **Consumer Secret** voor de **clientgeheim**.
8. Klik op **OK**, en klik vervolgens op **maken** naar uw Twitter-configuratie op te slaan.