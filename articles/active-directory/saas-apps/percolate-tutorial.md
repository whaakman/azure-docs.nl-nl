---
title: 'Zelfstudie: Azure Active Directory-integratie met Percolate | Microsoft Docs'
description: In deze zelfstudie leert u hoe het configureren van eenmalige aanmelding tussen Azure Active Directory en Percolate.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 355f9659-b378-44c9-aa88-236e9b529a53
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/01/2019
ms.author: jeedes
ms.openlocfilehash: 83027e9fbc1826de727f123afe4507c2858c49ff
ms.sourcegitcommit: 179918af242d52664d3274370c6fdaec6c783eb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/13/2019
ms.locfileid: "65560570"
---
# <a name="tutorial-azure-active-directory-integration-with-percolate"></a>Zelfstudie: Azure Active Directory-integratie met Percolate

In deze zelfstudie leert u hoe u Percolate integreert met Azure Active Directory (Azure AD).

Deze integratie biedt de volgende voordelen:

* U kunt Azure AD om te bepalen wie toegang tot Percolate heeft gebruiken.
* U kunt uw gebruikers kunnen automatisch worden aangemeld bij Percolate (eenmalige aanmelding) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts in één centrale locatie kunt beheren: de Azure-portal.

Zie [Eenmalige aanmelding voor toepassingen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) voor meer informatie over de integratie van SaaS-apps met Azure AD.

Als u een Azure-abonnement geen [Maak een gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Percolate, moet u beschikken over:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/).
* Een Percolate-abonnement met eenmalige aanmelding ingeschakeld.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie configureert en Azure AD eenmalige aanmelding testen in een testomgeving.

* Percolate ondersteunt SP geïnitieerde en IdP gestart door SSO.

## <a name="add-percolate-from-the-gallery"></a>Percolate uit de galerie toevoegen

Voor het configureren van de integratie van Percolate in Azure AD, moet u Percolate uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster **Azure Active Directory**:

    ![Selecteer Azure Active Directory](common/select-azuread.png)

2. Ga naar **bedrijfstoepassingen** > **alle toepassingen**:

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u wilt een toepassing hebt toegevoegd, selecteert u **nieuwe toepassing** aan de bovenkant van het venster:

    ![Nieuwe toepassing selecteren](common/add-new-app.png)

