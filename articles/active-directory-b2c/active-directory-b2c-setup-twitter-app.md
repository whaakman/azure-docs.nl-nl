---
title: Instellen van zich kunnen registreren en aanmelden met een Twitter-account met behulp van Azure Active Directory B2C | Microsoft Docs
description: Meld u aan en meld u bieden aan klanten met een Twitter-account in uw toepassingen met behulp van Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 5732293510a75a3c40df5cf3d31978c5ce599791
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "44720154"
---
# <a name="set-up-sign-up-and-sign-in-with-a-twitter-account-using-azure-active-directory-b2c"></a>Instellen van zich kunnen registreren en aanmelden met een Twitter-account met behulp van Azure Active Directory B2C

## <a name="create-a-twitter-application"></a>Een Twitter-toepassing maken

Voor het gebruik van een Twitter-account als id-provider in Azure Active Directory (Azure AD) B2C, moet u een toepassing maken in de tenant die aangeeft. Als u nog een Twitter-account hebt, kunt u krijgen via [ https://twitter.com/signup ](https://twitter.com/signup).

1. Aanmelden bij de [Twitter Apps](https://apps.twitter.com/) met uw Twitter-referenties.
2. Selecteer **maken van een app**.
3. Voer de **appnaam**, **toepassingsbeschrijving**, en **Website-URL**.
4. Voer `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/your-policy-name/oauth1/authresp` in **Callback-URL's**. Vervang `your-tenant-name` met de naam van uw tenant en `your-policy-name` met de naam van uw beleid. Bijvoorbeeld `b2c_1_signupsignin`. U moet alle kleine letters gebruiken bij het invoeren van uw tenantnaam en de naam van beleid, zelfs als ze zijn gedefinieerd met behulp van hoofdletters in Azure AD B2C.
5. Ga akkoord met de **Developer-overeenkomst** en selecteer **maken**.
7. Selecteer de **Keys and Access Tokens** tabblad.
8. Kopieer de waarden van **API-sleutel** en **API geheime sleutel**. U moet beide een Twitter-account worden geconfigureerd als een id-provider in uw tenant.

## <a name="configure-twitter-as-an-identity-provider-in-your-tenant"></a>Configureren van Twitter als id-provider in uw tenant

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) als globale beheerder van uw Azure AD B2C-tenant.
2. Zorg ervoor dat u de map met uw Azure AD B2C-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map waarin uw tenant te kiezen.  

    ![Overschakelen naar de Azure AD B2C-tenant](./media/active-directory-b2c-setup-twitter-app/switch-directories.png)

3. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
4. Selecteer **id-providers**, en selecteer vervolgens **toevoegen**.
5. Geef een **naam**. Voer bijvoorbeeld *Twitter*.
6. Selecteer **type id-provider**, selecteer **Twitter**, en klikt u op **OK**.
7. Selecteer **instellen van deze id-provider** en voer de API-sleutel voor de **Client-ID** en de geheime sleutel van de API voor de **clientgeheim**.
8. Klik op **OK**, en klik vervolgens op **maken** naar uw Twitter-configuratie op te slaan.