---
title: 'Zelfstudie: Azure Active Directory-integratie met Velpic SAML | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Velpic SAML.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/03/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: bc8431e2c45cc3bfdfa08dd0078220ab2d290309
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2019
ms.locfileid: "65905402"
---
# <a name="tutorial-azure-active-directory-integration-with-velpic-saml"></a>Zelfstudie: Azure Active Directory-integratie met Velpic SAML

In deze zelfstudie leert u hoe u Velpic SAML integreren met Azure Active Directory (Azure AD).
Velpic SAML integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot Velpic SAML heeft.
* U kunt uw gebruikers worden automatisch aangemeld Velpic SAML (Single Sign-On) inschakelen met hun Azure AD-accounts.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Velpic SAML, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* Eenmalige aanmelding Velpic SAML ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor Velpic SAML **SP** gestart door SSO

## <a name="adding-velpic-saml-from-the-gallery"></a>Velpic SAML vanuit de galerie toevoegen

Voor het configureren van de integratie van Velpic SAML in Azure AD, moet u Velpic SAML vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Velpic SAML vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **Velpic SAML**, selecteer **Velpic SAML** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Velpic SAML in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Velpic SAML op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in SAML-Velpic tot stand worden gebracht.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Velpic SAML, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureer Velpic SAML Single Sign-On](#configure-velpic-saml-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maak de testgebruiker Velpic SAML](#create-velpic-saml-test-user)**  : als u wilt een equivalent van Britta Simon in Velpic SAML die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met Velpic SAML, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **Velpic SAML** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Velpic SAML-domein en URL's, eenmalige aanmelding informatie](common/sp-identifier.png)

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<sub-domain>.velpicsaml.net`

    b. In het tekstvak **Id (Entiteits-id)** typt u een URL met de volgende notatie: `https://auth.velpic.com/saml/v2/<entity-id>/login`

    > [!NOTE]
    > Houd er rekening mee dat de aanmeldings-URL wordt geleverd door het Velpic SAML-team en id-waarde beschikbaar is bij het configureren van de invoegtoepassing SSO Velpic SAML aan. U moet deze waarde van de toepassingspagina Velpic SAML kopiëren en plak deze hier.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

6. Op de **Velpic SAML instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-velpic-saml-single-sign-on"></a>Configureer SAML Velpic Single Sign-On

1. In een ander browservenster, meld u aan bij uw bedrijf Velpic SAML site als beheerder.

2. Klik op **beheren** tabblad en Ga naar **integratie** sectie waar u nodig hebt op **invoegtoepassingen** om te maken van nieuwe invoegtoepassing voor aanmelding bij.

    ![Invoegtoepassing](./media/velpicsaml-tutorial/velpic_1.png)

3. Klik op de **'Add invoegtoepassing'** knop.
    
    ![Invoegtoepassing](./media/velpicsaml-tutorial/velpic_2.png)

4. Klik op de **SAML** tegel op de pagina toevoegen-invoegtoepassing.
    
    ![Invoegtoepassing](./media/velpicsaml-tutorial/velpic_3.png)

5. Voer de naam van de nieuwe SAML-invoegtoepassing en klik op de **'Toevoegen'** knop.

    ![Invoegtoepassing](./media/velpicsaml-tutorial/velpic_4.png)

6. Voer de gegevens als volgt:

    ![Invoegtoepassing](./media/velpicsaml-tutorial/velpic_5.png)

    a. In de **naam** tekstvak, typ de naam van SAML-invoegtoepassing.

    b. In de **URL-verlener** tekstvak, plak de **Azure AD-id** u gekopieerd uit de **aanmelding configureren** venster van de Azure-portal.

    c. In de **Provider metagegevens Config** uploaden van de Metadata-XML-bestand dat u hebt gedownload van Azure portal.

    d. U kunt er ook voor kiezen om in te schakelen van SAML just-in-time inrichting door in te schakelen de **'Automatisch maken van nieuwe gebruikers'** selectievakje. Als een gebruiker niet in Velpic bestaat en deze markering niet is ingeschakeld, mislukt de aanmelding van Azure. Als de vlag is ingeschakeld. de gebruiker automatisch worden ingericht in Velpic op het moment van de aanmelding. 

    e. Kopieer de **eenmalige aanmelding op URL** uit de tekst vak en plak deze in Azure portal.
    
    f. Klik op **Opslaan**.

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

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Velpic SAML.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **Velpic SAML**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **Velpic SAML**.

    ![De Velpic SAML-koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-velpic-saml-test-user"></a>Velpic SAML testgebruiker maken

Deze stap is doorgaans niet vereist als de toepassing just-in-tijd van gebruikersinrichting ondersteunt. Als het automatisch inrichten van gebruikers niet is ingeschakeld kan vervolgens handmatig maken worden uitgevoerd zoals hieronder wordt beschreven.

Meld u aan bij uw site van het bedrijf Velpic SAML als beheerder en voert u de volgende stappen uit:
    
1. Klik op het tabblad beheren en gaat u naar de sectie gebruikers, en klik vervolgens op de knop Nieuwe gebruikers toe te voegen.

    ![Gebruiker toevoegen](./media/velpicsaml-tutorial/velpic_7.png)

2. Op de **'Nieuwe gebruiker maken'** dialoogvenster pagina, de volgende stappen uitvoeren.

    ![user](./media/velpicsaml-tutorial/velpic_8.png)
    
    a. In de **voornaam** tekstvak, de eerste naam van Julia.

    b. In de **achternaam** tekstvak typt u de achternaam van Simon.

    c. In de **gebruikersnaam** tekstvak, typ de naam van de gebruiker van Britta Simon.

    d. In de **e** tekstvak typt u het e-mailadres van Brittasimon@contoso.com account.

    e. De rest van de informatie is optioneel en u deze indien nodig kunt invullen.
    
    f. Klik op **OPSLAAN**.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

1. Als u de Velpic SAML-tegel in het toegangsvenster klikt, krijgt u de aanmeldingspagina van Velpic SAML-toepassing. U ziet de **'Aanmelden met Azure AD'** knop op de aanmeldingspagina.

    ![Invoegtoepassing](./media/velpicsaml-tutorial/velpic_6.png)

1. Klik op de **'Aanmelden met Azure AD'** knop aanmelden bij Velpic met behulp van uw Azure AD-account.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

