---
title: 'Zelfstudie: Azure Active Directory-integratie met Everbridge | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Everbridge.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 58d7cd22-98c0-4606-9ce5-8bdb22ee8b3e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/18/2019
ms.author: jeedes
ms.openlocfilehash: 60463a00c6864bed7b3a18e816ef0143d3573782
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67103262"
---
# <a name="tutorial-azure-active-directory-integration-with-everbridge"></a>Zelfstudie: Azure Active Directory-integratie met Everbridge

In deze zelfstudie leert u hoe u Everbridge integreren met Azure Active Directory (Azure AD).
Wanneer u Everbridge met Azure AD integreert, kunt u het volgende doen:

* Beheren in Azure AD die toegang tot Everbridge heeft.
* Toestaan dat uw gebruikers automatisch worden aangemeld bij Everbridge met hun Azure AD-accounts. Dit toegangsbeheer wordt eenmalige aanmelding (SSO) genoemd.
* Uw accounts in één centrale locatie beheren met behulp van de Azure-portal.
Zie voor meer informatie over software als een service (SaaS)-app-integratie met Azure AD, [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Everbridge, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/).
* Een abonnement op Everbridge die gebruikmaakt van eenmalige aanmelding.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Everbridge biedt ondersteuning voor IDP gestart door SSO.

## <a name="add-everbridge-from-the-azure-marketplace"></a>Everbridge toevoegen in Azure Marketplace

Toevoegen voor het configureren van de integratie van Everbridge in Azure AD, Everbridge vanuit Azure Marketplace aan uw lijst met beheerde SaaS-apps.

Volg deze stappen om toe te voegen Everbridge vanuit Azure Marketplace.

