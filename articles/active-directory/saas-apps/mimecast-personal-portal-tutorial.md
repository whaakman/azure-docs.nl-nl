---
title: 'Zelfstudie: Azure Active Directory-integratie met de persoonlijke Portal Mimecast | Microsoft Docs'
description: Ontdek hoe u eenmalige aanmelding configureert tussen Azure Active Directory en Mimecast Personal Portal.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 345b22be-d87e-45a4-b4c0-70a67eaf9bfd
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/24/2018
ms.author: jeedes
ms.openlocfilehash: 24bd6fe29e05f84d78316d0bb3e8ffc0d25157f3
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54828032"
---
# <a name="tutorial-azure-active-directory-integration-with-mimecast-personal-portal"></a>Zelfstudie: Azure Active Directory-integratie met de persoonlijke Portal Mimecast

In deze zelfstudie leert u hoe u Mimecast Personal Portal kunt integreren met Azure Active Directory (Azure AD).
De integratie van Mimecast Personal Portal met Azure AD heeft de volgende voordelen:

* U kunt in Azure AD bepalen wie toegang heeft tot Mimecast Personal Portal.
* U kunt uw gebruikers zich automatisch laten aanmelden bij Mimecast Personal Portal (eenmalige aanmelding) met hun Azure AD-account.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Mimecast Personal Portal hebt u het volgende nodig:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Een abonnement op Mimecast Personal Portal waarvoor eenmalige aanmelding is ingeschakeld

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Mimecast Personal Portal ondersteunt door **SP** geïnitieerde eenmalige aanmelding (SSO)

## <a name="adding-mimecast-personal-portal-from-the-gallery"></a>Mimecast Personal Portal toevoegen vanuit de galerie

Voor het configureren van de integratie van Mimecast Personal Portal in Azure AD moet u Mimecast Personal Portal uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Voer de volgende stappen uit om Mimecast Personal Portal toe te voegen vanuit de galerie:**

