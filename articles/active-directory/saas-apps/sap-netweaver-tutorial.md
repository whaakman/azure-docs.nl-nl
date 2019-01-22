---
title: 'Zelfstudie: Integratie van Azure Active Directory met SAP NetWeaver | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en SAP NetWeaver.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 1b9e59e3-e7ae-4e74-b16c-8c1a7ccfdef3
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/07/2019
ms.author: jeedes
ms.openlocfilehash: 6bbe34cca1d5f3630f95c2748c852be24e5781d6
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2019
ms.locfileid: "54266159"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-netweaver"></a>Zelfstudie: Azure Active Directory-integratie met SAP NetWeaver

In deze zelfstudie leert u hoe u SAP NetWeaver integreert met Azure Active Directory (Azure AD).
De integratie van SAP NetWeaver met Azure AD heeft de volgende voordelen:

* U kunt in Azure AD beheren wie toegang tot SAP NetWeaver heeft.
* U kunt inschakelen dat gebruikers automatisch met hun Azure AD-account worden aangemeld bij SAP NetWeaver (eenmalige aanmelding).
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met SAP NetWeaver hebt u de volgende zaken nodig:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Een abonnement op SAP NetWeaver waarvoor eenmalige aanmelding is ingeschakeld
* Voor SAP NetWeaver V7.20 is ten minste vereist

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* SAP NetWeaver ondersteunt door **SP** geïnitieerde eenmalige aanmelding

## <a name="adding-sap-netweaver-from-the-gallery"></a>SAP NetWeaver toevoegen vanuit de galerie

Voor het configureren van de integratie van SAP NetWeaver met Azure AD moet u SAP NetWeaver uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u SAP NetWeaver vanuit de galerie wilt toevoegen, moet u de volgende stappen uitvoeren:**

