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
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2018
ms.author: jeedes
ms.openlocfilehash: e902f5969611dd3b1074e899003abe5067857c04
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39446135"
---
# <a name="tutorial-azure-active-directory-integration-with-knowledgeowl"></a>Zelfstudie: Azure Active Directory-integratie met KnowledgeOwl

In deze zelfstudie leert u hoe u KnowledgeOwl integreren met Azure Active Directory (Azure AD).

KnowledgeOwl integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot KnowledgeOwl heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij KnowledgeOwl (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met KnowledgeOwl, moet u de volgende items:

- Een Azure AD-abonnement
- Een KnowledgeOwl eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. KnowledgeOwl uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-knowledgeowl-from-the-gallery"></a>KnowledgeOwl uit de galerie toe te voegen
Voor het configureren van de integratie van KnowledgeOwl in Azure AD, moet u KnowledgeOwl uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen KnowledgeOwl uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

1. Typ in het zoekvak **KnowledgeOwl**, selecteer **KnowledgeOwl** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![KnowledgeOwl in de lijst met resultaten](./media/knowledgeowl-tutorial/tutorial_knowledgeowl_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met KnowledgeOwl op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in KnowledgeOwl is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in KnowledgeOwl tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met KnowledgeOwl, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Maak een testgebruiker KnowledgeOwl](#create-a-knowledgeowl-test-user)**  : als u wilt een equivalent van Britta Simon in KnowledgeOwl die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing KnowledgeOwl.

**Voor het configureren van Azure AD eenmalige aanmelding met KnowledgeOwl, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **KnowledgeOwl** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/knowledgeowl-tutorial/tutorial_knowledgeowl_samlbase.png)

1. Op de **KnowledgeOwl domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![KnowledgeOwl domein en URL's, eenmalige aanmelding informatie](./media/knowledgeowl-tutorial/tutorial_knowledgeowl_url.png)

    a. In de **id (entiteits-ID)** tekstvak, een URL met behulp van het volgende patroon:
    |||
    |-|-|
    | `https://app.knowledgeowl.com/sp`|
    | `https://app.knowledgeowl.com/sp/id/<unique ID>`|
    |||

    b. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon:
    |||
    |-|-|
    | `https://subdomain.knowledgeowl.com/help/saml-login`|
    | `https://subdomain.knowledgeowl.com/docs/saml-login`|
    | `https://subdomain.knowledgeowl.com/home/saml-login`|
    | `https://privatedomain.com/help/saml-login`|
    | `https://privatedomain.com/docs/saml-login`|
    | `https://privatedomain.com/home/saml-login`|
    |||

1. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![KnowledgeOwl domein en URL's, eenmalige aanmelding informatie](./media/knowledgeowl-tutorial/tutorial_knowledgeowl_url1.png)

    In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon:
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
    > Deze waarden zijn niet echt. U moet deze waarde van werkelijke-id, de antwoord-URL en aanmeldings-URL die verderop in de zelfstudie wordt wordt bijgewerkt.

1. De toepassing KnowledgeOwl wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit beheren de "**gebruikerskenmerken**" sectie op de pagina van de toepassing-integratie.

    ![Eenmalige aanmelding configureren](./media/knowledgeowl-tutorial/attribute.png)

1. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de voorgaande afbeelding en voer de volgende stappen uit:
    
    | Naam kenmerk | Waarde kenmerk | Naamruimte|
    | ------------------- | -------------------- | -----|
    | ssoid | User.mail | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims`|
    
    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.
    
    ![Eenmalige aanmelding configureren](./media/knowledgeowl-tutorial/tutorial_attribute_04.png)

    ![Eenmalige aanmelding configureren](./media/knowledgeowl-tutorial/tutorial_attribute_05.png)

    b. In de **naam** tekstvak typt u de naam van het kenmerk wordt weergegeven voor die rij.
    
    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d.From de **Namespace** , voert u de naamruimtewaarde die wordt weergegeven voor die rij.
    
    e. Klik op **OK**.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Raw)** en slaat u het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/knowledgeowl-tutorial/tutorial_knowledgeowl_certificate.png) 

1. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/knowledgeowl-tutorial/tutorial_general_400.png)
    
1. Op de **KnowledgeOwl configuratie** sectie, klikt u op **configureren KnowledgeOwl** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![KnowledgeOwl configuratie](./media/knowledgeowl-tutorial/tutorial_knowledgeowl_configure.png)

1. Meld u in een ander browservenster in uw bedrijf KnowledgeOwl site als beheerder.

1. Klik op **instellingen** en selecteer vervolgens **Security**.

    ![KnowledgeOwl configuratie](./media/knowledgeowl-tutorial/configure1.png)

1. Schuif omlaag in maximaal de **SAML SSO-integratie** en voer de volgende stappen uit:
    
    ![KnowledgeOwl configuratie](./media/knowledgeowl-tutorial/configure2.png)

    a. Selecteer **SAML SSO inschakelen**.

    b. Kopieer de **SP entiteit-ID** waarde en plak deze in de **id (entiteits-ID)** in de **KnowledgeOwl domein en URL's** sectie in Azure portal.

    c. Kopieer de **SP aanmeldings-URL** waarde en plak deze in de **aanmeldings-URL en antwoord-URL** tekstvakken in de **KnowledgeOwl domein en URL's** sectie in Azure portal.

    d. In de **IdP entityID** tekstvak, plak de **SAML entiteit-ID** waarde die u hebt gekopieerd vanuit Azure portal.

    e. In de **IdP aanmeldings-URL** tekstvak, plak de **Single Sign-On Service URL voor SAML** waarde die u hebt gekopieerd vanuit Azure portal.

    f. In de **afmeldings-URL van id-provider** tekstvak, plak de **afmelding URL** waarde die u hebt gekopieerd vanuit Azure portal

    g. Uploaden van de gedownloade certificaat in Azure portal door te klikken op de **IdP-certificaat uploaden**.

    h. Klik op **kaart SAML kenmerken** kenmerken toewijzen en voer de volgende stappen uit:
    
    ![KnowledgeOwl configuratie](./media/knowledgeowl-tutorial/configure3.png)

    * Voer `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/ssoid` in de **SSO-ID** tekstvak
    * Voer `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` in de **gebruikersnaam, e** tekstvak.
    * Voer `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname` in de **voornaam** tekstvak.
    * Voer `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname` in de **achternaam** tekstvak.
    * Klik op **Opslaan**.

    i. Klik op **Opslaan** onder aan de pagina.

    ![KnowledgeOwl configuratie](./media/knowledgeowl-tutorial/configure4.png)

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/knowledgeowl-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/knowledgeowl-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/knowledgeowl-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/knowledgeowl-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-knowledgeowl-test-user"></a>Maak een testgebruiker KnowledgeOwl

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in KnowledgeOwl. KnowledgeOwl biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot KnowledgeOwl als deze nog niet bestaat.
>[!Note]
>Als u maken van een gebruiker handmatig wilt, neem dan contact op met [KnowledgeOwl ondersteuningsteam](mailto:support@knowledgeowl.com).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan KnowledgeOwl.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan KnowledgeOwl toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **KnowledgeOwl**.

    ![De koppeling KnowledgeOwl in de lijst met toepassingen](./media/knowledgeowl-tutorial/tutorial_knowledgeowl_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel KnowledgeOwl in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing KnowledgeOwl.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/knowledgeowl-tutorial/tutorial_general_01.png
[2]: ./media/knowledgeowl-tutorial/tutorial_general_02.png
[3]: ./media/knowledgeowl-tutorial/tutorial_general_03.png
[4]: ./media/knowledgeowl-tutorial/tutorial_general_04.png

[100]: ./media/knowledgeowl-tutorial/tutorial_general_100.png

[200]: ./media/knowledgeowl-tutorial/tutorial_general_200.png
[201]: ./media/knowledgeowl-tutorial/tutorial_general_201.png
[202]: ./media/knowledgeowl-tutorial/tutorial_general_202.png
[203]: ./media/knowledgeowl-tutorial/tutorial_general_203.png

