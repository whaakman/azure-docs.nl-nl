---
title: Instellen van zich kunnen registreren en aanmelden met een Microsoft-account met behulp van Azure Active Directory B2C | Microsoft Docs
description: Meld u aan en meld u bieden aan klanten met een Microsoft-account in uw toepassingen met behulp van Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/05/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 338c2a197cb50091c3b272e0ce590341ffda1d7f
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43341080"
---
# <a name="set-up-sign-up-and-sign-in-with-a-microsoft-account-using-azure-active-directory-b2c"></a>Instellen van zich kunnen registreren en aanmelden met een Microsoft-account met behulp van Azure Active Directory B2C

## <a name="create-a-microsoft-account-application"></a>Een toepassing van Microsoft-account maken

Voor het gebruik van een Microsoft-account als id-provider in Azure Active Directory (Azure AD) B2C, moet u een toepassing maken in de tenant die aangeeft. Als u nog een Microsoft-account hebt, kunt u krijgen via [ https://www.live.com/ ](https://www.live.com/).

1. Aanmelden bij de [Portal voor Appregistratie Microsoft](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) met uw Microsoft-accountreferenties.
2. Selecteer in de rechterbovenhoek **een app toevoegen**.
3. Geef een **naam** voor uw toepassing en klik op **maken**.
4. Op de pagina voor de registratie, Kopieer de waarde van **toepassings-Id**. U kunt uw Microsoft-account als id-provider configureren in uw tenant.
5. Selecteer **toevoegen platform**, en vervolgens en kies **Web**.
6. Voer `https://{tenant}.b2clogin.com/te/{tenant}.onmicrosoft.com/oauth2/authresp` in **Omleidings-URL's**. Vervang **{tenant}** met de naam van uw tenant (bijvoorbeeld contosob2c).
7. Selecteer **nieuw wachtwoord genereren** onder **Toepassingsgeheimen**. Kopieer het nieuwe wachtwoord weergegeven op het scherm. U moet een Microsoft-account configureren als een id-provider in uw tenant. Dit wachtwoord is een belangrijke beveiligingsreferentie.

## <a name="configure-a-microsoft-account-as-an-identity-provider"></a>Een Microsoft-account configureren als een id-provider

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) als globale beheerder van uw Azure AD B2C-tenant.
2. Zorg ervoor dat u de map met uw Azure AD B2C-tenant gebruikt door hiernaar over te schakelen rechtsboven in de Azure Portal. Selecteer de abonnementsgegevens en selecteer vervolgens **Schakelen tussen mappen**. 

    ![Overschakelen naar de Azure AD B2C-tenant](./media/active-directory-b2c-setup-msa-app/switch-directories.png)

    Kies de map met uw tenant.

    ![Map selecteren](./media/active-directory-b2c-setup-msa-app/select-directory.png)

3. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
4. Selecteer **id-providers**, en selecteer vervolgens **toevoegen**.
5. Geef een **naam**. Voer bijvoorbeeld *MSA*.
6. Selecteer **type id-provider**, selecteer **Microsoft-Account**, en klikt u op **OK**.
7. Selecteer **instellen van deze id-provider** en voer de toepassings-Id die u eerder hebt genoteerd als de **Client-ID** en voer het wachtwoord die u hebt genoteerd als de **clientgeheim**van de Microsoft-account-toepassing die u eerder hebt gemaakt.
8. Klik op **OK** en klik vervolgens op **maken** om op te slaan van de configuratie van uw Microsoft-account.

