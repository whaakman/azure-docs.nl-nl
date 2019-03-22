---
title: Zelfstudie - id-providers toevoegen aan uw toepassingen - Azure Active Directory B2C | Microsoft Docs
description: Informatie over het id-providers toevoegen aan uw toepassingen in Azure Active Directory B2C met behulp van de Azure portal.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 02/01/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: ef264622be1e66c98f8c7f17cf3e04b3239e8553
ms.sourcegitcommit: 02d17ef9aff49423bef5b322a9315f7eab86d8ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58338272"
---
# <a name="tutorial-add-identity-providers-to-your-applications-in-azure-active-directory-b2c"></a>Zelfstudie: Id-providers toevoegen aan uw toepassingen in Azure Active Directory B2C

Het is wellicht wenselijk dat uw gebruikers zich in uw toepassingen bij verschillende id-providers kunnen aanmelden. Een *id-provider* maakt, onderhoudt en beheert id-gegevens en biedt tegelijkertijd verificatieservices voor toepassingen. U kunt toevoegen id-providers die worden ondersteund door Azure Active Directory (Azure AD) B2C aan uw [gebruikersstromen](active-directory-b2c-reference-policies.md) met behulp van de Azure portal.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * De identiteit van de provider-toepassingen maken
> * De id-providers toevoegen aan uw tenant
> * De id-providers toevoegen aan de gebruikersstroom

Doorgaans gebruikt u slechts één id-provider in uw toepassingen, maar u hebt de mogelijkheid om toe te voegen meer. Deze zelfstudie leert u hoe u een Azure AD-id-provider en een Facebook-id-provider toevoegt aan uw toepassing. Beide van deze id-providers toevoegen aan uw toepassing is optioneel. U kunt ook andere id-providers, zoals toevoegen [Amazon](active-directory-b2c-setup-amzn-app.md), [Github](active-directory-b2c-setup-github-app.md), [Google](active-directory-b2c-setup-goog-app.md), [LinkedIn](active-directory-b2c-setup-li-app.md), [Microsoft](active-directory-b2c-setup-msa-app.md), of [Twitter](active-directory-b2c-setup-twitter-app.md). 

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

[Een gebruikersstroom maken](tutorial-create-user-flows.md) zodat gebruikers kunnen zich registreren en aanmelden bij uw toepassing. 

## <a name="create-applications"></a>Toepassingen maken

Id-provider toepassingen bieden de id en de sleutel voor het inschakelen van de communicatie met uw Azure AD B2C-tenant. In deze sectie van de zelfstudie maakt u een Azure AD-toepassing en een Facebook-toepassing van waaruit u profiteert van id's en sleutels de id-providers toevoegen aan uw tenant. Als u slechts een van de id-providers toevoegen wilt, moet u alleen de toepassing voor die provider maken.

### <a name="create-an-azure-active-directory-application"></a>Een Azure Active Directory-toepassing maken

Om in te schakelen aanmelding voor gebruikers van Azure AD, moet u een toepassing registreren met de Azure AD-tenant. De Azure AD-tenant is niet gelijk zijn aan uw Azure AD B2C-tenant.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Zorg ervoor dat u de map met uw Azure AD-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map met uw Azure AD-tenant te kiezen.
3. Kies **alle services** in de linkerbovenhoek van de Azure portal en vervolgens zoeken naar en selecteer **App-registraties**.
4. Selecteer **Nieuwe toepassing registreren**.
5. Voer een naam in voor de toepassing. Bijvoorbeeld `Azure AD B2C App`.
6. Voor de **toepassingstype**, selecteer `Web app / API`.
7. Voor de **aanmeldings-URL**, voer de volgende URL in kleine letters, waarbij `your-B2C-tenant-name` wordt vervangen door de naam van uw Azure AD B2C-tenant.

    ```
    https://your-B2C-tenant-name.b2clogin.com/your-B2C-tenant-name.onmicrosoft.com/oauth2/authresp
    ```
    
    Bijvoorbeeld `https://contoso.b2clogin.com/contoso.onmicrosoft.com/oauth2/authresp`.
    
    Alle URL's moeten nu gebruikmaken van [b2clogin.com](b2clogin.md).

