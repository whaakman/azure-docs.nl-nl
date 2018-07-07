---
title: Instellen van zich kunnen registreren en aanmelden met een Facebook-account met behulp van Azure Active Directory B2C | Microsoft Docs
description: Meld u aan en meld u bieden aan klanten met een Facebook-account in uw toepassingen met behulp van Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/06/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: de059e3875b5f15526cb176d43a019fd2d9ee9b9
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2018
ms.locfileid: "37901378"
---
# <a name="set-up-sign-up-and-sign-in-with-a-facebook-account-using-azure-active-directory-b2c"></a>Instellen van zich kunnen registreren en aanmelden met een Facebook-account met behulp van Azure Active Directory B2C

## <a name="create-a-facebook-application"></a>Een Facebook-toepassing maken

Voor het gebruik van een Facebook-account als id-provider in Azure Active Directory (Azure AD) B2C, moet u een toepassing maken in de tenant die aangeeft. Als u nog een Facebook-account hebt, kunt u krijgen via [ https://www.facebook.com/ ](https://www.facebook.com/).

1. Aanmelden bij [Facebook voor ontwikkelaars](https://developers.facebook.com/) met de referenties van uw Facebook-account.
2. Als u hebt nog niet gedaan, moet u registreren als een Facebook-ontwikkelaar. Om dit te doen, selecteert u **registreren** in de rechterbovenhoek van de pagina accepteren van Facebook-beleid en de registratie van stappen.
3. Selecteer **mijn Apps** en klik vervolgens op **nieuwe App toevoegen**. 
4. Voer een **weergavenaam** en een geldig **Neem contact op met e-mailadres**.
5. Klik op **maken van App-ID**. Mogelijk moet u beleid voor het platform van Facebook accepteren en een online beveiliging voltooien.
6. Selecteer **instellingen** > **Basic**.
7. Selecteer aan de onderkant van de pagina **Platform toevoegen**, en selecteer vervolgens **Website**.
8. Voer `https://login.microsoftonline.com/` in **Site-URL**. Voer een URL voor de **URL privacybeleid**, bijvoorbeeld `http://www.contoso.com`.
9. Selecteer **wijzigingen opslaan**.
11. Aan de bovenkant van de pagina, Kopieer de waarde van **App-ID**. 
12. Klik op **weergeven** en kopieer de waarde van **Appgeheim**. U kunt van beide Facebook als id-provider configureren in uw tenant. **App-geheim** is een belangrijke beveiligingsreferentie.
13. Selecteer **producten**, en selecteer vervolgens **instellen** onder **aanmelden via Facebook**.
14. Selecteer **instellingen** onder **aanmelden via Facebook**.
15. Voer `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in **geldig OAuth omleidings-URI's** . Vervang **{tenant}** met de naam van uw tenant (bijvoorbeeld: contosob2c.onmicrosoft.com). Klik op **wijzigingen opslaan** aan de onderkant van de pagina.
16. Als u uw Facebook-toepassing naar Azure AD B2C, selecteer **App-revisie**, stel **openbaar maken van mijn toepassing?** naar **Ja**, een categorie kiezen, bijvoorbeeld `Business and Pages`en klik vervolgens op **bevestigen**.

## <a name="configure-a-facebook-account-as-an-identity-provider"></a>Configureren van een Facebook-account als id-provider

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) als globale beheerder van uw Azure AD B2C-tenant.
2. Zorg ervoor dat u de map met uw Azure AD B2C-tenant gebruikt door hiernaar over te schakelen rechtsboven in de Azure Portal. Selecteer de abonnementsgegevens en selecteer vervolgens **Schakelen tussen mappen**. 

    ![Overschakelen naar de Azure AD B2C-tenant](./media/active-directory-b2c-setup-fb-app/switch-directories.png)

    Kies de map met uw tenant.

    ![Map selecteren](./media/active-directory-b2c-setup-fb-app/select-directory.png)

3. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
4. Selecteer **id-providers**, en selecteer vervolgens **toevoegen**.
5. Voer een **naam**. Voer bijvoorbeeld *Facebook*.
6. Selecteer **type id-provider**, selecteer **Facebook**, en klikt u op **OK**.
7. Selecteer **instellen van deze id-provider** en voert u de App-ID die u eerder hebt genoteerd als de **Client-ID** en voert u de App-geheim die u hebt genoteerd als de **clientgeheim** van de Facebook-toepassing die u eerder hebt gemaakt).
8. Klik op **OK** en klik vervolgens op **maken** op uw Facebook-configuratie op te slaan.