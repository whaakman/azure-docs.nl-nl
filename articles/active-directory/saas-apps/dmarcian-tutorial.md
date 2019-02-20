---
title: 'Zelfstudie: Azure Active Directory-integratie met dmarcian | Microsoft Docs'
description: Ontdek hoe u eenmalige aanmelding configureert tussen Azure Active Directory en dmarcian.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: a04b9383-3a60-4d54-9412-123daaddff3b
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/30/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4768c38b2e76bfa8cdff3187c32c03fdaaf57bbf
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56207650"
---
# <a name="tutorial-azure-active-directory-integration-with-dmarcian"></a>Zelfstudie: Azure Active Directory-integratie met dmarcian

In deze zelfstudie leert u hoe u dmarcian kunt integreren met Azure Active Directory (Azure AD).
De integratie van dmarcian met Azure AD biedt de volgende voordelen:

* U kunt in Azure AD bepalen wie er toegang heeft tot dmarcian.
* U kunt instellen dat gebruikers automatisch met hun Azure AD-account worden aangemeld bij dmarcian (eenmalige aanmelding).
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Om Azure AD-integratie te configureren met dmarcian hebt u het volgende nodig:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Een abonnement op dmarcian waarvoor eenmalige aanmelding is ingeschakeld

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* dmarcian ondersteunt door **SP** en **IDP** geïnitieerde eenmalige aanmelding

## <a name="adding-dmarcian-from-the-gallery"></a>Dmarcian vanuit de galerie toevoegen

Om de integratie van dmarcian in Azure AD te configureren, moet u dmarcian vanuit de galerij toevoegen aan uw lijst van beheerde SaaS-apps.

**Om dmarcian vanuit de galerij toe te voegen, voert u de volgende stappen uit:**

