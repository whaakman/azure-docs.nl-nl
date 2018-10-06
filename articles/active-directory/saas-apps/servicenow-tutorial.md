---
title: 'Zelfstudie: Azure Active Directory-integratie met ServiceNow | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en ServiceNow.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: a5a1a264-7497-47e7-b129-a1b5b1ebff5b
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2018
ms.author: jeedes
ms.openlocfilehash: 470805b2bb77e367887767b95e0f1e04d79c8f9d
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/06/2018
ms.locfileid: "48830732"
---
# <a name="tutorial-azure-active-directory-integration-with-servicenow"></a>Zelfstudie: Azure Active Directory-integratie met ServiceNow

In deze zelfstudie leert u hoe u ServiceNow integreren met Azure Active Directory (Azure AD).

ServiceNow integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot ServiceNow heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij ServiceNow (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereiste onderdelen

Voor het Azure AD-integratie configureren met ServiceNow, moet u de volgende items:

- Een Azure AD-abonnement
- Voor ServiceNow, een exemplaar of een tenant van ServiceNow, Calgary versie of hoger
- Voor snelle ServiceNow, een exemplaar van ServiceNow Express, Helsinki versie of hoger
- De ServiceNow-tenant moet beschikken over de [meerdere Provider eenmalige aanmelding op invoegtoepassing](http://wiki.servicenow.com/index.php?title=Multiple_Provider_Single_Sign-On#gsc.tab=0) ingeschakeld. Dit kan worden gedaan door [indienen van een serviceaanvraag](https://hi.service-now.com).
- Schakel de invoegtoepassing multi-provider voor ServiceNow voor automatische configuratie.

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving.
Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. ServiceNow uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-servicenow-from-the-gallery"></a>ServiceNow uit de galerie toe te voegen

Voor het configureren van de integratie van ServiceNow in Azure AD, moet u ServiceNow uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen ServiceNow uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **ServiceNow**, selecteer **ServiceNow** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![ServiceNow in de lijst met resultaten](./media/servicenow-tutorial/tutorial_servicenow_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met ServiceNow op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in ServiceNow is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in ServiceNow tot stand worden gebracht.

In ServiceNow, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met ServiceNow, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren voor ServiceNow](#configure-azure-ad-single-sign-on-for-servicenow)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Azure AD eenmalige aanmelding configureren voor ServiceNow snelle](#configure-azure-ad-single-sign-on-for-servicenow-express)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
3. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
4. **[Maak een testgebruiker ServiceNow](#create-a-servicenow-test-user)**  : als u wilt een equivalent van Britta Simon in ServiceNow die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
5. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on-for-servicenow"></a>Azure AD voor eenmalige aanmelding configureren voor ServiceNow

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw ServiceNow-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met ServiceNow, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **ServiceNow** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Klik op **modus voor één wijziging aanmelding** boven op het scherm selecteren de **SAML** modus.

    ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/tutorial_general_300.png)

3. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster, klikt u op **Selecteer** voor **SAML** modus voor eenmalige aanmelding inschakelen.

    ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/tutorial_general_301.png)

4. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** te openen **SAML-basisconfiguratie** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/tutorial_general_302.png)

5. Op de **SAML-basisconfiguratie** sectie, voert u de volgende stappen uit:

    ![ServiceNow domein en URL's, eenmalige aanmelding informatie](./media/servicenow-tutorial/tutorial_servicenow_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<instance-name>.service-now.com/navpage.do`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<instance-name>.service-now.com`

    > [!NOTE]
    > Deze waarden zijn niet echt. U moet deze waarden uit de werkelijke aanmeldings-URL en de id die later in de zelfstudie wordt uitgelegd bijwerken.

6. Op de **SAML-handtekeningcertificaat** sectie, voert u de volgende stappen uit:

    ![De downloadkoppeling certificaat](./media/servicenow-tutorial/tutorial_servicenow_certificate.png)

    a. Klik op de knop kopiëren om te kopiëren **App-Url voor federatieve metagegevens** en plak deze in Kladblok, als deze App-Url voor federatieve metagegevens wordt later in de zelfstudie worden gebruikt.

    b. Klik op **downloaden** downloaden **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

7. Meld u aan bij uw ServiceNow-toepassing als beheerder.

8. Activeer de **integratie - meerdere Single Sign-On Providerinstallatieprogramma** invoegtoepassing door de volgende stappen te volgen:

    a. Zoek in het navigatiedeelvenster aan de linkerkant **System Definition** sectie uit de zoekbalk en klik vervolgens op **invoegtoepassingen**.

    ![Activeren van de invoegtoepassing](./media/servicenow-tutorial/tutorial_servicenow_03.png "invoegtoepassing activeren")

     b. Zoeken naar **integratie - meerdere Single Sign-On Providerinstallatieprogramma**.

     ![Activeren van de invoegtoepassing](./media/servicenow-tutorial/tutorial_servicenow_04.png "invoegtoepassing activeren")

    c. Selecteer de invoegtoepassing. Klik met de rechtermuisknop en selecteer **activeren/Upgrade**.

     ![Activeren van de invoegtoepassing](./media/servicenow-tutorial/tutorial_activate.png "invoegtoepassing activeren")

    d. Klik op de **activeren** knop.

     ![Activeren van de invoegtoepassing](./media/servicenow-tutorial/tutorial_activate1.png "invoegtoepassing activeren")

9. Zoek in het navigatiedeelvenster aan de linkerkant **multi-provider SSO** sectie uit de zoekbalk en klik vervolgens op **eigenschappen**.

    ![App-URL configureren](./media/servicenow-tutorial/tutorial_servicenow_06.png "app-URL configureren")

10. Op de **meerdere eigenschappen van de Provider SSO** dialoogvenster, voer de volgende stappen uit:

    ![App-URL configureren](./media/servicenow-tutorial/ic7694981.png "app-URL configureren")

    * Als **meerdere provider SSO inschakelen**, selecteer **Ja**.
  
    * Als **inschakelen het automatisch importeren van de gebruikers van alle id-providers in de gebruikerstabel**, selecteer **Ja**.

    * Als **logboekregistratie voor de provider die meerdere SSO-integratie voor het inschakelen van foutopsporing**, selecteer **Ja**.

    * In **het veld voor de gebruiker die tabel...**  tekstvak, type **gebruikersnaam**.
  
    * Klik op **Opslaan**.

11. Er zijn twee manieren waarop **ServiceNow** kan worden geconfigureerd - Automatic en Manual.

12. Voor het configureren van **ServiceNow** automatisch, volg de onderstaande stappen te volgen:

    * Ga terug naar de **ServiceNow** eenmalige aanmelding op de pagina in de Azure portal.

    * Met één klik service configureren is opgegeven voor ServiceNow dat wil zeggen, hebt u Azure AD automatisch configureren ServiceNow voor verificatie op basis van SAML. Om in te schakelen deze service gaat u naar **ServiceNow configuratie** sectie, klikt u op **ServiceNow configureren** om configureren aanmeldings-venster te openen.

        ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/tutorial_servicenow_configure.png)

    * Voer de naam van het ServiceNow-exemplaar, gebruikersnaam, en beheerders beheerderswachtwoord in de **aanmelding configureren** vormen en klikt u op **nu configureren**. Houd er rekening mee dat de opgegeven beheerdersgebruikersnaam moet de **security_admin** rol die is toegewezen in ServiceNow voor deze om te werken. Anders handmatig configureren van ServiceNow voor het gebruik van Azure AD als SAML-identiteitsprovider, klikt u op **handmatig configureren van eenmalige aanmelding** en kopieer de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de sectie ter referentie.

        ![App-URL configureren](./media/servicenow-tutorial/configure.png "app-URL configureren")

    * Meld u aan bij uw ServiceNow-toepassing als beheerder.

    * In de configuratie voor automatisch de vereiste instellingen zijn geconfigureerd op de **ServiceNow** aan clientzijde, maar de **X.509-certificaat** is niet standaard ingeschakeld. U moet deze handmatig toewijzen aan uw id-Provider in ServiceNow. Volg de onderstaande stappen voor dezelfde:

    * Zoek in het navigatiedeelvenster aan de linkerkant **multi-provider SSO** sectie uit de zoekbalk en klik vervolgens op **id-Providers**.

        ![Configureren van eenmalige aanmelding](./media/servicenow-tutorial/tutorial_servicenow_07.png "eenmalige aanmelding configureren")

    * Klik op de automatisch gegenereerde id-Provider

        ![Configureren van eenmalige aanmelding](./media/servicenow-tutorial/tutorial_servicenow_08.png "eenmalige aanmelding configureren")

    *  Op de **id-Provider** sectie, voert u de volgende stappen uit:

        ![Configureren van eenmalige aanmelding](./media/servicenow-tutorial/automatic_config.png "eenmalige aanmelding configureren")

        * In de **naam** tekstvak, typ een naam voor uw configuratie (bijvoorbeeld **Microsoft Azure voor federatieve eenmalige aanmelding**).

        * Verwijder de ingevuld **id-Provider SingleLogoutRequest** waarde van het tekstvak.

        * Kopie **ServiceNow Homepage** waarde, plak deze in de **aanmeldings-URL** -tekstvak in **ServiceNow domein en URL's** sectie in Azure portal.

            > [!NOTE]
            > De startpagina van de ServiceNow-exemplaar is een samenvoeging van uw **ServieNow tenant-URL** en **/navpage.do** (bijvoorbeeld:`https://fabrikam.service-now.com/navpage.do`).

        * Kopie **entiteit-ID / verlener** waarde, plak deze in **id** -tekstvak in **ServiceNow domein en URL's** sectie in Azure portal.

        * Zorg ervoor dat **NameID beleid** is ingesteld op `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified` waarde. 

    * Schuif omlaag naar de **X.509-certificaat** sectie, selecteer **bewerken**.

        ![Configureren van eenmalige aanmelding](./media/servicenow-tutorial/tutorial_servicenow_09.png "eenmalige aanmelding configureren")

    * Selecteer op het certificaat en klik op het pictogram pijl-rechts om toe te voegen van het certificaat

        ![Configureren van eenmalige aanmelding](./media/servicenow-tutorial/tutorial_servicenow_11.png "eenmalige aanmelding configureren")

    * Klik op **Opslaan**.

    * Klik op **testverbinding** in de rechterbovenhoek van de pagina.

        ![Activeren van de invoegtoepassing](./media/servicenow-tutorial/tutorial_activate2.png "invoegtoepassing activeren")

    * Nadat de gebruiker op de **testverbinding**, krijgt u de pop-upvenster waarin u moet referenties in te voeren en de volgende pagina met resultaten wordt weergegeven. De **SSO afmelden testresultaten** fout wordt verwacht de fout negeren en klikt u op **activeren** knop.

        ![Configureren van eenmalige aanmelding](./media/servicenow-tutorial/servicenowactivate.png "eenmalige aanmelding configureren")
  
13. Voor het configureren van **ServiceNow** handmatig, volg de onderstaande stappen te volgen:

    * Meld u aan bij uw ServiceNow-toepassing als beheerder.

    * Klik in het navigatiedeelvenster aan de linkerkant op **id-Providers**.

        ![Configureren van eenmalige aanmelding](./media/servicenow-tutorial/tutorial_servicenow_07.png "eenmalige aanmelding configureren")

    * Op de **id-Providers** dialoogvenster, klikt u op **nieuw**.

        ![Configureren van eenmalige aanmelding](./media/servicenow-tutorial/ic7694977.png "eenmalige aanmelding configureren")

    * Op de **id-Providers** dialoogvenster, klikt u op **SAML**.

        ![Configureren van eenmalige aanmelding](./media/servicenow-tutorial/ic7694978.png "eenmalige aanmelding configureren")

    * Op de **metagegevens importeren id-Provider** pop-upvenster de volgende stappen uitvoeren:

        ![Configureren van eenmalige aanmelding](./media/servicenow-tutorial/idp.png "eenmalige aanmelding configureren")

        * Voer de **App-Url voor federatieve metagegevens** die u hebt gekopieerd vanuit Azure portal.

        * Klik op **Import**.

    * Leest de IdP metagegevens-URL en wordt alle informatie van de velden ingevuld.

        ![Configureren van eenmalige aanmelding](./media/servicenow-tutorial/ic7694982.png "eenmalige aanmelding configureren")

        * In de **naam** tekstvak, typ een naam voor uw configuratie (bijvoorbeeld **Microsoft Azure voor federatieve eenmalige aanmelding**).

        * Verwijder de ingevuld **id-Provider SingleLogoutRequest** waarde van het tekstvak.

        * Kopie **ServiceNow Homepage** waarde, plak deze in de **aanmeldings-URL** -tekstvak in **ServiceNow domein en URL's** sectie in Azure portal.

            > [!NOTE]
            > De startpagina van de ServiceNow-exemplaar is een samenvoeging van uw **ServiceNow tenant-URL** en **/navpage.do** (bijvoorbeeld:`https://fabrikam.service-now.com/navpage.do`).

        * Kopie **entiteit-ID / verlener** waarde, plak deze in **id** -tekstvak in **ServiceNow domein en URL's** sectie in Azure portal.

        * Zorg ervoor dat **NameID beleid** is ingesteld op `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified` waarde.

        * Klik op **geavanceerde**. In de **Gebruikersveld** tekstvak, type **e** of **gebruikersnaam**, afhankelijk van welk veld wordt gebruikt voor het aanduiden van gebruikers in uw ServiceNow-implementatie.

            > [!NOTE]
            > U kunt Azure AD configureren voor de Azure AD-gebruikers-ID (UPN) of het e-mailadres verzenden als de unieke id in het SAML-token door te gaan naar de **ServiceNow > kenmerken > Single Sign-On** sectie van de Azure portal en het toewijzen van het gewenste veld voor de **nameidentifier** kenmerk. De waarde die voor het geselecteerde kenmerk wordt opgeslagen in Azure AD (bijvoorbeeld naam user principal name) moet overeenkomen met de waarde die is opgeslagen in ServiceNow voor het opgegeven veld (bijvoorbeeld gebruikersnaam)

        * Klik op **testverbinding** in de rechterbovenhoek van de pagina.

        * Nadat de gebruiker op de **testverbinding**, krijgt u de pop-upvenster waarin u moet referenties in te voeren en de volgende pagina met resultaten wordt weergegeven. De **SSO afmelden testresultaten** fout wordt verwacht de fout negeren en klikt u op **activeren** knop.

          ![Configureren van eenmalige aanmelding](./media/servicenow-tutorial/servicenowactivate.png "eenmalige aanmelding configureren")

### <a name="configure-azure-ad-single-sign-on-for-servicenow-express"></a>Azure AD voor eenmalige aanmelding voor ServiceNow snelle configureren

1. In de Azure-portal op de **ServiceNow** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Klik op **modus voor één wijziging aanmelding** boven op het scherm selecteren de **SAML** modus.

    ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/tutorial_general_300.png)

3. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster, klikt u op **Selecteer** voor **SAML** modus voor eenmalige aanmelding inschakelen.

    ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/tutorial_general_301.png)

4. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** te openen **SAML-basisconfiguratie** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/tutorial_general_302.png)

5. Op de **SAML-basisconfiguratie** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/tutorial_servicenow_url.png)

    a. In de **aanmeldings-URL** tekstvak typt u de waarde met behulp van het volgende patroon: `https://<instance-name>.service-now.com/navpage.do`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<instance-name>.service-now.com`

    > [!NOTE]
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke aanmeldings-URL en -id. Neem contact op met [ServiceNow-Client-ondersteuningsteam](https://www.servicenow.com/support/contact-support.html) om deze waarden te verkrijgen.

6. Op de **SAML-handtekeningcertificaat** sectie, klikt u op op **downloaden** downloaden **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/tutorial_servicenow_certificates.png)

7. Met één klik service configureren is opgegeven voor ServiceNow dat wil zeggen, hebt u Azure AD automatisch configureren ServiceNow voor verificatie op basis van SAML. Om in te schakelen deze service gaat u naar **instellen ServiceNow** sectie, klikt u op **Stapsgewijze instructies bekijken over** om configureren aanmeldings-venster te openen.

    ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/tutorial_servicenow_configure.png)

8. Voer de naam van het ServiceNow-exemplaar, gebruikersnaam, en beheerders beheerderswachtwoord in de **aanmelding configureren** vormen en klikt u op **nu configureren**. Houd er rekening mee dat de opgegeven beheerdersgebruikersnaam moet de **security_admin** rol die is toegewezen in ServiceNow voor deze om te werken. Anders handmatig configureren van ServiceNow voor het gebruik van Azure AD als SAML-identiteitsprovider, klikt u op **handmatig configureren van eenmalige aanmelding** en kopieer de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de sectie ter referentie.

    ![App-URL configureren](./media/servicenow-tutorial/configure.png "app-URL configureren")

9. Meld u aan bij uw ServiceNow-Express-toepassing als beheerder.

10. Klik in het navigatiedeelvenster aan de linkerkant op **Single Sign-On**.

    ![App-URL configureren](./media/servicenow-tutorial/ic7694980ex.png "app-URL configureren")

11. Op de **Single Sign-On** dialoogvenster, klikt u op het configuratiepictogram in de rechterbovenhoek en stel de volgende eigenschappen:

    ![App-URL configureren](./media/servicenow-tutorial/ic7694981ex.png "app-URL configureren")

    a. In-/ uitschakelen **meerdere provider SSO inschakelen** aan de rechterkant.

    b. In-/ uitschakelen **inschakelen foutopsporing SSO-integratie-registratie voor de provider van meerdere** aan de rechterkant.

    c. In **het veld voor de gebruiker die tabel...**  tekstvak, type **gebruikersnaam**.

12. Op de **Single Sign-On** dialoogvenster, klikt u op **nieuw certificaat toevoegen**.

    ![Configureren van eenmalige aanmelding](./media/servicenow-tutorial/ic7694973ex.png "eenmalige aanmelding configureren")

13. Op de **X.509-certificaten** dialoogvenster, voer de volgende stappen uit:

    ![Configureren van eenmalige aanmelding](./media/servicenow-tutorial/ic7694975.png "eenmalige aanmelding configureren")

    a. In de **naam** tekstvak, typ een naam voor uw configuratie (bijvoorbeeld: **TestSAML2.0**).

    b. Selecteer **Active**.

    c. Als **indeling**, selecteer **PEM**.

    d. Als **Type**, selecteer **Store-certificaat vertrouwen**.

    e. Open uw Base64-gecodeerd certificaat gedownload vanuit Azure portal in Kladblok, Kopieer de inhoud ervan in het Klembord en plakt u deze de **PEM certificaat** tekstvak.

    f. Klik op **Update**

14. Op de **Single Sign-On** dialoogvenster, klikt u op **nieuwe IdP toevoegen**.

    ![Configureren van eenmalige aanmelding](./media/servicenow-tutorial/ic7694976ex.png "eenmalige aanmelding configureren")

15. Op de **nieuwe id-Provider toevoegen** dialoogvenster onder **id-Provider configureren**, voer de volgende stappen uit:

    ![Configureren van eenmalige aanmelding](./media/servicenow-tutorial/ic7694982ex.png "eenmalige aanmelding configureren")

    a. In de **naam** tekstvak, typ een naam voor uw configuratie (bijvoorbeeld: **SAML 2.0**).

    b. In de **URL van de id-Provider** veld, plak de waarde van **Identity Provider-ID**, die u hebt gekopieerd vanuit Azure portal.

    c. In de **id-Provider AuthnRequest** veld, plak de waarde van **aanvraag-URL webverificatie**, die u hebt gekopieerd vanuit Azure portal.

    d. In de **id-Provider SingleLogoutRequest** veld, plak de waarde van **Service-URL voor eenmalige afmelding**, die u hebt gekopieerd vanuit Azure portal

    e. Als **Provider identiteitscertificaat**, selecteert u het certificaat dat u in de vorige stap hebt gemaakt.

16. Klik op **geavanceerde instellingen**, en klikt u onder **aanvullende eigenschappen van de id-Provider**, voer de volgende stappen uit:

    ![Configureren van eenmalige aanmelding](./media/servicenow-tutorial/ic7694983ex.png "eenmalige aanmelding configureren")

    a. In de **Protocol Binding voor de IDP SingleLogoutRequest** tekstvak, type **urn: oasis: namen: tc: SAML:2.0:bindings:HTTP-omleiden**.

    b. In de **NameID beleid** tekstvak, type **urn: oasis: namen: tc: SAML:1.1:nameid-indeling: niet-opgegeven**.

    c. In de **AuthnContextClassRef methode**, type `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`.

    d. Hef de selectie **maken van een AuthnContextClass**.

17. Onder **extra eigenschappen van de Service Provider**, voer de volgende stappen uit:

    ![Configureren van eenmalige aanmelding](./media/servicenow-tutorial/ic7694984ex.png "eenmalige aanmelding configureren")

    a. In de **ServiceNow Homepage** tekstvak typt u de URL van de startpagina van uw ServiceNow-exemplaar.

    > [!NOTE]
    > De startpagina van de ServiceNow-exemplaar is een samenvoeging van uw **ServiceNow tenant-URL** en **/navpage.do** (bijvoorbeeld: `https://fabrikam.service-now.com/navpage.do`).

    b. In de **entiteit-ID / verlener** tekstvak typt u de URL van uw ServiceNow-tenant.

    c. In de **doelgroep-URI** tekstvak typt u de URL van uw ServiceNow-tenant.

    d. In **klok scheeftrekken** tekstvak, type **60**.

    e. In de **Gebruikersveld** tekstvak, type **e** of **gebruikersnaam**, afhankelijk van welk veld wordt gebruikt voor het aanduiden van gebruikers in uw ServiceNow-implementatie.

    > [!NOTE]
    > U kunt Azure AD configureren voor de Azure AD-gebruikers-ID (UPN) of het e-mailadres verzenden als de unieke id in het SAML-token door te gaan naar de **ServiceNow > kenmerken > Single Sign-On** sectie van de Azure portal en het toewijzen van het gewenste veld voor de **nameidentifier** kenmerk. De waarde die voor het geselecteerde kenmerk wordt opgeslagen in Azure AD (bijvoorbeeld naam user principal name) moet overeenkomen met de waarde die is opgeslagen in ServiceNow voor het opgegeven veld (bijvoorbeeld gebruikersnaam)

    f. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

