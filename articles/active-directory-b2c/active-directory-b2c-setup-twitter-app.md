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
ms.openlocfilehash: bf5ae39d83fd021775fbd18cf23d2e6b9078e748
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37927893"
---
# <a name="set-up-sign-up-and-sign-in-with-a-twitter-account-using-azure-active-directory-b2c"></a>Instellen van zich kunnen registreren en aanmelden met een Twitter-account met behulp van Azure Active Directory B2C

## <a name="create-a-twitter-application"></a>Een Twitter-toepassing maken

Voor het gebruik van een Twitter-account als id-provider in Azure Active Directory (Azure AD) B2C, moet u een toepassing maken in de tenant die aangeeft. Als u nog een Twitter-account hebt, kunt u krijgen via [ https://twitter.com/signup ](https://twitter.com/signup).

1. Aanmelden bij de [Twitter Apps](https://apps.twitter.com/) met uw Twitter-referenties.
2. Selecteer **nieuwe App maken**.
3. Voer de **naam**, **beschrijving**, en **Website**.
4. Voer `https://login.microsoftonline.com/te/{tenant}/{policyId}/oauth1/authresp` in **Callback-URL's**. Vervang **{tenant}** met de naam van uw tenant (bijvoorbeeld: contosob2c.onmicrosoft.com) en **{policyId}** met uw beleids-ID (bijvoorbeeld b2c_1_policy). U moet een URL voor terugbellen voor alle beleidsregels die gebruikmaken van de Twitter-account toevoegen. Zorg ervoor dat u `b2clogin.com` in plaats van ` login.microsoftonline.com` als u deze in uw toepassing.
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