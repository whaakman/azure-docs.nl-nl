---
title: 'Zelfstudie: Azure Active Directory-integratie met Jamf Pro | Microsoft Docs'
description: Ontdek hoe u eenmalige aanmelding configureert tussen Azure Active Directory en Splunk Enterprise en Jamf Pro.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 35e86d08-c29e-49ca-8545-b0ff559c5faf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/19/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 18b575b74c80499f2ddd6648bf051b5245077d2f
ms.sourcegitcommit: 9f4eb5a3758f8a1a6a58c33c2806fa2986f702cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58906137"
---
# <a name="tutorial-azure-active-directory-integration-with-jamf-pro"></a>Zelfstudie: Azure Active Directory-integratie met Jamf Pro

In deze zelfstudie leert u hoe u Jamf Pro kunt integreren met Azure Active Directory (Azure AD).
Jamf Pro integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD wie toegang tot Jamf Pro heeft.
* U kunt inschakelen dat gebruikers automatisch met hun Azure AD-account worden aangemeld bij Jamf Pro (eenmalige aanmelding).
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Als u Azure AD-integratie met Jamf Pro wilt configureren, hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Een abonnement op Jamf Pro waarvoor eenmalige aanmelding is ingeschakeld

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Jamf Pro ondersteunt door **SP en IDP** geïnitieerde eenmalige aanmelding

## <a name="adding-jamf-pro-from-the-gallery"></a>Jamf Pro uit de galerie toevoegen

Voor het configureren van de integratie van Jamf Pro in Azure AD, moet u Jamf Pro uit de galerie aan uw lijst met beheerde SaaS-apps toevoegen.

