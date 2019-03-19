---
title: 'Zelfstudie: Integratie van Microsoft Azure Active Directory met Appraisd | Microsoft Docs'
description: Lees hoe u eenmalige aanmelding configureert tussen Microsoft Azure Active Directory en Appraisd.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: db063306-4d0d-43ca-aae0-09f0426e7429
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/17/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: fd7d1e3511bf74650ddc9dca1cf77c2b7c297c96
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57861499"
---
# <a name="tutorial-azure-active-directory-integration-with-appraisd"></a>Zelfstudie: Microsoft Azure Active Directory-integratie met Appraisd

In deze zelfstudie leert u hoe u Appraisd kunt integreren met Microsoft Azure Active Directory (Azure AD).
Het integreren van Appraisd met Microsoft Azure Active Directory biedt u de volgende voordelen:

* U kunt in Microsoft Azure Active Directory beheren wie toegang heeft tot Appraisd.
* U kunt uw gebruikers zich automatisch laten aanmelden bij Appraisd (eenmalige aanmelding) met hun Microsoft Azure Active Directory-account.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Om Microsoft Azure Active Directory-integratie met Appraisd te configureren, hebt u het volgende nodig:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Een abonnement op Appraisd waarop eenmalige aanmelding is ingeschakeld

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Appraisd biedt ondersteuning voor door **SP en IDP** geïnitieerde eenmalige aanmelding

## <a name="adding-appraisd-from-the-gallery"></a>Appraisd toevoegen vanuit de galerie

Om de integratie van Appraisd in Microsoft Azure Active Directory te configureren, moet u Appraisd vanuit de galerie aan uw lijst met beheerde SaaS-apps toevoegen.

