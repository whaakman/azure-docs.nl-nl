---
title: 'Zelfstudie: Azure Active Directory-integratie met ServiceNow | Microsoft Docs'
description: Ontdek hoe u eenmalige aanmelding configureert tussen Azure Active Directory en ServiceNow.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: a5a1a264-7497-47e7-b129-a1b5b1ebff5b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/19/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0efc69aa7fb081303df6fc6327253fd5b8336999
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57898598"
---
# <a name="tutorial-azure-active-directory-integration-with-servicenow"></a>Zelfstudie: Azure Active Directory-integratie met ServiceNow

In deze zelfstudie leert u hoe u ServiceNow kunt integreren met Azure Active Directory (Azure AD).
De integratie van ServiceNow met Azure AD heeft de volgende voordelen:

* U kunt in Azure AD beheren wie toegang heeft tot ServiceNow.
* U kunt uw gebruikers zich automatisch laten aanmelden bij ServiceNow (eenmalige aanmelding) met hun Azure AD-account.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met ServiceNow hebt u het volgende nodig:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Een abonnement op ServiceNow waarvoor eenmalige aanmelding is ingeschakeld
* Voor ServiceNow: een instantie of tenant van ServiceNow, versie Calgary of hoger
* Voor ServiceNow Express: een instantie van ServiceNow Express, versie Helsinki of hoger
* Voor de ServiceNow-tenant moet de [invoegtoepassing Multiple-Provider single sign-on (SSO)](https://wiki.servicenow.com/index.php?title=Multiple_Provider_Single_Sign-On#gsc.tab=0) zijn ingeschakeld. Dit kan worden gedaan door het [indienen van een serviceaanvraag](https://hi.service-now.com).
* Schakel voor automatische configuratie de multi-provider-invoegtoepassing voor ServiceNow in.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* ServiceNow ondersteunt door **SP** geïnitieerde eenmalige aanmelding (SSO)

* ServiceNow ondersteunt het [**geautomatiseerd** inrichten van gebruikers](servicenow-provisioning-tutorial.md)

## <a name="adding-servicenow-from-the-gallery"></a>ServiceNow toevoegen vanuit de galerie

Voor het configureren van de integratie van ServiceNow in Azure AD moet u ServiceNow uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Voer de volgende stappen uit om ServiceNow toe te voegen vanuit de galerie:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ **ServiceNow** in het zoekvak, selecteer **ServiceNow** in het resultaatvenster en klik op de knop **Toevoegen** om de toepassing toe te voegen.

     ![ServiceNow in de resultatenlijst](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie gaat u Azure AD-eenmalige aanmelding met ServiceNow configureren en testen met behulp van een testgebruiker met de naam **Britta Simon**.
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Azure AD-gebruiker en de daaraan gerelateerde gebruiker in ServiceNow tot stand is gebracht.

Als u Azure AD-eenmalige aanmelding met ServiceNow wilt configureren en testen, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren voor ServiceNow](#configure-azure-ad-single-sign-on-for-servicenow)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Eenmalige aanmelding voor ServiceNow configureren](#configure-servicenow-single-sign-on)**: als u de instellingen voor eenmalige aanmelding aan de clientzijde wil configureren.
3. **[Azure AD eenmalige aanmelding configureren voor ServiceNow Express](#configure-azure-ad-single-sign-on-for-servicenow-express)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
4. **[Eenmalige aanmelding voor ServiceNow Express configureren](#configure-servicenow-express-single-sign-on)**: als u de instellingen voor eenmalige aanmelding aan de clientzijde wil configureren.
5. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
6. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
7. **[Een testgebruiker maken in ServiceNow](#create-servicenow-test-user)**: om in ServiceNow een tegenhanger van Britta Simon te hebben die is gekoppeld aan de Azure AD-weergave van de gebruiker.
8. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on-for-servicenow"></a>Azure AD-eenmalige aanmelding configureren voor ServiceNow

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD-eenmalige aanmelding met ServiceNow moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/) selecteert u **Eenmalige aanmelding** op de integratiepagina van de toepassing **ServiceNow**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Domein- en URL-gegevens voor eenmalige aanmelding bij ServiceNow](common/sp-identifier.png)

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<instance-name>.service-now.com/navpage.do`

    b. In het tekstvak **Id (Entiteits-id)** typt u een URL met de volgende notatie: `https://<instance-name>.service-now.com`

    > [!NOTE]
    > Dit zijn geen echte waarden. U moet deze waarden bijwerken uit de werkelijke URL voor eenmalige aanmelding en de id, zoals later in de zelfstudie wordt uitgelegd. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

    a. Klik op de knop Kopiëren om de **App-URL voor federatieve metagegevens** te kopiëren en in Kladblok te plakken, want deze wordt later in de zelfstudie gebruikt.

    b. Klik op **Downloaden** om het **Certificaat (Base64)** te downloaden en het certificaatbestand op te slaan op uw computer.

6. In de sectie **ServiceNow instellen** kopieert u de juiste URL('s) op basis van uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-servicenow-single-sign-on"></a>Eenmalige aanmelding voor ServiceNow configureren

1. Meld u aan bij uw ServiceNow-toepassing als beheerder.

2. Activeer de invoegtoepassing **Integration - Multiple Provider Single Sign-On Installer** door deze stappen te volgen:

    a. Gebruik de zoekbalk in het navigatievenster aan de linkerkant om de sectie **System Definition** op te zoeken, en klik op **Plugins**.

    ![Activate plugin](./media/servicenow-tutorial/tutorial_servicenow_03.png "Activate plugin")

    b. Zoek naar **Integration - Multiple Provider Single Sign-On Installer**.

     ![Activate plugin](./media/servicenow-tutorial/tutorial_servicenow_04.png "Activate plugin")

    c. Selecteer de invoegtoepassing. Klik met de rechtermuisknop en selecteer **Activate/Upgrade**.

     ![Activate plugin](./media/servicenow-tutorial/tutorial_activate.png "Activate plugin")

    d. Klik op de knop **Activate**.

     ![Activate plugin](./media/servicenow-tutorial/tutorial_activate1.png "Activate plugin")

3. Gebruik de zoekbalk in het navigatievenster aan de linkerkant om de sectie **Multi-Provider SSO** op te zoeken, en klik op **Properties**.

    ![Configure app URL](./media/servicenow-tutorial/tutorial_servicenow_06.png "Configure app URL")

4. Voer in het dialoogvenster **Multiple Provider SSO Properties** de volgende stappen uit:

    ![Configure app URL](./media/servicenow-tutorial/ic7694981.png "Configure app URL")

    * Selecteer **Yes** bij **Enable multiple provider SSO**.
  
    * Selecteer **Yes** bij **Enable Auto Importing of users from all identity providers into the user table**.

    * Selecteer **Yes** bij **Enable debug logging for the multiple provider SSO integration**.

    * Typ **user_name** in het tekstvak **The field on the user table that...**
  
    * Klik op **Opslaan**.

5. Er zijn twee manieren waarop **ServiceNow** kan worden geconfigureerd: automatisch en handmatig.

6. Volg de stappen hieronder om **ServiceNow** automatisch te configureren:

    * Ga terug naar de eenmalige aanmeldingspagina van **ServiceNow** in de Azure-portal.

    * Er is een service voor configureren met één klik beschikbaar voor ServiceNow; dat wil zeggen dat Azure AD ServiceNow automatisch kan configureren voor verificatie op basis van SAML. Om deze service in te schakelen gaat u naar de sectie **ServiceNow Configuration** en klikt u op **Configure ServiceNow** om het venster Configure sign-on te openen.

        ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/tutorial_servicenow_configure.png)

    * Voer in het formulier **Configure sign-on** de naam van uw ServiceNow-instantie, de naam van de gebruiker met beheerdersrechten en het beheerderswachtwoord in en klik op **Configure Now**. Let op: de opgegeven beheerdersgebruikersnaam moet de rol **security_admin** hebben in ServiceNow om dit te laten werken. U kunt ServiceNow ook handmatig configureren om Azure AD te gebruiken als SAML Identity Provider. Klik op **Manually configure single sign-on** en kopieer de **Sign-Out URL, SAML Entity ID en SAML Single Sign-On Service URL** uit de sectie Quick Reference.

        ![Configure app URL](./media/servicenow-tutorial/configure.png "Configure app URL")

    * Meld u aan bij uw ServiceNow-toepassing als beheerder.

    * Bij de automatische configuratie worden alle benodigde instellingen aan de kant van **ServiceNow** geconfigureerd, maar het **X.509 Certificate** wordt standaard niet ingeschakeld. U moet het handmatig toewijzen aan uw Identiteitsprovider in ServiceNow. Volg de onderstaande stappen om dit te doen:

    * Gebruik de zoekbalk in het navigatievenster aan de linkerkant om de sectie **Multi-Provider SSO** op te zoeken, en klik op **Identity Providers**.

        ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/tutorial_servicenow_07.png "Eenmalige aanmelding configureren")

    * Klik op de automatisch gegenereerde identiteitsprovider

        ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/tutorial_servicenow_08.png "Eenmalige aanmelding configureren")

    *  Voer in de sectie **Identity Provider** de volgende stappen uit:

        ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/automatic_config.png "Eenmalige aanmelding configureren")

        * Typ in het tekstvak **Name** een naam voor uw configuratie (bijvoorbeeld **Microsoft Azure Federated Single Sign-on**).

        * Verwijder de standaard ingevulde waarde uit het tekstvak **Identity Provider's SingleLogoutRequest**.

        * Kopieer de waarde van **ServiceNow Homepage** en plak deze in het tekstvak **Sign-on URL** in de sectie **Domein- en URL-gegevens voor ServiceNow** in de Azure-portal.

            > [!NOTE]
            > De startpagina van de ServiceNow-instantie is een samenvoeging van uw **ServiceNow-tenant-URL** en **/navpage.do** (bijvoorbeeld:`https://fabrikam.service-now.com/navpage.do`).

        * Kopieer de waarde van **Entity ID / Issuer** en plak deze in het tekstvak **Id** in de sectie **Domein- en URL-gegevens voor ServiceNow** in de Azure-portal.

        * Zorg ervoor dat **NameID Policy** op de waarde `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified` is ingesteld. 

    * Schuif omlaag naar de sectie **X.509 Certificate** en selecteer **Edit**.

        ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/tutorial_servicenow_09.png "Eenmalige aanmelding configureren")

    * Selecteer het certificaat en klik op de pijl naar rechts om het certificaat toe te voegen

        ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/tutorial_servicenow_11.png "Eenmalige aanmelding configureren")

    * Klik op **Opslaan**.

    * Klik op **Test Connection** in de rechterbovenhoek van de pagina.

        ![Activate plugin](./media/servicenow-tutorial/tutorial_activate2.png "Activate plugin")

    * Nadat u op **Test Connection** hebt geklikt, wordt het pop-upvenster weergegeven waarin u referenties moet invoeren, en wordt de onderstaande pagina met resultaten weergegeven. De foutmelding **SSO Logout Test Results** is verwacht; negeer deze fout en klik op de knop **Activate**.

        ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/servicenowactivate.png "Eenmalige aanmelding configureren")
  