**Als u wilt toevoegen Jamf Pro uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **Jamf Pro**, selecteer **Jamf Pro** in het deelvenster met resultaten klik vervolgens op de knop **Toevoegen** om de toepassing toe te voegen.

     ![Jamf Pro in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie configureert en test u Azure AD-eenmalige aanmelding met Jamf Pro op basis van een testgebruiker met de naam **Britta Simon**.
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Azure AD-gebruiker en de daaraan gerelateerde gebruiker in Jamf Pro tot stand is gebracht.

Als u Azure AD-eenmalige aanmelding met Jamf Pro wilt configureren en testen, moet u de volgende bouwstenen uitvoeren:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Jamf Pro-eenmalige aanmelding configureren](#configure-jamf-pro-single-sign-on)**: als u de instellingen voor eenmalige aanmelding aan de clientzijde wilt configureren.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Jamf Pro-testgebruiker maken](#create-jamf-pro-test-user)** : als u een equivalent van Britta Simon in Jamf Pro wilt hebben dat is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD-eenmalige aanmelding met Jamf Pro moet u de volgende stappen uitvoeren:

1. Selecteer in [Azure Portal](https://portal.azure.com/) op de integratiepagina van de **Jamf Pro**-toepassing de optie **Eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. Voer in de sectie **Standaard SAML-configuratie** de volgende stappen uit als u in de door **IDP** geïnitieerde modus wilt configureren:

    ![Informatie over eenmalige aanmelding bij het Jamf Pro-domein en Jamf Pro-URL's](common/idp-intiated.png)

    a. In het tekstvak **Id** typt u een URL met het volgende patroon: `https://<subdomain>.jamfcloud.com/saml/metadata`

    b. In het tekstvak **Antwoord-URL** typt u een URL met het volgende patroon: `https://<subdomain>.jamfcloud.com/saml/SSO`

5. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<subdomain>.jamfcloud.com`

    ![Informatie over eenmalige aanmelding bij het Jamf Pro-domein en Jamf Pro-URL's](common/metadata-upload-additional-signon.png)

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke id, de antwoord-URL en aanmeldings-URL. U vindt de werkelijke id-waarde van de sectie **eenmalige aanmelding** in de Jamf Pro-portal, op de manier die verderop in de zelfstudie wordt uitgelegd. U kunt de werkelijke **subdomein**-waarde uit de id-waarde extraheren en deze **subdomein**-informatie gebruiken in de aanmeldings-URL en antwoord-URL. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

6. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op de kopieerknop om de **URL voor federatieve metagegevens van de app** te kopiëren en slaat u deze op uw computer op.

    ![De link om het certificaat te downloaden](common/copy-metadataurl.png)

### <a name="configure-jamf-pro-single-sign-on"></a>Jamf Pro-eenmalige aanmelding configureren

1. Als u de configuratie met Jamf Pro wilt automatiseren, moet u **My Apps-browserextensie voor veilig aanmelden** installeren door op **De extensie installeren** te klikken.

    ![image](./media/jamfprosamlconnector-tutorial/install_extension.png)

2. Als u op **Jamf Pro instellen** klikt nadat u de extensie aan de browser hebt toegevoegd, wordt u doorgestuurd naar de Jamf Pro-toepassing. Geef hier de referenties voor de beheerder op om u aan te melden bij Jamf Pro. In de browserextensie wordt de toepassing automatisch voor u geconfigureerd en worden stappen 3-7 geautomatiseerd.

    ![image](./media/jamfprosamlconnector-tutorial/d1_saml.png)

3. Als u Jamf Pro handmatig wilt instellen, opent u een nieuw browservenster en meldt u zich als beheerder aan bij de Jamf Pro-bedrijfssite. Voer daarna de volgende stappen uit:

4. Klik op het pictogram **Instellen** in de rechterbovenhoek van de pagina.

    ![Configuratie van Jamf Pro](./media/jamfprosamlconnector-tutorial/configure1.png)

5. Klik op **Eenmalige aanmelding**.

    ![Configuratie van Jamf Pro](./media/jamfprosamlconnector-tutorial/configure2.png)

6. Voer op de pagina **Eenmalige aanmelding** de volgende stappen uit:

    ![Eenmalige aanmelding bij Jamf Pro](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_single.png)

    a. Selecteer **Jamf Pro-server** om toegang via eenmalige aanmelding in te schakelen.

    b. Door het selecteren van **Bypass toestaan voor alle gebruikers** worden gebruikers niet omgeleid naar de aanmeldingspagina van de id-provider voor verificatie, maar kunnen gebruikers zich rechtsreeks bij Jamf Pro aanmelden. Wanneer een gebruiker toegang probeert te krijgen tot Jamf Pro via de id-provider, vindt IdP-geïnitieerde SSO-verificatie en -autorisatie plaats.

    c. Selecteer de **NameID**-optie voor **gebruikerstoewijzing: SAML**. Deze instelling is standaard ingesteld op **NameID**, maar u kunt een aangepast attribuut definiëren.

    d. Selecteer **Email** voor **gebruikerstoewijzing: JAMF PRO**. Jamf Pro wijst SAML-kenmerken die door de IdP zijn verzonden op de volgende manieren toe: aan de gebruikers en aan groepen. Wanneer een gebruiker probeert toegang te krijgen tot Jamf Pro, krijgt Jamf Pro standaard informatie over de gebruiker van de id-provider en vergelijkt deze met de Jamf Pro-gebruikersaccounts. Als het binnenkomende gebruikersaccount niet in Jamf Pro bestaat, wordt de bijpassende groepsnaam gezocht.

    e. Plak de waarde `http://schemas.microsoft.com/ws/2008/06/identity/claims/groups` in het tekstvak **Naam groepskenmerk**.

7. Schuif op dezelfde pagina naar **ID-PROVIDER** onder de sectie **Eenmalige aanmelding** en voer de volgende stappen uit:

    ![Configuratie van Jamf Pro](./media/jamfprosamlconnector-tutorial/configure3.png)

    a. Selecteer **andere** als een optie in de **id-PROVIDER** vervolgkeuzelijst.

    b. Voer in het tekstvak **Andere provider** **Azure AD** in.

    c. Selecteer **metagegevens-URL** als een optie in de **IDENTITEITSBRON PROVIDER metagegevens** vervolgkeuzelijst en plak in het volgende tekstvak de **App-Url voor federatieve metagegevens** waarde die u hebt gekopieerd vanuit Azure portal.

    d. Kopieer de waarde **Entiteits-id** en plak deze in het tekstvak **Id (entiteits-id)** in de sectie **Jamf Pro-domein en URL's** in Azure Portal.

    > [!NOTE]
    > Hier is de wazige waarde het subdomein. Gebruik deze waarde om de aanmeldings-URL en antwoord-URL in de sectie **Jamf Pro-domein en URL's** op Azure Portal in te vullen.

    e. Klik op **Opslaan**.

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

In dit gedeelte gaat u Britta Simon toestemming geven voor gebruik van eenmalige aanmelding met Azure door haar toegang te geven tot Jamf Pro.

1. Selecteer in Azure Portal **Bedrijfstoepassingen**, selecteer **Alle toepassingen** en selecteer vervolgens **Jamf Pro**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen **Jamf Pro**.

    ![De Jamf Pro-koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-jamf-pro-test-user"></a>Testgebruiker voor Jamf Pro maken

Als u wilt dat Azure AD-gebruikers zich kunnen aanmelden bij Jamf Pro, moeten ze worden ingericht in Jamf Pro. In het geval van Jamf Pro is inrichten een handmatige taak.

**Voer de volgende stappen uit als u een gebruikersaccount wilt inrichten:**

1. Meld u aan als beheerder bij uw Jamf Pro-bedrijfspagina.

2. Klik op het pictogram **Instellen** in de rechterbovenhoek van de pagina.

    ![Werknemer toevoegen](./media/jamfprosamlconnector-tutorial/configure1.png)

3. Klik op **Jamf Pro-gebruikersaccounts en groepen**.

    ![Werknemer toevoegen](./media/jamfprosamlconnector-tutorial/user1.png)

4. Klik op **Nieuw**.

    ![Werknemer toevoegen](./media/jamfprosamlconnector-tutorial/user2.png)

5. Selecteer **Standaard account maken**.

    ![Werknemer toevoegen](./media/jamfprosamlconnector-tutorial/user3.png)

6. Klik in het dialoogvenster **Nieuw account** en voer de volgende stappen uit:

    ![Werknemer toevoegen](./media/jamfprosamlconnector-tutorial/user4.png)

    a. Typ in het tekstvak **Gebruikersnaam** de volledige naam van BrittaSimon.

    b. Selecteer de voor uw organisatie toepasselijke opties voor **toegangsniveau**, **bevoegdheden**  en **toegangsstatus**.

    c. Typ in het tekstvak **Volledige naam** de volledige naam van Britta Simon.

    d. Typ in het tekstvak **E-mailadres** het e-mailadres van het account van Britta Simon.

    e. Typ in het tekstvak **Wachtwoord** het wachtwoord van de gebruiker.

    f. Typ in het tekstvak **Wachtwoord bevestigen** het wachtwoord van de gebruiker.

    g. Klik op **Opslaan**.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Jamf Pan in het toegangsvenster klikt, wordt u automatisch aangemeld bij de instantie van Jamf Pro waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
