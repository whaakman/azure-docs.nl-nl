---
title: Amazon-configuratie in Azure Active Directory B2C | Microsoft Docs
description: Meld u aan en meld u bieden voor gebruikers met een Amazon-account in uw toepassingen die zijn beveiligd met Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/06/2016
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: e3b3d66b913b595e68c03b68990d1a4806952579
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37443695"
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-amazon-accounts"></a>Azure Active Directory B2C: Bieden zich kunnen registreren en aanmelden voor consumenten met Amazon-accounts
## <a name="create-an-amazon-application"></a>Een Amazon-toepassing maken
Voor het gebruik van Amazon als id-provider in Azure Active Directory (Azure AD) B2C, moet u een Amazon-toepassing maakt en met de juiste parameters op te geven. U moet een Amazon-account om dit te doen. Als u niet hebt, kunt u krijgen via [ http://www.amazon.com/ ](http://www.amazon.com/).

1. Ga naar de [Amazon Developer Center](https://login.amazon.com/) en meld u aan met de referenties van uw Amazon-account.
2. Als u hebt nog niet gedaan, klikt u op **registreren**, volgt u de stappen van de registratie van ontwikkelaars en accepteren van het beleid.
3. Klik op **nieuwe toepassing registreren**.
   
    ![Registreren van een nieuwe toepassing op de Amazon-website](./media/active-directory-b2c-setup-amzn-app/amzn-new-app.png)
4. Geef informatie over toepassingen (**naam**, **beschrijving**, en **Privacy-URL voor kennisgeving**) en klikt u op **opslaan**.
   
    ![Biedt informatie over de toepassing voor het registreren van een nieuwe toepassing met Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-register-app.png)
5. In de **Webinstellingen** sectie, Kopieer de waarden van **Client-ID** en **Clientgeheim**. (U moet klikken op de **geheim weergeven** knop om dit te zien.) U moet beide van Amazon worden geconfigureerd als een id-provider in uw tenant. Klik op **bewerken** aan de onderkant van de sectie. **Clientgeheim** is een belangrijke beveiligingsreferentie.
   
    ![Client-ID en Clientgeheim leveren voor uw nieuwe toepassing vanaf verschillende Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-client-secret.png)
6. Voer `https://login.microsoftonline.com` in de **JavaScript oorsprongen toegestaan** veld en `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in de **retourneren URL's toegestaan** veld. Vervang **{tenant}** met de naam van uw tenant (bijvoorbeeld contoso.onmicrosoft.com). Klik op **Opslaan**. De **{tenant}** hoofdlettergevoelig.
   
    ![Geef daarbij JavaScript oorsprongen en retourneren URL's voor uw nieuwe toepassing vanaf verschillende Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-urls.png)

## <a name="configure-amazon-as-an-identity-provider-in-your-tenant"></a>Amazon configureren als een id-provider in uw tenant
1. Volg deze stappen om [gaat u naar de blade B2C-functies](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) in Azure portal.
2. Klik op de blade B2C-functies **id-providers**.
3. Klik op **+Toevoegen** boven aan de blade.
4. Geef een beschrijvende **naam** voor de configuratie van de id-provider. Voer bijvoorbeeld 'Amzn'.
5. Klik op **type id-provider**, selecteer **Amazon**, en klikt u op **OK**.
6. Klik op **instellen van deze id-provider** en voer de client-ID en clientgeheim van de Amazon-toepassing die u eerder hebt gemaakt.
7. Klik op **OK** en klik vervolgens op **maken** om op te slaan van de Amazon-configuratie.

