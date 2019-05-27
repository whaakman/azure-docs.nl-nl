---
title: 'Zelfstudie: Azure Active Directory-integratie met InsideView | Microsoft Docs'
description: In deze zelfstudie leert u hoe het configureren van eenmalige aanmelding tussen Azure Active Directory en InsideView.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: c489a7ab-6b1f-4efb-8a66-8bc13bca78c3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/20/2019
ms.author: jeedes
ms.openlocfilehash: 0fdabd237fa128326673d84e889387d03f184b00
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66236578"
---
# <a name="tutorial-azure-active-directory-integration-with-insideview"></a>Zelfstudie: Azure Active Directory-integratie met InsideView

In deze zelfstudie leert u hoe u InsideView integreert met Azure Active Directory (Azure AD).
Deze integratie biedt de volgende voordelen:

* U kunt Azure AD om te bepalen wie toegang tot InsideView heeft gebruiken.
* U kunt uw gebruikers kunnen automatisch worden aangemeld bij InsideView (eenmalige aanmelding) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts in één centrale locatie kunt beheren: de Azure-portal.

Zie [Eenmalige aanmelding voor toepassingen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) voor meer informatie over de integratie van SaaS-apps met Azure AD.

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met InsideView, moet u beschikken over:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/).
* Een abonnement InsideView met eenmalige aanmelding ingeschakeld.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie configureert en Azure AD eenmalige aanmelding testen in een testomgeving.

* InsideView biedt ondersteuning voor IdP gestart door SSO.

## <a name="add-insideview-from-the-gallery"></a>InsideView uit de galerie toevoegen

Als u de integratie van InsideView in Azure AD instelt, moet u InsideView uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster **Azure Active Directory**:

    ![Selecteer Azure Active Directory](common/select-azuread.png)

2. Ga naar **bedrijfstoepassingen** > **alle toepassingen**:

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u wilt een toepassing hebt toegevoegd, selecteert u **nieuwe toepassing** aan de bovenkant van het venster:

    ![Nieuwe toepassing selecteren](common/add-new-app.png)