1. In de [Azure-portal](https://portal.azure.com), selecteer in het navigatiedeelvenster links **Azure Active Directory**.

    ![Knop Azure Active Directory](common/select-azuread.png)

2. Ga naar **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u een nieuwe toepassing wilt toevoegen, selecteert u **Nieuwe toepassing** bovenaan het dialoogvenster.

    ![Knop Nieuwe toepassing](common/add-new-app.png)

4. Voer in het zoekvak **Everbridge**. Selecteer **Everbridge** uit het deelvenster met resultaten en selecteer **toevoegen**.

     ![Everbridge in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Everbridge op basis van de testgebruiker Britta Simon.
Voor eenmalige aanmelding in om te werken, een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Everbridge vast te stellen.

Als u wilt configureren en Azure AD eenmalige aanmelding met Everbridge testen, voert u de volgende bouwstenen:

- [Eenmalige aanmelding van Azure AD configureren](#configure-azure-ad-single-sign-on) zodat uw gebruikers deze functie kunnen gebruiken.
- [Everbridge configureren als Everbridge manager portal eenmalige aanmelding](#configure-everbridge-as-everbridge-manager-portal-single-sign-on) de instellingen voor eenmalige aanmelding configureren aan de toepassing.
- [Everbridge configureren als Everbridge lid portal eenmalige aanmelding](#configure-everbridge-as-everbridge-member-portal-single-sign-on) de instellingen voor eenmalige aanmelding configureren aan de toepassing.
- [Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user) voor het testen van eenmalige aanmelding van Azure AD met Britta Simon.
- [De Azure AD-testgebruiker toewijzen](#assign-the-azure-ad-test-user) zodat Britta Simon gebruik kan maken van eenmalige aanmelding van Azure AD.
- [Maak een testgebruiker Everbridge](#create-an-everbridge-test-user) hebben een equivalent van Britta Simon in Everbridge die gekoppeld aan de Azure AD-weergave van de gebruiker.
- [Eenmalige aanmelding testen](#test-single-sign-on) om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Volg deze stappen voor het configureren van Azure AD eenmalige aanmelding met Everbridge.

1. In de [Azure-portal](https://portal.azure.com/)op de **Everbridge** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. Selecteer in het dialoogvenster **Selecteer een methode voor eenmalige aanmelding** de modus **SAML/WS-Fed** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de **instellen van eenmalige aanmelding met SAML** weergeeft, schakelt **bewerken** openen de **SAML-basisconfiguratie** in het dialoogvenster.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

    >[!NOTE]
    >Configureren van de toepassing als de manager portal *of* als de lid-portal op zowel de Azure-portal als de Everbridge-portal.

4. Het configureren van de **Everbridge** toepassing als de **Everbridge manager portal**, in de **SAML-basisconfiguratie** sectie, als volgt te werk:

    ![Everbridge domein en URL's, eenmalige aanmelding informatie](common/idp-intiated.png)

    a. In de **id** voert u een URL die het patroon `https://sso.everbridge.net/<API_Name>`

    b. In de **antwoord-URL** voert u een URL die het patroon `https://manager.everbridge.net/saml/SSO/<API_Name>/alias/defaultAlias`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden met de werkelijke waarden van de id en antwoord-URL. Als u deze waarden, neem contact op met de [Everbridge ondersteuningsteam](mailto:support@everbridge.com). Ook kunt u verwijzen naar de patronen die wordt weergegeven in de **SAML-basisconfiguratie** sectie in Azure portal.

5. Het configureren van de **Everbridge** toepassing als de **Everbridge lid portal**, in de **SAML-basisconfiguratie** sectie, als volgt te werk:

  * Als u wilt configureren van de toepassing in de modus voor IDP gestart door, volg deze stappen:

     ![Everbridge domein en URL's eenmalige aanmelding-informatie voor de modus voor IDP gestart door](common/idp-intiated.png)

    a. In de **id** voert u een URL die het patroon `https://sso.everbridge.net/<API_Name>/<Organization_ID>`

    b. In de **antwoord-URL** voert u een URL die het patroon `https://member.everbridge.net/saml/SSO/<API_Name>/<Organization_ID>/alias/defaultAlias`

   * Als u configureren van de toepassing in de modus SP geïnitieerde wilt, selecteert u **extra URL's instellen** en volgt u deze stap:

     ![Everbridge domein en URL's aanmeldings-informatie voor de modus Serviceprovider geïnitieerde eenmalige](common/both-signonurl.png)

     a. In de **aanmeldings-URL** voert u een URL die het patroon `https://member.everbridge.net/saml/login/<API_Name>/<Organization_ID>/alias/defaultAlias?disco=true`

     > [!NOTE]
     > Dit zijn geen echte waarden. Deze waarden met de werkelijke-id en antwoord-URL, bijwerken en meld u aan de URL-waarden. Als u deze waarden, neem contact op met de [Everbridge ondersteuningsteam](mailto:support@everbridge.com). Ook kunt u verwijzen naar de patronen die wordt weergegeven in de **SAML-basisconfiguratie** sectie in Azure portal.

6. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, selecteer **downloaden** voor het downloaden van de **federatieve metagegevens-XML** . Sla deze op uw computer.

    ![De koppeling om het certificaat te downloaden](common/metadataxml.png)

7. In de **Everbridge instellen** sectie, Kopieer de URL's die u nodig hebt voor uw vereisten:

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    - Aanmeldings-URL
    - Azure AD-id
    - Afmeldings-URL

### <a name="configure-everbridge-as-everbridge-manager-portal-single-sign-on"></a>Everbridge als Everbridge manager portal eenmalige aanmelding configureren

Voor het configureren van eenmalige aanmelding op **Everbridge** als een **Everbridge manager portal** toepassing, als volgt te werk.
 
1. In een ander browservenster aanmelden bij Everbridge als beheerder.

1. Selecteer in het menu bovenaan de **instellingen** tabblad. Onder **Security**, selecteer **Single Sign-On**.
   
     ![Eenmalige aanmelding configureren](./media/everbridge-tutorial/tutorial_everbridge_002.png)
   
     a. In de **naam** voert u de naam van de id-provider. Een voorbeeld is de naam van uw bedrijf.
   
     b. In de **API-naam** voert u de naam van de API.
   
     c. Selecteer **bestand kiezen** voor het uploaden van het bestand met metagegevens die u hebt gedownload van de Azure-portal.
   
     d. Voor **SAML identiteit locatie**, selecteer **identiteit is in de NameIdentifier-element van het onderwerp overzicht**.
   
     e. In de **aanmeldings-URL van id-Provider** vak, plak de **aanmeldings-URL** waarde die u hebt gekopieerd uit de Azure-portal.
   
     f. Voor **serviceprovider geïnitieerde aanvraag Binding**, selecteer **HTTP-omleiding**.

     g. Selecteer **Opslaan**.

### <a name="configure-everbridge-as-everbridge-member-portal-single-sign-on"></a>Everbridge als Everbridge lid portal eenmalige aanmelding configureren

Het configureren van eenmalige aanmelding op **Everbridge** als een **Everbridge lid portal**, verzendt de gedownloade **federatieve metagegevens-XML** naar de [Everbridge ondersteuning voor team](mailto:support@everbridge.com). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken 

Volg deze stappen voor het maken van de testgebruiker Britta Simon in Azure portal.

1. Selecteer in de Azure-portal aan de linkerkant **Azure Active Directory** > **Gebruikers** > **Alle gebruikers**.

    ![Gebruikers en alle gebruikerskoppelingen](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![De knop Nieuwe gebruiker](common/new-user.png)

3. Volg deze stappen in het dialoogvenster **Gebruiker**.

    ![In het dialoogvenster](common/user-properties.png)

    a. Voer in het vak **Naam** **Britta Simon**in.
  
    b. In de **gebruikersnaam** Voer `brittasimon@yourcompanydomain.extension`. Een voorbeeld is BrittaSimon@contoso.com.

    c. Selecteer de **wachtwoord weergeven** selectievakje. Noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Selecteer **Maken**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Everbridge inschakelen.

1. Selecteer in de Azure portal, **bedrijfstoepassingen** > **alle toepassingen** >**Everbridge**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **Everbridge**.

    ![Everbridge koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![Koppeling van gebruikers en groepen](common/users-groups-blade.png)

4. Selecteer **Gebruiker toevoegen**. In de **toevoegen toewijzing** in het dialoogvenster, selecteer **gebruikers en groepen**.

    ![Het dialoogvenster toewijzing toevoegen](common/add-assign-user.png)

5. In de **gebruikers en groepen** in het dialoogvenster, selecteer **Britta Simon** in de gebruikerslijst. Kies **Selecteren** onderaan het scherm.

6. Als u een rolwaarde verwacht in de SAML-assertie, selecteert u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst. Kies **Selecteren** onderaan het scherm.

7. Selecteer **Toewijzen** in het dialoogvenster **Toewijzing toevoegen**.

### <a name="create-an-everbridge-test-user"></a>Maak een testgebruiker Everbridge

In deze sectie maakt u de testgebruiker Britta Simon in Everbridge. Als gebruikers wilt toevoegen in het platform Everbridge, werken met de [Everbridge ondersteuningsteam](mailto:support@everbridge.com). Gebruikers moeten worden gemaakt en worden geactiveerd in Everbridge voordat u eenmalige aanmelding gebruiken. 

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

Test uw Azure AD eenmalige aanmelding-configuratie met behulp van het toegangsvenster.

Wanneer u de tegel Everbridge in het toegangsvenster selecteert, moet u worden automatisch aangemeld bij de Everbridge account waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)
- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

