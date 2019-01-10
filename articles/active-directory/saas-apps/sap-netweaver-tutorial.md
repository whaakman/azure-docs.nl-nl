---
title: 'Zelfstudie: Azure Active Directory-integratie met SAP NetWeaver | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en SAP NetWeaver.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 1b9e59e3-e7ae-4e74-b16c-8c1a7ccfdef3
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/19/2018
ms.author: jeedes
ms.openlocfilehash: 607d05818966e62407795640d223f1aed2f59bbb
ms.sourcegitcommit: 33091f0ecf6d79d434fa90e76d11af48fd7ed16d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2019
ms.locfileid: "54156745"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-netweaver"></a>Zelfstudie: Azure Active Directory-integratie met SAP NetWeaver

In deze zelfstudie leert u hoe u SAP NetWeaver integreren met Azure Active Directory (Azure AD).

SAP NetWeaver integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de SAP NetWeaver heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij SAP NetWeaver (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met SAP NetWeaver, moet u de volgende items:

- Een Azure AD-abonnement
- SAP NetWeaver eenmalige aanmelding ingeschakeld abonnement
- SAP NetWeaver V7.20 vereist ten minste

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving.
Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. SAP NetWeaver uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-sap-netweaver-from-the-gallery"></a>SAP NetWeaver uit de galerie toe te voegen

Voor het configureren van de integratie van SAP NetWeaver in Azure AD, moet u SAP NetWeaver uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen SAP NetWeaver uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **SAP NetWeaver**, selecteer **SAP NetWeaver** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![SAP NetWeaver in de lijst met resultaten](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met SAP NetWeaver op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in SAP NetWeaver is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in SAP NetWeaver tot stand worden gebracht.

Als u wilt configureren en Azure AD eenmalige aanmelding met SAP NetWeaver testen, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van SAP NetWeaver testgebruiker](#creating-sapnetweaver-test-user)**  : als u wilt een equivalent van Britta Simon hebben in SAP NetWeaver die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw SAP NetWeaver-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met SAP NetWeaver, moet u de volgende stappen uitvoeren:**

1. Open een nieuw browservenster en log in uw bedrijf SAP NetWeaver site als beheerder

2. Zorg ervoor dat **http** en **https** services actief zijn en de juiste poorten zijn toegewezen **SMICM** T-Code.

3. Meld u aan bij de zakelijke client van SAP-systeem (T01), waarbij eenmalige aanmelding vereist is en beveiliging van de HTTP-sessie Management activeren.

    a. Ga naar de code van de transactie **SICF_SESSIONS**. Alle relevante profiel parameters met de huidige waarden worden weergegeven. Ze als volgt uit:-
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
    > Aanpassen hierboven parameters aan de hand van de vereisten van uw organisatie, hierboven parameters zijn opgegeven als alleen-vermelding.

    b. Indien nodig de parameters in het exemplaar/standaardprofiel van SAP-systeem aanpassen en opnieuw starten van SAP-systeem.

    c. Dubbelklik op relevante client waarmee HTTP-sessie van beveiliging.

    ![De downloadkoppeling certificaat](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_profileparameter.png)

    d. Hieronder SICF services te activeren:
    ```
    /sap/public/bc/sec/saml2
    /sap/public/bc/sec/cdc_ext_service
    /sap/bc/webdynpro/sap/saml2
    /sap/bc/webdynpro/sap/sec_diag_tool (This is only to enable / disable trace)
    ```
4. Ga naar de code van de transactie **SAML2** in business-client van SAP-systeem [T01/122]. Een gebruikersinterface wordt in een browser geopend. In dit voorbeeld wordt ervan uitgegaan dat 122 als SAP business-client.

    ![De downloadkoppeling certificaat](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_sapbusinessclient.png)

5. Geef uw gebruikersnaam en wachtwoord in te voeren in de gebruikersinterface en klikt u op **bewerken**.

    ![De downloadkoppeling certificaat](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_userpwd.png)

6. Vervang **providernaam** van T01122 naar `http://T01122` en klikt u op **opslaan**.

    > [!NOTE]
    > Geleverd door de naam van de provider standaard <sid> <client> indeling, maar Azure AD wordt verwacht dat de naam in de indeling van <protocol>://<name>, aanbeveelt voor het onderhouden van de naam van de provider als https://<sid> <client> om toe te staan van meerdere SAP NetWeaver ABAP-engines te configureren in Azure AD.

    ![De downloadkoppeling certificaat](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_providername.png)

7. **Genereren van de metagegevens van Provider**: - als we klaar bent met het configureren van de **lokale Provider** en **vertrouwde Providers** instellingen op de gebruikersinterface van SAML 2.0, de volgende stap is te bestand met metagegevens van de serviceprovider (die zou bevatten alle instellingen, verificatie-contexten en andere configuraties in SAP) genereren. Wanneer dit bestand is gegenereerd, moeten we dit uploaden in Azure AD.

    ![De downloadkoppeling certificaat](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_generatesp.png)

    a. Ga naar **tabblad lokale Provider**.

    b. Klik op **metagegevens**.

    c. Sla het gegenereerde **Metadata XML-bestand** op uw computer en upload dit in **SAML-basisconfiguratie** sectie polulate automatisch de **id** en  **Antwoord-URL** waarden in Azure portal.

8. In de Azure-portal op de **SAP NetWeaver** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

9. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster, klikt u op **Selecteer** voor **SAML** modus voor eenmalige aanmelding inschakelen.

    ![Eenmalige aanmelding configureren](common/tutorial_general_301.png)

10. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** pictogram openen **SAML-basisconfiguratie** dialoogvenster.

    ![Eenmalige aanmelding configureren](common/editconfigure.png)

11. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    a. Klik op **metagegevensbestand uploaden** uploaden de **Service Provider-bestand met metagegevens** die u eerder hebt ontvangen.

    ![Metagegevensbestand uploaden](common/editmetadataupload.png)

    b. Klik op **map logo** voor het selecteren van het bestand met metagegevens en klikt u op **uploaden**.

    ![Metagegevensbestand uploaden](common/uploadmetadata.png)

    c. Zodra het bestand met metagegevens is geüpload, de **id** en **antwoord-URL** waarden ophalen automatisch ingevuld **SAML-basisconfiguratie** sectie tekstvak, zoals hieronder wordt weergegeven :

    ![SAP NetWeaver-domein en URL's, eenmalige aanmelding informatie](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_url.png)

    d. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<your company instance of SAP NetWeaver>`

12. SAP NetWeaver-toepassing wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit de sectie **Gebruikerskenmerken** op de integratiepagina van de toepassing-beheren. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op de knop **Bewerken** om het dialoogvenster **Gebruikerskenmerken** te openen.

    ![Kenmerk-sectie](./media/sapnetweaver-tutorial/edit_attribute.png)

13. In de sectie **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** configureert u het kenmerk van het SAML-token zoals wordt weergegeven in de bovenstaande afbeelding en voert u de volgende stappen uit:

    a. Klik op **bewerken** pictogram opent de **gebruikersclaims beheren** dialoogvenster.
    
    ![Kenmerk-sectie](./media/sapnetweaver-tutorial/nameidattribute.png)

    b. Op de **gebruikersclaims beheren** tabblad, voert u de volgende stappen uit:

    ![Kenmerk-sectie](./media/sapnetweaver-tutorial/nameidattribute1.png)

    * Selecteer **transformatie**.
  
    * Uit de **transformatie** in de lijst met `ExtractMailPrefix()`.
  
    * Uit de **Parameter 1** in de lijst met `user.userprincipalname`.

    * Klik op **Opslaan**.

14. Op de **SAML-handtekeningcertificaat** pagina, in de **SAML-handtekeningcertificaat** sectie, klikt u op **downloaden** downloaden **voorfederatievemetagegevens-XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_certificate.png)

15. Op de **instellen van SAP NetWeaver** sectie, kopieert u de juiste URL aan de hand van uw behoeften.

    a. Aanmeldings-URL

    b. Azure AD-id

    c. URL voor afmelden

    ![SAP NetWeaver-configuratie](common/configuresection.png)

16. Meld u aan bij de SAP-systeem en Ga naar transactiecode SAML2. Het browservenster geopend met SAML-Configuratiescherm.

17. Voor het configureren van eindpunten voor de id van de vertrouwde provider (Azure AD) Ga naar **vertrouwde Providers** tabblad.

    ![Eenmalige aanmelding configureren](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_samlconfig.png)

18. Druk op **toevoegen** en selecteer **metagegevensbestand uploaden** in het contextmenu.

    ![Eenmalige aanmelding configureren](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_uploadmetadata.png)

19. Uploaden van bestand met metagegevens, die u hebt gedownload vanuit Azure portal.

    ![Eenmalige aanmelding configureren](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_metadatafile.png)

20. Typ de naam van de Alias in het volgende scherm. Bijvoorbeeld aadsts en druk op **volgende** om door te gaan.

    ![Eenmalige aanmelding configureren](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_aliasname.png)

21. Zorg ervoor dat uw **Digest-algoritme** moet **SHA-256** en eventuele wijzigingen vereisen en druk op **volgende**.

    ![Eenmalige aanmelding configureren](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_identityprovider.png)

22. Op **Single Sign-On eindpunten**, gebruikt u **HTTP POST** en klikt u op **volgende** om door te gaan.

    ![Eenmalige aanmelding configureren](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_httpredirect.png)

23. Op **eenmalige afmelding eindpunten** Selecteer **HTTPRedirect** en klikt u op **volgende** om door te gaan.

    ![Eenmalige aanmelding configureren](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_httpredirect1.png)

24. Op **artefact eindpunten**, drukt u op **volgende** om door te gaan.

    ![Eenmalige aanmelding configureren](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_artifactendpoint.png)

25. Op **verificatievereisten**, klikt u op **voltooien**.

    ![Eenmalige aanmelding configureren](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_authentication.png)

26. Ga naar het tabblad **vertrouwde Provider** > **identiteitsfederatie** (aan de onderkant van het scherm). Klik op **Bewerken**.

    ![Eenmalige aanmelding configureren](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_trustedprovider.png)

27. Klik op **toevoegen** onder de **identiteitsfederatie** tabblad (onderste venster).

    ![Eenmalige aanmelding configureren](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_addidentityprovider.png)

28. Selecteer in het pop-upvenster **onbepaald** uit de **NameID ondersteunde indelingen** en klik op OK.

    ![Eenmalige aanmelding configureren](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_nameid.png)

29. Houd er rekening mee dat **gebruiker-ID bron** en **toewijzing van de gebruikersmodus-id** waarden bepalen de koppeling tussen SAP-gebruiker en Azure AD-claim.  

    ####<a name="scenario-sap-user-to-azure-ad-user-mapping"></a>Scenario: SAP-gebruiker toewijzing van de Azure AD-gebruiker.

    a. NameID details schermopname van SAP.

    ![Eenmalige aanmelding configureren](./media/sapnetweaver-tutorial/nameiddetails.png)

    b. Schermopname vereiste claims van Azure AD vermelden.

    ![Eenmalige aanmelding configureren](./media/sapnetweaver-tutorial/claimsaad1.png)

    ####<a name="scenario-select-sap-user-id-based-on-configured-email-address-in-su01-in-this-case-email-id-should-be-configured-in-su01-for-each-user-who-requires-sso"></a>Scenario: Selecteer de gebruikers-id van SAP op basis van geconfigureerde e-mailadres in SU01. In dit geval moet e-mail-id worden geconfigureerd in su01 voor elke gebruiker die eenmalige aanmelding is vereist.

    a.  NameID details schermopname van SAP.

    ![Eenmalige aanmelding configureren](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_nameiddetails1.png)

    b. Schermopname vereiste claims van Azure AD vermelden.

    ![Eenmalige aanmelding configureren](./media/sapnetweaver-tutorial/claimsaad2.png)

30. Klik op **opslaan** en klik vervolgens op **inschakelen** om in te schakelen van id-provider.

    ![Eenmalige aanmelding configureren](./media/sapnetweaver-tutorial/configuration1.png)

31. Klik op **OK** wanneer u hierom wordt gevraagd.

    ![Eenmalige aanmelding configureren](./media/sapnetweaver-tutorial/configuration2.png)

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

1. Selecteer in de Azure portal, in het linkerdeelvenster **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.

    ![Azure AD-gebruiker maken][100]

2. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm.

    ![Het maken van een Azure AD-testgebruiker](common/create_aaduser_01.png)

3. In de eigenschappen van de gebruiker de volgende stappen uitvoeren.

    ![Het maken van een Azure AD-testgebruiker](common/create_aaduser_02.png)

    a. In de **naam** veld **BrittaSimon**.
  
    b. In de **gebruikersnaam** veld, typt u **brittasimon@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Selecteer **eigenschappen**, selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in het wachtwoord.

    d. Selecteer **Maken**.

### <a name="creating-sap-netweaver-test-user"></a>Het maken van de gebruiker van SAP NetWeaver testen

In deze sectie maakt u een gebruiker met de naam van Britta Simon in SAP NetWeaver. Uw intern deskundige SAP-team of werkt samen met uw organisatie SAP-partner om toe te voegen de gebruikers in de SAP NetWeaver-platform.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan SAP NetWeaver.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **SAP NetWeaver**.

    ![Eenmalige aanmelding configureren](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. In de **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst met gebruikers, en klik op de **Selecteer** knop aan de onderkant van het scherm.

6. In de **toevoegen toewijzing** dialoogvenster Selecteer de **toewijzen** knop.

### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

1. Nadat u de id-provider Azure AD is geactiveerd, probeer het openen van onderstaande URL om te controleren op eenmalige aanmelding (er wordt geen prompt voor gebruikersnaam en wachtwoord)

    `https://<sapurl>/sap/bc/bsp/sap/it00/default.htm`

    (of) de onderstaande URL gebruiken

    `https://<sapurl>/sap/bc/bsp/sap/it00/default.htm`

    > [!NOTE]
    > Vervang sapurl door de werkelijke hostnaam van de SAP.

2. De bovenstaande URL kost u hieronder genoemde scherm. Als u kunnen bereiken, tot de onderstaande pagina setup van Azure AD-eenmalige aanmelding wordt bevestigd.

    ![Eenmalige aanmelding configureren](./media/sapnetweaver-tutorial/testingsso.png)

3. Als de gebruikersnaam en wachtwoord prompt weergegeven, neem vaststellen van het probleem door inschakelen de tracering met behulp van onderstaande URL

    `https://<sapurl>/sap/bc/webdynpro/sap/sec_diag_tool?sap-client=122&sap-language=EN#`

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: common/tutorial_general_01.png
[2]: common/tutorial_general_02.png
[3]: common/tutorial_general_03.png
[4]: common/tutorial_general_04.png

[100]: common/tutorial_general_100.png

[201]: common/tutorial_general_201.png
[202]: common/tutorial_general_202.png
[203]: common/tutorial_general_203.png
