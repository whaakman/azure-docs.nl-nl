---
title: 'Zelfstudie: Azure Active Directory-integratie met MOVEit overdracht - Azure AD-integratie | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en MOVEit overdracht - Azure AD-integratie.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 8ff7102d-be73-4888-ae81-d8e3d01dd534
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/25/2019
ms.author: jeedes
ms.openlocfilehash: 0f6b9c85fa3462486a32cf55b10cb06bb84b0072
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57853049"
---
# <a name="tutorial-azure-active-directory-integration-with-moveit-transfer---azure-ad-integration"></a>Zelfstudie: Azure Active Directory-integratie met MOVEit overdracht - Azure AD-integratie

In deze zelfstudie leert u hoe u om te integreren, overdracht MOVEit - Azure AD-integratie met Azure Active Directory (Azure AD).
Integratie van MOVEit overdracht - Azure AD-integratie met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot MOVEit overdracht - Azure AD-integratie heeft.
* U kunt uw gebruikers worden automatisch aangemeld MOVEit overdracht - Azure AD-integratie met hun Azure AD-accounts (Single Sign-On) inschakelen.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met MOVEit overdracht - integratie van Azure AD, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* MOVEit overdracht - Azure AD-integratie eenmalige aanmelding ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor MOVEit overdracht - Azure AD-integratie **SP** gestart door SSO

## <a name="adding-moveit-transfer---azure-ad-integration-from-the-gallery"></a>MOVEit overdracht - Azure AD-integratie uit de galerie toevoegen

Als u wilt configureren van de integratie van MOVEit overdracht - Azure AD-integratie met Azure AD, moet u MOVEit overdracht - Azure AD-integratie uit de galerie aan de lijst van beheerde SaaS-apps toevoegen.

