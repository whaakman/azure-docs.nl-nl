---
title: 'Zelfstudie: Azure Active Directory-integratie met RFPIO | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en RFPIO.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 87187076-7b50-4247-814f-f217b052703f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/14/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 63d7b6af8ff76c890b98c29ded0e8bdc637b45dd
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67092848"
---
# <a name="tutorial-azure-active-directory-integration-with-rfpio"></a>Zelfstudie: Azure Active Directory-integratie met RFPIO

In deze zelfstudie leert u hoe u RFPIO integreren met Azure Active Directory (Azure AD).
RFPIO integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot RFPIO heeft.
* U kunt uw gebruikers worden automatisch aangemeld RFPIO (Single Sign-On) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met RFPIO, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* Eenmalige aanmelding RFPIO ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor RFPIO **SP en IDP** gestart door SSO

## <a name="adding-rfpio-from-the-gallery"></a>RFPIO uit de galerie toe te voegen

Voor het configureren van de integratie van RFPIO in Azure AD, moet u RFPIO uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen RFPIO uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)** , klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **RFPIO**, selecteer **RFPIO** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![RFPIO in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met RFPIO op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in RFPIO tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met RFPIO, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)** : als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureren van eenmalige aanmelding RFPIO](#configure-rfpio-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)** : als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)** : als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maken van de testgebruiker RFPIO](#create-rfpio-test-user)**  : als u wilt een equivalent van Britta Simon in RFPIO die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)** : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met RFPIO, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **RFPIO** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. Op de **SAML-basisconfiguratie** sectie, als u wilt configureren van de toepassing in **IDP** gestart modus, de volgende stap uitvoeren:

    ![RFPIO domein en URL's, eenmalige aanmelding informatie](common/idp-identifier.png)

    a. In het tekstvak **Id** typt u een URL met het volgende patroon: `https://www.rfpio.com`

    b. Klik op **Extra URL's instellen**.

    c. In de **Relaystatus** tekstvak een string-waarde. Neem contact op met [RFPIO ondersteuningsteam](https://www.rfpio.com/contact/) deze waarde op te halen.

    ![RFPIO domein en URL's, eenmalige aanmelding informatie](common/idp-preintegrated-relay.png)

5. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    ![image](common/both-preintegrated-signon.png)

    In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://www.app.rfpio.com`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden met de werkelijke-id en de aanmeldings-URL. Neem contact op met [RFPIO Client ondersteuningsteam](https://www.rfpio.com/contact/) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

6. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

7. Op de **RFPIO instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-rfpio-single-sign-on"></a>Configureer RFPIO Single Sign-On

1. In een ander browservenster aanmelden bij de **RFPIO** website als beheerder.

1. Klik op de onderste linkerhoek vervolgkeuzelijst.

    ![Eenmalige aanmelding configureren](./media/rfpio-tutorial/app1.png)

1. Klik op de **organisatie-instellingen**. 

    ![Eenmalige aanmelding configureren](./media/rfpio-tutorial/app2.png)

1. Klik op de **functies en integratie**.

    ![Eenmalige aanmelding configureren](./media/rfpio-tutorial/app4.png)

1. In de **SAML SSO-configuratie** klikt u op **bewerken**.

    ![Eenmalige aanmelding configureren](./media/rfpio-tutorial/app3.png)

1. In deze sectie voert u de volgende acties:

    ![Eenmalige aanmelding configureren](./media/rfpio-tutorial/app5.png)
    
    a. Kopieer de inhoud van de **gedownload metagegevens-XML** en plak deze in de **identiteit configuratie** veld.

    > [!NOTE]
    >Kopieer de inhoud van gedownload **federatieve metagegevens-XML** gebruik **Kladblok ++** of een beginhoofdletter **XML-Editor**.

    b. Klik op **valideren**.

    c. Nadat u hebt geklikt **valideren**, spiegelen **SAML(Enabled)** op aan.

    d. Klik op **Indienen**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

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

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan RFPIO.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **RFPIO**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **RFPIO**.

    ![De koppeling RFPIO in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-rfpio-test-user"></a>RFPIO testgebruiker maken

1. Meld u aan uw bedrijf RFPIO site als een beheerder.

1. Klik op de onderste linkerhoek vervolgkeuzelijst.

    ![Eenmalige aanmelding configureren](./media/rfpio-tutorial/app1.png)

1. Klik op de **organisatie-instellingen**. 

    ![Eenmalige aanmelding configureren](./media/rfpio-tutorial/app2.png)

1. Klik op **TEAMLEDEN**.

    ![Eenmalige aanmelding configureren](./media/rfpio-tutorial/app6.png)

1. Klik op **leden toevoegen**.

    ![Eenmalige aanmelding configureren](./media/rfpio-tutorial/app7.png)

1. In de **nieuwe leden toevoegen** sectie. Voer de volgende acties:

    ![Eenmalige aanmelding configureren](./media/rfpio-tutorial/app8.png)

    a. ENTER **e-mailadres** in de **Voer één e-mailadres per regel** veld.

    b. Selecteer **rol** volgens uw vereisten.

    c. Klik op **leden toevoegen**.

    > [!NOTE]
    > De houder van het Azure Active Directory-account ontvangt een e-mail en volgt een koppeling om het account te bevestigen voordat het actief wordt.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel RFPIO in het toegangsvenster, moet u worden automatisch aangemeld bij de RFPIO waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

