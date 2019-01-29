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
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/05/2018
ms.author: jeedes
ms.openlocfilehash: ac88d3accad8aa9ec523ecce5eb11b6c10eb913c
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55181483"
---
# <a name="tutorial-azure-active-directory-integration-with-elium"></a>Zelfstudie: Azure Active Directory-integratie met Elium

In deze zelfstudie leert u hoe u Elium integreren met Azure Active Directory (Azure AD).

Elium integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Elium heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Elium (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Elium, moet u de volgende items:

- Een Azure AD-abonnement
- Een Elium eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Elium uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-elium-from-the-gallery"></a>Elium uit de galerie toe te voegen
Voor het configureren van de integratie van Elium in Azure AD, moet u Elium uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Elium uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **Elium**, selecteer **Elium** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Elium in de lijst met resultaten](./media/elium-tutorial/tutorial_elium_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Elium op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Elium is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Elium tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Elium, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker Elium](#create-an-elium-test-user)**  : als u wilt een equivalent van Britta Simon in Elium die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Elium.

**Voor het configureren van Azure AD eenmalige aanmelding met Elium, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Elium** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/elium-tutorial/tutorial_elium_samlbase.png)

1. Op de **Elium domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Elium domein en URL's, eenmalige aanmelding informatie](./media/elium-tutorial/tutorial_elium_url.png)

    a. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://<platform-domain>.elium.com/login/saml2/metadata`

    b. In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://<platform-domain>.elium.com/login/saml2/acs`

1. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Elium domein en URL's, eenmalige aanmelding informatie](./media/elium-tutorial/tutorial_elium_url1.png)

    Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: ` https://<platform-domain>.elium.com/login/saml2/login`
     
    > [!NOTE] 
    > Dit zijn geen echte waarden. U krijgt deze waarden uit de **SP-bestand met metagegevens** downloadbare op `https://<platform-domain>.elium.com/login/saml2/metadata`, die verderop in deze zelfstudie wordt uitgelegd.

1. De toepassing Elium wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit beheren de "**gebruikerskenmerken**" sectie op de pagina van de toepassing-integratie.

    ![Eenmalige aanmelding configureren](./media/elium-tutorial/tutorial_attribute.png)

1. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de voorgaande afbeelding en voer de volgende stappen uit:
           
    | Naam kenmerk | Waarde kenmerk |   
    | ---------------| ----------------|
    | e-mail   |user.mail |
    | first_name| user.givenname |
    | last_name| user.surname|
    | job_title| user.jobtitle|
    | Bedrijf| User.CompanyName|
    
    > [!NOTE]
    > Dit zijn de standaard-claims. **Alleen e-claim vereist is**. Claim is JIT inrichting ook alleen e-mailadres verplicht. Andere aangepaste claims kunnen variëren van één klant platform naar een andere klant-platform.

    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/elium-tutorial/tutorial_attribute_04.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    ![Eenmalige aanmelding configureren](./media/elium-tutorial/tutorial_attribute_05.png)

    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. Naamruimte leeg laten.
    
    e. Klik op **OK**. 

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/elium-tutorial/tutorial_elium_certificate.png) 

1. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/elium-tutorial/tutorial_general_400.png)
    
1. In een ander browservenster aanmelden bij uw bedrijf Elium site als beheerder.

1. Klik op de **gebruikersprofiel** in de rechterbovenhoek en selecteer vervolgens **beheer**.

    ![Eenmalige aanmelding configureren](./media/elium-tutorial/user1.png)

1. Selecteer het tabblad **Security**.

    ![Eenmalige aanmelding configureren](./media/elium-tutorial/user2.png)

1. Schuif omlaag naar de **eenmalige aanmelding (SSO)** sectie en voer de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/elium-tutorial/user3.png)

    a. Kopieer de waarde van **controleren die SAML2-verificatie werkt voor uw account** en plak deze in de **aanmeldings-URL** tekstvak op de **Elium domein en URL's** sectie in Azure Portal.

    > [!NOTE]
    > Na het configureren van eenmalige aanmelding, kunt u altijd toegang tot de standaardpagina van de externe aanmelding via de volgende URL: `https://<platform_domain>/login/regular/login` 

    b. Selecteer **SAML2 inschakelen federation** selectievakje.

    c. Selecteer **JIT inrichting** selectievakje.

    d. Open de **SP metagegevens** door te klikken op de **downloaden** knop.

    e. Zoek de **entityID** in de **SP metagegevens** bestand, kopieert u de **entityID** waarde en plak deze in de **id** tekstvak op de  **Elium domein en URL's** sectie in Azure portal. 

    ![Eenmalige aanmelding configureren](./media/elium-tutorial/user4.png)

    f. Zoek de **AssertionConsumerService** in de **SP metagegevens** bestand, kopieert u de **locatie** waarde en plak deze in de **antwoord-URL** tekstvak op de **Elium domein en URL's** sectie in Azure portal.

    ![Eenmalige aanmelding configureren](./media/elium-tutorial/user5.png)

    g. Open het gedownloade metagegevensbestand in Azure portal in Kladblok, Kopieer de inhoud en plak deze in de **IdP metagegevens** tekstvak.

    h. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/elium-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/elium-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/elium-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/elium-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-an-elium-test-user"></a>Maak een testgebruiker Elium

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in Elium. Elium biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot Elium als deze nog niet bestaat.
>[!Note]
>Als u maken van een gebruiker handmatig wilt, neem dan contact op met [Elium ondersteuningsteam](mailto:support@elium.com).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Elium.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan Elium toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Elium**.

    ![De koppeling Elium in de lijst met toepassingen](./media/elium-tutorial/tutorial_elium_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Elium in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Elium.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/elium-tutorial/tutorial_general_01.png
[2]: ./media/elium-tutorial/tutorial_general_02.png
[3]: ./media/elium-tutorial/tutorial_general_03.png
[4]: ./media/elium-tutorial/tutorial_general_04.png

[100]: ./media/elium-tutorial/tutorial_general_100.png

[200]: ./media/elium-tutorial/tutorial_general_200.png
[201]: ./media/elium-tutorial/tutorial_general_201.png
[202]: ./media/elium-tutorial/tutorial_general_202.png
[203]: ./media/elium-tutorial/tutorial_general_203.png

