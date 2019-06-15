---
title: 'Zelfstudie: Azure Active Directory-integratie met ITRP | Microsoft Docs'
description: In deze zelfstudie leert u hoe het configureren van eenmalige aanmelding tussen Azure Active Directory en ITRP.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: e09716a3-4200-4853-9414-2390e6c10d98
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/25/2019
ms.author: jeedes
ms.openlocfilehash: c3be09c998399cdca886f207bf5fc621bf51d8dd
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67099604"
---
# <a name="tutorial-azure-active-directory-integration-with-itrp"></a>Zelfstudie: Azure Active Directory-integratie met ITRP

In deze zelfstudie leert u hoe u ITRP integreert met Azure Active Directory (Azure AD).
Deze integratie biedt de volgende voordelen:

* U kunt Azure AD om te bepalen wie toegang tot ITRP heeft gebruiken.
* U kunt uw gebruikers kunnen automatisch worden aangemeld bij ITRP (eenmalige aanmelding) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts in één centrale locatie kunt beheren: de Azure-portal.

Zie [Eenmalige aanmelding voor toepassingen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) voor meer informatie over de integratie van SaaS-apps met Azure AD.

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met ITRP, moet u beschikken over:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/).
* Een abonnement ITRP met eenmalige aanmelding ingeschakeld.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie configureert en Azure AD eenmalige aanmelding testen in een testomgeving.

* ITRP biedt ondersteuning voor Serviceprovider geïnitieerde eenmalige aanmelding.

## <a name="add-itrp-from-the-gallery"></a>ITRP uit de galerie toevoegen

Als u de integratie van ITRP in Azure AD instelt, moet u ITRP uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster **Azure Active Directory**:

    ![Selecteer Azure Active Directory](common/select-azuread.png)

2. Ga naar **bedrijfstoepassingen** > **alle toepassingen**:

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u wilt een toepassing hebt toegevoegd, selecteert u **nieuwe toepassing** aan de bovenkant van het venster:

    ![Nieuwe toepassing selecteren](common/add-new-app.png)