**Als u wilt toevoegen MOVEit overdracht - Azure AD-integratie uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **MOVEit overdracht - Azure AD-integratie**, selecteer **MOVEit overdracht - Azure AD-integratie** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen de de toepassing.

     ![MOVEit overdracht - Azure AD-integratie in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met MOVEit overdracht - Azure AD-integratie op basis van een testgebruiker met de naam **Britta Simon**.
Voor single sign-on bij werk-, is de relatie van een koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in MOVEit overdracht - moet Azure AD-integratie tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met MOVEit overdracht - de Azure AD-integratie, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[MOVEit overdracht - integratie van Azure AD eenmalige aanmelding configureren](#configure-moveit-transfer---azure-ad-integration-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[MOVEit Transfer - Azure AD-integratie testgebruiker maken](#create-moveit-transfer---azure-ad-integration-test-user)**  : als u wilt een equivalent van Britta Simon in MOVEit overdracht - integratie van Azure AD die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met MOVEit overdracht - integratie van Azure AD, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **MOVEit overdracht - Azure AD-integratie** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. Voer in de sectie **Standaard SAML-configuratie** de volgende stappen uit als u beschikt over een **bestand met metagegevens van de serviceprovider**:

    a. Klik op **Metagegevensbestand uploaden**.

    ![Metagegevensbestand uploaden](common/upload-metadata.png)

    b. Klik op het **mappictogram** om het metagegevensbestand te selecteren en klik op **Uploaden**.

    ![Metagegevensbestand kiezen](common/browse-upload-metadata.png)

    c. Nadat het bestand met metagegevens is geüpload, de **id** en **antwoord-URL** wordt automatisch gevuld **SAML-basisconfiguratie** sectie:

    ![MOVEit overdracht - integratie van Azure AD-domein en URL's één aanmeldings-informatie](common/sp-identifier-reply.png)

    In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://contoso.com`

    > [!NOTE]
    > De **aanmeldings-URL** waarde is niet echt. Werk de waarde bij met de werkelijke aanmeldings-URL. Neem contact op met [MOVEit overdracht - Azure AD-integratie clientondersteuning](https://community.ipswitch.com/s/support) team om de waarde. U kunt downloaden de **Service Provider-metagegevensbestand** uit de **metagegevens-URL van Service Provider** die later in wordt uitgelegd de **configureren MOVEit Transfer - Azure AD-integratie één Aanmeldings-** gedeelte van de zelfstudie. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

4. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

6. Op de **MOVEit overdracht - Azure AD-integratie instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-moveit-transfer---azure-ad-integration-single-sign-on"></a>MOVEit overdracht - integratie van Azure AD eenmalige aanmelding configureren

1. Meld u aan bij uw tenant MOVEit overdracht als beheerder.

2. Klik in het linkernavigatiedeelvenster op **Instellingen**.

    ![Instellingen voor sectie op App aan clientzijde](./media/moveittransfer-tutorial/tutorial_moveittransfer_000.png)

3. Klik op **eenmalige aanmelding** koppeling naar deze bevindt zich onder **beveiligingsbeleid -> verificatie van de gebruiker**.

    ![Beveiliging-beleid op App aan clientzijde](./media/moveittransfer-tutorial/tutorial_moveittransfer_001.png)

4. Klik op de koppeling van de metagegevens-URL om de metadata-document te downloaden.

    ![Serviceprovider metagegevens-URL](./media/moveittransfer-tutorial/tutorial_moveittransfer_002.png)
    
   * Controleer of **entityID** komt overeen met **id** in de **SAML-basisconfiguratie** sectie.
   * Controleer of **AssertionConsumerService** locatie-URL overeenkomt met **antwoord-URL** in de **SAML-basisconfiguratie** sectie.
    
     ![Eenmalige aanmelding in de app configureren](./media/moveittransfer-tutorial/tutorial_moveittransfer_007.png)

5. Klik op **id-Provider toevoegen** om toe te voegen een nieuwe federatieve id-Provider.

    ![Id-provider toevoegen](./media/moveittransfer-tutorial/tutorial_moveittransfer_003.png)

6. Klik op **bladeren...**  om te selecteren in het bestand met metagegevens die u hebt gedownload van Azure portal, klik vervolgens op **id-Provider toevoegen** het gedownloade bestand te uploaden.

    ![SAML-identiteitsprovider](./media/moveittransfer-tutorial/tutorial_moveittransfer_004.png)

7. Selecteer "**Ja**' als **ingeschakeld** in de **bewerken voor federatieve identiteit Providerinstellingen...**  pagina en klik op **opslaan**.

    ![Instellingen van de federatieve identiteit](./media/moveittransfer-tutorial/tutorial_moveittransfer_005.png)

8. In de **bewerken voor federatieve identiteit Provider gebruikersinstellingen** pagina, de volgende acties uitvoeren:
    
    ![Instellingen van de federatieve identiteit bewerken](./media/moveittransfer-tutorial/tutorial_moveittransfer_006.png)
    
    a. Selecteer **NameID van SAML** als **aanmeldingsnaam**.
    
    b. Selecteer **andere** als **volledige naam** en in de **kenmerknaam** tekstvak plaatst de waarde: `http://schemas.microsoft.com/identity/claims/displayname`.
    
    c. Selecteer **andere** als **e** en in de **kenmerknaam** tekstvak plaatst de waarde: `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.
    
    d. Selecteer **Ja** als **-account maken met een automatisch aanmeldt**.
    
    e. Klik op de knop **Save**.

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

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan MOVEit overdracht - Azure AD-integratie.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **MOVEit overdracht - Azure AD-integratie**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **MOVEit overdracht - Azure AD-integratie**.

    ![De overdracht MOVEit - Azure AD-integratie koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-moveit-transfer---azure-ad-integration-test-user"></a>MOVEit Transfer - Azure AD-integratie testgebruiker maken

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in MOVEit overdracht - Azure AD-integratie. MOVEit overdracht - integratie van Azure AD biedt ondersteuning voor just-in-time inrichting, die u hebt ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot MOVEit overdracht - integratie van Azure AD als deze nog niet bestaat.

>[!NOTE]
>Als u een gebruiker handmatig hebt gemaakt wilt, moet u contact op met de [MOVEit overdracht - ondersteuningsteam voor Azure AD-integratie Client](https://community.ipswitch.com/s/support).

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u klikt op de overdracht MOVEit - tegel van Azure AD-integratie in het toegangsvenster, u moet worden automatisch aangemeld bij de overdracht MOVEit - Azure AD-integratie waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

