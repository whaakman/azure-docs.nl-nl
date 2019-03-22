---
title: "Zelfstudie: Azure Active Directory-integratie met N2F - onkostennota's | Microsoft Docs"
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en N2F - onkostennota's.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: f56d53d7-5a08-490a-bfb9-78fefc2751ec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/01/2019
ms.author: jeedes
ms.openlocfilehash: 2851383ed992b314af541b24e7753b1e6b12b28d
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57898666"
---
# <a name="tutorial-azure-active-directory-integration-with-n2f---expense-reports"></a>Zelfstudie: Azure Active Directory-integratie met N2F - onkostennota 's

In deze zelfstudie leert u hoe u integreert N2F - onkostennota's met Azure Active Directory (Azure AD).
Integratie N2F - onkostennota's met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot N2F heeft - 's onkostennota.
* U kunt uw gebruikers worden automatisch aangemeld N2F - onkostennota's (Single Sign-On) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met N2F - onkostennota's, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* N2F - onkostennota eenmalige aanmelding ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* N2F - onkostennota's ondersteunt **SP** en **IDP** gestart door SSO

## <a name="adding-n2f---expense-reports-from-the-gallery"></a>N2F - onkostennota's uit de galerie toevoegen

Als u wilt configureren van de integratie van N2F - onkostennota's in Azure AD, moet u toevoegen N2F - onkostennota's uit de galerie aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen, N2F - onkostennota's uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **N2F - onkostennota**, selecteer **N2F - onkostennota** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![N2F - onkostennota's in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met N2F - onkostennota's op basis van een testgebruiker met de naam **Britta Simon**.
Onkostennota's voor single sign-on bij werk-, is de relatie van een koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in N2F - behoeften tot stand is gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met N2F - onkostennota's, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[N2F configureren - onkostennota's Single Sign-On](#configure-n2f---expense-reports-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maken van N2F - onkostennota testgebruiker](#create-n2f---expense-reports-test-user)**  - dat een equivalent van Britta Simon in N2F - onkostennota's dat is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD Voer eenmalige aanmelding met N2F - onkostennota's, de volgende stappen uit:

1. In de [Azure-portal](https://portal.azure.com/)op de **N2F - onkostennota** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. Op de **SAML-basisconfiguratie** sectie, als u wilt configureren van de toepassing in **IDP** gestart modus wordt de gebruiker hoeft niet te werk als de app al vooraf geïntegreerd in Azure is.

    ![N2F - onkostennota domein en URL's, eenmalige aanmelding informatie](common/preintegrated.png)

5. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    ![N2F - onkostennota domein en URL's, eenmalige aanmelding informatie](common/metadata-upload-additional-signon.png)

    In het tekstvak **Aanmeldings-URL** typt u een URL: `https://www.n2f.com/app/`

6. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op de kopieerknop om de **URL voor federatieve metagegevens van de app** te kopiëren en slaat u deze op uw computer op.

    ![De link om het certificaat te downloaden](common/copy-metadataurl.png)

7. Op de **myPolicies instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-n2f---expense-reports-single-sign-on"></a>N2F - onkostennota's, eenmalige aanmelding configureren

1. In een ander browservenster, moet u zich aanmelden bij uw N2F - onkosten rapporten bedrijf site als beheerder.

2. Klik op **instellingen** en selecteer vervolgens **geavanceerde instellingen** in de vervolgkeuzelijst.

    ![N2F - onkostennota configuratie](./media/n2f-expensereports-tutorial/configure1.png)

3. Selecteer **Accountinstellingen** tabblad.

    ![N2F - onkostennota configuratie](./media/n2f-expensereports-tutorial/configure2.png)

4. Selecteer **verificatie** en selecteer vervolgens **+ toevoegen van een verificatiemethode** tabblad.

    ![N2F - onkostennota configuratie](./media/n2f-expensereports-tutorial/configure3.png)

5. Selecteer **SAML Microsoft Office 365** als verificatiemethode.

    ![N2F - onkostennota configuratie](./media/n2f-expensereports-tutorial/configure4.png)

6. Op de **verificatiemethode** sectie, voert u de volgende stappen uit:

    ![N2F - onkostennota configuratie](./media/n2f-expensereports-tutorial/configure5.png)

    a. In de **entiteit-ID** tekstvak, plak de **Azure AD-id** waarde die u hebt gekopieerd vanuit Azure portal.

    b. In de **metagegevens-URL** tekstvak, plak de **App-Url voor federatieve metagegevens** waarde die u hebt gekopieerd vanuit Azure portal.

    c. Klik op **Opslaan**.

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

In deze sectie maakt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan N2F inschakelen - onkostennota's.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **N2F - onkostennota**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **N2F - onkostennota**.

    ![De N2F - onkostennota's koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-n2f---expense-reports-test-user"></a>Maken van N2F - onkostennota gebruiker testen

Om Azure AD-gebruikers zich aanmelden bij N2F - onkostennota's, moeten ze worden ingericht voor N2F - onkostennota's. In het geval van N2F - onkostennota's, is inrichten een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Meld u aan bij uw N2F - onkosten rapporten bedrijf site als beheerder.

2. Klik op **instellingen** en selecteer vervolgens **geavanceerde instellingen** in de vervolgkeuzelijst.

    ![N2F - gebruiker onkosten toevoegen](./media/n2f-expensereports-tutorial/configure1.png)

3. Selecteer **gebruikers** tabblad in het linkernavigatievenster.

    ![N2F - onkostennota configuratie](./media/n2f-expensereports-tutorial/user1.png)

4. Selecteer **+ nieuwe gebruiker** tabblad.

    ![N2F - onkostennota configuratie](./media/n2f-expensereports-tutorial/user2.png)

5. Op de **gebruiker** sectie, voert u de volgende stappen uit:

    ![N2F - onkostennota configuratie](./media/n2f-expensereports-tutorial/user3.png)

    a. In de **e-mailadres** tekstvak, voer het e-mailadres van gebruiker, zoals **brittasimon\@contoso.com**.

    b. Voer in het tekstvak **First name** de voornaam van de gebruiker in, zoals **Britta**.

    c. Voer in het tekstvak **Volledige naam** de volledige naam van de gebruiker in, bijvoorbeeld **Britta Simon**.

    d. Kies **rol (N + 1) de directe manager**, en **deling** volgens de vereisten van uw organisatie.

    e. Klik op **valideren en te verzenden uitnodiging**.

    > [!NOTE]
    > Als u problemen ondervindt tijdens het toevoegen van de gebruiker, contact op met [N2F - ondersteuningsteam onkostennota](mailto:support@n2f.com)

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u klikt op de N2F - onkostennota tegel in het toegangsvenster, moet u worden automatisch aangemeld bij de N2F - onkostennota's waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

