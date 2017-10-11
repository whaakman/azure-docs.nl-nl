---
title: 'Azure Active Directory B2C: Facebook-configuratie | Microsoft Docs'
description: Registreren en aanmelden gebruikers met Facebook-accounts in uw toepassingen die zijn beveiligd met Azure Active Directory B2C bieden.
services: active-directory-b2c
documentationcenter: 
author: sromeroz
manager: krassk
editor: sromeroz
ms.assetid: b875f235-a1d2-4abb-b9f0-b89beac38a32
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/7/2017
ms.author: sromeroz
ms.openlocfilehash: 8c2154fcf33537358b549395d15b4ba937371cd0
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-facebook-accounts"></a>Azure Active Directory B2C: Zich kunnen registreren en aanmelden gebruikers bieden met Facebook-accounts
## <a name="create-a-facebook-application"></a>Een Facebook-toepassing maken
Als u wilt gebruiken als een id-provider in Azure Active Directory (Azure AD) B2C Facebook gebruikt, moet u een Facebook-toepassing maken en geeft deze met de juiste parameters. U moet een Facebook-account om dit te doen. Als u niet hebt, kunt u krijgen op het [https://www.facebook.com/](https://www.facebook.com/).

1. Ga naar de [Facebook voor ontwikkelaars](https://developers.facebook.com/) website en meld u aan met uw Facebook-accountreferenties.
2. Als u dit nog niet hebt gedaan, moet u registreren als een ontwikkelaar Facebook. Om dit te doen, klikt u op **registreren** (in de rechterbovenhoek van de pagina), accepteert de Facebook-beleid en voltooi de stappen registratie.
3. Klik op **mijn Apps** en klik vervolgens op **toevoegen van een nieuwe App**. 
4. Geef in het formulier een **weergavenaam** en een geldig **Neem contact op met E-mail**.
5. Klik op **maken van App-ID**. Mogelijk moet u Facebook platform beleidsregels accepteren en een online security-controle voltooien.
6. Klik in de linkerkolom **instellingen** en selecteer vervolgens **Basic** als niet al is ingeschakeld.
7. Selecteer een **categorie**. 
8. Klik op **+ toevoegen Platform** en selecteer **Website**.
   
    ![Facebook - instellingen](./media/active-directory-b2c-setup-fb-app/fb-settings.png)
   
    ![Facebook - instellingen - Website](./media/active-directory-b2c-setup-fb-app/fb-website.png)
9. Voer `https://login.microsoftonline.com/` in de **Site-URL** veld en klik vervolgens op **wijzigingen opslaan** aan de onderkant van de pagina.
   
    ![Facebook - Site-URL](./media/active-directory-b2c-setup-fb-app/fb-site-url.png)

10. Kopieer de waarde van **App-ID**. Klik op **weergeven** en kopieer de waarde van **App geheim**. U moet beide Facebook configureren als een id-provider in uw tenant. **App-geheim** is een belangrijke beveiligingsreferentie.
   
    ![Facebook - App-ID en App-geheim](./media/active-directory-b2c-setup-fb-app/fb-app-id-app-secret.png)
11. Klik op **+ Product toevoegen** voor het linkernavigatiegedeelte en vervolgens de **instellen** knop voor **Facebook aanmelding**.
   
    ![Facebook - Facebook-aanmelding](./media/active-directory-b2c-setup-fb-app/fb-login.png)
12. Klik op **instellingen** op de juiste nav onder **Facebook-aanmelding**

    ![Facebook - instellingen voor Facebook-aanmelding](./media/active-directory-b2c-setup-fb-app/fb-login-settings.png)
13. Voer `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in de **geldig OAuth omleidings-URI's** veld in de **OAuth clientinstellingen** sectie. Vervang **{tenant}** met de naam van uw tenant (bijvoorbeeld contosob2c.onmicrosoft.com). Klik op **wijzigingen opslaan** aan de onderkant van de pagina.
    
    ![Facebook - OAuth omleidings-URI](./media/active-directory-b2c-setup-fb-app/fb-oauth-redirect-uri.png)
14. Als u uw Facebook-toepassing gebruikt door Azure AD B2C, moet u het openbaar beschikbaar maken. U kunt dit doen door te klikken op **App revisie** voor het linkernavigatiegedeelte en door het inschakelen van de switch aan de bovenkant van de pagina **Ja** en te klikken op **bevestigen**.
    
    ![Facebook - App openbare](./media/active-directory-b2c-setup-fb-app/fb-app-public.png)

## <a name="configure-facebook-as-an-identity-provider-in-your-tenant"></a>Facebook configureren als een id-provider in uw tenant
1. Volg deze stappen voor [gaat u naar de blade B2C-functies](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) in de Azure portal.
2. Klik op de blade B2C-functies op **identiteitsproviders**.
3. Klik op **+Toevoegen** boven aan de blade.
4. Geef een beschrijvende **naam** voor de configuratie van de id-provider. Voer bijvoorbeeld 'Facebook'.
5. Klik op **identiteit providertype**, selecteer **Facebook**, en klik op **OK**.
6. Klik op **instellen van deze id-provider** en voer de app-ID en app-geheim (van de Facebook-toepassing die u eerder hebt gemaakt) in de **Client-ID** en **clientgeheim** respectievelijk de velden.
7. Klik op **OK**, en klik vervolgens op **maken** naar uw Facebook-configuratie op te slaan.

> [!NOTE]
> Toevoegen van een **identiteitsprovider** in uw tenant, uw bestaande beleidsregels niet wijzigen. Vergeet niet uw beleidsregels bijwerken door de identiteitsprovider die u zojuist hebt gemaakt.
>