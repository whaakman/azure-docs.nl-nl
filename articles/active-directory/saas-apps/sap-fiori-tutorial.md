---
title: 'Zelfstudie: Azure Active Directory-integratie met SAP Fiori | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en SAP Fiori.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 77ad13bf-e56b-4063-97d0-c82a19da9d56
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/11/2019
ms.author: jeedes
ms.openlocfilehash: 9e7993ee1cb439ebeaa9f64bee55429aa54f9cee
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2019
ms.locfileid: "65903958"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-fiori"></a>Zelfstudie: Azure Active Directory-integratie met SAP Fiori

In deze zelfstudie leert u hoe u SAP Fiori integreren met Azure Active Directory (Azure AD).

SAP Fiori integreren met Azure AD biedt u de volgende voordelen:

* U kunt Azure AD om te bepalen wie toegang tot SAP Fiori heeft gebruiken.
* Gebruikers kunnen worden automatisch aangemeld bij SAP Fiori met hun Azure AD-accounts (eenmalige aanmelding).
* U kunt uw accounts vanaf één locatie beheren, de Azure-portal.

Zie voor meer informatie over software als een service (SaaS)-app-integratie met Azure AD, [eenmalige aanmelding voor toepassingen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met SAP Fiori, moet u de volgende items:

* Een Azure AD-abonnement Als u een Azure AD-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.
* Een abonnement op SAP Fiori met eenmalige aanmelding ingeschakeld.
* SAP Fiori 7.20 of hoger is vereist.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie hebt u configureert en Azure AD eenmalige aanmelding testen in een testomgeving en SAP Fiori integreren met Azure AD.

SAP Fiori ondersteunt de volgende functies:

* **Serviceprovider geïnitieerde eenmalige aanmelding**

## <a name="add-sap-fiori-in-the-azure-portal"></a>SAP Fiori toevoegen in Azure portal

Als u wilt SAP Fiori integreren met Azure AD, moet u SAP Fiori toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

1. Selecteer in het menu links **Azure Active Directory**.

    ![De Azure Active Directory-optie](common/select-azuread.png)

1. Selecteer **Bedrijfstoepassingen** > **Alle toepassingen**.

    ![Het deelvenster Bedrijfstoepassingen](common/enterprise-applications.png)

1. Als u wilt een toepassing hebt toegevoegd, selecteert u **nieuwe toepassing**.

    ![De nieuwe optie voor de App](common/add-new-app.png)

1. Voer in het zoekvak **SAP Fiori**. Selecteer in de lijst met zoekresultaten **SAP Fiori**, en selecteer vervolgens **toevoegen**.

    ![SAP Fiori in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met SAP Fiori op basis van een testgebruiker met de naam **Britta Simon**. Voor eenmalige aanmelding om te werken, moet u een gekoppelde relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker maken in SAP Fiori.

Als u wilt configureren en Azure AD eenmalige aanmelding met SAP Fiori testen, moet u de volgende bouwstenen uitvoeren:

| Taak | Description |
| --- | --- |
| **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)** | Hiermee kunnen uw gebruikers deze functie wilt gebruiken. |
| **[Configureren van eenmalige aanmelding SAP Fiori](#configure-sap-fiori-single-sign-on)** | Hiermee configureert u de instellingen voor eenmalige aanmelding in de toepassing. |
| **[Maak een testgebruiker Azure AD](#create-an-azure-ad-test-user)** | Met de naam Britta Simon tests Azure AD eenmalige aanmelding voor een gebruiker. |
| **[De Azure AD-testgebruiker toewijzen](#assign-the-azure-ad-test-user)** | Hiermee kunt Britta Simon gebruik van Azure AD eenmalige aanmelding. |
| **[Maak een testgebruiker SAP Fiori](#create-an-sap-fiori-test-user)** | Hiermee maakt u een equivalent van Britta Simon in SAP Fiori die is gekoppeld aan de Azure AD-weergave van de gebruiker. |
| **[Eenmalige aanmelding testen](#test-single-sign-on)** | Hiermee wordt gecontroleerd of de configuratie werkt. |

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie configureert u Azure AD eenmalige aanmelding met SAP Fiori in Azure portal.

1. Open een nieuw browservenster en meld u aan de site van het bedrijf SAP Fiori als beheerder.

1. Zorg ervoor dat **http** en **https** services actief zijn en dat de relevante poorten zijn toegewezen aan de transactiecode **SMICM**.

1. Voor SAP-systeem aanmelden bij de SAP Business Client **T01**, waarbij eenmalige aanmelding is vereist. Vervolgens activeert u HTTP-sessie beveiligingsbeheer.

    1. Ga naar de transactiecode **SICF_SESSIONS**. Alle relevante profiel parameters met de huidige waarden worden weergegeven. Ze zien eruit als in het volgende voorbeeld:

        ```
        login/create_sso2_ticket = 2
        login/accept_sso2_ticket = 1
        login/ticketcache_entries_max = 1000
        login/ticketcache_off = 0  login/ticket_only_by_https = 0 
        icf/set_HTTPonly_flag_on_cookies = 3
        icf/user_recheck = 0  http/security_session_timeout = 1800
        http/security_context_cache_size = 2500
        rdisp/plugin_auto_logout = 1800
        rdisp/autothtime = 60
        ```

        >[!NOTE]
        > Pas de parameters op basis van de organisatievereisten van uw. De bovenstaande parameters krijgt alleen als voorbeeld.

    1. Indien nodig, aanpassen van de parameters in het profiel van de instantie (standaard) van de SAP-systeem en het SAP-systeem opnieuw opstarten.

    1. Dubbelklik op de relevante client om in te schakelen van een HTTP-security-sessie.

        ![De huidige waarden van de relevante profiel pagina Parameters in SAP](./media/sapfiori-tutorial/tutorial-sapnetweaver-profileparameter.png)

    1. De volgende SICF-services te activeren:

        ```
        /sap/public/bc/sec/saml2
        /sap/public/bc/sec/cdc_ext_service
        /sap/bc/webdynpro/sap/saml2
        /sap/bc/webdynpro/sap/sec_diag_tool (This is only to enable / disable trace)
        ```

1. Ga naar de transactiecode **SAML2** in Business-Client voor SAP-systeem [**T01/122**]. De configuratie van de gebruikersinterface wordt geopend in een nieuw browservenster. In dit voorbeeld gebruiken we Business Client voor SAP-systeem 122.

    ![De aanmeldingspagina van SAP Fiori Business Client](./media/sapfiori-tutorial/tutorial-sapnetweaver-sapbusinessclient.png)

1. Voer uw gebruikersnaam en wachtwoord en selecteer vervolgens **aanmelden**.

    ![De pagina SAML 2.0 configuratie van ABAP System T01/122 in SAP](./media/sapfiori-tutorial/tutorial-sapnetweaver-userpwd.png)

1. In de **providernaam** vak, Vervang **T01122** met **http:\//T01122**, en selecteer vervolgens **opslaan**.

    > [!NOTE]
    > Naam van de provider is standaard in de indeling \<sid >\<client >. Azure AD wordt verwacht dat de naam in de indeling \<protocol > ://\<naam >. Het is raadzaam dat u de naam van de provider als https behoudt\://\<sid >\<client > zodat u meerdere SAP Fiori ABAP-engines in Azure AD configureren kunt.

    ![De naam van de bijgewerkte provider op de pagina SAML 2.0 configuratie van ABAP System T01/122 in SAP](./media/sapfiori-tutorial/tutorial-sapnetweaver-providername.png)

1. Selecteer **tabblad lokale Provider** > **metagegevens**.

1. In de **SAML 2.0-metagegevens** dialoogvenster vak, downloaden de gegenereerde metagegevens-XML-bestand en sla deze op uw computer.

    ![De koppeling metagegevens downloaden in het dialoogvenster SAP SAML 2.0-metagegevens](./media/sapfiori-tutorial/tutorial-sapnetweaver-generatesp.png)

1. In de [Azure-portal](https://portal.azure.com/), in de **SAP Fiori** toepassing integratie venster **eenmalige aanmelding**.

    ![De optie voor eenmalige aanmelding](common/select-sso.png)

1. In de **selecteert u een methode voor eenmalige aanmelding** deelvenster Selecteer **SAML** of **SAML/WS-Federation** modus voor eenmalige aanmelding inschakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

1. In de **instellen van eenmalige aanmelding met SAML** venster **bewerken** (het potloodpictogram) te openen de **SAML-basisconfiguratie** deelvenster.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. Voer in de sectie **Standaard SAML-configuratie** de volgende stappen uit:

    1. Selecteer **metagegevensbestand uploaden**.

        ![De optie-Metagegevens uploaden](common/upload-metadata.png)

   1. Selecteer het bestand met metagegevens, selecteer het mappictogram en selecteer vervolgens **uploaden**.

       ![Selecteer het bestand met metagegevens en selecteer vervolgens de knop voor uploaden](common/browse-upload-metadata.png)

1. Wanneer het bestand met metagegevens is geüpload, de **id** en **antwoord-URL** waarden worden automatisch ingevuld de **SAML-basisconfiguratie** deelvenster. In de **aanmeldings-URL** voert u een URL met de volgende indeling: https:\//\<uw exemplaar van het bedrijf van SAP Fiori\>.

    ![SAP Fiori domein en URL's, eenmalige aanmelding informatie](common/sp-identifier-reply.png)

    > [!NOTE]
    > Enkele klanten rapportfouten met betrekking tot foutief geconfigureerde **antwoord-URL** waarden. Als u deze fout ziet, kunt u het volgende PowerShell-script om in te stellen van de juiste antwoord-URL voor uw exemplaar:
    >
    > ```
    > Set-AzureADServicePrincipal -ObjectId $ServicePrincipalObjectId -ReplyUrls "<Your Correct Reply URL(s)>"
    > ``` 
    > 
    > U kunt instellen dat de `ServicePrincipal` object-ID zelf voordat het script is uitgevoerd, of u kunt deze hier doorgeven.

1. De SAP Fiori-toepassing wordt verwacht dat de SAML-asserties ondertekend zijn in een specifieke indeling. Configureer de volgende claims voor deze toepassing. Voor het beheren van deze kenmerkwaarden de **instellen van eenmalige aanmelding met SAML** venster **bewerken**.

    ![De gebruiker-deelvenster kenmerken](common/edit-attribute.png)

1. In de **gebruikerskenmerken en Claims** in het deelvenster de kenmerken van SAML-token configureren zoals wordt weergegeven in de voorgaande afbeelding. Voer de volgende stappen uit:

    1. Selecteer **bewerken** openen de **gebruikersclaims beheren** deelvenster.

    1. In de **transformatie** in de lijst met **ExtractMailPrefix()**.

    1. In de **Parameter 1** in de lijst met **user.userprinicipalname**.

    1. Selecteer **Opslaan**.

       ![Het deelvenster beheren gebruiker claims](./media/sapfiori-tutorial/nameidattribute.png)

       ![De transformatie-sectie in het deelvenster beheren gebruiker claims](./media/sapfiori-tutorial/nameidattribute1.png)


1. In de **instellen van eenmalige aanmelding met SAML** deelvenster in de **SAML-handtekeningcertificaat** sectie, selecteer **downloaden** naast **voorfederatievemetagegevens-XML**. Selecteer een optie voor downloaden op basis van uw vereisten. Sla het certificaat op uw computer.

    ![De optie voor het downloaden van certificaat](common/metadataxml.png)

1. In de **instellen op SAP Fiori** sectie, kopieert u de volgende URL's op basis van uw vereisten:

    * Aanmeldings-URL
    * Azure AD-id
    * Afmeldings-URL

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

### <a name="configure-sap-fiori-single-sign-on"></a>Configureren van eenmalige aanmelding SAP Fiori

1. Meld u aan met de SAP-systeem en Ga naar de transactiecode **SAML2**. Er wordt een nieuw browservenster geopend met de pagina van het SAML-configuratie.

1. Voor het configureren van eindpunten voor een vertrouwde id-provider (Azure AD), selecteer de **vertrouwde Providers** tabblad.

    ![Het tabblad vertrouwde Providers in SAP](./media/sapfiori-tutorial/tutorial-sapnetweaver-samlconfig.png)

1. Selecteer **toevoegen**, en selecteer vervolgens **metagegevensbestand uploaden** in het contextmenu.

    ![De opties toevoegen en metagegevensbestand uploaden in SAP](./media/sapfiori-tutorial/tutorial-sapnetweaver-uploadmetadata.png)

1. Upload het bestand met metagegevens die u hebt gedownload in de Azure-portal. Selecteer **Next**.

    ![Selecteer het bestand met metagegevens uploaden in SAP](./media/sapfiori-tutorial/tutorial-sapnetweaver-metadatafile.png)

1. Op de volgende pagina in de **Alias** voert u de aliasnaam. Bijvoorbeeld, **aadsts**. Selecteer **Next**.

    ![Het vak Alias in SAP](./media/sapfiori-tutorial/tutorial-sapnetweaver-aliasname.png)

1. Zorg ervoor dat de waarde in de **Digest-algoritme** vak is **SHA-256**. Selecteer **Next**.

    ![Controleer de waarde Digest-algoritme in SAP](./media/sapfiori-tutorial/tutorial-sapnetweaver-identityprovider.png)

1. Onder **Single Sign-On eindpunten**, selecteer **HTTP POST**, en selecteer vervolgens **volgende**.

    ![Opties voor eenmalige aanmelding-eindpunten in SAP](./media/sapfiori-tutorial/tutorial-sapnetweaver-httpredirect.png)

1. Onder **eenmalige afmelding eindpunten**, selecteer **HTTP-omleiding**, en selecteer vervolgens **volgende**.

    ![Opties voor eenmalige afmelding eindpunten in SAP](./media/sapfiori-tutorial/tutorial-sapnetweaver-httpredirect1.png)

1. Onder **artefact eindpunten**, selecteer **volgende** om door te gaan.

    ![Opties voor artefact-eindpunten in SAP](./media/sapfiori-tutorial/tutorial-sapnetweaver-artifactendpoint.png)

1. Onder **verificatievereisten**, selecteer **voltooien**.

    ![Opties voor verificatie vereisten en de optie voltooien in SAP](./media/sapfiori-tutorial/tutorial-sapnetweaver-authentication.png)

1. Selecteer **vertrouwde Provider** > **identiteitsfederatie** (aan de onderkant van de pagina). Selecteer **Bewerken**.

    ![De vertrouwde Provider en identiteitsfederatie tabbladen in SAP](./media/sapfiori-tutorial/tutorial-sapnetweaver-trustedprovider.png)

1. Selecteer **Toevoegen**.

    ![De optie toevoegen op het tabblad identiteitsfederatie](./media/sapfiori-tutorial/tutorial-sapnetweaver-addidentityprovider.png)

1. In de **NameID-indelingen ondersteund** in het dialoogvenster, selecteer **niet opgegeven**. Selecteer **OK**.

    ![De opties in SAP en NameID-indelingen ondersteund in het dialoogvenster](./media/sapfiori-tutorial/tutorial-sapnetweaver-nameid.png)

    De waarden voor **gebruiker-ID bron** en **toewijzing gebruikersmodus ID** bepalen van de koppeling tussen de SAP-gebruiker en de Azure AD-claim.  

    **Scenario 1**: SAP-gebruiker toewijzing van de Azure AD-gebruiker

    1. In SAP onder **Details van NameID-indeling 'Onbekend'**, noteert u de details:

        ![De Details van NameID-indeling 'Onbekende' dialoogvenster in SAP](./media/sapfiori-tutorial/nameiddetails.png)

    1. In de Azure-portal onder **gebruikerskenmerken en Claims**, houd er rekening mee de vereiste claims van Azure AD.

        ![Het dialoogvenster gebruikerskenmerken en Claims in de Azure-portal](./media/sapfiori-tutorial/claimsaad1.png)

    **Scenario 2**: Selecteer de gebruikers-ID van SAP op basis van de geconfigureerde e-mailadres in SU01. In dit geval moet de e-mail-ID in SU01 worden geconfigureerd voor elke gebruiker die eenmalige aanmelding is vereist.

    1.  In SAP onder **Details van NameID-indeling 'Onbekend'**, noteert u de details:

        ![De Details van NameID-indeling 'Onbekende' dialoogvenster in SAP](./media/sapfiori-tutorial/tutorial-sapnetweaver-nameiddetails1.png)

    1. In de Azure-portal onder **gebruikerskenmerken en Claims**, houd er rekening mee de vereiste claims van Azure AD.

       ![Het dialoogvenster gebruikerskenmerken en Claims in de Azure-portal](./media/sapfiori-tutorial/claimsaad2.png)

1. Selecteer **opslaan**, en selecteer vervolgens **inschakelen** om in te schakelen van de id-provider.

    ![Opties voor het opslaan en inschakelen in SAP](./media/sapfiori-tutorial/configuration1.png)

1. Selecteer **OK** wanneer hierom wordt gevraagd.

    ![De optie OK in het dialoogvenster configuratie van SAML 2.0 in SAP](./media/sapfiori-tutorial/configuration2.png)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

In deze sectie gaat u een testgebruiker met de naam Britta Simon maken in de Azure-portal.

1. Selecteer in de Azure-portal **Azure Active Directory** > **Gebruikers** > **Alle gebruikers**.

    ![De gebruikers en alle opties voor gebruikers](common/users.png)

1. Selecteer **Nieuwe gebruiker**.

    ![De nieuwe Gebruikersoptie](common/new-user.png)

1. Voer in het deelvenster **Gebruiker** de volgende stappen uit:

    1. Voer in het vak **Naam** **Britta Simon**in.
  
    1. In de **gebruikersnaam** Voer **brittasimon\@\<uw-bedrijfsdomein >.\< extensie >**. Bijvoorbeeld, **brittasimon\@contoso.com**.

    1. Selecteer de **Show wachtwoord** selectievakje. Noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    1. Selecteer **Maken**.

    ![De gebruiker-deelvenster](common/user-properties.png)

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt u Britta Simon toegang verlenen aan SAP Fiori, zodat ze Azure eenmalige aanmelding kan gebruiken.

1. Selecteer in de Azure portal, **bedrijfstoepassingen** > **alle toepassingen** > **SAP Fiori**.

    ![Het deelvenster Bedrijfstoepassingen](common/enterprise-applications.png)

1. Selecteer in de lijst met toepassingen, **SAP Fiori**.

    ![SAP Fiori in de lijst met toepassingen](common/all-applications.png)

1. Selecteer **Gebruikers en groepen** in het menu.

    ![De optie gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **Gebruiker toevoegen**. Selecteer vervolgens **Gebruikers en groepen** in het deelvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

1. In de **gebruikers en groepen** venster **Britta Simon** in de lijst met gebruikers. Kies **Selecteren**.

1. Als u een waarde voor de rol in het SAML-verklaring verwacht de **rol selecteren** deelvenster, selecteer de relevante functieservices voor de gebruiker in de lijst. Kies **Selecteren**.

1. In de **toevoegen toewijzing** venster **toewijzen**.

### <a name="create-an-sap-fiori-test-user"></a>Maak een testgebruiker SAP Fiori

In deze sectie maakt u een gebruiker met de naam Britta Simon in SAP Fiori. Werken met uw interne SAP-team van experts of uw organisatie SAP-partner om toe te voegen van de gebruiker in de SAP Fiori-platform.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

1. Nadat de identiteitsprovider Azure AD is geactiveerd in SAP Fiori, probeert u toegang tot een van de volgende URL's voor het testen van eenmalige aanmelding (u mag niet worden gevraagd een gebruikersnaam en wachtwoord):

    * https:\//\<sapurl\>/sap/bc/bsp/sap/it00/default.htm
    * https:\//\<sapurl\>/sap/bc/bsp/sap/it00/default.htm

    > [!NOTE]
    > Vervang *sapurl* met de werkelijke naam van de SAP-host.

1. De URL van de test moet rekening houden u op naar de volgende pagina van de toepassing test in SAP. Als de pagina wordt geopend, is Azure AD eenmalige aanmelding ingesteld.

    ![De standaard toepassingspagina in SAP testen](./media/sapfiori-tutorial/testingsso.png)

1. Als u wordt gevraagd een gebruikersnaam en wachtwoord, schakelt u traceren om het probleem vast te stellen. Gebruik de volgende URL voor de tracering: https:\//\<sapurl\>/sap/bc/webdynpro/sap/sec_diag_tool? sap-client = 122 & sap-taal = EN #.

## <a name="next-steps"></a>Volgende stappen

Lees deze artikelen voor meer informatie:

- [Lijst met zelfstudies voor het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)
- [Eenmalige aanmelding bij toepassingen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)
- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
