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
ms.date: 05/16/2018
ms.author: jeedes
ms.openlocfilehash: 5d5c4d5e26fa21488dd637805a4c22bd3ed18a7f
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39421080"
---
# <a name="tutorial-azure-active-directory-integration-with-servicenow"></a>Zelfstudie: Azure Active Directory-integratie met ServiceNow

In deze zelfstudie leert u hoe u ServiceNow integreren met Azure Active Directory (Azure AD).

ServiceNow integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot ServiceNow heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij ServiceNow (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

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
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. ServiceNow uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-servicenow-from-the-gallery"></a>ServiceNow uit de galerie toe te voegen
Voor het configureren van de integratie van ServiceNow in Azure AD, moet u ServiceNow uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen ServiceNow uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

1. Typ in het zoekvak **ServiceNow**, selecteer **ServiceNow** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![ServiceNow in de lijst met resultaten](./media/servicenow-tutorial/tutorial_servicenow_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met ServiceNow op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in ServiceNow is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in ServiceNow tot stand worden gebracht.

In ServiceNow, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met ServiceNow, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren voor ServiceNow](#configure-azure-ad-single-sign-on-for-servicenow)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Azure AD eenmalige aanmelding configureren voor ServiceNow snelle](#configure-azure-ad-single-sign-on-for-servicenow-express)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Maak een testgebruiker ServiceNow](#create-a-servicenow-test-user)**  : als u wilt een equivalent van Britta Simon in ServiceNow die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on-for-servicenow"></a>Azure AD voor eenmalige aanmelding configureren voor ServiceNow

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw ServiceNow-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met ServiceNow, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **ServiceNow** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/servicenow-tutorial/tutorial_servicenow_samlbase.png)

1. Op de **ServiceNow domein en URL's** sectie, voert u de volgende stappen uit:

    ![ServiceNow domein en URL's, eenmalige aanmelding informatie](./media/servicenow-tutorial/tutorial_servicenow_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<instance-name>.service-now.com/navpage.do`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<instance-name>.service-now.com`

    > [!NOTE] 
    > Deze waarden zijn niet echt. U moet deze waarden uit de werkelijke aanmeldings-URL en de id die later in de zelfstudie wordt uitgelegd bijwerken.

1. Op de **SAML-handtekeningcertificaat** sectie, voert u de volgende stappen uit: 

    ![De downloadkoppeling certificaat](./media/servicenow-tutorial/tutorial_servicenow_certificate.png)

    a. Klik op de knop kopiëren om te kopiëren **App-Url voor federatieve metagegevens** en plak deze in Kladblok, als deze App-Url voor federatieve metagegevens wordt later in de zelfstudie worden gebruikt.

    b. Klik op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

1. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/servicenow-tutorial/tutorial_general_400.png)

1. Meld u aan bij uw ServiceNow-toepassing als beheerder.

1. Activeer de **integratie - meerdere Single Sign-On Providerinstallatieprogramma** invoegtoepassing door de volgende stappen te volgen:

    a. Zoek in het navigatiedeelvenster aan de linkerkant **System Definition** sectie uit de zoekbalk en klik vervolgens op **invoegtoepassingen**.

    ![Activeren van de invoegtoepassing](./media/servicenow-tutorial/tutorial_servicenow_03.png "invoegtoepassing activeren")

     b. Zoeken naar **integratie - meerdere Single Sign-On Providerinstallatieprogramma**.

     ![Activeren van de invoegtoepassing](./media/servicenow-tutorial/tutorial_servicenow_04.png "invoegtoepassing activeren")

    c. Selecteer de invoegtoepassing. Klik met de rechtermuisknop en selecteer **activeren/Upgrade**.

    d. Klik op de **activeren** knop.

1. Er zijn twee manieren waarop **ServiceNow** kunnen worden geconfigureerd automatic en manual.

1. Voor het configureren van **ServiceNow** automatisch Volg de onderstaande stappen te volgen

    a. Ga terug naar de **ServiceNow** eenmalige aanmelding op de pagina in de Azure portal.

    b. Met één klik service configureren is opgegeven voor ServiceNow dat wil zeggen, hebt u Azure AD automatisch configureren ServiceNow voor verificatie op basis van SAML. Om in te schakelen deze service gaat u naar **ServiceNow configuratie** sectie, klikt u op **ServiceNow configureren** om configureren aanmeldings-venster te openen.

    ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/tutorial_servicenow_configure.png)

    c. Voer de naam van het ServiceNow-exemplaar, gebruikersnaam, en beheerders beheerderswachtwoord in de **aanmelding configureren** vormen en klikt u op **nu configureren**. Houd er rekening mee dat de opgegeven beheerdersgebruikersnaam moet de **security_admin** rol die is toegewezen in ServiceNow voor deze om te werken. Anders handmatig configureren van ServiceNow voor het gebruik van Azure AD als SAML-identiteitsprovider, klikt u op **handmatig configureren van eenmalige aanmelding** en kopieer de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de sectie ter referentie.

    ![App-URL configureren](./media/servicenow-tutorial/configure.png "app-URL configureren")

    d. Meld u aan bij uw ServiceNow-toepassing als beheerder.

    e. In de configuratie voor automatisch de vereiste instellingen zijn geconfigureerd op de **ServiceNow** aan clientzijde, maar de **X.509-certificaat** is niet standaard ingeschakeld. U moet deze handmatig toewijzen aan uw id-provider in ServiceNow. Volg de onderstaande stappen voor dezelfde:
    
    * Klik in het navigatiedeelvenster aan de linkerkant op **id-Providers** onder **multi-provider SSO**.

      ![Configureren van eenmalige aanmelding](./media/servicenow-tutorial/tutorial_servicenow_07.png "eenmalige aanmelding configureren")

    * Klik op de automatisch gegenereerde id-provider

      ![Configureren van eenmalige aanmelding](./media/servicenow-tutorial/tutorial_servicenow_08.png "eenmalige aanmelding configureren")

    * Schuif omlaag naar de **X.509-certificaat** sectie. Selecteer **Bewerken**.

      ![Configureren van eenmalige aanmelding](./media/servicenow-tutorial/tutorial_servicenow_09.png "eenmalige aanmelding configureren")
    
    * Selecteer op het certificaat en klik op het pictogram pijl-rechts om toe te voegen van het certificaat

      ![Configureren van eenmalige aanmelding](./media/servicenow-tutorial/tutorial_servicenow_11.png "eenmalige aanmelding configureren")

    * Klik op **Opslaan**.

    * Klik op **activeren** in de rechterbovenhoek van de pagina.