4. Voer in het zoekvak **InsideView**. Selecteer **InsideView** in de zoekresultaten en selecteer vervolgens **toevoegen**.

    ![Zoekresultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureren en testen van Azure AD eenmalige aanmelding met InsideView met behulp van een testgebruiker met de naam Britta Simon.
Om in te schakelen eenmalige aanmelding, moet u een relatie tussen een Azure AD-gebruiker en de bijbehorende gebruiker in InsideView vast te stellen.

Als u wilt configureren en Azure AD eenmalige aanmelding met InsideView testen, moet u deze stappen:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  om in te schakelen van de functie voor uw gebruikers.
2. **[Configureren van eenmalige aanmelding InsideView](#configure-insideview-single-sign-on)**  aan de toepassing.
3. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  voor het testen van Azure AD eenmalige aanmelding.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  zodat Azure AD eenmalige aanmelding voor de gebruiker.
5. **[Maak een testgebruiker InsideView](#create-an-insideview-test-user)**  dat gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**  om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie schakelt u Azure AD eenmalige aanmelding in de Azure-portal.

Voor het configureren van Azure AD eenmalige aanmelding met InsideView, de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/), selecteert u op de pagina InsideView toepassingen integratie **eenmalige aanmelding**:

    ![Schakel eenmalige aanmelding](common/select-sso.png)

2. In de **selecteert u een methode voor eenmalige aanmelding** in het dialoogvenster, selecteer **SAML/WS-Federation** modus voor eenmalige aanmelding inschakelen:

    ![Selecteer een methode voor eenmalige aanmelding](common/select-saml-option.png)

3. Op de **instellen van eenmalige aanmelding met SAML** weergeeft, schakelt de **bewerken** pictogram opent de **SAML-basisconfiguratie** in het dialoogvenster:

    ![Pictogram bewerken](common/edit-urls.png)

4. In de **SAML-basisconfiguratie** dialoogvenster vak, voer de volgende stappen uit.

    ![In het dialoogvenster van Basic SAML-configuratie](common/idp-reply.png)

    In de **antwoord-URL** vak, een URL opgeven in dit patroon:

    `https://my.insideview.com/iv/<STS Name>/login.iv`

    > [!NOTE]
    > Deze waarde is een tijdelijke aanduiding. U moet de werkelijke antwoord-URL gebruiken. Neem contact op met de [InsideView ondersteuningsteam](mailto:support@insideview.com) om de waarde. U kunt ook verwijzen naar de patronen die wordt weergegeven in de **SAML-basisconfiguratie** in het dialoogvenster in de Azure-portal.

5. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, selecteer de **downloaden** koppelen naast **certificaat (Raw)** , overeenkomstig uw vereisten en sla het certificaat op uw computer:

    ![De koppeling om het certificaat te downloaden](common/certificateraw.png)

6. In de **InsideView instellen** sectie, kopieert u de juiste URL's, op basis van uw vereisten:

    ![De configuratie van URL's kopiëren](common/copy-configuration-urls.png)

    1. **Aanmeldings-URL**.

    1. **Azure AD Identifier**.

    1. **Afmeldings-URL van**.

### <a name="configure-insideview-single-sign-on"></a>InsideView eenmalige aanmelding configureren

1. In een nieuw browservenster aanmelden bij uw bedrijf InsideView site als een beheerder.

1. Selecteer aan de bovenkant van het venster **Admin**, **SingleSignOn instellingen**, en vervolgens **toevoegen SAML**.
   
   ![Eenmalige SAML-aanmelding instellingen](./media/insideview-tutorial/ic794135.png "eenmalige SAML-aanmelding instellingen")

1. In de **toevoegen van een nieuwe SAML** sectie, de volgende stappen uitvoeren.

    ![Toevoegen van een nieuwe SAML-sectie](./media/insideview-tutorial/ic794136.png "toevoegen van een nieuwe SAML-sectie")

    1. In de **STS Name** voert u een naam voor uw configuratie.

    1. In de **SamlP/WS-Federation ongevraagde eindpunt** vak, plak de **aanmeldings-URL** waarde die u hebt gekopieerd uit de Azure-portal.

    1. Open het certificaat met onbewerkte gegevens die u hebt gedownload van de Azure-portal. Kopieer de inhoud van het certificaat naar het Klembord en plak de inhoud in de **STS Certificate** vak.

    1. In de **Crm-Id Gebruikerstoewijzing** Voer **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.

    1. In de **Crm-e-mailbericht toewijzen** Voer **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.

    1. In de **Crm voornaam toewijzing** Voer **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.

    1. In de **Crm lastName toewijzing** Voer **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.  

    1. Selecteer **Opslaan**.

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

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding door haar toegang verlenen tot InsideView gebruiken.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**, en selecteer vervolgens **InsideView**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen **InsideView**.

    ![Lijst met toepassingen](common/all-applications.png)

3. Selecteer in het linkerdeelvenster **gebruikers en groepen**:

    ![Gebruikers en groepen selecteren](common/users-groups-blade.png)

4. Selecteer **Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Gebruiker toevoegen selecteren](common/add-assign-user.png)

5. In de **gebruikers en groepen** in het dialoogvenster, selecteer **Britta Simon** in de lijst met gebruikers, en klik op de **Selecteer** knop aan de onderkant van het venster.

6. Als u een waarde voor de rol in het SAML-verklaring verwacht in de **rol selecteren** dialoogvenster Selecteer de juiste rol voor de gebruiker in de lijst. Klik op de **Selecteer** knop aan de onderkant van het venster.

7. Selecteer **Toewijzen** in het dialoogvenster **Toewijzing toevoegen**.

### <a name="create-an-insideview-test-user"></a>Maak een testgebruiker InsideView

Als u wilt dat Azure AD-gebruikers kunnen zich aanmelden bij InsideView, moet u toe te voegen aan InsideView. U moet deze handmatig toevoegen.

Voor het maken van gebruikers en contactpersonen in InsideView, neem contact op met de [InsideView ondersteuningsteam](mailto:support@insideview.com).

> [!NOTE]
> U kunt een hulpprogramma voor het maken van gebruiker-account gebruiken of API wordt geleverd door InsideView voor het inrichten van gebruikersaccounts van de Azure AD.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

Nu moet u uw configuratie Azure AD eenmalige aanmelding testen met behulp van het toegangsvenster.

Wanneer u de tegel InsideView in het toegangsvenster selecteert, moet u worden automatisch aangemeld bij de InsideView-exemplaar waarvoor u eenmalige aanmelding hebt ingesteld. Zie voor meer informatie over het toegangsvenster, [toegang en gebruik apps op de portal mijn Apps](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Aanvullende resources

- [Zelfstudies voor het integreren van SaaS-toepassingen met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
