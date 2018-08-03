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
ms.date: 12/08/2017
ms.author: jeedes
ms.openlocfilehash: 870a753a8f10255a602616ab54234b295f4d6e13
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39431480"
---
# <a name="tutorial-azure-active-directory-integration-with-successfactors"></a>Zelfstudie: Azure Active Directory-integratie met SuccessFactors

In deze zelfstudie leert u hoe u SuccessFactors integreren met Azure Active Directory (Azure AD).

SuccessFactors integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot SuccessFactors heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij SuccessFactors (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

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
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-successfactors-from-the-gallery"></a>SuccessFactors uit de galerie toe te voegen
Voor het configureren van de integratie van SuccessFactors in Azure AD, moet u SuccessFactors uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen SuccessFactors uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

1. Typ in het zoekvak **SuccessFactors**, selecteer **SuccessFactors** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![SuccessFactors in de lijst met resultaten](./media/successfactors-tutorial/tutorial_successfactors_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met SuccessFactors op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in SuccessFactors is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in SuccessFactors tot stand worden gebracht.

In SuccessFactors, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met SuccessFactors, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Maak een testgebruiker SuccessFactors](#create-a-successfactors-test-user)**  : als u wilt een equivalent van Britta Simon in SuccessFactors die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing SuccessFactors.

**Voor het configureren van Azure AD eenmalige aanmelding met SuccessFactors, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **SuccessFactors** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/successfactors-tutorial/tutorial_successfactors_samlbase.png)

1. Op de **SuccessFactors domein en URL's** sectie, voert u de volgende stappen uit:

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
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke-id, de antwoord-URL en aanmeldings-URL. Neem contact op met [SuccessFactors Client ondersteuningsteam](https://www.successfactors.com/en_us/support.html) om deze waarden te verkrijgen. 

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/successfactors-tutorial/tutorial_successfactors_certificate.png) 

1. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/successfactors-tutorial/tutorial_general_400.png)
    
1. Op de **SuccessFactors configuratie** sectie, klikt u op **configureren SuccessFactors** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/successfactors-tutorial/tutorial_successfactors_configure.png) 

1. In een ander browservenster aanmelden bij uw **SuccessFactors-beheerportal** als beheerder.
    
1. Ga naar **Toepassingsbeveiliging** en systeemeigen **eenmalige aanmelding op functie**. 

1. Plaats elke waarde in de **Token opnieuw** en klikt u op **Token opslaan** SAML SSO inschakelen.
   
    ![Configureren van eenmalige aanmelding aan van de app][11]

    > [!NOTE] 
    > Deze waarde wordt gebruikt als de switch in-of uitschakelen. Als een willekeurige waarde wordt opgeslagen, is de SAML SSO ingeschakeld. Als een lege waarde wordt opgeslagen is de SAML SSO uitgeschakeld.

1. Ingebouwd in de schermafbeelding hieronder en de volgende acties uitvoeren:
   
    ![Configureren van eenmalige aanmelding aan van de app][12]
   
    a. Selecteer de **SAML v2 SSO** keuzerondje
   
    b. Stel de **SAML-naam aantonen partij**(bijvoorbeeld SAML verlener + bedrijfsnaam).
   
    c. In de **URL-verlener** tekstvak, plak de **SAML entiteit-ID** waarde die u hebt gekopieerd vanuit Azure portal.
   
    d. Selecteer **antwoord (klant die zijn gegenereerd/IdP/AP)** als **vereisen dat de verplichte handtekening**.
   
    e. Selecteer **ingeschakeld** als **SAML-vlag inschakelen**.
   
    f. Selecteer **Nee** als **aanmelding van Aanvraaghandtekening (SF gegenereerd/SP/RP)**.
   
    g. Selecteer **Browser/Post profiel** als **SAML-profiel**.
   
    h. Selecteer **Nee** als **afdwingen geldigheidsduur van certificaat**.
   
    i. De inhoud van het gedownloade bestand kopiëren vanuit Azure portal en plak deze in de **SAML-certificaat controleren** tekstvak.

    > [!NOTE] 
    > De certificaatinhoud moet hebben begincodes certificaat- en einddatum certificaat.

