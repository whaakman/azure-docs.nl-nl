---
title: 'Zelfstudie: Azure Active Directory-integratie met SuccessFactors | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en SuccessFactors.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 32bd8898-c2d2-4aa7-8c46-f1f5c2aa05f1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: jeedes
ms.openlocfilehash: b9545599b4ac02927c38931777a3cee623a4e940
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="tutorial-azure-active-directory-integration-with-successfactors"></a>Zelfstudie: Azure Active Directory-integratie met SuccessFactors

In deze zelfstudie leert u hoe SuccessFactors integreren met Azure Active Directory (Azure AD).

SuccessFactors integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot SuccessFactors heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij SuccessFactors (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met SuccessFactors, moet u de volgende items:

- Een Azure AD-abonnement
- Een SuccessFactors eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. SuccessFactors uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-successfactors-from-the-gallery"></a>SuccessFactors uit de galerie toevoegen
Voor het configureren van de integratie van SuccessFactors in Azure AD, moet u SuccessFactors uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen SuccessFactors uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **SuccessFactors**, selecteer **SuccessFactors** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![SuccessFactors in de lijst met resultaten](./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met SuccessFactors op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in SuccessFactors is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in SuccessFactors tot stand worden gebracht.

Wijs in SuccessFactors, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met SuccessFactors, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker SuccessFactors](#create-a-successfactors-test-user)**  - SuccessFactors die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing SuccessFactors configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met SuccessFactors, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **SuccessFactors** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_samlbase.png)

3. Op de **SuccessFactors domein en de URL's** sectie, voert u de volgende stappen uit:

    ![URL's en SuccessFactors domein eenmalige aanmelding informatie](./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:
    | |
    |--|
    | `https://<companyname>.successfactors.com/<companyname>`|
    | `https://<companyname>.sapsf.com/<companyname>`|
    | `https://<companyname>.successfactors.eu/<companyname>`|
    | `https://<companyname>.sapsf.eu`|

    b. In de **id** textbox, typ een URL met het volgende patroon volgen:
    | |
    |--|
    | `https://www.successfactors.com/<companyname>`|
    | `https://www.successfactors.com`|
    | `https://<companyname>.successfactors.eu`|
    | `https://www.successfactors.eu/<companyname>`|
    | `https://<companyname>.sapsf.com`|
    | `https://hcm4preview.sapsf.com/<companyname>`|
    | `https://<companyname>.sapsf.eu`|
    | `https://www.successfactors.cn`|
    | `https://www.successfactors.cn/<companyname>`|

    c. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:
    | |
    |--|
    | `https://<companyname>.successfactors.com/<companyname>`|
    | `https://<companyname>.successfactors.com`|
    | `https://<companyname>.sapsf.com/<companyname>`|
    | `https://<companyname>.sapsf.com`|
    | `https://<companyname>.successfactors.eu/<companyname>`|
    | `https://<companyname>.successfactors.eu`|
    | `https://<companyname>.sapsf.eu`|
    | `https://<companyname>.sapsf.eu/<companyname>`|
    | `https://<companyname>.sapsf.cn`|
    | `https://<companyname>.sapsf.cn/<companyname>`|
         
    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke id, antwoord-URL en aanmeldings-URL. Neem contact op met [SuccessFactors Client ondersteuningsteam](https://www.successfactors.com/en_us/support.html) ophalen van deze waarden. 

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Certificate(Base64)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_certificate.png) 

5. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-successfactors-tutorial/tutorial_general_400.png)
    
6. Op de **SuccessFactors configuratie** sectie, klikt u op **configureren SuccessFactors** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_configure.png) 

7. In een ander browservenster geopend, moet u zich aanmelden bij uw **SuccessFactors-beheerportal** als beheerder.
    
8. Ga naar **Toepassingsbeveiliging** en systeemeigen naar **eenmalige aanmelding op functie**. 

9. Plaats van elke waarde in de **Token opnieuw** en klik op **Token opslaan** SAML SSO inschakelen.
   
    ![Configureren van eenmalige aanmelding op app aan clientzijde][11]

    > [!NOTE] 
    > Deze waarde wordt gebruikt als de schakeloptie/verbergen. Als een willekeurige waarde wordt opgeslagen, is het SAML SSO ingeschakeld. Als een lege waarde wordt opgeslagen is SAML SSO uitgeschakeld.

10. Systeemeigen naar onderstaande schermafbeelding en de volgende acties uitvoeren:
   
    ![Configureren van eenmalige aanmelding op app aan clientzijde][12]
   
    a. Selecteer de **SAML v2 SSO** keuzerondje
   
    b. Stel de **SAML-naam aantonen partij**(bijvoorbeeld SAML verlener + bedrijfsnaam).
   
    c. In de **URL-verlener** textbox, plak de **SAML entiteit-ID** waarde die u hebt gekopieerd uit de Azure portal.
   
    d. Selecteer **(klant gegenereerd/IdP/AP) antwoord** als **verplichte handtekening vereisen**.
   
    e. Selecteer **ingeschakeld** als **inschakelen SAML vlag**.
   
    f. Selecteer **Nee** als **aanmelding Aanvraaghandtekening (SF gegenereerd/SP/RP)**.
   
    g. Selecteer **Browser/Post profiel** als **SAML profiel**.
   
    h. Selecteer **Nee** als **geldigheidsduur van certificaat afdwingen**.
   
    ik. Kopieer de inhoud van het gedownloade certificaatbestand van Azure-portal en plakt u deze in de **SAML verifiëren certificaat** textbox.

    > [!NOTE] 
    > De inhoud van certificaten moet certificaat en certificaat afsluitcodes hebben beginnen.

11. Navigeer naar de SAML-V2 en voer de volgende stappen uit:
   
    ![Configureren van eenmalige aanmelding op app aan clientzijde][13]
   
    a. Selecteer **Ja** als **ondersteuning voor globale afmelding Serviceprovider geïnitieerde**.
   
    b. In de **globale afmelding Service-URL (LogoutRequest doel)** textbox, plak de **Sign-Out URL** waarde die u hebt gekopieerd, vormen de Azure-portal.
   
    c. Selecteer **Nee** als **vereisen sp alle NameID element moet versleutelen**.
   
    d. Selecteer **niet nader omschreven** als **NameID indeling**.
   
    e. Selecteer **Ja** als **inschakelen geïnitieerd sp aanmelding (AuthnRequest)**.
   
    f. In de **Verzendaanvraag als uitgever van het hele bedrijf** textbox plakken **SAML Single Sign-On Service-URL** waarde die u hebt gekopieerd uit de Azure portal.

12. Voer deze stappen uit als u wilt maken van de aanmelding gebruikersnamen niet hoofdlettergevoelig.
   
    ![Eenmalige aanmelding configureren][29]
    
    a. Ga naar **Bedrijfsinstellingen**(in de buurt van de onderkant).
   
    b. Schakel dit selectievakje in bij **inschakelen niet hoofdlettergevoelig gebruikersnaam**.
   
    c.Click **opslaan**.
   
    > [!NOTE] 
    > Als u probeert deze, controleert het systeem als maakt het een dubbele SAML-aanmeldingsnaam. Bijvoorbeeld als de klant heeft gebruikersnamen Gebruiker1 als Gebruiker1. Blijf de hoofdlettergevoeligheid, kunt u deze dubbele vermeldingen. Het systeem krijgt u een foutbericht weergegeven en kan de functie niet. De klant moet een van de gebruikersnamen wijzigen zodat deze verschillende gespeld.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-successfactors-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-successfactors-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-successfactors-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-successfactors-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-successfactors-test-user"></a>Een testgebruiker SuccessFactors maken

Om Azure AD-gebruikers zich aanmelden bij SuccessFactors, moeten ze worden ingericht in SuccessFactors.  
In het geval van SuccessFactors is inrichting een handmatige taak.

Als u gebruikers in SuccessFactors hebt gemaakt, moet u contact opnemen met de [SuccessFactors ondersteuningsteam](https://www.successfactors.com/en_us/support.html).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan SuccessFactors.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen SuccessFactors, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **SuccessFactors**.

    ![De koppeling SuccessFactors in de lijst met toepassingen](./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel SuccessFactors in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing SuccessFactors.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_04.png

[11]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_07.png
[12]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_08.png
[13]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_09.png
[14]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_05.png
[15]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_06.png
[29]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_10.png

[100]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_203.png

