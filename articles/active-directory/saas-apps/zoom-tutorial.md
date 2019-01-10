---
title: 'Zelfstudie: Azure Active Directory-integratie met Zoom | Microsoft Docs'
description: Ontdek hoe u eenmalige aanmelding configureert tussen Azure Active Directory en Zoom.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 0ebdab6c-83a8-4737-a86a-974f37269c31
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/24/2018
ms.author: jeedes
ms.openlocfilehash: a9c0cf9dbe14478d805ff84aa480db0f9fac5d2c
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2019
ms.locfileid: "53971883"
---
# <a name="tutorial-azure-active-directory-integration-with-zoom"></a>Zelfstudie: Azure Active Directory-integratie met Zoom

In deze zelfstudie leert u hoe u Zoom kunt integreren met Azure Active Directory (Azure AD).
Zoom integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD wie toegang tot Zoom heeft.
* U kunt inschakelen dat gebruikers automatisch met hun Azure AD-account worden aangemeld bij Zoom (eenmalige aanmelding).
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Zoom hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Een abonnement op Zoom waarvoor eenmalige aanmelding is ingeschakeld

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Zoom ondersteunt door **SP** geïnitieerde eenmalige aanmelding

## <a name="adding-zoom-from-the-gallery"></a>Zoom toevoegen vanuit de galerie

Voor het configureren van de integratie van Zoom in Azure AD moet u Zoom uit de galerie aan uw lijst met beheerde SaaS-apps toevoegen.

**Als u Zoom uit de galerie wilt toevoegen, moet u de volgende stappen uitvoeren:**