1. Klik in het linkernavigatievenster in de **[Azure-portal](https://portal.azure.com)** op het **Azure Active Directory**-pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **dmarcian**, selecteer **dmarcian** in het resultaatvenster en klik vervolgens op de knop **Toevoegen** om de toepassing toe te voegen.

     ![dmarcian in de resultatenlijst](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie gaat u Azure AD-eenmalige aanmelding bij dmarcian configureren en testen op basis van een testgebruiker met de naam **Britta Simon**.
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Azure AD-gebruiker en de daaraan gerelateerde gebruiker in dmarcian tot stand is gebracht.

Om Azure AD-eenmalige aanmelding bij dmarcian te configureren en te testen, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Eenmalige aanmelding bij dmarcian configureren](#configure-dmarcian-single-sign-on)** - de instellingen voor eenmalige aanmelding aan de toepassingszijde configureren.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Testgebruiker voor dmarcian maken](#create-dmarcian-test-user)** - als u een tegenhanger van Britta Simon in dmarcian wilt hebben die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voer de volgende stappen uit als u Azure AD-eenmalige aanmelding bij dmarcian wilt configureren:

1. In de [Azure-portal](https://portal.azure.com/) selecteert u **Eenmalige aanmelding** op de integratiepagina van de toepassing **dmarcian**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit als u de toepassing in de door **IDP** geïnitieerde modus wilt configureren:

    ![Domein- en URL-gegevens voor eenmalige aanmelding bij dmarcian](common/idp-intiated.png)

    a. In het tekstvak **Id** typt u een URL met het volgende patroon:
    | |
    | -- |
    | `https://us.dmarcian.com/sso/saml/<ACCOUNT_ID>/sp.xml` |
    | `https://dmarcian-eu.com/sso/saml/<ACCOUNT_ID>/sp.xml` |
    | `https://dmarcian-ap.com/sso/saml/<ACCOUNT_ID>/sp.xml` |

    b. In het tekstvak **Antwoord-URL** typt u een URL met het volgende patroon:
    | |
    |--|
    | `https://us.dmarcian.com/login/<ACCOUNT_ID>/handle/` |
    | `https://dmarcian-eu.com/login/<ACCOUNT_ID>/handle/` |
    | `https://dmarcian-ap.com/login/<ACCOUNT_ID>/handle/` |

5. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    ![Domein- en URL-gegevens voor eenmalige aanmelding bij dmarcian](common/metadata-upload-additional-signon.png)

    In het tekstvak **Aanmeldings-URL** typt u een URL met het volgende patroon:
    | |
    |--|
    | `https://us.dmarcian.com/login/<ACCOUNT_ID>` |
    | `https://dmarcian-eu.com/login/<ACCOUNT_ID>` |
    | `https://dmarciam-ap.com/login/<ACCOUNT_ID>` |
     
    > [!NOTE] 
    > Dit zijn geen echte waarden. U gaat deze waarden bijwerken met de daadwerkelijke id, antwoord-URL en URL voor eenmalige aanmelding, wat later in de zelfstudie nog wordt uitgelegd. 

6. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op de kopieerknop om de **URL voor federatieve metagegevens van de app** te kopiëren en slaat u deze op uw computer op.

    ![De link om het certificaat te downloaden](common/copy-metadataurl.png)

### <a name="configure-dmarcian-single-sign-on"></a>Eenmalige aanmelding bij dmarcian configureren

1. Meld u in een ander browservenster bij dmarcian aan als een beveiligingsbeheerder.

2. Klik op **Profiel** in de rechterbovenhoek hoek en navigeer naar **Voorkeuren**.

    ![Voorkeuren ](./media/dmarcian-tutorial/tutorial_dmarcian_pref.png)

3. Schuif omlaag en klik op de sectie **Eenmalige aanmelding** en klik vervolgens op **Configureren**.

    ![Eenmalig ](./media/dmarcian-tutorial/tutorial_dmarcian_sso.png)

4. Zet op de **Eenmalige aanmelding op basis van SAML** de **Status** op **Ingeschakeld** en voer de volgende stappen uit:

    ![Verificatie ](./media/dmarcian-tutorial/tutorial_dmarcian_auth.png)

    * In de sectie **dmarcian aan uw id-provider toevoegen** klikt u op **Kopiëren** om de **ACS-URL** voor uw exemplaar te kopiëren, en plak deze in het tekstvak **Antwoord-URL** in de sectie **SAML-basisconfiguratie** in de Azure-portal.

    * In de sectie **dmarcian aan uw id-provider toevoegen** klikt u op **Kopiëren** om de **Entiteits-id** voor uw exemplaar te kopiëren, en plak deze in het tekstvak **Id** in de sectie **SAML-basisconfiguratie** in de Azure-portal.

    * In de sectie **Verificatie instellen** plakt u in het testvak **Metadata id-provider** de **App-URL voor federatieve metagegevens** die u uit de Azure-portal hebt gekopieerd.

    * Onder de sectie **Verificatie instellen** plakt u in het tekstvak **Kenmerkinstructies** tekstvak de URL `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

    * In de sectie **Aanmeldings-URL instellen** kopieert u de **aanmeldings-URL** voor uw exemplaar en plak deze in het tekstvak **Aanmeldings-URL** in de sectie **SAML-basisconfiguratie** in de Azure-portal.

        > [!Note]
        > U kunt de **aanmeldings-URL** aanpassen in overeenstemming met uw organisatie.

    * Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken 

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam****Britta Simon** in.
  
    b. In het veld **Gebruikersnaam** typt u **brittasimon@yourcompanydomain.extension**.  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In dit gedeelte gaat u Britta Simon toestemming geven voor gebruik van eenmalige aanmelding met Azure door haar toegang te geven tot dmarcian.

1. Selecteer **Bedrijfstoepassingen** in de Azure-portal, selecteer **Alle toepassingen** en selecteer vervolgens **dmarcian**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer **dmarcian** in de lijst met toepassingen.

    ![dmarcian-koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst Gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-dmarcian-test-user"></a>Testgebruiker voor dmarcian maken

Als u wilt dat Azure AD-gebruikers zich kunnen aanmelden bij dmarcian, moeten ze worden ingericht in dmarcian. In het geval van dmarcian is dat een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Meld u bij dmarcian aan als een beveiligingsbeheerder.

2. Klik op **Profiel** in de rechterbovenhoek hoek en navigeer naar **Gebruiker beheren**.

    ![Gebruiker ](./media/dmarcian-tutorial/tutorial_dmarcian_user.png)

3. Aan de rechterkant van de sectie **SSO-gebruikers** klikt u op **Nieuwe gebruiker toevoegen**.

    ![Gebruiker toevoegen ](./media/dmarcian-tutorial/tutorial_dmarcian_addnewuser.png)

4. Voer in de pop-up **Nieuwe gebruiker toevoegen** de volgende stappen uit:

    ![Nieuwe gebruiker ](./media/dmarcian-tutorial/tutorial_dmarcian_save.png)

    a. Voer in het tekstvak **Email nieuwe gebruiker** het e-mailadres van de gebruiker in, bijvoorbeeld **brittasimon@contoso.com**.

    b. Als u beheerdersrechten aan de gebruiker wilt verlenen, selecteert u **Gebruiker beheerder maken**.

    c. Klik op **Add User**.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie gaat u uw configuratie van Azure AD-eenmalige aanmelding testen via het toegangsvenster.

Wanneer u op de tegel dmarcian in het toegangsvenster klikt, wordt u automatisch aangemeld bij de instantie van dmarcian waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

