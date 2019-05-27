---
title: 'Zelfstudie: Azure Active Directory-integratie met ThirdLight | Microsoft Docs'
description: In deze zelfstudie leert u hoe het configureren van eenmalige aanmelding tussen Azure Active Directory en ThirdLight.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 168aae9a-54ee-4c2b-ab12-650a2c62b901
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/25/2019
ms.author: jeedes
ms.openlocfilehash: 67c8dcfffd78d4c0114a96622235d6548627fa92
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66236963"
---
# <a name="tutorial-azure-active-directory-integration-with-thirdlight"></a>Zelfstudie: Azure Active Directory-integratie met ThirdLight

In deze zelfstudie leert u hoe u ThirdLight integreert met Azure Active Directory (Azure AD). Deze integratie biedt de volgende voordelen:

* U kunt Azure AD om te bepalen wie toegang tot ThirdLight heeft gebruiken.
* U kunt uw gebruikers kunnen automatisch worden aangemeld bij ThirdLight (eenmalige aanmelding) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts in één centrale locatie kunt beheren: de Azure-portal.

Als u meer informatie over SaaS-app-integratie met Azure AD wilt, Zie [eenmalige aanmelding voor toepassingen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met ThirdLight, moet u beschikken over:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/).
* Een ThirdLight-abonnement met eenmalige aanmelding ingeschakeld.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie configureert en Azure AD eenmalige aanmelding testen in een testomgeving.

* ThirdLight biedt ondersteuning voor Serviceprovider geïnitieerde eenmalige aanmelding.

## <a name="add-thirdlight-from-the-gallery"></a>ThirdLight uit de galerie toevoegen

Als u de integratie van ThirdLight in Azure AD instelt, moet u ThirdLight uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster **Azure Active Directory**:

    ![Selecteer Azure Active Directory](common/select-azuread.png)

2. Ga naar **bedrijfstoepassingen** > **alle toepassingen**:

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u wilt een toepassing hebt toegevoegd, selecteert u **nieuwe toepassing** aan de bovenkant van het venster:

    ![Nieuwe toepassing selecteren](common/add-new-app.png)

