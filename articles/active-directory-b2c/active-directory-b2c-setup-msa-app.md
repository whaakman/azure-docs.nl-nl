---
title: Configuratie van de Microsoft-account in Azure Active Directory B2C | Microsoft Docs
description: Meld u aan en meld u bieden voor gebruikers met een Microsoft-account in uw toepassingen die zijn beveiligd met Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/06/2016
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: c788b14a99125a208390cd4f8ead338efed06933
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37444164"
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-microsoft-accounts"></a>Azure Active Directory B2C: Bieden zich kunnen registreren en aanmelden voor consumenten met Microsoft-accounts
## <a name="create-a-microsoft-account-application"></a>Een toepassing van Microsoft-account maken
Voor het gebruik van Microsoft-account als id-provider in Azure Active Directory (Azure AD) B2C, moet u een toepassing van Microsoft-account maken en geven met de juiste parameters. U moet een Microsoft-account om dit te doen. Als u niet hebt, kunt u krijgen via [ https://www.live.com/ ](https://www.live.com/).

1. Ga naar de [Portal voor Appregistratie Microsoft](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) en meld u aan met uw Microsoft-accountreferenties.
2. Klik op **een app toevoegen**.
   
    ![Microsoft-account: een nieuwe app toevoegen](./media/active-directory-b2c-setup-msa-app/msa-add-new-app.png)
3. Geef een **naam** voor uw toepassing en klik op **-toepassing maken**.
   
    ![Microsoft-account - App-naam](./media/active-directory-b2c-setup-msa-app/msa-app-name.png)
4. Kopieer de waarde van **toepassings-Id**. U moet het Microsoft-account configureren als een id-provider in uw tenant.
   
    ![Microsoft-account - toepassings-Id](./media/active-directory-b2c-setup-msa-app/msa-app-id.png)
5. Klik op **toevoegen platform** en kies **Web**.
   
    ![Microsoft-account - platform toevoegen](./media/active-directory-b2c-setup-msa-app/msa-add-platform.png)
   
    ![Microsoft-account - Web](./media/active-directory-b2c-setup-msa-app/msa-web.png)
6. Voer `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in de **omleidings-URI's** veld. Vervang **{tenant}** met de naam van uw tenant (bijvoorbeeld: contosob2c.onmicrosoft.com).
   
    ![Microsoft-account - Omleidings-URL](./media/active-directory-b2c-setup-msa-app/msa-redirect-url.png)
7. Klik op **nieuw wachtwoord genereren** onder de **Toepassingsgeheimen** sectie. Kopieer het nieuwe wachtwoord weergegeven op het scherm. U moet het Microsoft-account configureren als een id-provider in uw tenant. Dit wachtwoord is een belangrijke beveiligingsreferentie.
   
    ![Microsoft-account: nieuw wachtwoord genereren](./media/active-directory-b2c-setup-msa-app/msa-generate-new-password.png)
   
    ![Microsoft-account - nieuw wachtwoord](./media/active-directory-b2c-setup-msa-app/msa-new-password.png)
8. Schakel het selectievakje in met de melding dat **Live SDK-ondersteuning** onder de **geavanceerde opties** sectie. Klik op **Opslaan**.
   
    ![Microsoft-account - ondersteuning voor Live SDK](./media/active-directory-b2c-setup-msa-app/msa-live-sdk-support.png)

## <a name="configure-microsoft-account-as-an-identity-provider-in-your-tenant"></a>Microsoft-account configureren als een id-provider in uw tenant
1. Volg deze stappen om [gaat u naar de blade B2C-functies](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) in Azure portal.
2. Klik op de blade B2C-functies **id-providers**.
3. Klik op **+Toevoegen** boven aan de blade.
4. Geef een beschrijvende **naam** voor de configuratie van de id-provider. Voer bijvoorbeeld 'MSA'.
5. Klik op **type id-provider**, selecteer **Microsoft-account**, en klikt u op **OK**.
6. Klik op **instellen van deze id-provider** en voer de toepassings-Id en het wachtwoord van de Microsoft-account-toepassing die u eerder hebt gemaakt.
7. Klik op **OK** en klik vervolgens op **maken** om op te slaan van de configuratie van uw Microsoft-account.

