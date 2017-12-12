---
title: 'Azure Active Directory B2C: Amazon configuratie | Microsoft Docs'
description: Registreren en aanmelden gebruikers met Amazon-accounts in uw toepassingen die zijn beveiligd met Azure Active Directory B2C bieden.
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mtillman
editor: bryanla
ms.assetid: 77c099bb-a005-4d75-87f9-f61e3de48725
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: df69b075717e6b4311e336f5bd2d385c5f0e1f52
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-amazon-accounts"></a>Azure Active Directory B2C: Zich kunnen registreren en aanmelden gebruikers bieden met Amazon-accounts
## <a name="create-an-amazon-application"></a>Een Amazon-toepassing maken
Als u wilt gebruiken als een id-provider in Azure Active Directory (Azure AD) B2C Amazon gebruikt, moet u een Amazon-toepassing maken en geeft deze met de juiste parameters. U moet een Amazon-account om dit te doen. Als u niet hebt, kunt u krijgen op het [http://www.amazon.com/](http://www.amazon.com/).

1. Ga naar de [Amazon Developer Center](https://login.amazon.com/) en meld u aan met de referenties van uw Amazon-account.
2. Als u dit nog niet hebt gedaan, klikt u op **aanmelden**, volg de stappen van de registratie van ontwikkelaars en accepteren van het beleid.
3. Klik op **nieuwe toepassing registreren**.
   
    ![Registreren van een nieuwe toepassing op de Amazon-website](./media/active-directory-b2c-setup-amzn-app/amzn-new-app.png)
4. Bevatten toepassingsinformatie (**naam**, **beschrijving**, en **Privacy-URL voor kennisgeving**) en klik op **opslaan**.
   
    ![Informatie over de toepassing voor het registreren van een nieuwe toepassing op de Amazon bieden](./media/active-directory-b2c-setup-amzn-app/amzn-register-app.png)
5. In de **Webinstellingen** sectie, Kopieer de waarden van **Client-ID** en **Clientgeheim**. (U moet op de **geheim weergeven** knop om dit te zien.) U moet beide Amazon configureren als een id-provider in uw tenant. Klik op **bewerken** aan de onderkant van de sectie. **Clientgeheim** is een belangrijke beveiligingsreferentie.
   
    ![Client-ID en Clientgeheim bieden voor uw nieuwe toepassing op de Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-client-secret.png)
6. Voer `https://login.microsoftonline.com` in de **toegestaan JavaScript oorsprongen** veld en `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in de **retourneren URL's toegestaan** veld. Vervang **{tenant}** met de naam van uw tenant (bijvoorbeeld: contoso.onmicrosoft.com). Klik op **Opslaan**. De **{tenant}** hoofdlettergevoelig.
   
    ![Oorsprongen JavaScript en retourneren URL's voorziet in uw nieuwe toepassing op de Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-urls.png)

## <a name="configure-amazon-as-an-identity-provider-in-your-tenant"></a>Amazon configureren als een id-provider in uw tenant
1. Volg deze stappen voor [gaat u naar de blade B2C-functies](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) in de Azure portal.
2. Klik op de blade B2C-functies op **identiteitsproviders**.
3. Klik op **+Toevoegen** boven aan de blade.
4. Geef een beschrijvende **naam** voor de configuratie van de id-provider. Voer bijvoorbeeld 'Amzn'.
5. Klik op **identiteit providertype**, selecteer **Amazon**, en klik op **OK**.
6. Klik op **instellen van deze id-provider** en voer de client-ID en clientgeheim van de Amazon-toepassing die u eerder hebt gemaakt.
7. Klik op **OK** en klik vervolgens op **maken** naar uw Amazon-configuratie op te slaan.

