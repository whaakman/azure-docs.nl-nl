---
title: 'Zelfstudie: Azure Active Directory-integratie met SuccessFactors | Microsoft Docs'
description: Meer informatie over het gebruik van SuccessFactors met Azure Active Directory voor het inschakelen van eenmalige aanmelding, geautomatiseerde inrichting en meer!
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 32bd8898-c2d2-4aa7-8c46-f1f5c2aa05f1
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/21/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: e85a38ccbe25263ac42bc76351416b023fb77c87
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-successfactors"></a>Zelfstudie: Azure Active Directory-integratie met SuccessFactors
Er is het doel van deze zelfstudie leert u SuccessFactors integreren met Azure Active Directory (Azure AD).

SuccessFactors integreren met Azure AD biedt de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot SuccessFactors heeft
* U kunt uw gebruikers automatisch ophalen aangemeld bij SuccessFactors (Single Sign-On) met hun Azure AD-accounts inschakelen
* U kunt uw accounts op één centrale locatie - en de klassieke Azure portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten
Voor het configureren van Azure AD-integratie met SuccessFactors, moet u de volgende items:

* Een geldige Azure-abonnement
* Een tenant in SuccessFactors

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.
> 
> 

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

* U moet uw productieomgeving niet gebruiken tenzij dit noodzakelijk is.
* Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
Het doel van deze zelfstudie is zodat u kunt eenmalige aanmelding Azure AD te testen in een testomgeving.

Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. SuccessFactors uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-successfactors-from-the-gallery"></a>SuccessFactors uit de galerie toevoegen
Voor het configureren van de integratie van SuccessFactors in Azure AD, moet u SuccessFactors uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen SuccessFactors uit de galerie, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke Azure-portal op het linkernavigatievenster op **Active Directory**.
   
    ![Eenmalige aanmelding configureren][1]
2. Van de **Directory** , selecteert u de map waarvoor u wilt inschakelen van Active directory-integratie.
3. De weergave toepassingen in de directoryweergave, klikt u op **toepassingen** in het menu bovenaan.
   
    ![Eenmalige aanmelding configureren][2]
4. Klik op **toevoegen** aan de onderkant van de pagina.
   
    ![Toepassingen][3]
5. Op de **wat wilt u doen** dialoogvenster, klikt u op **toevoegen van een toepassing uit de galerie**.
   
    ![Eenmalige aanmelding configureren][4]
6. In de **zoekvak**, type **SuccessFactors**.
   
    ![Eenmalige aanmelding configureren][5]
7. Selecteer in het deelvenster resultaten **SuccessFactors**, en klik vervolgens op **Complete** de toepassing toevoegen.
   
    ![Eenmalige aanmelding configureren][6]

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van deze sectie is het beschreven hoe u met het configureren en testen eenmalige aanmelding Azure AD met SuccessFactors op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker van het bijbehorende equivalent in SuccessFactors aan een gebruiker in Azure AD is. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in SuccessFactors tot stand worden gebracht.

Deze relatie koppeling wordt ingesteld door het toewijzen van de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in SuccessFactors.

