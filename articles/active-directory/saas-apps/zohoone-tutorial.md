---
title: 'Zelfstudie: Azure Active Directory-integratie met één Zoho | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en één Zoho.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: bbc3038c-0d8b-45dd-9645-368bd3d01a0f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/16/2019
ms.author: jeedes
ms.openlocfilehash: 0a37789e7c7efeb71770ff0e8061d57e6603b6c4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67086238"
---
# <a name="tutorial-azure-active-directory-integration-with-zoho-one"></a>Zelfstudie: Azure Active Directory-integratie met één Zoho

In deze zelfstudie leert u hoe u een Zoho integreren met Azure Active Directory (Azure AD).
Zoho één integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot één Zoho heeft.
* U kunt uw gebruikers worden automatisch aangemeld bij een Zoho (Single Sign-On) inschakelen met hun Azure AD-accounts.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het Azure AD-integratie configureren met één Zoho, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* Zoho een eenmalige aanmelding ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor één Zoho **SP** en **IDP** gestart door SSO

## <a name="adding-zoho-one-from-the-gallery"></a>Toevoegen van een Zoho uit de galerie

Voor het configureren van de integratie van één Zoho in Azure AD, moet u één Zoho toevoegen uit de galerie aan de lijst met beheerde SaaS-apps.

**Als u wilt één Zoho uit de galerie toevoegen, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)** , klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **Zoho één**, selecteer **Zoho één** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![Een Zoho in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Zoho één op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in een Zoho tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met één Zoho, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)** : als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureer Zoho een Single Sign-On](#configure-zoho-one-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)** : als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)** : als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Zoho een testgebruiker maken](#create-zoho-one-test-user)**  : als u wilt een equivalent van Britta Simon in Zoho één die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)** : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met één Zoho, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **Zoho één** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In het gedeelte **Standaard SAML-configuratie** voert u de volgende stappen uit als u de toepassing in de door **IDP** geïnitieerde modus wilt configureren:

    ![Zoho één domein en URL's, eenmalige aanmelding informatie](common/idp-relay.png)

    a. In het tekstvak **Id** typt u een URL: `one.zoho.com`

    b. In het tekstvak **Antwoord-URL** typt u een URL met het volgende patroon: `https://accounts.zoho.com/samlresponse/<saml-identifier>`

    > [!NOTE]
    > De voorgaande **antwoord-URL** waarde is niet echt. U krijgt de `<saml-identifier>` waarde van #step4 van **configureren Zoho een Single Sign-On** sectie, die later in de zelfstudie wordt uitgelegd.

    c. Klik op **Extra URL's instellen**.

    d. In het tekstvak **Relaystatus** typt u een URL: `https://one.zoho.com`

5. Als u wilt configureren van de toepassing in **SP** gestart modus, de volgende stap uitvoeren:


    ![Zoho één domein en URL's, eenmalige aanmelding informatie](common/both-signonurl.png)

    In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://accounts.zoho.com/samlauthrequest/<domain_name>?serviceurl=https://one.zoho.com` 

    > [!NOTE] 
    > De voorgaande **aanmeldings-URL** waarde is niet echt. U kunt de waarde wordt bijgewerkt met de werkelijke aanmeldings-URL van de **configureren Zoho een Single Sign-On** sectie, die later in de zelfstudie wordt uitgelegd. 

6. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

7. Op de **instellen van een Zoho** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-zoho-one-single-sign-on"></a>Zoho een Single Sign-On configureren

1. In een ander browservenster aanmelden met uw Zoho een bedrijf site als beheerder.

2. Op de **organisatie** tabblad, klikt u op **Setup** onder **SAML-verificatie**.

    ![Zoho One org](./media/zohoone-tutorial/tutorial_zohoone_setup.png)

3. Voer de volgende stappen uit op de pop-pagina:

    ![Zoho One sig](./media/zohoone-tutorial/tutorial_zohoone_save.png)

    a. In de **aanmelden URL** tekstvak, plak de waarde van **aanmeldings-URL**, die u hebt gekopieerd vanuit Azure portal.

    b. In de **afmelding URL** tekstvak, plak de waarde van **afmeldings-URL van**, die u hebt gekopieerd vanuit Azure portal.

    c. Klik op **Bladeren** het uploaden van de **certificaat (Base64)** die u hebt gedownload vanuit Azure portal.

    d. Klik op **Opslaan**.

4. Na het opslaan van de SAML-verificatie-instellingen, Kopieer de **SAML-id** waarde en voeg de **antwoord-URL** in plaats van `<saml-identifier>`, bijvoorbeeld `https://accounts.zoho.com/samlresponse/one.zoho.com` en plak de gegenereerde waarde in de **Antwoord-URL** tekstvak onder **SAML-basisconfiguratie** sectie.

    ![Zoho One saml](./media/zohoone-tutorial/tutorial_zohoone_samlidenti.png)

5. Ga naar de **domeinen** tabblad en klik vervolgens op **domein toevoegen**.

    ![Zoho One domain](./media/zohoone-tutorial/tutorial_zohoone_domain.png)

6. Op de **domein toevoegen** pagina, voert u de volgende stappen uit:

    ![Zoho één toevoegen domein](./media/zohoone-tutorial/tutorial_zohoone_adddomain.png)

    a. In de **domeinnaam** tekstvak, typ het domein, zoals contoso.com.

    b. Klik op **Toevoegen**.

    >[!Note]
    >Na het toevoegen van het domein volgen [deze](https://www.zoho.com/one/help/admin-guide/domain-verification.html) stappen om uw domein te verifiëren. Zodra het domein is geverifieerd, gebruikt u de naam van uw domein in **aanmeldings-URL** in **SAML-basisconfiguratie** sectie in Azure portal.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken 

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In de **gebruikersnaam** veldtype `brittasimon@yourcompanydomain.extension`. Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan één Zoho.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **Zoho één**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **Zoho één**.

    ![De Zoho een koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-zoho-one-test-user"></a>Zoho een testgebruiker maken

Als u wilt dat Azure AD-gebruikers kunnen zich aanmelden bij een Zoho, moeten ze worden ingericht voor Zoho één. In één Zoho is inrichten een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Aanmelden bij Zoho een als een beveiligingsbeheerder.

2. Op de **gebruikers** tabblad, klik op **gebruiker logo**.

    ![Zoho één gebruiker](./media/zohoone-tutorial/tutorial_zohoone_users.png)

3. Op de **gebruiker toevoegen** pagina, voert u de volgende stappen uit:

    ![Zoho één toevoegen gebruiker](./media/zohoone-tutorial/tutorial_zohoone_adduser.png)
    
    a. In **naam** tekst voert u de naam van gebruiker, zoals **Julia steen**.
    
    b. In **e-mailadres** tekst vak, voer het e-mailadres van gebruiker, zoals brittasimon@contoso.com.

    >[!Note]
    >Selecteer uw geverifieerde domein in de lijst met domeinen.

    c. Klik op **Toevoegen**.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de Zoho een tegel in het toegangsvenster, moet u worden automatisch aangemeld bij de waarvoor u een SSO instellen van het type Zoho één. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

