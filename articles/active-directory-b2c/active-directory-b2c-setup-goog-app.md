---
title: Google + configuratie in Azure Active Directory B2C | Microsoft Docs
description: Bieden zich kunnen registreren en aanmelden voor consumenten met Google + accounts in uw toepassingen die zijn beveiligd met Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/06/2016
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: fe5722e8d5a0a8a5bf172577777ccb899fb7b94d
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37443423"
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-google-accounts"></a>Azure Active Directory B2C: Bieden zich kunnen registreren en aanmelden voor consumenten met Google + accounts
## <a name="create-a-google-application"></a>Een Google +-toepassing maken
Voor het gebruik van Google + als id-provider in Azure Active Directory (Azure AD) B2C, moet u een Google +-toepassing maken en geven met de juiste parameters. U moet een Google +-account om dit te doen. Als u niet hebt, kunt u krijgen via [ https://accounts.google.com/SignUp ](https://accounts.google.com/SignUp).

1. Ga naar de [Google ontwikkelaars Console](https://console.developers.google.com/) en meld u aan met de referenties van uw Google + account.
2. Klik op **project maken**, voer een **projectnaam**, en klik vervolgens op **maken**.
   
    ![Google + - aan de slag](./media/active-directory-b2c-setup-goog-app/google-get-started.png)
   
    ![Google + - nieuw project](./media/active-directory-b2c-setup-goog-app/google-new-project.png)
3. Klik op **API Manager** en klik vervolgens op **referenties** in het linkernavigatievenster.
4. Klik op de **OAuth instemmingsscherm** tabblad bovenaan.
   
    ![Google + - referenties](./media/active-directory-b2c-setup-goog-app/google-add-cred.png)
5. Selecteer of geef een geldige **e-mailadres**, bieden een **Productnaam**, en klikt u op **opslaan**.
   
    ![Google + - OAuth-toestemmingsscherm](./media/active-directory-b2c-setup-goog-app/google-consent-screen.png)
6. Klik op **nieuwe referenties** en kies vervolgens **OAuth-Clientidentiteit**.
   
    ![Google + - OAuth-toestemmingsscherm](./media/active-directory-b2c-setup-goog-app/google-add-oauth2-client-id.png)
7. Onder **toepassingstype**, selecteer **webtoepassing**.
   
    ![Google + - OAuth-toestemmingsscherm](./media/active-directory-b2c-setup-goog-app/google-web-app.png)
8. Geef een **naam** voor uw toepassing, voert u `https://login.microsoftonline.com` in de **geautoriseerd JavaScript oorsprongen** veld en `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in de **geautoriseerde omleidings-URI's** het veld. Vervang **{tenant}** met de naam van uw tenant (bijvoorbeeld: contosob2c.onmicrosoft.com). De **{tenant}** hoofdlettergevoelig. Klik op **Create**.
   
    ![Google + - client-ID maken](./media/active-directory-b2c-setup-goog-app/google-create-client-id.png)
9. Kopieer de waarden van **Client-ID** en **clientgeheim**. U moet beide Google + als id-provider in uw tenant worden geconfigureerd. **Clientgeheim** is een belangrijke beveiligingsreferentie.
   
    ![Google + - clientgeheim](./media/active-directory-b2c-setup-goog-app/google-client-secret.png)

## <a name="configure-google-as-an-identity-provider-in-your-tenant"></a>Configureren van Google + als id-provider in uw tenant
1. Volg deze stappen om [gaat u naar de blade B2C-functies](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) in Azure portal.
2. Klik op de blade B2C-functies **id-providers**.
3. Klik op **+Toevoegen** boven aan de blade.
4. Geef een beschrijvende **naam** voor de configuratie van de id-provider. Voer bijvoorbeeld "G +".
5. Klik op **type id-provider**, selecteer **Google**, en klikt u op **OK**.
6. Klik op **instellen van deze id-provider** en voer de client-ID en clientgeheim van de Google +-toepassing die u eerder hebt gemaakt.
7. Klik op **OK** en klik vervolgens op **maken** aan uw Google +-configuratie op te slaan.

