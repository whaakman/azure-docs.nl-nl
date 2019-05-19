---
title: 'Zelfstudie: Azure Active Directory-integratie met Signagelive | Microsoft Docs'
description: Ontdek hoe u eenmalige aanmelding configureert tussen Azure Active Directory en Signagelive.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: d923f0e7-ad31-4d59-a6fd-f0e895e1a32d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 1/11/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: bff57635c87b77270e1bd20e04834dec132b2df6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "65867306"
---
# <a name="tutorial-azure-active-directory-integration-with-signagelive"></a>Zelfstudie: Azure Active Directory-integratie met Signagelive

In deze zelfstudie leert u hoe u Signagelive kunt integreren met Azure AD (Azure Active Directory).
Integratie van Signagelive met Azure AD biedt u de volgende voordelen:

* U kunt in Azure AD beheren wie toegang heeft tot Signagelive.
* U kunt uw gebruikers kunnen automatisch worden aangemeld bij Signagelive (eenmalige aanmelding) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts in één centrale locatie kunt beheren: de Azure-portal.

Zie voor meer informatie over de integratie met Azure AD SaaS [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis). Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

U hebt de volgende items nodig om Azure AD-integratie met Signagelive te configureren:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).
* Een abonnement Signagelive single sign-op-is ingeschakeld.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Signagelive biedt ondersteuning voor Serviceprovider geïnitieerde eenmalige aanmelding.

## <a name="add-signagelive-from-the-gallery"></a>Signagelive uit de galerie toevoegen

Het configureren van de integratie van Signagelive in Azure AD, eerst Signagelive uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

Als u wilt toevoegen Signagelive uit de galerie, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster de **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Ga naar **bedrijfstoepassingen**, en selecteer vervolgens de **alle toepassingen** optie.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u wilt een nieuwe toepassing toevoegen, selecteert u de **nieuwe toepassing** knop aan de bovenkant van het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Voer in het zoekvak **Signagelive**. 

     ![Signagelive in de resultatenlijst](common/search-new-app.png)

5. Selecteer **Signagelive** vanuit het deelvenster met resultaten en selecteer vervolgens de **toevoegen** om toe te voegen van de toepassing.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie configureert en test u Azure AD-eenmalige aanmelding met Signagelive op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet u een koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker maken in Signagelive.

Als u wilt configureren en Azure AD eenmalige aanmelding met Signagelive testen, voert u eerst de volgende bouwstenen:

1. [Eenmalige aanmelding van Azure AD configureren](#configure-azure-ad-single-sign-on) zodat uw gebruikers deze functie kunnen gebruiken.
2. [Configureren van eenmalige aanmelding Signagelive](#configure-signagelive-single-sign-on) de instellingen voor eenmalige aanmelding configureren aan de toepassing.
3. [Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user) voor het testen van eenmalige aanmelding van Azure AD met Britta Simon.
4. [De Azure AD-testgebruiker toewijzen](#assign-the-azure-ad-test-user) zodat Britta Simon gebruik kan maken van eenmalige aanmelding van Azure AD.
5. [Maak een testgebruiker Signagelive](#create-a-signagelive-test-user) hebben een equivalent van Britta Simon in Signagelive die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. [Eenmalige aanmelding testen](#test-single-sign-on) om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met Signagelive, voert u de volgende stappen uit:

1. Selecteer in de [Azure-portal](https://portal.azure.com/) op de pagina voor integratie van toepassingen met **Signagelive** de optie **Eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In de **selecteert u een methode voor eenmalige aanmelding** in het dialoogvenster, selecteer **SAML** eenmalige aanmelding inschakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de **instellen van eenmalige aanmelding met SAML** weergeeft, schakelt **bewerken** openen de **SAML-basisconfiguratie** in het dialoogvenster.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de **SAML-basisconfiguratie** sectie, de volgende stappen uit:

    ![Informatie over eenmalige aanmelding bij Signagelive-domeinen en -URL's](common/sp-signonurl.png)

    In de **aanmeldings-URL** voert u een URL die gebruikmaakt van het volgende patroon:  `https://login.signagelive.com/sso/<ORGANIZATIONALUNITNAME>`

    > [!NOTE]
    > De waarde is niet echt. Werk de waarde bij met de werkelijke aanmeldings-URL. Als u de waarde, neem contact op met de [Signagelive Client ondersteuningsteam](mailto:support@signagelive.com) . U kunt ook verwijzen naar de patronen die worden weergegeven in de **SAML-basisconfiguratie** sectie in Azure portal.

5. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, selecteer **downloaden** voor het downloaden van de **certificaat (Raw)** uit de opgegeven opties per uw behoeften. Sla deze op uw computer.

    ![De link om het certificaat te downloaden](common/certificateraw.png)

6. In de **Signagelive instellen** sectie, Kopieer de URL's die u nodig hebt.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-signagelive-single-sign-on"></a>Signagelive voor eenmalige aanmelding configureren

Voor het configureren van eenmalige aanmelding aan de Signagelive, verzendt de gedownloade **certificaat (Raw)** en URL's gekopieerd vanuit Azure portal naar de [Signagelive ondersteuningsteam](mailto:support@signagelive.com). Ze zorgen ervoor dat de SAML SSO-verbinding aan beide zijden correct is ingesteld.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken 

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![De knop Nieuwe gebruiker](common/new-user.png)

3. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon** in.
  
    b. In de **gebruikersnaam** Voer 'brittasimon@yourcompanydomain.extension'. Bijvoorbeeld: in dit geval, u kunt invoeren 'BrittaSimon@contoso.com'.

    c. Selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in het wachtwoord.

    d. Selecteer **Maken**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie geeft u Britta Simon toestemming voor gebruik van eenmalige aanmelding met Azure door haar toegang te geven tot Signagelive.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**, en selecteer vervolgens **Signagelive**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen de optie **Signagelive**.

    ![De Signagelive-koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Selecteer de **gebruiker toevoegen** knop. Klik in de **toevoegen toewijzing** in het dialoogvenster, selecteer **gebruikers en groepen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. In de **gebruikers en groepen** in het dialoogvenster de **gebruikers** in de lijst met **Britta Simon**. Klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.

6. Als u een waarde voor de rol verwacht in het SAML-verklaring vervolgens, in de **rol selecteren** dialoogvenster Selecteer de juiste rol voor de gebruiker in de lijst. Klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.

7. In de **toevoegen toewijzing** in het dialoogvenster, selecteer de **toewijzen** knop.

### <a name="create-a-signagelive-test-user"></a>Maak een testgebruiker Signagelive

In deze sectie maakt u een gebruiker in Signagelive met de naam Britta Simon. Werken met de [Signagelive ondersteuningsteam](mailto:support@signagelive.com) om toe te voegen de gebruikers in het Signagelive-platform. U moet maken en gebruikers te activeren voordat u eenmalige aanmelding.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie kunt u uw configuratie Azure AD eenmalige aanmelding testen met behulp van de MyApps-portal.

Wanneer u selecteert de **Signagelive** tegel in de MyApps-portal, u moet worden automatisch aangemeld. Zie voor meer informatie over de MyApps-portal, [wat is de MyApps-portal?](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