1. Voor het configureren van **ServiceNow** handmatig Volg de onderstaande stappen te volgen

1. Meld u aan bij uw ServiceNow-toepassing als beheerder.

1. Zoek in het navigatiedeelvenster aan de linkerkant **multi-provider SSO** sectie uit de zoekbalk en klik vervolgens op **eigenschappen**.

    ![App-URL configureren](./media/servicenow-tutorial/tutorial_servicenow_06.png "app-URL configureren")

1. Op de **meerdere eigenschappen van de Provider SSO** dialoogvenster, voer de volgende stappen uit:

    ![App-URL configureren](./media/servicenow-tutorial/ic7694981.png "app-URL configureren")

    a. Als **meerdere provider SSO inschakelen**, selecteer **Ja**.

    b. Als **inschakelen het automatisch importeren van de gebruikers van alle id-providers in de gebruikerstabel**, selecteer **Ja**.

    c. Als **logboekregistratie voor de provider die meerdere SSO-integratie voor het inschakelen van foutopsporing**, selecteer **Ja**.

    d. In **het veld voor de gebruiker die tabel...**  tekstvak, type **gebruikersnaam**.

    e. Klik op **Opslaan**.

1. Zoek in het navigatiedeelvenster aan de linkerkant **multi-provider SSO** sectie uit de zoekbalk en klik vervolgens op **x509 certificaten**.

    ![Configureren van eenmalige aanmelding](./media/servicenow-tutorial/tutorial_servicenow_05.png "eenmalige aanmelding configureren")

