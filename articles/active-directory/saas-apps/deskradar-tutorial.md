---
title: 'Zelfstudie: Azure Active Directory-integratie met Deskradar | Microsoft Docs'
description: Ontdek hoe u eenmalige aanmelding configureert tussen Azure Active Directory en Deskradar.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 5ff3e014-b79f-4a6c-bb0b-38462bea5d10
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/04/2019
ms.author: jeedes
ms.openlocfilehash: 41846e2e7fd047ca573892e39d960a353bc7fd18
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54079790"
---
# <a name="tutorial-azure-active-directory-integration-with-deskradar"></a>Zelfstudie: Azure Active Directory-integratie met Deskradar

In deze zelfstudie leert u hoe u Deskradar integreert met Azure Active Directory (Azure AD).
Integratie van Deskradar met Azure AD heeft de volgende voordelen:

* U kunt in Azure AD beheren wie toegang heeft tot Deskradar.
* U kunt instellen dat gebruikers automatisch met hun Azure AD-account worden aangemeld bij Deskradar (eenmalige aanmelding).
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor de configuratie van Azure AD-integratie met Deskradar, hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Een abonnement op Deskradar waarvoor eenmalige aanmelding is ingeschakeld

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Deskradar ondersteunt door **SP en IDP** geïnitieerde eenmalige aanmelding (SSO)

## <a name="add-deskradar-from-the-gallery"></a>Deskradar toevoegen vanuit de galerie