**Als u Appraisd vanuit de galerie wilt toevoegen, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **Appraisd**, selecteer **Appraisd** in het resultaatvenster en klik vervolgens op de knop **Toevoegen** om de toepassing toe te voegen.

     ![Appraisd in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In dit gedeelte configureert en test u eenmalige aanmelding van Microsoft Azure Active Directory met Appraisd op basis van een testgebruiker met de naam **Britta Simon**.
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Microsoft Azure Active Directory-gebruiker en de daaraan gerelateerde gebruiker in Appraisd tot stand is gebracht.

Om eenmalige aanmelding van Microsoft Azure Active Directory met Appraisd te configureren en te testen, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Eenmalige aanmelding bij Appraisd configureren](#configure-appraisd-single-sign-on)**: als u de instellingen voor eenmalige aanmelding aan de clientzijde wilt configureren.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Testgebruiker voor Appraisd maken](#create-appraisd-test-user)**: als u een tegenhanger van Britta Simon in Appraisd wilt hebben die is gekoppeld aan de Microsoft Azure Active Directory-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Om eenmalige aanmelding van Microsoft Azure Active Directory met Appraisd te configureren, moet u de volgende stappen uitvoeren:

1. In de [Microsoft Azure-portal](https://portal.azure.com/) selecteert u **Eenmalige aanmelding** op de integratiepagina van de toepassing **Appraisd**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit als u de toepassing in de door **IDP** geïnitieerde modus wilt configureren:

    ![Informatie over eenmalige aanmelding van domeinen en URL’s van Appraisd](common/both-preintegrated-advanced-urls.png)

    a. Klik op **Extra URL's instellen**.

    b. In het tekstvak **Relaystatus** typt u een URL: `<TENANTCODE>`

    c. Als u de toepassing in de door **SP** geïnitieerde modus wilt configureren, typt u in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://app.appraisd.com/saml/<TENANTCODE>`

    > [!NOTE]
    > U krijgt de werkelijke waarde van de aanmeldings-URL en de relaystatus op de pagina voor de configuratie van eenmalige aanmelding met Appraisd, wat later in de zelfstudie wordt uitgelegd.

5. De Appraisd-toepassing verwacht de SAML-asserties in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit de sectie **Gebruikerskenmerken** op de integratiepagina van de toepassing-beheren. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op de knop **Bewerken** om het dialoogvenster **Gebruikerskenmerken** te openen.

    ![image](common/edit-attribute.png)

6. Bewerk in het gedeelte **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** de claims met het **pictogram Bewerken** of voeg de claims toe door met **Nieuwe claim toevoegen** het kenmerk van het SAML-token te configureren, zoals wordt weergegeven in de bovenstaande afbeelding. Hierna voert u de volgende stappen uit:

    | Name |  Bronkenmerk|
    | ---------------| --------------- |
    | nameidentifier | user.mail |
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

7. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

8. In het gedeelte **Appraisd instellen** kopieert u de juiste URL('s) op basis van uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-appraisd-single-sign-on"></a>Appraisd configureren voor eenmalige aanmelding

1. Meld u in een ander browservenster bij Appraisd als een beveiligingsbeheerder aan.

2. Klik rechts boven in de pagina op het pictogram **Settings** en navigeer vervolgens naar  **Configuration**.

    ![image](./media/appraisd-tutorial/tutorial_appraisd_sett.png)

3. Klik aan de linkerkant van het menu op **SAML single sign-on**.

    ![image](./media/appraisd-tutorial/tutorial_appraisd_single.png)

4. Voer op de **configuratiepagina voor eenmalige aanmelding op basis van SAML 2.0** de volgende stappen uit:

    ![image](./media/appraisd-tutorial/tutorial_appraisd_saml.png)

    a. Kopieer de waarde van de **standaardrelaystatus** en plak deze in het tekstvak  **Relaystatus** in  **Standaard SAML-configuratie** in de Microsoft Azure-portal.

    b. Kopieer de waarde van de **door de service geïnitieerde aanmeldings-URL** en plak deze in het tekstvak  **Aanmeldings-URL** in  **Standaard SAML-configuratie** in de Microsoft Azure-portal.

5. Schuif omlaag op de dezelfde pagina en voer de volgende stappen uit onder **Gebruikers identificeren**:

    ![image](./media/appraisd-tutorial/tutorial_appraisd_identifying.png)

    a. Plak in het tekstvak **Identity Provider Single Sign-On URL** de waarde van de **aanmeldings-URL** die u hebt gekopieerd uit de Microsoft Azure-portal en klik op **Save**.

    b. Plak in het tekstvak **Identity Provider Issuer URL** de waarde van de **Microsoft Azure Active Directory-id** die u hebt gekopieerd uit de Microsoft Azure-portal en klik op **Save**.

    c. Open in Kladblok het base-64 gecodeerde certificaat dat u hebt gedownload vanuit Azure Portal, kopieer de inhoud en plak deze in het vak  **X.509-certificaat**  en klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon** in.
  
    b. In de **gebruikersnaam** veld, typt u **brittasimon\@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In dit gedeelte gaat u Britta Simon toestemming geven voor gebruik van eenmalige aanmelding met Azure door haar toegang te geven tot Appraisd.

1. Selecteer **Bedrijfstoepassingen** in de Microsoft Azure-portal, selecteer **Alle toepassingen** en selecteer vervolgens **Appraisd**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer **Appraisd** in de lijst met toepassingen.

    ![De Appraisd-koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst Gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-appraisd-test-user"></a>Appraisd testgebruiker maken

Als u wilt dat Microsoft Azure Active Directory-gebruikers zich kunnen aanmelden bij Appraisd, moeten ze worden ingericht in Appraisd. In het geval van Appraisd is dat een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Meld u bij Appraisd als een beveiligingsbeheerder aan.

2. Klik rechts boven in de pagina op het pictogram **Settings** en navigeer vervolgens naar  **Administration centre**.

    ![image](./media/appraisd-tutorial/tutorial_appraisd_admin.png)

3. Klik in de werkbalk boven aan de pagina op  **People** en navigeer naar  **Add a new user**.

    ![image](./media/appraisd-tutorial/tutorial_appraisd_user.png)

4. Voer de volgende stappen uit in het dialoogvenster **Add a new user**:

    ![image](./media/appraisd-tutorial/tutorial_appraisd_newuser.png)

    a. Typ in het tekstvak **First Name** de voornaam van de gebruiker, zoals **Britta**.

    b. Typ in het tekstvak **Last Name** de achternaam van de gebruiker, zoals **Simon**.

    c. In **e** tekst vak, voer het e-mailadres van gebruiker, zoals **Brittasimon\@contoso.com**.

    d. Klik op **Gebruiker toevoegen**.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Appraisd in het toegangsvenster klikt, wordt u automatisch aangemeld bij de instantie van Appraisd waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