1. Op de **X.509-certificaten** dialoogvenster, klikt u op **nieuw**.

    ![Configureren van eenmalige aanmelding](./media/servicenow-tutorial/ic7694974.png "eenmalige aanmelding configureren")

1. Op de **X.509-certificaten** dialoogvenster, voer de volgende stappen uit:

    ![Configureren van eenmalige aanmelding](./media/servicenow-tutorial/ic7694975.png "eenmalige aanmelding configureren")

    a. In de **naam** tekstvak, typ een naam voor uw configuratie (bijvoorbeeld: **TestSAML2.0**).

    b. Selecteer **Active**.

    c. Als **indeling**, selecteer **PEM**.

    d. Als **Type**, selecteer **Store-certificaat vertrouwen**.

    e. Open uw Base64-gecodeerd certificaat gedownload van Azure in Kladblok, Kopieer de inhoud ervan in het Klembord en plakt u deze de **PEM certificaat** tekstvak.

     f. Klik op **indienen**.

1. Klik in het navigatiedeelvenster aan de linkerkant op **id-Providers**.

    ![Configureren van eenmalige aanmelding](./media/servicenow-tutorial/tutorial_servicenow_07.png "eenmalige aanmelding configureren")

1. Op de **id-Providers** dialoogvenster, klikt u op **nieuw**.

    ![Configureren van eenmalige aanmelding](./media/servicenow-tutorial/ic7694977.png "eenmalige aanmelding configureren")

1. Op de **id-Providers** dialoogvenster, klikt u op **SAML2-Update1?**.

    ![Configureren van eenmalige aanmelding](./media/servicenow-tutorial/ic7694978.png "eenmalige aanmelding configureren")

1. In het dialoogvenster Eigenschappen voor SAML2-Update1, moet u de volgende stappen uitvoeren:

    ![Configureren van eenmalige aanmelding](./media/servicenow-tutorial/idp.png "eenmalige aanmelding configureren")

    a. Selecteer **URL** optie **metagegevens importeren id-Provider** dialoogvenster.

    b. Voer de **App-Url voor federatieve metagegevens** die u hebt gekopieerd vanuit Azure portal.

    c. Klik op **Import**.

1. Leest de IdP metagegevens-URL en wordt alle informatie van de velden ingevuld.

    ![Configureren van eenmalige aanmelding](./media/servicenow-tutorial/ic7694982.png "eenmalige aanmelding configureren")

    a. In de **naam** tekstvak, typ een naam voor uw configuratie (bijvoorbeeld **SAML 2.0**).
    
    b. Kopie **ServiceNow Homepage** waarde, plak deze in de **aanmeldings-URL** -tekstvak in **ServiceNow domein en URL's** sectie in Azure portal.

    > [!NOTE]
    > De startpagina van de ServiceNow-exemplaar is een samenvoeging van uw **ServieNow tenant-URL** en **/navpage.do** (bijvoorbeeld:`https://fabrikam.service-now.com/navpage.do`).

    c. Kopie **entiteit-ID / verlener** waarde, plak deze in **id** -tekstvak in **ServiceNow domein en URL's** sectie in Azure portal.

    d. Klik op **geavanceerde**. In de **Gebruikersveld** tekstvak, type **e** of **gebruikersnaam**, afhankelijk van welk veld wordt gebruikt voor het aanduiden van gebruikers in uw ServiceNow-implementatie.

    > [!NOTE]
    > U kunt Azure AD configureren voor de Azure AD-gebruikers-ID (UPN) of het e-mailadres verzenden als de unieke id in het SAML-token door te gaan naar de **ServiceNow > kenmerken > Single Sign-On** sectie van de Azure portal en het toewijzen van het gewenste veld voor de **nameidentifier** kenmerk. De waarde die voor het geselecteerde kenmerk wordt opgeslagen in Azure AD (bijvoorbeeld naam user principal name) moet overeenkomen met de waarde die is opgeslagen in ServiceNow voor het opgegeven veld (bijvoorbeeld gebruikersnaam)

    e. Onder **x509 certificaat**, geeft een lijst van het certificaat dat u in de vorige stap hebt gemaakt.

    > [!NOTE]
    > ServiceNow is niet toegestaan voor activering van de Idp zonder te klikken op de knop test connection, als u wilt onderdrukken hetzelfde, volg de onderstaande stappen.