1. Navigeer naar de SAML-V2 en voer de volgende stappen uit:
   
    ![Configureren van eenmalige aanmelding aan van de app][13]
   
    a. Selecteer **Ja** als **ondersteuning voor algemene afmelden SP geïnitieerde**.
   
    b. In de **globale afmelden Service-URL (LogoutRequest doel)** tekstvak, plak de **afmelding URL** waarde die u hebt gekopieerd, vormen de Azure-portal.
   
    c. Selecteer **Nee** als **vereisen sp alle NameID-element moet versleutelen**.
   
    d. Selecteer **niet nader omschreven** als **NameID-indeling**.
   
    e. Selecteer **Ja** als **sp geïnitieerde aanmelding (AuthnRequest) inschakelen**.
   
    f. In de **Verzendaanvraag als bedrijfsbrede verlener** tekstvak plakken **Single Sign-On Service URL voor SAML** waarde die u hebt gekopieerd vanuit Azure portal.

1. Deze stappen uitvoert als u wilt maken van de gebruikersnamen van de aanmelding is hoofdlettergevoelig.
   
    ![Eenmalige aanmelding configureren][29]
    
    a. Ga naar **Bedrijfsinstellingen**(in de buurt van de onderkant).
   
    b. Schakel het selectievakje in de buurt **inschakelen niet-hoofdlettergevoelig gebruikersnaam**.
   
    c.Click **opslaan**.
   
    > [!NOTE] 
    > Als u probeert deze, controleert het systeem als er wordt een dubbele naam van de SAML-aanmelding gemaakt. Bijvoorbeeld als de klant heeft gebruikersnamen Gebruiker1 als Gebruiker1. Blijf de hoofdlettergevoeligheid, kunt u deze dubbele vermeldingen. Het systeem biedt u een foutbericht weergegeven en wordt de functie niet inschakelen. De klant moet een van de gebruikersnamen wijzigen zodat deze verschillende gespeld.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/successfactors-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/successfactors-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/successfactors-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/successfactors-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-successfactors-test-user"></a>Maak een testgebruiker SuccessFactors

Als u wilt dat Azure AD-gebruikers zich aanmelden bij SuccessFactors, moeten ze worden ingericht voor SuccessFactors.  
In het geval van SuccessFactors is inrichten een handmatige taak.

Als u gebruikers in SuccessFactors hebt gemaakt, moet u contact opnemen met de [SuccessFactors ondersteuningsteam](https://www.successfactors.com/en_us/support.html).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan SuccessFactors.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan SuccessFactors toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **SuccessFactors**.

    ![De koppeling SuccessFactors in de lijst met toepassingen](./media/successfactors-tutorial/tutorial_successfactors_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel SuccessFactors in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing SuccessFactors.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/successfactors-tutorial/tutorial_general_01.png
[2]: ./media/successfactors-tutorial/tutorial_general_02.png
[3]: ./media/successfactors-tutorial/tutorial_general_03.png
[4]: ./media/successfactors-tutorial/tutorial_general_04.png

[11]: ./media/successfactors-tutorial/tutorial_successfactors_07.png
[12]: ./media/successfactors-tutorial/tutorial_successfactors_08.png
[13]: ./media/successfactors-tutorial/tutorial_successfactors_09.png
[14]: ./media/successfactors-tutorial/tutorial_general_05.png
[15]: ./media/successfactors-tutorial/tutorial_general_06.png
[29]: ./media/successfactors-tutorial/tutorial_successfactors_10.png

[100]: ./media/successfactors-tutorial/tutorial_general_100.png

[200]: ./media/successfactors-tutorial/tutorial_general_200.png
[201]: ./media/successfactors-tutorial/tutorial_general_201.png
[202]: ./media/successfactors-tutorial/tutorial_general_202.png
[203]: ./media/successfactors-tutorial/tutorial_general_203.png

