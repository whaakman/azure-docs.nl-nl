---
title: 'Zelfstudie: Azure Active Directory-integratie met MindTickle | Microsoft Docs'
description: Ontdek hoe u eenmalige aanmelding configureert tussen Azure Active Directory en MindTickle.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 49751a98-bfde-48b4-a8f8-b96e1d18aefa
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/23/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3a02ef6da245db4fca19c2e20390cdad8718b301
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67097193"
---
# <a name="tutorial-azure-active-directory-integration-with-mindtickle"></a>Zelfstudie: Azure Active Directory-integratie met MindTickle

In deze zelfstudie leert u hoe u MindTickle kunt integreren met Azure Active Directory (Azure AD).
De integratie van MindTickle met Azure AD heeft de volgende voordelen:

* U kunt in Azure AD beheren wie toegang heeft tot MindTickle.
* U kunt inschakelen dat gebruikers automatisch met hun Azure AD-account worden aangemeld bij MindTickle (eenmalige aanmelding).
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om Azure AD-integratie met MindTickle te configureren:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Een abonnement op MindTickle waarvoor eenmalige aanmelding is ingeschakeld

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* MindTickle ondersteunt door **SP** geïnitieerde eenmalige aanmelding

* MindTickle biedt ondersteuning voor **Just-In-Time**-inrichting van gebruikers

## <a name="adding-mindtickle-from-the-gallery"></a>MindTickle toevoegen vanuit de galerie

Als u de integratie van MindTickle in Azure AD wilt configureren, moet u MindTickle vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Voer de volgende stappen uit om MindTickle toe te voegen vanuit de galerie:**

1. In de **[Azure-portal](https://portal.azure.com)** , klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ **MindTickle** in het zoekvak, selecteer **MindTickle** in het venster met resultaten en klik vervolgens op de knop **Toevoegen** om de toepassing toe te voegen.

     ![MindTickle in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie configureert en test u eenmalige aanmelding van Azure AD met MindTickle op basis van een testgebruiker met de naam **Britta Simon**.
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Azure AD-gebruiker en de daaraan gerelateerd gebruiker in MindTickle tot stand is gebracht.

U moet de volgende bouwstenen voltooien om eenmalige aanmelding voor Azure AD met MindTickle te configureren en te testen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)** : als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Eenmalige aanmelding bij MindTickle configureren](#configure-mindtickle-single-sign-on)** : als u de instellingen voor eenmalige aanmelding aan de clientzijde wilt configureren.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)** : als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)** : als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Testgebruiker voor MindTickle maken](#create-mindtickle-test-user)** : als u een tegenhanger van Britta Simon in MindTickle wilt hebben die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)** : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voer de volgende stappen uit om eenmalige aanmelding voor Azure Ad met MindTickle te configureren:

1. In de [Azure-portal](https://portal.azure.com/) selecteert u **Eenmalige aanmelding** op de integratiepagina van de toepassing **MindTickle**.

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

    c. Wanneer het bestand met metagegevens is geüpload, wordt de waarde voor **Id** automatisch ingevuld in de sectie **Standaard SAML-configuratie**:

    ![Informatie over eenmalige aanmelding voor MindTickle-domeinen en -URL's](common/sp-identifier.png)

    In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<subdomain>.mindtickle.com`

    > [!Note]
    > Als de waard voor **Id** niet automatisch wordt ingevuld, kunt u de waarde zelf invullen afhankelijk van uw behoeften. De waarde voor de aanmeldings-URL is niet echt. Werk de waarde bij met de werkelijke aanmeldings-URL. Neem contact op met [het MindTickle-ondersteuningsteam](mailto:support@mindtickle.com) om deze waarde te verkrijgen.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

6. In het gedeelte **MindTickle instellen** kopieert u de juiste URL('s) op basis van uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-mindtickle-single-sign-on"></a>Eenmalige aanmelding configureren voor MindTickle

Als u eenmalige aanmelding aan de **MindTickle**-zijde wilt configureren, moet u het gedownloade **XML-bestand met federatieve metagegevens** en de correcte uit de Azure-portal gekopieerde URL's verzenden naar het [MindTickle-ondersteuningsteam](mailto:support@mindtickle.com). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

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

In deze sectie gaat u Britta Simon toestemming geven voor gebruik van eenmalige aanmelding met Azure door haar toegang te geven tot MindTickle.

1. Selecteer in de Azure-portal achtereenvolgens **Bedrijfstoepassingen**, **Alle toepassingen** en **MindTickle**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer **MindTickle** in de lijst met toepassingen.

    ![Het koppeling naar MindTickle in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-mindtickle-test-user"></a>MindTickle-testgebruiker maken

In deze sectie wordt een gebruiker met de naam Britta Simon gemaakt in MindTickle. MindTickle biedt ondersteuning voor **Just-In-Time-inrichting van gebruikers**. Deze functie is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Als er nog geen gebruiker in MindTickle bestaat, wordt er na authenticatie een nieuwe gemaakt.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel MindTickle in het toegangsvenster klikt, wordt u automatisch aangemeld bij de instantie van MindTickle waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