1. Klik in het linkernavigatievenster in de **[Azure-portal](https://portal.azure.com)** op het **Azure Active Directory**-pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **Zoom**, selecteer **Zoom** in het deelvenster met resultaten en klik vervolgens op **Toevoegen** om de toepassing toe te voegen.

     ![Zoom in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie configureert en test u Azure AD-eenmalige aanmelding met Zoom op basis van een testgebruiker met de naam **Britta Simon**.
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Azure AD-gebruiker en de daaraan gerelateerde gebruiker in Zoom tot stand is gebracht.

Als u Azure AD eenmalige aanmelding met Zoom wilt configureren en testen, moet u de volgende bouwstenen uitvoeren:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Zoom-eenmalige aanmelding configureren](#configure-zoom-single-sign-on)**: als u de instellingen voor eenmalige aanmelding aan de clientzijde wilt configureren.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Zoom-testgebruiker maken](#create-zoom-test-user)**: als u wilt een equivalent van Britta Simon in Zoom wilt hebben dat is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met Zoom, moet u de volgende stappen uitvoeren:

1. In [Azure Portal](https://portal.azure.com/) selecteert u op de integratiepagina van de **Zoom**-toepassing de optie **Eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Informatie over eenmalige aanmelding bij het Zoom-domein en Zoom-URL's](common/sp-identifier.png)

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met de volgende notatie: `https://<companyname>.zoom.us`

    b. In het tekstvak **Id (Entiteits-id)** typt u een URL met de volgende notatie: `<companyname>.zoom.us`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke aanmeldings-URL en -id. Neem contact op met het [Zoom-ondersteuningsteam](https://support.zoom.us/hc/en-us) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Zoom verwacht dat de SAML-asserties een specifieke indeling hebben. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit de sectie **Gebruikerskenmerken** op de integratiepagina van de toepassing-beheren. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op de knop **Bewerken** om het dialoogvenster **Gebruikerskenmerken** te openen.

    ![image](common/edit-attribute.png)

6. In de sectie **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** configureert u het kenmerk van het SAML-token zoals wordt weergegeven in de bovenstaande afbeelding en voert u de volgende stappen uit:
    
    | Naam | Naamruimte  |  Bronkenmerk|
    | ---------------| --------------- | --------- |
    | E-mailadres  | user.mail  | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/mail |
    | Voornaam  | user.givenname  | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname |
    | Achternaam  | user.surname  | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname |
    | Telefoonnummer  | User.telephonenumber  | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/phone |
    | Afdeling  | user.department  | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/department |

    a. Klik op **Nieuwe claim toevoegen** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Selecteer Bron bij **Kenmerk**.

    d. Typ de kenmerkwaarde voor die rij in de lijst met **bronkenmerken**.

    e. Klik op **OK**.

    f. Klik op **Opslaan**.

4. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

6. In de sectie **Zoom instellen** kopieert u de juiste URL('s) op basis van uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-zoom-single-sign-on"></a>Zoom-eenmalige aanmelding configureren

1. Meld u in een ander webbrowservenster aan als beheerder bij uw Zoom-bedrijfswebsite.

2. Klik op het tabblad **Eenmalige aanmelding**.
   
    ![Tabblad Eenmalige aanmelding](./media/zoom-tutorial/IC784700.png "Eenmalige aanmelding")

3. Klik op het tabblad **Beveiligingsbeheer** en ga vervolgens naar de instellingen voor **Eenmalige aanmelding**.

4. Voer in de sectie Eenmalige aanmelding de volgende stappen uit:
   
    ![Sectie Eenmalige aanmelding](./media/zoom-tutorial/IC784701.png "Eenmalige aanmelding")
   
    a. Plak in het tekstvak **Aanmeldingspagina-URL** de waarde van **Aanmeldings-URL** die u hebt gekopieerd uit Azure Portal.
   
    b. Plak in het tekstvak **Afmeldingspagina-URL** de waarde van **Afmeldings-URL** die u hebt gekopieerd uit Azure Portal.
     
    c. Open het base-64 gecodeerde certificaat in Kladblok, kopieer de inhoud ervan naar het klembord en plak het in het tekstvak **Id-providercertificaat**.

    d. Plak in het tekstvak **Verlener** de waarde van **Azure AD-id**, die u hebt gekopieerd uit Azure Portal. 

    e. Klik op **Opslaan**.

    > [!NOTE] 
    > Ga naar de Zoom-documentatie voor meer informatie[https://zoomus.zendesk.com/hc/articles/115005887566](https://zoomus.zendesk.com/hc/articles/115005887566)

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

In dit gedeelte gaat u Britta Simon toestemming geven voor gebruik van eenmalige aanmelding met Azure door haar toegang te geven tot Zoom.

1. Selecteer **Bedrijfstoepassingen** in Azure Portal, selecteer **Alle toepassingen** en selecteer vervolgens **Zoom**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Typ en selecteer **Zoom** in de lijst met toepassingen.

    ![De koppeling Zoom in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-zoom-test-user"></a>Zoom-testgebruiker maken

Als u wilt dat Azure AD-gebruikers zich kunnen aanmelden bij Zoom, moeten ze worden ingericht voor Zoom. In het geval van Zoom is inrichten een handmatige taak.

### <a name="to-provision-a-user-account-perform-the-following-steps"></a>Voer de volgende stappen uit als u een gebruikersaccount wilt inrichten:

1. Meld u aan als beheerder bij uw **Zoom**-bedrijfswebsite.
 
2. Klik op het tabblad **Accountbeheer** en klik vervolgens op **Gebruikersbeheer**.

3. Klik in de sectie Gebruikersbeheer op **Gebruikers toevoegen**.
   
    ![Gebruikersbeheer](./media/zoom-tutorial/IC784703.png "Gebruikersbeheer")

4. Voer op de pagina **Gebruikers toevoegen** de volgende stappen uit:
   
    ![Gebruikers toevoegen](./media/zoom-tutorial/IC784704.png "Gebruikers toevoegen")
   
    a. Selecteer als **Gebruikerstype**, **Standaard**.

    b. Typ in het tekstvak **E-mails** het e-mailadres van een geldig Azure AD-account dat u wilt inrichten.

    c. Klik op **Add**.

> [!NOTE]
> U kunt alle andere hulpprogramma's of API's voor het maken van Zoom-gebruikersaccounts gebruiken die worden geleverd door Zoom voor het inrichten van Azure Active Directory-gebruikersaccounts.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie gaat u uw configuratie van Azure AD-eenmalige aanmelding testen via het toegangsvenster.

Wanneer u op de tegel Zoom in het toegangsvenster klikt, wordt u automatisch aangemeld bij de instantie van Zoom waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

