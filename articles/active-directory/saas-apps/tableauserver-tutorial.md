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
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2018
ms.author: jeedes
ms.openlocfilehash: 84ea1d999a26ce0ce1d548da92549c6a718d5978
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52850360"
---
# <a name="tutorial-azure-active-directory-integration-with-tableau-server"></a>Zelfstudie: Azure Active Directory-integratie met Tableau Server

In deze zelfstudie leert u hoe u Tableau Server integreren met Azure Active Directory (Azure AD).

Tableau Server integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Tableau Server heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Tableau Server (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Tableau Server, moet u de volgende items:

- Een Azure AD-abonnement
- Een Tableau Server eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
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
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **Tableau Server**, selecteer **Tableau Server** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Tableau Server in de lijst met resultaten](./media/tableauserver-tutorial/tutorial-tableauserver-addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Tableau Server op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Tableau Server is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Tableau Server tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Tableau Server, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een testgebruiker Tableau Server](#creating-a-tableau-server-test-user)**  : als u wilt een equivalent van Britta Simon in Tableau Server die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Tableau Server.

**Voor het configureren van Azure AD eenmalige aanmelding met Tableau Server, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Tableau Server** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster, klikt u op **Selecteer** voor **SAML** modus voor eenmalige aanmelding inschakelen.

    ![Eenmalige aanmelding configureren](common/tutorial-general-301.png)

3. Tableau Server-toepassing wordt verwacht dat een aangepaste claim **gebruikersnaam** die moet worden gedefinieerd als hieronder. Dit wordt gebruikt als gebruikers-id in plaats van unieke gebruikers-id claim. U kunt de waarden van deze kenmerken vanuit beheren de **gebruikerskenmerken en Claims** sectie op de pagina van de toepassing-integratie. Klik op **bewerken** te openen **gebruikerskenmerken en Claims** dialoogvenster.

    ![image](./media/tableauserver-tutorial/tutorial-tableauserver-attribute.png)

4. In de **gebruikersclaims** sectie op de **gebruikerskenmerken en Claims** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de bovenstaande afbeelding en voer de volgende stappen uit:
    
    | Naam kenmerk | Waarde kenmerk |
    | ---------------| --------------- |    
    | gebruikersnaam | User.userPrincipalName |

    a. Klik op **toevoegen nieuwe claim** openen de **gebruikersclaims beheren** dialoogvenster.

    ![image](./media/tableauserver-tutorial/tutorial-tableauserver-add-attribute.png)

    ![image](./media/tableauserver-tutorial/tutorial-tableauserver-manage-attribute.png)

    b. In de **naam** tekstvak typt u de naam van het kenmerk wordt weergegeven voor die rij.

    c. Voer de **Namespace** waarde.

    d. Selecteer de bron als **kenmerk**.

    e. Uit de **bronkenmerk** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    f. Klik op **Opslaan**.

5. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** pictogram openen **SAML-basisconfiguratie** dialoogvenster.

    ![Eenmalige aanmelding configureren](common/editconfigure.png)

6. Op de **SAML-basisconfiguratie** sectie, voert u de volgende stappen uit:

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://azure.<domain name>.link`
    
    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://azure.<domain name>.link`

    c. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://azure.<domain name>.link/wg/saml/SSO/index.html`

    ![image](./media/tableauserver-tutorial/tutorial-tableauserver-url.png)
     
    > [!NOTE] 
    > De bovenstaande waarden zijn niet echt waarden. Werk de waarden bij met de werkelijke URL en de id van de configuratiepagina Tableau Server, die later in de zelfstudie wordt uitgelegd.

7. Op de **SAML-handtekeningcertificaat** pagina, in de **SAML-handtekeningcertificaat** sectie, klikt u op **downloaden** downloaden **voorfederatievemetagegevens-XML** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/tableauserver-tutorial/tutorial-tableauserver-certificate.png) 

8. Als u eenmalige aanmelding configureren voor uw toepassing, moet u aan te melden bij uw tenant Tableau Server als beheerder.

