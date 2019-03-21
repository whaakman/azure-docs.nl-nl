---
title: 'Zelfstudie: Azure Active Directory-integratie met NetSuite | Microsoft Docs'
description: Ontdek hoe u eenmalige aanmelding tussen Azure Active Directory en NetSuite configureert.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: dafa0864-aef2-4f5e-9eac-770504688ef4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/17/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 73bb37607af681fcd505706ae99b308024be2c2d
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57887422"
---
# <a name="tutorial-azure-active-directory-integration-with-netsuite"></a>Zelfstudie: Azure Active Directory-integratie met NetSuite

In deze zelfstudie leert u hoe u NetSuite integreert met Azure Active Directory (Azure AD).
De integratie van NetSuite met Azure AD heeft de volgende voordelen:

* U kunt in Azure AD bepalen wie er toegang heeft tot NetSuite.
* U kunt instellen dat gebruikers automatisch met hun Azure AD-account worden aangemeld bij NetSuite (eenmalige aanmelding).
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om Azure AD-integratie te configureren met NetSuite:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Abonnement op NetSuite met eenmalige aanmelding (SSO) ingeschakeld

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* NetSuite ondersteunt door **IDP** geïnitieerde SSO
* NetSuite biedt ondersteuning voor het **Just In Time** inrichten van gebruikers
* NetSuite biedt ondersteuning voor het [automatisch inrichten van gebruikers](NetSuite-provisioning-tutorial.md)

## <a name="adding-netsuite-from-the-gallery"></a>NetSuite toevoegen vanuit de galerie