8. Klik op **Create**. Kopieer de **toepassings-ID** moet later worden gebruikt.
9. Selecteer de toepassing en selecteer vervolgens **instellingen**.
10. Selecteer **sleutels**, geef de beschrijving van de sleutel, selecteer een tijdsduur en klik vervolgens op **opslaan**. Kopieer de waarde van de sleutel zodat u deze later in deze zelfstudie kunt gebruiken.

### <a name="create-a-facebook-application"></a>Een Facebook-toepassing maken

Voor het gebruik van een Facebook-account als id-provider in Azure AD B2C, moet u een toepassing maakt op Facebook. Als u nog een Facebook-account hebt, kunt u krijgen via [ https://www.facebook.com/ ](https://www.facebook.com/).

1. Aanmelden bij [Facebook voor ontwikkelaars](https://developers.facebook.com/) met de referenties van uw Facebook-account.
2. Als u dit nog niet hebt gedaan, moet u registreren als een Facebook-ontwikkelaar. Als u wilt registreren, selecteert u **registreren** in de rechterbovenhoek van de pagina accepteren van Facebook-beleid en de registratie van stappen.
3. Selecteer **mijn Apps** en klik vervolgens op **toevoegen van een nieuwe App**. 
4. Voer een **weergavenaam** en een geldig **Neem contact op met e-mailadres**.
5. Klik op **maken van App-ID**. Mogelijk moet u beleid voor het platform van Facebook accepteren en een online beveiliging voltooien.
6. Selecteer **instellingen** > **Basic**.
7. Kies een **categorie**, bijvoorbeeld `Business and Pages`. Deze waarde is vereist voor Facebook, maar niet voor Azure AD B2C gebruikt.
8. Selecteer aan de onderkant van de pagina **Platform toevoegen**, en selecteer vervolgens **Website**.
9. In **Site-URL**, voer `https://your-tenant-name.b2clogin.com/` vervangen `your-tenant-name` met de naam van uw tenant. Voer een URL voor de **URL privacybeleid**, bijvoorbeeld `http://www.contoso.com`. De beleids-URL is een pagina die u moet onderhouden voor privacy-informatie voor uw toepassing.
10. Selecteer **Save changes**.
11. Aan de bovenkant van de pagina, Kopieer de waarde van **App-ID**. 
12. Klik op **weergeven** en kopieer de waarde van **Appgeheim**. U kunt van beide Facebook als id-provider configureren in uw tenant. **App-geheim** is een belangrijke beveiligingsreferentie.
13. Selecteer **producten**, en selecteer vervolgens **instellen** onder **aanmelden via Facebook**.
14. Selecteer **instellingen** onder **aanmelden via Facebook**.
15. In **geldig OAuth omleidings-URI's**, voer `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp`. Vervang `your-tenant-name` met de naam van uw tenant. Klik op **wijzigingen opslaan** aan de onderkant van de pagina.
16. Als u uw Facebook-toepassing naar Azure AD B2C, klikt u op de **Status** selector aan de bovenkant van de pagina en stel deze in op **op**. Klik op **Bevestigen**. Op dit moment de Status te wijzigen van **ontwikkeling** naar **Live**.

## <a name="add-the-identity-providers"></a>De id-providers toevoegen

Nadat u de toepassing voor de id-provider die u wilt toevoegen hebt gemaakt, kunt u de id-provider toevoegen aan uw tenant.

### <a name="add-the-azure-active-directory-identity-provider"></a>De Azure Active Directory-id-provider toevoegen

1. Zorg ervoor dat u de map met uw Azure AD B2C-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map met uw Azure AD B2C-tenant te kiezen.
2. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
3. Selecteer **id-providers**, en selecteer vervolgens **toevoegen**.
4. Voer een **naam**. Voer bijvoorbeeld *Contoso Azure AD*.
5. Selecteer **type id-provider**, selecteer **Open ID Connect (Preview)**, en klik vervolgens op **OK**.
6. Klik op **deze id-provider instellen**
7. Voor **metagegevens-url**, voer de volgende URL vervangt `your-AD-tenant-domain` met de domeinnaam van uw Azure AD-tenant.

    ```
    https://login.microsoftonline.com/your-AD-tenant-domain/.well-known/openid-configuration
    ```

    Bijvoorbeeld `https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration`.

8. Voor **Client-ID**, voer de toepassings-ID die u eerder hebt genoteerd en voor **clientgeheim**, voer de waarde van de sleutel die u eerder hebt genoteerd.
9. Geef eventueel een waarde voor **Domain_hint**. Bijvoorbeeld `ContosoAD`. 
10. Klik op **OK**.
11. Selecteer **deze id-provider claims toewijzen** en stel de volgende claims:
    
    - Voor **gebruikers-ID**, voer `oid`.
    - Voor **weergavenaam**, voer `name`.
    - Voor **voornaam**, voer `given_name`.
    - Voor **achternaam**, voer `family_name`.
    - Voor **e**, voer `unique_name`.

12. Klik op **OK**, en klik vervolgens op **maken** aan uw configuratie op te slaan.

### <a name="add-the-facebook-identity-provider"></a>De Facebook-id-provider toevoegen

1. Selecteer **id-providers**, en selecteer vervolgens **toevoegen**.
2. Voer een **naam**. Voer bijvoorbeeld *Facebook*.
3. Selecteer **type id-provider**, selecteer **Facebook**, en klikt u op **OK**.
4. Selecteer **instellen van deze id-provider** en voert u de App-ID die u eerder hebt genoteerd als de **Client-ID**. Voer de App-geheim die u hebt genoteerd als de **clientgeheim**.
5. Klik op **OK** en klik vervolgens op **maken** op uw Facebook-configuratie op te slaan.

## <a name="update-the-user-flow"></a>De gebruikersstroom bijwerken

In de zelfstudie die u als onderdeel van de vereisten hebt voltooid, hebt u een gebruikersstroom gemaakt voor aanmelden en meld u met de naam *B2C_1_signupsignin1*. In deze sectie maakt u de id-providers te toevoegen de *B2C_1_signupsignin1* gebruikersstroom.

1. Selecteer **gebruikersstromen (beleid)**, en selecteer vervolgens de *B2C_1_signupsignin1* gebruikersstroom.
2. Selecteer **id-providers**, selecteer de **Facebook** en **Contoso Azure AD** id-providers die u hebt toegevoegd.
3. Selecteer **Opslaan**.

### <a name="test-the-user-flow"></a>De gebruikersstroom testen

1. Selecteer op de pagina overzicht van de gebruikersstroom die u hebt gemaakt, **gebruikersstroom uitvoeren**.
2. Voor **toepassing**, selecteert u de web-App met de naam *webapp1* die u eerder hebt geregistreerd. De **antwoord-URL** moet worden weergegeven `https://jwt.ms`.
3. Klik op **gebruikersstroom uitvoeren**, en vervolgens Meld u aan met een id-provider die u eerder hebt toegevoegd.
4. Herhaal stappen 1 t/m 3 voor de andere id-providers die u hebt toegevoegd.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u het volgende geleerd:

> [!div class="checklist"]
> * De identiteit van de provider-toepassingen maken
> * De id-providers toevoegen aan uw tenant
> * De id-providers toevoegen aan de gebruikersstroom

> [!div class="nextstepaction"]
> [De gebruikersinterface van uw toepassingen in Azure Active Directory B2C aanpassen](tutorial-customize-ui.md)