9. Op de **Tableau Server Configuration** pagina, voert u de volgende stappen uit:
   
    ![Eenmalige aanmelding configureren](./media/tableauserver-tutorial/tutorial-tableauserver-001.png)

    a. Klik in de configuratie Tableau Server op de **SAML** tabblad. 
  
    b. Schakel het selectievakje in van **SAML gebruiken voor eenmalige aanmelding**.
   
    c. Tableau Server URL geretourneerd: de URL die Tableau Server-gebruikers toegang, zoals tot krijgen http://tableau_server. Met behulp van http://localhost wordt niet aanbevolen. Met behulp van een URL met een slash (bijvoorbeeld http://tableau_server/) wordt niet ondersteund. Kopie **Tableau Server return URL** en plak deze naar Azure AD **aanmelding URL** -tekstvak in **Tableau serverdomein en URL's** sectie.
   
    d. SAML-entiteit-ID, de entiteit-ID is uniek voor uw Tableau Server-installatie voor de id-provider. Hier geeft u de URL van uw Tableau Server opnieuw, indien gewenst, maar dit hoeft niet te worden van de URL van uw Tableau Server. Kopie **SAML entiteit-ID** en plak deze naar Azure AD **id** -tekstvak in **Tableau serverdomein en URL's** sectie.
     
    e. Klik op de **metagegevensbestand exporteren** en opent u het in de toepassing van de editor voor tekst. Ga naar de URL van de Bevestigingsconsumerservice met Http Post en Index 0 en kopieer de URL. Plak nu naar Azure AD **antwoord-URL** -tekstvak in **Tableau serverdomein en URL's** sectie.
   
    f. Ga naar uw Federatiemetagegevens van bestand gedownload vanuit Azure portal en upload het in de **Idp SAML-metagegevensbestand**.
   
    g. Klik op de **OK** knop in de configuratiepagina van Tableau Server.
   
    >[!NOTE] 
    >Klant hebben om een certificaat in de Tableau Server SAML SSO-configuratie te uploaden en deze in de stroom SSO ophalen genegeerd.
    >Als u nodig hebt helpen SAML op Tableau Server configureren, Raadpleeg dit artikel [configureren SAML](https://onlinehelp.tableau.com/current/server/en-us/config_saml.htm).

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

1. Selecteer in de Azure portal, in het linkerdeelvenster **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.

    ![Azure AD-gebruiker maken][100]

2. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm.

    ![Het maken van een Azure AD-testgebruiker](common/create-aaduser-01.png) 

3. In de eigenschappen van de gebruiker de volgende stappen uitvoeren.

    ![Het maken van een Azure AD-testgebruiker](common/create-aaduser-02.png)

    a. In de **naam** veld **BrittaSimon**.
  
    b. In de **gebruikersnaam** veld, typt u **brittasimon@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Selecteer **eigenschappen**, selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in het wachtwoord.

    d. Selecteer **Maken**.
  
### <a name="creating-a-tableau-server-test-user"></a>Het maken van een testgebruiker Tableau Server

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in Tableau Server. U moet alle gebruikers in de Tableau server inrichten. 

Die gebruikersnaam van de gebruiker moet overeenkomen met de waarde die u hebt geconfigureerd in de aangepaste Azure AD-kenmerk van **gebruikersnaam**. Met de juiste toewijzing van de integratie moet werken [Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on).

>[!NOTE]
>Als u een gebruiker handmatig hebt gemaakt wilt, moet u contact op met de beheerder Tableau Server in uw organisatie.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Tableau Server.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **Tableau Server**.

    ![Eenmalige aanmelding configureren](./media/tableauserver-tutorial/tutorial-tableauserver-app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. In de **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst met gebruikers, en klik op de **Selecteer** knop aan de onderkant van het scherm.

6. In de **toevoegen toewijzing** dialoogvenster, selecteer de **toewijzen** knop.

### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

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
