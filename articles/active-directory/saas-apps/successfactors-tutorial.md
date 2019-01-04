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
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2018
ms.author: jeedes
ms.openlocfilehash: 467db4046c0600142338dcfa39e136f45255caba
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2019
ms.locfileid: "53976746"
---
# <a name="tutorial-azure-active-directory-integration-with-successfactors"></a>Zelfstudie: Azure Active Directory-integratie met SuccessFactors

In deze zelfstudie leert u hoe u SuccessFactors integreren met Azure Active Directory (Azure AD).

SuccessFactors integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot SuccessFactors heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij SuccessFactors (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met SuccessFactors, moet u de volgende items:

- Een Azure AD-abonnement
- Een SuccessFactors eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. SuccessFactors uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-successfactors-from-the-gallery"></a>SuccessFactors uit de galerie toe te voegen

Voor het configureren van de integratie van SuccessFactors in Azure AD, moet u SuccessFactors uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen SuccessFactors uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

4. Typ in het zoekvak **SuccessFactors**, selecteer **SuccessFactors** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![SuccessFactors in de lijst met resultaten](./media/successfactors-tutorial/tutorial_successfactors_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met SuccessFactors op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in SuccessFactors is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in SuccessFactors tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met SuccessFactors, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een testgebruiker SuccessFactors](#creating-a-successfactors-test-user)**  : als u wilt een equivalent van Britta Simon in SuccessFactors die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing SuccessFactors.

**Voor het configureren van Azure AD eenmalige aanmelding met SuccessFactors, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **SuccessFactors** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

2. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster, klikt u op **Selecteer** voor **SAML** modus voor eenmalige aanmelding inschakelen.

    ![Eenmalige aanmelding configureren](common/tutorial_general_301.png)

3. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** pictogram openen **SAML-basisconfiguratie** dialoogvenster.

    ![Eenmalige aanmelding configureren](common/editconfigure.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![SuccessFactors domein en URL's, eenmalige aanmelding informatie](./media/successfactors-tutorial/tutorial_successfactors_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon:
    | |
    |--|
    | `https://<companyname>.successfactors.com/<companyname>`|
    | `https://<companyname>.sapsf.com/<companyname>`|
    | `https://<companyname>.successfactors.eu/<companyname>`|
    | `https://<companyname>.sapsf.eu`|

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon:
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

    c. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon:
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
    > Dit zijn geen echte waarden. Werk deze waarden met de werkelijke aanmeldings-URL, id en antwoord-URL. Neem contact op met [SuccessFactors Client ondersteuningsteam](https://www.successfactors.com/content/ssf-site/en/support.html) om deze waarden te verkrijgen. 

5. Op de **SAML-handtekeningcertificaat** pagina, in de **SAML-handtekeningcertificaat** sectie, klikt u op **downloaden** downloaden **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![De link om het certificaat te downloaden](./media/successfactors-tutorial/tutorial_successfactors_certificate.png) 

6. Op de **SuccessFactors instellen** sectie, kopieert u de juiste URL aan de hand van uw behoeften.

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

    ![SuccessFactors configuratie](common/configuresection.png)

7. In een ander browservenster aanmelden bij uw **SuccessFactors-beheerportal** als beheerder.
    
8. Ga naar **Toepassingsbeveiliging** en systeemeigen **eenmalige aanmelding op functie**. 

9. Plaats elke waarde in de **Token opnieuw** en klikt u op **Token opslaan** SAML SSO inschakelen.
   
    ![Configureren van eenmalige aanmelding aan van de app][11]

    > [!NOTE] 
    > Deze waarde wordt gebruikt als de switch in-of uitschakelen. Als een willekeurige waarde wordt opgeslagen, is de SAML SSO ingeschakeld. Als een lege waarde wordt opgeslagen is de SAML SSO uitgeschakeld.

10. Ingebouwd in de schermafbeelding hieronder en de volgende acties uitvoeren:
   
    ![Configureren van eenmalige aanmelding aan van de app][12]
   
    a. Selecteer de **SAML v2 SSO** keuzerondje
   
    b. Stel de **SAML-naam aantonen partij**(bijvoorbeeld SAML verlener + bedrijfsnaam).
   
    c. In de **URL-verlener** tekstvak, plak de **Azure AD-id** waarde die u hebt gekopieerd vanuit Azure portal.
   
    d. Selecteer **Assertion** als **vereisen dat de verplichte handtekening**.
   
    e. Selecteer **ingeschakeld** als **SAML-vlag inschakelen**.
   
    f. Selecteer **Nee** als **aanmelding van Aanvraaghandtekening (SF gegenereerd/SP/RP)**.
   
    g. Selecteer **Browser/Post profiel** als **SAML-profiel**.
   
    h. Selecteer **Nee** als **afdwingen geldigheidsduur van certificaat**.
   
    i. De inhoud van het gedownloade bestand kopiëren vanuit Azure portal en plak deze in de **SAML-certificaat controleren** tekstvak.

    > [!NOTE] 
    > De certificaatinhoud moet hebben begincodes certificaat- en einddatum certificaat.

11. Navigeer naar de SAML-V2 en voer de volgende stappen uit:
   
    ![Configureren van eenmalige aanmelding aan van de app][13]
   
    a. Selecteer **Ja** als **ondersteuning voor algemene afmelden SP geïnitieerde**.
   
    b. In de **globale afmelden Service-URL (LogoutRequest doel)** tekstvak, plak de **afmelding URL** waarde die u hebt gekopieerd, vormen de Azure-portal.
   
    c. Selecteer **Nee** als **vereisen sp alle NameID-element moet versleutelen**.
   
    d. Selecteer **niet nader omschreven** als **NameID-indeling**.
   
    e. Selecteer **Ja** als **sp geïnitieerde aanmelding (AuthnRequest) inschakelen**.
   
    f. In de **Verzendaanvraag als bedrijfsbrede verlener** tekstvak plakken **Single Sign-On Service URL voor SAML** waarde die u hebt gekopieerd vanuit Azure portal.

12. Deze stappen uitvoert als u wilt maken van de gebruikersnamen van de aanmelding is hoofdlettergevoelig.
   
    ![Eenmalige aanmelding configureren][29]
    
    a. Ga naar **Bedrijfsinstellingen**(in de buurt van de onderkant).
   
    b. Schakel het selectievakje in de buurt **inschakelen niet-hoofdlettergevoelig gebruikersnaam**.
   
    c.Click **opslaan**.
   
    > [!NOTE] 
    > Als u probeert deze, controleert het systeem als er wordt een dubbele naam van de SAML-aanmelding gemaakt. Bijvoorbeeld als de klant heeft gebruikersnamen Gebruiker1 als Gebruiker1. Blijf de hoofdlettergevoeligheid, kunt u deze dubbele vermeldingen. Het systeem biedt u een foutbericht weergegeven en wordt de functie niet inschakelen. De klant moet een van de gebruikersnamen wijzigen zodat deze verschillende gespeld.

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![Azure AD-gebruiker maken][100]

2. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm.

    ![Het maken van een Azure AD-testgebruiker](common/create_aaduser_01.png) 

3. In de eigenschappen van de gebruiker de volgende stappen uitvoeren.

    ![Het maken van een Azure AD-testgebruiker](common/create_aaduser_02.png)

    a. Voer in het veld **Naam****Britta Simon** in.
  
    b. In het veld **Gebruikersnaam** typt u **brittasimon@yourcompanydomain.extension**.  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Selecteer **eigenschappen**, selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in het wachtwoord.

    d. Selecteer **Maken**.

### <a name="creating-a-successfactors-test-user"></a>Het maken van een testgebruiker SuccessFactors

Als u wilt dat Azure AD-gebruikers zich aanmelden bij SuccessFactors, moeten ze worden ingericht voor SuccessFactors.  
In het geval van SuccessFactors is inrichten een handmatige taak.

Als u gebruikers in SuccessFactors hebt gemaakt, moet u contact opnemen met de [SuccessFactors ondersteuningsteam](https://www.successfactors.com/content/ssf-site/en/support.html).

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan SuccessFactors.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **SuccessFactors**.

    ![Eenmalige aanmelding configureren](./media/successfactors-tutorial/tutorial_successfactors_app.png)

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. In de **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst met gebruikers, en klik op de **Selecteer** knop aan de onderkant van het scherm.

6. In de **toevoegen toewijzing** dialoogvenster Selecteer de **toewijzen** knop.

### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel SuccessFactors in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing SuccessFactors.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

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
[11]: ./media/successfactors-tutorial/tutorial_successfactors_07.png
[12]: ./media/successfactors-tutorial/tutorial_successfactors_08.png
[13]: ./media/successfactors-tutorial/tutorial_successfactors_09.png
[29]: ./media/successfactors-tutorial/tutorial_successfactors_10.png
