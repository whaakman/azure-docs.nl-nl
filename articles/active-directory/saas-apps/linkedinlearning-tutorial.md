---
title: 'Zelfstudie: Azure Active Directory-integratie met LinkedIn Learning | Microsoft Docs'
description: Ontdek hoe u eenmalige aanmelding configureert tussen Azure Active Directory en LinkedIn Learning.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: d5857070-bf79-4bd3-9a2a-4c1919a74946
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/18/2018
ms.author: jeedes
ms.openlocfilehash: be5dd0a0fd1eb45c505a2f0ddf1489d21bfd38e6
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54821181"
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-learning"></a>Zelfstudie: Azure Active Directory-integratie met LinkedIn Learning

In deze zelfstudie leert u hoe u LinkedIn Learning met Azure Active Directory (Azure AD) kunt integreren.
De integratie van LinkedIn Learning met Azure AD heeft de volgende voordelen:

* U kunt in Azure AD beheren wie toegang tot LinkedIn Learning heeft.
* U kunt inschakelen dat gebruikers automatisch met hun Azure AD-account worden aangemeld bij LinkedIn Learning (eenmalige aanmelding).
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om Azure AD-integratie met LinkedIn Learning te configureren:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Een abonnement op LinkedIn Learning waarvoor eenmalige aanmelding is ingeschakeld

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* LinkedIn Learning biedt ondersteuning voor door **SP en IDP** geïnitieerde eenmalige aanmelding
* LinkedIn Learning biedt ondersteuning voor **Just In Time**-gebruikersinrichting

## <a name="adding-linkedin-learning-from-the-gallery"></a>LinkedIn Learning vanuit de galerie toevoegen

Als u de integratie van LinkedIn Learning met Azure AD wilt configureren, moet u LinkedIn Learning vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u LinkedIn Learning vanuit de galerie wilt toevoegen, voert u de volgende stappen uit:**