7. Volg de stappen hieronder om **ServiceNow** handmatig te configureren:

    * Meld u aan bij uw ServiceNow-toepassing als beheerder.

    * Klik in het navigatievenster aan de linkerkant op **Identity Providers**.

        ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/tutorial_servicenow_07.png "Eenmalige aanmelding configureren")

    * Klik in het dialoogvenster **Identity Providers** op **New**.

        ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/ic7694977.png "Eenmalige aanmelding configureren")

    * Klik in het dialoogvenster **Identity Providers** op **SAML**.

        ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/ic7694978.png "Eenmalige aanmelding configureren")

    * Voer in het pop-upvenster **Import Identity Provider Metadata** de volgende stappen uit:

        ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/idp.png "Eenmalige aanmelding configureren")

        * Voer de **App Federation Metadata Url** in die u hebt gekopieerd uit de Azure-portal.

        * Klik op **Import**.

    * De URL met de IdP-metagegevens wordt gelezen en de overeenkomende velden worden ingevuld.

        ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/ic7694982.png "Eenmalige aanmelding configureren")

        * Typ in het tekstvak **Name** een naam voor uw configuratie (bijvoorbeeld **Microsoft Azure Federated Single Sign-on**).

        * Verwijder de standaard ingevulde waarde uit het tekstvak **Identity Provider's SingleLogoutRequest**.

        * Kopieer de waarde van **ServiceNow Homepage** en plak deze in het tekstvak **Sign-on URL** in de sectie **Domein- en URL-gegevens voor ServiceNow** in de Azure-portal.

            > [!NOTE]
            > De startpagina van de ServiceNow-instantie is een samenvoeging van uw **ServiceNow-tenant-URL** en **/navpage.do** (bijvoorbeeld:`https://fabrikam.service-now.com/navpage.do`).

        * Kopieer de waarde van **Entity ID / Issuer** en plak deze in het tekstvak **Id** in de sectie **Domein- en URL-gegevens voor ServiceNow** in de Azure-portal.

        * Zorg ervoor dat **NameID Policy** op de waarde `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified` is ingesteld.

        * Klik op **Advanced**. Typ in het vak **User Field** **email** of **user_name**, afhankelijk van welk veld wordt gebruikt om gebruikers in uw ServiceNow-implementatie uniek te identificeren.

            > [!NOTE]
            > U kunt Azure AD om de Azure AD-gebruikers-id (user principal name) of het e-mailadres als de unieke id in het SAML-token te gebruiken door naar de sectie **ServiceNow > Attributen > Eenmalige aanmelding** in de Azure-portal te gaan en het gewenste veld toe te wijzen aan het kenmerk **nameidentifier**. De waarde die voor het geselecteerde kenmerk wordt opgeslagen in Azure AD (bijvoorbeeld de user principal name) moet overeenkomen met de waarde die is opgeslagen in ServiceNow voor het ingevoerde veld (bijvoorbeeld user_name)

        * Klik op **Test Connection** in de rechterbovenhoek van de pagina.

        * Nadat u op **Test Connection** hebt geklikt, wordt het pop-upvenster weergegeven waarin u referenties moet invoeren, en wordt de onderstaande pagina met resultaten weergegeven. De foutmelding **SSO Logout Test Results** is verwacht; negeer deze fout en klik op de knop **Activate**.

          ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/servicenowactivate.png "Eenmalige aanmelding configureren")

