---
title: 'Zelfstudie: Azure Active Directory-integratie met SAP Business Object Cloud | Microsoft Docs'
description: Leer hoe u eenmalige aanmelding tussen Azure Active Directory en SAP Business Object Cloud configureert.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 6c5e44f0-4e52-463f-b879-834d80a55cdf
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/31/2018
ms.author: jeedes
ms.openlocfilehash: 18adc7728ffd0b4faf2e63e7c5d3be0da7dd651c
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/07/2019
ms.locfileid: "54065111"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-business-object-cloud"></a>Zelfstudie: Azure Active Directory-integratie met SAP Business Object Cloud

In deze zelfstudie leert u hoe u SAP Business Object Cloud kunt integreren met Azure Active Directory (Azure AD).
De integratie van SAP Business Object Cloud met Azure AD biedt de volgende voordelen:

* U kunt in Azure AD beheren wie toegang heeft tot SAP Business Object Cloud.
* U kunt inschakelen dat gebruikers automatisch met hun Azure AD-account worden aangemeld bij SAP Business Object Cloud (eenmalige aanmelding).
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van de Azure AD-integratie met SAP Business Object Cloud hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Abonnement op SAP Business Object Cloud waarvoor eenmalige aanmelding is ingeschakeld

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* SAP Business Object Cloud biedt ondersteuning voor door **SP** geïnitieerde eenmalige aanmelding

## <a name="adding-sap-business-object-cloud-from-the-gallery"></a>SAP Business Object Cloud toevoegen vanuit de galerie

Voor het configureren van de integratie van SAP Business Object Cloud met Azure AD moet u SAP Business Object Cloud vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Ga als volgt te werk om SAP Business Object Cloud vanuit de galerie toe te voegen:**

