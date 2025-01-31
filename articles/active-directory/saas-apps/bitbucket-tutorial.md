---
title: 'Zelfstudie: Azure Active Directory-integratie met SAML SSO for Bitbucket by resolution GmbH | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en SAML SSO for Bitbucket by resolution GmbH.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: fc947df1-f24e-43ae-9a34-518293583d69
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/27/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: b003d4b08d393b6f3eb57539774df16a23b39b4d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67106208"
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-bitbucket-by-resolution-gmbh"></a>Zelfstudie: Azure Active Directory-integratie met SAML SSO for Bitbucket by resolution GmbH

Deze zelfstudie laat zien hoe u SAML SSO for Bitbucket by resolution GmbH integreert met Azure Active Directory (Azure AD).
Integratie van SAML SSO for Bitbucket by resolution GmbH met Azure AD biedt de volgende voordelen:

* U kunt in Azure AD beheren wie toegang heeft tot SAML SSO for Bitbucket by resolution GmbH.
* U kunt inschakelen dat gebruikers automatisch met hun Azure AD-account worden aangemeld bij SAML SSO for Bitbucket by resolution GmbH (eenmalige aanmelding).
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om Azure AD-integratie met SAML SSO for Bitbucket by resolution GmbH te configureren:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Een abonnement op SAML SSO for Bitbucket by resolution GmbH waarvoor eenmalige aanmelding is ingeschakeld

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* SAML SSO for Bitbucket by resolution GmbH ondersteunt door **SP en IDP** geïnitieerde eenmalige aanmelding
* SAML SSO for Bitbucket by resolution GmbH ondersteunt **Just-In-Time** gebruikers inrichten


## <a name="adding-saml-sso-for-bitbucket-by-resolution-gmbh-from-the-gallery"></a>SAML SSO for Bitbucket by resolution GmbH toevoegen vanuit de galerie

Voor het configureren van de integratie van SAML SSO for Bitbucket by resolution GmbH in Azure AD moet u SAML SSO for Bitbucket by resolution GmbH vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Voer de volgende stappen uit om SAML SSO for Bitbucket by resolution GmbH toe te voegen vanuit de galerie:**

