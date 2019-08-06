---
title: 'Zelfstudie: Azure Active Directory integratie met LinkedIn-verhoging | Microsoft Docs'
description: Meer informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en LinkedIn-bevoegdheden.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 2ad9941b-c574-42c3-bd0f-5d6ec68537ef
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/17/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5b7cb8d6ab34a632e36ea2fd1c87005a038bc523
ms.sourcegitcommit: 3073581d81253558f89ef560ffdf71db7e0b592b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68823714"
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-elevate"></a>Zelfstudie: Integratie met LinkedIn-bevoegdheden Azure Active Directory

In deze zelf studie leert u hoe u de verhoging van LinkedIn kunt integreren met Azure Active Directory (Azure AD).
Het integreren van LinkedIn-bevoegdheden met Azure AD biedt de volgende voor delen:

* U kunt beheren in azure AD die toegang heeft tot een LinkedIn-verhoging.
* U kunt ervoor zorgen dat uw gebruikers automatisch worden aangemeld bij LinkedIn (eenmalige aanmelding) met hun Azure AD-accounts.
* U kunt uw accounts in één centrale locatie - Azure portal beheren.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Als u Azure AD-integratie met LinkedIn wilt configureren, hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u een [gratis account](https://azure.microsoft.com/free/) aanvragen
* Abonnement voor het inschakelen van eenmalige aanmelding met LinkedIn

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* De LinkedIn-uitbrei ding ondersteunt SSO **met SP en IDP**

* Het niveau van LinkedIn-bevoegdheden ondersteunt **just-in-time** -gebruikers inrichting

* LinkedIn-uitbrei ding ondersteunt [ **geautomatiseerde** gebruikers inrichting](linkedinelevate-provisioning-tutorial.md)

## <a name="adding-linkedin-elevate-from-the-gallery"></a>LinkedIn-verhoging toevoegen vanuit de galerie

Als u de integratie van LinkedIn-bevoegdheden wilt configureren in azure AD, moet u een uitbrei ding op LinkedIn toevoegen vanuit de galerie naar uw lijst met beheerde SaaS-apps.

**Voer de volgende stappen uit om LinkedIn-verhoging toe te voegen vanuit de galerie:**

1. In de **[Azure-portal](https://portal.azure.com)** , klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak LinkedIn- **bevoegdheden**, selecteer een uitbrei **ding op LinkedIn** in het resultaten paneel en klik vervolgens op knop **toevoegen** om de toepassing toe te voegen.

    ![LinkedIn-verhoging in de resultaten lijst](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie kunt u eenmalige aanmelding voor Azure AD configureren en testen met LinkedIn-verhoging op basis van een test gebruiker met de naam **Julia Simon**.
Voor een goede werking van eenmalige aanmelding moet een koppelings relatie tussen een Azure AD-gebruiker en de bijbehorende gebruiker in LinkedIn-uitbrei ding tot stand worden gebracht.

Als u eenmalige aanmelding voor Azure AD wilt configureren en testen met LinkedIn-bevoegdheden, moet u de volgende bouw stenen volt ooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[LinkedIn-uitbrei ding voor eenmalige aanmelding configureren](#configure-linkedin-elevate-single-sign-on)** : Hiermee configureert u de instellingen voor eenmalige aanmelding aan de kant van de toepassing.
3. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Maak een gebruiker met een verhoogde bevoegdheid](#create-linkedin-elevate-test-user)** voor het maken van een gebruikers-naar-Julia Simon in LinkedIn-bevoegdheden die is gekoppeld aan de Azure AD-representatie van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voer de volgende stappen uit om eenmalige aanmelding voor Azure AD te configureren met LinkedIn-bevoegdheden:

1. Selecteer in de [Azure Portal](https://portal.azure.com/)op de pagina LinkedIn-toepassing voor het **uitbreiden** van de toepassings integratie de optie **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In het gedeelte **Standaard SAML-configuratie** voert u de volgende stappen uit als u de toepassing in de door **IDP** geïnitieerde modus wilt configureren:

    ![Meer informatie over het verhogen van domein en Url's voor eenmalige aanmelding](common/idp-intiated.png)

    a. Voer in het tekstvak **id** de waarde voor de **entiteits-** id in, die u in de LinkedIn-Portal kunt kopiëren. dit wordt verderop in deze zelf studie uitgelegd.

    b. In het tekstvak **antwoord-URL** voert u de waarde voor de **Assertion Consumer Access (ACS)** -URL in. u kopieert de URL-waarde voor Assertion Consumer Access (ACS) van de LinkedIn-Portal verderop in deze zelf studie.

5. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    ![Meer informatie over het verhogen van domein en Url's voor eenmalige aanmelding](common/metadata-upload-additional-signon.png)

    In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://www.linkedin.com/checkpoint/enterprise/login/<AccountId>?application=elevate&applicationInstanceId=<InstanceId>`

6. Voor de toepassing LinkedIn Elevation wordt de SAML-beweringen in een specifieke indeling verwacht. hiervoor moet u aangepaste kenmerk toewijzingen toevoegen aan de configuratie van uw SAML-token kenmerken. In de volgende schermafbeelding ziet u de lijst met standaardkenmerken, waarbij **nameidentifier** is toegewezen aan **user.userprincipalname**. Voor de toepassing LinkedIn Elevation wordt verwacht dat nameidentifier wordt toegewezen aan **User. mail**, dus u moet de kenmerk toewijzing bewerken door op het pictogram bewerken te klikken en de kenmerk toewijzing te wijzigen.

    ![image](common/edit-attribute.png)

7. Naast de bovenstaande voor LinkedIn-toepassingen met verhoogde bevoegdheid, verwachten we nog enkele kenmerken te worden door gegeven in het SAML-antwoord. Voer in de sectie gebruikers claims van het dialoog venster **gebruikers kenmerken** de volgende stappen uit om het SAML-token kenmerk toe te voegen, zoals wordt weer gegeven in de onderstaande tabel:

    | Name | Bronkenmerk|
    | -------| -------------|
    | Afdeling | user.department |

    a. Klik op **Nieuwe claim toevoegen** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Laat **Naamruimte** leeg.

    d. Selecteer Bron bij **Kenmerk**.

    e. Typ de kenmerkwaarde voor die rij in de lijst met **bronkenmerken**.

    f. Klik op **OK**.

    g. Klik op **Opslaan**.

8. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De downloadkoppeling certificaat](common/metadataxml.png)

9. Kopieer op de sectie **LinkedIn-uitbrei ding instellen** de gewenste URL ('s) volgens uw vereiste.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. URL voor afmelden

### <a name="configure-linkedin-elevate-single-sign-on"></a>Een eenmalige aanmelding voor LinkedIn-verhoging configureren

1. Meld u in een ander browser venster aan bij uw LinkedIn-Tenant met verhoogde bevoegdheden als beheerder.

1. Klik in **Accountcentrum** onder **Instellingen** op **Algemene instellingen**. Selecteer ook de optie voor het uitbreiden van de **Aad-test** in de vervolg keuzelijst.

    ![Eenmalige aanmelding configureren](./media/linkedinelevate-tutorial/tutorial_linkedin_admin_01.png)

1. Klik op **of klik hier om afzonderlijke velden uit het formulier te laden en te kopiëren** en de volgende stappen uit te voeren:

    ![Eenmalige aanmelding configureren](./media/linkedinelevate-tutorial/tutorial_linkedin_admin_03.png)

    a. Kopieer de **Entiteits-ID** en plak deze in het tekstvak **id** in de basis- **SAML-configuratie** in de Azure Portal.

    b. Kopieer de URL van de **Assertion Consumer toegang (ACS)** en plak deze in het tekstvak **antwoord-URL** in de **basis-SAML-configuratie** in de Azure Portal.

1. Ga naar het gedeelte **met beheerdersinstellingen voor LinkedIn**. Upload het XML-bestand dat u hebt gedownload van de Azure Portal door te klikken op de optie XML-bestand uploaden.

    ![Eenmalige aanmelding configureren](./media/linkedinelevate-tutorial/tutorial_linkedin_metadata_03.png)

1. Klik op **Aan** om SSO in te schakelen. De SSO-status wordt gewijzigd van **niet verbonden** met **verbonden**

    ![Eenmalige aanmelding configureren](./media/linkedinelevate-tutorial/tutorial_linkedin_admin_05.png)

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. Typ`brittasimon@yourcompanydomain.extension`in het veld **gebruikers naam** . Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Julia Simon in om gebruik te maken van eenmalige aanmelding van Azure door toegang te verlenen aan een hoger niveau.

1. Selecteer in het Azure Portal **bedrijfs toepassingen**, selecteer **alle toepassingen**en selecteer vervolgens de optie **LinkedIn-bevoegdheden**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst toepassingen de optie **LinkedIn-verhoging**.

    ![De koppeling voor het uitbreiden van de LinkedIn-verbinding in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-linkedin-elevate-test-user"></a>Test gebruiker voor LinkedIn-bevoegdheden maken

De coactieve toepassing voor LinkedIn ondersteunt just-in-time-gebruikers inrichting en nadat gebruikers met verificatie automatisch in de toepassing worden gemaakt. Op de pagina beheer instellingen van de LinkedIn-Portal uitbrei ding spie gelen de switch **Automatisch licenties toewijzen** aan actief, net zo lang het inrichten is. er wordt ook een licentie aan de gebruiker toegewezen. Meer informatie over het verbeteren van de gebruikers ondersteuning wordt door LinkedIn-bevoegdheden ondersteund. u kunt [hier](linkedinelevate-provisioning-tutorial.md) meer details vinden over het configureren van automatische gebruikers inrichting.

   ![Het maken van een Azure AD-testgebruiker](./media/linkedinelevate-tutorial/LinkedinUserprovswitch.png)

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel LinkedIn-verhoging in het toegangs venster klikt, moet u automatisch worden aangemeld bij de LinkedIn-verhoging waarvoor u SSO hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)