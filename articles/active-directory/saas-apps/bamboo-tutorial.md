---
title: 'Zelfstudie: Azure Active Directory-integratie met SAML SSO voor Bamboe resolutie GmbH | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en SAML SSO voor Bamboe resolutie GmbH.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: f00160c7-f4cc-43bf-af18-f04168d3767c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/16/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 97c13469c9c70aec31314048b8971c66bae5cd49
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/22/2019
ms.locfileid: "60004717"
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-bamboo-by-resolution-gmbh"></a>Zelfstudie: Azure Active Directory-integratie met SAML SSO voor Bamboe resolutie GmbH

In deze zelfstudie leert u over het integreren van SAML SSO voor Bamboe resolutie GmbH met Azure Active Directory (Azure AD).
Integratie van SAML SSO voor Bamboe resolutie GmbH met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot SAML SSO voor Bamboe resolutie GmbH heeft.
* U kunt uw gebruikers worden automatisch aangemeld SAML SSO voor Bamboe resolutie GmbH (Single Sign-On) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met SAML SSO voor Bamboe resolutie GmbH, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* SAML SSO voor Bamboe resolutie GmbH eenmalige aanmelding ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* SAML SSO voor Bamboe door de oplossing biedt ondersteuning voor GmbH **SP en IDP** gestart door SSO
* SAML SSO voor Bamboe door de oplossing biedt ondersteuning voor GmbH **Just In Time** inrichten van gebruikers

## <a name="adding-saml-sso-for-bamboo-by-resolution-gmbh-from-the-gallery"></a>Toevoegen van SAML SSO voor Bamboe resolutie GmbH uit de galerie

Voor het configureren van de integratie van SAML SSO voor Bamboe resolutie GmbH in Azure AD, moet u de SAML SSO voor Bamboe resolutie GmbH uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**SAML SSO voor Bamboe met resolutie GmbH uit de galerie toevoegen, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **SAML SSO voor Bamboe resolutie GmbH**, selecteer **SAML SSO voor Bamboe resolutie GmbH** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen de de toepassing.

    ![SAML SSO voor Bamboe door oplossing GmbH in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met SAML SSO voor Bamboe resolutie GmbH op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in de SAML SSO voor Bamboe resolutie GmbH tot stand is gebracht.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met SAML SSO voor Bamboe resolutie GmbH, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[SAML SSO voor Bamboe configureren door de resolutie GmbH Single Sign-On](#configure-saml-sso-for-bamboo-by-resolution-gmbh-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maken van de SAML SSO voor Bamboe door resolutie GmbH testgebruiker](#create-saml-sso-for-bamboo-by-resolution-gmbh-test-user)**  : als u wilt een equivalent van Britta Simon hebben in de SAML SSO voor Bamboe resolutie GmbH die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met SAML SSO voor Bamboe resolutie GmbH, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **SAML SSO voor Bamboe resolutie GmbH** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In het gedeelte **Standaard SAML-configuratie** voert u de volgende stappen uit als u de toepassing in de door **IDP** geïnitieerde modus wilt configureren:

    ![SAML SSO voor Bamboe resolutie GmbH domein en URL's, eenmalige aanmelding informatie](common/idp-intiated.png)

    a. In het tekstvak **Id** typt u een URL met het volgende patroon: `https://<server-base-url>/plugins/servlet/samlsso`

    b. In het tekstvak **Antwoord-URL** typt u een URL met de volgende notatie: `https://<server-base-url>/plugins/servlet/samlsso`

5. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    ![SAML SSO voor Bamboe resolutie GmbH domein en URL's, eenmalige aanmelding informatie](common/metadata-upload-additional-signon.png)

    In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<server-base-url>/plugins/servlet/samlsso`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke-id, de antwoord-URL en de aanmeldings-URL. Neem contact op met [SAML SSO voor Bamboe resolutie GmbH Client ondersteuningsteam](https://marketplace.atlassian.com/plugins/com.resolution.atlasplugins.samlsso-bamboo/server/support) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

6. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

7. Op de **SAML SSO voor Bamboe instellen door de resolutie GmbH** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-saml-sso-for-bamboo-by-resolution-gmbh-single-sign-on"></a>SAML SSO voor Bamboe configureren door de resolutie GmbH Single Sign-On

1. Aanmelding bij de SAML SSO voor Bamboe door resolutie GmbH bedrijf site als administrator.

1. Aan de rechterkant van de belangrijkste werkbalk, klikt u op **instellingen** > **invoegtoepassingen**.

    ![De instellingen](./media/bamboo-tutorial/tutorial_bamboo_setings.png)

1. Ga naar de sectie beveiliging, klikt u op **SAML SingleSignOn** op de menubalk.

    ![De eenmalige aanmelding met SAML](./media/bamboo-tutorial/tutorial_bamboo_samlsingle.png)

1. Op de **pagina SAML SIngleSignOn Plugin Configuration** (Configuratie van plug-in voor eenmalige aanmelding met SAML) klikt u op **Add idp** (IdP toevoegen).

    ![De IdP toevoegen](./media/bamboo-tutorial/tutorial_bamboo_addidp.png)

1. Voer op de pagina **Choose your SAML Identity Provider** (Uw SAML-id-provider kiezen) de volgende stappen uit:

    ![De id-provider](./media/bamboo-tutorial/tutorial_bamboo_identityprovider.png)

    a. Selecteer bij **Idp Type** (Type IdP) de optie **AZURE AD**.

    b. Voer in het tekstvak **Name** (Naam) de naam in.

    c. Voer in het tekstvak **Description** (Omschrijving) de omschrijving in.

    d. Klik op **volgende**.

1. Op de **id-providerconfiguratie** pagina op **volgende**.

    ![De id-configuratie](./media/bamboo-tutorial/tutorial_bamboo_identityconfig.png)

1. Klik op de pagina **Import SAML Idp Metadata** (Metagegevens voor SAML-IdP importeren) op de optie **Load File** (Bestand laden) om het bestand **METADATA XML** te uploaden die u uit de Azure-portal hebt gedownload.

    ![De IdP-metagegevens](./media/bamboo-tutorial/tutorial_bamboo_idpmetadata.png)

1. Klik op **volgende**.

1. Klik op **Save settings** (Instellingen opslaan).

    ![Het opslaan](./media/bamboo-tutorial/tutorial_bamboo_save.png)

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

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan SAML SSO voor Bamboe resolutie GmbH.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **SAML SSO voor Bamboe resolutie GmbH**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **SAML SSO voor Bamboe resolutie GmbH**.

    ![De SAML SSO voor Bamboe door resolutie GmbH koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-saml-sso-for-bamboo-by-resolution-gmbh-test-user"></a>SAML SSO voor Bamboe door resolutie GmbH testgebruiker maken

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in SAML SSO voor Bamboe resolutie GmbH. SAML SSO voor Bamboe resolutie GmbH biedt ondersteuning voor just-in-time inrichting en ook gebruikers kunnen handmatig worden gemaakt, neem contact op met [SAML SSO voor Bamboe resolutie GmbH Client ondersteuningsteam](https://marketplace.atlassian.com/plugins/com.resolution.atlasplugins.samlsso-bamboo/server/support) volgens uw behoeften.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Als u de SAML SSO voor Bamboe op door het probleem zou moeten GmbH tegel in het toegangsvenster, moet u worden automatisch aangemeld bij de SAML SSO voor Bamboe door resolutie GmbH waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
