---
title: 'Zelfstudie: Azure Active Directory-integratie met StatusPage | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en StatusPage.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: f6ee8bb3-df43-4c0d-bf84-89f18deac4b9
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/22/2019
ms.author: jeedes
ms.openlocfilehash: 4b14d268c42938e9ccadb1d64ad8627ac6f0a6d7
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/13/2019
ms.locfileid: "59549930"
---
# <a name="tutorial-azure-active-directory-integration-with-statuspage"></a>Zelfstudie: Azure Active Directory-integratie met StatusPage

In deze zelfstudie leert u hoe u StatusPage integreren met Azure Active Directory (Azure AD).
StatusPage integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot StatusPage heeft.
* U kunt uw gebruikers worden automatisch aangemeld StatusPage (Single Sign-On) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met StatusPage, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* Eenmalige aanmelding StatusPage ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor StatusPage **IDP** gestart door SSO

## <a name="adding-statuspage-from-the-gallery"></a>StatusPage uit de galerie toe te voegen

Voor het configureren van de integratie van StatusPage in Azure AD, moet u StatusPage uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen StatusPage uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **StatusPage**, selecteer **StatusPage** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![StatusPage in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met StatusPage op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in StatusPage tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met StatusPage, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureren van eenmalige aanmelding StatusPage](#configure-statuspage-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maken van de testgebruiker StatusPage](#create-statuspage-test-user)**  : als u wilt een equivalent van Britta Simon in StatusPage die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met StatusPage, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **StatusPage** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. Op de pagina **Eenmalige aanmelding instellen met SAML** voert u de volgende stappen uit:

    ![StatusPage domein en URL's, eenmalige aanmelding informatie](common/idp-intiated.png)

    a. In het tekstvak **Id** typt u een URL met het volgende patroon:
    
    | |
    |--|
    | `https://<subdomain>.statuspagestaging.com/`|
    | `https://<subdomain>.statuspage.io/`|

    b. In het tekstvak **Antwoord-URL** typt u een URL met het volgende patroon:
    
    | |
    |--|
    | `https://<subdomain>.statuspagestaging.com/sso/saml/consume`|
    | `https://<subdomain>.statuspage.io/sso/saml/consume`|

    > [!NOTE]
    > Neem contact op met het ondersteuningsteam StatusPage op [ SupportTeam@statuspage.io ](mailto:SupportTeam@statuspage.io)om aan te vragen van metagegevens die nodig zijn voor het configureren van eenmalige aanmelding. 
    >
    > a. Kopieer de uitgeverwaarde van de metagegevens en plak deze in de **id** tekstvak.
    >
    > b. De antwoord-URL kopiëren van de metagegevens en plak deze in de **antwoord-URL** tekstvak.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

6. Op de **StatusPage instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-statuspage-single-sign-on"></a>StatusPage voor eenmalige aanmelding configureren

1. In een ander browservenster aanmelden bij uw bedrijf StatusPage site als beheerder.

1. Klik in de belangrijkste werkbalk **Account beheren**.

    ![Eenmalige aanmelding configureren](./media/statuspage-tutorial/tutorial_statuspage_06.png)

1. Klik op de **Single Sign-on** tabblad.

    ![Eenmalige aanmelding configureren](./media/statuspage-tutorial/tutorial_statuspage_07.png)

1. Voer de volgende stappen uit op de pagina instellingen voor eenmalige aanmelding:

    ![Eenmalige aanmelding configureren](./media/statuspage-tutorial/tutorial_statuspage_08.png)

    ![Eenmalige aanmelding configureren](./media/statuspage-tutorial/tutorial_statuspage_09.png)

    a. In de **doel-URL voor eenmalige aanmelding** tekstvak, plak de waarde van **aanmeldings-URL**, die u hebt gekopieerd vanuit Azure portal.

    b. Open het gedownloade certificaat in Kladblok, Kopieer de inhoud en plak deze in de **certificaat** tekstvak.

    c. Klik op **configuratie opslaan**.

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

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan StatusPage.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **StatusPage**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **StatusPage**.

    ![De koppeling StatusPage in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-statuspage-test-user"></a>StatusPage testgebruiker maken

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in StatusPage.

StatusPage biedt ondersteuning voor just-in-time inrichting. U hebt al ingeschakeld in [configureren Azure AD Single Sign-On](#configure-azure-ad-single-sign-on).

**Voor het maken van een gebruiker met de naam van Britta Simon in StatusPage, moet u de volgende stappen uitvoeren:**

1. Aanmelding bij uw bedrijf StatusPage site als beheerder.

1. Klik in het menu aan de bovenkant op **Account beheren**.

    ![Eenmalige aanmelding configureren](./media/statuspage-tutorial/tutorial_statuspage_06.png)

1. Klik op de **teamleden** tabblad.
  
    ![Het maken van een Azure AD-testgebruiker](./media/statuspage-tutorial/tutorial_statuspage_10.png) 

1. Klik op **TEAMLID toevoegen**.
  
    ![Het maken van een Azure AD-testgebruiker](./media/statuspage-tutorial/tutorial_statuspage_11.png) 

1. Type de **e-mailadres**, **voornaam**, en **achternaam** van een geldige gebruiker die u inrichten in de bijbehorende tekstvakken wilt. 

    ![Het maken van een Azure AD-testgebruiker](./media/statuspage-tutorial/tutorial_statuspage_12.png) 

1. Als **rol**, kiest u **Clientbeheerder**.

1. Klik op **maken ACCOUNT**.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel StatusPage in het toegangsvenster, moet u worden automatisch aangemeld bij de StatusPage waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
