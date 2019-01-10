---
title: 'Zelfstudie: Azure Active Directory-integratie met Qlik Sense Enterprise | Microsoft Docs'
description: Ontdek hoe u eenmalige aanmelding configureert tussen Azure Active Directory en Qlik Sense Enterprise.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 8c27e340-2b25-47b6-bf1f-438be4c14f93
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/17/2018
ms.author: jeedes
ms.openlocfilehash: 5ecb26a7ca0f164c2ba8d9a9de26bf19777653a0
ms.sourcegitcommit: 9f87a992c77bf8e3927486f8d7d1ca46aa13e849
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/28/2018
ms.locfileid: "53810625"
---
# <a name="tutorial-azure-active-directory-integration-with-qlik-sense-enterprise"></a>Zelfstudie: Azure Active Directory-integratie met Qlik Sense Enterprise

In deze zelfstudie leert u hoe u Qlik Sense Enterprise integreert in Azure Active Directory (Azure AD).
Het integreren van Qlik Sense Enterprise in Azure AD biedt de volgende voordelen:

* U kunt in Azure AD bepalen wie toegang krijgt tot Qlik Sense Enterprise.
* U kunt inschakelen dat gebruikers automatisch met hun Azure AD-account worden aangemeld bij Qlik Sense Enterprise (eenmalige aanmelding).
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Als u de Azure AD-integratie met Qlik Sense Enterprise wilt configureren, hebt u de volgende zaken nodig:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Een abonnement op Qlik Sense Enterprise waarvoor eenmalige aanmelding is ingeschakeld

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Qlik Sense Enterprise ondersteunt door **SP** geïnitieerde eenmalige aanmelding

## <a name="adding-qlik-sense-enterprise-from-the-gallery"></a>Qlik Sense Enterprise uit de galerie toevoegen

Als u de integratie van Qlik Sense Enterprise in Azure AD wilt configureren, moet u Qlik Sense Enterprise vanuit de galerie toevoegen aan uw lijst beheerde SaaS-apps.

**Als u Qlik Sense Enterprise uit de galerie wilt toevoegen, moet u de volgende stappen uitvoeren:**

