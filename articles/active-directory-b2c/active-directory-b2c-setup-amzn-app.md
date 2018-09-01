---
title: Instellen van zich kunnen registreren en aanmelden met een Amazon-account met behulp van Azure Active Directory B2C | Microsoft Docs
description: Meld u aan en meld u bieden aan klanten met een Amazon-account in uw toepassingen met behulp van Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/06/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: c64b32656db2d3b821833450b4e866b9e33e44cd
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43337342"
---
# <a name="set-up-sign-up-and-sign-in-with-an-amazon-account-using-azure-active-directory-b2c"></a>Instellen van zich kunnen registreren en aanmelden met een Amazon-account met behulp van Azure Active Directory B2C

## <a name="create-an-amazon-application"></a>Een Amazon-toepassing maken

Voor het gebruik van een Amazon-account als id-provider in Azure Active Directory (Azure AD) B2C, moet u een toepassing maken in de tenant die aangeeft. Als u nog een Amazon-account hebt kunt u krijgen via [ http://www.amazon.com/ ](http://www.amazon.com/).

1. Aanmelden bij de [Amazon Developer Center](https://login.amazon.com/) met de referenties van de Amazon-account.
2. Als u hebt nog niet gedaan, klikt u op **registreren**, volgt u de stappen van de registratie van ontwikkelaars en accepteren van het beleid.
3. Selecteer **nieuwe toepassing registreren**.
4. Voer een **naam**, **beschrijving**, en **Privacy-URL voor kennisgeving**, en klik vervolgens op **opslaan**.
5. In de **Webinstellingen** sectie, Kopieer de waarden van **Client-ID**. Selecteer **geheim weergeven** voor het ophalen van het clientgeheim en kopieer deze vervolgens. U moet beide een Amazon-account configureert als een id-provider in uw tenant. **Clientgeheim** is een belangrijke beveiligingsreferentie.
6. In de **Webinstellingen** sectie, selecteer **bewerken**, en voer vervolgens `https://{tenant}.b2clogin.com` in **JavaScript oorsprongen toegestaan** en `https://{tenant}.b2clogin.com/te/{tenant}.onmicrosoft.com/oauth2/authresp` in **toegestaan URL's retourneren**. Vervang **{tenant}** met de naam van uw tenant (bijvoorbeeld contosob2c). 
7. Klik op **Opslaan**.

## <a name="configure-an-amazon-account-as-an-identity-provider"></a>Een Amazon-account configureert als een id-provider

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) als globale beheerder van uw Azure AD B2C-tenant.
2. Zorg ervoor dat u de map met uw Azure AD B2C-tenant gebruikt door hiernaar over te schakelen rechtsboven in de Azure Portal. Selecteer de abonnementsgegevens en selecteer vervolgens **Schakelen tussen mappen**. 

    ![Overschakelen naar de Azure AD B2C-tenant](./media/active-directory-b2c-setup-fb-app/switch-directories.png)

    Kies de map met uw tenant.

    ![Map selecteren](./media/active-directory-b2c-setup-fb-app/select-directory.png)

3. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
4. Selecteer **id-providers**, en selecteer vervolgens **toevoegen**.
5. Voer een **naam**. Voer bijvoorbeeld *Amazon*.
6. Selecteer **type id-provider**, selecteer **Amazon**, en klikt u op **OK**.
7. Selecteer **instellen van deze id-provider** en voert u de Client-ID die u eerder hebt genoteerd als de **Client-ID** en voer het Clientgeheim die u hebt genoteerd als de **clientgeheim**van de Amazon-toepassing die u eerder hebt gemaakt.
8. Klik op **OK** en klik vervolgens op **maken** om op te slaan van de Amazon-configuratie.