Om te configureren en testen van Azure AD eenmalige aanmelding met SuccessFactors, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker SuccessFactors](#creating-a-successfactors-test-user)**  - SuccessFactors die is gekoppeld aan de Azure AD-representatie van haar van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren
In deze sectie maakt u Azure AD eenmalige aanmelding inschakelen in de klassieke portal en eenmalige aanmelding configureren in uw toepassing SuccessFactors.

**Voor het configureren van Azure AD eenmalige aanmelding met SuccessFactors, moet u de volgende stappen uitvoeren:**

1. In de klassieke Azure-portal op de **SuccessFactors** toepassing Integratiepagina, klikt u op **eenmalige aanmelding configureren** openen de **configureren eenmalige aanmelding** dialoogvenster.
   
    ![Eenmalige aanmelding configureren][7]
2. Op de **hoe wilt u dat gebruikers zich aanmelden op SuccessFactors** pagina **Microsoft Azure AD Single Sign-On**, en klik vervolgens op **volgende**.
   
    ![Eenmalige aanmelding configureren][8]
3. Op de **App-URL configureren** pagina, voer de volgende stappen uit en klik vervolgens op **volgende**.
   
    ![Eenmalige aanmelding configureren][9]
   
    a. In de **aanmelding op URL** textbox, typ een URL met een van de volgende patronen: 
   
    |  |
    | --- |
    | `https://<company name>.successfactors.com/<company name>` |
    | `https://<company name>.sapsf.com/<company name>` |
    | `https://<company name>.successfactors.eu/<company name>` |
    | `https://<company name>.sapsf.eu` |
   
    b. In de **antwoord-URL** textbox, typ een URL met een van de volgende patronen: 
   
    |  |
    | --- |
    | `https://<company name>.successfactors.com/<company name>` |
    | `https://<company name>.sapsf.com/<company name>` |
    | `https://<company name>.successfactors.eu/<company name>` |
    | `https://<company name>.sapsf.eu` |
    | `https://<company name>.sapsf.eu/<company name>` |
   
    c. Klik op **Volgende**. 

    > [!NOTE]
    > Houd er rekening mee dat deze niet de werkelijke waarden zijn. U hebt deze waarden bijwerken met de werkelijke aanmelding op de URL en antwoord-URL. Als u deze waarden, neem contact op met [SuccessFactors ondersteuningsteam](https://www.successfactors.com/en_us/support.html).

1. Op de **eenmalige aanmelding configureren op SuccessFactors** pagina, klikt u op **certificaat downloaden**, en sla het certificaatbestand lokaal op uw computer.
   
    ![Eenmalige aanmelding configureren][10]

2. In een ander browservenster, meld u aan bij uw **SuccessFactors-beheerportal** als beheerder.

3. Ga naar **Toepassingsbeveiliging** en systeemeigen naar **eenmalige aanmelding op functie**. 

4. Plaats van elke waarde in de **Token opnieuw** en klik op **Token opslaan** SAML SSO inschakelen.
   
    ![Configureren van eenmalige aanmelding op app aan clientzijde][11]

    > [!NOTE] 
    > Deze waarde wordt alleen gebruikt als de schakeloptie/verbergen. Als een willekeurige waarde wordt opgeslagen, is het SAML SSO ingeschakeld. Als een lege waarde wordt opgeslagen is SAML SSO uitgeschakeld.

1. Systeemeigen naar onderstaande schermafbeelding en de volgende acties uitvoeren.
   
    ![Configureren van eenmalige aanmelding op app aan clientzijde][12]
   
    a. Selecteer de **SAML v2 SSO** keuzerondje
   
    b. De SAML aantonen partij Name(e.g. SAml issuer + company name) ingesteld.
   
    c. In de **SAML verlener** textbox plaatsen de waarde van **URL-verlener** van de configuratiewizard voor Azure AD-toepassing.
   
    d. Selecteer **(klant gegenereerd/IdP/AP) antwoord** als **verplichte handtekening vereisen**.
   
    e. Selecteer **ingeschakeld** als **inschakelen SAML vlag**.
   
    f. Selecteer **Nee** als **aanmelding Aanvraaghandtekening (SF gegenereerd/SP/RP)**.
   
    g. Selecteer **Browser/Post profiel** als **SAML profiel**.
   
    h. Selecteer **Nee** als **geldigheidsduur van certificaat afdwingen**.
   
    ik. Kopieer de inhoud van het gedownloade certificaatbestand en plakt u deze in de **SAML verifiëren certificaat** textbox.

    > [!NOTE] 
    > De inhoud van certificaten moet certificaat en certificaat afsluitcodes hebben beginnen.

1. Navigeer naar de SAML-V2 en voer de volgende stappen uit:
   
    ![Configureren van eenmalige aanmelding op app aan clientzijde][13]
   
    a. Selecteer **Ja** als **ondersteuning voor globale afmelding Serviceprovider geïnitieerde**.
   
    b. In de **globale afmelding Service-URL (LogoutRequest doel)** textbox plaatsen de waarde van **externe URL voor afmelden** van de configuratiewizard voor Azure AD-toepassing.
   
    c. Selecteer **Nee** als **vereisen sp alle NameID element moet versleutelen**.
   
    d. Selecteer **niet nader omschreven** als **NameID indeling**.
   
    e. Selecteer **Ja** als **inschakelen geïnitieerd sp aanmelding (AuthnRequest)**.
   
    f. In de **Verzendaanvraag als uitgever van het hele bedrijf** textbox plaatsen de waarde van **externe aanmeldings-URL** van de configuratiewizard voor Azure AD-toepassing.
2. Voer deze stappen uit als u wilt maken van de aanmelding gebruikersnamen niet hoofdlettergevoelig.
   
    a. Ga naar **Bedrijfsinstellingen**(in de buurt van de onderkant).
   
    b. Schakel dit selectievakje in bij **inschakelen niet hoofdlettergevoelig gebruikersnaam**.
   
    c.Click **opslaan**.
   
    ![Eenmalige aanmelding configureren][29]

    > [!NOTE] 
    > Als u probeert deze, controleert het systeem als een dubbele naam van de SAML-aanmelding wordt gemaakt. Bijvoorbeeld als de klant heeft gebruikersnamen Gebruiker1 als Gebruiker1. Blijf de hoofdlettergevoeligheid, kunt u deze dubbele vermeldingen. Het systeem, krijgt u een foutbericht weergegeven en wordt de functie niet inschakelen. De klant moet een van de gebruikersnamen wijzigen zodat gespeld daadwerkelijk verschillende. 

1. Bij de klassieke Azure portal, selecteert u de configuratie voor één aanmelding bevestiging en klik op **Complete** sluiten de **configureren eenmalige aanmelding** dialoogvenster.
   
    ![Toepassingen][14]
2. Op de **eenmalige aanmelding bevestiging** pagina, klikt u op **Complete**.
   
    ![Toepassingen][15]

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de klassieke portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][16]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **klassieke Azure-Portal**, klik op het navigatiedeelvenster links **Active Directory**.
   
    ![Een Azure AD-testgebruiker maken][17]
2. Van de **Directory** , selecteert u de map waarvoor u wilt inschakelen van Active directory-integratie.
3. De lijst met gebruikers, in het menu bovenaan, klikt u op **gebruikers**.
   
    ![Een Azure AD-testgebruiker maken][18]
4. Openen van de **gebruiker toevoegen** dialoogvenster op de werkbalk aan de onderkant, klikt u op **gebruiker toevoegen**.
   
    ![Een Azure AD-testgebruiker maken][19]
5. Op de **Vertel ons meer over deze gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
   
    ![Een Azure AD-testgebruiker maken][20]
   
    a. Selecteer de nieuwe gebruiker in uw organisatie als Type gebruiker.
   
    b. De gebruikersnaam **textbox**, type **BrittaSimon**.
   
    c. Klik op **Volgende**.
6. Op de **gebruikersprofiel** dialoogvenster pagina, voert u de volgende stappen uit:
   
    ![Een Azure AD-testgebruiker maken][21]
   
    a. In de **voornaam** textbox type **Britta**.  
   
    b. In de **achternaam** textbox type, **Simon**.
   
    c. In de **weergavenaam** textbox type **Britta Simon**.
   
    d. In de **rol** selecteert **gebruiker**.
   
    e. Klik op **Volgende**.
7. Op de **tijdelijk wachtwoord** dialoogvenster pagina, klikt u op **maken**.
   
    ![Een Azure AD-testgebruiker maken][22]
8. Op de **tijdelijk wachtwoord** dialoogvenster pagina, voert u de volgende stappen uit:
   
    ![Een Azure AD-testgebruiker maken][23]
   
    a. Noteer de waarde van de **nieuw wachtwoord**.
   
    b. Klik op **Voltooien**.  

### <a name="creating-a-successfactors-test-user"></a>Een testgebruiker SuccessFactors maken
Om in te schakelen gebruikers van Azure AD aan te melden bij SuccessFactors, moeten ze worden ingericht in SuccessFactors.  
In het geval van SuccessFactors is inrichting een handmatige taak.

Als u gebruikers in SuccessFactors hebt gemaakt, moet u contact opnemen met de [SuccessFactors ondersteuningsteam](https://www.successfactors.com/en_us/support.html).

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD
Het doel van deze sectie is het Britta Simon Azure eenmalige aanmelding gebruiken door haar toegang verlenen aan SuccessFactors inschakelen.

![Gebruiker toewijzen][24]

**Britta Simon om aan te wijzen SuccessFactors, moet u de volgende stappen uitvoeren:**

1. In de klassieke portal, de weergave toepassingen in de directoryweergave, klikt u op **toepassingen** in het menu bovenaan.
   
    ![Gebruiker toewijzen][25]
2. Selecteer in de lijst met toepassingen **SuccessFactors**.
   
    ![Eenmalige aanmelding configureren][26]
3. Klik in het menu bovenaan op **gebruikers**.
   
    ![Gebruiker toewijzen][27]
4. Selecteer in de lijst gebruikers **Britta Simon**.
5. Klik in de werkbalk aan de onderkant op **toewijzen**.
   
    ![Gebruiker toewijzen][28]

### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding
Het doel van deze sectie is het testen van uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Als u op de tegel SuccessFactors in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing SuccessFactors.

## <a name="additional-resources"></a>Aanvullende bronnen
* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[0]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_00.png
[1]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_01.png
[6]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_02.png
[7]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_03.png
[8]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_04.png
[9]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_05.png
[10]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_06.png

[11]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_07.png
[12]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_08.png
[13]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_09.png
[14]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_05.png
[15]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_06.png

[16]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_00.png
[17]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_01.png
[18]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_02.png
[19]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_03.png
[20]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_04.png
[21]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_05.png
[22]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_06.png
[23]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_07.png

[24]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_07.png
[25]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_08.png
[26]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_11.png
[27]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_09.png
[28]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_10.png
[29]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_10.png
