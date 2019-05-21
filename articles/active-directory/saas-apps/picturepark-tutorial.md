---
title: 'Zelfstudie: Azure Active Directory-integratie met Picturepark | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Picturepark.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 31c21cd4-9c00-4cad-9538-a13996dc872f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/18/2019
ms.author: jeedes
ms.openlocfilehash: 617c75024b45dab7ff2466b99bfb71c18cdd778a
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2019
ms.locfileid: "65904584"
---
# <a name="tutorial-azure-active-directory-integration-with-picturepark"></a>Zelfstudie: Azure Active Directory-integratie met Picturepark

In deze zelfstudie leert u hoe u Picturepark integreren met Azure Active Directory (Azure AD).
Picturepark integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot Picturepark heeft.
* U kunt uw gebruikers worden automatisch aangemeld Picturepark (Single Sign-On) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Picturepark, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* Eenmalige aanmelding Picturepark ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor Picturepark **SP** gestart door SSO

## <a name="adding-picturepark-from-the-gallery"></a>Picturepark uit de galerie toe te voegen

Voor het configureren van de integratie van Picturepark in Azure AD, moet u Picturepark uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Picturepark uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **Picturepark**, selecteer **Picturepark** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![Picturepark in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Picturepark op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Picturepark tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Picturepark, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureren van eenmalige aanmelding Picturepark](#configure-picturepark-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maken van de testgebruiker Picturepark](#create-picturepark-test-user)**  : als u wilt een equivalent van Britta Simon in Picturepark die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met Picturepark, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **Picturepark** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Picturepark domein en URL's, eenmalige aanmelding informatie](common/sp-identifier.png)

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<companyname>.picturepark.com`

    b. In het tekstvak **Id (Entiteits-id)** typt u een URL met het volgende patroon:

    |  |
    |--|
    | `https://<companyname>.current-picturepark.com`|
    | `https://<companyname>.picturepark.com`|
    | `https://<companyname>.next-picturepark.com`|
    | |

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke aanmeldings-URL en -id. Neem contact op met [Picturepark Client ondersteuningsteam](https://picturepark.com/about/contact/) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Klik in de sectie **SAML-handtekeningcertificaat** op de knop **Bewerken** om het dialoogvenster **SAML-handtekeningcertificaat** te openen.

    ![SAML-handtekeningcertificaat bewerken](common/edit-certificate.png)

6. Kopieer in de sectie **SAML-handtekeningcertificaat** de waarde voor **VINGERAFDRUK** en sla deze op de computer op.

    ![Waarde van vingerafdruk kopiëren](common/copy-thumbprint.png)

7. Op de **Picturepark instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften. Voor **aanmeldings-URL**, gebruikt u de waarde met het volgende patroon: `https://login.microsoftonline.com/_my_directory_id_/wsfed`

    > [!Note]
    > _my_directory_id_ is de tenant-id van Azure AD-abonnement.

    ![Configuratie-URL's kopiëren](./media/picturepark-tutorial/configurls.png)

    a. Azure AD-id

    b. Afmeldings-URL

### <a name="configure-picturepark-single-sign-on"></a>Picturepark voor eenmalige aanmelding configureren

1. In een ander browservenster, meld u aan bij uw bedrijf Picturepark site als beheerder.

2. Klik in de werkbalk bovenaan op **Systeembeheer**, en klik vervolgens op **beheerconsole**.
   
    ![Beheerconsole](./media/picturepark-tutorial/ic795062.png "-beheerconsole")

3. Klik op **verificatie**, en klik vervolgens op **id-providers**.
   
    ![Authentication](./media/picturepark-tutorial/ic795063.png "Authentication")

4. In de **identiteit providerconfiguratie** sectie, voert u de volgende stappen uit:
   
    ![ID-providerconfiguratie](./media/picturepark-tutorial/ic795064.png "Identity provider configureren")
   
    a. Klik op **Toevoegen**.
  
    b. Typ een naam voor uw configuratie.
   
    c. Selecteer **ingesteld als standaard**.
   
    d. In **verlener URI** tekstvak, plak de waarde van **aanmeldings-URL** die u hebt gekopieerd vanuit Azure portal.
   
    e. In **vertrouwde verlener Duimafdruk** tekstvak, plak de waarde van **vingerafdruk** die u hebt gekopieerd uit **SAML-handtekeningcertificaat** sectie. 

5. Klik op **JoinDefaultUsersGroup**.

6. Om in te stellen de **Emailaddress** kenmerk in de **Claim** tekstvak, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` en klikt u op **opslaan**.

      ![Configuratie](./media/picturepark-tutorial/ic795065.png "configuratie")

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

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Picturepark.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **Picturepark**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **Picturepark**.

    ![De koppeling Picturepark in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-picturepark-test-user"></a>Picturepark testgebruiker maken

Als u wilt dat Azure AD-gebruikers zich aanmelden bij Picturepark, moeten ze worden ingericht voor Picturepark. In het geval van Picturepark is inrichten een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Aanmelden bij uw **Picturepark** tenant.

1. Klik in de werkbalk bovenaan op **Systeembeheer**, en klik vervolgens op **gebruikers**.
   
    ![Gebruikers](./media/picturepark-tutorial/ic795067.png "Gebruikers")

1. In de **overzicht van gebruikers** tabblad **nieuw**.
   
    ![Gebruikersbeheer](./media/picturepark-tutorial/ic795068.png "Gebruikersbeheer")

1. Op de **Create User** dialoogvenster, voer de volgende stappen uit een geldige Azure Active Directory-gebruiker u wilt om in te richten:
   
    ![Create User](./media/picturepark-tutorial/ic795069.png "Create User")
   
    a. In de **e-mailadres** tekstvak, type de **e-mailadres** van de gebruiker `BrittaSimon@contoso.com`.  
   
    b. In de **wachtwoord** en **wachtwoord bevestigen** tekstvakken, type de **wachtwoord** van BrittaSimon. 
   
    c. In de **voornaam** tekstvak, type de **voornaam** van de gebruiker **Julia**. 
   
    d. In de **achternaam** tekstvak, type de **achternaam** van de gebruiker **Simon**.
   
    e. In de **bedrijf** tekstvak, type de **bedrijfsnaam** van de gebruiker. 
   
    f. In de **land** tekstvak, selecteer de **land/regio** van de gebruiker.
  
    g. In de **ZIP** tekstvak, type de **postcode** van de plaats.
   
    h. In de **plaats** tekstvak, type de **plaatsnaam** van de gebruiker.

    i. Selecteer een **taal**.
   
    j. Klik op **Create**.

>[!NOTE]
>U kunt alle andere Picturepark gebruiker-account maken van hulpprogramma's of API's geleverd door Picturepark voor het inrichten van gebruikersaccounts van de Azure AD.
>

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Picturepark in het toegangsvenster, moet u worden automatisch aangemeld bij de Picturepark waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