1. Klik in het linkernavigatievenster in de **[Azure-portal](https://portal.azure.com)** op het **Azure Active Directory**-pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ **SAP NetWeaver** in het zoekvak, selecteer **SAP NetWeaver** in het deelvenster met resultaten en klik op **Toevoegen** om de toepassing toe te voegen.

     ![SAP NetWeaver in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In dit gedeelte kunt u eenmalige aanmelding via Azure AD configureren en testen met SAP NetWeaver op basis van een testgebruiker met de naam **Britta Simon**.
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Azure AD-gebruiker en de daaraan gerelateerde gebruiker in SAP NetWeaver tot stand is gebracht.

Voor het configureren en testen van eenmalige aanmelding via Azure AD bij SAP NetWeaver moet u de volgende bouwstenen uitvoeren:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Eenmalige aanmelding voor SAP NetWeaver configureren](#configure-sap-netweaver-single-sign-on)**: als u de instellingen voor eenmalige aanmelding aan de toepassingszijde wilt configureren.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[De testgebruiker voor SAP NetWeaver maken](#create-sap-netweaver-test-user)**: als u een equivalent van Britta Simon in SAP NetWeaver wilt hebben dat gekoppeld is aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD-eenmalige aanmelding met SAP NetWeaver moet u de volgende stappen uitvoeren:

1. Open een nieuw browservenster en meld u aan als beheerder bij de SAP NetWeaver-site van uw bedrijf

2. Zorg ervoor dat **http**- en **https**-services actief zijn en de juiste poorten zijn toegewezen in T-code **SMICM**.

3. Meld u aan bij de bedrijfsclient van het SAP-systeem (T01), waarvoor eenmalige aanmelding is vereist, en activeer HTTP-beveiligingssessie Management.

    a. Ga naar transactiecode **SICF_SESSIONS**. Hier worden alle relevante profielparameters met de huidige waarden weergegeven. Deze zien eruit als hieronder:
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
    > Pas de bovenstaande parameters aan volgens de vereisten van uw organisatie. De bovenstaande parameters worden hier slechts als indicatie weergegeven.

    b. Pas zo nodig de parameters aan in het exemplaar-/standaardprofiel van het SAP-systeem en start het SAP-systeem opnieuw op.

    c. Dubbelklik op de relevante client om de HTTP-beveiligingssessie in te schakelen.

    ![De link om het certificaat te downloaden](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_profileparameter.png)

    d. Activeer de onderstaande SICF-services:
    ```
    /sap/public/bc/sec/saml2
    /sap/public/bc/sec/cdc_ext_service
    /sap/bc/webdynpro/sap/saml2
    /sap/bc/webdynpro/sap/sec_diag_tool (This is only to enable / disable trace)
    ```
4. Ga naar transactiecode **SAML2** in de bedrijfsclient van het SAP-systeem [T01/122]. In een browser wordt een gebruikersinterface geopend. In dit voorbeeld wordt ervanuit gegaan dat 122 een SAP-bedrijfsclient is.

    ![De link om het certificaat te downloaden](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_sapbusinessclient.png)

5. Voer uw gebruikersnaam en wachtwoord in de gebruikersinterface in en klik op **Edit**.

    ![De link om het certificaat te downloaden](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_userpwd.png)

6. Vervang **Provider Name** T01122 door `http://T01122` en klik op **Save**.

    > [!NOTE]
    > Standaard wordt Provider Name in de indeling <sid><client> weergegeven, maar in Azure AD wordt verwacht dat de naam in de indeling <protocol>://<name> wordt weergegeven, waarbij wordt aangeraden Provider Name als https://<sid><client> te handhaven zodat meerdere SAP NetWeaver ABAP-engines in Azure AD kunnen worden geconfigureerd.

    ![De link om het certificaat te downloaden](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_providername.png)

7. **Metagegevens van de serviceprovider genereren**: als de instellingen **Local Provider** en **Trusted Providers** in de SAML 2.0-gebruikersinterface zijn geconfigureerd, bestaat de volgende stap uit het genereren van het bestand met metagegevens van de serviceprovider (dat alle instellingen, contexten voor verificatie en overige configuraties in SAP bevat). Als dit bestand is gegenereerd, moet het in Azure AD worden geüpload.

    ![De link om het certificaat te downloaden](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_generatesp.png)

    a. Ga naar het tabblad **Local Provider**.

    b. Klik op **Metadata**.

    c. Sla het gegenereerde **XML-bestand met metagegevens** op de computer op en upload het naar de sectie **Basic SAML Configuration** om de waarden **Identifier** en **Reply URL** automatisch in de Azure-portal in te vullen.

8. Selecteer in [Azure Portal](https://portal.azure.com/) op de pagina voor integratie van de **SAP NetWeaver**-toepassing **Eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

9. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

10. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

11. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    a. Klik op **Metagegevensbestand uploaden** om het **metagegevensbestand van de serviceprovider** te uploaden dat u eerder hebt verkregen.

    ![Metagegevensbestand uploaden](common/upload-metadata.png)

    b. Klik op het **mappictogram** om het metagegevensbestand te selecteren en klik op **Uploaden**.

    ![Metagegevensbestand kiezen](common/browse-upload-metadata.png)

    c. Nadat het bestand met metagegevens is geüpload, worden de waarden voor **Identifier** en **Reply URL** automatisch ingevuld in het tekstvak van de sectie **Standaard SAML-configuratie**, zoals hieronder weergegeven:

    ![Informatie over eenmalige aanmelding bij het SAP NetWeaver-domein en SAP NetWeaver-URL's](common/sp-identifier-reply.png)

    In het tekstvak **Aanmeldings-URL** typt u een URL met het volgende patroon: `https://<your company instance of SAP NetWeaver>`

12. Voor de SAP NetWeaver-toepassing worden de SAML-asserties in een bepaalde indeling verwacht. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit de sectie **Gebruikerskenmerken** op de integratiepagina van de toepassing-beheren. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op de knop **Bewerken** om het dialoogvenster **Gebruikerskenmerken** te openen.

    ![image](common/edit-attribute.png)

13. In de sectie **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** configureert u het kenmerk van het SAML-token zoals wordt weergegeven in de bovenstaande afbeelding en voert u de volgende stappen uit:

    a. Klik op **Pictogram bewerken** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    ![image](./media/sapnetweaver-tutorial/nameidattribute.png)

    ![image](./media/sapnetweaver-tutorial/nameidattribute1.png)

    b. Selecteer in de lijst **Transformatie** **ExtractMailPrefix()**.

    c. Selecteer **user.userprinicipalname** in de lijst **Parameter 1**.

    d. Klik op **Opslaan**.

14. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

15. In de sectie **SAP NetWeaver instellen** kopieert u de juiste URL('s) op basis van uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-sap-netweaver-single-sign-on"></a>Eenmalige aanmelding configureren voor SAP NetWeaver

1. Meld u aan bij het SAP-systeem en ga naar transactiecode SAML2. Er wordt een nieuwe browservenster geopend met een SAML-configuratiescherm.

2. Ga naar het tabblad **Trusted Providers** om eindpunten voor vertrouwde identiteitsproviders (Azure AD) te configureren.

    ![Eenmalige aanmelding configureren](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_samlconfig.png)

3. Druk op **Add** en selecteer **Upload Metadata File** in het snelmenu.

    ![Eenmalige aanmelding configureren](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_uploadmetadata.png)

4. Upload het metagegevensbestand dat u eerder in de Azure-portal hebt gedownload.

    ![Eenmalige aanmelding configureren](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_metadatafile.png)

5. Typ de aliasnaam in het volgende scherm. Bijvoorbeeld aadsts en druk op **Next** om door te gaan.

    ![Eenmalige aanmelding configureren](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_aliasname.png)

6. Zorg ervoor dat **Digest Algorithm** **SHA-256** is, vereis geen wijzigingen en druk op **Next**.

    ![Eenmalige aanmelding configureren](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_identityprovider.png)

7. Gebruik bij **Single Sign-On Endpoints** **HTTP POST** en klik op **Next** om door te gaan.

    ![Eenmalige aanmelding configureren](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_httpredirect.png)

8. Gebruik bij **Single Logout Endpoints** **HTTPRedirect** en klik op **Next** om door te gaan.

    ![Eenmalige aanmelding configureren](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_httpredirect1.png)

9. Druk bij **Artifact Endpoints** op **Next** om door te gaan.

    ![Eenmalige aanmelding configureren](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_artifactendpoint.png)

10. Klik bij **Authentication Requirements** op **Finish**.

    ![Eenmalige aanmelding configureren](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_authentication.png)

11. Ga naar het tabblad **Trusted Provider** > **Identity Federation** (vanaf de onderkant van het scherm). Klik op **Bewerken**.

    ![Eenmalige aanmelding configureren](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_trustedprovider.png)

12. Klik onder het tabblad **Identity Federation** op **Add** (onderste venster).

    ![Eenmalige aanmelding configureren](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_addidentityprovider.png)

13. Selecteer in het pop-upvenster **Supported NameID formats** de optie **Unspecified** en klik op OK.

    ![Eenmalige aanmelding configureren](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_nameid.png)

14. De waarden **user ID Source** en **user id mapping mode** bepalen de koppeling tussen de SAP-gebruiker en de Azure AD-claim.  

    #### <a name="scenario-sap-user-to-azure-ad-user-mapping"></a>Scenario: Toewijzing van SAP-gebruiker aan Azure AD-gebruiker.

    a. Schermopname van details NameID van SAP.

    ![Eenmalige aanmelding configureren](./media/sapnetweaver-tutorial/nameiddetails.png)

    b. Schermopname met Vereiste claims van Azure AD.

    ![Eenmalige aanmelding configureren](./media/sapnetweaver-tutorial/claimsaad1.png)

    #### <a name="scenario-select-sap-user-id-based-on-configured-email-address-in-su01-in-this-case-email-id-should-be-configured-in-su01-for-each-user-who-requires-sso"></a>Scenario: Selecteer de SAP-gebruikers-id op basis van geconfigureerd e-mailadres in SU01. In dit geval moet de e-mail-id voor elke gebruiker waarvoor eenmalige aanmelding is vereist, worden geconfigureerd in SU01.

    a.  Schermopname van details NameID van SAP.

    ![Eenmalige aanmelding configureren](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_nameiddetails1.png)

    b. Schermopname met Vereiste claims van Azure AD.

    ![Eenmalige aanmelding configureren](./media/sapnetweaver-tutorial/claimsaad2.png)

15. Klik op **Save** en vervolgens op **Enable** om de id-provider in te schakelen.

    ![Eenmalige aanmelding configureren](./media/sapnetweaver-tutorial/configuration1.png)

16. Klik op **OK** zodra daarom wordt gevraagd.

    ![Eenmalige aanmelding configureren](./media/sapnetweaver-tutorial/configuration2.png)

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

In dit gedeelte schakelt u Britta Simon in voor gebruik van eenmalige aanmelding met Azure door toegang te verlenen aan SAP NetWeaver.

1. Selecteer **Bedrijfstoepassingen** in Azure Portal, selecteer **Alle toepassingen** en selecteer vervolgens **SAP NetWeaver**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Typ en selecteer **SAP NetWeaver** in de lijst met toepassingen.

    ![De SAP NetWeaver-koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-sap-netweaver-test-user"></a>SAP NetWeaver-testgebruiker maken

In deze sectie maakt u een gebruiker met de naam Britta Simon in SAP NetWeaver. Werk samen met uw interne SAP-deskundige of met de SAP-partner van uw organisatie om de gebruikers aan het SAP NetWeaver-platform toe te voegen.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

1. Als de id-provider Azure AD is geactiveerd, opent u de onderstaande URL om eenmalige aanmelding te controleren (u wordt niet om gebruikersnaam en wachtwoord gevraagd)

    `https://<sapurl>/sap/bc/bsp/sap/it00/default.htm`

    (of) gebruik de onderstaande URL

    `https://<sapurl>/sap/bc/bsp/sap/it00/default.htm`

    > [!NOTE]
    > Vervang sapurl door de feitelijke SAP-hostnaam.

2. Via de bovenstaande URL moet u op het hieronder genoemd scherm terechtkomen. Als u de onderstaande pagina kunt bereiken, is eenmalige aanmelding voor Azure AD ingesteld.

    ![Eenmalige aanmelding configureren](./media/sapnetweaver-tutorial/testingsso.png)

3. Als u om een gebruikersnaam en wachtwoord wordt gevraagd, probeert u het probleem vast te stellen door de tracering in te schakelen met behulp van de onderstaande URL

    `https://<sapurl>/sap/bc/webdynpro/sap/sec_diag_tool?sap-client=122&sap-language=EN#`

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

