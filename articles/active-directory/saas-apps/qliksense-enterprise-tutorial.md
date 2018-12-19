---
title: 'Zelfstudie: Azure Active Directory-integratie met Qlik Sense Enterprise | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Qlik Sense Enterprise.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 8c27e340-2b25-47b6-bf1f-438be4c14f93
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/06/2018
ms.author: jeedes
ms.openlocfilehash: 24353f89f7c833a16675841f741a4a81ac34597c
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2018
ms.locfileid: "53015214"
---
# <a name="tutorial-azure-active-directory-integration-with-qlik-sense-enterprise"></a>Zelfstudie: Azure Active Directory-integratie met Qlik Sense Enterprise

In deze zelfstudie leert u hoe u Qlik Sense Enterprise integreren met Azure Active Directory (Azure AD).

Qlik Sense Enterprise integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Qlik Sense Enterprise heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Qlik Sense Enterprise (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Qlik Sense Enterprise, moet u de volgende items:

- Een Azure AD-abonnement
- Een Qlik Sense Enterprise eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u een proefversie van één maand hier downloaden: [proefversie aanbieding](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Qlik Sense Enterprise uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-qlik-sense-enterprise-from-the-gallery"></a>Qlik Sense Enterprise uit de galerie toe te voegen
Voor het configureren van de integratie van Qlik Sense Enterprise in Azure AD, moet u Qlik Sense Enterprise uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Qlik Sense Enterprise uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **Qlik Sense Enterprise**, selecteer **Qlik Sense Enterprise** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Qlik Sense onderneming in de lijst met resultaten](./media/qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Qlik Sense Enterprise op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Qlik Sense onderneming is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Qlik Sense onderneming tot stand worden gebracht.

In Qlik Sense Enterprise, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Qlik Sense Enterprise, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Qlik Sense Enterprise](#create-a-qlik-sense-enterprise-test-user)**  : als u wilt een equivalent van Britta Simon in Qlik Sense onderneming die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Qlik Sense Enterprise.

**Voor het configureren van Azure AD eenmalige aanmelding met Qlik Sense Enterprise, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Qlik Sense Enterprise** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![In het dialoogvenster voor eenmalige aanmelding](./media/qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_samlbase.png)

3. Op de **Qlik Sense Enterprise domein en URL's** sectie, voert u de volgende stappen uit:

    ![Qlik Sense Enterprise domein en URL's, eenmalige aanmelding informatie](./media/qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<Qlik Sense Fully Qualifed Hostname>:4443/azure/hub`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon:
    
    | |
    |--|
    | `https://<Qlik Sense Fully Qualifed Hostname>.qlikpoc.com`|
    | `https://<Qlik Sense Fully Qualifed Hostname>.qliksense.com`|
    | |

    c. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon:

    `https://<Qlik Sense Fully Qualifed Hostname>:4443/samlauthn/`

    > [!NOTE]
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke aanmeldings-URL, id en antwoord-URL, die worden beschreven verderop in deze zelfstudie of neem contact op met [Qlik Sense Enterprise Client-ondersteuningsteam](https://www.qlik.com/us/services/support) om deze waarden te verkrijgen.

4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_certificate.png)

5. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/qliksense-enterprise-tutorial/tutorial_general_400.png)

6. Bereid het federatieve metagegevens-XML-bestand zodat u die naar Qlik Sense-server uploaden kunt.

    > [!NOTE]
    > Voordat u de metagegevens van de IdP uploadt naar de server Qlik Sense, het bestand moet worden bewerkt om gegevens om ervoor te zorgen goede werking tussen Azure AD te verwijderen en Qlik Sense-server.

    ![QlikSense][qs24]

    a. Open het bestand FederationMetaData.xml, die u hebt gedownload vanuit Azure portal in een teksteditor.

    b. Zoek de waarde **RoleDescriptor**.  Er zijn vier items (twee paren van openen en sluiten van element tags).

    c. Verwijder tussen de codes RoleDescriptor en alle gegevens uit het bestand.

    d. Sla het bestand op en bewaar deze in de buurt voor gebruik verderop in dit document.

7. Navigeer naar de Qlik Sense Qlik Management Console (QMC) als een gebruiker die de configuraties van virtuele webproxy's kunt maken.

8. Klik in de QMC op de **virtuele proxy's** menu-item.

    ![QlikSense][qs6]

9. Aan de onderkant van het scherm, klikt u op de **nieuw** knop.

    ![QlikSense][qs7]

10. Het bewerkingsscherm van de virtuele-proxy wordt weergegeven.  Aan de rechterkant van het scherm is een menu voor het maken van configuratie-opties zichtbaar.

    ![QlikSense][qs9]

11. Met de id-menuoptie is ingeschakeld, voer de identificatiegegevens voor de Azure virtuele proxyconfiguratie.

    ![QlikSense][qs8]  

    a. De **beschrijving** veld is een beschrijvende naam voor de virtuele-proxyconfiguratie.  Voer een waarde voor een beschrijving.

    b. De **voorvoegsel** veld identificeert het eindpunt van de virtuele-proxy voor het verbinden met Qlik Sense met Azure AD eenmalige aanmelding.  Voer een van de unieke voorvoegselnaam voor deze virtuele-proxy.

    c. **Sessietime-out voor inactiviteit (minuten)** is de time-out voor verbindingen via deze virtuele proxy.

    d. De **sessienaam cookie-header** is de naam van de cookie voor het opslaan van de sessie-id voor de Qlik Sense sessie een gebruiker na een geslaagde authenticatie ontvangt.  Deze naam moet uniek zijn.

12. Klik op de menuoptie verificatie zichtbaar te maken.  De verificatie-scherm wordt weergegeven.

    ![QlikSense][qs10]

    a. De **anonieme toegangsmodus** vervolgkeuzelijst bepaalt als anonieme gebruikers kunnen toegang hebben tot Qlik Sense via de virtuele-proxy.  De standaardoptie is geen anonieme gebruiker.

    b. De **verificatiemethode** vervolgkeuzelijst bepaalt het verificatieschema de virtuele-proxy wordt gebruikt.  Selecteer SAML in de vervolgkeuzelijst.  Meer opties als gevolg hiervan worden weergegeven.

    c. In de **SAML host URI veld**, voer de gebruikers van de hostnaam invoeren voor toegang tot Qlik Sense via deze virtuele SAML-proxy.  De hostnaam is de uri van de Qlik Sense-server.

    d. In de **SAML entiteit-ID**, dezelfde waarde ingevoerd voor het veld SAML host URI invoeren.

    e. De **IdP SAML-metagegevens** is het bestand bewerkt eerder in de **Federatiemetagegevens bewerken van Azure AD-configuratie** sectie.  **Voordat u uploadt de metagegevens van de id-provider, het bestand moet worden bewerkt** gegevens om ervoor te zorgen goede werking tussen Azure AD te verwijderen en Qlik Sense-server.  **Raadpleeg de bovenstaande instructies als het bestand nog heeft kunnen worden bewerkt.**  Klik op de bladerknop en selecteer vervolgens het bewerkte metagegevensbestand te uploaden naar de virtuele-proxyconfiguratie als het bestand is bewerkt.

    f. Voer in de kenmerkverwijzing naam of het schema voor de SAML-kenmerk voor de **UserID** Azure AD verzendt naar de Qlik Sense-server.  Schema-referentie-informatie is beschikbaar in de Azure-app schermen post-configuratie.  Voer voor het gebruik van het kenmerk name `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.

    g. Voer de waarde voor de **gebruikerslijst** die zal worden gekoppeld aan gebruikers wanneer ze worden geverifieerd met de server Qlik Sense via Azure AD.  Vastgelegde waarden moeten tussen **vierkant vierkante haken []**.  Voor het gebruik van een kenmerk in de Azure AD-SAML-verklaring verzonden, voert u de naam van het kenmerk in dit tekstvak **zonder** tussen vierkante haken.

    h. De **algoritme voor ondertekening die SAML** Hiermee stelt u de serviceprovider (in dit geval Qlik Sense-server) voor Certificaatondertekening voor de virtuele-proxyconfiguratie.  Als Qlik Sense server gebruikmaakt van een vertrouwd certificaat gegenereerd met behulp van de Microsoft Enhanced RSA and AES Cryptographic Provider, wijzigt u de SAML-ondertekeningsalgoritme naar **SHA-256**.

    i. De sectie voor SAML-kenmerk toewijzing kunt voor aanvullende kenmerken, zoals groepen moet worden verzonden naar Qlik Sense voor gebruik in regels.

13. Klik op de **LOAD BALANCING** menuoptie zichtbaar te maken.  Het scherm Load Balancing wordt weergegeven.

    ![QlikSense][qs11]

14. Klik op de **de nieuwe server-knooppunt toevoegen** knop, selecteer engine knooppunt of knooppunten Qlik Sense ontvangt sessies laden voor taakverdeling doeleinden, en klikt u op de **toevoegen** knop.

    ![QlikSense][qs12]

15. Klik op de geavanceerde optie zichtbaar te maken. Het geavanceerde scherm wordt weergegeven.

    ![QlikSense][qs13]

    De Host acceptatielijst identificeert hostnamen die bij het verbinden met de server Qlik Sense worden geaccepteerd.  **Voer de hostnaam die gebruikers opgeven bij het verbinden met Qlik Sense-server.** De hostnaam is dezelfde waarde als de uri van de host SAML zonder de https://.

16. Klik op de **toepassen** knop.

    ![QlikSense][qs14]

17. Klik op OK om de waarschuwing waarin wordt vermeld proxy's die zijn gekoppeld aan de virtuele-proxy wordt opnieuw gestart te accepteren.

    ![QlikSense][qs15]

18. Aan de rechterkant van het scherm, is het gekoppelde items menu wordt weergegeven.  Klik op de **proxy's** menu-optie.

    ![QlikSense][qs16]

19. De proxy-scherm wordt weergegeven.  Klik op de **koppeling** onder een proxy koppelen aan de virtuele-proxy.

    ![QlikSense][qs17]

20. Selecteer de proxyknooppunt dat ondersteuning biedt voor deze virtuele proxyverbinding en klikt u op de **koppeling** knop.  Na de koppeling, worden de proxy vermeld in de bijbehorende proxy's.

    ![QlikSense][qs18]
  
    ![QlikSense][qs19]

21. Na ongeveer vijf tot tien seconden verschijnt het bericht QMC vernieuwen.  Klik op de **QMC vernieuwen** knop.

    ![QlikSense][qs20]

22. Wanneer de QMC is vernieuwd, klikt u op de **virtuele proxy's** menu-item. Het nieuwe item van de SAML-virtuele-proxy wordt vermeld in de tabel op het scherm.  Één klik op de vermelding van de virtuele-proxy.

    ![QlikSense][qs51]

23. Aan de onderkant van het scherm wordt de knop downloaden SP-metagegevens geactiveerd.  Klik op de **downloaden SP metagegevens** om op te slaan van de metagegevens naar een bestand.

    ![QlikSense][qs52]

24. Open het bestand met sp metagegevens.  Bekijk de **entityID** vermelding en de **AssertionConsumerService** vermelding.  Deze waarden zijn gelijk aan de **id**, **aanmeldings-URL** en de **antwoord-URL** in de configuratie van de Azure AD-toepassing. Plak deze waarden in de **Qlik Sense Enterprise domein en URL's** sectie in de configuratie van de toepassing Azure AD als ze niet overeenkomen met de, en vervolgens u deze in de wizard Azure AD-App configureren vervangen moet.

    ![QlikSense][qs53]

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

   ![De Azure Active Directory-knop](./media/qliksense-enterprise-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

   !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/qliksense-enterprise-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

   ![De knop toevoegen](./media/qliksense-enterprise-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

   ![Het dialoogvenster gebruiker](./media/qliksense-enterprise-tutorial/create_aaduser_04.png)

   a. In de **naam** in het vak **BrittaSimon**.

   b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

   c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

   d. Klik op **Create**.

### <a name="create-a-qlik-sense-enterprise-test-user"></a>Maak een testgebruiker Qlik Sense Enterprise

In deze sectie maakt u een gebruiker met de naam van Britta Simon in Qlik Sense onderneming. Werken met [Qlik Sense Enterprise Client-ondersteuningsteam](https://www.qlik.com/us/services/support) om toe te voegen de gebruikers in het Qlik Sense Enterprise-platform. Gebruikers moeten worden gemaakt en worden geactiveerd voordat u eenmalige aanmelding gebruiken.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Qlik Sense onderneming.

![De de gebruikersrol toewijzen][200]

**Als u wilt toewijzen Britta Simon naar Qlik Sense Enterprise, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **Qlik Sense Enterprise**.

    ![De koppeling Qlik Sense Enterprise in de lijst met toepassingen](./media/qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Qlik Sense Enterprise in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Qlik Sense Enterprise.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/qliksense-enterprise-tutorial/tutorial_general_01.png
[2]: ./media/qliksense-enterprise-tutorial/tutorial_general_02.png
[3]: ./media/qliksense-enterprise-tutorial/tutorial_general_03.png
[4]: ./media/qliksense-enterprise-tutorial/tutorial_general_04.png

[100]: ./media/qliksense-enterprise-tutorial/tutorial_general_100.png

[200]: ./media/qliksense-enterprise-tutorial/tutorial_general_200.png
[201]: ./media/qliksense-enterprise-tutorial/tutorial_general_201.png
[202]: ./media/qliksense-enterprise-tutorial/tutorial_general_202.png
[203]: ./media/qliksense-enterprise-tutorial/tutorial_general_203.png

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