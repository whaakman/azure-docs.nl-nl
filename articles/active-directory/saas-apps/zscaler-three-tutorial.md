---
title: 'Zelfstudie: Azure Active Directory-integratie met Zscaler drie | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Zscaler drie.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: f352e00d-68d3-4a77-bb92-717d055da56f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/09/2019
ms.author: jeedes
ms.openlocfilehash: 658c501575de70cc57650931d3167b7194f4bf9d
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/13/2019
ms.locfileid: "59548760"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-three"></a>Zelfstudie: Azure Active Directory-integratie met drie Zscaler

In deze zelfstudie leert u hoe u drie Zscaler integreren met Azure Active Directory (Azure AD).
Drie Zscaler integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot drie Zscaler heeft.
* U kunt uw gebruikers worden automatisch aangemeld drie Zscaler (Single Sign-On) inschakelen met hun Azure AD-accounts.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Zscaler drie, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Drie Zscaler eenmalige aanmelding ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor drie Zscaler **SP** gestart door SSO

* Biedt ondersteuning voor drie Zscaler **Just In Time** inrichten van gebruikers

## <a name="adding-zscaler-three-from-the-gallery"></a>Uit de galerie toe te voegen drie Zscaler

Voor het configureren van de integratie van drie Zscaler in Azure AD, moet u drie Zscaler toevoegen uit de galerie aan de lijst met beheerde SaaS-apps.