1. Klik in het linkernavigatievenster in de **[Azure-portal](https://portal.azure.com)** op het **Azure Active Directory**-pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ **SAP Business Object Cloud** in het zoekvak, selecteer **SAP Business Object Cloud** in het deelvenster met resultaten en klik vervolgens op **Toevoegen** om de toepassing toe te voegen.

     ![SAP Business Object Cloud toevoegen vanuit de galerie](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie configureert en test u Azure AD-eenmalige aanmelding met SAP Business Object Cloud op basis van een testgebruiker met de naam **Britta Simon**.
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Azure AD-gebruiker en de daaraan gerelateerde gebruiker in SAP Business Object Cloud tot stand is gebracht.

Om eenmalige aanmelding bij SAP Business Object Cloud met Azure AD te configureren en testen, moet u de volgende procedures voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Eenmalige aanmelding bij SAP Business Object Cloud configureren](#configure-sap-business-object-cloud-single-sign-on)**: als u de instellingen voor eenmalige aanmelding aan de toepassingszijde wilt configureren.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Een testgebruiker voor SAP Business Object Cloud maken](#create-sap-business-object-cloud-test-user)**: als u een tegenhanger van Britta Simon in SAP Business Object Cloud wilt hebben die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD-eenmalige aanmelding met SAP Business Object Cloud moet u de volgende stappen uitvoeren:

1. Ga in de [Azure-portal](https://portal.azure.com/) naar de overzichtspagina van de integratie voor **SAP Business Object Cloud** en selecteer **Eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Domein- en URL-gegevens voor eenmalige aanmelding bij SAP Business Object Cloud](common/sp-identifier.png)

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met het volgende patroon:
    | |
    |-|-|
    | `https://<sub-domain>.sapanalytics.cloud/` |
    | `https://<sub-domain>.sapbusinessobjects.cloud/` |

    b. In het tekstvak **Id (Entiteits-id)** typt u een URL met het volgende patroon:
    | |
    |-|-|
    | `<sub-domain>.sapbusinessobjects.cloud` |
    | `<sub-domain>.sapanalytics.cloud` |

    > [!NOTE] 
    > De waarden in deze URL's zijn alleen ter demonstratie. Werk de waarden bij met de werkelijke aanmeldings-URL en identificatie-URL. Neem contact op met het [klantondersteuningsteam van SAP Business Object Cloud](https://help.sap.com/viewer/product/SAP_BusinessObjects_Cloud/release/) om de aanmeldings-URL op te vragen. U kunt de identificatie-URL vaststellen door het bestand met metagegevens van SAP Business Object Cloud te downloaden via de beheerconsole. Dit wordt verderop in de zelfstudie uitgelegd.

4. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

### <a name="configure-sap-business-object-cloud-single-sign-on"></a>Eenmalige aanmelding van SAP Business Object Cloud configureren

1. Meld u in een ander venster van de browser als beheerder aan bij de bedrijfssite van SAP Business Object Cloud.

2. Selecteer **Menu** > **System** > **Administration**.
    
    ![Selecteer achtereenvolgens Menu, System en Administration](./media/sapboc-tutorial/config1.png)

3. Selecteer het pictogram **Edit** (pen) op het tabblad **Security**.
    
    ![Selecteer het pictogram Edit op het tabblad Security](./media/sapboc-tutorial/config2.png)  

4. Selecteer **SAML Single Sign-On (SSO)** bij **Authentication Method**.

    ![Selecteer SAML Single Sign-On als de verificatiemethode](./media/sapboc-tutorial/config3.png)  

5. Selecteer **Download** om de metagegevens van de serviceprovider te downloaden (stap 1). Zoek in het bestand met metagegevens de waarde voor **entityID** en kopieer deze. Ga in de Azure-portal naar het dialoogvenster **Standaard SAML-configuratie** en plak de waarde in het vak **Id**.

    ![Kopieer en plak de waarde van entityID](./media/sapboc-tutorial/config4.png)  

6. Selecteer **Upload** onder **Upload Identity Provider metadata** om de metagegevens van de provider (stap 2) te uploaden die u van de Azure-portal hebt gedownload.  

    ![Selecteer Upload onder Upload Identity Provider metadata](./media/sapboc-tutorial/config5.png)

7. Selecteer in de lijst **User Attribute** het gebruikerskenmerk (stap 3) dat u wilt gebruiken voor uw implementatie. Dit gebruikerskenmerk wordt toegewezen aan de id-provider. Als u een aangepast kenmerk wilt invoeren op de pagina van de gebruiker, gebruikt u de optie **Custom SAML Mapping**. Selecteer anders **Email** of **USER ID** als het gebruikerskenmerk. In ons voorbeeld hebben we **Email** geselecteerd omdat we de claim voor de gebruikers-id met het kenmerk **userprincipalname** hebben toegewezen in de sectie **Gebruikerskenmerken en claims** in de Azure-portal. Hierdoor krijgen we de beschikking over een uniek e-mailadres voor de gebruiker, dat bij elke geslaagde SAML-reactie wordt verzonden naar SAP Business Object Cloud.

    ![Gebruikerskenmerk selecteren](./media/sapboc-tutorial/config6.png)

8. Om het account te controleren bij de id-provider (stap 4), voert u in het vak **Login Credential (Email)** het e-mailadres van de gebruiker in. Selecteer vervolgens **Verify Account**. Het systeem voegt aanmeldingsreferenties toe aan het gebruikersaccount.

    ![E-mailadres invoeren en Verify Account selecteren](./media/sapboc-tutorial/config7.png)

9. Selecteer het pictogram **Save**.

    ![Het pictogram Save](./media/sapboc-tutorial/save.png)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken 

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam****Britta Simon** in.
  
    b. In het veld **Gebruikersnaam** typt u **brittasimon@yourcompanydomain.extension**.  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie zorgt u ervoor dat Britta Simon van eenmalige aanmelding met Azure gebruik kan maken door haar toegang te verlenen tot SAP Business Object Cloud.

1. Selecteer in de Azure-portal achtereenvolgens **Bedrijfstoepassingen**, **Alle toepassingen** en **SAP Business Object Cloud**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer **SAP Business Object Cloud** in de lijst met toepassingen.

    ![De koppeling SAP Business Object Cloud in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-sap-business-object-cloud-test-user"></a>SAP Business Object Cloud-testgebruiker maken

Azure AD-gebruikers moeten worden ingericht in SAP Business Object Cloud voordat ze zich kunnen aanmelden bij SAP Business Object Cloud. In het geval van SAP Business Object Cloud is inrichten een handmatige taak.

Ga als volgt te werk om een gebruikersaccount in te richten:

1. Meld u als beheerder aan bij de bedrijfssite van SAP Business Object Cloud.

2. Selecteer **Menu** > **Security** > **Users**.

    ![Werknemer toevoegen](./media/sapboc-tutorial/user1.png)

3. Voeg op de pagina **Users** gegevens toe van de nieuwe gebruiker en selecteer **+**. 

    ![Pagina voor toevoegen van gebruikers](./media/sapboc-tutorial/user4.png)

    Voer de volgende stappen uit:

    a. Voer in het vak **USER ID** de gebruikers-id van de gebruiker in, zoals **Britta**.

    b. Voer in het vak **FIRST NAME** de voornaam van de gebruiker in, zoals **Britta**.

    c. Voer in het vak **LAST NAME** de achternaam van de gebruiker in, zoals **Simon**.

    d. Voer in het vak **DISPLAY NAME** de volledige naam van de gebruiker in, zoals **Britta Simon**.

    e. Voer in het vak **E-MAIL** het e-mailadres van de gebruiker in, zoals **brittasimon@contoso.com**.

    f. Selecteer op de pagina **Select Roles** de juiste rol voor de gebruiker en selecteer vervolgens **OK**.

      ![Rol selecteren](./media/sapboc-tutorial/user3.png)

    g. Selecteer het pictogram **Save**.    

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie gaat u uw configuratie van Azure AD-eenmalige aanmelding testen via het toegangsvenster.

Wanneer u op de tegel SAP Business Object Cloud klikt in het toegangsvenster, wordt u automatisch aangemeld bij het exemplaar van SAP Business Object Cloud waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