Voor het configureren van de integratie van Deskradar in Azure AD, moet u Deskradar vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u Deskradar wilt toevoegen vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. Klik in het linkernavigatievenster in de **[Azure-portal](https://portal.azure.com)** op het **Azure Active Directory**-pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ **Deskradar** in het zoekvak, selecteer **Deskradar** in het resultaatvenster en klik vervolgens op de knop **Toevoegen** om de toepassing toe te voegen.

     ![Deskradar in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie gaat u Azure AD-eenmalige aanmelding met [toepassingsnaam] configureren en testen met behulp van een testgebruiker met de naam **Britta Simon**.
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Azure AD-gebruiker en de daaraan gerelateerd gebruiker in [toepassingsnaam] tot stand is gebracht.

Als u Azure AD-eenmalige aanmelding met [toepassingsnaam] wilt configureren en testen, moet u de volgende stappen uitvoeren:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Eenmalige aanmelding voor Deskradar configureren](#configure-deskradar-single-sign-on)**: de instellingen voor eenmalige aanmelding aan de clientzijde configureren.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Een testgebruiker maken voor Deskradar](#create-deskradar-test-user)**: als u in Deskradar een equivalent van Britta Simon wilt die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD-eenmalige aanmelding met [toepassingsnaam], moet u de volgende stappen uitvoeren:

1. Ga in de [Azure-portal](https://portal.azure.com/) naar de pagina voor de integratie van de toepassing **Deskradar** en selecteer **Eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit als u de toepassing in de door **IDP** geïnitieerde modus wilt configureren:

    ![Informatie over eenmalige aanmelding bij het Deskradar-domein en URL's](common/idp-intiated.png)

    a. In het tekstvak **Id** typt u een URL met het volgende patroon: `https://YOURDOMAIN.deskradar.cloud`

    b. In het tekstvak **Antwoord-URL** typt u een URL met het volgende patroon: `https://YOURDOMAIN.deskradar.cloud/auth/sso/saml/consume`

5. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    In het tekstvak **Aanmeldings-URL** typt u een URL met het volgende patroon: `https://YOURDOMAIN.deskradar.cloud/auth/sso/saml/login`

    ![Informatie over eenmalige aanmelding bij het Deskradar-domein en URL's](common/metadata-upload-additional-signon.png)

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke-id, de antwoord-URL en de aanmeldings-URL. Vervang **UWDOMEIN** door het domein van uw Deskradar-exemplaar. Neem contact op met het [Deskradar-clientondersteuningsteam](mailto:support@deskradar.com) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

6. De toepassing Deskradar verwacht de SAML-asserties in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit de sectie **Gebruikerskenmerken** op de integratiepagina van de toepassing-beheren. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op de knop **Bewerken** om het dialoogvenster **Gebruikerskenmerken** te openen.

    ![image](common/edit-attribute.png)

7. In de sectie **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** configureert u het kenmerk van het SAML-token zoals wordt weergegeven in de bovenstaande afbeelding en voert u de volgende stappen uit:

    | Naam | Bronkenmerk|
    | ---------------| --------- |
    | FirstName | user.givenname |
    | LastName | user.surname |
    | Email | user.userprincipalname |
    | | |

    a. Klik op **Nieuwe claim toevoegen** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Laat **Naamruimte** leeg.

    d. Selecteer Bron bij **Kenmerk**.

    e. Typ de kenmerkwaarde voor die rij in de lijst met **bronkenmerken**.

    f. Klik op **OK**.

    g. Klik op **Opslaan**.

8. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

9. In de sectie **Deskradar instellen** kopieert u de juiste URL('s) op basis van uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-deskradar-single-sign-on"></a>Deskradar configureren voor eenmalige aanmelding

1. Meld u bij uw Deskradar-exemplaar aan met het administrator-account en gebruik het e-mailadres en wachtwoord dat u hebt gedefinieerd toen u zich registreerde met behulp van een uitnodigingskoppeling.

2. Open het deelvenster **Team** door te klikken op het pictogram in de zijbalk.

3. Ga naar het tabblad **Verificatie**.

4. Voer op het tabblad **SAML 2.0** de volgende stappen uit:

    ![Configuratie van Deskradar](./media/deskradar-tutorial/14-paste-urls.jpg)

    a. Schakel de verificatiemethode **SAML** in.

    b. Voer in het tekstvak **URL voor eenmalige aanmelding met SAML** de waarde van de **aanmeldings-URL** in die u uit de Azure-portal hebt gekopieerd.

    c. Voer in het tekstvak **URL van id-provider** de waarde van de **Azure Ad-id** in die u hebt gekopieerd uit de Azure-portal.

5. Open het gedownloade **certificaatbestand (Base64)** met een teksteditor en kopieer en plak de inhoud ervan in het veld **Openbaar certificaat** in Deskradar.

    ![Configuratie van Deskradar](./media/deskradar-tutorial/15-paste-certificate.jpg)

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

In dit gedeelte gaat u Britta Simon toestemming geven voor gebruik van eenmalige aanmelding met Azure door haar toegang te geven tot Deskradar.

1. Selecteer **Bedrijfstoepassingen** in de Azure-portal, selecteer **Alle toepassingen** en selecteer vervolgens **Deskradar**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer **Deskradar** in de lijst met toepassingen.

    ![De koppeling Deskradar in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-deskradar-test-user"></a>Een Deskradar-testgebruiker maken

In deze sectie maakt u een gebruiker met de naam Britta Simon in Deskradar. Neem contact op met het  [Deskradar-clientondersteuningsteam](mailto:support@deskradar.com)  als u gebruikers wilt toevoegen in het Deskradar-platform. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie gaat u uw configuratie van Azure AD-eenmalige aanmelding testen via het toegangsvenster.

**Volg onderstaande stappen als u de toepassing wilt gebruiken in de door SP geïnitieerde modus:**

1. Start uw Deskradar-exemplaar door de URL te openen in uw browser: `https://YOURDOMAIN.deskradar.cloud` (vervang `YOURDOMAIN` door het domein van uw Deskradar-exemplaar ). 
1. Selecteer **Eenmalige aanmelding in de organisatie**.

    ![Deskradar](./media/deskradar-tutorial/16-sso-button.jpg)

- U wordt direct aangemeld bij Deskradar als u al bent aangemeld in Active Directory.
- U wordt omgeleid naar het aanmeldingsformulier van Active Directory als u nog niet bent aangemeld. U kunt daar de referenties van uw zakelijk account invoeren om u aan te melden.
- U gaat naar Deskradar en wordt bij Deskradar aangemeld na een geslaagde aanmelding met de referenties van uw zakelijk account.

**Volg onderstaande stap als u de toepassing wilt gebruiken in de door IDP geïnitieerde modus:**

Wanneer u in het toegangsvenster op de tegel Deskradar klikt, zou u automatisch moeten worden aangemeld bij het exemplaar van Deskradar waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
