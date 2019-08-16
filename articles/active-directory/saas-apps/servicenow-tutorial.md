---
title: 'Zelfstudie: Azure Active Directory-integratie met eenmalige aanmelding (SSO) met ServiceNow | Microsoft Docs'
description: Ontdek hoe u eenmalige aanmelding configureert tussen Azure Active Directory en ServiceNow.
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
ms.date: 08/14/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 431d20c4c5ae5355d456ca3453b832e590cbb199
ms.sourcegitcommit: a6888fba33fc20cc6a850e436f8f1d300d03771f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2019
ms.locfileid: "69558952"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-servicenow"></a>Zelfstudie: Azure Active Directory-integratie met eenmalige aanmelding (SSO) met ServiceNow

In deze zelf studie leert u hoe u ServiceNow integreert met Azure Active Directory (Azure AD). Wanneer u ServiceNow integreert met Azure AD, kunt u het volgende doen:

* Controle in azure AD die toegang heeft tot ServiceNow.
* Zorg ervoor dat uw gebruikers automatisch worden aangemeld bij ServiceNow met hun Azure AD-accounts.
* Beheer uw accounts op één centrale locatie: de Azure Portal.

Zie [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)voor meer informatie over SaaS-app-integratie met Azure AD.

## <a name="prerequisites"></a>Vereisten

U hebt de volgende items nodig om aan de slag te gaan:

