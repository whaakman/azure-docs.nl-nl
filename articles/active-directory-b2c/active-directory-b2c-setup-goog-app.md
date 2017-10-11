---
title: 'Azure Active Directory B2C: Google + configuratie | Microsoft Docs'
description: Registreren en aanmelden gebruikers met Google + accounts in uw toepassingen die zijn beveiligd met Azure Active Directory B2C bieden.
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: 4dcca66f-29e4-4b4d-8840-50baad736bd7
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: 6ab73e5c79742ab548733f5712dee1e28461db9f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-google-accounts"></a>Azure Active Directory B2C: Zich kunnen registreren en aanmelden gebruikers bieden met Google + accounts
## <a name="create-a-google-application"></a>Een Google +-toepassing maken
Voor het gebruik van Google + als een id-provider in Azure Active Directory (Azure AD) B2C, moet u een Google +-toepassing maken en geeft deze met de juiste parameters. U moet een account Google + om dit te doen. Als u niet hebt, kunt u krijgen op het [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp).

1. Ga naar de [Google ontwikkelaars Console](https://console.developers.google.com/) en meld u aan met de referenties van uw Google + account.
2. Klik op **project maken**, voer een **projectnaam**, en klik vervolgens op **maken**.
   
    ![Google + - aan de slag](./media/active-directory-b2c-setup-goog-app/google-get-started.png)
   
    ![Google + - nieuw project](./media/active-directory-b2c-setup-goog-app/google-new-project.png)
3. Klik op **API Manager** en klik vervolgens op **referenties** in de linkernavigatiebalk.
4. Klik op de **OAuth toestemming scherm** boven op het tabblad.
   
    ![Google + - referenties](./media/active-directory-b2c-setup-goog-app/google-add-cred.png)
5. Selecteer of geef een geldige **e-mailadres**, bieden een **Productnaam**, en klik op **opslaan**.
   
    ![Google + - OAuth toestemming scherm](./media/active-directory-b2c-setup-goog-app/google-consent-screen.png)
6. Klik op **nieuwe referenties** en kies vervolgens **OAuth-Clientidentiteit**.
   
    ![Google + - OAuth toestemming scherm](./media/active-directory-b2c-setup-goog-app/google-add-oauth2-client-id.png)
7. Onder **toepassingstype**, selecteer **webtoepassing**.
   
    ![Google + - OAuth toestemming scherm](./media/active-directory-b2c-setup-goog-app/google-web-app.png)
8. Geef een **naam** voor uw toepassing, voert u `https://login.microsoftonline.com` in de **geautoriseerd JavaScript oorsprongen** veld en `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in de **geautoriseerde omleidings-URI's** veld. Vervang **{tenant}** met de naam van uw tenant (bijvoorbeeld contosob2c.onmicrosoft.com). De **{tenant}** hoofdlettergevoelig. Klik op **Create**.
   
    ![Google + - client-ID maken](./media/active-directory-b2c-setup-goog-app/google-create-client-id.png)
9. Kopieer de waarden van **Client-ID** en **clientgeheim**. U moet beide parameters voor het configureren van Google + als een id-provider in uw tenant. **Clientgeheim** is een belangrijke beveiligingsreferentie.
   
    ![Google + - clientgeheim](./media/active-directory-b2c-setup-goog-app/google-client-secret.png)

## <a name="configure-google-as-an-identity-provider-in-your-tenant"></a>Google + als een id-provider in uw tenant configureren
1. Volg deze stappen voor [gaat u naar de blade B2C-functies](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) in de Azure portal.
2. Klik op de blade B2C-functies op **identiteitsproviders**.
3. Klik op **+Toevoegen** boven aan de blade.
4. Geef een beschrijvende **naam** voor de configuratie van de id-provider. Voer bijvoorbeeld "G +".
5. Klik op **identiteit providertype**, selecteer **Google**, en klik op **OK**.
6. Klik op **instellen van deze id-provider** en voer de client-ID en clientgeheim van de Google +-toepassing die u eerder hebt gemaakt.
7. Klik op **OK** en klik vervolgens op **maken** voor uw Google +-configuratie op te slaan.

