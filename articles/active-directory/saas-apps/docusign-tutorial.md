---
title: 'Zelfstudie: Integratie met DocuSign Azure Active Directory | Microsoft Docs'
description: Meer informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en DocuSign.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: a691288b-84c1-40fb-84bd-5b06878865f0
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/01/2019
ms.author: jeedes
ms.openlocfilehash: 5c7d6116ed2925e57f094a67f27a11f9e2d61831
ms.sourcegitcommit: a0b37e18b8823025e64427c26fae9fb7a3fe355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2019
ms.locfileid: "68499248"
---
# <a name="tutorial-azure-active-directory-integration-with-docusign"></a>Zelfstudie: Integratie met DocuSign Azure Active Directory

In deze zelf studie leert u hoe u DocuSign integreert met Azure Active Directory (Azure AD).
Het integreren van DocuSign met Azure AD biedt de volgende voor delen:

* U kunt beheren in azure AD die toegang heeft tot DocuSign.
* U kunt ervoor zorgen dat uw gebruikers automatisch worden aangemeld bij DocuSign (eenmalige aanmelding) met hun Azure AD-accounts.
* U kunt uw accounts in één centrale locatie - Azure portal beheren.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Als u Azure AD-integratie met DocuSign wilt configureren, hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u een [gratis account](https://azure.microsoft.com/free/) aanvragen
* Abonnement voor eenmalige aanmelding DocuSign ingeschakeld

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* DocuSign ondersteunt door **SP** GEÏNITIEERDe SSO

* DocuSign ondersteunt [automatische gebruikers inrichting](https://docs.microsoft.com/azure/active-directory/saas-apps/dropboxforbusiness-provisioning-tutorial)

## <a name="adding-docusign-from-the-gallery"></a>DocuSign toevoegen uit de galerie

Als u de integratie van DocuSign in azure AD wilt configureren, moet u DocuSign uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Voer de volgende stappen uit om DocuSign toe te voegen uit de galerie:**

1. In de **[Azure-portal](https://portal.azure.com)** , klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing](common/add-new-app.png)

4. Typ **DocuSign**in het zoekvak, selecteer **DocuSign** in het resultaten paneel en klik vervolgens op de knop **toevoegen** om de toepassing toe te voegen.

     ![DocuSign in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie kunt u eenmalige aanmelding voor Azure AD configureren en testen met DocuSign op basis van een test gebruiker met de naam **Julia Simon**.
Voor een goede werking van eenmalige aanmelding moet er een koppelings relatie tussen een Azure AD-gebruiker en de bijbehorende gebruiker in DocuSign tot stand worden gebracht.

Als u eenmalige aanmelding voor Azure AD wilt configureren en testen met DocuSign, moet u de volgende bouw stenen volt ooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[DocuSign eenmalige aanmelding configureren](#configure-docusign-single-sign-on)** : Hiermee configureert u de instellingen voor eenmalige aanmelding aan de kant van de toepassing.
3. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Maak een DocuSign-test gebruiker](#create-docusign-test-user)** -om een equivalent van Julia Simon in DocuSign te hebben dat is gekoppeld aan de Azure AD-representatie van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voer de volgende stappen uit om eenmalige aanmelding voor Azure AD te configureren met DocuSign:

1. Selecteer in de [Azure Portal](https://portal.azure.com/)op de pagina **DocuSign** Application Integration de optie **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Informatie over eenmalige aanmelding voor DocuSign domein en Url's](common/sp-identifier.png)

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<subdomain>.docusign.com/organizations/<OrganizationID>/saml2/login/sp/<IDPID>`

    b. In het tekstvak **Id (Entiteits-id)** typt u een URL met de volgende notatie: `https://<subdomain>.docusign.com/organizations/<OrganizationID>/saml2`

    > [!NOTE]
    > Dit zijn geen echte waarden. Als u deze waarden bijwerkt met de werkelijke aanmeldings-URL en de id, die later wordt uitgelegd, wordt de sectie **SAML 2,0-eind punten weer gegeven** in de zelf studie.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De downloadkoppeling certificaat](common/certificatebase64.png)

6. Kopieer op de sectie **DocuSign instellen** de gewenste URL ('s) volgens uw vereiste.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. URL voor afmelden

### <a name="configure-docusign-single-sign-on"></a>Eenmalige aanmelding voor DocuSign configureren

1. Meld u in een ander webbrowser venster als beheerder aan bij uw **DocuSign-beheer Portal** .

2. Klik in de rechter bovenhoek van de pagina op profiel **logo** en klik vervolgens op **Ga naar beheerder**.
  
    ![Eenmalige aanmelding configureren][51]

3. Klik op de pagina domein oplossingen op **domeinen**

    ![Eenmalige aanmelding configureren][50]

4. Klik onder de sectie **domeinen** op **claim domein**.

    ![Eenmalige aanmelding configureren][52]

5. Typ in het tekstvak **domein naam** van het dialoog venster **een domein claim** het domein van uw bedrijf en klik vervolgens op **claim**. Zorg ervoor dat u het domein verifieert en of de status actief is.

    ![Eenmalige aanmelding configureren][53]

6. Klik op de pagina domein oplossingen op **id-providers**.
  
    ![Eenmalige aanmelding configureren][54]

7. Klik onder **id-providers** op **ID-provider toevoegen**. 

    ![Eenmalige aanmelding configureren][55]

8. Voer de volgende stappen uit op de pagina instellingen van de **identiteits provider** :

    ![Eenmalige aanmelding configureren][56]

    a. Typ in het tekstvak **naam** een unieke naam voor uw configuratie. Gebruik geen spaties.

    b. Plak in het tekstvak voor de uitgever van de **identiteits provider**de waarde van de **Azure ad-id**die u van Azure Portal hebt gekopieerd.

    c. Plak in het tekstvak **aanmeldings-URL van ID-provider** de waarde van de AANMELDINGS- **URL**die u hebt gekopieerd uit Azure Portal.

    d. Plak in het tekstvak afmeldings-URL van de **identiteits provider** de waarde van de afmeldings- **URL**, die u van Azure Portal hebt gekopieerd.

    e. Selecteer **authn-aanvraag ondertekenen**.

    f. Selecteer als **verificatie aanvraag verzenden door** **post**.

    g. Selecteer **ophalen**bij afmeldings **aanvraag verzenden door**.

    h. Klik in de sectie **toewijzing van aangepast kenmerk** op **nieuwe toewijzing toevoegen**.

    ![Eenmalige aanmelding configureren][62]

    i. Kies het veld dat u wilt toewijzen aan de Azure AD-claim. In dit voor beeld wordt de claim **EmailAddress** toegewezen aan de waarde van **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress** . Het is de standaard naam van de claim van Azure AD voor de e-mail claim en klik vervolgens op **Opslaan**.

    ![Eenmalige aanmelding configureren][57]

    > [!NOTE]
    > Gebruik de juiste **gebruikers-id** om de gebruiker toe te wijzen vanuit Azure AD aan DocuSign-gebruikers toewijzing. Selecteer het juiste veld en voer de juiste waarde in op basis van de instellingen van uw organisatie.

    j. Klik in de sectie **identiteits provider certificaten** op **certificaat toevoegen**en upload het certificaat dat u hebt gedownload van de Azure AD-Portal en klik op **Opslaan**.

    ![Eenmalige aanmelding configureren][58]

    k. Klik in de sectie **id-providers** op **acties**en klik vervolgens op **eind punten**.

    ![Eenmalige aanmelding configureren][59]

    l. Voer de volgende stappen uit in de sectie **SAML 2,0-eind punten weer geven** in de **Beheer Portal van DocuSign**:

    ![Eenmalige aanmelding configureren][60]

    * Kopieer de URL van de uitgever van de **service provider**en plak deze in het tekstvak **id** in de sectie **basis configuratie van SAML** op het Azure Portal.

    * Kopieer de **aanmeldings-URL van de service provider**en plak deze in het TEKSTVAK **aanmeld URL** in het gedeelte **basis configuratie van SAML** in het Azure Portal.

    * Klik op **sluiten**

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD 

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. Typbrittasimon@yourcompanydomain.extensionin het veld **gebruikers naam** . Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Julia Simon in om eenmalige aanmelding van Azure te gebruiken door toegang te verlenen aan DocuSign.

1. Selecteer in het Azure Portal **bedrijfs toepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **DocuSign**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst toepassingen de optie **DocuSign**.

    ![De koppeling DocuSign in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-docusign-test-user"></a>DocuSign-test gebruiker maken

In deze sectie wordt een gebruiker met de naam Julia Simon gemaakt in DocuSign. DocuSign biedt ondersteuning voor Just-in-time-gebruikers inrichting, die standaard is ingeschakeld. Er is geen actie-item voor u in deze sectie. Als een gebruiker nog niet bestaat in DocuSign, wordt er een nieuwe gemaakt na verificatie.

>[!Note]
>Als u hand matig een gebruiker moet maken, neemt u contact op met het ondersteunings [team van DocuSign](https://support.docusign.com/).

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel DocuSign in het toegangs venster klikt, moet u automatisch worden aangemeld bij de DocuSign waarvoor u SSO hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

<!--Image references-->

[50]: ./media/docusign-tutorial/tutorial_docusign_18.png
[51]: ./media/docusign-tutorial/tutorial_docusign_21.png
[52]: ./media/docusign-tutorial/tutorial_docusign_22.png
[53]: ./media/docusign-tutorial/tutorial_docusign_23.png
[54]: ./media/docusign-tutorial/tutorial_docusign_19.png
[55]: ./media/docusign-tutorial/tutorial_docusign_20.png
[56]: ./media/docusign-tutorial/tutorial_docusign_24.png
[57]: ./media/docusign-tutorial/tutorial_docusign_25.png
[58]: ./media/docusign-tutorial/tutorial_docusign_26.png
[59]: ./media/docusign-tutorial/tutorial_docusign_27.png
[60]: ./media/docusign-tutorial/tutorial_docusign_28.png
[61]: ./media/docusign-tutorial/tutorial_docusign_29.png
[62]: ./media/docusign-tutorial/tutorial_docusign_30.png
