---
title: 'Zelfstudie: Azure Active Directory-integratie met KnowledgeOwl | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en KnowledgeOwl.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 2ae30996-864d-4872-90bc-f770e1ea159a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2018
ms.author: jeedes
ms.openlocfilehash: 1ac63d512645b2eb9627798809ce699129799c94
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-knowledgeowl"></a>Zelfstudie: Azure Active Directory-integratie met KnowledgeOwl

In deze zelfstudie leert u hoe KnowledgeOwl integreren met Azure Active Directory (Azure AD).

KnowledgeOwl integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot KnowledgeOwl heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij KnowledgeOwl (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met KnowledgeOwl, moet u de volgende items:

- Een Azure AD-abonnement
- Een KnowledgeOwl eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. KnowledgeOwl uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-knowledgeowl-from-the-gallery"></a>KnowledgeOwl uit de galerie toevoegen
Voor het configureren van de integratie van KnowledgeOwl in Azure AD, moet u KnowledgeOwl uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen KnowledgeOwl uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **KnowledgeOwl**, selecteer **KnowledgeOwl** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![KnowledgeOwl in de lijst met resultaten](./media/active-directory-saas-knowledgeowl-tutorial/tutorial_knowledgeowl_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met KnowledgeOwl op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in KnowledgeOwl is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in KnowledgeOwl tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met KnowledgeOwl, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker KnowledgeOwl](#create-a-knowledgeowl-test-user)**  - KnowledgeOwl die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing KnowledgeOwl configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met KnowledgeOwl, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **KnowledgeOwl** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-knowledgeowl-tutorial/tutorial_knowledgeowl_samlbase.png)

3. Op de **KnowledgeOwl domein en de URL's** sectie, voert u de volgende stappen uit als u wilt configureren, de toepassing in **IDP** modus gestart:

    ![URL's en KnowledgeOwl domein eenmalige aanmelding informatie](./media/active-directory-saas-knowledgeowl-tutorial/tutorial_knowledgeowl_url.png)

    a. In de **id (entiteit-ID)** textbox, typ een URL met het volgende patroon volgen:
    |||
    |-|-|
    | `https://app.knowledgeowl.com/sp`|
    | `https://app.knowledgeowl.com/sp/id/<unique ID>`|
    |||

    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:
    |||
    |-|-|
    | `https://subdomain.knowledgeowl.com/help/saml-login`|
    | `https://subdomain.knowledgeowl.com/docs/saml-login`|
    | `https://subdomain.knowledgeowl.com/home/saml-login`|
    | `https://privatedomain.com/help/saml-login`|
    | `https://privatedomain.com/docs/saml-login`|
    | `https://privatedomain.com/home/saml-login`|
    |||

4. Controleer **weergeven geavanceerde instellingen voor URL** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![URL's en KnowledgeOwl domein eenmalige aanmelding informatie](./media/active-directory-saas-knowledgeowl-tutorial/tutorial_knowledgeowl_url1.png)

    In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:
    |||
    |-|-|
    | `https://subdomain.knowledgeowl.com/help/saml-login`|
    | `https://subdomain.knowledgeowl.com/docs/saml-login`|
    | `https://subdomain.knowledgeowl.com/home/saml-login`|
    | `https://privatedomain.com/help/saml-login`|
    | `https://privatedomain.com/docs/saml-login`|
    | `https://privatedomain.com/home/saml-login`|
    |||
     
    > [!NOTE]
    > Deze waarden zijn niet echt. U moet deze waarde van de werkelijke identificatie, antwoord-URL en aanmeldings-URL die verderop in de zelfstudie wordt uitgelegd bijwerken.

5. De toepassing KnowledgeOwl verwacht de SAML-asserties in een specifieke indeling waarvoor u aangepaste kenmerktoewijzingen toevoegen aan uw configuratie van SAML-token kenmerken. Configureer de volgende claims voor deze toepassing. U kunt beheren de waarden van deze kenmerken van de '**gebruikerskenmerken**' sectie op de pagina van de toepassing-integratie.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-knowledgeowl-tutorial/attribute.png)

6. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de voorgaande afbeelding en de volgende stappen uitvoeren:
    
    | Naam kenmerk | Waarde kenmerk | Naamruimte|
    | ------------------- | -------------------- | -----|
    | ssoid | User.mail | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims`|
    
    a. Klik op **toevoegen kenmerk** openen de **kenmerk toevoegen** dialoogvenster.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-knowledgeowl-tutorial/tutorial_attribute_04.png)

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-knowledgeowl-tutorial/tutorial_attribute_05.png)

    b. In de **naam** textbox, typ de naam van het kenmerk wordt weergegeven voor die rij.
    
    c. Van de **waarde** typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d.From de **Namespace** , voert u de naamruimtewaarde wordt weergegeven voor die rij.
    
    e. Klik op **OK**.

7. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Raw)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-knowledgeowl-tutorial/tutorial_knowledgeowl_certificate.png) 

8. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-knowledgeowl-tutorial/tutorial_general_400.png)
    
9. Op de **KnowledgeOwl configuratie** sectie, klikt u op **configureren KnowledgeOwl** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![KnowledgeOwl configuratie](./media/active-directory-saas-knowledgeowl-tutorial/tutorial_knowledgeowl_configure.png)

10. In een ander browservenster, meld u bij uw bedrijf KnowledgeOwl site als beheerder.

11. Klik op **instellingen** en selecteer vervolgens **beveiliging**.

    ![KnowledgeOwl configuratie](./media/active-directory-saas-knowledgeowl-tutorial/configure1.png)

12. Schuif omlaag tot aan de **SAML SSO-integratie** en voer de volgende stappen uit:
    
    ![KnowledgeOwl configuratie](./media/active-directory-saas-knowledgeowl-tutorial/configure2.png)

    a. Selecteer **SAML eenmalige aanmelding inschakelen**.

    b. Kopieer de **SP entiteit-ID** waarde en plak deze in de **id (entiteit-ID)** in de **KnowledgeOwl domein en de URL's** sectie in de Azure portal.

    c. Kopieer de **SP aanmeldings-URL** waarde en plak deze in de **aanmeldings-URL en de antwoord-URL** tekstvakken in de **KnowledgeOwl domein en de URL's** sectie in de Azure portal.

    d. In de **IdP id van de entiteit** textbox, plak de **SAML entiteit-ID** waarde, die u hebt gekopieerd uit de Azure portal.

    e. In de **IdP aanmeldings-URL** textbox, plak de **SAML Single Sign-On Service-URL** waarde, die u hebt gekopieerd uit de Azure portal.

    f. In de **IdP afmelding URL** textbox, plak de **Sign-Out URL** waarde, die u hebt gekopieerd uit de Azure portal

    g. Uploaden van het formulier voor gedownloade certificaat de Azure-portal door te klikken op de **IdP-certificaat uploaden**.

    h. Klik op **kaart SAML kenmerken** kenmerken toewijzen en voer de volgende stappen uit:
    
    ![KnowledgeOwl configuratie](./media/active-directory-saas-knowledgeowl-tutorial/configure3.png)

    * Voer `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/ssoid` in de **SSO-ID** tekstvak
    * Voer `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` in de **gebruikersnaam, e** textbox.
    * Voer `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname` in de **voornaam** textbox.
    * Voer `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname` in de **achternaam** textbox.
    * Klik op **Opslaan**.

    i. Klik op **Opslaan** onder aan de pagina.

    ![KnowledgeOwl configuratie](./media/active-directory-saas-knowledgeowl-tutorial/configure4.png)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-knowledgeowl-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-knowledgeowl-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-knowledgeowl-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-knowledgeowl-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-knowledgeowl-test-user"></a>Een testgebruiker KnowledgeOwl maken

Het doel van deze sectie is het maken van een gebruiker Britta Simon in KnowledgeOwl genoemd. KnowledgeOwl ondersteunt just-in-time-inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot KnowledgeOwl als deze nog niet bestaat.
>[!Note]
>Als u maken van een gebruiker handmatig wilt, neem dan contact op met [KnowledgeOwl ondersteuningsteam](mailto:support@knowledgeowl.com).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan KnowledgeOwl.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen KnowledgeOwl, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **KnowledgeOwl**.

    ![De koppeling KnowledgeOwl in de lijst met toepassingen](./media/active-directory-saas-knowledgeowl-tutorial/tutorial_knowledgeowl_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel KnowledgeOwl in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing KnowledgeOwl.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-knowledgeowl-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-knowledgeowl-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-knowledgeowl-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-knowledgeowl-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-knowledgeowl-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-knowledgeowl-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-knowledgeowl-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-knowledgeowl-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-knowledgeowl-tutorial/tutorial_general_203.png