1. Klik in het linkernavigatievenster in de **[Azure-portal](https://portal.azure.com)** op het **Azure Active Directory**-pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ **LinkedIn Learning** in het zoekvak, selecteer **LinkedIn Learning** in het deelvenster met resultaten en klik vervolgens op de knop **Toevoegen** om de app toe te voegen.

     ![LinkedIn Learning in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In dit gedeelte configureert en test u eenmalige aanmelding van Azure AD met LinkedIn Learning op basis van een testgebruiker met de naam **Britta Simon**.
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Azure AD-gebruiker en de daaraan gerelateerde gebruiker in LinkedIn Learning tot stand is gebracht.

Als u eenmalige aanmelding met Azure AD wilt configureren en testen met LinkedIn Learning, moet u aan de volgende vereisten voldoen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Eenmalige aanmelding voor LinkedIn Learning configureren](#configure-linkedin-learning-single-sign-on)**: als u de instellingen voor eenmalige aanmelding aan de toepassingszijde wil configureren.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Testgebruiker voor LinkedIn Learning maken](#create-linkedin-learning-test-user)**: als u een equivalent van Britta Simon wilt hebben in LinkedIn Learning dat is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voer de volgende stappen uit om eenmalige aanmelding van Azure AD met LinkedIn Learning te configureren:

1. In [Azure Portal](https://portal.azure.com/) selecteert u op de integratiepagina van de **LinkedIn Learning**-toepassing de optie **Eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In het gedeelte **Standaard SAML-configuratie** voert u de volgende stappen uit als u de toepassing in de door **IdP geïnitieerde** modus wilt configureren:

    ![Informatie over eenmalige aanmelding voor LinkedIn Learning-domein en -URL's](common/idp-intiated.png)

    a. Voer in het tekstvak **id** de **Entiteits-id** in die u hebt gekopieerd uit LinkedIn Portal. 

    b. Voer in het tekstvak **Antwoord-URL** de **ACS-URL (Assertion Consumer Service)** in die u uit LinkedIn Portal hebt gekopieerd.

    c. Als u de toepassing in de **SP geïnitieerde** modus wilt configureren, klikt u in het gedeelte **Standaard SAML-configuratie** waarin u de aanmeldings-URL opgeeft op **Extra URL's instellen**. Kopieer de **ACS-URL (Assertion Consumer Service)** en vervang /saml/ door /login/ om een kopie van de aanmeldings-URL te maken. Zodra dit is gebeurd, zou de aanmeldings-URL het volgende patroon moeten hebben:

    `https://www.linkedin.com/checkpoint/enterprise/login/<AccountId>?application=learning&applicationInstanceId=<InstanceId>`

    ![Informatie over eenmalige aanmelding voor LinkedIn Learning-domein en -URL's](common/metadata-upload-additional-signon.png)

    > [!NOTE]
    > Deze waarden zijn geen werkelijke waarden. U gaat deze waarden bijwerken met de werkelijke id en de Antwoord-URL. Dit wordt later uitgelegd in het gedeelte **Eenmalige aanmelding voor LinkedIn Learning configureren** van deze zelfstudie.

5. Uw LinkedIn Learning-toepassing verwacht de SAML-asserties in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen moet toevoegen aan de configuratie van de SAML-tokenkenmerken. In de volgende schermopname ziet u een voorbeeld hiervan. De standaardwaarde van **Unieke gebruikers-id** is **user.userprincipalname**, maar LinkedIn Learning verwacht dat aan deze waarde het e-mailadres van de gebruiker wordt toegewezen. Hiervoor kunt u het kenmerk **user.mail** in de lijst gebruiken of de juiste kenmerkwaarde op basis van uw organisatieconfiguratie. 

    ![image](common/edit-attribute.png)

6. Bewerk in het gedeelte **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** de claims met het **pictogram Bewerken** of voeg de claims toe door met **Nieuwe claim toevoegen** het kenmerk van het SAML-token te configureren, zoals wordt weergegeven in de bovenstaande afbeelding. Hierna voert u de volgende stappen uit:
    
    | Naam | Bronkenmerk |
    | ---------------| --------------- |
    | e-mail  | user.mail  |
    | department  | user.department  |
    | firstname  | user.givenname  |
    | lastname  | user.surname  |

    a. Klik op **Nieuwe claim toevoegen** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    ![image](./media/linkedinlearning-tutorial/tutorial_usermail.png)

    ![image](./media/linkedinlearning-tutorial/tutorial_usermailedit.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Laat **Naamruimte** leeg.

    d. Selecteer Bron bij **Kenmerk**.

    e. Typ de kenmerkwaarde voor die rij in de lijst met **bronkenmerken**.

    f. Klik op **OK**.

    g. Klik op **Opslaan**.

7. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

### <a name="configure-linkedin-learning-single-sign-on"></a>Eenmalige aanmelding voor LinkedIn Learning configureren

1. Meld u in een ander browservenster als beheerder aan bij uw LinkedIn Learning-tenant.

2. Klik in **Accountcentrum** onder **Instellingen** op **Algemene instellingen**. Selecteer ook **Learning - standaard** uit de vervolgkeuzelijst.

    ![Eenmalige aanmelding configureren](./media/linkedinlearning-tutorial/tutorial_linkedin_admin_01.png)

3. Klik op **Of klik hier om afzonderlijke velden van het formulier te laden en kopiëren** en kopieer **Entiteits-id** en **ACS-URL (Assertion Consumer Service)** en plak deze in het gedeelte **Standaard SAML-configuratie** in Azure Portal.

    ![Eenmalige aanmelding configureren](./media/linkedinlearning-tutorial/tutorial_linkedin_admin_03.png)

4. Ga naar het gedeelte **met beheerdersinstellingen voor LinkedIn**. Upload het XML-bestand dat u hebt gedownload uit Azure Portal door op de optie **XML-bestand uploaden** te klikken.

    ![Eenmalige aanmelding configureren](./media/linkedinlearning-tutorial/tutorial_linkedin_metadata_03.png)

5. Klik op **Aan** om SSO in te schakelen. De SSO-status verandert van **Niet verbonden** naar **Verbonden**

    ![Eenmalige aanmelding configureren](./media/linkedinlearning-tutorial/tutorial_linkedin_admin_05.png)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken 

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam****Britta Simon** in.
  
    b. In het veld **Gebruikersnaam** typt u **brittasimon@yourcompanydomain.extension**.  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In dit gedeelte gaat u Britta Simon toestemming geven voor gebruik van eenmalige aanmelding met Azure door haar toegang te geven tot LinkedIn Learning.

1. Selecteer in Azure Portal **Bedrijfstoepassingen**, selecteer **Alle toepassingen** en selecteer vervolgens **LinkedIn Learning**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Typ en selecteer **LinkedIn Learning** in de lijst met toepassingen.

    ![De link LinkedIn Learning in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-linkedin-learning-test-user"></a>Testgebruiker voor LinkedIn Learning maken

De LinkedIn Learning-toepassing biedt ondersteuning voor Just in time-gebruikersinrichting. Na verificatie worden gebruikers automatisch aangemaakt in de toepassing. Ga naar het tabblad met beheerdersinstellingen in de LinkedIn Learning-portal en zet de schakelaar voor **automatisch toewijzen van licenties** op actieve Just in time-inrichting. Hiermee wordt ook een licentie aan de gebruiker toegewezen.

   ![Een Azure AD-testgebruiker maken](./media/linkedinlearning-tutorial/LinkedinUserprovswitch.png)

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie gaat u uw configuratie van Azure AD-eenmalige aanmelding testen via het toegangsvenster.

Wanneer u op de tegel LinkedIn Learning in het toegangsvenster klikt, zou u automatisch moeten worden aangemeld bij de instantie van LinkedIn Learning waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