### <a name="configure-azure-ad-single-sign-on-for-servicenow-express"></a>Azure AD-eenmalige aanmelding configureren voor ServiceNow Express

1. In de [Azure-portal](https://portal.azure.com/) selecteert u **Eenmalige aanmelding** op de integratiepagina van de toepassing **ServiceNow**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Domein- en URL-gegevens voor eenmalige aanmelding bij ServiceNow](common/sp-identifier.png)

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<instance-name>.service-now.com/navpage.do`

    b. In het tekstvak **Id (Entiteits-id)** typt u een URL met de volgende notatie: `https://<instance-name>.service-now.com`

    > [!NOTE]
    > Dit zijn geen echte waarden. U moet deze waarden bijwerken uit de werkelijke URL voor eenmalige aanmelding en de id, zoals later in de zelfstudie wordt uitgelegd. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

6. Er is een service voor configureren met één klik beschikbaar voor ServiceNow; dat wil zeggen dat Azure AD ServiceNow automatisch kan configureren voor verificatie op basis van SAML. Om deze service in te schakelen gaat u naar de sectie **ServiceNow instellen** en klikt u op **Stapsgewijze instructies weergeven** om het venster Aanmelding configureren weer te geven.

    ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/tutorial_servicenow_configure.png)

7. Voer in het formulier **Configure sign-on** de naam van uw ServiceNow-instantie, de naam van de gebruiker met beheerdersrechten en het beheerderswachtwoord in en klik op **Configure Now**. Let op: de opgegeven beheerdersgebruikersnaam moet de rol **security_admin** hebben in ServiceNow om dit te laten werken. U kunt ServiceNow ook handmatig configureren om Azure AD te gebruiken als SAML Identity Provider. Klik op **Manually configure single sign-on** en kopieer de **Sign-Out URL, SAML Entity ID en SAML Single Sign-On Service URL** uit de sectie Quick Reference.

    ![Configure app URL](./media/servicenow-tutorial/configure.png "Configure app URL")

