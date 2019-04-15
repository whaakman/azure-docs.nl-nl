---
title: 'Zelfstudie: Azure Active Directory-integratie met Zscaler persoonlijke toegang Administrator | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Zscaler particuliere Access-beheerder.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: c87392a7-e7fe-4cdc-a8e6-afe1ed975172
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/03/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ce75431de24886c038cd2eb4ee7db02d2b6cde31
ms.sourcegitcommit: b8a8d29fdf199158d96736fbbb0c3773502a092d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/15/2019
ms.locfileid: "59563350"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-private-access-administrator"></a>Zelfstudie: Azure Active Directory-integratie met Zscaler persoonlijke toegang beheerder

In deze zelfstudie leert u hoe u Zscaler persoonlijke toegang beheerder integreren met Azure Active Directory (Azure AD).
Zscaler persoonlijke toegang beheerder integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die naar Zscaler persoonlijke toegang beheerder toegang heeft.
* U kunt uw gebruikers worden automatisch aangemeld Zscaler persoonlijke toegang beheerder (Single Sign-On) inschakelen met hun Azure AD-accounts.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Zscaler particuliere Access-beheerder, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* Eenmalige aanmelding Zscaler persoonlijke toegang beheerder ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor Zscaler persoonlijke toegang beheerder **SP** en **IDP** gestart door SSO

## <a name="adding-zscaler-private-access-administrator-from-the-gallery"></a>Beheerder van de Zscaler persoonlijke toegang toe te voegen uit de galerie

