---
title: Instellen van zich kunnen registreren en aanmelden met een Amazon-account met behulp van Azure Active Directory B2C | Microsoft Docs
description: Meld u aan en meld u bieden aan klanten met een Amazon-account in uw toepassingen met behulp van Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 5ded04f564439be332152bfad0fc571fe8eaf8c2
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "44720970"
---
# <a name="set-up-sign-up-and-sign-in-with-an-amazon-account-using-azure-active-directory-b2c"></a>Instellen van zich kunnen registreren en aanmelden met een Amazon-account met behulp van Azure Active Directory B2C

## <a name="create-an-amazon-application"></a>Een Amazon-toepassing maken

Voor het gebruik van een Amazon-account als id-provider in Azure Active Directory (Azure AD) B2C, moet u een toepassing maken in de tenant die aangeeft. Als u nog een Amazon-account hebt kunt u krijgen via [ http://www.amazon.com/ ](http://www.amazon.com/).

1. Aanmelden bij de [Amazon Developer Center](https://login.amazon.com/) met de referenties van de Amazon-account.
2. Als u hebt nog niet gedaan, klikt u op **registreren**, volgt u de stappen van de registratie van ontwikkelaars en accepteren van het beleid.
3. Selecteer **nieuwe toepassing registreren**.
4. Voer een **naam**, **beschrijving**, en **Privacy-URL voor kennisgeving**, en klik vervolgens op **opslaan**. De privacyverklaring is een pagina die u beheert waarop privacyinformatie aan gebruikers biedt.
5. In de **Webinstellingen** sectie, Kopieer de waarden van **Client-ID**. Selecteer **geheim weergeven** voor het ophalen van het clientgeheim en kopieer deze vervolgens. U moet beide een Amazon-account configureert als een id-provider in uw tenant. **Clientgeheim** is een belangrijke beveiligingsreferentie.
6. In de **Webinstellingen** sectie, selecteer **bewerken**, en voer vervolgens `https://your-tenant-name.b2clogin.com` in **JavaScript oorsprongen toegestaan** en `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` in **toegestaan URL's retourneren**. Vervang `your-tenant-name` met de naam van uw tenant. U moet alle kleine letters gebruiken bij het invoeren van de tenantnaam van uw, zelfs als de tenant is gedefinieerd met behulp van hoofdletters in Azure AD B2C.
7. Klik op **Opslaan**.

## <a name="configure-an-amazon-account-as-an-identity-provider"></a>Een Amazon-account configureert als een id-provider

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) als globale beheerder van uw Azure AD B2C-tenant.
2. Zorg ervoor dat u de map met uw Azure AD B2C-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map waarin uw tenant te kiezen. 

    ![Overschakelen naar de Azure AD B2C-tenant](./media/active-directory-b2c-setup-amzn-app/switch-directories.png)

3. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
4. Selecteer **id-providers**, en selecteer vervolgens **toevoegen**.
5. Voer een **naam**. Voer bijvoorbeeld *Amazon*.
6. Selecteer **type id-provider**, selecteer **Amazon**, en klikt u op **OK**.
7. Selecteer **instellen van deze id-provider** en voert u de Client-ID die u eerder hebt genoteerd als de **Client-ID** en voer het Clientgeheim die u hebt genoteerd als de **clientgeheim**van de Amazon-toepassing die u eerder hebt gemaakt.
8. Klik op **OK** en klik vervolgens op **maken** om op te slaan van de Amazon-configuratie.