4. Voer in het zoekvak **ITRP**. Selecteer **ITRP** in de zoekresultaten en selecteer vervolgens **toevoegen**.

     ![Zoekresultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureren en testen van Azure AD eenmalige aanmelding met ITRP met behulp van een testgebruiker met de naam Britta Simon.
Om in te schakelen eenmalige aanmelding, moet u een relatie tussen een Azure AD-gebruiker en de bijbehorende gebruiker in ITRP vast te stellen.

Als u wilt configureren en Azure AD eenmalige aanmelding met ITRP testen, moet u deze stappen:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  om in te schakelen van de functie voor uw gebruikers.
2. **[Configureren van eenmalige aanmelding ITRP](#configure-itrp-single-sign-on)**  aan de toepassing.
3. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  voor het testen van Azure AD eenmalige aanmelding.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  zodat Azure AD eenmalige aanmelding voor de gebruiker.
5. **[Maak een testgebruiker ITRP](#create-an-itrp-test-user)**  dat gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**  om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie schakelt u Azure AD eenmalige aanmelding in de Azure-portal.

Voor het configureren van Azure AD eenmalige aanmelding met ITRP, de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/), selecteert u op de pagina ITRP toepassingen integratie **eenmalige aanmelding**:

    ![Schakel eenmalige aanmelding](common/select-sso.png)

2. In de **selecteert u een methode voor eenmalige aanmelding** in het dialoogvenster, selecteer **SAML/WS-Federation** modus voor eenmalige aanmelding inschakelen:

    ![Selecteer een methode voor eenmalige aanmelding](common/select-saml-option.png)

3. Op de **instellen van eenmalige aanmelding met SAML** weergeeft, schakelt de **bewerken** pictogram opent de **SAML-basisconfiguratie** in het dialoogvenster:

    ![Pictogram bewerken](common/edit-urls.png)

4. In de **SAML-basisconfiguratie** dialoogvenster vak, voer de volgende stappen uit.

    ![In het dialoogvenster van Basic SAML-configuratie](common/sp-identifier.png)

    1. In de **aanmeldings-URL** vak, een URL opgeven in dit patroon:
    
       `https://<tenant-name>.itrp.com`

    1. In de **id (entiteits-ID)** vak, een URL opgeven in dit patroon:

       `https://<tenant-name>.itrp.com`

    > [!NOTE]
    > Deze waarden zijn tijdelijke aanduidingen. U moet de werkelijke aanmeldings-URL en -id gebruiken. Neem contact op met de [ITRP ondersteuningsteam](https://www.itrp.com/support) om de waarden te verkrijgen. U kunt ook verwijzen naar de patronen die wordt weergegeven in de **SAML-basisconfiguratie** in het dialoogvenster in de Azure-portal.

5. In de **SAML-handtekeningcertificaat** sectie, selecteer de **bewerken** pictogram opent de **SAML-handtekeningcertificaat** in het dialoogvenster:

    ![Pictogram bewerken](common/edit-certificate.png)

6. In de **SAML-handtekeningcertificaat** in het dialoogvenster, Kopieer de **vingerafdruk** waarde en sla het bestand:

    ![Kopieer de vingerafdruk-waarde](common/copy-thumbprint.png)

7. In de **ITRP instellen** sectie, kopieert u de juiste URL's, op basis van uw vereisten:

    ![De configuratie van URL's kopiëren](common/copy-configuration-urls.png)

    1. **Aanmeldings-URL**.

    1. **Azure AD Identifier**.

    1. **Afmeldings-URL van**.

### <a name="configure-itrp-single-sign-on"></a>ITRP eenmalige aanmelding configureren

1. In een nieuw browservenster aanmelden bij uw bedrijf ITRP site als een beheerder.

1. Aan de bovenkant van het venster, selecteer de **instellingen** pictogram:

    ![Instellingenpictogram](./media/itrp-tutorial/ic775570.png "pictogram instellingen")

1. Selecteer in het linkerdeelvenster **Single Sign-On**:

    ![Selecteer Single Sign-On](./media/itrp-tutorial/ic775571.png "Selecteer Single Sign-On")

1. In de **Single Sign-On** configuratie sectie, de volgende stappen uitvoeren.

    ![Single Sign-On sectie](./media/itrp-tutorial/ic775572.png "sectie Single Sign-On")

    ![Single Sign-On sectie](./media/itrp-tutorial/ic775573.png "sectie Single Sign-On")

    1. Selecteer **ingeschakeld**.

    1. In de **externe afmeldings-URL van** vak, plak de **afmeldings-URL van** waarde die u hebt gekopieerd uit de Azure-portal.

    1. In de **URL voor SAML SSO-** vak, plak de **aanmeldings-URL** waarde die u hebt gekopieerd uit de Azure-portal.

    1. In de **certificaat vingerafdruk** vak, plak de **vingerafdruk** waarde van het certificaat dat u hebt gekopieerd uit de Azure-portal.

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
  
    1. In de **gebruikersnaam** Voer **BrittaSimon @\<uwbedrijfsdomein >.\< extensie >** . (Bijvoorbeeld BrittaSimon@contoso.com.)

    1. Selecteer **wachtwoord weergeven**, en noteer de waarde in de **wachtwoord** vak.

    1. Selecteer **Maken**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding door haar toegang verlenen tot ITRP gebruiken.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**, en selecteer vervolgens **ITRP**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen **ITRP**.

    ![Lijst met toepassingen](common/all-applications.png)

3. Selecteer in het linkerdeelvenster **gebruikers en groepen**:

    ![Gebruikers en groepen selecteren](common/users-groups-blade.png)

4. Selecteer **Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Gebruiker toevoegen selecteren](common/add-assign-user.png)

5. In de **gebruikers en groepen** in het dialoogvenster, selecteer **Britta Simon** in de lijst met gebruikers, en klik op de **Selecteer** knop aan de onderkant van het venster.

6. Als u een waarde voor de rol in het SAML-verklaring verwacht in de **rol selecteren** dialoogvenster Selecteer de juiste rol voor de gebruiker in de lijst. Klik op de **Selecteer** knop aan de onderkant van het venster.

7. Selecteer **Toewijzen** in het dialoogvenster **Toewijzing toevoegen**.

### <a name="create-an-itrp-test-user"></a>Maak een testgebruiker ITRP

Als u wilt dat Azure AD-gebruikers kunnen zich aanmelden bij ITRP, moet u toe te voegen aan ITRP. U moet deze handmatig toevoegen.

Voor het maken van een gebruikersaccount, de volgende stappen uitvoeren:

1. Aanmelden bij uw tenant ITRP.

1. Aan de bovenkant van het venster, selecteer de **Records** pictogram:

    ![Pictogram records](./media/itrp-tutorial/ic775575.png "Records pictogram")

1. Selecteer in het menu **mensen**:

    ![Personen selecteren](./media/itrp-tutorial/ic775587.png "personen selecteren")

1. Selecteer het plusteken ( **+** ) om toe te voegen een nieuwe persoon:

    ![Selecteer het plusteken](./media/itrp-tutorial/ic775576.png "Selecteer het plusteken")

1. In de **nieuwe persoon toevoegen** dialoogvenster vak, voer de volgende stappen uit.

    ![In het dialoogvenster toevoegen-nieuwe persoon](./media/itrp-tutorial/ic775577.png "in het dialoogvenster voor nieuwe persoon toevoegen")

    1. Voer de naam en e-mailadres van een geldige Azure AD-account dat u wilt toevoegen.

    1. Selecteer **Opslaan**.

> [!NOTE]
> U kunt een hulpprogramma voor het maken van gebruiker-account gebruiken of API wordt geleverd door ITRP voor het inrichten van gebruikersaccounts van de Azure AD.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

Nu moet u uw configuratie Azure AD eenmalige aanmelding testen met behulp van het toegangsvenster.

Wanneer u de tegel ITRP in het toegangsvenster selecteert, moet u worden automatisch aangemeld bij de ITRP-exemplaar waarvoor u eenmalige aanmelding hebt ingesteld. Zie voor meer informatie over het toegangsvenster, [toegang en gebruik apps op de portal mijn Apps](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Aanvullende resources

- [Zelfstudies voor het integreren van SaaS-toepassingen met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
