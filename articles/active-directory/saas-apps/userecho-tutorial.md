---
title: 'Zelfstudie: Azure Active Directory-integratie met UserEcho | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en UserEcho.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: bedd916b-8f69-4b50-9b8d-56f4ee3bd3ed
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/29/2019
ms.author: jeedes
ms.openlocfilehash: 59d61eda7002fe46cf99fac63822b2333b2d64b5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67087772"
---
# <a name="tutorial-azure-active-directory-integration-with-userecho"></a>Zelfstudie: Azure Active Directory-integratie met UserEcho

In deze zelfstudie leert u hoe u UserEcho integreren met Azure Active Directory (Azure AD).
UserEcho integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot UserEcho heeft.
* U kunt uw gebruikers worden automatisch aangemeld UserEcho (Single Sign-On) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met UserEcho, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* Eenmalige aanmelding UserEcho ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor UserEcho **SP** gestart door SSO

## <a name="adding-userecho-from-the-gallery"></a>UserEcho uit de galerie toe te voegen

Voor het configureren van de integratie van UserEcho in Azure AD, moet u UserEcho uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen UserEcho uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)** , klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **UserEcho**, selecteer **UserEcho** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![UserEcho in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met UserEcho op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in UserEcho tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met UserEcho, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)** : als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureren van eenmalige aanmelding UserEcho](#configure-userecho-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)** : als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)** : als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maken van de testgebruiker UserEcho](#create-userecho-test-user)**  : als u wilt een equivalent van Britta Simon in UserEcho die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)** : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met UserEcho, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **UserEcho** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![UserEcho domein en URL's, eenmalige aanmelding informatie](common/sp-identifier.png)

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<companyname>.userecho.com/`

    b. In het tekstvak **Id (Entiteits-id)** typt u een URL met de volgende notatie: `https://<companyname>.userecho.com/saml/metadata/`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke aanmeldings-URL en -id. Neem contact op met [UserEcho Client ondersteuningsteam](https://feedback.userecho.com/) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

4. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

6. Op de **UserEcho instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-userecho-single-sign-on"></a>UserEcho voor eenmalige aanmelding configureren

1. In een ander browservenster, meld u aan bij uw bedrijf UserEcho site als beheerder.

2. Klik op de naam van de gebruiker om het menu te openen in de werkbalk in de rechterbovenhoek en klik vervolgens op **Setup**.
   
    ![Eenmalige aanmelding configureren](./media/userecho-tutorial/tutorial_userecho_06.png) 

3. Klik op **integraties**.
   
    ![Eenmalige aanmelding configureren](./media/userecho-tutorial/tutorial_userecho_07.png) 

4. Klik op **Website**, en klik vervolgens op **eenmalige aanmelding (SAML2)** .
   
    ![Eenmalige aanmelding configureren](./media/userecho-tutorial/tutorial_userecho_08.png) 

5. Op de **eenmalige aanmelding (SAML)** pagina, voert u de volgende stappen uit:
   
    ![Eenmalige aanmelding configureren](./media/userecho-tutorial/tutorial_userecho_09.png)
    
    a. Als **SAML-functionaliteit**, selecteer **Ja**.
    
    b. Plakken **aanmeldings-URL**, die u hebt gekopieerd vanuit de Azure portal in de **URL voor SAML SSO-** tekstvak.
    
    c. Plakken **afmeldings-URL van**, die u hebt gekopieerd vanuit de Azure portal in de **externe URL voor afmelden** tekstvak.
    
    d. Open het gedownloade certificaat in Kladblok, Kopieer de inhoud en plak deze in de **X.509-certificaat** tekstvak.
    
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
  
    b. In de **gebruikersnaam** veldtype brittasimon@yourcompanydomain.extension. Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan UserEcho.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **UserEcho**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **UserEcho**.

    ![De koppeling UserEcho in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-userecho-test-user"></a>UserEcho testgebruiker maken

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in UserEcho.

**Voor het maken van een gebruiker met de naam van Britta Simon in UserEcho, moet u de volgende stappen uitvoeren:**

1. Aanmelding bij uw bedrijf UserEcho site als beheerder.

2. Klik op de naam van de gebruiker om het menu te openen in de werkbalk in de rechterbovenhoek en klik vervolgens op **Setup**.
   
    ![Eenmalige aanmelding configureren](./media/userecho-tutorial/tutorial_userecho_06.png)

3. Klik op **gebruikers**, om uit te breiden de **gebruikers** sectie.
   
    ![Eenmalige aanmelding configureren](./media/userecho-tutorial/tutorial_userecho_10.png)

4. Klik op **Users**.
   
    ![Eenmalige aanmelding configureren](./media/userecho-tutorial/tutorial_userecho_11.png)

5. Klik op **uitnodigen een nieuwe gebruiker**.
   
    ![Eenmalige aanmelding configureren](./media/userecho-tutorial/tutorial_userecho_12.png)

6. Op de **uitnodigen een nieuwe gebruiker** dialoogvenster, voer de volgende stappen uit:
   
    ![Eenmalige aanmelding configureren](./media/userecho-tutorial/tutorial_userecho_13.png)

    a. In de **naam** tekstvak, de typenaam van de gebruiker, zoals Britta Simon.
    
    b.  Typ in het tekstvak **Email** het e-mailadres van de gebruiker, bijvoorbeeld Brittasimon@contoso.com.
    
    c. Klik op **Uitnodigen**.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel UserEcho in het toegangsvenster, moet u worden automatisch aangemeld bij de UserEcho waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