**Als u wilt Zscaler drie uit de galerie toevoegen, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **Zscaler drie**, selecteer **Zscaler drie** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![Drie Zscaler in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Zscaler drie op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Zscaler drie tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Zscaler drie, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureer Zscaler drie Single Sign-On](#configure-zscaler-three-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maak drie Zscaler testgebruiker](#create-zscaler-three-test-user)**  : als u wilt een equivalent van Britta Simon in Zscaler drie die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met Zscaler drie, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **Zscaler drie** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Zscaler drie domein en URL's, eenmalige aanmelding informatie](common/sp-intiated.png)

    In het tekstvak **Aanmeldings-URL** typt u een URL: `https://login.zscalerthree.net/sfc_sso`

5. Uw toepassing Zscaler drie wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. In de volgende schermafbeelding wordt de lijst met standaardkenmerken weergegeven. Klik op het pictogram  **Bewerken**  om het dialoogvenster  **Gebruikerskenmerken**  te openen.

    ![image](common/edit-attribute.png)

6. Drie Zscaler toepassing verwacht met bovenstaande bovendien enkele meer kenmerken moeten worden doorgegeven in SAML-antwoord. In de sectie **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** voert u de volgende stappen uit om het kenmerk van het SAML-token toe te voegen zoals wordt weergegeven in de onderstaande tabel:
    
    | Name | Bronkenmerk |
    | ---------| ------------ |
    | memberOf     | user.assignedroles |

    a. Klik op **Nieuwe claim toevoegen** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Laat **Naamruimte** leeg.

    d. Selecteer Bron bij **Kenmerk**.

    e. Typ de kenmerkwaarde voor die rij in de lijst met **bronkenmerken**.
    
    f. Klik op **Opslaan**.

    > [!NOTE]
    > Klik [hier](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-app-role-management) als u wilt weten hoe u rollen in Azure AD moet configureren

7. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

8. Op de **instellen Zscaler drie** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-zscaler-three-single-sign-on"></a>Configureer Zscaler drie Single Sign-On

1. In een ander browservenster aanmelden bij uw Zscaler drie bedrijf site als beheerder.

2. Ga naar **Beheer > Verificatie > Verificatie-instellingen** en voer de volgende stappen uit:
   
    ![Beheer](./media/zscaler-three-tutorial/ic800206.png "Beheer")

    a. Kies onder Verificatietype de optie **SAML**.

    b. Klik op **SAML configureren**.

3. Voer in het venster **SAML bewerken** de volgende stappen uit en klik op Opslaan.  
            
    ![Manage Users & Authentication](./media/zscaler-three-tutorial/ic800208.png "Manage Users & Authentication")
    
    a. Plak in het tekstvak **SAML Portal URL** de **aanmeldings-URL** die u in de Azure-portal hebt gekopieerd.

    b. Voer in het tekstvak **Login Name Attribute** **NameID** in.

    c. Klik op **Uploaden** om het Azure SAML-ondertekeningscertificaat te uploaden dat u in de Azure-portal in het **openbare SSL-certificaat** hebt gedownload.

    d. Schakel **Enable SAML Auto-Provisioning** in.

    e. Voer in het tekstvak **User Display Name Attribute** **displayName** in als u automatische inrichting van SAML wilt inschakelen voor displayName-kenmerken.

    f. Voer in het tekstvak **Group Name Attribute** **memberOf** in als u automatische inrichting van SAML wilt inschakelen voor memberOf-kenmerken.

    g. Voer in het tekstvak **Department Name Attribute** **department** in als u automatische inrichting van SAML wilt inschakelen voor department-kenmerken.

    h. Klik op **Opslaan**.

4. Voer in het dialoogvenster **Configure User Authentication** de volgende stappen uit:

    ![Beheer](./media/zscaler-three-tutorial/ic800207.png)

    a. Beweeg de muisaanwijzer boven het menu **Activering** linksonder.

    b. Klik op **Activeren**.

## <a name="configuring-proxy-settings"></a>Proxyinstellingen configureren
### <a name="to-configure-the-proxy-settings-in-internet-explorer"></a>De proxyinstellingen configureren in Internet Explorer

1. Start **Internet Explorer**.

2. Selecteer **Internetopties** in het menu **Extra** om het dialoogvenster **Internetopties** te openen.   
    
     ![Internetopties](./media/zscaler-three-tutorial/ic769492.png "Internetopties")

3. Klik op het tabblad **Verbindingen**.   
  
     ![Verbindingen](./media/zscaler-three-tutorial/ic769493.png "Verbindingen")

4. Klik op **LAN-instellingen** om het dialoogvenster **LAN-instellingen** te openen.

5. In het gedeelte Proxyserver voert u de volgende stappen uit:   
   
    ![Proxyserver](./media/zscaler-three-tutorial/ic769494.png "Proxyserver")

    a. Selecteer **Een proxyserver voor uw LAN-netwerk gebruiken**.

    b. Typ in het tekstvak adres **gateway. Zscaler Three.net**.

    c. Typ **80** in het tekstvak Poort.

    d. Selecteer **Proxyserver niet voor lokale adressen gebruiken**.

    e. Klik op **OK** om het dialoogvenster **LAN-instellingen** te sluiten.

6. Klik op **OK** om het dialoogvenster **Internetopties** te sluiten.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken 

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In de **gebruikersnaam** veldtype brittasimon@yourcompanydomain.extension. Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding toegang verlenen tot drie Zscaler.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **Zscaler drie**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **Zscaler drie**.

    ![De drie Zscaler koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst en klik op de knop **Selecteren** onder aan het scherm.

    ![image](./media/zscaler-three-tutorial/tutorial_zscalerthree_users.png)

6. In het dialoogvenster **Rol selecteren** kiest u de desbetreffende gebruikersrol in de lijst en klikt u vervolgens op de knop **Selecteren** onder aan het scherm.

    ![image](./media/zscaler-three-tutorial/tutorial_zscalerthree_roles.png)

7. Selecteer in het dialoogvenster **Toewijzing toevoegen** de knop **Toewijzen**.

    ![image](./media/zscaler-three-tutorial/tutorial_zscalerthree_assign.png)

### <a name="create-zscaler-three-test-user"></a>Drie Zscaler testgebruiker maken

In deze sectie wordt een gebruiker met de naam Britta Simon gemaakt in drie Zscaler. Drie Zscaler biedt ondersteuning voor just-in-time inrichting, dat standaard is ingeschakeld. Er is geen actie-item voor u in deze sectie. Als een gebruiker nog niet in drie Zscaler bestaat, wordt een nieuw wordt gemaakt wanneer u probeert te krijgen tot Zscaler drie.

>[!Note]
>Als u maken van een gebruiker handmatig wilt, neem dan contact op met [Zscaler drie ondersteuningsteam](https://www.zscaler.com/company/contact).

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de drie Zscaler tegel in het toegangsvenster, moet u worden automatisch aangemeld bij de waarvoor u een SSO instellen van het type Zscaler drie. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