### <a name="configure-servicenow-express-single-sign-on"></a>Eenmalige aanmelding voor ServiceNow Express configureren

1. Meld u aan bij uw ServiceNow Express-toepassing als beheerder.

2. Klik in het navigatievenster aan de linkerkant op **Single Sign-On**.

    ![Configure app URL](./media/servicenow-tutorial/ic7694980ex.png "Configure app URL")

3. Klik in het dialoogvenster **Single Sign-On** op het configuratiepictogram rechtsboven en stel de volgende eigenschappen in:

    ![Configure app URL](./media/servicenow-tutorial/ic7694981ex.png "Configure app URL")

    a. Schuif de schakelaar **Enable multiple provider SSO** naar rechts.

    b. Schuif de schakelaar **Enable debug logging for the multiple provider SSO integration** naar rechts.

    c. Typ **user_name** in het tekstvak **The field on the user table that...**

4. Klik in het dialoogvenster **Single Sign-On** op **Add New Certificate**.

    ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/ic7694973ex.png "Eenmalige aanmelding configureren")

5. Voer in het dialoogvenster **X.509 Certificates** de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/ic7694975.png "Eenmalige aanmelding configureren")

    a. Typ in het tekstvak **Name** een naam voor de configuratie (bijvoorbeeld: **TestSAML2.0**).

    b. Selecteer **Active**.

    c. Selecteer **PEM** als **Format**.

    d. Selecteer **Trust Store Cert** als **Type**.

    e. Open uw met Base 64 gecodeerde certificaat dat u hebt gedownload in de Azure-portal, kopieer de inhoud ervan naar het Klembord en plak deze in het tekstvak **PEM Certificate**.

    f. Klik op **Update**

