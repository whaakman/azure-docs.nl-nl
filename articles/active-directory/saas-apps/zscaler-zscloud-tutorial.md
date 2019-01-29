---
title: 'Zelfstudie: Azure Active Directory-integratie met Zscaler ZSCloud | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Zscaler ZSCloud.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 411d5684-a780-410a-9383-59f92cf569b5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/10/2018
ms.author: jeedes
ms.openlocfilehash: c3eb96c15991c3b470164bf504c68fbc266e69a8
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55193408"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-zscloud"></a>Zelfstudie: Azure Active Directory-integratie met Zscaler ZSCloud

In deze zelfstudie leert u hoe u Zscaler ZSCloud integreren met Azure Active Directory (Azure AD).

Zscaler ZSCloud integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Zscaler ZSCloud heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Zscaler ZSCloud (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Zscaler ZSCloud, moet u de volgende items:

- Een Azure AD-abonnement
- Een Zscaler ZSCloud eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Zscaler ZSCloud uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-zscaler-zscloud-from-the-gallery"></a>Zscaler ZSCloud uit de galerie toe te voegen

Voor het configureren van de integratie van Zscaler ZSCloud in Azure AD, moet u Zscaler ZSCloud uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Zscaler ZSCloud uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

4. Typ in het zoekvak **Zscaler ZSCloud**, selecteer **Zscaler ZSCloud** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Zscaler ZSCloud in de lijst met resultaten](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie configureert u en test Azure AD eenmalige aanmelding met Zscaler ZSCloud op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Zscaler ZSCloud is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Zscaler ZSCloud tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Zscaler ZSCloud, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een testgebruiker Zscaler ZSCloud](#creating-a-zscaler-zscloud-test-user)**  : als u wilt een equivalent van Britta Simon in Zscaler ZSCloud die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Zscaler ZSCloud.

**Voor het configureren van Azure AD eenmalige aanmelding met Zscaler ZSCloud, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Zscaler ZSCloud** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

2. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster, klikt u op **Selecteer** voor **SAML** modus voor eenmalige aanmelding inschakelen.

    ![Eenmalige aanmelding configureren](common/tutorial_general_301.png)

3. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** pictogram openen **SAML-basisconfiguratie** dialoogvenster.

    ![Eenmalige aanmelding configureren](common/editconfigure.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Zscaler ZSCloud domein en URL's, eenmalige aanmelding informatie](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_url.png)

    In de **aanmeldings-URL** tekstvak, typ de URL moet worden gebruikt door uw gebruikers aan te melden bij uw toepassing ZScaler ZSCloud.

    > [!NOTE] 
    > U moet deze waarde bijwerken met de werkelijke aanmeldings-URL. Neem contact op met [Zscaler twee Client ondersteuningsteam](https://help.zscaler.com/) om deze waarden te verkrijgen.

5. Zscaler ZSCloud toepassing verwacht het SAML-asserties ondertekend in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit de sectie **Gebruikerskenmerken en claims** op de integratiepagina van de toepassing-beheren. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op de knop **Bewerken** om het dialoogvenster **Gebruikerskenmerken en claims** te openen.

    ![Koppeling Kenmerk](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_attribute.png)

6. In de sectie **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** configureert u het kenmerk van het SAML-token zoals wordt weergegeven in de bovenstaande afbeelding en voert u de volgende stappen uit:

    | Name  | Bronkenmerk  |
    | ---------| ------------ |
    | MemberOf     | user.assignedroles |

    a. Klik op **Nieuwe claim toevoegen** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    ![image](./common/new_save_attribute.png)
    
    ![image](./common/new_attribute_details.png)

    b. Selecteer de kenmerkwaarde in de lijst **Bronkenmerken**.

    c. Klik op **OK**.

    d. Klik op **Opslaan**.

    > [!NOTE]
    > Klik [hier](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-app-role-management) als u wilt weten hoe u rollen in Azure AD moet configureren

7. Op de **SAML-handtekeningcertificaat** pagina, in de **SAML-handtekeningcertificaat** sectie, klikt u op **downloaden** downloaden **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![De link om het certificaat te downloaden](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_certificate.png) 

8. Op de **Zscaler ZSCloud instellen** sectie, kopieert u de juiste URL aan de hand van uw behoeften.

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

    ![Zscaler ZSCloud Configuration](common/configuresection.png)

9. In een ander browservenster aanmelden bij uw bedrijf Zscaler ZSCloud site als beheerder.

10. Ga naar **Beheer > Verificatie > Verificatie-instellingen** en voer de volgende stappen uit:
   
    ![Beheer](./media/zscaler-zscloud-tutorial/ic800206.png "Beheer")

    a. Kies onder Verificatietype de optie **SAML**.

    b. Klik op **SAML configureren**.

11. Op de **bewerken SAML** venster de volgende stappen uitvoeren: en klik op opslaan.  
            
    ![Manage Users & Authentication](./media/zscaler-zscloud-tutorial/ic800208.png "Manage Users & Authentication")
    
    a. Plak in het tekstvak **SAML Portal URL** de **aanmeldings-URL** die u in de Azure-portal hebt gekopieerd.

    b. Voer in het tekstvak **Login Name Attribute** **NameID** in.

    c. Klik op **Uploaden** om het Azure SAML-ondertekeningscertificaat te uploaden dat u in de Azure-portal in het **openbare SSL-certificaat** hebt gedownload.

    d. Schakel **Enable SAML Auto-Provisioning** in.

    e. Voer in het tekstvak **User Display Name Attribute** **displayName** in als u automatische inrichting van SAML wilt inschakelen voor displayName-kenmerken.

    f. Voer in het tekstvak **Group Name Attribute** **memberOf** in als u automatische inrichting van SAML wilt inschakelen voor memberOf-kenmerken.

    g. Voer in het tekstvak **Department Name Attribute** **department** in als u automatische inrichting van SAML wilt inschakelen voor department-kenmerken.

    i. Klik op **Opslaan**.

12. Voer in het dialoogvenster **Configure User Authentication** de volgende stappen uit:

    ![Beheer](./media/zscaler-zscloud-tutorial/ic800207.png)

    a. Beweeg de muisaanwijzer boven het menu **Activering** linksonder.

    b. Klik op **Activeren**.

## <a name="configuring-proxy-settings"></a>Proxy-instellingen configureren
### <a name="to-configure-the-proxy-settings-in-internet-explorer"></a>De proxy-instellingen in Internet Explorer configureren

1. Start **Internet Explorer**.

1. Selecteer **Internetopties** uit de **extra** menu voor open de **Internetopties** dialoogvenster.   
    
     ![Internetopties](./media/zscaler-zscloud-tutorial/ic769492.png "Internetopties")

1. Klik op de **verbindingen** tabblad.   
  
     ![Verbindingen](./media/zscaler-zscloud-tutorial/ic769493.png "verbindingen")

1. Klik op **LAN-instellingen** openen de **LAN-instellingen** dialoogvenster.

1. In het gedeelte van de server Proxy de volgende stappen uitvoeren:   
   
    ![Proxyserver](./media/zscaler-zscloud-tutorial/ic769494.png "proxyserver")

    a. Selecteer **een proxyserver gebruiken voor uw LAN**.

    b. Typ in het tekstvak adres **gateway. Zscaler ZSCloud.net**.

    c. Typ in het tekstvak poort **80**.

    d. Selecteer **proxyserver niet gebruiken voor lokale adressen**.

    e. Klik op **OK** sluiten de **Local Area Network (LAN)-instellingen** dialoogvenster.

1. Klik op **OK** sluiten de **Internetopties** dialoogvenster.

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

### <a name="creating-a-zscaler-zscloud-test-user"></a>Het maken van een testgebruiker Zscaler ZSCloud

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in Zscaler ZSCloud. Zscaler ZSCloud biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot Zscaler ZSCloud als deze nog niet bestaat.
>[!Note]
>Als u maken van een gebruiker handmatig wilt, neem dan contact op met [Zscaler ZSCloud ondersteuningsteam](https://help.zscaler.com/).

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Zscaler ZSCloud.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **Zscaler ZSCloud**.

    ![Eenmalige aanmelding configureren](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_app.png)

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202]

4. Klik op **toevoegen** knop en selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. In de **gebruikers en groepen** dialoogvenster, selecteer de gebruiker, zoals **Britta Simon** in de lijst, klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.

    ![image](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_users.png)

6. Uit de **rol selecteren** dialoogvenster kiest u de desbetreffende gebruikersrol in de lijst en klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.

    ![image](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_roles.png)

7. In de **toevoegen toewijzing** dialoogvenster Selecteer de **toewijzen** knop.

    ![image](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_assign.png)

### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Zscaler ZSCloud in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Zscaler ZSCloud.
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
