---
title: 'Zelfstudie: Azure Active Directory-integratie met Elium | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Elium.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: fae344b3-5bd9-40e2-9a1d-448dcd58155f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/05/2018
ms.author: jeedes
ms.openlocfilehash: 2c41b2c297d8d89554950e8c77201ff5bedf743e
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="tutorial-azure-active-directory-integration-with-elium"></a>Zelfstudie: Azure Active Directory-integratie met Elium

In deze zelfstudie leert u hoe Elium integreren met Azure Active Directory (Azure AD).

Elium integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Elium heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Elium (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Elium, moet u de volgende items:

- Een Azure AD-abonnement
- Een Elium eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Elium uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-elium-from-the-gallery"></a>Elium uit de galerie toevoegen
Voor het configureren van de integratie van Elium in Azure AD, moet u Elium uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Elium uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Elium**, selecteer **Elium** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Elium in de lijst met resultaten](./media/active-directory-saas-elium-tutorial/tutorial_elium_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met Elium op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Elium is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Elium tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Elium, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Elium](#create-an-elium-test-user)**  - Elium die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Elium configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Elium, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Elium** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-elium-tutorial/tutorial_elium_samlbase.png)

3. Op de **Elium domein en de URL's** sectie, voert u de volgende stappen uit als u wilt configureren, de toepassing in **IDP** modus gestart:

    ![URL's en Elium domein eenmalige aanmelding informatie](./media/active-directory-saas-elium-tutorial/tutorial_elium_url.png)

    a. In de **id** textbox, typ een URL met het volgende patroon volgen: `https://<platform-domain>.elium.com/login/saml2/metadata`

    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen: `https://<platform-domain>.elium.com/login/saml2/acs`

4. Controleer **weergeven geavanceerde instellingen voor URL** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![URL's en Elium domein eenmalige aanmelding informatie](./media/active-directory-saas-elium-tutorial/tutorial_elium_url1.png)

    In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen: ` https://<platform-domain>.elium.com/login/saml2/login`
     
    > [!NOTE] 
    > Deze waarden zijn niet echt. U krijgt deze waarden uit de **SP metagegevensbestand** downloadbare op `https://<platform-domain>.elium.com/login/saml2/metadata`, die verderop in deze zelfstudie wordt uitgelegd.

5. De toepassing Elium verwacht de SAML-asserties in een specifieke indeling waarvoor u aangepaste kenmerktoewijzingen toevoegen aan uw configuratie van SAML-token kenmerken. Configureer de volgende claims voor deze toepassing. U kunt beheren de waarden van deze kenmerken van de '**gebruikerskenmerken**' sectie op de pagina van de toepassing-integratie.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-elium-tutorial/tutorial_attribute.png)

6. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de voorgaande afbeelding en de volgende stappen uitvoeren:
           
    | Naam kenmerk | Waarde kenmerk |   
    | ---------------| ----------------|
    | e-mail   |User.mail |
    | first_name| User.givenName |
    | last_name| User.surname|
    | job_title| user.jobtitle|
    | Bedrijf| User.CompanyName|
    
    > [!NOTE]
    > Dit zijn de claims standaard. **Alleen de claim e-mailadres is vereist**. Claim is verplicht voor JIT inrichting ook alleen e-mailbericht. Andere aangepaste claims kunnen variëren van één klant platform naar een ander platform van de klant.

    a. Klik op **toevoegen kenmerk** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-elium-tutorial/tutorial_attribute_04.png)

    b. In de **naam** textbox, typ de naam van het kenmerk wordt weergegeven voor die rij.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-elium-tutorial/tutorial_attribute_05.png)

    c. Van de **waarde** typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. Laat leeg naamruimte.
    
    e. Klik op **OK**. 

5. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-elium-tutorial/tutorial_elium_certificate.png) 

6. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-elium-tutorial/tutorial_general_400.png)
    
7. In een ander browservenster, meld u aan bij uw bedrijf Elium site als beheerder.

8. Klik op de **gebruikersprofiel** van rechterbovenhoek en selecteer vervolgens **beheer**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-elium-tutorial/user1.png)

9. Selecteer **beveiliging** tabblad.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-elium-tutorial/user2.png)

10. Schuif omlaag naar de **eenmalige aanmelding (SSO)** sectie en voer de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-elium-tutorial/user3.png)

    a. Kopieer de waarde van **controleren dat SAML2-verificatie werkt voor uw account** en plak deze in de **aanmeldings-URL** textbox op de **Elium domein en de URL's** sectie in Azure Portal.

    > [!NOTE]
    > Na het configureren van eenmalige aanmelding, kunt u altijd toegang hebt tot de standaardpagina van de externe aanmelding op de volgende URL: `https://<platform_domain>/login/regular/login` 

    b. Selecteer **SAML2 inschakelen federation** selectievakje.

    c. Selecteer **JIT-inrichting** selectievakje.

    d. Open de **SP metagegevens** door te klikken op de **downloaden** knop.

    e. Zoeken naar de **id van de entiteit** in de **SP metagegevens** bestand, kopieert u de **id van de entiteit** waarde en plak deze in de **id** textbox op de  **URL's en Elium domein** sectie in de Azure-portal. 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-elium-tutorial/user4.png)

    f. Zoeken naar de **AssertionConsumerService** in de **SP metagegevens** bestand, kopieert u de **locatie** waarde en plak deze in de **antwoord-URL** TextBox op de **Elium domein en de URL's** sectie in de Azure-portal.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-elium-tutorial/user5.png)

    g. Het metagegevensbestand van de gedownloade vanuit Azure-portal openen in Kladblok, Kopieer de inhoud en plak deze in de **IdP metagegevens** textbox.

    h. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-elium-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-elium-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-elium-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-elium-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-an-elium-test-user"></a>Een testgebruiker Elium maken

Het doel van deze sectie is het maken van een gebruiker Britta Simon in Elium genoemd. Elium ondersteunt just-in-time-inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot Elium als deze nog niet bestaat.
>[!Note]
>Als u maken van een gebruiker handmatig wilt, neem dan contact op met [Elium ondersteuningsteam](mailto:support@elium.com).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Elium.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen Elium, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Elium**.

    ![De koppeling Elium in de lijst met toepassingen](./media/active-directory-saas-elium-tutorial/tutorial_elium_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Elium in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Elium.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-elium-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-elium-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-elium-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-elium-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-elium-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-elium-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-elium-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-elium-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-elium-tutorial/tutorial_general_203.png