4. Voer in het zoekvak **Percolate**. Selecteer **Percolate** in de zoekresultaten en selecteer vervolgens **toevoegen**.

     ![Zoekresultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureren en testen van Azure AD eenmalige aanmelding met Percolate met behulp van een testgebruiker met de naam Britta Simon.
Om in te schakelen eenmalige aanmelding, moet u een relatie tussen een Azure AD-gebruiker en de bijbehorende gebruiker in Percolate vast te stellen.

Als u wilt configureren en Azure AD eenmalige aanmelding met Percolate testen, moet u deze stappen:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  om in te schakelen van de functie voor uw gebruikers.
2. **[Configureren van eenmalige aanmelding Percolate](#configure-percolate-single-sign-on)**  aan de toepassing.
3. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  voor het testen van Azure AD eenmalige aanmelding.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  zodat Azure AD eenmalige aanmelding voor de gebruiker.
5. **[Maak een testgebruiker Percolate](#create-a-percolate-test-user)**  dat gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**  om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie schakelt u Azure AD eenmalige aanmelding in de Azure-portal.

Voor het configureren van Azure AD eenmalige aanmelding met Percolate, de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **Percolate** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**:

    ![Schakel eenmalige aanmelding](common/select-sso.png)

2. In de **selecteert u een methode voor eenmalige aanmelding** in het dialoogvenster, selecteer **SAML/WS-Federation** modus voor eenmalige aanmelding inschakelen:

    ![Selecteer een methode voor eenmalige aanmelding](common/select-saml-option.png)

3. Op de **instellen van eenmalige aanmelding met SAML** weergeeft, schakelt de **bewerken** pictogram opent de **SAML-basisconfiguratie** in het dialoogvenster:

    ![Pictogram bewerken](common/edit-urls.png)

4. In de **SAML-basisconfiguratie** in het dialoogvenster, u hoeft niet te doen om te configureren van de toepassing in de modus voor IdP gestart door. De app is al geïntegreerd met Azure.

    ![Domein en URL's één aanmelding informatie percolate](common/preintegrated.png)

5. Als u configureren van de toepassing in de modus SP geïnitieerde wilt, selecteert u **extra URL's instellen** en in de **aanmeldings-URL** Voer **https://percolate.com/app/login**:

   ![Domein en URL's één aanmelding informatie percolate](common/metadata-upload-additional-signon.png)
6. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, selecteer de **kopie** pictogram kopiëren de **App-Url voor federatieve metagegevens** . Sla deze URL.

    ![Kopiëren van de App-URL voor federatieve metagegevens](common/copy-metadataurl.png)

7. In de **Percolate instellen** sectie, kopieert u de juiste URL's, op basis van uw vereisten.

    ![De configuratie van URL's kopiëren](common/copy-configuration-urls.png)

    1. **Aanmeldings-URL**.

    1. **Azure AD Identifier**.

    1. **Afmeldings-URL van**.

### <a name="configure-percolate-single-sign-on"></a>Percolate eenmalige aanmelding configureren

1. In een nieuw browservenster aanmelden bij Percolate als een beheerder.

2. Selecteer aan de linkerkant van de startpagina, **instellingen**:
    
    ![Instellingen selecteren](./media/percolate-tutorial/configure01.png)

3. Selecteer in het linkerdeelvenster **SSO** onder **organisatie**:

    ![Schakel eenmalige aanmelding bij de organisatie](./media/percolate-tutorial/configure02.png)

    1. In de **aanmeldings-URL** vak, plak de **aanmeldings-URL** waarde die u hebt gekopieerd uit de Azure-portal.

    1. In de **entiteit-ID** vak, plak de **Azure AD-id** waarde die u hebt gekopieerd uit de Azure-portal.

    1. Open in Kladblok het base-64 gecodeerde certificaat dat u hebt gedownload vanuit Azure portal. Kopieer de inhoud en plak deze in de **x509 certificaten** vak.

    1. In de **e kenmerk** Voer **emailaddress**.

    1. De **metagegevens-URL van identiteitsprovider** box is een optioneel veld. Als u hebt gekopieerd een **App-Url voor federatieve metagegevens** vanuit Azure portal, kunt u deze plakken in dit vak.

    1. In de **AuthNRequests moet worden ondertekend?** in de lijst met **Nee**.

    1. In de **SSO inschakelen automatische inrichting** in de lijst met **Nee**.

    1. Selecteer **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

In deze sectie maakt u een testgebruiker Britta Simon met de naam in Azure portal.

1. Selecteer in de Azure portal, **Azure Active Directory** selecteren in het linkerdeelvenster **gebruikers**, en selecteer vervolgens **alle gebruikers**:

    ![Selecteer alle gebruikers](common/users.png)

2. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm:

    ![Nieuwe gebruiker selecteren](common/new-user.png)

3. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit.

    ![In het dialoogvenster](common/user-properties.png)

    1. Voer in het vak **Naam** **Britta Simon**in.
  
    1. In de **gebruikersnaam** Voer **BrittaSimon @\<uwbedrijfsdomein >.\< extensie >**. (Bijvoorbeeld BrittaSimon@contoso.com.)

    1. Selecteer **wachtwoord weergeven**, en noteer de waarde in de **wachtwoord** vak.

    1. Selecteer **Maken**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruik van Azure AD eenmalige aanmelding door haar toegang verlenen tot Percolate.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**, en selecteer vervolgens **Percolate**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen **Percolate**.

    ![Lijst met toepassingen](common/all-applications.png)

3. Selecteer in het linkerdeelvenster **gebruikers en groepen**:

    ![Gebruikers en groepen selecteren](common/users-groups-blade.png)

4. Selecteer **Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Gebruikers en groepen selecteren](common/add-assign-user.png)

5. In de **gebruikers en groepen** in het dialoogvenster, selecteer **Britta Simon** in de lijst met gebruikers, en klik op de **Selecteer** knop aan de onderkant van het scherm.

6. Als u een waarde voor de rol in het SAML-verklaring verwacht in de **rol selecteren** dialoogvenster Selecteer de juiste rol voor de gebruiker in de lijst. Klik op de **Selecteer** knop aan de onderkant van het scherm.

7. Selecteer **Toewijzen** in het dialoogvenster **Toewijzing toevoegen**.

### <a name="create-a-percolate-test-user"></a>Maak een testgebruiker Percolate

Als u wilt dat Azure AD-gebruikers kunnen zich aanmelden bij Percolate, moet u toe te voegen aan Percolate. U moet deze handmatig toevoegen.

Voor het maken van een gebruikersaccount, de volgende stappen uitvoeren:

1. Aanmelden bij Percolate als een beheerder.

2. Selecteer in het linkerdeelvenster **gebruikers** onder **organisatie**. Selecteer **nieuwe gebruikers**:

    ![Nieuwe gebruikers selecteren](./media/percolate-tutorial/configure03.png)

3. Op de **maken gebruikers** pagina, de volgende stappen uitvoeren.

    ![Pagina gebruikers maken](./media/percolate-tutorial/configure04.png)

    1. In de **e** voert u het e-mailadres van de gebruiker. Bijvoorbeeld brittasimon@contoso.com.

    1. In de **volledige naam** voert u de naam van de gebruiker. Bijvoorbeeld, **Brittasimon**.

    1. Selecteer **maken gebruikers**.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

Nu moet u uw configuratie Azure AD eenmalige aanmelding testen met behulp van het toegangsvenster.

Wanneer u de tegel Percolate in het toegangsvenster selecteert, moet u worden automatisch aangemeld bij de Percolate-exemplaar waarvoor u eenmalige aanmelding hebt ingesteld. Zie voor meer informatie, [toegang en gebruik apps op de portal mijn Apps](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Aanvullende bronnen

- [Zelfstudies voor het integreren van SaaS-toepassingen met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)