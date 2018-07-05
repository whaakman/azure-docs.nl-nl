---
title: LinkedIn-configuratie in Azure Active Directory B2C | Microsoft Docs
description: Meld u aan en meld u aan consumenten met LinkedIn-accounts in uw toepassingen die zijn beveiligd met Azure Active Directory B2C bieden.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/06/2016
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 7588711bd1c2a02e2e9a100d2ba182f43e7df488
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37446067"
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-linkedin-accounts"></a>Azure Active Directory B2C: Bieden zich kunnen registreren en aanmelden voor consumenten met LinkedIn-accounts
## <a name="create-a-linkedin-application"></a>Een LinkedIn-toepassing maken
Voor het gebruik van LinkedIn als id-provider in Azure Active Directory (Azure AD) B2C, moet u een LinkedIn-toepassing maken en geven met de juiste parameters. U moet een LinkedIn-account om dit te doen. Als u niet hebt, kunt u krijgen via [ https://www.linkedin.com/ ](https://www.linkedin.com/).

1. Ga naar de [LinkedIn ontwikkelaars website](https://www.developer.linkedin.com/) en meld u aan met de referenties van uw LinkedIn-account.
2. Klik op **mijn Apps** in de bovenste menubalk en klik vervolgens op **toepassing maken**.
   
    ![LinkedIn - nieuwe app](./media/active-directory-b2c-setup-li-app/linkedin-new-app.png)
3. In de **Maak een nieuwe toepassing** formulier, vul de relevante gegevens (**bedrijfsnaam**, **naam**, **beschrijving**, **Toepassing Logo URL**, **gebruik**, **Website-URL**, **zakelijke e-mailadres** en **telefoon**).
4. Ga akkoord met de **LinkedIn gebruiksvoorwaarden API** en klikt u op **indienen**.
   
    ![LinkedIn - app registreren](./media/active-directory-b2c-setup-li-app/linkedin-register-app.png)
5. Kopieer de waarden van **Client-ID** en **Clientgeheim**. (U vindt deze onder **verificatiesleutels**.) U moet beide LinkedIn configureren als een id-provider in uw tenant.
   
   > [!NOTE]
   > **Clientgeheim** is een belangrijke beveiligingsreferentie.
   > 
   > 
6. Voer `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in de **Omleidings-URL's toegestaan** veld (onder **OAuth 2.0**). Vervang **{tenant}** met de naam van uw tenant (bijvoorbeeld contoso.onmicrosoft.com). Klik op **toevoegen**, en klik vervolgens op **Update**. De **{tenant}** -waarde moet liggen in kleine letters.
   
    ![LinkedIn - app instellen](./media/active-directory-b2c-setup-li-app/linkedin-setup.png)

## <a name="configure-linkedin-as-an-identity-provider-in-your-tenant"></a>LinkedIn configureren als een id-provider in uw tenant
1. Volg deze stappen om [gaat u naar de blade B2C-functies](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) in Azure portal.
2. Klik op de blade B2C-functies **id-providers**.
3. Klik op **+Toevoegen** boven aan de blade.
4. Geef een beschrijvende **naam** voor de configuratie van de id-provider. Voer bijvoorbeeld 'LI'.
5. Klik op **type id-provider**, selecteer **LinkedIn**, en klikt u op **OK**.
6. Klik op **instellen van deze id-provider** en voer de client-ID en clientgeheim van de LinkedIn-toepassing die u eerder hebt gemaakt.
7. Klik op **OK** en klik vervolgens op **maken** aan uw LinkedIn-configuratie op te slaan.