1. In de **[Azure-portal](https://portal.azure.com)** , klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ **SAML SSO for Bitbucket by resolution GmbH** in het zoekvak, selecteer **SAML SSO for Bitbucket by resolution GmbH** in het resultatenvenster en klik op **Toevoegen** om de toepassing toe te voegen.

     ![SAML SSO for Bitbucket by resolution GmbH in de resultatenlijst](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In dit gedeelte configureert en test u eenmalige aanmelding via Azure AD met SAML SSO for Bitbucket by resolution GmbH op basis van een testgebruiker met de naam **Britta Simon**.
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Azure AD-gebruiker en de daaraan gerelateerde gebruiker in SAML SSO for Bitbucket by resolution GmbH tot stand is gebracht.

Voltooi de volgende stappen om eenmalige aanmelding via Azure AD met SAML SSO for Bitbucket by resolution GmbH te configureren en te testen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)** : als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Eenmalige aanmelding configureren voor SAML SSO for Bitbucket by resolution GmbH](#configure-saml-sso-for-bitbucket-by-resolution-gmbh-single-sign-on)** : als u de instellingen voor eenmalige aanmelding aan de toepassingszijde wil configureren.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)** : als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)** : als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Testgebruiker voor SAML SSO for Bitbucket by resolution GmbH maken](#create-saml-sso-for-bitbucket-by-resolution-gmbh-test-user)** : als u een tegenhanger van Britta Simon in SAML SSO for Bitbucket by resolution GmbH wilt hebben die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)** : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voer de volgende stappen uit om eenmalige aanmelding via Azure AD met SAML SSO for Bitbucket by resolution GmbH te configureren:

1. In de [Azure-portal](https://portal.azure.com/), op de integratiepagina voor de toepassing **SAML SSO for Bitbucket by resolution GmbH**, selecteert u **Eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. Voer in het gedeelte **Standaard SAML-configuratie** de volgende stappen uit als u de toepassing in de door **IDP** geïnitieerde modus wilt configureren:

    ![Informatie over domein en URL's voor eenmalige aanmelding met SAML SSO for Bitbucket by resolution GmbH](common/idp-intiated.png)

    a. In het tekstvak **Id** typt u een URL met het volgende patroon: `https://<server-base-url>/plugins/servlet/samlsso`

    b. In het tekstvak **Antwoord-URL** typt u een URL met de volgende notatie: `https://<server-base-url>/plugins/servlet/samlsso`

    c. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    ![Informatie over domein en URL's voor eenmalige aanmelding met SAML SSO for Bitbucket by resolution GmbH](common/metadata-upload-additional-signon.png)

    In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<server-base-url>/plugins/servlet/samlsso`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke-id, de antwoord-URL en de aanmeldings-URL. Neem contact op met het [klantondersteuningsteam voor SAML SSO for Bitbucket by resolution GmbH](https://marketplace.atlassian.com/apps/1217045/saml-single-sign-on-sso-bitbucket?hosting=server&tab=support) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

### <a name="configure-saml-sso-for-bitbucket-by-resolution-gmbh-single-sign-on"></a>SAML SSO for Bitbucket by resolution GmbH configureren voor eenmalige aanmelding

1. Meld u aan bij de bedrijfssite van SAML SSO for Bitbucket by resolution GmbH als een beheerder.

2. Klik aan de rechterkant van de hoofdwerkbalk op **Settings** (Instellingen).

3. Ga naar het gedeelte ACCOUNTS en klik in de menubalk op **SAML SingleSignOn** (Eenmalige aanmelding met SAML).

    ![De eenmalige aanmelding met SAML](./media/bitbucket-tutorial/tutorial_bitbucket_samlsingle.png)

4. Op de **pagina SAML SIngleSignOn Plugin Configuration** (Configuratie van plug-in voor eenmalige aanmelding met SAML) klikt u op **Add idp** (IdP toevoegen). 

    ![De IdP toevoegen](./media/bitbucket-tutorial/tutorial_bitbucket_addidp.png)

5. Voer op de pagina **Choose your SAML Identity Provider** (Uw SAML-id-provider kiezen) de volgende stappen uit:

    ![De id-provider](./media/bitbucket-tutorial/tutorial_bitbucket_identityprovider.png)

    a. Selecteer bij **Idp Type** (Type IdP) de optie **AZURE AD**.

    b. Voer in het tekstvak **Name** (Naam) de naam in.

    c. Voer in het tekstvak **Description** (Omschrijving) de omschrijving in.

    d. Klik op **volgende**.

6. Op de **pagina voor het configureren van de id-provider** klikt u op **Next** (Volgende).

    ![De id-configuratie](./media/bitbucket-tutorial/tutorial_bitbucket_identityconfig.png)

7.  Klik op de pagina **Import SAML Idp Metadata** (Metagegevens voor SAML-IdP importeren) op de optie **Load File** (Bestand laden) om het bestand **METADATA XML** te uploaden die u uit de Azure-portal hebt gedownload.

    ![De IdP-metagegevens](./media/bitbucket-tutorial/tutorial_bitbucket_idpmetadata.png)
    
8. Klik op **volgende**.

9. Klik op **Save settings** (Instellingen opslaan).

    ![Het opslaan](./media/bitbucket-tutorial/tutorial_bitbucket_save.png)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken 

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In de **gebruikersnaam** veldtype **brittasimon\@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In dit gedeelte stelt u Britta Simon in staat om eenmalige aanmelding van Azure te gebruiken door haar toegangsrechten voor SAML SSO for Bitbucket by resolution GmbH te verlenen.

1. Selecteer in de Azure-portal **Bedrijfstoepassingen**, selecteer **Alle toepassingen** en selecteer vervolgens **SAML SSO for Bitbucket by resolution GmbH**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Typ en selecteer in de lijst met toepassingen **SAML SSO for Bitbucket by resolution GmbH**.

    ![De link SAML SSO for Bitbucket by resolution GmbH in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-saml-sso-for-bitbucket-by-resolution-gmbh-test-user"></a>Testgebruiker voor SAML SSO for Bitbucket by resolution GmbH maken

Het doel van dit gedeelte is het maken van een gebruiker met de naam Britta Simon in SAML SSO for Bitbucket by resolution GmbH. SAML SSO for Bitbucket by resolution GmbH biedt ondersteuning voor Just-In-Time-inrichting en het handmatig inrichten van gebruikers. Neem contact op met [het klantondersteuningsteam van SAML SSO for Bitbucket by resolution GmbH](https://marketplace.atlassian.com/plugins/com.resolution.atlasplugins.samlsso-bitbucket/server/support) naargelang uw behoeften.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Als u in het toegangsvenster op de tegel SAML SSO for Bitbucket by resolution GmbH klikt, zou u automatisch moeten worden aangemeld bij het exemplaar van SAML SSO for Bitbucket by resolution GmbH waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

