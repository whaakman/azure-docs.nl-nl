---
title: 'Zelfstudie: Azure Active Directory-integratie met EverBridge | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en EverBridge.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 58d7cd22-98c0-4606-9ce5-8bdb22ee8b3e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/18/2019
ms.author: jeedes
ms.openlocfilehash: 886cfc59ed41e25c8c3953888690f58e4cc4c252
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64695336"
---
# <a name="tutorial-azure-active-directory-integration-with-everbridge"></a>Zelfstudie: Azure Active Directory-integratie met EverBridge

In deze zelfstudie leert u hoe u EverBridge integreren met Azure Active Directory (Azure AD).
EverBridge integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot EverBridge heeft.
* U kunt uw gebruikers worden automatisch aangemeld EverBridge (Single Sign-On) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met EverBridge, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* Eenmalige aanmelding EverBridge ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor EverBridge **IDP** gestart door SSO

## <a name="adding-everbridge-from-the-gallery"></a>EverBridge uit de galerie toe te voegen

Voor het configureren van de integratie van EverBridge in Azure AD, moet u EverBridge uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen EverBridge uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **EverBridge**, selecteer **EverBridge** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![EverBridge in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met EverBridge op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in EverBridge tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met EverBridge, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[EverBridge configureren als EverBridge Manager Portal Single Sign-On](#configure-everbridge-as-everbridge-manager-portal-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[EverBridge configureren als EverBridge Manager Portal Single Sign-On](#configure-everbridge-as-everbridge-member-portal-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
4. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
5. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
6. **[Maken van de testgebruiker EverBridge](#create-everbridge-test-user)**  : als u wilt een equivalent van Britta Simon in EverBridge die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
7. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met EverBridge, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **EverBridge** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

    >[!NOTE]
    >U moet de configuraties van de toepassing als de Manager-Portal of als de lid-Portal aan beide uiteinden dat wil zeggen op Azure-Portal en Everbridge Portal doen.

4. Het configureren van de **EverBridge** toepassing als **EverBridge Manager Portal**op de **SAML-basisconfiguratie** sectie de volgende stappen uitvoeren:

    ![EverBridge domein en URL's, eenmalige aanmelding informatie](common/idp-intiated.png)

    a. In het tekstvak **Id** typt u een URL met het volgende patroon: `https://sso.everbridge.net/<API_Name>`

    b. In het tekstvak **Antwoord-URL** typt u een URL met het volgende patroon: `https://manager.everbridge.net/saml/SSO/<API_Name>/alias/defaultAlias`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke id en antwoord-URL. Neem contact op met [EverBridge ondersteuningsteam](mailto:support@everbridge.com) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Het configureren van de **EverBridge** toepassing als **EverBridge lid Portal**op de **SAML-basisconfiguratie** sectie, voert u de volgende stappen uit:

   * Als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![EverBridge domein en URL's, eenmalige aanmelding informatie](common/idp-intiated.png)

    * Typ in het tekstvak **Id** een URL met het volgende patroon: `https://sso.everbridge.net/<API_Name>/<Organization_ID>`

    * In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://member.everbridge.net/saml/SSO/<API_Name>/<Organization_ID>/alias/defaultAlias`

* Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    ![EverBridge domein en URL's, eenmalige aanmelding informatie](common/both-signonurl.png)

    * Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://member.everbridge.net/saml/login/<API_Name>/<Organization_ID>/alias/defaultAlias?disco=true`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden met de werkelijke-id, de antwoord-URL en aanmeldings-URL. Neem contact op met [EverBridge ondersteuningsteam](mailto:support@everbridge.com) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

6. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

7. Op de **EverBridge instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-everbridge-as-everbridge-manager-portal-single-sign-on"></a>EverBridge configureren als EverBridge Manager Portal Single Sign-On

1. Ophalen van geconfigureerd voor eenmalige aanmelding **EverBridge** als **EverBridge Manager Portal** toepassing, de volgende stappen uitvoeren: 
 
2. In een ander browservenster aanmelden bij EverBridge als beheerder.

3. Klik in het menu bovenaan op de **instellingen** tabblad en selecteer **Single Sign-On** onder **Security**.
   
     ![Eenmalige aanmelding configureren](./media/everbridge-tutorial/tutorial_everbridge_002.png)
   
     a. In de **naam** tekstvak typt u de naam van de id-Provider (bijvoorbeeld: naam van uw bedrijf).
   
     b. In de **API-naam** tekstvak typt u de naam van de API.
   
     c. Klik op **bestand kiezen** knop voor het uploaden van het bestand met metagegevens die u hebt gedownload van Azure portal.
   
     d. Selecteer in de locatie van de identiteit SAML **identiteit is in de NameIdentifier-element van het onderwerp overzicht**.
   
     e. In de **aanmeldings-URL van id-Provider** tekstvak, plak de waarde van **aanmeldings-URL** die u hebt gekopieerd vanuit Azure portal.
   
     f. Selecteer in de Service Provider gestart aanvragen Binding, **HTTP-omleiding**.

     g. Klik op **Opslaan**.

### <a name="configure-everbridge-as-everbridge-member-portal-single-sign-on"></a>EverBridge configureren als lid van de EverBridge Portal Single Sign-On

Het configureren van eenmalige aanmelding op **EverBridge** als **EverBridge lid Portal**, moet u voor het verzenden van de gedownloade **federatieve metagegevens-XML** naar [Everbridge ondersteuning voor team](mailto:support@everbridge.com). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken 

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In de **gebruikersnaam** veldtype `brittasimon@yourcompanydomain.extension`. Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan EverBridge.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **EverBridge**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **EverBridge**.

    ![De koppeling EverBridge in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-everbridge-test-user"></a>EverBridge testgebruiker maken

In deze sectie maakt u een gebruiker met de naam van Britta Simon in Everbridge. Werken met [EverBridge ondersteuningsteam](mailto:support@everbridge.com) om toe te voegen de gebruikers in het Everbridge-platform. Gebruikers moeten worden gemaakt en worden geactiveerd in EverBridge voordat u eenmalige aanmelding gebruiken. 

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel EverBridge in het toegangsvenster, moet u worden automatisch aangemeld bij de EverBridge waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

