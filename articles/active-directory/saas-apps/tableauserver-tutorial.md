---
title: 'Zelfstudie: Integratie met tableau-server Azure Active Directory | Microsoft Docs'
description: Meer informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en tableau server.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: c1917375-08aa-445c-a444-e22e23fa19e0
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/22/2019
ms.author: jeedes
ms.openlocfilehash: f9ef179c1a93d8b2f97c47eb4c68d0312d55d3d1
ms.sourcegitcommit: 3073581d81253558f89ef560ffdf71db7e0b592b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68825977"
---
# <a name="tutorial-azure-active-directory-integration-with-tableau-server"></a>Zelfstudie: Integratie met tableau-server Azure Active Directory

In deze zelf studie leert u hoe u tableau server kunt integreren met Azure Active Directory (Azure AD).
De integratie van tableau server met Azure AD biedt de volgende voor delen:

* U kunt beheren in azure AD die toegang heeft tot de tableau-server.
* U kunt ervoor zorgen dat uw gebruikers automatisch worden aangemeld bij de tableau-server (eenmalige aanmelding) met hun Azure AD-accounts.
* U kunt uw accounts in één centrale locatie - Azure portal beheren.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Als u Azure AD-integratie met tableau-server wilt configureren, hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u een [gratis account](https://azure.microsoft.com/free/) aanvragen
* Abonnement voor eenmalige aanmelding van tableau-server

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Tableau-server ondersteunt door **SP** GEÏNITIEERDe SSO

## <a name="adding-tableau-server-from-the-gallery"></a>De tableau-server toevoegen vanuit de galerie

Als u de integratie van tableau-server in azure AD wilt configureren, moet u tableau-server uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Voer de volgende stappen uit om tableau-server toe te voegen vanuit de galerie:**

1. In de **[Azure-portal](https://portal.azure.com)** , klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing](common/add-new-app.png)

4. Typ **tableau server**in het zoekvak, selecteer **tableau server** van result panel en klik vervolgens op knop **toevoegen** om de toepassing toe te voegen.

    ![Tableau-server in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie kunt u eenmalige aanmelding voor Azure AD configureren en testen met tableau-server op basis van een test gebruiker met de naam **Julia Simon**.
Als u eenmalige aanmelding wilt gebruiken, moet er een koppelings relatie tussen een Azure AD-gebruiker en de bijbehorende gebruiker in tableau-server tot stand worden gebracht.

Als u eenmalige aanmelding voor Azure AD wilt configureren en testen met tableau-server, moet u de volgende bouw stenen volt ooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Eenmalige aanmelding voor tableau server configureren](#configure-tableau-server-single-sign-on)** : Hiermee configureert u de instellingen voor eenmalige aanmelding aan de kant van de toepassing.
3. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Maak een tableau-server test gebruiker](#create-tableau-server-test-user)** -om een equivalent van Julia Simon te hebben in tableau-server dat is gekoppeld aan de Azure AD-representatie van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voer de volgende stappen uit om eenmalige aanmelding voor Azure AD met tableau-server te configureren:

1. Selecteer in de [Azure Portal](https://portal.azure.com/)op de pagina **tableau server** Application Integration de optie **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Informatie over eenmalige aanmelding voor tableau-Server domein en Url's](common/sp-identifier-reply.png)

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met het volgende patroon: `https://azure.<domain name>.link`

    b. In het tekstvak **Id** typt u een URL met het volgende patroon: `https://azure.<domain name>.link`

    c. In het tekstvak **Antwoord-URL** typt u een URL met de volgende notatie: `https://azure.<domain name>.link/wg/saml/SSO/index.html`

    > [!NOTE]
    > De voor gaande waarden zijn geen echte waarden. Werk de waarden bij met de werkelijke URL en id van de tableau-server configuratie pagina die verderop in de zelf studie wordt beschreven.

5. Tableau-server toepassing verwacht een aangepaste claim **gebruikersnaam** die hieronder moet worden gedefinieerd. Dit wordt gebruikt als gebruikers-id in plaats van een unieke claim voor de gebruikers-id. U kunt de waarden van deze kenmerken vanuit de sectie **Gebruikerskenmerken en claims** op de integratiepagina van de toepassing-beheren. Klik op de knop **bewerken** om **gebruikers kenmerken** te openen & dialoog venster claims.

    ![image](common/edit-attribute.png)

6. Configureer in de sectie **gebruikers claims** van het dialoog venster **gebruikers kenmerken & claims** het kenmerk SAML-token, zoals wordt weer gegeven in de bovenstaande installatie kopie en voer de volgende stappen uit:

    | Name | Bronkenmerk | Naamruimte |
    | ---------------| --------------- | ----------- |
    | userName | user.userprincipalname | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims` |
    | | |

    a. Klik op **Nieuwe claim toevoegen** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Voer de waarde van de **naam ruimte** in.

    d. Selecteer Bron bij **Kenmerk**.

    e. Typ de kenmerkwaarde voor die rij in de lijst met **bronkenmerken**.

    f. Klik op **OK**.

    g. Klik op **Opslaan**.

7. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De downloadkoppeling certificaat](common/metadataxml.png)

8. Kopieer op de sectie **tableau-server instellen** de gewenste URL ('s) volgens uw vereiste.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. URL voor afmelden

### <a name="configure-tableau-server-single-sign-on"></a>Eenmalige aanmelding voor de tableau-server configureren

1. Als u SSO wilt ophalen die voor uw toepassing is geconfigureerd, moet u zich bij uw tableau-server Tenant aanmelden als beheerder.

2. Op het tabblad **configuratie** selecteert u **gebruikers identiteit & toegang**en selecteert u vervolgens het tabblad **verificatie** methode.

    ![Eenmalige aanmelding configureren](./media/tableauserver-tutorial/tutorial-tableauserver-auth.png)

3. Voer de volgende stappen uit op de pagina **configuratie** :

    ![Eenmalige aanmelding configureren](./media/tableauserver-tutorial/tutorial-tableauserver-config.png)

    a. Selecteer voor **verificatie methode**de optie SAML.

    b. Schakel het selectie vakje in **voor het inschakelen van SAML-verificatie voor de-server**.

    c. Tableau server-retour-URL: de URL van tableau Server-gebruikers die toegang krijgen <http://tableau_server>, zoals. Gebruiken `http://localhost` wordt niet aanbevolen. Het gebruik van een URL met een slash (bijvoorbeeld `http://tableau_server/`) wordt niet ondersteund. Kopieer de URL van de **tableau-server** en plak deze in om het **URL** -tekstvak op te geven in de sectie **basis configuratie van SAML** in de Azure Portal

    d. SAML-entiteit-ID: de entiteit-ID is een unieke aanduiding voor uw tableau-server installatie naar de IdP. U kunt de URL van uw tableau-server hier hier opgeven, maar dit is niet uw tableau-server-URL. Kopieer de ID van de **SAML-entiteit** en plak deze in het tekstvak **id** in de sectie **basis configuratie van SAML** in de Azure Portal

    e. Klik op het **XML-bestand met meta gegevens downloaden** en open het in de toepassing tekst editor. Zoek de URL van de service voor de bewerings verbruiker met http post en index 0 en kopieer de URL. Plak het in het tekstvak **antwoord URL** in het gedeelte **basis configuratie van SAML** in de Azure Portal

    f. Zoek het bestand met federatieve meta gegevens dat is gedownload van Azure Portal en upload het naar het bestand met de **SAML IDP-meta gegevens**.

    g. Voer de namen in voor de kenmerken die de IdP gebruikt om de gebruikers namen, weergave namen en e-mail adressen op te slaan.

    h. Klik op **Opslaan**

    > [!NOTE]
    > De klant moet een certificaat uploaden in de SAML SSO-configuratie van de tableau-server en wordt genegeerd in de SSO-stroom. Als u hulp nodig hebt bij het configureren van SAML op tableau server, raadpleegt u dit artikel [SAML configureren](https://help.tableau.com/current/server/en-gb/saml_config_steps_tsm_ui.htm).

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. Typ in het veld **gebruikers naam**`brittasimon@yourcompanydomain.extension`  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Julia Simon in om de eenmalige aanmelding van Azure te gebruiken door toegang te verlenen aan de tableau-server.

1. Selecteer in het Azure Portal **bedrijfs toepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **tableau server**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst toepassingen de optie **tableau server**.

    ![De koppeling van de tableau-server in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-tableau-server-test-user"></a>Test gebruiker voor tableau-server maken

Het doel van deze sectie is het maken van een gebruiker met de naam Julia Simon in tableau-server. U moet alle gebruikers op de tableau-server inrichten.

De gebruikers naam van de gebruiker moet overeenkomen met de waarde die u hebt geconfigureerd in het aangepaste Azure AD-kenmerk van de **gebruikers naam**. Met de juiste toewijzing moet de integratie werken met het configureren van eenmalige aanmelding voor Azure AD.

> [!NOTE]
> Als u hand matig een gebruiker moet maken, moet u contact opnemen met de beheerder van de tableau-server in uw organisatie.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel tableau server in het toegangs venster klikt, moet u automatisch worden aangemeld bij de tableau-server waarvoor u SSO hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