4. Voer in het zoekvak **ThirdLight**. Selecteer **ThirdLight** in de zoekresultaten en selecteer vervolgens **toevoegen**.

     ![Zoekresultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureren en testen van Azure AD eenmalige aanmelding met ThirdLight met behulp van een testgebruiker met de naam Britta Simon.
Om in te schakelen eenmalige aanmelding, moet u een relatie tussen een Azure AD-gebruiker en de bijbehorende gebruiker in ThirdLight vast te stellen.

Als u wilt configureren en Azure AD eenmalige aanmelding met ThirdLight testen, moet u deze stappen:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  om in te schakelen van de functie voor uw gebruikers.
2. **[Configureren van eenmalige aanmelding ThirdLight](#configure-thirdlight-single-sign-on)**  aan de toepassing.
3. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  voor het testen van Azure AD eenmalige aanmelding.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  zodat Azure AD eenmalige aanmelding voor de gebruiker.
5. **[Maak een testgebruiker ThirdLight](#create-a-thirdlight-test-user)**  dat gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**  om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie schakelt u Azure AD eenmalige aanmelding in de Azure-portal.

Voor het configureren van Azure AD eenmalige aanmelding met ThirdLight, de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/), selecteert u op de pagina ThirdLight toepassingen integratie **eenmalige aanmelding**:

    ![Schakel eenmalige aanmelding](common/select-sso.png)

2. In de **selecteert u een methode voor eenmalige aanmelding** in het dialoogvenster, selecteer **SAML/WS-Federation** modus voor eenmalige aanmelding inschakelen:

    ![Selecteer een methode voor eenmalige aanmelding](common/select-saml-option.png)

3. Op de **instellen van eenmalige aanmelding met SAML** weergeeft, schakelt de **bewerken** pictogram opent de **SAML-basisconfiguratie** in het dialoogvenster:

    ![Pictogram bewerken](common/edit-urls.png)

4. In de **SAML-basisconfiguratie** dialoogvenster vak, voer de volgende stappen uit.

    ![In het dialoogvenster van Basic SAML-configuratie](common/sp-identifier.png)

    1. In de **aanmeldings-URL** vak, een URL opgeven in dit patroon:
    
          `https://<subdomain>.thirdlight.com/`

    1. In de **id (entiteits-ID)** vak, een URL opgeven in dit patroon:

       `https://<subdomain>.thirdlight.com/saml/sp`

       > [!NOTE]
       > Deze waarden zijn tijdelijke aanduidingen. U moet de werkelijke aanmeldings-URL en -id gebruiken. Neem contact op met de [ThirdLight ondersteuningsteam](https://www.thirdlight.com/support) om de waarden te verkrijgen. U kunt ook verwijzen naar de patronen die wordt weergegeven in de **SAML-basisconfiguratie** in het dialoogvenster in de Azure-portal.

5. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, selecteer de **downloaden** koppelen naast **federatieve metagegevens-XML** , overeenkomstig uw vereisten en slaat u het bestand op uw computer:

    ![De koppeling om het certificaat te downloaden](common/metadataxml.png)

6. In de **ThirdLight instellen** sectie, kopieert u de juiste URL's, op basis van uw vereisten:

    ![De configuratie van URL's kopiëren](common/copy-configuration-urls.png)

    1. **Aanmeldings-URL**.

    1. **Azure AD Identifier**.

    1. **Afmeldings-URL van**.

### <a name="configure-thirdlight-single-sign-on"></a>ThirdLight eenmalige aanmelding configureren

1. In een nieuw browservenster aanmelden bij uw bedrijf ThirdLight site als een beheerder.

1. Ga naar **configuratie** > **Systeembeheer** > **SAML2**:

    ![Systeembeheer](./media/thirdlight-tutorial/ic805843.png "Systeembeheer")

1. In de sectie SAML2-configuratie moet u de volgende stappen uitvoeren.
  
    ![De configuratiesectie SAML2](./media/thirdlight-tutorial/ic805844.png "SAML2-configuratiesectie")

    1. Selecteer **inschakelen SAML2 Single Sign-On**.

    1. Onder **bron voor de metagegevens van de IdP**, selecteer **IdP-metagegevens laden van XML**.

    1. Open het bestand met metagegevens die u hebt gedownload van Azure portal in de vorige sectie. Kopieer de inhoud van het bestand en plak deze in de **IdP metagegevens-XML** vak.

    1. Selecteer **opslaan SAML2-instellingen**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

In deze sectie maakt u een testgebruiker Britta Simon met de naam in Azure portal.

1. Selecteer in de Azure portal, **Azure Active Directory** selecteren in het linkerdeelvenster **gebruikers**, en selecteer vervolgens **alle gebruikers**:

    ![Selecteer alle gebruikers](common/users.png)

2. Selecteer **nieuwe gebruiker** aan de bovenkant van het venster:

    ![Nieuwe gebruiker selecteren](common/new-user.png)

3. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit.

    ![In het dialoogvenster](common/user-properties.png)

    1. Voer in het vak **Naam** **Britta Simon**in.
  
    1. In de **gebruikersnaam** Voer **BrittaSimon @\<uwbedrijfsdomein >.\< extensie >** . (Bijvoorbeeld BrittaSimon@contoso.com.)

    1. Selecteer **wachtwoord weergeven**, en noteer de waarde in de **wachtwoord** vak.

    1. Selecteer **Maken**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding door haar toegang verlenen tot ThirdLight gebruiken.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**, en selecteer vervolgens **ThirdLight**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen **ThirdLight**.

    ![Lijst met toepassingen](common/all-applications.png)

3. Selecteer in het linkerdeelvenster **gebruikers en groepen**:

    ![Gebruikers en groepen selecteren](common/users-groups-blade.png)

4. Selecteer **Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Gebruiker toevoegen selecteren](common/add-assign-user.png)

5. In de **gebruikers en groepen** in het dialoogvenster, selecteer **Britta Simon** in de lijst met gebruikers, en klik op de **Selecteer** knop aan de onderkant van het venster.

6. Als u een waarde voor de rol in het SAML-verklaring verwacht in de **rol selecteren** dialoogvenster Selecteer de juiste rol voor de gebruiker in de lijst. Klik op de **Selecteer** knop aan de onderkant van het venster.

7. Selecteer **Toewijzen** in het dialoogvenster **Toewijzing toevoegen**.

### <a name="create-a-thirdlight-test-user"></a>Maak een testgebruiker ThirdLight

Als u wilt dat Azure AD-gebruikers kunnen zich aanmelden bij ThirdLight, moet u toe te voegen aan ThirdLight. U moet deze handmatig toevoegen.

Voor het maken van een gebruikersaccount, de volgende stappen uitvoeren:

1. Aanmelden bij uw bedrijf ThirdLight site als een beheerder.

1. Ga naar de **gebruikers** tabblad.

1. Selecteer **gebruikers en groepen**.

1. Selecteer **nieuwe gebruiker toevoegen**.

1. Voer de gebruikersnaam, een naam of beschrijving en het e-mailadres van een geldige Azure AD-account dat u wilt inrichten. Kies een vooraf ingestelde of nieuwe leden van de groep.

1. Selecteer **Maken**.

> [!NOTE]
> U kunt een hulpprogramma voor het maken van gebruiker-account gebruiken of API wordt geleverd door ThirdLight voor het inrichten van gebruikersaccounts van de Azure AD.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

Nu moet u uw configuratie Azure AD eenmalige aanmelding testen met behulp van het toegangsvenster.

Wanneer u de tegel ThirdLight in het toegangsvenster selecteert, moet u worden automatisch aangemeld bij de ThirdLight-exemplaar waarvoor u eenmalige aanmelding hebt ingesteld. Zie voor meer informatie over het toegangsvenster, [toegang en gebruik apps op de portal mijn Apps](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Aanvullende resources

- [Zelfstudies voor het integreren van SaaS-toepassingen met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
