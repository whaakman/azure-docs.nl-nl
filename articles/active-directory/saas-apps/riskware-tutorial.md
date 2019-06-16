---
title: 'Zelfstudie: Azure Active Directory-integratie met Riskware | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Riskware.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 81866167-b163-4695-8978-fd29a25dac7a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/25/2019
ms.author: jeedes
ms.openlocfilehash: b2bfbed33433521fd086d474ea4b754f5435f5e6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67092912"
---
# <a name="tutorial-azure-active-directory-integration-with-riskware"></a>Zelfstudie: Azure Active Directory-integratie met Riskware

In deze zelfstudie leert u hoe u Riskware integreren met Azure Active Directory (Azure AD).
Riskware integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot Riskware heeft.
* U kunt uw gebruikers worden automatisch aangemeld Riskware (Single Sign-On) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Riskware, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* Eenmalige aanmelding Riskware ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor Riskware **SP** gestart door SSO

## <a name="adding-riskware-from-the-gallery"></a>Riskware uit de galerie toe te voegen

Voor het configureren van de integratie van Riskware in Azure AD, moet u Riskware uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Riskware uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)** , klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **Riskware**, selecteer **Riskware** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Riskware in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Riskware op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Riskware tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Riskware, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)** : als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureren van eenmalige aanmelding Riskware](#configure-riskware-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)** : als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)** : als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maken van de testgebruiker Riskware](#create-riskware-test-user)**  : als u wilt een equivalent van Britta Simon in Riskware die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)** : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met Riskware, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **Riskware** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Riskware domein en URL's, eenmalige aanmelding informatie](common/sp-identifier.png)

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met het volgende patroon:
    
    | Omgeving| URL-patroon|
    |--|--|
    | UAT|  `https://riskcloud.net/uat?ccode=<COMPANYCODE>` |
    | PROD| `https://riskcloud.net/prod?ccode=<COMPANYCODE>` |
    | DEMO| `https://riskcloud.net/demo?ccode=<COMPANYCODE>` |
    |||

    b. In de **id (entiteits-ID)** tekstvak typt u de URL:
    
    | Omgeving| URL-patroon|
    |--|--|
    | UAT| `https://riskcloud.net/uat` |
    | PROD| `https://riskcloud.net/prod` |
    | DEMO| `https://riskcloud.net/demo` |
    |||

    > [!NOTE]
    > De waarde van de aanmeldings-URL is niet echt. Werk de waarde bij met de werkelijke aanmeldings-URL. Neem contact op met [Riskware Client ondersteuningsteam](mailto:support@pansoftware.com.au) om de waarde. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

6. Op de **Riskware instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-riskware-single-sign-on"></a>Riskware voor eenmalige aanmelding configureren

1. In een ander browservenster aanmelden bij uw bedrijf Riskware site als beheerder.

1. Klik in de rechterbovenhoek op **onderhoud** om de onderhoudspagina te openen.

    ![Riskware configuraties beheren](./media/riskware-tutorial/tutorial_riskware_maintain.png)

1. Klik in de onderhoudspagina op **verificatie**.

    ![Configuratie van Riskware authen](./media/riskware-tutorial/tutorial_riskware_authen.png)

1. In **verificatieconfiguratie** pagina, voert u de volgende stappen uit:

    ![Configuratie van Riskware authenconfig](./media/riskware-tutorial/tutorial_riskware_config.png)

    a. Selecteer **Type** als **SAML** voor verificatie.

    b. In de **Code** tekstvak typt u uw code zoals AZURE_UAT.

    c. In de **beschrijving** tekstvak typt u de beschrijving in, zoals AZURE-configuratie voor eenmalige aanmelding.

    d. In **één teken op pagina** tekstvak, plak de **aanmeldings-URL** waarde die u hebt gekopieerd vanuit Azure portal.

    e. In **afmelden pagina** tekstvak, plak de **afmeldings-URL van** waarde die u hebt gekopieerd vanuit Azure portal.

    f. In de **Post formulierveld** tekstvak typt u de veldnaam aanwezig zijn in de Post-antwoord met SAML, zoals SAMLResponse

    g. In de **XML-tagnaam identiteit** tekstvak kenmerk van het type, waarin de unieke id in het SAML-antwoord, zoals NameID.

    h. Open het gedownloade **Metadata Xml** kopieert het certificaat uit het bestand met metagegevens van Azure-portal in Kladblok en plak deze in de **certificaat** tekstvak

    i. In **consument URL** tekstvak, plak de waarde van **antwoord-URL**, die u ontvangt van het ondersteuningsteam.

    j. In **verlener** tekstvak, plak de waarde van **id**, die u ontvangt van het ondersteuningsteam.

    > [!Note]
    > Neem contact op met [Riskware Client ondersteuningsteam](mailto:support@pansoftware.com.au) om deze waarden te verkrijgen

    k. Selecteer **gebruik POST** selectievakje.

    l. Selecteer **gebruik SAML-aanvraag** selectievakje.

    m. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In de **gebruikersnaam** veldtype `brittasimon@yourcompanydomain.extension`  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Riskware.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **Riskware**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **Riskware**.

    ![De koppeling Riskware in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-riskware-test-user"></a>Riskware testgebruiker maken

Als u wilt dat Azure AD-gebruikers kunnen zich aanmelden bij Riskware, moeten ze worden ingericht voor Riskware. In Riskware is inrichten een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Aanmelden bij Riskware als een beveiligingsbeheerder.

1. Klik in de rechterbovenhoek op **onderhoud** om de onderhoudspagina te openen. 

    ![Configuratie van Riskware onderhoudt](./media/riskware-tutorial/tutorial_riskware_maintain.png)

1. Klik in de onderhoudspagina op **mensen**.

    ![Configuratie van Riskware personen](./media/riskware-tutorial/tutorial_riskware_people.png)

1. Selecteer **Details** tabblad en voer de volgende stappen uit:

    ![Informatie over de configuratie van Riskware](./media/riskware-tutorial/tutorial_riskware_details.png)

    a. Selecteer **Type persoon** zoals werknemer.

    b. In **voornaam** tekstvak, geef de voornaam van de gebruiker, zoals **Julia**.

    c. In **achternaam** tekstvak, geef de achternaam van de gebruiker, zoals **Simon**.

1. Op de **Security** tabblad, voert u de volgende stappen uit:

    ![Configuratie van Riskware beveiliging](./media/riskware-tutorial/tutorial_riskware_security.png)

    a. Onder **verificatie** sectie, selecteer de **verificatie** modus, die u hebt instellen zoals AZURE-configuratie voor eenmalige aanmelding.

    b. Onder **inloggegevens** sectie in de **gebruikers-ID** tekstvak, voer het e-mailadres van gebruiker, zoals `brittasimon@contoso.com`.

    c. In de **wachtwoord** tekstvak, voer het wachtwoord van de gebruiker.

1. Op de **organisatie** tabblad, voert u de volgende stappen uit:

    ![Configuratie van Riskware org](./media/riskware-tutorial/tutorial_riskware_org.png)

    a. Selecteer de optie als **Level1** organisatie.

    b. Onder **persoon primaire werkplek** sectie in de **locatie** tekstvak typt u uw locatie.

    c. Onder **werknemer** sectie, selecteer **Werknemerstatus** zoals Casual.

    d. Klik op **Opslaan**.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Riskware in het toegangsvenster, moet u worden automatisch aangemeld bij de Riskware waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
