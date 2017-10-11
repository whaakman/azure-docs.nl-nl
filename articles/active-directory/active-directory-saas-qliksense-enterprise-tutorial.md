---
title: 'Zelfstudie: Azure Active Directory-integratie met Qlik zin Enterprise | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Qlik zin Enterprise.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8c27e340-2b25-47b6-bf1f-438be4c14f93
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: jeedes
ms.openlocfilehash: 4964634cd5aaf0dbb98c766f5e12700c4d118750
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-qlik-sense-enterprise"></a>Zelfstudie: Azure Active Directory-integratie met Qlik zin Enterprise

In deze zelfstudie leert u hoe Qlik zin Enterprise integreren met Azure Active Directory (Azure AD).

Qlik zin Enterprise integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Qlik zin Enterprise heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Qlik zin Enterprise (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Qlik zin voor ondernemingen, moet u de volgende items:

- Een Azure AD-abonnement
- Een Qlik zin Enterprise eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand hier downloaden: [proefversie aanbieding](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Qlik zin Enterprise uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-qlik-sense-enterprise-from-the-gallery"></a>Qlik zin Enterprise uit de galerie toevoegen
Voor het configureren van de integratie van Qlik zin Enterprise in Azure AD, moet u Qlik zin Enterprise uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Qlik zin Enterprise uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Qlik zin Enterprise**, selecteer **Qlik zin Enterprise** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Qlik zin onderneming in de lijst met resultaten](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met Qlik zin Enterprise op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Qlik zin onderneming is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Qlik zin onderneming tot stand worden gebracht.

Wijs in Qlik zin Enterprise, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Qlik zin voor ondernemingen, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Qlik zin Enterprise](#create-a-qlik-sense-enterprise-test-user)**  - Qlik zin Enterprise die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing Qlik zin Enterprise.

**Voor het configureren van Azure AD eenmalige aanmelding met Qlik zin voor ondernemingen, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Qlik zin Enterprise** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_samlbase.png)

3. Op de **Qlik zin Enterprise domein en de URL's** sectie, voert u de volgende stappen uit:

    ![URL's en Qlik zin Enterprise domein eenmalige aanmelding informatie](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://<Qlik Sense Fully Qualifed Hostname>:443//samlauthn/`
    
    > [!NOTE]
    > Noteer de afsluitende slash aan het einde van deze URI. Dit is vereist.
    
    b. In de **id** textbox, typ een URL met het volgende patroon volgen:
    | |
    |--|
    | `https://<Qlik Sense Fully Qualifed Hostname>.qlikpoc.com`|
    | `https://<Qlik Sense Fully Qualifed Hostname>.qliksense.com`|

    > [!NOTE] 
    > Deze waarden zijn niet echt. Bijwerken van deze waarden met de werkelijke aanmeldings-URL en de id, die worden beschreven verderop in deze zelfstudie of neem contact op met [Qlik zin Enterprise Client ondersteuningsteam](https://www.qlik.com/us/services/support) ophalen van deze waarden. 

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_certificate.png) 

5. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_400.png)

6. Bereid het federatieve metagegevens-XML-bestand zodat u die naar Qlik zin-server uploaden kunt.
   
    > [!NOTE]
    > Voordat u de metagegevens van de IdP uploadt naar de server Qlik zin, het bestand moet worden bewerkt om informatie om te controleren of de juiste werking tussen Azure AD te verwijderen en Qlik zin-server.
    
    ![QlikSense][qs24]
   
    a. Open het bestand FederationMetaData.xml, dat u hebt gedownload vanuit Azure-portal in een teksteditor.
   
    b. Zoeken naar de waarde **RoleDescriptor**.  Er zijn vier gegevens (twee paren openen en sluiten elementlabels).
   
    c. De codes RoleDescriptor en alle informatie daartussen uit het bestand verwijderen.
   
    d. Sla het bestand en het in de buurt te houden voor gebruik verderop in dit document.

7. Navigeer naar de Qlik zin Qlik Management Console (QMC) als een gebruiker die virtuele proxyconfiguraties kunt maken.

8. Klik in de QMC op de **virtuele proxy's** menu-item.
   
    ![QlikSense][qs6] 

9. Aan de onderkant van het scherm, klikt u op de **nieuw** knop.
   
    ![QlikSense][qs7]

10. Het scherm virtuele proxy bewerken wordt weergegeven.  Aan de rechterkant van het scherm wordt een menu voor het zichtbaar maken van configuratie-opties.
   
    ![QlikSense][qs9]

11. Voer de gegevens voor de virtuele Azure-proxy-configuratie met de id-menuoptie gecontroleerd.
    
    ![QlikSense][qs8]  
    
    a. De **beschrijving** veld is een beschrijvende naam voor de virtuele-proxyconfiguratie.  Voer een waarde voor een beschrijving.
    
    b. De **voorvoegsel** veld identificeert het eindpunt van de virtuele proxy voor het verbinden met Qlik zin met Azure AD eenmalige aanmelding.  Voer een naam uniek voorvoegsel voor deze virtuele proxy.
    
    c. **Time-out voor inactiviteit sessie (minuten)** is de time-out voor verbindingen via deze virtuele proxy.
    
    d. De **cookie-kop sessienaam** is de naam van de cookie voor het opslaan van de sessie-id voor de sessie Qlik zin is een gebruiker na een geslaagde authenticatie ontvangt.  Deze naam moet uniek zijn.

12. Klik op de menuoptie verificatie zichtbaar te maken.  De verificatie-scherm wordt weergegeven.
    
    ![QlikSense][qs10]
    
    a. De **anonieme toegangsmodus** vervolgkeuzelijst bepaalt als anonieme gebruikers toegang Qlik zin via de virtuele-proxy tot mogelijk.  De standaardoptie is geen anonieme gebruiker.
    
    b. De **verificatiemethode** vervolgkeuzelijst bepaalt het verificatieschema dat de virtuele-proxy wordt gebruikt.  Selecteer SAML in de vervolgkeuzelijst.  Meer opties als gevolg hiervan worden weergegeven.
    
    c. In de **SAML host URI veld**, voer de gebruikers van de hostnaam invoeren voor toegang tot Qlik zin via deze virtuele SAML-proxy.  De hostnaam is de uri van de server Qlik zin.
    
    d. In de **SAML entiteit-ID**, voert u de dezelfde waarde die is ingevoerd voor het veld SAML host URI.
    
    e. De **SAML IdP metagegevens** is het bestand bewerkt eerder in de **Federatiemetagegevens van Azure AD-configuratie bewerken** sectie.  **Voordat u uploadt de IdP-metagegevens, het bestand moet worden bewerkt** gegevens om te controleren of de juiste werking tussen Azure AD te verwijderen en Qlik zin-server.  **Raadpleeg de bovenstaande instructies als het bestand nog heeft moeten worden bewerkt.**  Klik op de knop Bladeren en selecteer vervolgens het bewerkte metagegevensbestand te uploaden naar de virtuele-proxyconfiguratie als het bestand is bewerkt.
    
    f. Voer in de kenmerkverwijzing naam of het schema voor de SAML-kenmerk voor de **UserID** Azure AD naar de server Qlik zin verzendt.  Schema referentie-informatie is beschikbaar in de configuratie van Azure app schermen post.  Voer voor het gebruik van het kenmerk name `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.
    
    g. Voer de waarde voor de **gebruikerslijst** die wordt gekoppeld aan gebruikers wanneer ze zich met Qlik zin server via Azure AD verifiëren.  Hardcoded waarden moeten worden omgeven door **vierkante haakjes []**.  Voor het gebruik van een kenmerk in de Azure AD SAML-bevestiging verzonden, voert u de naam van het kenmerk in dit tekstvak **zonder** vierkante haken.
    
    h. De **SAML-ondertekeningsalgoritme** Hiermee stelt u de serviceprovider (in dit geval Qlik zin server) voor Certificaatondertekening voor de virtuele-proxyconfiguratie.  Als Qlik zin server gebruikmaakt van een vertrouwd certificaat gegenereerd met behulp van de Microsoft Enhanced RSA and AES Cryptographic Provider, wijzigt u de SAML-ondertekeningsalgoritme naar **SHA-256**.
    
    ik. De SAML-kenmerk toewijzing sectie kan extra kenmerken zoals groepen worden verzonden naar Qlik zin voor gebruik in regels.

13. Klik op de **LOAD BALANCING** menuoptie zichtbaar te maken.  Het scherm Load Balancing wordt weergegeven.
    
    ![QlikSense][qs11]

14. Klik op de **toevoegen nieuwe serverknooppunt** knop, selecteer engine knooppunt of knooppunten Qlik zin stuurt sessies laden voor taakverdeling doeleinden, en klik op de **toevoegen** knop.
    
    ![QlikSense][qs12]

15. Klik op de geavanceerde optie zichtbaar te maken. Het geavanceerde scherm wordt weergegeven.
    
    ![QlikSense][qs13]
    
    De Host witte lijst identificeert hostnamen die worden geaccepteerd wanneer ze verbinden met de server Qlik zin.  **Geef de hostnaam van de gebruikers wordt opgeven bij het verbinden met server Qlik zin.** De hostnaam is dezelfde waarde als het SAML-uri die host zonder de https://.

16. Klik op de **toepassen** knop.
    
    ![QlikSense][qs14]

17. Klik op OK accepteer het waarschuwingsbericht dat aangeeft proxy's die zijn gekoppeld aan de virtuele-proxy wordt opnieuw gestart.
    
    ![QlikSense][qs15]

18. Klik aan de rechterkant van het scherm de gekoppelde items menu wordt weergegeven.  Klik op de **proxy's** menuoptie.
    
    ![QlikSense][qs16]

19. De proxy-scherm wordt weergegeven.  Klik op de **koppeling** knop onder een proxy te koppelen aan de virtuele-proxy.
    
    ![QlikSense][qs17]

20. Selecteer de proxyknooppunt dat ondersteuning biedt voor deze virtuele proxyverbinding en klikt u op de **koppeling** knop.  Na het koppelen, wordt de proxy wordt vermeld onder de bijbehorende proxy's.
    
    ![QlikSense][qs18]
  
    ![QlikSense][qs19]

21. Na ongeveer vijf tot tien seconden verschijnt het bericht QMC vernieuwen.  Klik op de **QMC vernieuwen** knop.
    
    ![QlikSense][qs20]

22. Wanneer de QMC is vernieuwd, klikt u op de **virtuele proxy's** menu-item. De nieuwe SAML virtuele proxy invoer wordt vermeld in de tabel op het scherm.  Één klik op de vermelding virtuele proxy.
    
    ![QlikSense][qs51]

23. Aan de onderkant van het scherm wordt de metagegevens-knop downloaden SP geactiveerd.  Klik op de **downloaden SP metagegevens** om op te slaan van de metagegevens naar een bestand.
    
    ![QlikSense][qs52]

24. Open het metagegevensbestand van de serviceprovider.  Houd rekening met de **id van de entiteit** vermelding en de **AssertionConsumerService** vermelding.  Deze waarden gelijk zijn aan de **id** en de **aanmelden URL** in de configuratie van de Azure AD-toepassing. Plak deze waarden in de **Qlik zin Enterprise domein en de URL's** sectie in de configuratie van de toepassing Azure AD als ze niet overeen komen en vervolgens u deze in de configuratiewizard van Azure AD-App vervangen moet.
    
    ![QlikSense][qs53]

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

   ![De Azure Active Directory-knop](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

   !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

   ![De knop toevoegen](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

   ![Het dialoogvenster gebruiker](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_04.png)

   a. In de **naam** in het vak **BrittaSimon**.

   b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

   c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

   d. Klik op **Create**.
 
### <a name="create-a-qlik-sense-enterprise-test-user"></a>Maak een testgebruiker Qlik zin Enterprise

In deze sectie kunt u een gebruiker met de naam van Britta Simon in Qlik zin onderneming maken. Werken met [Qlik zin Enterprise Client ondersteuningsteam](https://www.qlik.com/us/services/support) om toe te voegen de gebruikers van het platform Qlik zin Enterprise. Gebruikers moeten worden gemaakt en worden geactiveerd voordat u eenmalige aanmelding gebruiken. 

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding toegang verleent tot Qlik zin onderneming gebruiken.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen Qlik zin Enterprise, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Qlik zin Enterprise**.

    ![De koppeling Qlik zin Enterprise in de lijst met toepassingen](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Qlik zin Enterprise in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Qlik zin Enterprise. 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_203.png

[qs6]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_06.png
[qs7]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_07.png
[qs8]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_08.png
[qs9]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_09.png
[qs10]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_10.png
[qs11]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_11.png
[qs12]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_12.png
[qs13]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_13.png
[qs14]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_14.png
[qs15]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_15.png
[qs16]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_16.png
[qs17]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_17.png
[qs18]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_18.png
[qs19]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_19.png
[qs20]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_20.png
[qs24]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_24.png
[qs51]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_51.png
[qs52]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_52.png
[qs53]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_53.png