Voor het configureren van de integratie van de Zscaler particuliere Access-beheerder in Azure AD, moet u Zscaler persoonlijke toegang beheerder uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Zscaler particuliere Access-beheerder van de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak typt, **Zscaler persoonlijke toegang beheerder**, selecteer **Zscaler particuliere Access-beheerder** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen de de toepassing.

    ![Beheerder van de Zscaler persoonlijke toegang in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Zscaler particuliere beheerder toegang op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Zscaler particuliere beheerder toegang tot stand worden gebracht.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Zscaler particuliere Access-beheerder, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureer Zscaler persoonlijke toegang beheerder Single Sign-On](#configure-zscaler-private-access-administrator-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maak testgebruiker Zscaler persoonlijke toegang beheerder](#create-zscaler-private-access-administrator-test-user)**  - hebben een equivalent van Britta Simon in Zscaler persoonlijke toegang die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met Zscaler particuliere Access-beheerder, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **Zscaler persoonlijke toegang beheerder** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In het gedeelte **Standaard SAML-configuratie** voert u de volgende stappen uit als u de toepassing in de door **IDP** geïnitieerde modus wilt configureren:

    ![Zscaler persoonlijke toegang beheerder domein en URL's eenmalige aanmelding informatie](common/idp-relay.png)

    a. In het tekstvak **Id** typt u een URL met het volgende patroon: `https://<subdomain>.private.zscaler.com/auth/metadata`

    b. In het tekstvak **Antwoord-URL** typt u een URL met de volgende notatie: `https://<subdomain>.private.zscaler.com/auth/sso`

    c. Klik op **Extra URL's instellen**.

    d. In het tekstvak **Relaystatus** typt u een URL: `idpadminsso`

5.  Als u wilt configureren van de toepassing in **SP** gestart modus, de volgende stap uitvoeren:

    ![Zscaler persoonlijke toegang beheerder domein en URL's eenmalige aanmelding informatie](common/both-signonurl.png)

    In het tekstvak **Aanmeldings-URL** typt u een URL met het volgende patroon: `https://<subdomain>.private.zscaler.com/auth/sso`   

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke-id, de antwoord-URL en de aanmeldings-URL. Neem contact op met [Zscaler persoonlijke toegang beheerder Client ondersteuningsteam](https://help.zscaler.com/zpa-submit-ticket) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

6. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

7. Op de **ingesteld van Zscaler persoonlijke toegang beheerder** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-zscaler-private-access-administrator-single-sign-on"></a>Zscaler particuliere Access-beheerder voor eenmalige aanmelding configureren

1. Meld u aan Zscaler particuliere Access-beheerder als een beheerder in een ander browservenster.

2. Klik op de bovenkant op **beheer** en navigeer naar **verificatie** sectie Klik **IdP-configuratie**.

    ![Beheerder van de Zscaler persoonlijke toegang beheerder](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_admin.png)

3. Klik in de rechterbovenhoek, **IdP-configuratie toevoegen**. 

    ![Addidp Zscaler persoonlijke toegang beheerder](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_addpidp.png)

4. Op de **IdP-configuratie toevoegen** pagina de volgende stappen uitvoeren:
 
    ![Idpselect Zscaler persoonlijke toegang beheerder](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_idpselect.png)

    a. Klik op **bestand selecteren** voor het uploaden van het gedownloade bestand met metagegevens van Azure AD in de **IdP metagegevens bestand uploaden** veld.

    b. Is het ingesteld op de **IdP metagegevens** van Azure AD en alle informatie van de velden gevuld, zoals hieronder wordt weergegeven.

    ![Idpconfig Zscaler persoonlijke toegang beheerder](./media/zscalerprivateaccessadministrator-tutorial/idpconfig.png)

    c. Selecteer **eenmalige aanmelding** als **beheerder**.

    d. Selecteer uw domein uit **domeinen** veld.
    
    e. Klik op **Opslaan**.

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

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Zscaler persoonlijke toegang beheerder.

1. Selecteer in de Azure portal **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **Zscaler persoonlijke toegang beheerder**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **Zscaler persoonlijke toegang beheerder**.

    ![De koppeling Zscaler particuliere Access-beheerder in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-zscaler-private-access-administrator-test-user"></a>Testgebruiker Zscaler persoonlijke toegang beheerder maken

Als u wilt dat Azure AD-gebruikers kunnen zich aanmelden voor persoonlijke Zscaler Access-beheerder, als ze in Zscaler persoonlijke toegang beheerder worden ingericht. In het geval van Zscaler persoonlijke toegang Administrator inrichten is een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Meld u aan uw bedrijf Zscaler persoonlijke toegang beheerder site als een beheerder.

2. Klik op de bovenkant op **beheer** en navigeer naar **verificatie** sectie Klik **IdP-configuratie**.

    ![Beheerder van de Zscaler persoonlijke toegang beheerder](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_admin.png)

3. Klik op **beheerders** vanaf de linkerkant van het menu.

    ![Beheerder van de Zscaler persoonlijke toegang beheerder](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_adminstrator.png)

4. Klik in de rechterbovenhoek, **beheerder toevoegen**:

    ![Beheerder van de Zscaler persoonlijke toegang beheerder toevoegen](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_addadmin.png)

5. In de **beheerder toevoegen** pagina, voert u de volgende stappen uit:

    ![Gebruikersbeheerder Zscaler persoonlijke toegang beheerder](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_useradmin.png)

    a. In de **gebruikersnaam** tekstvak, voer het e-mailadres van gebruiker, zoals BrittaSimon@contoso.com.

    b. In de **wachtwoord** tekstvak typt u het wachtwoord.

    c. In de **wachtwoord bevestigen** tekstvak typt u het wachtwoord.

    d. Selecteer **rol** als **Zscaler persoonlijke toegang beheerder**.

    e. In de **e** tekstvak, voer het e-mailadres van gebruiker, zoals BrittaSimon@contoso.com.

    f. In de **Phone** tekstvak typt u het telefoonnummer.

    g. In de **tijdzone** textbox, selecteert u de tijdzone.

    h. Klik op **Opslaan**.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Zscaler particuliere Access-beheerder in het toegangsvenster, moet u worden automatisch aangemeld op de Zscaler particuliere Access-beheerder waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

