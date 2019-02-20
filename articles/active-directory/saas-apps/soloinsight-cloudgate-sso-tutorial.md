---
title: 'Zelfstudie: Azure Active Directory-integratie met Soloinsight-CloudGate SSO | Microsoft Docs'
description: Ontdek hoe u eenmalige aanmelding configureert tussen Azure Active Directory en Soloinsight-CloudGate.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 9263c241-85a4-4724-afac-0351d6275958
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/07/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5839d7212958cffe45fae19f1e502ecf04e744a3
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56162048"
---
# <a name="tutorial-azure-active-directory-integration-with-soloinsight-cloudgate-sso"></a>Zelfstudie: Azure Active Directory-integratie met Soloinsight-CloudGate SSO

In deze zelfstudie leert u hoe u Soloinsight-CloudGate SSO kunt integreren met Azure Active Directory (Azure AD).
De integratie van Soloinsight-CloudGate SSO met Azure Active Directory heeft de volgende voordelen:

* U kunt in Azure Active Directory beheren wie toegang heeft tot Soloinsight-CloudGate SSO.
* U kunt uw gebruikers zich automatisch laten aanmelden bij Soloinsight-CloudGate SSO (eenmalige aanmelding) met hun Azure Active Directory-account.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor de configuratie van Azure Active Directory-integratie met Soloinsight-CloudGate SSO hebt u het volgende nodig:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Soloinsight-CloudGate SSO-abonnement waarvoor eenmalige aanmelding is ingeschakeld

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Soloinsight-CloudGate SSO ondersteunt door **SP** geïnitieerde eenmalige aanmelding

## <a name="adding-soloinsight-cloudgate-sso-from-the-gallery"></a>Soloinsight-CloudGate SSO toevoegen uit de galerie

Voor de configuratie van de integratie van Soloinsight-CloudGate SSO in Azure Active Directory, moet u Soloinsight-CloudGate SSO vanuit de galerie aan uw lijst met beheerde SaaS-apps toevoegen.

**Voer de volgende stappen uit om Soloinsight-CloudGate SSO toe te voegen vanuit de galerie:**

