---
title: 'Zelfstudie: Azure Active Directory-integratie met ServiceNow | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en ServiceNow.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: a5a1a264-7497-47e7-b129-a1b5b1ebff5b
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/27/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8ba516aa2c3d2decaa4962f1ccd0394ebe9a4a62
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67706130"
---
# <a name="tutorial-integrate-servicenow-with-azure-active-directory"></a>Zelfstudie: ServiceNow integreren met Azure Active Directory

In deze zelfstudie leert u hoe u ServiceNow integreert met Azure Active Directory (Azure AD). Wanneer u ServiceNow met Azure AD integreert, kunt u het volgende doen:

* Beheren in Azure AD die toegang tot ServiceNow heeft.
* Kunnen uw gebruikers worden automatisch aangemeld met ServiceNow met hun Azure AD-accounts.
* Beheer uw accounts in één centrale locatie - Azure portal.

Zie voor meer informatie over de integratie van de SaaS-app met Azure AD, [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Vereisten

Om te beginnen, hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen abonnement hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/).
* ServiceNow eenmalige aanmelding (SSO) ingeschakeld abonnement.
* Voor ServiceNow: een instantie of tenant van ServiceNow, versie Calgary of hoger
* Voor ServiceNow Express: een instantie van ServiceNow Express, versie Helsinki of hoger
* Voor de ServiceNow-tenant moet de [invoegtoepassing Multiple-Provider single sign-on (SSO)](https://wiki.servicenow.com/index.php?title=Multiple_Provider_Single_Sign-On#gsc.tab=0) zijn ingeschakeld. Dit kan worden gedaan door het [indienen van een serviceaanvraag](https://hi.service-now.com).
* Schakel voor automatische configuratie de multi-provider-invoegtoepassing voor ServiceNow in.
* Voor het installeren van de ServiceNow-klassiek (mobiel)-toepassing moet u gaat u naar de juiste store en zoek naar de ServiceNow-Classic-toepassing en klik op downloaden.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie hebt u configureren en testen van Azure AD-eenmalige aanmelding in een testomgeving. Biedt ondersteuning voor ServiceNow **SP** geïnitieerde eenmalige aanmelding en ondersteunt [ **automatisch** gebruikersinrichting](servicenow-provisioning-tutorial.md).

ServiceNow klassiek (mobiel)-toepassing kan nu worden geconfigureerd met Azure AD om eenmalige aanmelding in te schakelen en deze ondersteunt zowel **Android** en **IOS** gebruikers. In deze zelfstudie hebt u configureren en testen van Azure AD-eenmalige aanmelding in een testomgeving.

## <a name="adding-servicenow-from-the-gallery"></a>ServiceNow toevoegen vanuit de galerie

Voor het configureren van de integratie van ServiceNow in Azure AD moet u ServiceNow uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer in het navigatiedeelvenster links in de **Azure Active Directory** service.
1. Navigeer naar **bedrijfstoepassingen** en selecteer vervolgens **alle toepassingen**.
1. Nieuwe toepassing toevoegen, selecteert u **nieuwe toepassing**.
1. In de **toevoegen vanuit de galerie** sectie, typt u **ServiceNow** in het zoekvak in.
1. Selecteer **ServiceNow** van resultaten van het deelvenster en vervolgens de app toevoegen. Wacht een paar seconden terwijl de app wordt toegevoegd aan uw tenant.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

Configureren en testen van Azure AD-eenmalige aanmelding met ServiceNow met behulp van een testgebruiker met de naam **B.Simon**. Voor eenmalige aanmelding om te werken, moet u een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in ServiceNow vast te stellen.

Als u wilt configureren en testen van Azure AD-eenmalige aanmelding met ServiceNow, voert u de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-sso)**  zodat uw gebruikers deze functie wilt gebruiken.
2. **[Configureren van ServiceNow](#configure-servicenow)**  de SSO-instellingen configureren op de kant van de toepassing.
3. **[Azure AD-eenmalige aanmelding configureren voor ServiceNow snelle](#configure-azure-ad-sso-for-servicenow-express)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
4. **[ServiceNow Express SSO configureren](#configure-servicenow-express-sso)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
5. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  voor het testen van Azure AD eenmalige aanmelding met B.Simon.
6. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  B.Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
7. **[ServiceNow-testgebruiker maken](#create-servicenow-test-user)**  hebben een equivalent van B.Simon in ServiceNow die is gekoppeld aan de Azure AD-weergave van de gebruiker.
8. **[Eenmalige aanmelding testen](#test-sso)**  om te controleren of de configuratie werkt.
9. **[Eenmalige aanmelding testen voor ServiceNow klassiek (mobiel)](#test-sso-for-servicenow-classic-mobile)**  om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-sso"></a>Azure AD-eenmalige aanmelding configureren

Volg deze stappen voor het inschakelen van Azure AD-eenmalige aanmelding in de Azure-portal.

1. In de [Azure-portal](https://portal.azure.com/)op de **ServiceNow** toepassingspagina integratie, vinden de **beheren** sectie en selecteer **Single Sign-On**.
1. Op de **Selecteer een methode voor eenmalige aanmelding** pagina, selecteert u **SAML**.
1. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op het pictogram voor bewerken/pen voor **SAML-basisconfiguratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<instance-name>.service-now.com/navpage.do`

    b. In het tekstvak **Id (Entiteits-id)** typt u een URL met de volgende notatie: `https://<instance-name>.service-now.com`

    > [!NOTE]
    > Dit zijn geen echte waarden. U moet deze waarden bijwerken met de werkelijke aanmeldings-URL en de id die later in de zelfstudie wordt uitgelegd. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

1. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, zoeken **certificaat (Base64)** en selecteer **downloaden** voor het downloaden van het certificaat en sla deze op uw computer.

   ![De link om het certificaat te downloaden](common/certificatebase64.png)

   a. Klik op de knop Kopiëren om de **App-URL voor federatieve metagegevens** te kopiëren en in Kladblok te plakken, want deze wordt later in de zelfstudie gebruikt.

    b. Klik op **Downloaden** om het **Certificaat (Base64)** te downloaden en het certificaatbestand op te slaan op uw computer.

1. Op de **ServiceNow instellen** sectie, kopieert u de juiste URL's op basis van uw behoeften.

   ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

### <a name="configure-servicenow"></a>ServiceNow configureren

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

    * Voer in het formulier **Configure sign-on** de naam van uw ServiceNow-instantie, de naam van de gebruiker met beheerdersrechten en het beheerderswachtwoord in en klik op **Configure Now**. Let op: de opgegeven beheerdersgebruikersnaam moet de rol **security_admin** hebben in ServiceNow om dit te laten werken. Anders handmatig configureren van ServiceNow voor het gebruik van Azure AD als SAML-identiteitsprovider, klikt u op **handmatig configureren van eenmalige aanmelding** en kopieer de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de sectie ter referentie.

        ![Configure app URL](./media/servicenow-tutorial/configure.png "Configure app URL")

    * Meld u aan bij uw ServiceNow-toepassing als beheerder.

    * Bij de automatische configuratie worden alle benodigde instellingen aan de kant van **ServiceNow** geconfigureerd, maar het **X.509 Certificate** wordt standaard niet ingeschakeld. U moet het handmatig toewijzen aan uw Identiteitsprovider in ServiceNow. Volg de onderstaande stappen om dit te doen:

    * Gebruik de zoekbalk in het navigatievenster aan de linkerkant om de sectie **Multi-Provider SSO** op te zoeken, en klik op **Identity Providers**.

        ![Configure Single Sign-On](./media/servicenow-tutorial/tutorial_servicenow_07.png "Configure Single Sign-On")

    * Klik op de automatisch gegenereerde identiteitsprovider

        ![Configure Single Sign-On](./media/servicenow-tutorial/tutorial_servicenow_08.png "Configure Single Sign-On")

    *  Voer in de sectie **Identity Provider** de volgende stappen uit:

        ![Configure Single Sign-On](./media/servicenow-tutorial/automatic_config.png "Configure Single Sign-On")

        * In de **naam** tekstvak, typ een naam voor uw configuratie (bijvoorbeeld **Microsoft Azure voor federatieve eenmalige aanmelding**).

        * Verwijder de standaard ingevulde waarde uit het tekstvak **Identity Provider's SingleLogoutRequest**.

        * Kopie **ServiceNow Homepage** waarde, plak deze in de **aanmeldings-URL** -tekstvak in **ServiceNow basisconfiguratie SAML** sectie in Azure portal.

            > [!NOTE]
            > De startpagina van de ServiceNow-instantie is een samenvoeging van uw **ServiceNow-tenant-URL** en **/navpage.do** (bijvoorbeeld:`https://fabrikam.service-now.com/navpage.do`).

        * Kopie **entiteit-ID / verlener** waarde, plak deze in **id** -tekstvak in **ServiceNow basisconfiguratie SAML** sectie in Azure portal.

        * Zorg ervoor dat **NameID Policy** op de waarde `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified` is ingesteld. 

    * Schuif omlaag naar de sectie **X.509 Certificate** en selecteer **Edit**.

        ![Configure Single Sign-On](./media/servicenow-tutorial/tutorial_servicenow_09.png "Configure Single Sign-On")

    * Selecteer het certificaat en klik op de pijl naar rechts om het certificaat toe te voegen

        ![Configure Single Sign-On](./media/servicenow-tutorial/tutorial_servicenow_11.png "Configure Single Sign-On")

    * Klik op **Opslaan**.

    * Klik op **Test Connection** in de rechterbovenhoek van de pagina.

        ![Activate plugin](./media/servicenow-tutorial/tutorial_activate2.png "Activate plugin")

    * Nadat u op **Test Connection** hebt geklikt, wordt het pop-upvenster weergegeven waarin u referenties moet invoeren, en wordt de onderstaande pagina met resultaten weergegeven. De foutmelding **SSO Logout Test Results** is verwacht; negeer deze fout en klik op de knop **Activate**.

        ![Configure Single Sign-On](./media/servicenow-tutorial/servicenowactivate.png "Configure Single Sign-On")
  
7. Volg de stappen hieronder om **ServiceNow** handmatig te configureren:

    * Meld u aan bij uw ServiceNow-toepassing als beheerder.

    * Klik in het navigatievenster aan de linkerkant op **Identity Providers**.

        ![Configure Single Sign-On](./media/servicenow-tutorial/tutorial_servicenow_07.png "Configure Single Sign-On")

    * Klik in het dialoogvenster **Identity Providers** op **New**.

        ![Configure Single Sign-On](./media/servicenow-tutorial/ic7694977.png "Configure Single Sign-On")

    * Klik in het dialoogvenster **Identity Providers** op **SAML**.

        ![Configure Single Sign-On](./media/servicenow-tutorial/ic7694978.png "Configure Single Sign-On")

    * Voer in het pop-upvenster **Import Identity Provider Metadata** de volgende stappen uit:

        ![Configure Single Sign-On](./media/servicenow-tutorial/idp.png "Configure Single Sign-On")

        * Voer de **App Federation Metadata Url** in die u hebt gekopieerd uit de Azure-portal.

        * Klik op **importeren**.

    * De URL met de IdP-metagegevens wordt gelezen en de overeenkomende velden worden ingevuld.

        ![Configure Single Sign-On](./media/servicenow-tutorial/ic7694982.png "Configure Single Sign-On")

        * In de **naam** tekstvak, typ een naam voor uw configuratie (bijvoorbeeld **Microsoft Azure voor federatieve eenmalige aanmelding**).

        * Verwijder de standaard ingevulde waarde uit het tekstvak **Identity Provider's SingleLogoutRequest**.

        * Kopie **ServiceNow Homepage** waarde, plak deze in de **aanmeldings-URL** -tekstvak in **ServiceNow basisconfiguratie SAML** sectie in Azure portal.

            > [!NOTE]
            > De startpagina van de ServiceNow-instantie is een samenvoeging van uw **ServiceNow-tenant-URL** en **/navpage.do** (bijvoorbeeld:`https://fabrikam.service-now.com/navpage.do`).

        * Kopie **entiteit-ID / verlener** waarde, plak deze in **id** -tekstvak in **ServiceNow basisconfiguratie SAML** sectie in Azure portal.

        * Zorg ervoor dat **NameID Policy** op de waarde `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified` is ingesteld.

        * Klik op **Advanced**. Typ in het vak **User Field** **email** of **user_name**, afhankelijk van welk veld wordt gebruikt om gebruikers in uw ServiceNow-implementatie uniek te identificeren.

            > [!NOTE]
            > U kunt Azure AD om de Azure AD-gebruikers-id (user principal name) of het e-mailadres als de unieke id in het SAML-token te gebruiken door naar de sectie **ServiceNow > Attributen > Eenmalige aanmelding** in de Azure-portal te gaan en het gewenste veld toe te wijzen aan het kenmerk **nameidentifier**. De waarde die voor het geselecteerde kenmerk wordt opgeslagen in Azure AD (bijvoorbeeld de user principal name) moet overeenkomen met de waarde die is opgeslagen in ServiceNow voor het ingevoerde veld (bijvoorbeeld user_name)

        * Klik op **Test Connection** in de rechterbovenhoek van de pagina.

        * Nadat u op **Test Connection** hebt geklikt, wordt het pop-upvenster weergegeven waarin u referenties moet invoeren, en wordt de onderstaande pagina met resultaten weergegeven. De foutmelding **SSO Logout Test Results** is verwacht; negeer deze fout en klik op de knop **Activate**.

          ![Configure Single Sign-On](./media/servicenow-tutorial/servicenowactivate.png "Configure Single Sign-On")

### <a name="configure-azure-ad-sso-for-servicenow-express"></a>Azure AD-eenmalige aanmelding voor ServiceNow snelle configureren

1. In de [Azure-portal](https://portal.azure.com/)op de **ServiceNow** toepassing integratie weergeeft, schakelt **Single Sign-On**.

    ![Koppeling voor eenmalige aanmelding configureren](common/select-sso.png)

2. Op de **Selecteer een methode voor eenmalige aanmelding** dialoogvenster **SAML/WS-Federation** modus voor eenmalige aanmelding inschakelen.

    ![Single Sign-On Selecteer modus](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<instance-name>.service-now.com/navpage.do`

    b. In het tekstvak **Id (Entiteits-id)** typt u een URL met de volgende notatie: `https://<instance-name>.service-now.com`

    > [!NOTE]
    > Dit zijn geen echte waarden. U moet deze waarden bijwerken met de werkelijke aanmeldings-URL en de id die later in de zelfstudie wordt uitgelegd. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

6. Er is een service voor configureren met één klik beschikbaar voor ServiceNow; dat wil zeggen dat Azure AD ServiceNow automatisch kan configureren voor verificatie op basis van SAML. Om deze service in te schakelen gaat u naar de sectie **ServiceNow instellen** en klikt u op **Stapsgewijze instructies weergeven** om het venster Aanmelding configureren weer te geven.

    ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/tutorial_servicenow_configure.png)

7. Voer in het formulier **Configure sign-on** de naam van uw ServiceNow-instantie, de naam van de gebruiker met beheerdersrechten en het beheerderswachtwoord in en klik op **Configure Now**. Let op: de opgegeven beheerdersgebruikersnaam moet de rol **security_admin** hebben in ServiceNow om dit te laten werken. Anders handmatig configureren van ServiceNow voor het gebruik van Azure AD als SAML-identiteitsprovider, klikt u op **handmatig configureren van eenmalige aanmelding** en kopieer de **afmeldings-URL, Azure AD-id en aanmeldings-URL** vanuit de Sectie met snelle documentatie.

    ![Configure app URL](./media/servicenow-tutorial/configure.png "Configure app URL")

### <a name="configure-servicenow-express-sso"></a>ServiceNow Express eenmalige aanmelding configureren

1. Meld u aan bij uw ServiceNow Express-toepassing als beheerder.

2. Klik in het navigatievenster aan de linkerkant op **Single Sign-On**.

    ![Configure app URL](./media/servicenow-tutorial/ic7694980ex.png "Configure app URL")

3. Klik in het dialoogvenster **Single Sign-On** op het configuratiepictogram rechtsboven en stel de volgende eigenschappen in:

    ![Configure app URL](./media/servicenow-tutorial/ic7694981ex.png "Configure app URL")

    a. Schuif de schakelaar **Enable multiple provider SSO** naar rechts.

    b. Schuif de schakelaar **Enable debug logging for the multiple provider SSO integration** naar rechts.

    c. Typ **user_name** in het tekstvak **The field on the user table that...**

4. Klik in het dialoogvenster **Single Sign-On** op **Add New Certificate**.

    ![Configure Single Sign-On](./media/servicenow-tutorial/ic7694973ex.png "Configure Single Sign-On")

5. Voer in het dialoogvenster **X.509 Certificates** de volgende stappen uit:

    ![Configure Single Sign-On](./media/servicenow-tutorial/ic7694975.png "Configure Single Sign-On")

    a. Typ in het tekstvak **Name** een naam voor de configuratie (bijvoorbeeld: **TestSAML2.0**).

    b. Selecteer **Active**.

    c. Selecteer **PEM** als **Format**.

    d. Selecteer **Trust Store Cert** als **Type**.

    e. Open uw met Base 64 gecodeerde certificaat dat u hebt gedownload in de Azure-portal, kopieer de inhoud ervan naar het Klembord en plak deze in het tekstvak **PEM Certificate**.

    f. Klik op **Update**

6. Klik in het dialoogvenster **Single Sign-On** op **Add New IdP**.

    ![Configure Single Sign-On](./media/servicenow-tutorial/ic7694976ex.png "Configure Single Sign-On")

7. Voer in het dialoogvenster **Add New Identity Provider**onder **Configure Identity Provider** de volgende stappen uit:

    ![Configure Single Sign-On](./media/servicenow-tutorial/ic7694982ex.png "Configure Single Sign-On")

    a. Typ in het tekstvak **Name** een naam voor de configuratie (bijvoorbeeld: **SAML 2.0**).

    b. Plak in het veld **Identity Provider URL** de waarde van **Identiteitsprovider-id** die u hebt gekopieerd uit de Azure-portal.

    c. Plak in het veld **Identity Provider's AuthnRequest** de waarde van **Verificatieaanvraag-URL** die u hebt gekopieerd uit de Azure-portal.

    d. Plak in het veld **Identity Provider's SingleLogoutRequest** de waarde van **URL voor afmelden** die u hebt gekopieerd uit de Azure-portal

    e. Selecteer het certificaat dat u in de vorige stap hebt gemaakt als **Identity Provider Certificate**.

8. Klik op **Advanced Settings** en voer onder **Additional Identity Provider Properties** de volgende stappen uit:

    ![Configure Single Sign-On](./media/servicenow-tutorial/ic7694983ex.png "Configure Single Sign-On")

    a. Typ **urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect** in het tekstvak **Protocol Binding for the IDP's SingleLogoutRequest**.

    b. Typ **urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified** in het tekstvak **NameID Policy**.

    c. Typ `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password` in **AuthnContextClassRef Method**.

    d. Deselecteer **Create an AuthnContextClass**.

9. Voer onder **Additional Service Provider Properties** de volgende stappen uit:

    ![Configure Single Sign-On](./media/servicenow-tutorial/ic7694984ex.png "Configure Single Sign-On")

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

In deze sectie maakt u een testgebruiker in Azure portal B.Simon genoemd.

1. Selecteer in het linkerdeelvenster in de Azure-portal, **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.
1. Selecteer **Nieuwe gebruiker** boven aan het scherm.
1. In de **gebruiker** eigenschappen als volgt te werk:
   1. Voer in het veld **Naam** `B.Simon` in.  
   1. In de **gebruikersnaam** en voer de username@companydomain.extension. Bijvoorbeeld `B.Simon@contoso.com`.
   1. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak **Wachtwoord**.
   1. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u B.Simon gebruiken Azure Single Sign-On door toegang te verlenen aan ServiceNow.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.
1. Selecteer **ServiceNow** in de lijst met toepassingen.
1. Zoek in de pagina overzicht van de app, de **beheren** sectie en selecteer **gebruikers en groepen**.

   ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. In de **gebruikers en groepen** dialoogvenster, selecteer **B.Simon** uit de lijst met gebruikers, klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Als u een waarde voor de rol in het SAML-verklaring verwacht de **rol selecteren** dialoogvenster, selecteer de juiste rol voor de gebruiker in de lijst en klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-servicenow-test-user"></a>Een testgebruiker maken in ServiceNow

Het doel van deze sectie is het maken van een gebruiker met de naam Britta Simon in ServiceNow. ServiceNow ondersteunt het automatisch inrichten van gebruikers, wat standaard is ingeschakeld. U kunt [hier](servicenow-provisioning-tutorial.md) meer informatie vinden over het configureren van het automatisch inrichten van gebruikers.

> [!NOTE]
> Als u handmatig een gebruiker moet maken, neemt u contact op met het [ondersteuningsteam van ServiceNow](https://www.servicenow.com/support/contact-support.html)

### <a name="test-sso"></a>Test eenmalige aanmelding

Wanneer u de ServiceNow-tegel in het toegangsvenster selecteert, moet u worden automatisch aangemeld bij de ServiceNow waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="test-sso-for-servicenow-classic-mobile"></a>Test eenmalige aanmelding voor ServiceNow klassiek (mobiel)

1. Open uw **ServiceNow klassiek (mobiel)** toepassing en voer de volgende stappen uit:

    a. Klik op de **toevoegen** symbool onder het scherm.

    ![De aanmelding](./media/servicenow-tutorial/test03.png)

    b. Typ de naam van uw ServiceNow-exemplaar en klik op **doorgaan**.

    ![De aanmelding](./media/servicenow-tutorial/test04.png)

    c. Op de **aanmelden** scherm, voer de volgende stappen uit:

    ![De aanmelding](./media/servicenow-tutorial/test01.png)

    *  Type **gebruikersnaam** zoals B.simon@contoso.com.

    *  Klik op **Gebruik externe aanmelding** en u wordt omgeleid naar Azure AD-pagina voor aanmelden.
    
    *  Voer uw referenties en als er verificatie van derden of een andere beveiligingsfunctie ingeschakeld en vervolgens de gebruiker moet beantwoorden en de toepassing **startpagina** wordt weergegeven zoals hieronder wordt weergegeven:

        ![De startpagina](./media/servicenow-tutorial/test02.png)

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Inrichten van gebruikers configureren](servicenow-provisioning-tutorial.md)