1. Selecteer in de Azure portal, in het linkerdeelvenster **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.

    ![Azure AD-gebruiker maken][100]

2. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm.

    ![Het maken van een Azure AD-testgebruiker](./media/servicenow-tutorial/create_aaduser_01.png) 

3. In de eigenschappen van de gebruiker de volgende stappen uitvoeren.

    ![Het maken van een Azure AD-testgebruiker](./media/servicenow-tutorial/create_aaduser_02.png)

    a. In de **naam** veld **BrittaSimon**.
  
    b. In de **gebruikersnaam** veldtype **brittasimon@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Selecteer **eigenschappen**, selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in het wachtwoord.

    d. Selecteer **Maken**.

### <a name="create-a-servicenow-test-user"></a>Maak een testgebruiker ServiceNow

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in ServiceNow. ServiceNow biedt ondersteuning voor automatisch gebruikers inrichten, dit is standaard ingeschakeld. Meer informatie vindt u [hier](servicenow-provisioning-tutorial.md) voor het automatisch inrichten van gebruikers configureren.

> [!NOTE]
> Als u een gebruiker handmatig hebt gemaakt wilt, moet u contact opnemen met [ondersteuningsteam ServiceNow-Client](https://www.servicenow.com/support/contact-support.html)

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan ServiceNow.

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **ServiceNow**.

    ![De ServiceNow-koppeling in de lijst met toepassingen](./media/servicenow-tutorial/tutorial_servicenow_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. In de **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst met gebruikers, en klik op de **Selecteer** knop aan de onderkant van het scherm.

6. In de **toevoegen toewijzing** dialoogvenster Selecteer de **toewijzen** knop.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de ServiceNow-tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw ServiceNow-toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
* [Inrichten van gebruikers configureren](servicenow-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/servicenow-tutorial/tutorial_general_01.png
[2]: ./media/servicenow-tutorial/tutorial_general_02.png
[3]: ./media/servicenow-tutorial/tutorial_general_03.png
[4]: ./media/servicenow-tutorial/tutorial_general_04.png

[100]: ./media/servicenow-tutorial/tutorial_general_100.png

[200]: ./media/servicenow-tutorial/tutorial_general_200.png
[201]: ./media/servicenow-tutorial/tutorial_general_201.png
[202]: ./media/servicenow-tutorial/tutorial_general_202.png
[203]: ./media/servicenow-tutorial/tutorial_general_203.png