1. Klik in het linkernavigatievenster in de **[Azure-portal](https://portal.azure.com)** op het **Azure Active Directory**-pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **Soloinsight-CloudGate SSO**, selecteer **Soloinsight-CloudGate SSO** in het resultaatvenster en klik vervolgens op de knop **Toevoegen** om de toepassing toe te voegen.

     ![Soloinsight-CloudGate SSO in de resultatenlijst](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie gaat u eenmalige aanmelding bij Azure Active Directory met Soloinsight-CloudGate SSO configureren en testen met behulp van een testgebruiker met de naam **Britta Simon**.
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Azure Active Directory-gebruiker en de daaraan gerelateerde gebruiker in Soloinsight-CloudGate SSO tot stand is gebracht.

Als u eenmalige aanmelding bij Azure Active Directory met Soloinsight-CloudGate SSO wilt configureren en testen, moet u de volgende onderdelen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Eenmalige aanmelding voor Soloinsight-CloudGate SSO configureren](#configure-soloinsight-cloudgate-sso-single-sign-on)**: als u de instellingen voor eenmalige aanmelding aan de clientzijde wil configureren.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Een testgebruiker maken in Soloinsight-CloudGate SSO](#create-soloinsight-cloudgate-sso-test-user)**: om in Soloinsight-CloudGate SSO een tegenhanger van Britta Simon te hebben die is gekoppeld aan de Azure Active Directory-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor de configuratie van eenmalige aanmelding in Azure AD met Soloinsight-CloudGate SSO moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/) selecteert u **Eenmalige aanmelding** op de integratiepagina van de toepassing **Soloinsight-CloudGate SSO**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Domein- en URL-gegevens voor eenmalige aanmelding bij Soloinsight-CloudGate SSO](common/sp-identifier.png)

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<SUBDOMAIN>.sigateway.com/login`

    b. In het tekstvak **Id (Entiteits-id)** typt u een URL met de volgende notatie: `https://<SUBDOMAIN>.sigateway.com/process/sso`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke URL voor eenmalige aanmelding en de id, zoals later in de zelfstudie wordt uitgelegd, in de sectie **Eenmalige aanmelding voor Soloinsight-CloudGate SSO configureren**.

4. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

6. In het gedeelte **Soloinsight-CloudGate SSO instellen** kopieert u de juiste URL('s) op basis van uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-soloinsight-cloudgate-sso-single-sign-on"></a>Eenmalige aanmelding voor Soloinsight-CloudGate SSO configureren

1. Om de waarden te verkrijgen die tijdens het configureren van Basic SAML in de Azure-portal moet plakken, meldt u zich aan bij de CloudGate Web Portal met behulp van uw referenties. Ga vervolgens naar de SSO-instellingen, die te vinden zijn via **Home (Start) > Administration (Beheer) > System Settings (Systeeminstellingen) > General (Algemeen)**.

    ![CloudGate SSO-instellingen](./media/soloinsight-cloudgate-sso-tutorial/sso-main-settings.png)

2. **URL voor SAML-consument**

    * Kopieer de koppelingen die worden weergegeven in de velden **SAML Consumer URL** (URL voor SAML-consument) en **Redirect URL** (Omleidings-URL) en plak deze in de Azure-portal in de sectie **Standaard SAML-configuratie** in de velden **Id (Entiteits-id)** en **Antwoord-URL**, respectievelijk.

        ![SAMLIdentifier](./media/soloinsight-cloudgate-sso-tutorial/saml-identifier.png)

3. **SAML-handtekeningcertificaat**

    * Ga naar de bron van het (Base64)-certificaatbestand dat is gedownload via de SAML-ondertekeningcertificaatlijsten in de Azure-portal en klik hierop met de rechtermuisknop. Kies in de lijst de optie **Bewerken met Notepad++**. 

        ![SAMLcertificate](./media/soloinsight-cloudgate-sso-tutorial/certificate-file.png)

    * Kopieer de inhoud in het (Base64)-certificaat naar het Notepad++-bestand.

        ![Certificaat kopiëren](./media/soloinsight-cloudgate-sso-tutorial/certificate-copy.png)

    * Plak de inhoud in de CloudGate Web Portal SSO-instellingen in het veld **Certificaat** en klik op de knop Opslaan.

        ![Certificaatportal](./media/soloinsight-cloudgate-sso-tutorial/certificate-portal.png)

4. **Standaardgroep**

    * Selecteer in de CloudGate Web Portal in de vervolgkeuzelijst voor de optie **Default Group** (Standaardgroep) **Business Admin** (Bedrijfsbeheerder)

        ![Standaardgroep](./media/soloinsight-cloudgate-sso-tutorial/default-group.png)

5. **AD-id en aanmeldings-URL**

    * De **Aanmeldings-URL** die u in de Azure-portal tijdens de configuratiestap bij **Soloinsight CloudGate SSO instellen** hebt gekopieerd, moet worden ingevoerd in de sectie met instellingen voor CloudGate Web Portal SSO. 

    * Plak de link van de **Aanmeldings-URL** vanuit de Azure-portal in het veld **AD Login URL** (URL voor AD-aanmeldings) van de CloudGate Web Portal.
     
    * Plak de link van het **Azure AD-id** vanuit de Azure-portal in het veld **AD Identifier** (AD-id) van de CloudGate Web Portal.

        ![AD-aanmelding](./media/soloinsight-cloudgate-sso-tutorial/ad-login.png)

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

In dit gedeelte gaat u Britta Simon toestemming geven voor gebruik van eenmalige aanmelding met Azure door haar toegang te geven tot Soloinsight-CloudGate SSO.

1. Selecteer **Bedrijfstoepassingen** in de Azure-portal, selecteer **Alle toepassingen** en selecteer vervolgens **Soloinsight-CloudGate SSO**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen **Soloinsight-CloudGate SSO**.

    ![De koppeling voor Soloinsight-CloudGate SSO in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-soloinsight-cloudgate-sso-test-user"></a>Soloinsight CloudGate SSO-testgebruiker maken

Om een testgebruiker te maken, selecteert u **Employees** (Werknemers) in het hoofdmenu van uw CloudGate Web Portal en vult u het formulier voor het toevoegen van nieuwe werknemers in. Het machtigingsniveau dat aan de testgebruiker moet worden toegewezen is **Business Admin** (Bedrijfsbeheerder). Klik op **Create** (Maken) wanneer u alle vereiste velden hebt ingevuld.

![Werknemertest](./media/soloinsight-cloudgate-sso-tutorial/employee-test.png)

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie gaat u uw configuratie van Azure AD-eenmalige aanmelding testen via het toegangsvenster.

Wanneer u in het toegangsvenster op de tegel Soloinsight-CloudGate SSO klikt, wordt u automatisch aangemeld bij de instantie van Soloinsight-CloudGate SSO waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

