---
title: 'Zelfstudie: Azure Active Directory-integratie met Microsoft Azure Active Directory eenmalige aanmelding voor JIRA 5.2 | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Microsoft Azure Active Directory eenmalige aanmelding voor JIRA 5.2.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d0c00408-f9b8-4a79-bccc-c346a7331845
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2018
ms.author: jeedes
ms.openlocfilehash: 456a0c63881f6eb15e8166bee0105e25c951b536
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="tutorial-azure-active-directory-integration-with-microsoft-azure-active-directory-single-sign-on-for-jira-52"></a>Zelfstudie: Azure Active Directory-integratie met Microsoft Azure Active Directory eenmalige aanmelding voor JIRA 5.2

In deze zelfstudie leert u hoe Microsoft Azure Active Directory eenmalige aanmelding voor JIRA 5.2 integreren met Azure Active Directory (Azure AD).

Microsoft Azure Active Directory eenmalige aanmelding voor JIRA 5.2 integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Microsoft Azure Active Directory eenmalige aanmelding voor JIRA 5.2 heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Microsoft Azure Active Directory eenmalige aanmelding voor JIRA 5.2 (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="description"></a>Beschrijving

Uw Microsoft Azure Active Directory-account gebruiken met Atlassian JIRA server eenmalige aanmelding inschakelen. Op deze manier alle gebruikers in uw organisatie kunnen de Azure AD-referenties gebruiken om aan te melden bij de toepassing JIRA. Deze invoegtoepassing maakt gebruik van SAML 2.0 voor Federatie.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Microsoft Azure Active Directory eenmalige aanmelding voor JIRA 5.2, moet u de volgende items:

- Een Azure AD-abonnement
- JIRA Core en 5.2 van de Software moet geïnstalleerd en geconfigureerd op Windows 64-bits versie
- JIRA-server is een HTTPS-functionaliteit
- Houd er rekening mee dat in de ondersteunde versies voor de invoegtoepassing JIRA worden vermeld onder sectie.
- JIRA server bereikbaar is op internet met name naar Azure AD-aanmeldingspagina voor verificatie en moet de token ontvangen van Azure AD
- Beheerdersreferenties worden ingesteld in JIRA
- WebSudo is uitgeschakeld in JIRA
- Testgebruiker gemaakt in de servertoepassing JIRA

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving van JIRA. De integratie in ontwikkeling of faseringsomgeving van de toepassing eerst te testen en gebruik vervolgens de productie-omgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

**Ondersteunde versies:**

*   JIRA Core en Software: 5.2
*   JIRA ondersteunt ook 6.0 en 7.2.0. Klik voor meer informatie [Microsoft Azure Active Directory eenmalige aanmelding voor JIRA](./active-directory-saas-jiramicrosoft-tutorial.md)

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Microsoft Azure Active Directory eenmalige aanmelding voor JIRA 5.2 uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-microsoft-azure-active-directory-single-sign-on-for-jira-52-from-the-gallery"></a>Microsoft Azure Active Directory eenmalige aanmelding voor JIRA 5.2 uit de galerie toevoegen
Voor het configureren van de integratie van Microsoft Azure Active Directory eenmalige aanmelding voor JIRA 5.2 in Azure AD, moet u Microsoft Azure Active Directory eenmalige aanmelding voor JIRA 5.2 uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen in Microsoft Azure Active Directory eenmalige aanmelding voor JIRA 5.2 uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Microsoft Azure Active Directory eenmalige aanmelding voor JIRA 5.2**, selecteer **Microsoft Azure Active Directory eenmalige aanmelding voor JIRA 5.2** van resultaat deelvenster klik vervolgens op  **Voeg** om toe te voegen van de toepassing.

    ![Microsoft Azure Active Directory eenmalige aanmelding voor JIRA 5.2 in de lijst met resultaten](.\media\active-directory-saas-msaadssojira5.2-tutorial\tutorial_singlesign-onforjira5.2_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Microsoft Azure Active Directory eenmalige aanmelding voor JIRA 5.2 op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Microsoft Azure Active Directory eenmalige aanmelding voor JIRA 5.2 is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Microsoft Azure Active Directory eenmalige aanmelding voor JIRA 5.2 tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Microsoft Azure Active Directory eenmalige aanmelding voor JIRA 5.2, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een Microsoft Azure Active Directory eenmalige aanmelding voor de testgebruiker JIRA 5.2](#create-a-singlesign-onforjira5.2-test-user)**  - hebben een equivalent van Britta Simon in Microsoft Azure Active Directory eenmalige aanmelding voor JIRA 5.2 die is gekoppeld aan de Azure AD-weergave van gebruiker .
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw Microsoft Azure Active Directory eenmalige aanmelding voor JIRA 5.2 toepassing configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Microsoft Azure Active Directory eenmalige aanmelding voor JIRA 5.2, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Microsoft Azure Active Directory eenmalige aanmelding voor JIRA 5.2** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](.\media\active-directory-saas-msaadssojira5.2-tutorial\tutorial_singlesign-onforjira5.2_samlbase.png)

3. Op de **Microsoft Azure Active Directory eenmalige aanmelding voor JIRA 5.2 domein en URL's** sectie, voert u de volgende stappen uit:

    ![Microsoft Azure Active Directory eenmalige aanmelding voor JIRA 5.2 domein- en URL's één aanmelding](.\media\active-directory-saas-msaadssojira5.2-tutorial\tutorial_singlesign-onforjira5.2_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen: `https://<domain:port>/plugins/servlet/saml/auth`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen: `https://<domain:port>/`

    c. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen: `https://<domain:port>/plugins/servlet/saml/auth`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke id, antwoord-URL en aanmeldings-URL. Poort is optioneel, mocht dat een benoemde URL. Deze waarden worden ontvangen tijdens de configuratie van de invoegtoepassing Jira, die verderop in de zelfstudie wordt beschreven.
 
4. Voor het genereren van de **metagegevens** -url, de volgende stappen uitvoeren:

    a. Klik op **App registraties**.
    
    ![Eenmalige aanmelding configureren](.\media\active-directory-saas-msaadssojira5.2-tutorial\appregistrations.png)
   
    b. Klik op **eindpunten** openen **eindpunten** in het dialoogvenster.  
    
    ![Eenmalige aanmelding configureren](.\media\active-directory-saas-msaadssojira5.2-tutorial\endpointicon.png)

    c. Klik op de knop kopiëren om te kopiëren **DOCUMENT met federatieve metagegevens** url en plak deze in Kladblok.
    
    ![Eenmalige aanmelding configureren](.\media\active-directory-saas-msaadssojira5.2-tutorial\endpoint.png)
     
    d. Nu gaat u naar de eigenschappenpagina van **Microsoft Azure Active Directory eenmalige aanmelding voor JIRA 5.2** en kopieer de **toepassings-Id** met **kopie** knop en plak deze in Kladblok.
 
    ![Eenmalige aanmelding configureren](.\media\active-directory-saas-msaadssojira5.2-tutorial\appid.png)

    e. Genereren van de **metagegevens-URL** met het volgende patroon volgen: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>` en kopieer deze waarde in Kladblok, zoals het later voor de configuratie van de invoegtoepassing gebruikt wordt.

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](.\media\active-directory-saas-msaadssojira5.2-tutorial\tutorial_general_400.png)

6. In een ander browservenster, meld u aan bij uw JIRA-exemplaar als beheerder.

7. Beweeg de muisaanwijzer op het tandwiel en klik op de **invoegtoepassingen**.
    
    ![Eenmalige aanmelding configureren](.\media\active-directory-saas-msaadssojira5.2-tutorial\addon1.png)

8. Klik onder sectie tabblad invoegtoepassingen op **invoegtoepassingen beheren**.

    ![Eenmalige aanmelding configureren](.\media\active-directory-saas-msaadssojira5.2-tutorial\addon7.png)

9. Download de invoegtoepassing van [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=56521). De opgegeven met behulp van Microsoft-invoegtoepassing handmatig uploaden **uploaden invoegtoepassing** menu. Het downloaden van de invoegtoepassing valt onder [Microsoft serviceovereenkomst](https://www.microsoft.com/en-us/servicesagreement/).

    ![Eenmalige aanmelding configureren](.\media\active-directory-saas-msaadssojira5.2-tutorial\addon12.png)

10. Zodra de invoegtoepassing is geïnstalleerd, wordt deze weergegeven **gebruiker geïnstalleerd** invoegtoepassingen-sectie. Klik op **configureren** voor het configureren van de nieuwe invoegtoepassing.

    ![Eenmalige aanmelding configureren](.\media\active-directory-saas-msaadssojira5.2-tutorial\addon13.png)

11. Voer de volgende stappen uit op de configuratiepagina:

    ![Eenmalige aanmelding configureren](.\media\active-directory-saas-msaadssojira5.2-tutorial\addon52.png)

    > [!TIP]
    > Zorg ervoor dat er slechts één certificaat toegewezen aan de app, zodat er geen fout is bij het oplossen van de metagegevens. Als er meerdere certificaten, bij het omzetten van de metagegevens, haalt de beheerder een fout opgetreden.
 
    a. In **metagegevens-URL** plak de **metagegevens-URL** gegenereerd op basis van Azure AD en klik op de **oplossen** knop. De URL van de metagegevens IdP worden gelezen en alle informatie in de velden gevuld.

    b. Kopiëren de **id, de antwoord-URL en de URL met eenmalige** waarden en plak ze in **id, de antwoord-URL en de URL met eenmalige** respectievelijk in tekstvakken **eenmalige aanmelding Microsoft Azure Active Directory voor JIRA 5.2 domein en URL's** sectie op Azure-portal.

    c. In **aanmeldingsnaam van de knop** Typ de naam van uw organisatie wil de gebruikers dat te zien op het aanmeldingsscherm knop.

    d. In **SAML-ID gebruikerslocaties** Selecteer **gebruikers-ID is in het element NameIdentifier van het onderwerp overzicht** of **gebruikers-ID is in een kenmerkelement**.  Deze ID is de JIRA gebruikers-id. Als de gebruikers-id niet overeen komt, kan vervolgens niet worden gebruikers zich aanmelden.

    > [!Note]
    > Gebruikers-ID van SAML-standaardlocatie is naam-id. U kunt dit wijzigen in een kenmerk-optie en geef de juiste naam. 
    
    e. Als u selecteert **gebruikers-ID is in een kenmerkelement** optie, klik dan in **kenmerknaam** textbox Typ de naam van het kenmerk waar gebruikers-Id wordt verwacht. 

    f. Als u het federatieve domein (zoals ADFS enz.) met Azure AD gebruikt, klikt u op de **inschakelen Thuisrealmdetectie** optie en configureer de **domeinnaam**.
    
    g. In **domeinnaam** typt u de domeinnaam hier in geval van de aanmelding op basis van AD FS.

    h. Controleer **eenmalige aanmelding inschakelen uit** als u zich afmelden bij Azure AD wanneer een gebruiker zich wilt afmeldt van JIRA. 

    i. Klik op **opslaan** knop de instellingen op te slaan.

    > [!NOTE]
    > Voor meer informatie over installatie en probleemoplossing, gaat u naar [MS JIRA SSO Connector Admin handleiding](ms-confluence-jira-plugin-adminguide.md) en er ook [Veelgestelde vragen over](ms-confluence-jira-plugin-faq.md) voor uw hulp

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](.\media\active-directory-saas-msaadssojira5.2-tutorial\create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](.\media\active-directory-saas-msaadssojira5.2-tutorial\create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](.\media\active-directory-saas-msaadssojira5.2-tutorial\create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](.\media\active-directory-saas-msaadssojira5.2-tutorial\create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-microsoft-azure-active-directory-single-sign-on-for-jira-52-test-user"></a>Een Microsoft Azure Active Directory eenmalige aanmelding voor JIRA 5.2 testgebruiker maken

Om Azure AD-gebruikers zich aanmelden bij JIRA on-premises server, moeten ze worden ingericht in Microsoft Azure Active Directory eenmalige aanmelding voor JIRA 5.2. Voor Microsoft Azure Active Directory eenmalige aanmelding voor JIRA 5.2 is inrichting een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw JIRA on-premises server als beheerder.

2. Beweeg de muisaanwijzer op het tandwiel en klik op de **Gebruikersbeheer**.

    ![Werknemer toevoegen](.\media\active-directory-saas-msaadssojira5.2-tutorial\user1.png) 

3. U wordt omgeleid naar de pagina beheerderstoegang in te voeren **wachtwoord** en klik op **bevestigen** knop.

    ![Werknemer toevoegen](.\media\active-directory-saas-msaadssojira5.2-tutorial\user2.png) 

4. Onder **Gebruikersbeheer** tabblad gedeelte, klikt u op **gebruiker maken**.

    ![Werknemer toevoegen](.\media\active-directory-saas-msaadssojira5.2-tutorial\user3.png) 

5. Op de **'Een nieuwe gebruiker maken'** dialoogvenster pagina, voert u de volgende stappen uit:

    ![Werknemer toevoegen](.\media\active-directory-saas-msaadssojira5.2-tutorial\user4.png) 

    a. In de **e-mailadres** textbox, typ het e-mailadres van gebruiker, zoals Brittasimon@contoso.com.

    b. In de **volledige naam** textbox, volledige naam van de gebruiker zoals Britta Simon type.

    c. In de **gebruikersnaam** textbox, typ het e-mailadres van gebruiker, zoals Brittasimon@contoso.com.

    d. In de **wachtwoord** textbox, typt u het wachtwoord van gebruiker.

    e. Klik op **gebruiker maken**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang tot Microsoft Azure Active Directory eenmalige aanmelding voor JIRA 5.2.

![Toewijzen van de gebruikersrol][200] 

**Als u wilt toewijzen Britta Simon aan Microsoft Azure Active Directory eenmalige aanmelding voor JIRA 5.2, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Microsoft Azure Active Directory eenmalige aanmelding voor JIRA 5.2**.

    ![De Microsoft Azure Active Directory eenmalige aanmelding voor JIRA 5.2-koppeling in de lijst met toepassingen](.\media\active-directory-saas-msaadssojira5.2-tutorial\tutorial_singlesign-onforjira5.2_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de Microsoft Azure Active Directory eenmalige aanmelding voor JIRA 5.2-tegel in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw Microsoft Azure Active Directory eenmalige aanmelding voor JIRA 5.2-toepassing.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: .\media\active-directory-saas-msaadssojira5.2-tutorial\tutorial_general_01.png
[2]: .\media\active-directory-saas-msaadssojira5.2-tutorial\tutorial_general_02.png
[3]: .\media\active-directory-saas-msaadssojira5.2-tutorial\tutorial_general_03.png
[4]: .\media\active-directory-saas-msaadssojira5.2-tutorial\tutorial_general_04.png

[100]: .\media\active-directory-saas-msaadssojira5.2-tutorial\tutorial_general_100.png

[200]: .\media\active-directory-saas-msaadssojira5.2-tutorial\tutorial_general_200.png
[201]: .\media\active-directory-saas-msaadssojira5.2-tutorial\tutorial_general_201.png
[202]: .\media\active-directory-saas-msaadssojira5.2-tutorial\tutorial_general_202.png
[203]: .\media\active-directory-saas-msaadssojira5.2-tutorial\tutorial_general_203.png