* Een Azure AD-abonnement Als u geen abonnement hebt, kunt u een [gratis account](https://azure.microsoft.com/free/)aanvragen.
* ServiceNow-abonnement dat is ingeschakeld voor eenmalige aanmelding (SSO).
* Voor ServiceNow: een instantie of tenant van ServiceNow, versie Calgary of hoger
* Voor ServiceNow Express: een instantie van ServiceNow Express, versie Helsinki of hoger
* Voor de ServiceNow-tenant moet de [invoegtoepassing Multiple-Provider single sign-on (SSO)](https://wiki.servicenow.com/index.php?title=Multiple_Provider_Single_Sign-On#gsc.tab=0) zijn ingeschakeld. Dit kan worden gedaan door het [indienen van een serviceaanvraag](https://hi.service-now.com).
* Schakel voor automatische configuratie de multi-provider-invoegtoepassing voor ServiceNow in.
* Als u de ServiceNow klassieke (Mobile) toepassing wilt installeren, gaat u naar de juiste Store en zoekt u naar de klassieke ServiceNow-toepassing en klikt u op downloaden.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelf studie configureert en test u Azure AD SSO in een test omgeving. ServiceNow ondersteunt door **SP** geïnitieerde SSO en ondersteunt [ **geautomatiseerde** gebruikers inrichting](servicenow-provisioning-tutorial.md).

De toepassing ServiceNow Classic (Mobile) kan nu worden geconfigureerd met Azure AD voor het inschakelen van SSO en het ondersteunt zowel **Android** -als **IOS** -gebruikers. In deze zelf studie configureert en test u Azure AD SSO in een test omgeving.

## <a name="adding-servicenow-from-the-gallery"></a>ServiceNow toevoegen vanuit de galerie

Voor het configureren van de integratie van ServiceNow in Azure AD moet u ServiceNow uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer de **Azure Active Directory** -service in het navigatie deel venster aan de linkerkant.
1. Ga naar **bedrijfs toepassingen** en selecteer **alle toepassingen**.
1. Selecteer **nieuwe toepassing**om een nieuwe toepassing toe te voegen.
1. Typ in de sectie **toevoegen vanuit de galerie** **ServiceNow** in het zoekvak.
1. Selecteer **ServiceNow** uit het paneel resultaten en voeg vervolgens de app toe. Wacht een paar seconden wanneer de app aan uw Tenant is toegevoegd.

## <a name="configure-and-test-azure-ad-single-sign-on-for-servicenow"></a>Eenmalige aanmelding voor Azure AD configureren en testen voor ServiceNow

Azure AD SSO met ServiceNow configureren en testen met behulp van een test gebruiker met de naam **B. Simon**. Voor het werken met SSO moet u een koppelings relatie tot stand brengen tussen een Azure AD-gebruiker en de bijbehorende gebruiker in ServiceNow.

Als u Azure AD SSO wilt configureren en testen met ServiceNow, voltooit u de volgende bouw stenen:

1. **[Configureer Azure AD SSO](#configure-azure-ad-sso)** zodat uw gebruikers deze functie kunnen gebruiken.
    1. **[Maak een Azure AD-test gebruiker om de](#create-an-azure-ad-test-user)** eenmalige aanmelding van Azure ad te testen met B. Simon.
    1. **[Wijs de Azure AD-test gebruiker](#assign-the-azure-ad-test-user)** toe om B. Simon in te scha kelen voor het gebruik van eenmalige aanmelding voor Azure AD.
    1. **[Configureer Azure AD SSO voor ServiceNow Express](#configure-azure-ad-sso-for-servicenow-express)** -om uw gebruikers in staat te stellen deze functie te gebruiken.
2. **[Configureer ServiceNow](#configure-servicenow)** om de SSO-instellingen aan de kant van de toepassing te configureren.
    1. **[Maak een ServiceNow-test gebruiker](#create-servicenow-test-user)** voor het maken van een equivalent van B. Simon in ServiceNow dat is gekoppeld aan de Azure AD-representatie van de gebruiker.
    1. **[ServiceNow Express SSO configureren](#configure-servicenow-express-sso)** : voor het configureren van de instellingen voor eenmalige aanmelding aan de kant van de toepassing.    
3. **[Test SSO](#test-sso)** om te controleren of de configuratie werkt.
4. **[Test SSO voor ServiceNow Classic (Mobile)](#test-sso-for-servicenow-classic-mobile)** om te controleren of de configuratie werkt.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO configureren

Volg deze stappen om Azure AD SSO in te scha kelen in de Azure Portal.

1. Zoek in het [Azure Portal](https://portal.azure.com/)op de pagina Toepassings integratie van **ServiceNow** de sectie **beheren** en selecteer **eenmalige aanmelding**.
1. Selecteer op de pagina **Eén aanmeldings methode selecteren** de optie **SAML**.
1. Klik op de pagina **eenmalige aanmelding met SAML instellen** op het pictogram bewerken/pen voor **eenvoudige SAML-configuratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<instance-name>.service-now.com/navpage.do`

    b. In het tekstvak **Id (Entiteits-id)** typt u een URL met de volgende notatie: `https://<instance-name>.service-now.com`

    > [!NOTE]
    > Dit zijn geen echte waarden. U moet deze waarden bijwerken met de werkelijke aanmeldings-URL en de id die verderop in de zelf studie wordt uitgelegd. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

1. Zoek op de pagina **eenmalige aanmelding met SAML instellen** , in de sectie **SAML-handtekening certificaat** , naar **certificaat (base64)** en selecteer **downloaden** om het certificaat te downloaden en op uw computer op te slaan.

   ![De downloadkoppeling certificaat](common/certificatebase64.png)

   a. Klik op de knop Kopiëren om de **App-URL voor federatieve metagegevens** te kopiëren en in Kladblok te plakken, want deze wordt later in de zelfstudie gebruikt.

    b. Klik op **Downloaden** om het **Certificaat (Base64)** te downloaden en het certificaatbestand op te slaan op uw computer.

1. Op de sectie **ServiceNow instellen** kopieert u de gewenste URL ('s) op basis van uw vereiste.

   ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

In deze sectie maakt u een test gebruiker in de Azure Portal met de naam B. Simon.

1. Selecteer in het linkerdeel venster van de Azure Portal **Azure Active Directory**, selecteer **gebruikers**en selecteer vervolgens **alle gebruikers**.
1. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm.
1. Voer de volgende stappen uit in de eigenschappen van de **gebruiker** :
   1. Voer in het veld **Naam** `B.Simon` in.  
   1. Voer in het veld **gebruikers naam** het username@companydomain.extensionin. Bijvoorbeeld `B.Simon@contoso.com`.
   1. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak **Wachtwoord**.
   1. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u B. Simon in om eenmalige aanmelding van Azure te gebruiken door toegang te verlenen aan ServiceNow.

1. Selecteer in het Azure Portal **bedrijfs toepassingen**en selecteer vervolgens **alle toepassingen**.
1. Selecteer **ServiceNow** in de lijst met toepassingen.
1. Ga op de pagina overzicht van de app naar de sectie **beheren** en selecteer **gebruikers en groepen**.

   ![De koppeling 'Gebruikers en groepen'](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer vervolgens **gebruikers en groepen** in het dialoog venster **toewijzing toevoegen** .

    ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. Selecteer in het dialoog venster **gebruikers en groepen** **B. Simon** van de lijst gebruikers en klik vervolgens op de knop **selecteren** onder aan het scherm.
1. Als u een wille keurige rol verwacht in de SAML-bewering, selecteert u in het dialoog venster **rol selecteren** de juiste rol voor de gebruiker in de lijst en klikt u op de knop **selecteren** onder aan het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="configure-azure-ad-sso-for-servicenow-express"></a>Azure AD SSO voor ServiceNow Express configureren

1. Selecteer in de [Azure Portal](https://portal.azure.com/)op de pagina **ServiceNow** Application Integration de optie **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren](common/select-sso.png)

2. Selecteer in het dialoog venster **eenmalige aanmelding selecteren** de optie **SAML/WS-** gegevensinvoermodus om eenmalige aanmelding in te scha kelen.

    ![modus voor eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Klik op de pagina **eenmalige aanmelding met SAML instellen** op pictogram **bewerken** om het dialoog venster **basis configuratie van SAML** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<instance-name>.service-now.com/navpage.do`

    b. In het tekstvak **Id (Entiteits-id)** typt u een URL met de volgende notatie: `https://<instance-name>.service-now.com`

    > [!NOTE]
    > Dit zijn geen echte waarden. U moet deze waarden bijwerken met de werkelijke aanmeldings-URL en de id die verderop in de zelf studie wordt uitgelegd. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Klik op de pagina **eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekening certificaat** op **downloaden** om het **certificaat (base64)** te downloaden van de opgegeven opties volgens uw vereiste en op uw computer op te slaan.

    ![De downloadkoppeling certificaat](common/certificatebase64.png)

6. Er is een service voor configureren met één klik beschikbaar voor ServiceNow; dat wil zeggen dat Azure AD ServiceNow automatisch kan configureren voor verificatie op basis van SAML. Om deze service in te schakelen gaat u naar de sectie **ServiceNow instellen** en klikt u op **Stapsgewijze instructies weergeven** om het venster Aanmelding configureren weer te geven.

    ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/tutorial_servicenow_configure.png)

7. Voer in het formulier **Configure sign-on** de naam van uw ServiceNow-instantie, de naam van de gebruiker met beheerdersrechten en het beheerderswachtwoord in en klik op **Configure Now**. Zorg ervoor dat de naam van de door de beheerder aangestelde beheerdersrol de rol **security_admin** heeft toegewezen in ServiceNow, zodat dit werkt. Als u ServiceNow hand matig wilt configureren voor het gebruik van Azure AD als een SAML-ID-provider, klikt u op **eenmalige aanmelding hand matig configureren** en kopieert u de afmeldings **-URL, de Azure ad-id en de aanmeldings-URL** uit de sectie snelle verwijzing.

    ![Configure app URL](./media/servicenow-tutorial/configure.png "Configure app URL")

## <a name="configure-servicenow"></a>ServiceNow configureren

1. Meld u aan bij uw ServiceNow-toepassing als beheerder.

2. Activeer de invoeg toepassing voor de **installatie van eenmalige aanmelding met meerdere providers** door de volgende stappen uit te voeren:

    a. Gebruik de zoekbalk in het navigatievenster aan de linkerkant om de sectie **System Definition** op te zoeken, en klik op **Plugins**.

    ![Activate plugin](./media/servicenow-tutorial/tutorial_servicenow_03.png "Activate plugin")

    b. Zoek naar **integratie-installatie programma voor eenmalige aanmelding met meerdere providers**.

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

    * Voer in het formulier **Configure sign-on** de naam van uw ServiceNow-instantie, de naam van de gebruiker met beheerdersrechten en het beheerderswachtwoord in en klik op **Configure Now**. Zorg ervoor dat de naam van de door de beheerder aangestelde beheerdersrol de rol **security_admin** heeft toegewezen in ServiceNow, zodat dit werkt. Als u ServiceNow hand matig wilt configureren voor het gebruik van Azure AD als een SAML-ID-provider, klikt u op **eenmalige aanmelding hand matig configureren** en kopieert u de **URL voor de afmelding, id van de SAML-entiteit en de SSO-service voor eenmalige aanmelding** via het gedeelte snelle verwijzing.

        ![Configure app URL](./media/servicenow-tutorial/configure.png "Configure app URL")

    * Meld u aan bij uw ServiceNow-toepassing als beheerder.

    * Bij de automatische configuratie worden alle benodigde instellingen aan de kant van **ServiceNow** geconfigureerd, maar het **X.509 Certificate** wordt standaard niet ingeschakeld. U moet het handmatig toewijzen aan uw Identiteitsprovider in ServiceNow. Volg de onderstaande stappen om dit te doen:

    * Gebruik de zoekbalk in het navigatievenster aan de linkerkant om de sectie **Multi-Provider SSO** op te zoeken, en klik op **Identity Providers**.

        ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/tutorial_servicenow_07.png "Eenmalige aanmelding configureren")

    * Klik op de automatisch gegenereerde identiteitsprovider

        ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/tutorial_servicenow_08.png "Eenmalige aanmelding configureren")

    *  Voer in de sectie **Identity Provider** de volgende stappen uit:

        ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/automatic_config.png "Eenmalige aanmelding configureren")

        * Typ in het tekstvak **naam** een naam voor uw configuratie (bijvoorbeeld **Microsoft Azure federatieve eenmalige aanmelding**).

        * Verwijder de standaard ingevulde waarde uit het tekstvak **Identity Provider's SingleLogoutRequest**.

        * Kopieer de waarde van de **Start pagina van ServiceNow** , plak deze in het tekstvak voor de **aanmeldings-URL** in het gedeelte **ServiceNow Basic SAML-configuratie** op Azure Portal.

            > [!NOTE]
            > De startpagina van de ServiceNow-instantie is een samenvoeging van uw **ServiceNow-tenant-URL** en **/navpage.do** (bijvoorbeeld:`https://fabrikam.service-now.com/navpage.do`).

        * Kopieer **Entiteits-ID/** waarde van verlener, plak deze in de sectie **id** -TEKSTVAK in **ServiceNow Basic SAML-configuratie** op Azure Portal.

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

        * Klik op **importeren**.

    * De URL met de IdP-metagegevens wordt gelezen en de overeenkomende velden worden ingevuld.

        ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/ic7694982.png "Eenmalige aanmelding configureren")

        * Typ in het tekstvak **naam** een naam voor uw configuratie (bijvoorbeeld **Microsoft Azure federatieve eenmalige aanmelding**).

        * Verwijder de standaard ingevulde waarde uit het tekstvak **Identity Provider's SingleLogoutRequest**.

        * Kopieer de waarde van de **Start pagina van ServiceNow** , plak deze in het tekstvak voor de **aanmeldings-URL** in het gedeelte **ServiceNow Basic SAML-configuratie** op Azure Portal.

            > [!NOTE]
            > De startpagina van de ServiceNow-instantie is een samenvoeging van uw **ServiceNow-tenant-URL** en **/navpage.do** (bijvoorbeeld:`https://fabrikam.service-now.com/navpage.do`).

        * Kopieer **Entiteits-ID/** waarde van verlener, plak deze in de sectie **id** -TEKSTVAK in **ServiceNow Basic SAML-configuratie** op Azure Portal.

        * Zorg ervoor dat **NameID Policy** op de waarde `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified` is ingesteld.

        * Klik op **Advanced**. Typ in het vak **User Field** **email** of **user_name**, afhankelijk van welk veld wordt gebruikt om gebruikers in uw ServiceNow-implementatie uniek te identificeren.

            > [!NOTE]
            > U kunt Azure AD configureren voor het verzenden van de Azure AD-gebruikers-ID (user principal name) of het e-mail adres als de unieke id in het SAML-token door naar de **ServiceNow > kenmerken > eenmalige aanmelding** van de Azure portal te gaan en de toewijzing van de gewenst veld aan het **nameidentifier** -kenmerk. De waarde die voor het geselecteerde kenmerk wordt opgeslagen in Azure AD (bijvoorbeeld de user principal name) moet overeenkomen met de waarde die is opgeslagen in ServiceNow voor het ingevoerde veld (bijvoorbeeld user_name)

        * Klik op **Test Connection** in de rechterbovenhoek van de pagina.

        * Nadat u op **Test Connection** hebt geklikt, wordt het pop-upvenster weergegeven waarin u referenties moet invoeren, en wordt de onderstaande pagina met resultaten weergegeven. De foutmelding **SSO Logout Test Results** is verwacht; negeer deze fout en klik op de knop **Activate**.

          ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/servicenowactivate.png "Eenmalige aanmelding configureren")

### <a name="create-servicenow-test-user"></a>Een testgebruiker maken in ServiceNow

Het doel van deze sectie is het maken van een gebruiker met de naam Britta Simon in ServiceNow. ServiceNow ondersteunt het automatisch inrichten van gebruikers, wat standaard is ingeschakeld. U kunt [hier](servicenow-provisioning-tutorial.md) meer informatie vinden over het configureren van het automatisch inrichten van gebruikers.

> [!NOTE]
> Als u handmatig een gebruiker moet maken, neemt u contact op met het [ondersteuningsteam van ServiceNow](https://www.servicenow.com/support/contact-support.html)

### <a name="configure-servicenow-express-sso"></a>ServiceNow Express SSO configureren

1. Meld u aan bij uw ServiceNow Express-toepassing als beheerder.

2. Klik in het navigatie deel venster aan de linkerkant op **eenmalige aanmelding**.

    ![Configure app URL](./media/servicenow-tutorial/ic7694980ex.png "Configure app URL")

3. Klik in het dialoog venster **eenmalige aanmelding** op het configuratie pictogram in de rechter bovenhoek en stel de volgende eigenschappen in:

    ![Configure app URL](./media/servicenow-tutorial/ic7694981ex.png "Configure app URL")

    a. Schuif de schakelaar **Enable multiple provider SSO** naar rechts.

    b. Schuif de schakelaar **Enable debug logging for the multiple provider SSO integration** naar rechts.

    c. Typ **user_name** in het tekstvak **The field on the user table that...**

4. Klik in het dialoog venster **eenmalige aanmelding** op **Nieuw certificaat toevoegen**.

    ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/ic7694973ex.png "Eenmalige aanmelding configureren")

5. Voer in het dialoogvenster **X.509 Certificates** de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/ic7694975.png "Eenmalige aanmelding configureren")

    a. Typ in het tekstvak **Name** een naam voor de configuratie (bijvoorbeeld: **TestSAML2.0**).

    b. Selecteer **Active**.

    c. Selecteer **PEM** als **Format**.

    d. Selecteer **Trust Store Cert** als **Type**.

    e. Open uw met Base 64 gecodeerde certificaat dat u hebt gedownload in de Azure-portal, kopieer de inhoud ervan naar het Klembord en plak deze in het tekstvak **PEM Certificate**.

    f. Klik op **Update**

6. Klik in het dialoog venster **eenmalige aanmelding** op **nieuwe IDP toevoegen**.

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
    > U kunt Azure AD configureren voor het verzenden van de Azure AD-gebruikers-ID (user principal name) of het e-mail adres als de unieke id in het SAML-token door naar de **ServiceNow > kenmerken > eenmalige aanmelding** van de Azure portal te gaan en de toewijzing van de gewenst veld aan het **nameidentifier** -kenmerk. De waarde die voor het geselecteerde kenmerk wordt opgeslagen in Azure AD (bijvoorbeeld de user principal name) moet overeenkomen met de waarde die is opgeslagen in ServiceNow voor het ingevoerde veld (bijvoorbeeld user_name)

    f. Klik op **Opslaan**.

## <a name="test-sso"></a>SSO testen

Wanneer u de tegel ServiceNow in het toegangs venster selecteert, wordt u automatisch aangemeld bij de ServiceNow waarvoor u SSO hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="test-sso-for-servicenow-classic-mobile"></a>SSO testen voor ServiceNow Classic (mobiel)

1. Open de toepassing **ServiceNow Classic (Mobile)** en voer de volgende stappen uit:

    a. Klik op het symbool **toevoegen** onder het scherm.

    ![De aanmelding](./media/servicenow-tutorial/test03.png)

    b. Typ de naam van uw ServiceNow-exemplaar en klik op **door gaan**.

    ![De aanmelding](./media/servicenow-tutorial/test04.png)

    c. Voer de volgende stappen uit op het scherm **Aanmelden** :

    ![De aanmelding](./media/servicenow-tutorial/test01.png)

    *  Typ de **gebruikers naam** zoals B.simon@contoso.com.

    *  Klik op **externe aanmelding gebruiken** en u wordt omgeleid naar de Azure AD-pagina om u aan te melden.
    
    *  Voer uw referenties in en als er sprake is van een verificatie van derden of een andere beveiligings functie die is ingeschakeld, moet de gebruiker op de juiste wijze reageren en de **Start pagina** van de toepassing worden weer gegeven, zoals hieronder wordt aangegeven:

        ![De start pagina](./media/servicenow-tutorial/test02.png)

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Inrichten van gebruikers configureren](servicenow-provisioning-tutorial.md)

- [Probeer ServiceNow met Azure AD](https://aad.portal.azure.com)