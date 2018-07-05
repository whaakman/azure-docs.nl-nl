---
title: Facebook-configuratie in Azure Active Directory B2C | Microsoft Docs
description: Meld u aan en meld u bieden voor gebruikers met een Facebook-account in uw toepassingen die zijn beveiligd met Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 8/7/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 316e44ea92a25ab804c8cc499f91c45e4a66ef02
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37445497"
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-facebook-accounts"></a>Azure Active Directory B2C: Bieden zich kunnen registreren en aanmelden voor consumenten met Facebook-accounts
## <a name="create-a-facebook-application"></a>Een Facebook-toepassing maken
Voor het gebruik van Facebook als id-provider in Azure Active Directory (Azure AD) B2C, moet u een Facebook-toepassing maken en geven met de juiste parameters. U moet een Facebook-account om dit te doen. Als u niet hebt, kunt u krijgen via [ https://www.facebook.com/ ](https://www.facebook.com/).

1. Ga naar de [Facebook voor ontwikkelaars](https://developers.facebook.com/) website en meld u aan met uw Facebook-accountreferenties.
2. Als u hebt nog niet gedaan, moet u registreren als een Facebook-ontwikkelaar. Om dit te doen, klikt u op **registreren** (in de rechterbovenhoek van de pagina), accepteren van Facebook-beleid en de registratie van stappen.
3. Klik op **mijn Apps** en klik vervolgens op **toevoegen van een nieuwe App**. 
4. Geef in het formulier een **weergavenaam** en een geldig **Neem contact op met e-mailadres**.
5. Klik op **maken van App-ID**. Mogelijk moet u beleid voor het platform van Facebook accepteren en een online beveiliging voltooien.
6. Klik in de linkerkolom op **instellingen** en selecteer vervolgens **Basic** als niet is ingeschakeld.
7. Selecteer een **categorie**. 
8. Klik op **+ toevoegen Platform** en selecteer **Website**.
   
    ![Facebook - instellingen](./media/active-directory-b2c-setup-fb-app/fb-settings.png)
   
    ![Facebook - Settings - Website](./media/active-directory-b2c-setup-fb-app/fb-website.png)
9. Voer `https://login.microsoftonline.com/` in de **Site-URL** veld en klik vervolgens op **wijzigingen opslaan** aan de onderkant van de pagina.
   
    ![Facebook - Site-URL](./media/active-directory-b2c-setup-fb-app/fb-site-url.png)

10. Kopieer de waarde van **App-ID**. Klik op **weergeven** en kopieer de waarde van **Appgeheim**. U moet beide Facebook configureren als een id-provider in uw tenant. **App-geheim** is een belangrijke beveiligingsreferentie.
   
    ![Facebook - App-ID & App-geheim](./media/active-directory-b2c-setup-fb-app/fb-app-id-app-secret.png)
11. Klik op **+ Product toevoegen** in de Linkernavigatie en vervolgens de **instellen** voor knop **aanmelden via Facebook**.
   
    ![Facebook - aanmelding via Facebook](./media/active-directory-b2c-setup-fb-app/fb-login.png)
12. Klik op **instellingen** in het navigatievenster rechts onder **aanmelden via Facebook**

    ![Facebook - Facebook Login settings](./media/active-directory-b2c-setup-fb-app/fb-login-settings.png)
13. Voer `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in de **geldig OAuth omleidings-URI's** veld in de **OAuth-clientinstellingen** sectie. Vervang **{tenant}** met de naam van uw tenant (bijvoorbeeld: contosob2c.onmicrosoft.com). Klik op **wijzigingen opslaan** aan de onderkant van de pagina.
    
    ![Facebook - OAuth omleidings-URI](./media/active-directory-b2c-setup-fb-app/fb-oauth-redirect-uri.png)
14. Als u uw Facebook-toepassing kan worden gebruikt door Azure AD B2C, moet u dat openbaar beschikbaar. U kunt dit doen door te klikken op **App-revisie** in de Linkernavigatie en door het inschakelen van de switch aan de bovenkant van de pagina **Ja** en te klikken op **bevestigen**.
    
    ![Facebook - App openbaar](./media/active-directory-b2c-setup-fb-app/fb-app-public.png)

## <a name="configure-facebook-as-an-identity-provider-in-your-tenant"></a>Facebook configureren als een id-provider in uw tenant
1. Volg deze stappen om [gaat u naar de blade B2C-functies](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) in Azure portal.
2. Klik op de blade B2C-functies **id-providers**.
3. Klik op **+Toevoegen** boven aan de blade.
4. Geef een beschrijvende **naam** voor de configuratie van de id-provider. Voer bijvoorbeeld 'Facebook'.
5. Klik op **type id-provider**, selecteer **Facebook**, en klikt u op **OK**.
6. Klik op **instellen van deze id-provider** en voer de app-ID en app-geheim (van de Facebook-toepassing die u eerder hebt gemaakt) in de **Client-ID** en **clientgeheim** respectievelijk de velden.
7. Klik op **OK**, en klik vervolgens op **maken** op uw Facebook-configuratie op te slaan.

> [!NOTE]
> Toevoegen van een **id-provider** aan uw tenant uw bestaande beleidsregels niet wijzigen. Vergeet niet om uw beleid bijwerken door op te nemen van de id-provider die u zojuist hebt gemaakt.
>