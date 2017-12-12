---
title: 'Azure Active Directory B2C: Configuratie Microsoft-account | Microsoft Docs'
description: Registreren en aanmelden gebruikers met een Microsoft-account in uw toepassingen die zijn beveiligd met Azure Active Directory B2C bieden.
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mtillman
editor: bryanla
ms.assetid: 06407322-142c-4cb3-9106-a8d752c4c853
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: e746a5e7a2d9a13eb23ac0268104a9394c9b198d
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-microsoft-accounts"></a>Azure Active Directory B2C: Zich kunnen registreren en aanmelden gebruikers bieden met Microsoft-accounts
## <a name="create-a-microsoft-account-application"></a>Een toepassing van Microsoft-account maken
Voor het gebruik van Microsoft-account als een id-provider in Azure Active Directory (Azure AD) B2C, moet u een toepassing van Microsoft-account maken en geeft deze met de juiste parameters. U moet een Microsoft-account om dit te doen. Als u niet hebt, kunt u krijgen op het [https://www.live.com/](https://www.live.com/).

1. Ga naar de [Portal voor registratie van Microsoft-toepassing](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) en meld u aan met uw Microsoft-accountreferenties.
2. Klik op **een app toevoegen**.
   
    ![Microsoft-account: een nieuwe app toevoegen](./media/active-directory-b2c-setup-msa-app/msa-add-new-app.png)
3. Geef een **naam** voor uw toepassing en klik op **-toepassing maken**.
   
    ![Microsoft-account - App-naam](./media/active-directory-b2c-setup-msa-app/msa-app-name.png)
4. Kopieer de waarde van **toepassings-Id**. U hebt deze Microsoft-account configureren als een id-provider in uw tenant.
   
    ![Microsoft-account - toepassings-Id](./media/active-directory-b2c-setup-msa-app/msa-app-id.png)
5. Klik op **toevoegen platform** en kies **Web**.
   
    ![Microsoft-account - platform toevoegen](./media/active-directory-b2c-setup-msa-app/msa-add-platform.png)
   
    ![Microsoft-account - webservice](./media/active-directory-b2c-setup-msa-app/msa-web.png)
6. Voer `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in de **omleidings-URI's** veld. Vervang **{tenant}** met de naam van uw tenant (bijvoorbeeld contosob2c.onmicrosoft.com).
   
    ![Microsoft-account - Omleidings-URL](./media/active-directory-b2c-setup-msa-app/msa-redirect-url.png)
7. Klik op **nieuw wachtwoord genereren** onder de **toepassing geheimen** sectie. Kopieer het nieuwe wachtwoord op het scherm wordt weergegeven. U hebt deze Microsoft-account configureren als een id-provider in uw tenant. Dit wachtwoord is een belangrijke beveiligingsreferentie.
   
    ![Microsoft-account: nieuw wachtwoord genereren](./media/active-directory-b2c-setup-msa-app/msa-generate-new-password.png)
   
    ![Microsoft-account - nieuw wachtwoord](./media/active-directory-b2c-setup-msa-app/msa-new-password.png)
8. Schakel het selectievakje waarin staat dat **ondersteuning voor Live SDK** onder de **geavanceerde opties** sectie. Klik op **Opslaan**.
   
    ![Microsoft-account - ondersteuning voor Live SDK](./media/active-directory-b2c-setup-msa-app/msa-live-sdk-support.png)

## <a name="configure-microsoft-account-as-an-identity-provider-in-your-tenant"></a>Microsoft-account configureren als een id-provider in uw tenant
1. Volg deze stappen voor [gaat u naar de blade B2C-functies](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) in de Azure portal.
2. Klik op de blade B2C-functies op **identiteitsproviders**.
3. Klik op **+Toevoegen** boven aan de blade.
4. Geef een beschrijvende **naam** voor de configuratie van de id-provider. Voer bijvoorbeeld 'MSA'.
5. Klik op **identiteit providertype**, selecteer **Microsoft-account**, en klik op **OK**.
6. Klik op **instellen van deze id-provider** en voer de toepassings-Id en het wachtwoord van de Microsoft-account-toepassing die u eerder hebt gemaakt.
7. Klik op **OK** en klik vervolgens op **maken** om op te slaan van de configuratie van uw Microsoft-account.