1. Klik in het linkernavigatievenster in de **[Azure-portal](https://portal.azure.com)** op het **Azure Active Directory**-pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ **Mimecast Personal Portal** in het zoekvak, selecteer **Mimecast Personal Portal** in het resultaatvenster en klik op de knop **Toevoegen** om de toepassing toe te voegen.

     ![Mimecast Personal Portal in de resultatenlijst](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie gaat u Azure AD-eenmalige aanmelding met Mimecast Personal Portal configureren en testen met behulp van een testgebruiker met de naam **Britta Simon**.
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Azure AD-gebruiker en de daaraan gerelateerde gebruiker in Mimecast Personal Portal tot stand is gebracht.

Als u Azure AD-eenmalige aanmelding met Mimecast Personal Portal wilt configureren en testen, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Eenmalige aanmelding voor Mimecast Personal Portal configureren](#configure-mimecast-personal-portal-single-sign-on)**: als u de instellingen voor eenmalige aanmelding aan de clientzijde wil configureren.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Een testgebruiker maken in Mimecast Personal Portal](#create-mimecast-personal-portal-test-user)**: om in Mimecast Personal Portal een tegenhanger van Britta Simon te hebben die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voer de volgende stappen uit om Azure AD-eenmalige aanmelding bij Mimecast Personal Portal te configureren:

1. In de [Azure-portal](https://portal.azure.com/) selecteert u **Eenmalige aanmelding** op de integratiepagina van de toepassing **Mimecast Personal Portal**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Domein- en URL-gegevens voor eenmalige aanmelding bij Mimecast Personal Portal](common/sp-identifier-reply.png)

    a. Typ een URL in het tekstvak **Aanmeldings-URL**: 

    | Regio  |  Waarde | 
    | --------------- | --------------- | 
    | Europa          | `https://eu-api.mimecast.com/login/saml`|
    | Verenigde Staten   | `https://us-api.mimecast.com/login/saml`|
    | Zuid-Afrika    | `https://za-api.mimecast.com/login/saml`|
    | Australië       | `https://au-api.mimecast.com/login/saml`|
    | Offshore        | `https://jer-api.mimecast.com/login/saml`|

    b. In het tekstvak **Id** typt u een URL met het volgende patroon:

    | Regio  |  Waarde | 
    | --------------- | --------------- |
    | Europa          | `https://eu-api.mimecast.com/sso/<accountcode>`|
    | Verenigde Staten   | `https://us-api.mimecast.com/sso/<accountcode>`|    
    | Zuid-Afrika    | `https://za-api.mimecast.com/sso/<accountcode>`|
    | Australië       | `https://au-api.mimecast.com/sso/<accountcode>`|
    | Offshore        | `https://jer-api.mimecast.com/sso/<accountcode>`|

    c. Typ een URL in het tekstvak **Antwoord-URL**: 

    | Regio  |  Waarde | 
    | --------------- | --------------- | 
    | Europa          | `https://eu-api.mimecast.com/login/saml`|
    | Verenigde Staten   | `https://us-api.mimecast.com/login/saml`|
    | Zuid-Afrika    | `https://za-api.mimecast.com/login/saml`|
    | Australië       | `https://au-api.mimecast.com/login/saml`|
    | Offshore        | `https://jer-api.mimecast.com/login/saml`|

    > [!NOTE]
    > De id-waarde is niet echt. Werk de waarde bij met de werkelijke id. Neem contact op met het [ondersteuningsteam van Mimecast Personal Portal](https://www.mimecast.com/customer-success/technical-support/) om de waarde te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

4. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

6. In het gedeelte **Mimecast Personal Portal instellen** kopieert u de juiste URL('s) op basis van uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-mimecast-personal-portal-single-sign-on"></a>Eenmalige aanmelding voor Mimecast Personal Portal configureren

1. Meld u in een ander browservenster aan bij uw Mimecast Personal Portal als beheerder.

2. Ga naar **Services \> Applications**.
   
    ![Applications](./media/mimecast-personal-portal-tutorial/ic794998.png "Applications")

3. Klik op **Authentication Profiles**.
   
    ![Authentication Profiles](./media/mimecast-personal-portal-tutorial/ic794999.png "Authentication Profiles")

4. Klik op **New Authentication Profile**.
   
    ![New Authentication Profile](./media/mimecast-personal-portal-tutorial/ic795000.png "New Authentication Profile")

5. Voer in de sectie **Authentication Profile** de volgende stappen uit:
   
    ![Authentication Profile](./media/mimecast-personal-portal-tutorial/ic795001.png "Authentication Profile")
   
    a. Typ in het tekstvak **Description** een naam voor de configuratie.
   
    b. Selecteer **Enforce SAML Authentication for Mimecast Personal Portal**.
   
    c. Selecteer **Azure Active Directory** als **Provider**.
   
    d. Plak in het tekstvak **Issuer URL** de waarde van **Azure AD-id** die u hebt gekopieerd uit de Azure-portal.
   
    e. Plak in het tekstvak **Login URL** de waarde van **Aanmeldings-URL** die u hebt gekopieerd uit de Azure-portal.
   
    f. Plak in het tekstvak **Logout URL** de waarde van **Afmeldings-URL** die u hebt gekopieerd uit de Azure-portal.

    g. Open in Kladblok het **met Base 64 gecodeerde certificaat** dat u hebt gedownload uit de Azure-portal, kopieer de inhoud ervan naar het Klembord en plak deze vervolgens in het tekstvak **id-providercertificaat (metagegevens)**.

    h. Selecteer **Allow Single Sign On**.
   
    i. Klik op **Opslaan**.

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

In dit gedeelte gaat u Britta Simon toestemming geven voor gebruik van eenmalige aanmelding met Azure door haar toegang te geven tot Mimecast Personal Portal.

1. Selecteer in Azure Portal de optie **Bedrijfstoepassingen**, selecteer **Alle toepassingen**en selecteer vervolgens **Mimecast Personal Portal**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Typ **Mimecast Personal Portal** in de lijst met toepassingen en selecteer het.

    ![De koppeling Mimecast Personal Portal in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-mimecast-personal-portal-test-user"></a>Een testgebruiker maken in Mimecast Personal Portal

Als u wilt dat Azure AD-gebruikers zich kunnen aanmelden bij Mimecast Personal Portal, moeten ze worden ingericht in Mimecast Personal Portal. In het geval van Mimecast Personal Portal is het inrichten een handmatige taak.

U moet een domein registreren voordat u gebruikers kunt maken.

**Voer de volgende stappen uit om de gebruikersinrichting te configureren:**

1. Meld u aan bij uw **Mimecast Personal Portal** als beheerder.

2. Ga naar **Directories \> Internal**.
   
    ![Directories](./media/mimecast-personal-portal-tutorial/ic795003.png "Directories")

3. Klik op **Register New Domain**.
   
    ![Register New Domain](./media/mimecast-personal-portal-tutorial/ic795004.png "Register New Domain")

4. Nadat het nieuwe domein is gemaakt, klikt u op **New Address**.
   
    ![New Address](./media/mimecast-personal-portal-tutorial/ic795005.png "New Address")

5. Voer in het dialoogvenster New Address de volgende stappen uit voor een geldig Azure AD-account dat u wilt inrichten:
   
    ![Save](./media/mimecast-personal-portal-tutorial/ic795006.png "Save")
   
    a. Typ in het tekstvak **Email Address** het **e-mailadres** van de gebruiker: **BrittaSimon@contoso.com**.
    
    b. Typ in het tekstvak **Global Name** de **gebruikersnaam**: **BrittaSimon**.

    c. Typ in de tekstvakken **Password** en **Confirm Password** het **wachtwoord** van de gebruiker.
   
    b. Klik op **Opslaan**.

>[!NOTE]
>U kunt ook alle andere hulpprogramma's voor het creëren van Mimecast Personal Portal-gebruikersaccounts of API's van Mimecast Personal Portal gebruiken om Azure AD-gebruikersaccounts in te richten.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie gaat u uw configuratie van Azure AD-eenmalige aanmelding testen via het toegangsvenster.

Wanneer u op de tegel Mimecast Personal Portal in het toegangsvenster klikt, wordt u automatisch aangemeld bij de instantie van Mimecast Personal Portal waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

