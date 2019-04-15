---
title: 'Zelfstudie: Azure Active Directory-integratie met de manier waarop We doen | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en de manier waarop We doen.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 84fc4f36-ecd1-42c6-8a70-cb0f3dc15655
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/03/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 541be5466b65705daa0485976eab3df8eb3d707f
ms.sourcegitcommit: b8a8d29fdf199158d96736fbbb0c3773502a092d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/15/2019
ms.locfileid: "59565509"
---
# <a name="tutorial-azure-active-directory-integration-with-way-we-do"></a>Zelfstudie: Azure Active Directory-integratie met de manier waarop We doen

In deze zelfstudie leert u over het integreren van de manier waarop We doen met Azure Active Directory (Azure AD).
We doen integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot de manier waarop We doen heeft.
* U kunt uw gebruikers kunnen automatisch worden aangemeld bij de manier waarop We doen (Single Sign-On) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met de manier waarop We doen, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* Hoe We eenmalige aanmelding ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* We doen ondersteunt **SP** gestart door SSO

* We doen ondersteunt **Just In Time** inrichten van gebruikers

## <a name="adding-way-we-do-from-the-gallery"></a>Toe te voegen We doen vanuit de galerie

Voor het configureren van de integratie van de manier waarop We doen in Azure AD, moet u de manier waarop We doen vanuit de galerie aan de lijst met beheerde SaaS-apps toevoegen.

**Om toe te voegen We doen vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **manier doen We**, selecteer **manier doen We** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![We doen in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met de manier waarop We doen op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in de manier waarop We doen tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met de manier waarop We doen, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Configureren van de manier waarop We eenmalige aanmelding](#configure-way-we-do-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maken van de manier waarop We testgebruiker](#create-way-we-do-test-user)**  : als u wilt een equivalent van Britta Simon hebben in de manier waarop We doen dat is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met de manier waarop We doen, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **manier doen We** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Hoe We doen domein en URL's eenmalige aanmelding informatie](common/sp-identifier.png)

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<SUBDOMAIN>.waywedo.com/Authentication/ExternalSignIn`

    b. In het tekstvak **Id (Entiteits-id)** typt u een URL met de volgende notatie: `https://<SUBDOMAIN>.waywedo.com`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke aanmeldings-URL en id. Neem contact op met [manier waarop We doen Client ondersteuningsteam](mailto:support@waywedo.com) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Raw)** te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificateraw.png)

6. Op de **instellen van de manier waarop We doen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-way-we-do-single-sign-on"></a>Configureren van de manier waarop We Single Sign-On

1. In een ander browservenster aanmelden bij manier waarop We doen als een beveiligingsbeheerder.

2. Klik op de **persoon pictogram** in de rechterbovenhoek van een willekeurige pagina in de manier waarop We doen, en klik op **Account** in het vervolgkeuzemenu.

    ![We doen manier-account](./media/waywedo-tutorial/tutorial_waywedo_account.png) 

3. Klik op de **menupictogram** de push-navigatiemenu en klikt u op openen **Single Sign On**.

    ![We doen één](./media/waywedo-tutorial/tutorial_waywedo_single.png)

4. Op de **setup voor eenmalige aanmelding** pagina, voert u de volgende stappen uit:

    ![We doen opslaan](./media/waywedo-tutorial/tutorial_waywedo_save.png)

    a. Klik op de **eenmalige aanmelding inschakelen** overzet naar **Ja** eenmalige aanmelding inschakelen.

    b. In de **één aanmelding naam** tekstvak, Voer uw naam in.

    c. In de **entiteit-ID** tekstvak, plak de waarde van **Azure AD-id**, die u hebt gekopieerd vanuit Azure portal.

    d. Plak in het tekstvak **SAML SSO URL** de waarde van **Aanmeldings-URL** die u hebt gekopieerd uit de Azure-portal.

    e. Upload het certificaat door te klikken op de **Selecteer** naast **certificaat**.

    f. **Optionele instellingen** -
    
    * Wachtwoorden - inschakelen wanneer deze optie is uitgeschakeld, het normale wachtwoord fungeert voor de manier waarop We doen zodat gebruikers alleen eenmalige aanmelding kunnen gebruiken.

    * Automatische inrichting - inschakelen wanneer deze optie is ingeschakeld, het e-mailadres dat is gebruikt voor aanmelding automatisch wordt vergeleken met de lijst met gebruikers in de manier waarop We doen. Als het e-mailadres komt niet overeen met een actieve gebruiker in de manier waarop We doen, worden automatisch een nieuw gebruikersaccount voor de persoon aanmelden, vraagt eventueel ontbrekende informatie toegevoegd.

      > [!NOTE]
      > Gebruikers die zijn toegevoegd via eenmalige aanmelding als algemene gebruikers zijn toegevoegd en een rol niet is toegewezen in het systeem. Een beheerder is in te gaan en hun beveiligingsrol als een editor of een beheerder wijzigen en kunt ook een of meer organigram rollen toewijzen. 

    g. Klik op **opslaan** om vast te leggen van de instellingen.

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

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan de manier waarop We doen.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **manier doen We**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **manier doen We**.

    ![De manier waarop We doen koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-way-we-do-test-user"></a>We doen testgebruiker maken

In deze sectie wordt een gebruiker met de naam Britta Simon gemaakt in de manier waarop We doen. We doen biedt ondersteuning voor just-in-time-gebruikersinrichting, dat standaard is ingeschakeld. Er is geen actie-item voor u in deze sectie. Als een gebruiker nog niet in de manier waarop We doen bestaat, wordt een nieuw gemaakt nadat verificatie.

> [!Note]
> Als u maken van een gebruiker handmatig wilt, neem dan contact op met [manier waarop We doen Client ondersteuningsteam](mailto:support@waywedo.com).

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel manier waarop We doen in het toegangsvenster, moet u worden automatisch aangemeld bij de manier waarop We doen waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

