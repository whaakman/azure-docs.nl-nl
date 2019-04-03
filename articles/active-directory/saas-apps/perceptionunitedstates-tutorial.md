---
title: 'Zelfstudie: Azure Active Directory-integratie met perceptie Verenigde Staten (niet-UltiPro) | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en perceptie Verenigde Staten (niet-UltiPro).
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: b4a8f026-cb5f-41eb-9680-68eddc33565e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/25/2019
ms.author: jeedes
ms.openlocfilehash: 31528c30ac8b795a661539349c2bee31edb6973c
ms.sourcegitcommit: 04716e13cc2ab69da57d61819da6cd5508f8c422
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58850982"
---
# <a name="tutorial-azure-active-directory-integration-with-perception-united-states-non-ultipro"></a>Zelfstudie: Azure Active Directory-integratie met perceptie Verenigde Staten (niet-UltiPro)

In deze zelfstudie leert u hoe u perceptie Verenigde Staten (niet-UltiPro) integreren met Azure Active Directory (Azure AD).
Perceptie Verenigde Staten (niet-UltiPro) integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang hebben naar perceptie Verenigde Staten (niet-UltiPro).
* U kunt uw gebruikers worden automatisch aangemeld op perceptie Verenigde Staten (niet-UltiPro) (Single Sign-On) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met perceptie Verenigde Staten (niet-UltiPro), moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Perceptie Verenigde Staten (niet-UltiPro) eenmalige aanmelding ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Perceptie Verenigde Staten (niet-UltiPro) ondersteunt **IDP** gestart door SSO

## <a name="adding-perception-united-states-non-ultipro-from-the-gallery"></a>Perceptie Verenigde Staten (niet-UltiPro) uit de galerie toe te voegen

Als u wilt de integratie van perceptie Verenigde Staten (niet-UltiPro) in Azure AD configureert, moet u perceptie Verenigde Staten (niet-UltiPro) toevoegen vanuit de galerie aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen perceptie Verenigde Staten (niet-UltiPro) uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **perceptie Verenigde Staten (niet-UltiPro)**, selecteer **perceptie Verenigde Staten (niet-UltiPro)** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen de de toepassing.

     ![Perceptie Verenigde Staten (niet-UltiPro) in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met perceptie Verenigde Staten (niet-UltiPro) op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in de perceptie Verenigde Staten (niet-UltiPro) tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met perceptie Verenigde Staten (niet-UltiPro), moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureren van eenmalige aanmelding perceptie Verenigde Staten (niet-UltiPro)](#configure-perception-united-states-non-ultipro-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Perceptie Verenigde Staten (niet-UltiPro) testgebruiker maken](#create-perception-united-states-non-ultipro-test-user)**  - hebben een equivalent van Britta Simon in perceptie Verenigde Staten (niet-UltiPro) die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met perceptie Verenigde Staten (niet-UltiPro), moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **perceptie Verenigde Staten (niet-UltiPro)** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. Op de pagina **Eenmalige aanmelding instellen met SAML** voert u de volgende stappen uit:

    ![Perceptie Verenigde Staten (niet-UltiPro)-domein en URL's eenmalige aanmelding informatie](common/idp-intiated.png)

    a. In het tekstvak **Id** typt u een URL: `https://perception.kanjoya.com/sp`

    b. In het tekstvak **Antwoord-URL** typt u een URL met de volgende notatie: `https://perception.kanjoya.com/sso?idp=<entity_id>`

    c. De **perceptie Verenigde Staten (niet-UltiPro)** toepassing vereist de **Azure AD-id** waarde als < entity_id >, u van ontvangt de **perceptie Verenigde Staten (instellen Non-UltiPro)** sectie URI-codering krijgen. Als u de uri-gecodeerde waarde, gebruikt u de volgende koppeling: **http://www.url-encode-decode.com/**.

    d. Nadat u de uri gecodeerde waarde combineren met de **antwoord-URL** zoals vermeld hieronder:

    `https://perception.kanjoya.com/sso?idp=<URI encooded entity_id>`
    
    e. Plak de bovenstaande waarde in de **antwoord-URL** tekstvak.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

6. Op de **van perceptie Verenigde Staten (niet-UltiPro) ingesteld** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL   

### <a name="configure-perception-united-states-non-ultipro-single-sign-on"></a>Perceptie Verenigde Staten (niet-UltiPro) eenmalige aanmelding configureren

1. In een ander browservenster, meld u aan bij uw bedrijf perceptie Verenigde Staten (niet-UltiPro) site als beheerder.

2. Klik in de belangrijkste werkbalk **Accountinstellingen**.

    ![Perceptie Verenigde Staten (niet-UltiPro) gebruiker](./media/perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_user.png)

3. Op de **Accountinstellingen** pagina, voert u de volgende stappen uit:

    ![Perceptie Verenigde Staten (niet-UltiPro) gebruiker](./media/perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_account.png)

    a. In de **bedrijfsnaam** tekstvak, typ de naam van de **bedrijf**.
    
    b. In de **accountnaam** tekstvak, typ de naam van de **Account**.

    c. In **standaard antwoordadres e** tekst typt u de geldige **e**.

    d. Selecteer **id-Provider voor eenmalige aanmelding** als **SAML 2.0**.

4. Op de **SSO-configuratie** pagina, voert u de volgende stappen uit:

    ![Perceptie Verenigde Staten (niet-UltiPro) SSOConfig](./media/perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_ssoconfig.png)

    a. Selecteer **SAML NameID Type** als **e**.

    b. In de **SSO configuratienaam** tekstvak, typ de naam van uw **configuratie**.
    
    c. In **identiteit providernaam** tekstvak, plak de waarde van **Azure AD-id**, die u hebt gekopieerd vanuit Azure portal. 

    d. In **SAML domein tekstvak**, voert u het domein, zoals @contoso.com.

    e. Klik op **opnieuw uploaden** het uploaden van de **Metadata XML** bestand.

    f. Klik op **Update**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken 

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In de **gebruikersnaam** veldtype brittasimon@yourcompanydomain.extension. Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan perceptie Verenigde Staten (niet-UltiPro).

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **perceptie Verenigde Staten (niet-UltiPro)**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **perceptie Verenigde Staten (niet-UltiPro)**.

    ![De koppeling perceptie Verenigde Staten (niet-UltiPro) in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-perception-united-states-non-ultipro-test-user"></a>Perceptie Verenigde Staten (niet-UltiPro) testgebruiker maken

In deze sectie maakt u een gebruiker met de naam Britta Simon in perceptie Verenigde Staten (niet-UltiPro). Werken met [perceptie Verenigde Staten (niet-UltiPro)-ondersteuningsteam](https://www.ultimatesoftware.com/Contact/ContactUs) om toe te voegen de gebruikers in het platform, Perception Verenigde Staten (niet-UltiPro).

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel perceptie Verenigde Staten (niet-UltiPro) in het toegangsvenster, moet u worden automatisch aangemeld op de perceptie Verenigde Staten (niet-UltiPro) waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

