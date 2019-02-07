---
title: 'Zelfstudie: Azure Active Directory-integratie met Tableau Server | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Tableau Server.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: c1917375-08aa-445c-a444-e22e23fa19e0
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/12/2018
ms.author: jeedes
ms.openlocfilehash: 69ae32f63005d3d5ea83dce5aa349bb6c0c92e0c
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55818427"
---
# <a name="tutorial-azure-active-directory-integration-with-tableau-server"></a>Zelfstudie: Azure Active Directory-integratie met Tableau Server

In deze zelfstudie leert u hoe u Tableau Server integreren met Azure Active Directory (Azure AD).

Tableau Server integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Tableau Server heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Tableau Server (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Tableau Server, moet u de volgende items:

- Een Azure AD-abonnement
- Een Tableau Server eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Tableau Server uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-tableau-server-from-the-gallery"></a>Tableau Server uit de galerie toevoegen

Voor het configureren van de integratie van Tableau Server in Azure AD, moet u Tableau Server uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Tableau Server uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

4. Typ in het zoekvak **Tableau Server**, selecteer **Tableau Server** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Tableau Server in de lijst met resultaten](./media/tableauserver-tutorial/tutorial-tableauserver-addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Tableau Server op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Tableau Server is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Tableau Server tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Tableau Server, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Tableau Server Single Sign-On configureren](#configure-tableau-server-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[Tableau Server testgebruiker maken](#create-tableau-server-test-user)**  : als u wilt een equivalent van Britta Simon in Cisco-familie vallen dat is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
5. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Tableau Server.

**Voor het configureren van Azure AD eenmalige aanmelding met Tableau Server, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Tableau Server** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

2. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster, klikt u op **Selecteer** voor **SAML** modus voor eenmalige aanmelding inschakelen.

    ![Eenmalige aanmelding configureren](common/tutorial-general-301.png)

3. Tableau Server-toepassing wordt verwacht dat een aangepaste claim **gebruikersnaam** die moet worden gedefinieerd als hieronder. Dit wordt gebruikt als gebruikers-id in plaats van unieke gebruikers-id claim. U kunt de waarden van deze kenmerken vanuit de sectie **Gebruikerskenmerken en claims** op de integratiepagina van de toepassing-beheren. Klik op **bewerken** te openen **gebruikerskenmerken en Claims** dialoogvenster.

    ![image](./media/tableauserver-tutorial/tutorial-tableauserver-attribute.png)

4. In de **gebruikersclaims** sectie op de **gebruikerskenmerken en Claims** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de bovenstaande afbeelding en voer de volgende stappen uit:
    
    | Naam kenmerk | Waarde kenmerk | Naamruimte |
    | ---------------| --------------- | ----------- |   
    | gebruikersnaam | user.userprincipalname | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims` |

    a. Klik op **Nieuwe claim toevoegen** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    ![image](./media/tableauserver-tutorial/tutorial-tableauserver-add-attribute.png)

    ![image](./media/tableauserver-tutorial/tutorial-tableauserver-manage-attribute.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Voer de **Namespace** waarde.

    d. Selecteer Bron bij **Kenmerk**.

    e. Typ de kenmerkwaarde voor die rij in de lijst met **bronkenmerken**.

    f. Klik op **Opslaan**.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Eenmalige aanmelding configureren](common/editconfigure.png)

6. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://azure.<domain name>.link`
    
    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://azure.<domain name>.link`

    c. In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://azure.<domain name>.link/wg/saml/SSO/index.html`

    ![image](./media/tableauserver-tutorial/tutorial-tableauserver-url.png)
     
    > [!NOTE] 
    > De bovenstaande waarden zijn niet echt waarden. Werk de waarden bij met de werkelijke URL en de id van de configuratiepagina Tableau Server, die later in de zelfstudie wordt uitgelegd.

7. Op de **SAML-handtekeningcertificaat** pagina, in de **SAML-handtekeningcertificaat** sectie, klikt u op **downloaden** downloaden **voorfederatievemetagegevens-XML** en sla het certificaatbestand op uw computer.

    ![De link om het certificaat te downloaden](./media/tableauserver-tutorial/tutorial-tableauserver-certificate.png)

### <a name="configure-tableau-server-single-sign-on"></a>Tableau Server eenmalige aanmelding configureren 

1. Als u eenmalige aanmelding configureren voor uw toepassing, moet u aan te melden bij uw tenant Tableau Server als beheerder.

2. Op de **configuratie** tabblad **gebruiker Identity & Access**, en selecteer vervolgens de **verificatie** methode tabblad.

    ![Eenmalige aanmelding configureren](./media/tableauserver-tutorial/tutorial-tableauserver-auth.png)

3. Op de **configuratie** pagina, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/tableauserver-tutorial/tutorial-tableauserver-config.png)

    a. Voor **verificatiemethode**, SAML selecteren.
    
    b. Schakel het selectievakje in van **SAML-verificatie inschakelen voor de server**.

    c. Tableau Server URL geretourneerd: de URL die Tableau Server-gebruikers toegang, zoals tot krijgen http://tableau_server. Met behulp van http://localhost wordt niet aanbevolen. Met behulp van een URL met een slash (bijvoorbeeld http://tableau_server/) wordt niet ondersteund. Kopie **Tableau Server return URL** en plak deze naar Azure AD **aanmelding URL** -tekstvak in **Tableau serverdomein en URL's** sectie.

    d. SAML-entiteit-ID, de entiteit-ID is uniek voor uw Tableau Server-installatie voor de id-provider. Hier geeft u de URL van uw Tableau Server opnieuw, indien gewenst, maar dit hoeft niet te worden van de URL van uw Tableau Server. Kopie **SAML entiteit-ID** en plak deze naar Azure AD **id** -tekstvak in **Tableau serverdomein en URL's** sectie.

    e. Klik op de **downloaden XML-bestand met metagegevens** en opent u het in de toepassing van de editor voor tekst. Ga naar de URL van de Bevestigingsconsumerservice met Http Post en Index 0 en kopieer de URL. Plak nu naar Azure AD **antwoord-URL** -tekstvak in **Tableau serverdomein en URL's** sectie.

    f. Ga naar uw Federatiemetagegevens van bestand gedownload vanuit Azure portal en upload het in de **Idp SAML-metagegevensbestand**.

    g. Geef de naam voor de kenmerken die gebruikmaakt van de IdP houdt de gebruikersnamen, weergavenamen en e-mailadressen.

    h. Klik op **Opslaan**.

    >[!NOTE] 
    >Klant hebben om een certificaat in de Tableau Server SAML SSO-configuratie te uploaden en deze in de stroom SSO ophalen genegeerd.
    >Als u nodig hebt helpen SAML op Tableau Server configureren, Raadpleeg dit artikel [configureren SAML](https://onlinehelp.tableau.com/v2018.2/server/en-us/saml_config_steps_tsm_ui.htm).

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![Azure AD-gebruiker maken][100]

2. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm.

    ![Het maken van een Azure AD-testgebruiker](common/create-aaduser-01.png) 

3. In de eigenschappen van de gebruiker de volgende stappen uitvoeren.

    ![Het maken van een Azure AD-testgebruiker](common/create-aaduser-02.png)

    a. Voer in het veld **Naam****Britta Simon** in.
  
    b. In het veld **Gebruikersnaam** typt u **brittasimon@yourcompanydomain.extension**.  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Selecteer **eigenschappen**, selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in het wachtwoord.

    d. Selecteer **Maken**.
  
### <a name="create-tableau-server-test-user"></a>Tableau Server testgebruiker maken

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in Tableau Server. U moet alle gebruikers in de Tableau server inrichten. 

Die gebruikersnaam van de gebruiker moet overeenkomen met de waarde die u hebt geconfigureerd in de aangepaste Azure AD-kenmerk van **gebruikersnaam**. Met de juiste toewijzing moet de integratie van Azure AD configureren Single Sign-On werken.

>[!NOTE]
>Als u een gebruiker handmatig hebt gemaakt wilt, moet u contact op met de beheerder Tableau Server in uw organisatie.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Tableau Server.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **Tableau Server**.

    ![Eenmalige aanmelding configureren](./media/tableauserver-tutorial/tutorial-tableauserver-app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. In de **toevoegen toewijzing** dialoogvenster, selecteer de **toewijzen** knop.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Tableau Server in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Tableau Server.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: common/tutorial-general-01.png
[2]: common/tutorial-general-02.png
[3]: common/tutorial-general-03.png
[4]: common/tutorial-general-04.png

[100]: common/tutorial-general-100.png

[201]: common/tutorial-general-201.png
[202]: common/tutorial-general-202.png
[203]: common/tutorial-general-203.png