Om de integratie van NetSuite in Azure AD te configureren, moet u NetSuite vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Ga als volgt te werk om NetSuite vanuit de galerie toe te voegen:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ **NetSuite** in het zoekvak, selecteer **NetSuite** in de lijst met resultaten en klik vervolgens op de knop **Toevoegen** om de toepassing toe te voegen.

     ![NetSuite in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In dit gedeelte gaat u eenmalige aanmelding van Azure AD met NetSuite configureren en testen op basis van een testgebruiker met de naam **Britta Simon**.
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Azure AD-gebruiker en de daaraan gerelateerde gebruiker in NetSuite tot stand is gebracht.

Als u eenmalige aanmelding van Azure AD wilt configureren en testen met NetSuite, moet u de volgende procedures uitvoeren:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Eenmalige aanmelding voor NetSuite configureren](#configure-netsuite-single-sign-on)**: de instellingen voor eenmalige aanmelding aan de clientzijde configureren.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Testgebruiker voor NetSuite maken](#create-netsuite-test-user)**: een tegenhanger voor Britta Simon maken in NetSuite die wordt gekoppeld aan de Azure AD-voorstelling van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voer de volgende stappen uit om eenmalige aanmelding van Azure AD te configureren met NetSuite:

1. Ga in de [Azure-portal](https://portal.azure.com/) naar de pagina met de integratie van de toepassing **NetSuite** en selecteer **Eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Gegevens voor domein en URL's voor eenmalige aanmelding bij NetSuite](common/idp-reply.png)

    In het tekstvak **Antwoord-URL** typt u een URL met het volgende patroon:

    `https://<tenant-name>.NetSuite.com/saml2/acs`

    `https://<tenant-name>.na1.NetSuite.com/saml2/acs`

    `https://<tenant-name>.na2.NetSuite.com/saml2/acs`

    `https://<tenant-name>.sandbox.NetSuite.com/saml2/acs`

    `https://<tenant-name>.na1.sandbox.NetSuite.com/saml2/acs`

    `https://<tenant-name>.na2.sandbox.NetSuite.com/saml2/acs`

    > [!NOTE]
    > De waarde is niet echt. Werk de waarde bij met de werkelijke antwoord-URL. Neem contact op met het [ondersteuningsteam van NetSuite](http://www.netsuite.com/portal/services/support-services/suitesupport.shtml) om de waarde op te vragen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. De NetSuite-toepassing verwacht dat de SAML-asserties een specifieke indeling hebben. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit de sectie **Gebruikerskenmerken** op de integratiepagina van de toepassing-beheren. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op de knop **Bewerken** om het dialoogvenster **Gebruikerskenmerken** te openen.

    ![image](common/edit-attribute.png)

6. Bewerk in het gedeelte **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** de claims met het **pictogram Bewerken** of voeg de claims toe door met **Nieuwe claim toevoegen** het kenmerk van het SAML-token te configureren, zoals wordt weergegeven in de bovenstaande afbeelding. Hierna voert u de volgende stappen uit:
    
    | Name | Bronkenmerk | 
    | ---------------| --------------- |
    | account  | `account id` |

    a. Klik op **Nieuwe claim toevoegen** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Laat **Naamruimte** leeg.

    d. Selecteer Bron bij **Kenmerk**.

    e. Typ de kenmerkwaarde voor die rij in de lijst met **bronkenmerken**.

    f. Klik op **OK**.

    g. Klik op **Opslaan**.

    >[!NOTE]
    >De waarde voor het accountkenmerk is niet echt. U gaat deze waarde later aanpassen. Dit wordt verderop in de zelfstudie uitgelegd.

4. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om de **metagegevens-XML** te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

6. Kopieer in het gedeelte **NetSuite instellen** de juiste URL('s) op basis van uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-netsuite-single-sign-on"></a>Eenmalige aanmelding voor NetSuite configureren

1. Open een nieuw tabblad in uw browser en meld u als beheerder aan bij de bedrijfssite van NetSuite.

2. Klik op de werkbalk bovenaan de pagina op  **Setup**, ga naar  **Company** en klik op  **Enable Features**.

    ![Eenmalige aanmelding configureren](./media/NetSuite-tutorial/ns-setupsaml.png)

3. Klik op de werkbalk in het midden van de pagina op **SuiteCloud**.

    ![Eenmalige aanmelding configureren](./media/NetSuite-tutorial/ns-suitecloud.png)

4. Selecteer in het gedeelte  **Manage Authentication**  **SAML SINGLE SIGN-ON** om de optie SAML SINGLE SIGN-ON in te schakelen in NetSuite.

    ![Eenmalige aanmelding configureren](./media/NetSuite-tutorial/ns-ticksaml.png)

5. Klik op de werkbalk in het midden van de pagina op **Setup**.

    ![Eenmalige aanmelding configureren](./media/NetSuite-tutorial/ns-setup.png)

6. Klik in de lijst **SETUP TASKS** op **Integration**.

    ![Eenmalige aanmelding configureren](./media/NetSuite-tutorial/ns-integration.png)

7. Klik in het gedeelte **MANAGE AUTHENTICATION** op **SAML Single Sign-on**.

    ![Eenmalige aanmelding configureren](./media/NetSuite-tutorial/ns-saml.png)

8. Voer op de pagina **SAML Setup**, onder **NetSuite Configuration**, de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/NetSuite-tutorial/ns-saml-setup.png)
  
    a. Selecteer **PRIMARY AUTHENTICATION METHOD**.

    b. Selecteer **UPLOAD IDP METADATA FILE** voor het veld met het label **SAMLV2 IDENTITY PROVIDER METADATA**. Klik vervolgens op **Browse** om het bestand met metagegevens dat u hebt gedownload van de Azure-portal te uploaden.

    c. Klik op **Submit**

9. Klik in NetSuite op **Setup**, ga naar **Company** en klik op **Company Information** in het bovenste navigatiemenu.

    ![Eenmalige aanmelding configureren](./media/NetSuite-tutorial/ns-com.png)

    ![Eenmalige aanmelding configureren](./media/NetSuite-tutorial/ns-account-id.png)

    b. Kopieer de **ACCOUNT ID** in de rechterkolom op de pagina **Company Information**.

    c. Plak de **account-id** die u hebt gekopieerd uit het NetSuite-account in het veld **Waarde kenmerk** in Azure AD. 

10. Voordat gebruikers eenmalige aanmelding kunnen gebruiken met NetSuite moeten ze over de juiste machtigingen beschikken in NetSuite. Volg de onderstaande instructies om deze machtigingen toe te wijzen.

    a. Klik in het bovenste navigatiemenu op **Setup**.

    ![Eenmalige aanmelding configureren](./media/NetSuite-tutorial/ns-setup.png)

    b. Selecteer in het navigatiemenu links **Users/Roles** en klik op **Manage Roles**.

    ![Eenmalige aanmelding configureren](./media/NetSuite-tutorial/ns-manage-roles.png)

    c. Klik op **New Role**.

    d. Typ in het veld **Name** een naam voor de nieuwe rol.

    ![Eenmalige aanmelding configureren](./media/NetSuite-tutorial/ns-new-role.png)

    e. Klik op **Opslaan**.

    f. Klik in het menu aan de bovenkant op **Permissions**. Klik vervolgens op **Setup**.

    ![Eenmalige aanmelding configureren](./media/NetSuite-tutorial/ns-sso.png)

    g. Selecteer **SAML Single Sign-on** en klik vervolgens op **Add**.

    h. Klik op **Opslaan**.

    i. Klik in het bovenste navigatiemenu op **Setup** en daarna op **Setup Manager**.

    ![Eenmalige aanmelding configureren](./media/NetSuite-tutorial/ns-setup.png)

    j. Selecteer in het navigatiemenu links **Users/Roles** en klik op **Manage Users**.

    ![Eenmalige aanmelding configureren](./media/NetSuite-tutorial/ns-manage-users.png)

    k. Selecteer een testgebruiker. Klik vervolgens op **Edit** en ga naar het tabblad **Access**.

    ![Eenmalige aanmelding configureren](./media/NetSuite-tutorial/ns-edit-user.png)

    l. Wijs in het dialoogvenster Roles de juiste rol toe die u hebt gemaakt.

    ![Eenmalige aanmelding configureren](./media/NetSuite-tutorial/ns-add-role.png)

    m. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken 

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In de **gebruikersnaam** veldtype **brittasimon\@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In dit gedeelte gaat u Britta Simon toestemming geven voor gebruik van eenmalige aanmelding met Azure door haar toegang te geven tot NetSuite.

1. Selecteer **Bedrijfstoepassingen** in de Azure-portal, selecteer **Alle toepassingen** en selecteer vervolgens **NetSuite**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Typ en selecteer **NetSuite** in de lijst met toepassingen.

    ![De koppeling NetSuite in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-netsuite-test-user"></a>Testgebruiker voor NetSuite maken

In dit gedeelte wordt er een gebruiker met de naam Britta Simon gemaakt in NetSuite. NetSuite biedt ondersteuning voor Just-In-Time-inrichting van gebruikers. Deze functie is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Als er nog geen gebruiker bestaat in NetSuite, wordt er een nieuwe gemaakt na verificatie.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u in het toegangsvenster op de tegel NetSuite klikt, zou u automatisch moeten worden aangemeld bij het exemplaar van in het toegangsvenster waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Inrichten van gebruikers configureren](NetSuite-provisioning-tutorial.md)

