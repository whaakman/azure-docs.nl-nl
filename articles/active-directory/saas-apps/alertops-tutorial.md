---
title: 'Zelfstudie: Azure Active Directory-integratie met AlertOps | Microsoft Docs'
description: Leer hoe u eenmalige aanmelding configureert tussen Azure Active Directory en AlertOps.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 3db13ed4-35c2-4b1e-bed8-9b5977061f93
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/08/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7914bc3775631f3cc5d6ae68fed10c6d5fecb853
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57838048"
---
# <a name="tutorial-azure-active-directory-integration-with-alertops"></a>Zelfstudie: Azure Active Directory-integratie met AlertOps

In deze zelfstudie leert u hoe u AlertOps kunt integreren met Azure Active Directory (Azure AD).
De integratie van AlertOps met Azure AD biedt de volgende voordelen:

* U kunt in Azure AD bepalen wie er toegang heeft tot AlertOps.
* U kunt instellen dat gebruikers automatisch met hun Azure AD-account worden aangemeld bij AlertOps (eenmalige aanmelding).
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met AlertOps hebt u het volgende nodig:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Een abonnement op AlertOps waarvoor eenmalige aanmelding is ingeschakeld

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* AlertOps ondersteunt door **SP en IDP** geïnitieerde eenmalige aanmelding

## <a name="adding-alertops-from-the-gallery"></a>AlertOps toevoegen vanuit de galerie

Om de integratie van AlertOps in Azure AD te configureren, moet u AlertOps vanuit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Ga als volgt te werk om AlertOps vanuit de galerie toe te voegen:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ **AlertOps** in het zoekvak, selecteer **AlertOps** in het deelvenster met resultaten en klik vervolgens op de knop **Toevoegen** om de toepassing toe te voegen.

    ![AlertOps toevoegen vanuit de galerie](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In dit gedeelte configureert en test u eenmalige aanmelding van Azure AD met AlertOps op basis van een testgebruiker met de naam **Britta Simon**.
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Azure AD-gebruiker en de daaraan gerelateerde gebruiker in AlertOps tot stand is gebracht.

Als u eenmalige aanmelding van Azure AD met AlertOps wilt configureren en testen, moet u de volgende procedures uitvoeren:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Eenmalige aanmelding voor AlertOps configureren](#configure-alertops-single-sign-on)**: de instellingen voor eenmalige aanmelding aan de clientzijde configureren.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Testgebruiker voor AlertOps maken](#create-alertops-test-user)**: als u een tegenhanger van Britta Simon in AlertOps wilt hebben die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voer de volgende stappen uit om eenmalige aanmelding van Azure AD te configureren voor AlertOps:

1. Ga in de [Azure-portal](https://portal.azure.com/) naar de pagina van de integratie van **AlertOps** en selecteer **Eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit als u de toepassing in de door **IDP** geïnitieerde modus wilt configureren:

    ![Gegevens van domein en URL's voor eenmalige aanmelding bij AlertOps](common/idp-intiated.png)

    a. In het tekstvak **Id** typt u een URL met het volgende patroon: `https://<SUBDOMAIN>.alertops.com`

    b. In het tekstvak **Antwoord-URL** typt u een URL met de volgende notatie: `https://<SUBDOMAIN>.alertops.com/login.aspx`

5. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    ![Gegevens van domein en URL's voor eenmalige aanmelding bij AlertOps](common/metadata-upload-additional-signon.png)

    In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<SUBDOMAIN>.alertops.com/login.aspx`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke-id, de antwoord-URL en de aanmeldings-URL. Neem contact op met het [klantondersteuningsteam van AlertOps](mailto:support@alertops.com) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

6. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

7. Kopieer in het gedeelte **AlertOps instellen** de juiste URL('s) op basis van uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-alertops-single-sign-on"></a>Eenmalige aanmelding voor AlertOps configureren

1. Meld u in een ander browservenster als beheerder aan bij de bedrijfssite van AlertOps.

2. Klik in het linkernavigatievenster op **Account Settings**.

    ![AlertOps-configuratie](./media/alertops-tutorial/configure1.png)

3. Selecteer **SSO** op de pagina **Subscription Settings** en voer de volgende stappen uit:

    ![AlertOps-configuratie](./media/alertops-tutorial/configure2.png)

    a. Schakel het selectievakje **Use Single Sign-On(SSO)** in.

    b. Selecteer **Azure Active Directory** in de vervolgkeuzelijst **SSO Provider**.

    c. Voer in het tekstvak **Issuer URL** de id-waarde in die u hebt gebruikt in het gedeelte **Standaard SAML-configuratie** in de Azure-portal.

    d. Plak in het tekstvak **SAML endpoint URL** de waarde van **Aanmeldings-URL** die u uit de Azure-portal hebt gekopieerd.

    e. Plak in het tekstvak **SLO endpoint URL** de waarde van **Aanmeldings-URL** die u uit de Azure-portal hebt gekopieerd.

    f. Selecteer **SHA256** in de vervolgkeuzelijst **SAML Signature Algorithm**.

    g. Open het gedownloade bestand Certificate(Base64) in Kladblok. Kopieer de inhoud van het bestand naar het klembord en plak deze vervolgens in het tekstvak X.509 Certificate.

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

In dit gedeelte gaat u Britta Simon toestemming geven voor gebruik van eenmalige aanmelding met Azure door haar toegang te geven tot AlertOps.

1. Selecteer **Bedrijfstoepassingen** in de Azure-portal, selecteer **Alle toepassingen** en selecteer vervolgens **AlertOps**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer **AlertOps** in de lijst met toepassingen.

    ![De koppeling naar AlertOps in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-alertops-test-user"></a>Een testgebruiker voor AlertOps maken

1. Meld u in een ander browservenster als beheerder aan bij de bedrijfssite van AlertOps.

2. Klik op **Users** in het linkernavigatievenster.

    ![AlertOps-configuratie](./media/alertops-tutorial/user1.png)

3. Selecteer **Add User**.

    ![AlertOps-configuratie](./media/alertops-tutorial/user2.png)

4. Voer in het dialoogvenster **Add User** de volgende stappen uit:

    ![AlertOps-configuratie](./media/alertops-tutorial/user3.png)

    a. Voer in het tekstvak **Login User Name** de gebruikersnaam van de gebruiker in, bijvoorbeeld **Brittasimon**.

    b. In de **officiële e** tekstvak, voer het e-mailadres van de gebruiker, zoals **Brittasimon\@contoso.com**.

    c. Voer in het tekstvak **First Name** de voornaam van de gebruiker in, zoals **Britta**.

    d. Voer in het tekstvak **Last Name** de achternaam van de gebruiker in, zoals **Simon**.

    e. Selecteer in de vervolgkeuzelijst **Type** een waarde die geschikt is voor uw organisatie.

    f. Selecteer in de vervolgkeuzelijst **Role** een waarde voor de rol van de gebruiker in uw organisatie.

    g. Selecteer **Toevoegen**.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u in het toegangsvenster op de tegel van AlertOps klikt, wordt u automatisch aangemeld bij het exemplaar van AlertOps waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
