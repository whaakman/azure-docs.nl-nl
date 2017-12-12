---
title: 'Azure Active Directory B2C: LinkedIn configuratie | Microsoft Docs'
description: Gebruikers met een account in uw toepassingen die zijn beveiligd met Azure Active Directory B2C LinkedIn bieden zich kunnen registreren en aanmelden
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mtillman
editor: bryanla
ms.assetid: fa51a16b-9ce9-4e27-9eff-0869b4c4f0ef
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: 860aa90c391604924850a00cf2137d59fa4a1b53
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-linkedin-accounts"></a>Azure Active Directory B2C: Zich kunnen registreren en aanmelden gebruikers bieden met LinkedIn-accounts
## <a name="create-a-linkedin-application"></a>Een LinkedIn-toepassing maken
Als u wilt gebruiken als een id-provider in Azure Active Directory (Azure AD) B2C LinkedIn gebruikt, moet u een LinkedIn-toepassing maken en geeft deze met de juiste parameters. U moet een LinkedIn-account om dit te doen. Als u niet hebt, kunt u krijgen op het [https://www.linkedin.com/](https://www.linkedin.com/).

1. Ga naar de [LinkedIn ontwikkelaars website](https://www.developer.linkedin.com/) en meld u aan met de referenties van uw LinkedIn-account.
2. Klik op **mijn Apps** in de bovenste menubalk en klik vervolgens op **toepassing maken**.
   
    ![LinkedIn - nieuwe app](./media/active-directory-b2c-setup-li-app/linkedin-new-app.png)
3. In de **Maak een nieuwe toepassing** Vul de relevante informatie (**bedrijfsnaam**, **naam**, **beschrijving**, **Toepassing Logo URL**, **toepassing gebruik**, **Website-URL**, **zakelijke e** en **telefoon (werk)**).
4. Ga akkoord met de **LinkedIn API gebruiksvoorwaarden** en klik op **indienen**.
   
    ![LinkedIn - app registreren](./media/active-directory-b2c-setup-li-app/linkedin-register-app.png)
5. Kopieer de waarden van **Client-ID** en **Clientgeheim**. (U vindt deze onder **verificatiesleutels**.) U moet beide LinkedIn configureren als een id-provider in uw tenant.
   
   > [!NOTE]
   > **Clientgeheim** is een belangrijke beveiligingsreferentie.
   > 
   > 
6. Voer `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in de **geautoriseerd Omleidings-URL's** veld (onder **OAuth 2.0**). Vervang **{tenant}** met de naam van uw tenant (bijvoorbeeld: contoso.onmicrosoft.com). Klik op **toevoegen**, en klik vervolgens op **Update**. De **{tenant}** hoofdlettergevoelig.
   
    ![LinkedIn - Setup-app](./media/active-directory-b2c-setup-li-app/linkedin-setup.png)

## <a name="configure-linkedin-as-an-identity-provider-in-your-tenant"></a>LinkedIn configureren als een id-provider in uw tenant
1. Volg deze stappen voor [gaat u naar de blade B2C-functies](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) in de Azure portal.
2. Klik op de blade B2C-functies op **identiteitsproviders**.
3. Klik op **+Toevoegen** boven aan de blade.
4. Geef een beschrijvende **naam** voor de configuratie van de id-provider. Voer bijvoorbeeld 'LI'.
5. Klik op **identiteit providertype**, selecteer **LinkedIn**, en klik op **OK**.
6. Klik op **instellen van deze id-provider** en voer de client-ID en clientgeheim van de LinkedIn-toepassing die u eerder hebt gemaakt.
7. Klik op **OK** en klik vervolgens op **maken** naar uw LinkedIn-configuratie op te slaan.