6. Klik in het dialoogvenster **Single Sign-On** op **Add New IdP**.

    ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/ic7694976ex.png "Eenmalige aanmelding configureren")

7. Voer in het dialoogvenster **Add New Identity Provider**onder **Configure Identity Provider** de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/ic7694982ex.png "Eenmalige aanmelding configureren")

    a. Typ in het tekstvak **Name** een naam voor de configuratie (bijvoorbeeld: **SAML 2.0**).

    b. Plak in het veld **Identity Provider URL** de waarde van **Identiteitsprovider-id** die u hebt gekopieerd uit de Azure-portal.

    c. Plak in het veld **Identity Provider's AuthnRequest** de waarde van **Verificatieaanvraag-URL** die u hebt gekopieerd uit de Azure-portal.

    d. Plak in het veld **Identity Provider's SingleLogoutRequest** de waarde van **URL voor afmelden** die u hebt gekopieerd uit de Azure-portal

    e. Selecteer het certificaat dat u in de vorige stap hebt gemaakt als **Identity Provider Certificate**.

8. Klik op **Advanced Settings** en voer onder **Additional Identity Provider Properties** de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/ic7694983ex.png "Eenmalige aanmelding configureren")

    a. Typ **urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect** in het tekstvak **Protocol Binding for the IDP's SingleLogoutRequest**.

    b. Typ **urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified** in het tekstvak **NameID Policy**.

    c. Typ `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password` in **AuthnContextClassRef Method**.

    d. Deselecteer **Create an AuthnContextClass**.

9. Voer onder **Additional Service Provider Properties** de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/ic7694984ex.png "Eenmalige aanmelding configureren")

    a. Typ in het tekstvak **ServiceNow Homepage** de URL van de startpagina van uw ServiceNow-instantie.

    > [!NOTE]
    > De startpagina van de ServiceNow-instantie is een samenvoeging van uw **ServiceNow-tenant-URL** en **/navpage.do** (bijvoorbeeld: `https://fabrikam.service-now.com/navpage.do`).

    b. Typ in het tekstvak **Entity ID / Issuer** de URL van uw ServiceNow-tenant.

    c. Typ in het tekstvak **Audience URI** de URL van uw ServiceNow-tenant.

    d. Typ **60** in het tekstvak **Clock Skew**.

    e. Typ in het vak **User Field** **email** of **user_name**, afhankelijk van welk veld wordt gebruikt om gebruikers in uw ServiceNow-implementatie uniek te identificeren.

    > [!NOTE]
    > U kunt Azure AD om de Azure AD-gebruikers-id (user principal name) of het e-mailadres als de unieke id in het SAML-token te gebruiken door naar de sectie **ServiceNow > Attributen > Eenmalige aanmelding** in de Azure-portal te gaan en het gewenste veld toe te wijzen aan het kenmerk **nameidentifier**. De waarde die voor het geselecteerde kenmerk wordt opgeslagen in Azure AD (bijvoorbeeld de user principal name) moet overeenkomen met de waarde die is opgeslagen in ServiceNow voor het ingevoerde veld (bijvoorbeeld user_name)

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
  
    b. In de **gebruikersnaam** veldtype **brittasimon\@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In dit gedeelte gaat u Britta Simon toestemming geven voor gebruik van eenmalige aanmelding met Azure door haar toegang te geven tot ServiceNow.

1. Selecteer in Azure Portal **Bedrijfstoepassingen**, selecteer **Alle toepassingen** en selecteer vervolgens **ServiceNow**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer **ServiceNow** in de lijst met toepassingen.

    ![De koppeling ServiceNow in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-servicenow-test-user"></a>Een testgebruiker maken in ServiceNow

Het doel van deze sectie is het maken van een gebruiker met de naam Britta Simon in ServiceNow. ServiceNow ondersteunt het automatisch inrichten van gebruikers, wat standaard is ingeschakeld. U kunt [hier](servicenow-provisioning-tutorial.md) meer informatie vinden over het configureren van het automatisch inrichten van gebruikers.

> [!NOTE]
> Als u handmatig een gebruiker moet maken, neemt u contact op met het [ondersteuningsteam van ServiceNow](https://www.servicenow.com/support/contact-support.html)

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel ServiceNow in het toegangsvenster klikt, zou u automatisch moeten worden aangemeld bij de ServiceNow-instantie waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Inrichten van gebruikers configureren](servicenow-provisioning-tutorial.md)