1. Klik in het linkernavigatievenster in de **[Azure-portal](https://portal.azure.com)** op het **Azure Active Directory**-pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **Qlik Sense Enterprise**, selecteer **Qlik Sense Enterprise** in het deelvenster met resultaten en klik vervolgens op **Toevoegen** om de toepassing toe te voegen.

     ![Qlik Sense Enterprise in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie configureert en test u eenmalige aanmelding van Azure AD met Qlik Sense Enterprise op basis van een testgebruiker met de naam **Britta Simon**.
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Azure AD-gebruiker en de daaraan gerelateerde gebruiker in Qlik Sense Enterprise tot stand is gebracht.

Voltooi de volgende bouwstenen om eenmalige aanmelding van Azure AD voor Qlik Sense Enterprise te configureren en testen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Qlik Sense Enterprise-eenmalige aanmelding configureren](#configure-qlik-sense-enterprise-single-sign-on)**: als u de instellingen voor eenmalige aanmelding aan de clientzijde wil configureren.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Een Qlik Sense Enterprise-testgebruiker maken](#create-qlik-sense-enterprise-test-user)**: als u een equivalent van Britta Simon wilt maken in Qlik Sense Enterprise dat is gekoppeld aan de Azure AD-versie van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Als u de eenmalige aanmelding van Azure AD bij Qlik Sense Enterprise wilt configureren, voert u de volgende stappen uit:

1. In [Azure Portal](https://portal.azure.com/) selecteert u op de integratiepagina van de **Qlik Sense Enterprise**-toepassing de optie **Eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Informatie over eenmalige aanmelding bij het Qlik Sense Enterprise-domein en Qlik Sense Enterprise-URL's](common/sp-identifier-reply.png)

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<Qlik Sense Fully Qualifed Hostname>:4443/azure/hub`

    b. Typ in het tekstvak **Id** een URL met het volgende notatie:
    | |
    |--|
    | `https://<Qlik Sense Fully Qualifed Hostname>.qlikpoc.com`|
    | `https://<Qlik Sense Fully Qualifed Hostname>.qliksense.com`|
    | |

    c. In het tekstvak **Antwoord-URL** typt u een URL met de volgende indeling:

    `https://<Qlik Sense Fully Qualifed Hostname>:4443/samlauthn/`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke aanmeldings-URL, id en antwoord-URL die verderop in deze zelfstudie worden beschreven of neem contact op met het [Qlik Sense Enterprise-ondersteuningsteam](https://www.qlik.com/us/services/support) om deze waarden te verkrijgen.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens** te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

### <a name="configure-qlik-sense-enterprise-single-sign-on"></a>Qlik Sense Enterprise-eenmalige aanmelding configureren

1. Bereid het XML-bestand met federatieve metagegevens voor zodat u die naar de Qlik Sense-server kunt uploaden.

    > [!NOTE]
    > Voordat u de metagegevens van de IdP naar de Qlik Sense-server uploadt, moet u het bestand bewerken om gegevens te verwijderen voor een goede werking tussen Azure AD en de Qlik Sense-server.

    ![QlikSense][qs24]

    a. Open het bestand FederationMetaData.xml dat u vanuit Azure Portal naar een teksteditor hebt gedownload.

    b. Zoek de waarde **RoleDescriptor**.  Er zijn vier items (twee paren openings- en afsluitelementlabels).

    c. Verwijder de RoleDescriptor-labels en alle gegevens ertussen uit het bestand.

    d. Sla het bestand op en houd deze bij de hand voor later gebruik in dit document.

2. Navigeer naar de Qlik Management Console (QMC) van Qlik Sense als een gebruiker die de configuraties van virtuele webproxy's kan maken.

3. Klik in de QMC op het menu-item **Virtuele proxy's**.

    ![QlikSense][qs6]

4. Klik aan de onderkant van het scherm op de knop **Nieuw**.

    ![QlikSense][qs7]

5. Het bewerkingsscherm voor de virtuele proxy wordt weergegeven.  Aan de rechterkant van het scherm ziet u een menu voor het maken van configuratie-opties.

    ![QlikSense][qs9]

6. Voer, terwijl de id-menuoptie is ingeschakeld, de identificatiegegevens in voor de configuratie van de virtuele proxy in Azure.

    ![QlikSense][qs8]  

    a. In het veld **Beschrijving** vindt u een beschrijvende naam voor de configuratie van de virtuele proxy.  Voer een waarde voor een beschrijving in.

    b. In het veld **Voorvoegsel** wordt het eindpunt van de virtuele proxy bepaald voor het verbinding maken met Qlik Sense via Azure AD-eenmalige aanmelding.  Voer een unieke voorvoegselnaam in voor deze virtuele proxy.

    c. **Sessietime-out bij inactiviteit (minuten)** is de time-out voor verbindingen via deze virtuele proxy.

    d. De **Sessienaam cookieheader** is de naam van de cookie waarin de sessie-id van de Qlik Sense-sessie wordt opgeslagen welke een gebruiker na een geslaagde verificatie ontvangt.  Deze naam moet uniek zijn.

7. Klik op de menuoptie Verificatie om deze zichtbaar te maken.  Het scherm Verificatie wordt weergegeven.

    ![QlikSense][qs10]

    a. In de vervolgkeuzelijst **anonieme toegangsmodus** bepaalt u of anonieme gebruikers toegang kunnen hebben tot Qlik Sense via de virtuele proxy.  De standaardoptie is Geen anonieme gebruiker.

    b. In de vervolgkeuzelijst **verificatiemethode** bepaalt u het verificatieschema dat door de virtuele proxy wordt gebruikt.  Selecteer SAML in de vervolgkeuzelijst.  Hierdoor worden meer opties weergegeven.

    c. Geef in het veld **Host-URI SAML** de hostnaam op die gebruikers invoeren om Qlik Sense via deze virtuele SAML-proxy te openen.  De hostnaam is de URI van de Qlik Sense-server.

    d. Geef in de **Entiteit-ID SAML** dezelfde waarde op die u hebt ingevoerd voor het veld Host-URI SAML.

    e. De **IdP-metagegevens SAML** is het bestand dat eerder in de sectie **Federatieve metagegevens bewerken van Azure AD-configuratie** is bewerkt.  **Voordat u de metagegevens van de IdP uploadt, moet u het bestand bewerken** om gegevens te verwijderen voor een goede werking tussen Azure AD en de Qlik Sense-server.  **Raadpleeg de bovenstaande instructies als het bestand nog moet worden bewerkt.**  Klik, als het bestand is bewerkt, op de knop Bladeren en selecteer het bewerkte metagegevensbestand om dit naar de virtuele-proxyconfiguratie te uploaden.

    f. Voer de kenmerknaam of schemareferentie in voor het SAML-kenmerk waarin de **UserID** wordt weergegeven die Azure AD naar de Qlik Sense-server verzendt.  Schemareferentiegegevens zijn beschikbaar in de Azure-appschermen na configuratie.  Voer `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name` in om gebruik te maken van het naamkenmerk.

    g. Voer de waarde in voor de **gebruikerslijst** die zal worden gekoppeld aan gebruikers wanneer deze worden geverifieerd met de Qlik Sense-server via Azure AD.  Vastgelegde waarden moeten tussen **vierkante haken []** staan.  Als u een kenmerk wilt gebruiken dat in de Azure AD SAML-assertie is verzonden, voert u in dit tekstvak de naam van het kenmerk in **zonder** vierkante haken.

    h. Met het **algoritme voor SAML-ondertekening** stelt u de serviceprovider (in dit geval de Qlik Sense-server) in voor certificaatondertekening voor de virtuele-proxyconfiguratie.  Als de Qlik Sense-server gebruikmaakt van een vertrouwd certificaat dat is gegenereerd met behulp van de Microsoft Enhanced RSA en AES Cryptographic Provider, wijzigt u het SAML-ondertekeningsalgoritme in **SHA-256**.

    i. In de sectie voor het toewijzen van SAML-kenmerken kunt u aanvullende kenmerken toewijzen, zoals groepen die naar Qlik Sense moeten worden verzonden voor gebruik in regels.

8. Klik op de menuoptie **TAAKVERDELING** om deze zichtbaar te maken.  Het scherm Taakverdeling wordt weergegeven.

    ![QlikSense][qs11]

9. Klik op de knop **Nieuw serverknooppunt toevoegen**, selecteer een of meer engineknooppunten waarnaar Qlik Sense voor taakverdelingsdoeleinden sessies verzendt, en klik op de knop **Toevoegen**.

    ![QlikSense][qs12]

10. Klik op de menuoptie Geavanceerd om deze zichtbaar te maken. Het scherm Geavanceerd wordt weergegeven.

    ![QlikSense][qs13]

    Op de lijst met toegestane hosts bevinden zich hostnamen die worden geaccepteerd wanneer deze verbinding maken met de Qlik Sense-server.  **Voer de hostnaam in die gebruikers opgeven wanneer deze verbinding maken met de Qlik Sense-server.** De hostnaam heeft dezelfde waarde als de host-URI van SAML zonder de https://.

11. Klik op de knop **Toepassen**.

    ![QlikSense][qs14]

12. Klik op OK om de waarschuwing te accepteren waarin wordt gemeld dat proxy's die zijn gekoppeld aan de virtuele proxy opnieuw worden gestart.

    ![QlikSense][qs15]

13. Aan de rechterkant van het scherm wordt het menu Gekoppelde items weergegeven.  Klik op de menu-optie **Proxy's**.

    ![QlikSense][qs16]

14. Het proxy-scherm wordt weergegeven.  Klik op de knop **Koppeling** onderaan om een proxy aan de virtuele proxy te koppelen.

    ![QlikSense][qs17]

15. Selecteer het proxyknooppunt dat deze virtuele proxyverbinding ondersteunt en klik op de knop **Koppeling**.  Na de koppeling wordt de proxy vermeld bij Gekoppelde proxy's.

    ![QlikSense][qs18]
  
    ![QlikSense][qs19]

16. Na ongeveer vijf tot tien seconden verschijnt het bericht QMC vernieuwen.  Klik op de knop **QMC vernieuwen**.

    ![QlikSense][qs20]

17. Klik, wanneer de QMC is vernieuwd, op het menu-item **Virtuele proxy's**. De vermelding van de nieuwe virtuele SAML-proxy wordt in de tabel op het scherm weergegeven.  Klik eenmaal op de vermelding van de virtuele proxy.

    ![QlikSense][qs51]

18. Aan de onderkant van het scherm wordt de knop SP-metagegevens downloaden geactiveerd.  Klik op de knop **SP-metagegevens downloaden** om de metagegevens in een bestand op te slaan.

    ![QlikSense][qs52]

19. Open het bestand met SP-metagegevens.  Bekijk de vermelding **entityID** en de vermelding **AssertionConsumerService**.  Deze waarden zijn gelijk aan de **Id**, **Aanmeldings-URL** en de **Antwoord-URL** in de configuratie van de Azure AD-toepassing. Plak deze waarden in de sectie **Qlik Sense Enterprise-domein en -URL's** van de configuratie van de Azure AD-toepassing als deze niet overeenkomen. Vervolgens dient u deze in de wizard Azure AD-app configureren te vervangen.

    ![QlikSense][qs53]

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

In dit gedeelte gaat u Britta Simon toestemming geven voor gebruik van Azure-eenmalige aanmelding door haar toegang te geven tot Qlik Sense Enterprise.

1. Selecteer in Azure Portal de optie **Bedrijfstoepassingen**, selecteer **Alle toepassingen**en selecteer vervolgens **Qlik Sense Enterprise**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Typ en selecteer **Qlik Sense Enterprise** in de lijst met toepassingen.

    ![De koppeling Qlik Sense Enterprise in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-qlik-sense-enterprise-test-user"></a>Een Qlik Sense Enterprise-testgebruiker maken

In deze sectie maakt u de gebruiker Britta Simon in Qlik Sense Enterprise. Voeg in samenwerking met het  [Qlik Sense Enterprise-ondersteuningsteam](https://www.qlik.com/us/services/support) de gebruikers toe in het Qlik Sense Enterprise-platform. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie gaat u uw configuratie van Azure AD-eenmalige aanmelding testen via het toegangsvenster.

Wanneer u op de tegel Qlik Sense Enterprise in het toegangsvenster klikt, wordt u automatisch aangemeld bij de instantie van Qlik Sense Enterprise waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

<!--Image references-->

[qs6]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_06.png
[qs7]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_07.png
[qs8]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_08.png
[qs9]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_09.png
[qs10]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_10.png
[qs11]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_11.png
[qs12]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_12.png
[qs13]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_13.png
[qs14]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_14.png
[qs15]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_15.png
[qs16]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_16.png
[qs17]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_17.png
[qs18]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_18.png
[qs19]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_19.png
[qs20]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_20.png
[qs24]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_24.png
[qs51]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_51.png
[qs52]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_52.png
[qs53]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_53.png

