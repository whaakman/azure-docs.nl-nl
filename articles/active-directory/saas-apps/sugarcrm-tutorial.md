---
title: 'Zelfstudie: Azure Active Directory-integratie met CRM suiker | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en suiker CRM.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 3331b9fc-ebc0-4a3a-9f7b-bf20ee35d180
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/22/2019
ms.author: jeedes
ms.openlocfilehash: 2dea1dcd2f6ecef580d65a95d1227380901213eb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "65866453"
---
# <a name="tutorial-azure-active-directory-integration-with-sugar-crm"></a>Zelfstudie: Azure Active Directory-integratie met CRM suiker

In deze zelfstudie leert u hoe u suiker CRM integreren met Azure Active Directory (Azure AD).
Suiker CRM integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot suiker CRM heeft.
* U kunt uw gebruikers worden automatisch aangemeld suiker CRM (Single Sign-On) inschakelen met hun Azure AD-accounts.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met CRM suiker, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* Suiker CRM eenmalige aanmelding ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* CRM ondersteunt suiker **SP** gestart door SSO

## <a name="adding-sugar-crm-from-the-gallery"></a>Suiker CRM uit de galerie toe te voegen

Voor het configureren van de integratie van suiker CRM in Azure AD, moet u suiker CRM uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen suiker CRM vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **suiker CRM**, selecteer **suiker CRM** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![Suiker CRM in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met suiker CRM op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in suiker CRM tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met suiker CRM, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureer suiker CRM Single Sign-On](#configure-sugar-crm-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maken van de testgebruiker suiker CRM](#create-sugar-crm-test-user)**  : als u wilt een equivalent van Britta Simon in suiker CRM die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met suiker CRM, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **suiker CRM** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Suiker CRM-domein en URL's, eenmalige aanmelding informatie](common/sp-signonurl.png)

    Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon:
    
    | |
    |--|
    | `https://<companyname>.sugarondemand.com`|
    | `https://<companyname>.trial.sugarcrm`|

    > [!NOTE]
    > De waarde is niet echt. Werk de waarde bij met de werkelijke aanmeldings-URL. Neem contact op met [suiker CRM-Client-ondersteuningsteam](https://support.sugarcrm.com/) om de waarde. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

6. Op de **suiker CRM instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-sugar-crm-single-sign-on"></a>Suiker CRM eenmalige aanmelding configureren

1. In een ander browservenster aanmelden bij uw bedrijf suiker CRM site als beheerder.

1. Ga naar **Admin**.

    ![Admin](./media/sugarcrm-tutorial/ic795888.png "Admin")

1. In de **beheer** sectie, klikt u op **wachtwoordbeheer**.

    ![Beheer](./media/sugarcrm-tutorial/ic795889.png "Beheer")

1. Selecteer **SAML-verificatie inschakelen**.

    ![Beheer](./media/sugarcrm-tutorial/ic795890.png "Beheer")

1. Voer in het gedeelte **SAML-verificatie** de volgende stappen uit:

    ![SAML-verificatie](./media/sugarcrm-tutorial/ic795891.png "SAML-verificatie")  

    a. In de **aanmeldings-URL** tekstvak, plak de waarde van **aanmeldings-URL**, die u hebt gekopieerd vanuit Azure portal.
  
    b. In de **SLO URL** tekstvak, plak de waarde van **afmeldings-URL van**, die u hebt gekopieerd vanuit Azure portal.
  
    c. Open uw base-64 gecodeerde certificaat in Kladblok, Kopieer de inhoud ervan in het Klembord en plakt u het gehele certificaat in **X.509-certificaat** tekstvak.
  
    d. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In de **gebruikersnaam** veldtype `brittasimon@yourcompanydomain.extension`  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan suiker CRM.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **suiker CRM**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **suiker CRM**.

    ![De suiker CRM-koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-sugar-crm-test-user"></a>Suiker CRM testgebruiker maken

Als u wilt dat Azure AD-gebruikers kunnen zich aanmelden bij suiker CRM, moeten ze worden ingericht op suiker CRM. In het geval van CRM-suiker is inrichten een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Aanmelden bij uw **suiker CRM** bedrijf site als administrator.

1. Ga naar **Admin**.

    ![Admin](./media/sugarcrm-tutorial/ic795888.png "Admin")

1. In de **beheer** sectie, klikt u op **Gebruikersbeheer**.

    ![Beheer](./media/sugarcrm-tutorial/ic795893.png "Beheer")

1. Ga naar **gebruikers \> nieuwe gebruiker maken**.

    ![Nieuwe gebruiker maken](./media/sugarcrm-tutorial/ic795894.png "nieuwe gebruiker maken")

1. Op de **gebruikersprofiel** tabblad, voert u de volgende stappen uit:

    ![New User](./media/sugarcrm-tutorial/ic795895.png "New User")

    * Type de **gebruikersnaam**, **achternaam**, en **e-mailadres** van een geldige Azure Active Directory-gebruiker in de bijbehorende tekstvakken.
  
1. Als **Status**, selecteer **Active**.

1. Voer de volgende stappen uit op het tabblad wachtwoord:

    ![New User](./media/sugarcrm-tutorial/ic795896.png "New User")

    a. Typ het wachtwoord in het bijbehorende tekstvak.

    b. Klik op **Opslaan**.

> [!NOTE]
> Kunt u alle andere suiker CRM gebruiker-account maken van hulpprogramma's of API's die door CRM suiker inrichten AAD-gebruikersaccounts.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel suiker CRM in het toegangsvenster, moet u worden automatisch aangemeld bij de suiker CRM waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

