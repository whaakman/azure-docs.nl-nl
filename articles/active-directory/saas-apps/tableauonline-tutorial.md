---
title: 'Zelfstudie: Azure Active Directory-integratie met Tableau Online | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Tableau Online.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 1d4b1149-ba3b-4f4e-8bce-9791316b730d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/05/2019
ms.author: jeedes
ms.openlocfilehash: cdebeefac1a77cd834466a8731d0652136bdbd47
ms.sourcegitcommit: 9f4eb5a3758f8a1a6a58c33c2806fa2986f702cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58906171"
---
# <a name="tutorial-azure-active-directory-integration-with-tableau-online"></a>Zelfstudie: Azure Active Directory-integratie met Tableau Online

In deze zelfstudie leert u hoe u Tableau Online integreren met Azure Active Directory (Azure AD).
Tableau Online integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot Tableau Online heeft.
* U kunt uw gebruikers worden automatisch aangemeld met Tableau Online (Single Sign-On) inschakelen met hun Azure AD-accounts.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Tableau Online, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* Tableau Online eenmalige aanmelding ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Tableau Online ondersteunt **SP** gestart door SSO

## <a name="adding-tableau-online-from-the-gallery"></a>Tableau Online toevoegen vanuit de galerie

Voor het configureren van de integratie van Online Tableau in Azure AD, moet u Tableau Online toevoegen vanuit de galerie aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen vanuit de galerie Tableau Online, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **Tableau Online**, selecteer **Tableau Online** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![Tableau Online in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Tableau Online op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Tableau Online tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Tableau Online, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureer Tableau Online Single Sign-On](#configure-tableau-online-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Tableau Online testgebruiker maken](#create-tableau-online-test-user)**  : als u wilt een equivalent van Britta Simon in Tableau Online die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met Tableau Online, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **Tableau Online** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Tableau Online domein en URL's, eenmalige aanmelding informatie](common/sp-identifier.png)

    a. In de **aanmeldings-URL** tekstvak typt u de URL: `https://sso.online.tableau.com/public/sp/login?alias=<entityid>`

    b. In de **id (entiteits-ID)** tekstvak typt u de URL: `https://sso.online.tableau.com/public/sp/metadata?alias=<entityid>`

    > [!NOTE]
    > U krijgt de `<entityid>` waarde uit de **instellen Tableau Online** sectie in deze zelfstudie. De waarde van de entiteit-ID worden **Azure AD-id** waarde in de **instellen Tableau Online** sectie.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

6. Op de **instellen Tableau Online** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-tableau-online-single-sign-on"></a>Tableau Online Single Sign-On configureren

1. In een ander browservenster aanmelden voor uw toepassing Tableau Online. Ga naar **instellingen** en vervolgens **verificatie**.

    ![Eenmalige aanmelding configureren](./media/tableauonline-tutorial/tutorial_tableauonline_09.png)

2. Om in te schakelen SAML, onder **verificatietypen** sectie. Controleer **inschakelen van een aanvullende verificatiemethode** en controleer vervolgens of **SAML** selectievakje.

    ![Eenmalige aanmelding configureren](./media/tableauonline-tutorial/tutorial_tableauonline_12.png)

3. Schuif naar beneden tot **bestand met metagegevens importeren in Tableau Online** sectie.  Klik op Bladeren en importeer het bestand met metagegevens, die u hebt gedownload van Azure AD. Klik vervolgens op **toepassen**.

   ![Eenmalige aanmelding configureren](./media/tableauonline-tutorial/tutorial_tableauonline_13.png)

4. In de **overeenkomen met asserties** sectie, voegt u de bijbehorende verklaring-naam van id-Provider voor **e-mailadres**, **voornaam**, en **achternaam**. Deze informatie ophalen uit Azure AD: 
  
    a. Ga in de Azure-portal op de **Tableau Online** toepassingspagina integratie.

    b. In de ** gebruikerskenmerken en Claims *** sectie, klikt u op het pictogram voor bewerken.

   ![Eenmalige aanmelding configureren](./media/tableauonline-tutorial/attributesection.png)

    c. Kopieer de naamruimtewaarde voor deze kenmerken: givenname, e-mail- en achternaam met behulp van de volgende stappen uit:

   ![Azure AD voor eenmalige aanmelding](./media/tableauonline-tutorial/tutorial_tableauonline_10.png)

    d. Klik op **user.givenname** waarde

    e. Kopieer de waarde van de **Namespace** tekstvak.

    ![Eenmalige aanmelding configureren](./media/tableauonline-tutorial/attributesection2.png)

    f. Als u wilt kopiëren van de naamruimte Herhaal waarden voor de e-mail- en achternaam de bovenstaande stappen.

    g. Schakel over naar de Online Tableau toepassing, en stel vervolgens de **gebruikerskenmerken en Claims** sectie als volgt:

    * E-mailadres: **e-mail** of **userprincipalname**

    * Voornaam: **givenname**

    * Achternaam: **achternaam**

    ![Eenmalige aanmelding configureren](./media/tableauonline-tutorial/tutorial_tableauonline_14.png)

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

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen tot Tableau Online.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **Tableau Online**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **Tableau Online**.

    ![De Online Tableau koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-tableau-online-test-user"></a>Tableau Online testgebruiker maken

In deze sectie maakt u een gebruiker met de naam van Britta Simon in Tableau Online.

1. Op **Tableau Online**, klikt u op **instellingen** en vervolgens **verificatie** sectie. Schuif omlaag naar **gebruikers beheren** sectie. Klik op **gebruikers toevoegen** en klik vervolgens op **e-mailadressen invoeren**.
  
    ![Het maken van een Azure AD-testgebruiker](./media/tableauonline-tutorial/tutorial_tableauonline_15.png)

2. Selecteer **toevoegen van gebruikers voor verificatie (SAML)**. In de **Enter e-mailadressen** tekstvak toevoegen britta.simon@contoso.com
  
    ![Het maken van een Azure AD-testgebruiker](./media/tableauonline-tutorial/tutorial_tableauonline_11.png)

3. Klik op **gebruikers toevoegen**.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Tableau Online in het toegangsvenster, moet u worden automatisch aangemeld bij de waarvoor u een SSO instellen van het type Tableau Online. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