1. Klik op het menupictogram uit uw nieuwe id-provider die u hebt gemaakt als onderdeel van de configuratie en uit de lijst selecteren **sys_id kopiëren**

    ![Configureren van eenmalige aanmelding](./media/servicenow-tutorial/ic7694992.png "eenmalige aanmelding configureren")

1. Zoek in het bovenste links zoekvak **sys_properties.list** en druk op enter.

    ![Configureren van eenmalige aanmelding](./media/servicenow-tutorial/ic7694993.png "eenmalige aanmelding configureren")

1. Klik op **Nieuw**.

    ![Configureren van eenmalige aanmelding](./media/servicenow-tutorial/ic7694994.png "eenmalige aanmelding configureren")

1. In de **systeemeigenschap** sectie, voert u de volgende stappen uit:

    ![Configureren van eenmalige aanmelding](./media/servicenow-tutorial/ic7694995.png "eenmalige aanmelding configureren")

    a. Voer `glide.authenticate.sso.redirect.idp` waarde in het tekstvak naam.

    b. In de **waarde** tekstvak, plak de kopie-sys_id waarde die u in de voorgaande stappen hebt gekopieerd.

    c. Selecteer **persoonlijke**.

    d. Klik op **indienen**.

1. Klik op **Nieuw**.

    ![Configureren van eenmalige aanmelding](./media/servicenow-tutorial/ic7694994.png "eenmalige aanmelding configureren")

1. In de **systeemeigenschap** sectie, voert u de volgende stappen uit:

    ![Configureren van eenmalige aanmelding](./media/servicenow-tutorial/ic7694996.png "eenmalige aanmelding configureren")

    a. Voer `glide.authenticate.multisso.test.connection.mandatory` waarde in het tekstvak naam.

    b. In de **waarde** tekstvak, voer **false**.

    c. Klik op **indienen**.

1. Na dit hierboven genoemde stap nu kunt u zich aan het activeren van uw nieuwe id-provider en de eenmalige aanmelding werkt

> [!NOTE]
> Ook Houd er rekening mee dat u hebt het testen van uw nieuwe Idp-configuratie in een nieuw incognitovenster

### <a name="configure-azure-ad-single-sign-on-for-servicenow-express"></a>Azure AD voor eenmalige aanmelding voor ServiceNow snelle configureren

1. In de Azure-portal op de **ServiceNow** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/tutorial_servicenow_samlbase.png)

