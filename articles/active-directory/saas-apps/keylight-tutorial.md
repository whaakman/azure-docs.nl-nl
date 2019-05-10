---
title: 'Zelfstudie: Azure Active Directory-integratie met LockPath Keylight | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en LockPath Keylight.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 234a32f1-9f56-4650-9e31-7b38ad734b1a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/14/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7a03bb2626525bf022b109105a7c6bc0dee23aea
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65407030"
---
# <a name="tutorial-azure-active-directory-integration-with-lockpath-keylight"></a>Zelfstudie: Azure Active Directory-integratie met LockPath Keylight

In deze zelfstudie leert u hoe u LockPath Keylight integreren met Azure Active Directory (Azure AD).
LockPath Keylight integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot LockPath Keylight heeft.
* U kunt uw gebruikers worden automatisch aangemeld LockPath Keylight (Single Sign-On) inschakelen met hun Azure AD-accounts.
* U kunt uw accounts in één centrale locatie - Azure portal beheren.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met LockPath Keylight, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* Eenmalige aanmelding LockPath Keylight ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor LockPath Keylight **SP** gestart door SSO
* Biedt ondersteuning voor LockPath Keylight **Just In Time** inrichten van gebruikers

## <a name="adding-lockpath-keylight-from-the-gallery"></a>LockPath Keylight uit de galerie toe te voegen

Voor het configureren van de integratie van LockPath Keylight in Azure AD, moet u LockPath Keylight uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen LockPath Keylight uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **LockPath Keylight**, selecteer **LockPath Keylight** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![LockPath Keylight in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met LockPath Keylight op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in LockPath Keylight tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met LockPath Keylight, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Configureer LockPath Keylight Single Sign-On](#configure-lockpath-keylight-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Maken van de testgebruiker LockPath Keylight](#create-lockpath-keylight-test-user)**  : als u wilt een equivalent van Britta Simon in LockPath Keylight die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met LockPath Keylight, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **LockPath Keylight** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![LockPath Keylight domein en URL's, eenmalige aanmelding informatie](common/sp-identifier-reply.png)

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<company name>.keylightgrc.com/`

    b. In het tekstvak **Id (Entiteits-id)** typt u een URL met de volgende notatie: `https://<company name>.keylightgrc.com`

    c. In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://<company name>.keylightgrc.com/Login.aspx`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke aanmeldings-URL, id en antwoord-URL. Neem contact op met [LockPath Keylight Client ondersteuningsteam](https://www.lockpath.com/contact/) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Raw)** te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De downloadkoppeling certificaat](common/certificateraw.png)

6. Op de **LockPath Keylight instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. URL voor afmelden

### <a name="configure-lockpath-keylight-single-sign-on"></a>Configureer LockPath Keylight Single Sign-On

1. Schakel eenmalige aanmelding in LockPath Keylight door de volgende stappen uitvoeren:

    a. Aanmelding bij uw account LockPath Keylight als administrator.

    b. Klik in het menu aan de bovenkant op **persoon**, en selecteer **Keylight Setup**.

    ![Eenmalige aanmelding configureren](./media/keylight-tutorial/401.png)

    c. Klik in de structuurweergave aan de linkerkant op **SAML**.

    ![Eenmalige aanmelding configureren](./media/keylight-tutorial/402.png)

    d. Op de **SAML-instellingen** dialoogvenster, klikt u op **bewerken**.

    ![Eenmalige aanmelding configureren](./media/keylight-tutorial/404.png)

1. Op de **SAML-instellingen bewerken** dialoogvenster pagina, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/keylight-tutorial/405.png)

    a. Stel **SAML-verificatie** naar **Active**.

    b. In de **aanmeldings-URL van id-Provider** tekstvak, plak de **aanmeldings-URL** waarde die u hebt gekopieerd vanuit Azure portal.

    c. In de **afmeldings-URL van id-Provider** tekstvak, plak de **afmeldings-URL van** waarde die u hebt gekopieerd vanuit Azure portal.

    d. Klik op **bestand kiezen** uw gedownloade LockPath Keylight certificaat selecteren en klik vervolgens op **Open** om het certificaat te uploaden.

    e. Instellen **SAML gebruikers-Id locatie** naar **NameIdentifier-element van het onderwerp overzicht**.

    f. Geef de **Keylight serviceprovider** met behulp van het volgende patroon: `https://<CompanyName>.keylightgrc.com`.

    g. Stel **automatisch inrichten gebruikers** naar **Active**.

    h. Stel **automatisch inrichten accounttype** naar **volledige gebruiker**.

    i. Stel **beveiligingsrol automatisch inrichten**, selecteer **standaardgebruiker met SAML**.

    j. Stel **automatisch inrichten security config**, selecteert **Standard Gebruikersconfiguratie**.

    k. In de **e kenmerk** tekstvak, type `https://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    l. In de **voornaam kenmerk** tekstvak, type `https://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.

    m. In de **laatste naamkenmerk** tekstvak, type `https://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.

    n. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In de **gebruikersnaam** veldtype `brittasimon@yourcompanydomain.extension`. Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan LockPath Keylight.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **LockPath Keylight**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **LockPath Keylight**.

    ![De koppeling LockPath Keylight in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-lockpath-keylight-test-user"></a>LockPath Keylight testgebruiker maken

In deze sectie wordt een gebruiker met de naam Britta Simon gemaakt in LockPath Keylight. LockPath Keylight biedt ondersteuning voor just-in-time-gebruikersinrichting, dat standaard is ingeschakeld. Er is geen actie-item voor u in deze sectie. Als een gebruiker nog niet in LockPath Keylight bestaat, wordt een nieuw gemaakt nadat verificatie. Als u een gebruiker handmatig hebt gemaakt wilt, moet u contact op met de [LockPath Keylight Client ondersteuningsteam](https://www.lockpath.com/contact/).

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel LockPath Keylight in het toegangsvenster, moet u worden automatisch aangemeld bij de LockPath Keylight waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)