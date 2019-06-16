---
title: 'Zelfstudie: Azure Active Directory-integratie met Trello | Microsoft Docs'
description: Ontdek hoe u eenmalige aanmelding configureert tussen Azure Active Directory en Trello.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: cd5ae365-9ed6-43a6-920b-f7814b993949
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/02/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 730ff5ff12f18d1f85b3ca53adb42fee41e19fb4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67088297"
---
# <a name="tutorial-azure-active-directory-integration-with-trello"></a>Zelfstudie: Azure Active Directory-integratie met Trello

In deze zelfstudie leert u hoe u Trello kunt integreren met Azure Active Directory (Azure AD).
De integratie van Trello met Azure AD heeft de volgende voordelen:

* U kunt in Azure AD beheren wie toegang heeft tot Trello.
* U kunt uw gebruikers worden automatisch aangemeld met Trello (eenmalige aanmelding) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts in één centrale locatie kunt beheren: de Azure-portal.

Zie voor meer informatie over de integratie met Azure AD SaaS [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Trello hebt u het volgende nodig:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).
* Een abonnement Trello single sign-op-is ingeschakeld.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Trello biedt ondersteuning voor Serviceprovider en IDP-geïnitieerde eenmalige aanmelding

* Trello biedt ondersteuning voor Just In Time gebruikers inrichten

## <a name="add-trello-from-the-gallery"></a>Trello toevoegen uit de galerie

Het configureren van de integratie van Trello in Azure AD, eerst Trello uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

Als u wilt toevoegen Trello uit de galerie, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster de **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Selecteer **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u wilt een nieuwe toepassing toevoegen, selecteert u de **nieuwe toepassing** knop aan de bovenkant van het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Voer in het zoekvak **Trello**, en selecteer vervolgens **Trello** vanuit het deelvenster met resultaten.

