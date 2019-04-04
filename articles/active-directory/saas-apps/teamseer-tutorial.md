---
title: 'Zelfstudie: Azure Active Directory-integratie met TeamSeer | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en TeamSeer.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 6ec4806f-fe0f-4ed7-8cfa-32d1c840433f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/07/2019
ms.author: jeedes
ms.openlocfilehash: d3f95001ff26fe21c29baf0c8721be914e8fdf73
ms.sourcegitcommit: 9f4eb5a3758f8a1a6a58c33c2806fa2986f702cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58905134"
---
# <a name="tutorial-azure-active-directory-integration-with-teamseer"></a>Zelfstudie: Azure Active Directory-integratie met TeamSeer

In deze zelfstudie leert u hoe u TeamSeer integreren met Azure Active Directory (Azure AD).
TeamSeer integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot TeamSeer heeft.
* U kunt uw gebruikers worden automatisch aangemeld TeamSeer (Single Sign-On) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met TeamSeer, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* Eenmalige aanmelding TeamSeer ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor TeamSeer **SP** gestart door SSO

## <a name="adding-teamseer-from-the-gallery"></a>TeamSeer uit de galerie toe te voegen

Voor het configureren van de integratie van TeamSeer in Azure AD, moet u TeamSeer uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen TeamSeer uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **TeamSeer**, selecteer **TeamSeer** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![TeamSeer in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met TeamSeer op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in TeamSeer tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met TeamSeer, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureren van eenmalige aanmelding TeamSeer](#configure-teamseer-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maken van de testgebruiker TeamSeer](#create-teamseer-test-user)**  : als u wilt een equivalent van Britta Simon in TeamSeer die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met TeamSeer, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **TeamSeer** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![TeamSeer domein en URL's, eenmalige aanmelding informatie](common/sp-signonurl.png)

    In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://www.teamseer.com/<companyid>`

    > [!NOTE]
    > De waarde is niet echt. Werk de waarde bij met de werkelijke aanmeldings-URL. Neem contact op met [TeamSeer Client ondersteuningsteam](https://pages.theaccessgroup.com/solutions_business-suite_absence-management_contact.html) om de waarde. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

6. Op de **TeamSeer instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-teamseer-single-sign-on"></a>TeamSeer voor eenmalige aanmelding configureren

1. In een ander browservenster aanmelden bij uw bedrijf TeamSeer site als beheerder.

1. Ga naar **HR Admin**.

    ![HR-beheerder](./media/teamseer-tutorial/ic789634.png "HR-beheerder")

1. Klik op **Setup**.

    ![Setup](./media/teamseer-tutorial/ic789635.png "Setup")

1. Klik op **details voor SAML-provider instellen**.

    ![SAML Settings](./media/teamseer-tutorial/ic789636.png "SAML Settings")

1. In de sectie van de informatie in de SAML-provider, moet u de volgende stappen uitvoeren:

    ![SAML Settings](./media/teamseer-tutorial/ic789637.png "SAML Settings")

    a. In de **URL** tekstvak, plak de **aanmeldings-URL** waarde die u hebt gekopieerd vanuit Azure portal.

    b. Het base-64 gecodeerde certificaat openen in Kladblok, Kopieer de inhoud van in naar het Klembord en plakt u deze naar de **IdP openbaar certificaat** tekstvak.

1. Voor het voltooien van de configuratie van de SAML-provider, moet u de volgende stappen uitvoeren:

    ![SAML Settings](./media/teamseer-tutorial/ic789638.png "SAML Settings")

    a. In de **e-mailadressen testen**, typ de e-mailadres van de testgebruiker.
  
    b. In de **verlener** tekstvak typt u de URL-verlener van de serviceprovider.
  
    c. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In het veld **Gebruikersnaam** typt u **brittasimon@yourcompanydomain.extension**.  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan TeamSeer.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **TeamSeer**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **TeamSeer**.

    ![De koppeling TeamSeer in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-teamseer-test-user"></a>TeamSeer testgebruiker maken

Om Azure AD-gebruikers zich aanmelden bij TeamSeer, moeten ze worden ingericht voor ShiftPlanning. In het geval van TeamSeer is inrichten een handmatige taak.

**Voer de volgende stappen uit als u een gebruikersaccount wilt inrichten:**

1. Aanmelden bij uw **TeamSeer** bedrijf site als beheerder.

1. Ga naar **HR Admin \> gebruikers** en klik vervolgens op **voert u de wizard nieuwe gebruiker**.

    ![HR-beheerder](./media/teamseer-tutorial/ic789640.png "HR-beheerder")

1. In de **Gebruikersdetails** sectie, voert u de volgende stappen uit:

    ![User Details](./media/teamseer-tutorial/ic789641.png "User Details")

    a. Type de **voornaam**, **achternaam**, **gebruikersnaam (e-mailadres)** van een geldige Azure AD-account die u inrichten wilt de bijbehorende tekstvakken.
  
    b. Klik op **volgende**.

1. Volg de aanwijzingen op het scherm instructies voor het toevoegen van een nieuwe gebruiker en klikt u op **voltooien**.

> [!NOTE]
> U kunt alle andere TeamSeer gebruiker-account maken van hulpprogramma's of API's geleverd door TeamSeer voor het inrichten van gebruikersaccounts van de Azure AD.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel TeamSeer in het toegangsvenster, moet u worden automatisch aangemeld bij de TeamSeer waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
