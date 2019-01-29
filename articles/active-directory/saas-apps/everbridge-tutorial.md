---
title: 'Zelfstudie: Azure Active Directory-integratie met EverBridge | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en EverBridge.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 58d7cd22-98c0-4606-9ce5-8bdb22ee8b3e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2018
ms.author: jeedes
ms.openlocfilehash: 40645db589409ac80c69f1e7595e20869b08f5d0
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55194675"
---
# <a name="tutorial-azure-active-directory-integration-with-everbridge"></a>Zelfstudie: Azure Active Directory-integratie met EverBridge

In deze zelfstudie leert u hoe u EverBridge integreren met Azure Active Directory (Azure AD).

EverBridge integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot EverBridge heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij EverBridge (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met EverBridge, moet u de volgende items:

- Een Azure AD-abonnement
- Een EverBridge eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. EverBridge uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-everbridge-from-the-gallery"></a>EverBridge uit de galerie toe te voegen

Voor het configureren van de integratie van EverBridge in Azure AD, moet u EverBridge uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen EverBridge uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

4. Typ in het zoekvak **EverBridge**, selecteer **EverBridge** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![EverBridge in de lijst met resultaten](./media/everbridge-tutorial/tutorial_everbridge_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met EverBridge op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in EverBridge is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in EverBridge tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met EverBridge, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een testgebruiker EverBridge](#creating-an-everbridge-test-user)**  : als u wilt een equivalent van Britta Simon in EverBridge die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing EverBridge.

**Voor het configureren van Azure AD eenmalige aanmelding met EverBridge, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **EverBridge** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

2. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster, klikt u op **Selecteer** voor **SAML** modus voor eenmalige aanmelding inschakelen.

    ![Eenmalige aanmelding configureren](common/tutorial_general_301.png)

3. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** pictogram openen **SAML-basisconfiguratie** dialoogvenster.

    ![Eenmalige aanmelding configureren](common/editconfigure.png)

    >[!NOTE]
    >U moet de configuraties van de toepassing als de Manager-Portal of als de lid-Portal aan beide uiteinden dat wil zeggen op Azure-Portal en Everbridge Portal doen.

4. Het configureren van de **EverBridge** toepassing als **EverBridge Manager Portal**op de **SAML-basisconfiguratie** sectie de volgende stappen uitvoeren:

    ![EverBridge domein en URL's, eenmalige aanmelding informatie](./media/everbridge-tutorial/tutorial_everbridge_url.png)

    a. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://sso.everbridge.net/<API_Name>`

    b. In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://manager.everbridge.net/saml/SSO/<API_Name>/alias/defaultAlias`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke id en antwoord-URL. Neem contact op met [EverBridge ondersteuningsteam](mailto:support@everbridge.com) om deze waarden te verkrijgen.

5. Het configureren van de **EverBridge** toepassing als **EverBridge lid Portal**op de **SAML-basisconfiguratie** sectie, voert u de volgende stappen uit:

    * Als u wilt configureren van de toepassing in **IDP** modus gestart:

        ![EverBridge domein en URL's, eenmalige aanmelding informatie](./media/everbridge-tutorial/tutorial_everbridge_url1.png)

        * Typ in het tekstvak **Id** een URL met het volgende patroon: `https://sso.everbridge.net/<API_Name>/<Organization_ID>`

        * In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://member.everbridge.net/saml/SSO/<API_Name>/<Organization_ID>/alias/defaultAlias`

    * Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

        ![EverBridge domein en URL's, eenmalige aanmelding informatie](./media/everbridge-tutorial/tutorial_everbridge_url2.png)

        * Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://member.everbridge.net/saml/login/<API_Name>/<Organization_ID>/alias/defaultAlias?disco=true`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden met de werkelijke-id, de antwoord-URL en aanmeldings-URL. Neem contact op met [EverBridge ondersteuningsteam](mailto:support@everbridge.com) om deze waarden te verkrijgen.

6. Op de **SAML-handtekeningcertificaat** pagina, in de **SAML-handtekeningcertificaat** sectie, klikt u op **downloaden** downloaden **voorfederatievemetagegevens-XML** en sla het bestand met metagegevens op uw computer.

    ![De link om het certificaat te downloaden](./media/everbridge-tutorial/tutorial_everbridge_certificate.png) 

7. Op de **EverBridge instellen** sectie, kopieert u de juiste URL aan de hand van uw behoeften.

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

    ![EverBridge configuratie](common/configuresection.png)

8. Ophalen van geconfigureerd voor eenmalige aanmelding **EverBridge** als **EverBridge Manager Portal** toepassing, de volgende stappen uitvoeren: 
 
9. In een ander browservenster, meld u aan bij EverBridge als beheerder.

9. Klik in het menu bovenaan op de **instellingen** tabblad en selecteer **Single Sign-On** onder **Security**.
   
    ![Eenmalige aanmelding configureren](./media/everbridge-tutorial/tutorial_everbridge_002.png)
   
    a. In de **naam** tekstvak typt u de naam van de id-Provider (bijvoorbeeld: naam van uw bedrijf).
   
    b. In de **API-naam** tekstvak typt u de naam van de API.
   
    c. Klik op **bestand kiezen** knop voor het uploaden van het bestand met metagegevens die u hebt gedownload van Azure portal.
   
    d. Selecteer in de locatie van de identiteit SAML **identiteit is in de NameIdentifier-element van het onderwerp overzicht**.
   
    e. In de **aanmeldings-URL van id-Provider** tekstvak, plak de waarde van **aanmeldings-URL** die u hebt gekopieerd vanuit Azure portal.
   
    f. Selecteer in de Service Provider gestart aanvragen Binding, **HTTP-omleiding**.

    g. Klik op **Opslaan**.

10. Het configureren van eenmalige aanmelding op **EverBridge** toepassing als **EverBridge lid Portal**, moet u voor het verzenden van de gedownloade **federatieve metagegevens-XML** naar [ Het ondersteuningsteam Everbridge](mailto:support@everbridge.com). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![Azure AD-gebruiker maken][100]

2. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm.

    ![Het maken van een Azure AD-testgebruiker](common/create_aaduser_01.png) 

3. In de eigenschappen van de gebruiker de volgende stappen uitvoeren.

    ![Het maken van een Azure AD-testgebruiker](common/create_aaduser_02.png)

    a. Voer in het veld **Naam****Britta Simon** in.
  
    b. In het veld **Gebruikersnaam** typt u **brittasimon@yourcompanydomain.extension**.  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Selecteer **eigenschappen**, selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in het wachtwoord.

    d. Selecteer **Maken**.
  
### <a name="creating-an-everbridge-test-user"></a>Het maken van een testgebruiker EverBridge

In deze sectie maakt u een gebruiker met de naam van Britta Simon in Everbridge. Werken met [EverBridge ondersteuningsteam](mailto:support@everbridge.com) om toe te voegen de gebruikers in het Everbridge-platform.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan EverBridge.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **EverBridge**.

    ![Eenmalige aanmelding configureren](./media/everbridge-tutorial/tutorial_everbridge_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. In de **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst met gebruikers, en klik op de **Selecteer** knop aan de onderkant van het scherm.

6. In de **toevoegen toewijzing** dialoogvenster Selecteer de **toewijzen** knop.

### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel EverBridge in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing EverBridge.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: common/tutorial_general_01.png
[2]: common/tutorial_general_02.png
[3]: common/tutorial_general_03.png
[4]: common/tutorial_general_04.png

[100]: common/tutorial_general_100.png

[201]: common/tutorial_general_201.png
[202]: common/tutorial_general_202.png
[203]: common/tutorial_general_203.png