5. Selecteer de **toevoegen** om toe te voegen van de toepassing.

     ![Trello in de resultatenlijst](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Trello op basis van een testgebruiker met de naam **Britta Simon**.

Voor eenmalige aanmelding om te werken, moet u een koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker tot stand te brengen in Trello.

Om te configureren en testen van Azure AD eenmalige aanmelding met Trello, moet u de volgende bouwstenen voltooien:

1. [Eenmalige aanmelding van Azure AD configureren](#configure-azure-ad-single-sign-on) zodat uw gebruikers deze functie kunnen gebruiken.
2. [Configureren van eenmalige aanmelding Trello](#configure-trello-single-sign-on) de instellingen voor eenmalige aanmelding configureren aan de toepassing.
3. [Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user) voor het testen van eenmalige aanmelding van Azure AD met Britta Simon.
4. [De Azure AD-testgebruiker toewijzen](#assign-the-azure-ad-test-user) zodat Britta Simon gebruik kan maken van eenmalige aanmelding van Azure AD.
5. [Maak een testgebruiker Trello](#create-a-trello-test-user) hebben een equivalent van Britta Simon in Trello die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. [Eenmalige aanmelding testen](#test-single-sign-on) om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

> [!NOTE]
> U krijgt de **\<enterprise\>** -slug van Trello. Als u de waarde van de tijdelijke-aanduidingsveld geen hebt, neem dan contact op met de [Trello-ondersteuningsteam](mailto:support@trello.com) om op te halen van het tijdelijke-aanduidingsveld voor uw onderneming.

Voor het configureren van Azure AD eenmalige aanmelding met Trello, voert u de volgende stappen uit:

1. In de [Azure-portal](https://portal.azure.com/) selecteert u **Eenmalige aanmelding** op de integratiepagina van de toepassing **Trello**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In de **selecteert u een methode voor eenmalige aanmelding** in het dialoogvenster, selecteer **SAML** eenmalige aanmelding inschakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de **instellen van eenmalige aanmelding met SAML** weergeeft, schakelt de **bewerken** pictogram opent de **SAML-basisconfiguratie** in het dialoogvenster.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de **SAML-basisconfiguratie** sectie, als u wilt dat het configureren van de toepassing in de modus voor IDP gestart door de volgende stappen uit:

    ![Trello-domein en URL's, eenmalige aanmelding informatie](common/idp-intiated.png)

    a. In de **id** voert u een URL met behulp van het volgende patroon: `https://trello.com/auth/saml/metadata`

    b. In de **antwoord-URL** voert u een URL met behulp van het volgende patroon: `https://trello.com/auth/saml/consume/<enterprise>`

5. Selecteer **extra URL's instellen**, en vervolgens de volgende stappen uit als u wilt configureren van de toepassing in de modus SP geïnitieerde:

    ![Trello-domein en URL's, eenmalige aanmelding informatie](common/metadata-upload-additional-signon.png)

    In de **aanmeldings-URL** voert u een URL met behulp van het volgende patroon:  `https://trello.com/auth/saml/login/<enterprise>`

    > [!NOTE]
    > Dit zijn geen echte waarden. Deze waarden bijwerken met de werkelijke-id, antwoord-URL en aanmeldings-URL. Neem contact op met de [Trello-Client-ondersteuningsteam](mailto:support@trello.com) om deze waarden te verkrijgen. U kunt ook verwijzen naar de patronen in de **SAML-basisconfiguratie** sectie in Azure portal.

6. De toepassing Trello wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit de sectie **Gebruikerskenmerken** op de integratiepagina van de toepassing-beheren. Op de **instellen van eenmalige aanmelding met SAML** weergeeft, schakelt de **bewerken** te openen de **gebruikerskenmerken** in het dialoogvenster.

    ![Het dialoogvenster Gebruikerskenmerken](common/edit-attribute.png)

7. In de sectie **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** configureert u het kenmerk van het SAML-token zoals wordt weergegeven in de bovenstaande afbeelding. Voer dan de volgende stappen uit:

    | Name |  Bronkenmerk|
    | --- | --- |
    | User.Email | user.mail |
    | User.FirstName | user.givenname |
    | User.LastName | user.surname |

    a. Selecteer **Nieuwe claim toevoegen** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    ![In het dialoogvenster claims](common/new-save-attribute.png)

    ![Gebruikersclaims beheren](common/new-attribute-details.png)

    b. In de **naam** voert u de naam van het kenmerk dat wordt weergegeven voor die rij.

    c. Laat **Namespace** leeg.

    d. Voor **bron**, selecteer **kenmerk**.

    e. In de **bronkenmerk** , voert u de waarde van het kenmerk dat wordt weergegeven voor die rij.

    f. Selecteer **OK**.

    g. Selecteer **Opslaan**.

8. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, selecteer **downloaden** voor het downloaden van de **certificaat (Base64)**  uit de opgegeven opties overeenkomstig uw vereisten. Sla deze op uw computer.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

9. Op de **Trello instellen** sectie, kopieert u de juiste URL('s) volgens uw vereisten.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD identifier

    c. Afmeldings-URL

### <a name="configure-trello-single-sign-on"></a>Trello eenmalige aanmelding configureren

Voor het configureren van eenmalige aanmelding aan de Trello, eerst verzendt de gedownloade **certificaat (Base64)** en URL's gekopieerd vanuit Azure portal naar de [Trello-ondersteuningsteam](mailto:support@trello.com). Ze zorgen ervoor dat de SAML SSO-verbinding aan beide zijden correct is ingesteld.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken 

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![De knop Nieuwe gebruiker](common/new-user.png)

3. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon** in.
  
    b. In de **gebruikersnaam** Voer 'brittasimon@yourcompanydomain.extension'. Bijvoorbeeld: in dit geval, u kunt invoeren 'BrittaSimon@contoso.com'.

    c. Selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Selecteer **Maken**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In dit gedeelte gaat u Britta Simon toestemming geven voor gebruik van eenmalige aanmelding met Azure door haar toegang te geven tot Trello.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**, en selecteer vervolgens **Trello**.

    ![Blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer **Trello** in de lijst met toepassingen.

    ![De Trello-koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Selecteer de **gebruiker toevoegen** knop. Klik in de **toevoegen toewijzing** in het dialoogvenster, selecteer **gebruikers en groepen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. In de **gebruikers en groepen** in het dialoogvenster, selecteer **Britta Simon** in de gebruikerslijst. Klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.

6. Als u een waarde voor de rol verwacht in het SAML-verklaring vervolgens, in de **rol selecteren** dialoogvenster Selecteer de juiste rol voor de gebruiker in de lijst. Klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.

7. In de **toevoegen toewijzing** in het dialoogvenster, selecteer de **toewijzen** knop.

### <a name="create-a-trello-test-user"></a>Maak een testgebruiker Trello

In deze sectie maakt u een gebruiker met de naam van Britta Simon in Trello. Trello biedt ondersteuning voor Just-in-Time inrichten van gebruikers, die standaard is ingeschakeld. Er is geen actie-item voor u in deze sectie. Als er nog geen gebruiker in Trello bestaat, wordt er een nieuwe gemaakt na verificatie.

> [!NOTE]
> Als u maken van een gebruiker handmatig wilt, neem dan contact op met de [Trello-ondersteuningsteam](mailto:support@trello.com).

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie kunt u uw configuratie Azure AD eenmalige aanmelding testen met behulp van de MyApps-portal.

Wanneer u de tegel Trello in de MyApps-portal selecteert, moet u worden automatisch aangemeld met Trello. Zie voor meer informatie over de portal mijn Apps [wat is de MyApps-portal?](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

