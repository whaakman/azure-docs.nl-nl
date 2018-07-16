---
title: 'Zelfstudie: Azure Active Directory-integratie met JIRA SAML SSO door Microsoft | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en JIRA SAML SSO door Microsoft.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4b663047-7f88-443b-97bd-54224b232815
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2018
ms.author: jeedes
ms.openlocfilehash: 92b505e698f2d56a2678ac53f42bb49dbaf22463
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39046767"
---
# <a name="tutorial-azure-active-directory-integration-with-jira-saml-sso-by-microsoft"></a>Zelfstudie: Azure Active Directory-integratie met JIRA SAML SSO door Microsoft

In deze zelfstudie leert u hoe u JIRA SAML SSO door Microsoft integreren met Azure Active Directory (Azure AD).

JIRA SAML SSO door Microsoft integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot JIRA SAML SSO door Microsoft heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij JIRA SAML SSO door Microsoft (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="description"></a>Beschrijving

Uw Microsoft Azure Active Directory-account met Atlassian JIRA-server gebruiken voor eenmalige aanmelding inschakelen. Op deze manier alle gebruikers in uw organisatie kunnen de Azure AD-referenties aan te melden bij de JIRA-toepassing gebruiken. Deze invoegtoepassing maakt gebruik van SAML 2.0 voor Federatie.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met JIRA SAML SSO door Microsoft, moet u de volgende items:

- Een Azure AD-abonnement
- JIRA-Core- en Software 6.0 7.8 of JIRA Service Desk 3.0 3.2 moeten geïnstalleerd en geconfigureerd op Windows 64-bits versie
- JIRA-server is HTTPS-functionaliteit
- Houd er rekening mee dat de ondersteunde versies voor JIRA-invoegtoepassing worden vermeld in onderstaande sectie.
- JIRA-server bereikbaar is op internet met name naar Azure AD-aanmeldingspagina voor verificatie en moet kunnen ontvangen van het token uit Azure AD
- Referenties van een beheerder worden ingesteld in JIRA
- WebSudo is uitgeschakeld in JIRA
- Testgebruiker gemaakt in de JIRA-servertoepassing

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving van JIRA. De integratie in ontwikkelings- of faseringsomgeving van de toepassing eerst te testen en gebruik vervolgens de productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u een proefversie van één maand hier downloaden: [proefversie aanbieding](https://azure.microsoft.com/pricing/free-trial/).

## <a name="supported-versions-of-jira"></a>Ondersteunde versies van JIRA

*   JIRA-Core- en Software: 6.0-7.8
*   JIRA-servicedesk 3.0 en 3.2
*   JIRA biedt ook ondersteuning voor 5.2. Voor meer informatie klikt u op [Microsoft Azure Active Directory eenmalige aanmelding voor JIRA 5.2](jira52microsoft-tutorial.md)

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving.
Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. JIRA SAML SSO door Microsoft uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-jira-saml-sso-by-microsoft-from-the-gallery"></a>JIRA SAML SSO door Microsoft uit de galerie toe te voegen
Voor het configureren van de integratie van JIRA SAML SSO door Microsoft in Azure AD, moet u JIRA SAML SSO door Microsoft uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen JIRA SAML SSO door Microsoft uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **JIRA SAML SSO door Microsoft**, selecteer **JIRA SAML SSO door Microsoft** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![JIRA SAML SSO door Microsoft in de lijst met resultaten](./media/jiramicrosoft-tutorial/tutorial_singlesign-onforjira_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met JIRA SAML SSO door Microsoft op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in de SAML SSO-JIRA door Microsoft is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in JIRA SAML SSO door Microsoft tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met JIRA SAML SSO door Microsoft, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Een JIRA SAML SSO door Microsoft testgebruiker maken](#create-a-jira-saml-sso-by-microsoft-test-user)**  : als u wilt een equivalent van Britta Simon in JIRA SAML SSO door Microsoft en die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw JIRA SAML SSO door toepassing van Microsoft.

**Voor het configureren van Azure AD eenmalige aanmelding met JIRA SAML SSO door Microsoft, kunt u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **JIRA SAML SSO door Microsoft** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![In het dialoogvenster voor eenmalige aanmelding](./media/jiramicrosoft-tutorial/tutorial_singlesign-onforjira_samlbase.png)

3. Op de **JIRA SAML SSO door Microsoft Domain en URL's** sectie, voert u de volgende stappen uit:

    ![JIRA SAML SSO door Microsoft Domain en URL's één aanmeldings-informatie](./media/jiramicrosoft-tutorial/tutorial_singlesign-onforjira_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<domain:port>/plugins/servlet/saml/auth`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<domain:port>/`

    c. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<domain:port>/plugins/servlet/saml/auth`

    > [!NOTE]
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke-id, de antwoord-URL en aanmeldings-URL. Poort is optioneel als een URL met de naam is. Deze waarden zijn ontvangen tijdens de configuratie van Jira-invoegtoepassing, die later in de zelfstudie wordt uitgelegd.

4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op de knop kopiëren om te kopiëren **App-Url voor federatieve metagegevens** en plak deze in Kladblok.

    ![Eenmalige aanmelding configureren](./media/jiramicrosoft-tutorial/tutorial_metadataurl.png)

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/jiramicrosoft-tutorial/tutorial_general_400.png)

6. In een ander browservenster aanmelden bij uw JIRA exemplaar als beheerder.

7. Beweeg de muisaanwijzer op het tandwiel en klikt u op de **invoegtoepassingen**.

    ![Eenmalige aanmelding configureren](./media/jiramicrosoft-tutorial/addon1.png)

8. Download de invoegtoepassing van [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=56506). Handmatig uploaden van de invoegtoepassing die is opgegeven met behulp van Microsoft **invoegtoepassing uploaden** menu. Het downloaden van de invoegtoepassing wordt gedekt onder [Microsoft-serviceovereenkomst](https://www.microsoft.com/en-us/servicesagreement/).

    ![Eenmalige aanmelding configureren](./media/jiramicrosoft-tutorial/addon12.png)

9. Zodra de invoegtoepassing is geïnstalleerd, wordt deze weergegeven **gebruiker geïnstalleerd** invoegtoepassingen-sectie van **beheren invoegtoepassing** sectie. Klik op **configureren** het configureren van de nieuwe-invoegtoepassing.

    ![Eenmalige aanmelding configureren](./media/jiramicrosoft-tutorial/addon13.png)

10. Voert u de volgende stappen uit op de configuratiepagina:

    ![Eenmalige aanmelding configureren](./media/jiramicrosoft-tutorial/addon52.png)

    > [!TIP]
    > Zorg ervoor dat er slechts één certificaat toegewezen op basis van de app, zodat er geen fout is opgetreden bij het oplossen van de metagegevens. Als er meerdere certificaten, bij het oplossen van de metagegevens krijgt-beheerder een fout.

    a. In de **metagegevens-URL** tekstvak plakken **App-Url voor federatieve metagegevens** waarde die u hebt gekopieerd uit de Azure portal en klik op de **oplossen** knop. Leest de IdP metagegevens-URL en wordt alle informatie van de velden ingevuld.

    b. Kopiëren de **-id, de antwoord-URL en de aanmeldings-URL** waarden en plak deze in **-id, de antwoord-URL en de aanmeldings-URL** respectievelijk in tekstvakken **JIRA SAML SSO door Microsoft Domain en URL's** sectie in Azure portal.

    c. In **aanmeldingsnaam van de knop** Typ de naam van uw organisatie wil dat de gebruikers om te zien op het aanmeldingsscherm knop.

    d. In **SAML gebruiker-ID locaties** Selecteer **gebruikers-ID is in de NameIdentifier-element van het onderwerp overzicht** of **gebruikers-ID is in een kenmerkelement**.  Deze ID is de JIRA gebruikers-id. Als de gebruikers-id niet overeen komt, klikt u vervolgens systeem niet toestaan dat gebruikers zich aanmelden.

    > [!Note]
    > De standaardlocatie voor de SAML-gebruikers-ID is naam-id. U kunt dit wijzigen in een kenmerk-optie en geef de juiste naam.

    e. Als u selecteert **gebruikers-ID is in een kenmerkelement** optie, klikt u vervolgens in **kenmerknaam** tekstvak typt u de naam van het kenmerk waar gebruikers-Id wordt verwacht.

    f. Als u van de federatief domein (zoals ADFS enz.) met Azure AD gebruikmaakt, klikt u op de **inschakelen start Realm detectie** optie en configureer de **domeinnaam**.

    g. In **domeinnaam** typt u de domeinnaam hier in het geval van de aanmelding op basis van een AD FS.

    h. Controleer **eenmalige aanmelding inschakelen om** als u Meld u wilt af bij Azure AD wanneer een gebruiker zich afmeldt van JIRA.

    i. Klik op **opslaan** knop de instellingen op te slaan.

    > [!NOTE]
    > Voor meer informatie over de installatie en probleemoplossing, gaat u naar [MS JIRA SSO-Connector-beheerdershandleiding](../ms-confluence-jira-plugin-adminguide.md) en er is ook [Veelgestelde vragen over](../ms-confluence-jira-plugin-faq.md) voor uw hulp

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/jiramicrosoft-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/jiramicrosoft-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/jiramicrosoft-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/jiramicrosoft-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.

### <a name="create-a-jira-saml-sso-by-microsoft-test-user"></a>Een JIRA SAML SSO door Microsoft testgebruiker maken

Om Azure AD-gebruikers zich aanmelden met JIRA on-premises server, moeten deze in JIRA SAML SSO worden ingericht door Microsoft. Voor JIRA SAML SSO door Microsoft is het inrichten van een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw JIRA on-premises-server aan als beheerder.

2. Beweeg de muisaanwijzer op het tandwiel en klikt u op de **Gebruikersbeheer**.

    ![Werknemer toevoegen](./media/jiramicrosoft-tutorial/user1.png)

3. U wordt omgeleid naar de pagina beheerderstoegang in te voeren **wachtwoord** en klikt u op **bevestigen** knop.

    ![Werknemer toevoegen](./media/jiramicrosoft-tutorial/user2.png)

4. Onder **Gebruikersbeheer** tabblad sectie, klikt u op **gebruiker maken**.

    ![Werknemer toevoegen](./media/jiramicrosoft-tutorial/user3.png) 

5. Op de **'Een nieuwe gebruiker maken'** dialoogvenster pagina, voert u de volgende stappen uit:

    ![Werknemer toevoegen](./media/jiramicrosoft-tutorial/user4.png) 

    a. In de **e-mailadres** tekstvak, typ het e-mailadres van gebruiker, zoals Brittasimon@contoso.com.

    b. In de **volledige naam** tekstvak, volledige naam van de gebruiker, zoals Britta Simon.

    c. In de **gebruikersnaam** tekstvak, typ het e-mailadres van gebruiker, zoals Brittasimon@contoso.com.

    d. In de **wachtwoord** tekstvak typt u het wachtwoord van gebruiker.

    e. Klik op **gebruiker maken**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan JIRA SAML SSO door Microsoft.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon aan JIRA SAML SSO door Microsoft, kunt u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **JIRA SAML SSO door Microsoft**.

    ![De JIRA SAML SSO door een Microsoft-koppeling in de lijst met toepassingen](./media/jiramicrosoft-tutorial/tutorial_singlesign-onforjira_app.png)

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de JIRA SAML SSO door Microsoft-tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw JIRA SAML SSO door toepassing van Microsoft.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/msaadssojira-tutorial/tutorial_general_01.png
[2]: ./media/msaadssojira-tutorial/tutorial_general_02.png
[3]: ./media/msaadssojira-tutorial/tutorial_general_03.png
[4]: ./media/msaadssojira-tutorial/tutorial_general_04.png

[100]: ./media/msaadssojira-tutorial/tutorial_general_100.png

[200]: ./media/msaadssojira-tutorial\tutorial_general_200.png
[201]: ./media/msaadssojira-tutorial\tutorial_general_201.png
[202]: ./media/msaadssojira-tutorial\tutorial_general_202.png
[203]: ./media/msaadssojira-tutorial\tutorial_general_203.png