1. Op de **ServiceNow domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/tutorial_servicenow_url.png)

    a. In de **aanmeldings-URL** tekstvak typt u de waarde met behulp van het volgende patroon: `https://<instance-name>.service-now.com/navpage.do`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<instance-name>.service-now.com`

    > [!NOTE]
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke aanmeldings-URL en -id. Neem contact op met [ServiceNow-Client-ondersteuningsteam](https://www.servicenow.com/support/contact-support.html) om deze waarden te verkrijgen.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/tutorial_servicenow_certificates.png)

1. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/tutorial_general_400.png)

1. Met één klik service configureren is opgegeven voor ServiceNow dat wil zeggen, hebt u Azure AD automatisch configureren ServiceNow voor verificatie op basis van SAML. Om in te schakelen deze service gaat u naar **ServiceNow configuratie** sectie, klikt u op **ServiceNow configureren** om configureren aanmeldings-venster te openen.

    ![Eenmalige aanmelding configureren](./media/servicenow-tutorial/tutorial_servicenow_configure.png)

1. Voer de naam van het ServiceNow-exemplaar, gebruikersnaam, en beheerders beheerderswachtwoord in de **aanmelding configureren** vormen en klikt u op **nu configureren**. Houd er rekening mee dat de opgegeven beheerdersgebruikersnaam moet de **security_admin** rol die is toegewezen in ServiceNow voor deze om te werken. Anders handmatig configureren van ServiceNow voor het gebruik van Azure AD als SAML-identiteitsprovider, klikt u op **handmatig configureren van eenmalige aanmelding** en kopieer de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de sectie ter referentie.

    ![App-URL configureren](./media/servicenow-tutorial/configure.png "app-URL configureren")

1. Meld u aan bij uw ServiceNow-Express-toepassing als beheerder.

1. Klik in het navigatiedeelvenster aan de linkerkant op **Single Sign-On**.

    ![App-URL configureren](./media/servicenow-tutorial/ic7694980ex.png "app-URL configureren")

1. Op de **Single Sign-On** dialoogvenster, klikt u op het configuratiepictogram in de rechterbovenhoek en stel de volgende eigenschappen:

    ![App-URL configureren](./media/servicenow-tutorial/ic7694981ex.png "app-URL configureren")

    a. In-/ uitschakelen **meerdere provider SSO inschakelen** aan de rechterkant.
    
    b. In-/ uitschakelen **inschakelen foutopsporing SSO-integratie-registratie voor de provider van meerdere** aan de rechterkant.
    
    c. In **het veld voor de gebruiker die tabel...**  tekstvak, type **gebruikersnaam**.

1. Op de **Single Sign-On** dialoogvenster, klikt u op **nieuw certificaat toevoegen**.

    ![Configureren van eenmalige aanmelding](./media/servicenow-tutorial/ic7694973ex.png "eenmalige aanmelding configureren")

1. Op de **X.509-certificaten** dialoogvenster, voer de volgende stappen uit:

    ![Configureren van eenmalige aanmelding](./media/servicenow-tutorial/ic7694975.png "eenmalige aanmelding configureren")

    a. In de **naam** tekstvak, typ een naam voor uw configuratie (bijvoorbeeld: **TestSAML2.0**).

    b. Selecteer **Active**.

    c. Als **indeling**, selecteer **PEM**.

    d. Als **Type**, selecteer **Store-certificaat vertrouwen**.

    e. Open uw Base64-gecodeerd certificaat gedownload vanuit Azure portal in Kladblok, Kopieer de inhoud ervan in het Klembord en plakt u deze de **PEM certificaat** tekstvak.

    f. Klik op **Update**

1. Op de **Single Sign-On** dialoogvenster, klikt u op **nieuwe IdP toevoegen**.

    ![Configureren van eenmalige aanmelding](./media/servicenow-tutorial/ic7694976ex.png "eenmalige aanmelding configureren")

1. Op de **nieuwe id-Provider toevoegen** dialoogvenster onder **id-Provider configureren**, voer de volgende stappen uit:

    ![Configureren van eenmalige aanmelding](./media/servicenow-tutorial/ic7694982ex.png "eenmalige aanmelding configureren")

    a. In de **naam** tekstvak, typ een naam voor uw configuratie (bijvoorbeeld: **SAML 2.0**).

    b. In de **URL van de id-Provider** veld, plak de waarde van **Identity Provider-ID**, die u hebt gekopieerd vanuit Azure portal.
    
    c. In de **id-Provider AuthnRequest** veld, plak de waarde van **aanvraag-URL webverificatie**, die u hebt gekopieerd vanuit Azure portal.

    d. In de **id-Provider SingleLogoutRequest** veld, plak de waarde van **Service-URL voor eenmalige afmelding**, die u hebt gekopieerd vanuit Azure portal

    e. Als **Provider identiteitscertificaat**, selecteert u het certificaat dat u in de vorige stap hebt gemaakt.

1. Klik op **geavanceerde instellingen**, en klikt u onder **aanvullende eigenschappen van de id-Provider**, voer de volgende stappen uit:

    ![Configureren van eenmalige aanmelding](./media/servicenow-tutorial/ic7694983ex.png "eenmalige aanmelding configureren")

    a. In de **Protocol Binding voor de IDP SingleLogoutRequest** tekstvak, type **urn: oasis: namen: tc: SAML:2.0:bindings:HTTP-omleiden**.

    b. In de **NameID beleid** tekstvak, type **urn: oasis: namen: tc: SAML:1.1:nameid-indeling: niet-opgegeven**.

    c. In de **AuthnContextClassRef methode**, type `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`.

    d. Hef de selectie **maken van een AuthnContextClass**.

1. Onder **extra eigenschappen van de Service Provider**, voer de volgende stappen uit:

    ![Configureren van eenmalige aanmelding](./media/servicenow-tutorial/ic7694984ex.png "eenmalige aanmelding configureren")

    a. In de **ServiceNow Homepage** tekstvak typt u de URL van de startpagina van uw ServiceNow-exemplaar.

    > [!NOTE]
    > De startpagina van de ServiceNow-exemplaar is een samenvoeging van uw **ServieNow tenant-URL** en **/navpage.do** (bijvoorbeeld: `https://fabrikam.service-now.com/navpage.do`).

    b. In de **entiteit-ID / verlener** tekstvak typt u de URL van uw ServiceNow-tenant.

    c. In de **doelgroep-URI** tekstvak typt u de URL van uw ServiceNow-tenant.

    d. In **klok scheeftrekken** tekstvak, type **60**.

    e. In de **Gebruikersveld** tekstvak, type **e** of **gebruikersnaam**, afhankelijk van welk veld wordt gebruikt voor het aanduiden van gebruikers in uw ServiceNow-implementatie.

    > [!NOTE]
    > U kunt Azure AD configureren voor de Azure AD-gebruikers-ID (UPN) of het e-mailadres verzenden als de unieke id in het SAML-token door te gaan naar de **ServiceNow > kenmerken > Single Sign-On** sectie van de Azure portal en het toewijzen van het gewenste veld voor de **nameidentifier** kenmerk. De waarde die voor het geselecteerde kenmerk wordt opgeslagen in Azure AD (bijvoorbeeld naam user principal name) moet overeenkomen met de waarde die is opgeslagen in ServiceNow voor het opgegeven veld (bijvoorbeeld gebruikersnaam)

    f. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/servicenow-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/servicenow-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/servicenow-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/servicenow-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.

### <a name="create-a-servicenow-test-user"></a>Maak een testgebruiker ServiceNow

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in ServiceNow. ServiceNow biedt ondersteuning voor automatisch gebruikers inrichten, dit is standaard ingeschakeld. Meer informatie vindt u [hier](servicenow-provisioning-tutorial.md) voor het automatisch inrichten van gebruikers configureren.

> [!NOTE]
> Als u een gebruiker handmatig hebt gemaakt wilt, moet u contact opnemen met [ondersteuningsteam ServiceNow-Client](https://www.servicenow.com/support/contact-support.html)

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan ServiceNow.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon met ServiceNow, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **ServiceNow**.

    ![De ServiceNow-koppeling in de lijst met toepassingen](./media/servicenow-tutorial/tutorial_servicenow_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de ServiceNow-tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw ServiceNow-toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

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
