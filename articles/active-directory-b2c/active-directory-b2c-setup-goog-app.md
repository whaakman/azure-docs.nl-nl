---
title: Instellen van zich kunnen registreren en aanmelden met een Google-account met behulp van Azure Active Directory B2C | Microsoft Docs
description: Meld u aan en meld u bieden aan klanten met een Google-account in uw toepassingen met behulp van Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/06/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 477bd6047da639dcf21592a7ec0c1b80844e031e
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43337731"
---
# <a name="set-up-sign-up-and-sign-in-with-a-google-account-using-azure-active-directory-b2c"></a>Instellen van zich kunnen registreren en aanmelden met een Google-account met behulp van Azure Active Directory B2C

## <a name="create-a-google-application"></a>Een Google-toepassing maken

Voor het gebruik van een Google-account als id-provider in Azure Active Directory (Azure AD) B2C, moet u een toepassing maken in de tenant die aangeeft. Als u nog een Google-account hebt kunt u krijgen via [ https://accounts.google.com/SignUp ](https://accounts.google.com/SignUp).

1. Aanmelden bij de [Google ontwikkelaars Console](https://console.developers.google.com/) met de referenties van uw Google-account.
2. Selecteer **project maken**, en klik vervolgens op **maken**. Als u projecten voordat u hebt gemaakt, selecteert u de projectlijst en selecteer vervolgens **nieuw Project**.
3. Voer een **projectnaam**, en klik vervolgens op **maken**.
3. Selecteer **referenties** in het menu aan de linkerkant en selecteer vervolgens **referenties maken** > **Oauth-Clientidentiteit**.
4. Selecteer **configureren instemmingsscherm**.
5. Selecteer of geef een geldige **e-mailadres**, bieden een **productnaam weergegeven voor gebruikers**, en klikt u op **opslaan**.
6. Onder **toepassingstype**, selecteer **webtoepassing**.
7. Voer een **naam** voor uw toepassing, voert u `https://{tenant}.b2clogin.com` in **geautoriseerd JavaScript oorsprongen**, en `https://{tenant}.b2clogin.com/te/{tenant}.onmicrosoft.com/oauth2/authresp` in **geautoriseerde omleidings-URI's**. Vervang **{tenant}** met de naam van uw tenant (bijvoorbeeld contosob2c).
8. Klik op **Create**.
9. Kopieer de waarden van **Client-ID** en **clientgeheim**. U moet beide Google configureren als een id-provider in uw tenant. **Clientgeheim** is een belangrijke beveiligingsreferentie.

## <a name="configure-a-google-account-as-an-identity-provider"></a>Een Google-account configureren als een id-provider

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) als globale beheerder van uw Azure AD B2C-tenant.
2. Zorg ervoor dat u de map met uw Azure AD B2C-tenant gebruikt door hiernaar over te schakelen rechtsboven in de Azure Portal. Selecteer de abonnementsgegevens en selecteer vervolgens **Schakelen tussen mappen**. 

    ![Overschakelen naar de Azure AD B2C-tenant](./media/active-directory-b2c-setup-fb-app/switch-directories.png)

    Kies de map met uw tenant.

    ![Map selecteren](./media/active-directory-b2c-setup-fb-app/select-directory.png)

3. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
4. Selecteer **id-providers**, en selecteer vervolgens **toevoegen**.
5. Voer een **naam**. Voer bijvoorbeeld *Google*.
6. Selecteer **type id-provider**, selecteer **Google**, en klikt u op **OK**.
7. Selecteer **instellen van deze id-provider** en voert u de Client-ID die u eerder hebt genoteerd als de **Client-ID** en voer het Clientgeheim die u hebt genoteerd als de **clientgeheim**van de Google-toepassing die u eerder hebt gemaakt.
8. Klik op **OK** en klik vervolgens op **maken** aan uw Google-configuratie op te slaan.

