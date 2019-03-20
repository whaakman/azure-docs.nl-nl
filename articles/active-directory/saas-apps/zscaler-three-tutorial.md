---
title: 'Zelfstudie: Azure Active Directory-integratie met Zscaler drie | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Zscaler drie.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: f352e00d-68d3-4a77-bb92-717d055da56f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/12/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: eced1b92aba24c8083cbdc2ece099fab95567db7
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57877952"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-three"></a>Zelfstudie: Azure Active Directory-integratie met drie Zscaler

In deze zelfstudie leert u hoe u drie Zscaler integreren met Azure Active Directory (Azure AD).

Drie Zscaler integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot drie Zscaler heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij drie Zscaler (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Zscaler drie, moet u de volgende items:

- Een Azure AD-abonnement
- Een Zscaler drie eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Uit de galerie toe te voegen drie Zscaler
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-zscaler-three-from-the-gallery"></a>Uit de galerie toe te voegen drie Zscaler

Voor het configureren van de integratie van drie Zscaler in Azure AD, moet u drie Zscaler toevoegen uit de galerie aan de lijst met beheerde SaaS-apps.

**Als u wilt Zscaler drie uit de galerie toevoegen, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

4. Typ in het zoekvak **Zscaler drie**, selecteer **Zscaler drie** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Drie Zscaler in de lijst met resultaten](./media/zscaler-three-tutorial/tutorial_zscalerthree_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Zscaler drie op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in drie Zscaler is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in drie Zscaler tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Zscaler drie, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureer Zscaler drie Single Sign-On](#configure-zscaler-three-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[Maak drie Zscaler testgebruiker](#create-zscaler-three-test-user)**  : als u wilt een equivalent van Britta Simon in Cisco-familie vallen dat is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
5. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Zscaler drie.

**Voor het configureren van Azure AD eenmalige aanmelding met Zscaler drie, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Zscaler drie** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

2. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster, klikt u op **Selecteer** voor **SAML** modus voor eenmalige aanmelding inschakelen.

    ![Eenmalige aanmelding configureren](common/tutorial_general_301.png)

3. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** pictogram openen **SAML-basisconfiguratie** dialoogvenster.

    ![Eenmalige aanmelding configureren](common/editconfigure.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Zscaler drie domein en URL's, eenmalige aanmelding informatie](./media/zscaler-three-tutorial/tutorial_zscalerthree_url.png)

    Typ een URL in het tekstvak **Aanmeldings-URL**: `https://login.zscalerthree.net/sfc_sso`

5. De SAML-asserties ondertekend verwacht Zscaler drie toepassing in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit de sectie **Gebruikerskenmerken en claims** op de integratiepagina van de toepassing-beheren. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op de knop **Bewerken** om het dialoogvenster **Gebruikerskenmerken en claims** te openen.

    ![Koppeling Kenmerk](./media/zscaler-three-tutorial/tutorial_zscalerthree_attribute.png)

6. In de sectie **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** configureert u het kenmerk van het SAML-token zoals wordt weergegeven in de bovenstaande afbeelding en voert u de volgende stappen uit:

    | Name  | Bronkenmerk  |
    | ---------| ------------ |
    | memberOf     | user.assignedroles |

    a. Klik op **Nieuwe claim toevoegen** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    ![image](./common/new_save_attribute.png)
    
    ![image](./common/new_attribute_details.png)

    b. Selecteer de kenmerkwaarde in de lijst **Bronkenmerken**.

    c. Klik op **OK**.

    d. Klik op **Opslaan**.

    > [!NOTE]
    > Klik [hier](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-app-role-management) als u wilt weten hoe u rollen in Azure AD moet configureren

7. Op de **SAML-handtekeningcertificaat** pagina, in de **SAML-handtekeningcertificaat** sectie, klikt u op **downloaden** downloaden **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![De link om het certificaat te downloaden](./media/zscaler-three-tutorial/tutorial_zscalerthree_certificate.png) 

8. Op de **instellen Zscaler drie** sectie, kopieert u de juiste URL aan de hand van uw behoeften.

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

    ![Configuratie van de drie Zscaler](common/configuresection.png)

### <a name="configure-zscaler-three-single-sign-on"></a>Configureer Zscaler drie Single Sign-On

1. In een ander browservenster aanmelden bij uw Zscaler drie bedrijf site als beheerder.

1. Ga naar **Beheer > Verificatie > Verificatie-instellingen** en voer de volgende stappen uit:
   
   ![Beheer](./media/zscaler-three-tutorial/ic800206.png "Beheer")

   a. Kies onder Verificatietype de optie **SAML**.

   b. Klik op **SAML configureren**.

1. Voer in het venster **SAML bewerken** de volgende stappen uit en klik op Opslaan.  
            
   ![Manage Users & Authentication](./media/zscaler-three-tutorial/ic800208.png "Manage Users & Authentication")
    
   a. Plak in het tekstvak **SAML Portal URL** de **aanmeldings-URL** die u in de Azure-portal hebt gekopieerd.

   b. Voer in het tekstvak **Login Name Attribute** **NameID** in.

   c. Klik op **Uploaden** om het Azure SAML-ondertekeningscertificaat te uploaden dat u in de Azure-portal in het **openbare SSL-certificaat** hebt gedownload.

   d. Schakel **Enable SAML Auto-Provisioning** in.

   e. Voer in het tekstvak **User Display Name Attribute** **displayName** in als u automatische inrichting van SAML wilt inschakelen voor displayName-kenmerken.

   f. Voer in het tekstvak **Group Name Attribute** **memberOf** in als u automatische inrichting van SAML wilt inschakelen voor memberOf-kenmerken.

   g. Voer in het tekstvak **Department Name Attribute** **department** in als u automatische inrichting van SAML wilt inschakelen voor department-kenmerken.

   i. Klik op **Opslaan**.

1. Voer in het dialoogvenster **Configure User Authentication** de volgende stappen uit:

   ![Beheer](./media/zscaler-three-tutorial/ic800207.png)

   a. Beweeg de muisaanwijzer boven het menu **Activering** linksonder.

   b. Klik op **Activeren**.

## <a name="configuring-proxy-settings"></a>Proxyinstellingen configureren
### <a name="to-configure-the-proxy-settings-in-internet-explorer"></a>De proxyinstellingen configureren in Internet Explorer

1. Start **Internet Explorer**.

1. Selecteer **Internetopties** in het menu **Extra** om het dialoogvenster **Internetopties** te openen.   
    
     ![Internetopties](./media/zscaler-three-tutorial/ic769492.png "Internetopties")

1. Klik op het tabblad **Verbindingen**.   
  
     ![Verbindingen](./media/zscaler-three-tutorial/ic769493.png "Verbindingen")

1. Klik op **LAN-instellingen** om het dialoogvenster **LAN-instellingen** te openen.

1. In het gedeelte Proxyserver voert u de volgende stappen uit:   
   
    ![Proxyserver](./media/zscaler-three-tutorial/ic769494.png "Proxyserver")

    a. Selecteer **Een proxyserver voor uw LAN-netwerk gebruiken**.

    b. Typ in het tekstvak adres **gateway. Zscaler Three.net**.

    c. Typ **80** in het tekstvak Poort.

    d. Selecteer **Proxyserver niet voor lokale adressen gebruiken**.

    e. Klik op **OK** om het dialoogvenster **LAN-instellingen** te sluiten.

1. Klik op **OK** om het dialoogvenster **Internetopties** te sluiten.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![Azure AD-gebruiker maken][100]

2. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm.

    ![Het maken van een Azure AD-testgebruiker](common/create_aaduser_01.png) 

3. In de eigenschappen van de gebruiker de volgende stappen uitvoeren.

    ![Het maken van een Azure AD-testgebruiker](common/create_aaduser_02.png)

    a. Voer in het veld **Naam** **Britta Simon** in.
  
    b. In de **gebruikersnaam** veld, typt u **brittasimon\@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Selecteer **eigenschappen**, selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in het wachtwoord.

    d. Selecteer **Maken**.

### <a name="create-zscaler-three-test-user"></a>Drie Zscaler testgebruiker maken

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in drie Zscaler. Drie Zscaler biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot drie Zscaler als deze nog niet bestaat.
>[!Note]
>Als u maken van een gebruiker handmatig wilt, neem dan contact op met [Zscaler drie ondersteuningsteam](https://www.zscaler.com/company/contact).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding toegang verlenen tot drie Zscaler.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **Zscaler drie**.

    ![Eenmalige aanmelding configureren](./media/zscaler-three-tutorial/tutorial_zscalerthree_app.png)

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202]

4. Klik op **toevoegen** knop en selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst en klik op de knop **Selecteren** onder aan het scherm.

    ![image](./media/zscaler-three-tutorial/tutorial_zscalerthree_users.png)

6. In het dialoogvenster **Rol selecteren** kiest u de desbetreffende gebruikersrol in de lijst en klikt u vervolgens op de knop **Selecteren** onder aan het scherm.

    ![image](./media/zscaler-three-tutorial/tutorial_zscalerthree_roles.png)

7. Selecteer in het dialoogvenster **Toewijzing toevoegen** de knop **Toewijzen**.

    ![image](./media/zscaler-three-tutorial/tutorial_zscalerthree_assign.png)

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de drie Zscaler tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw Zscaler drie toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](tutorial-list.md)
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
