---
title: 'Zelfstudie: Azure Active Directory-integratie met Fluxx Labs | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Fluxx Labs.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: d8fac770-bb57-4e1f-b50b-9ffeae239d07
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/01/2019
ms.author: jeedes
ms.openlocfilehash: e1951a65c48c32f2ce4af722400d03c20dfa684b
ms.sourcegitcommit: b8a8d29fdf199158d96736fbbb0c3773502a092d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/15/2019
ms.locfileid: "59565390"
---
# <a name="tutorial-azure-active-directory-integration-with-fluxx-labs"></a>Zelfstudie: Azure Active Directory-integratie met Fluxx Labs

In deze zelfstudie leert u hoe u Fluxx Labs te integreren met Azure Active Directory (Azure AD).
Fluxx Labs te integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot Fluxx Labs heeft.
* U kunt uw gebruikers worden automatisch aangemeld Fluxx Labs (Single Sign-On) inschakelen met hun Azure AD-accounts.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Fluxx Labs, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* Fluxx Labs eenmalige aanmelding ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor Fluxx Labs **IDP** gestart door SSO

## <a name="adding-fluxx-labs-from-the-gallery"></a>Fluxx Labs uit de galerie toe te voegen

Voor het configureren van de integratie van Fluxx Labs in Azure AD, moet u Fluxx Labs uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Fluxx Labs uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **Fluxx Labs**, selecteer **Fluxx Labs** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![Fluxx Labs in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Fluxx Labs op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Fluxx Labs tot stand worden gebracht.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Fluxx Labs, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureer Fluxx Labs Single Sign-On](#configure-fluxx-labs-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Testgebruiker Fluxx Labs maken](#create-fluxx-labs-test-user)**  : als u wilt een equivalent van Britta Simon in Fluxx Labs die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met Fluxx Labs, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **Fluxx Labs** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. Op de pagina **Eenmalige aanmelding instellen met SAML** voert u de volgende stappen uit:

    ![Fluxx Labs domein en URL's, eenmalige aanmelding informatie](common/idp-intiated.png)

    a. In het tekstvak **Id** typt u een URL met het volgende patroon:

    | Omgeving | URL-patroon|
    |-------------|------------|
    | Productie | `https://<subdomain>.fluxx.io` |
    | Testfase vóór productie | `https://<subdomain>.preprod.fluxxlabs.com`|

    b. In het tekstvak **Antwoord-URL** typt u een URL met het volgende patroon:

    | Omgeving | URL-patroon|
    |-------------|------------|
    | Productie | `https://<subdomain>.fluxx.io/auth/saml/callback` |
    | Testfase vóór productie | `https://<subdomain>.preprod.fluxxlabs.com/auth/saml/callback`|

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke id en antwoord-URL. Neem contact op met [Fluxx Labs-Client-ondersteuningsteam](mailto:travis@fluxxlabs.com) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

6. Op de **Fluxx Labs instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-fluxx-labs-single-sign-on"></a>Fluxx Labs eenmalige aanmelding configureren

1. In een ander browservenster aanmelden bij uw bedrijf Fluxx Labs site als administrator.

2. Selecteer **Admin** hieronder de **instellingen** sectie.

    ![Fluxx Labs Configuration](./media/fluxxlabs-tutorial/config1.png)

3. Selecteer in het deelvenster Beheer **Plug-ins** > **integraties** en selecteer vervolgens **SAML SSO-(Disabled)**

    ![Fluxx Labs Configuration](./media/fluxxlabs-tutorial/config2.png)

4. Voer de volgende stappen uit in de sectie kenmerk:

    ![Fluxx Labs Configuration](./media/fluxxlabs-tutorial/config3.png)

    a. Selecteer de **SAML SSO** selectievakje.

    b. In de **pad aanvraag** tekstvak, type **/auth/saml**.

    c. In de **Callback pad** tekstvak, type **/auth/saml/callback**.

    d. In de **Assertion Consumer Service Url(Single Sign-On URL)** tekstvak, voer de **antwoord-URL** waarde die u hebt opgegeven in de Azure-portal.

    e. In de **doelgroep (SP entiteits-ID)** tekstvak, voer de **id** waarde die u hebt opgegeven in de Azure-portal.

    f. In de **doel-URL van SSO-identiteitsprovider** tekstvak, plak de **aanmeldings-URL** waarde die u hebt gekopieerd vanuit Azure portal.

    g. Open het base-64 gecodeerde certificaat in Kladblok, kopieer de inhoud ervan naar het klembord en plak het in het tekstvak **Id-providercertificaat**.

    h. In **naam-id indeling** tekstvak, voert u de waarde `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`.

    i. Klik op **Opslaan**.

    > [!NOTE]
    > Zodra de opgeslagen inhoud het veld wordt weergegeven leeg voor beveiliging, maar de waarde in de configuratie is opgeslagen.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken 

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In de **gebruikersnaam** veldtype brittasimon@yourcompanydomain.extension. Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen tot Fluxx Labs.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **Fluxx Labs**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **Fluxx Labs**.

    ![De koppeling Fluxx Labs in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-fluxx-labs-test-user"></a>Testgebruiker Fluxx Labs maken

Als u wilt dat Azure AD-gebruikers kunnen zich aanmelden bij Fluxx Labs, moeten ze worden ingericht in Fluxx Labs. In het geval van Fluxx Labs is inrichten een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Meld u aan uw bedrijf Fluxx Labs site als een beheerder.

2. Klik op de hieronder weergegeven **pictogram**.

    ![Fluxx Labs Configuration](./media/fluxxlabs-tutorial/config6.png)

3. Klik op het dashboard op de hieronder weergegeven pictogram opent de **nieuwe personen** kaart.

    ![Fluxx Labs Configuration](./media/fluxxlabs-tutorial/config4.png)

4. Op de **nieuwe personen** sectie, voert u de volgende stappen uit:

    ![Fluxx Labs Configuration](./media/fluxxlabs-tutorial/config5.png)

    a. E-mail als de unieke id Fluxx Labs gebruiken voor SSO-aanmeldingen. Vul de **SSO UID** veld met de e-mailadres van de gebruiker, die overeenkomt met het e-mailadres, die ze als voor aanmelding bij met eenmalige aanmelding gebruiken.

    b. Klik op **Opslaan**.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Fluxx Labs in het toegangsvenster, moet u worden automatisch aangemeld bij de Fluxx Labs waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

