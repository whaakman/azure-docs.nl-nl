---
title: 'Zelfstudie: Azure Active Directory-integratie met @Task| Microsoft Docs'
description: Meer informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en @Task.
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: aab8bd2f-f9dd-42da-a18e-d707865687d7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: ebb19ca6cbaf04106fbce937d95651e709854cfd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-task"></a>Zelfstudie: Azure Active Directory-integratie met@Task
Het doel van deze zelfstudie is beschreven hoe u met het integreren van @Task met Azure Active Directory (Azure AD).  
Integratie van @Task met Azure AD biedt u de volgende voordelen: 

* U kunt beheren in Azure AD die toegang heeft tot@Task
* U kunt uw gebruikers automatisch aangemeld bij inschakelen @Task (Single Sign-On) met hun Azure AD-accounts
* U kunt uw accounts op één centrale locatie - en de klassieke Azure Portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten
Voor het configureren van Azure AD-integratie met @Task, moet u de volgende items:

* Een Azure AD-abonnement
* Een @Task eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.
> 
> 

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

* U moet uw productieomgeving niet gebruiken tenzij dit noodzakelijk is.
* Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/). 

## <a name="scenario-description"></a>Scenariobeschrijving
Het doel van deze zelfstudie is zodat u kunt eenmalige aanmelding Azure AD te testen in een testomgeving.  
Het scenario in deze zelfstudie bestaat uit drie belangrijkste bouwstenen:

1. Toe te voegen @Task uit de galerie 
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-task-from-the-gallery"></a>Toe te voegen @Task uit de galerie
Voor het configureren van de integratie van @Task in Azure AD, moet u toevoegen @Task uit de galerie aan de lijst met beheerde SaaS-apps.

**Om toe te voegen @Task uit de galerie, de volgende stappen uitvoeren:**

1. In de **klassieke Azure-portal**, klik op het navigatiedeelvenster links **Active Directory**. 
   
    ![Active Directory][1] 
2. Van de **Directory** , selecteert u de map waarvoor u wilt inschakelen van Active directory-integratie.
3. De weergave toepassingen in de directoryweergave, klikt u op **toepassingen** in het menu bovenaan.
   
    ![Toepassingen][2] 
4. Klik op **toevoegen** aan de onderkant van de pagina.
   
    ![Toepassingen][3] 
5. Op de **wat wilt u doen** dialoogvenster, klikt u op **toevoegen van een toepassing uit de galerie**.
   
    ![Toepassingen][4] 
6. Typ in het zoekvak  **@Task** .
   
    ![Toepassingen][5] 
7. Selecteer in het deelvenster met resultaten  **@Task** , en klik vervolgens op **Complete** de toepassing toevoegen.
   
    ![Toepassingen][30] 

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van deze sectie is beschreven hoe u met het configureren en testen Azure AD eenmalige aanmelding met @Task op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, Azure AD moet weten welke gebruiker de bijbehorende equivalent in @Task is aan een gebruiker in Azure AD. Met andere woorden, een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in @Task moet worden vastgesteld.   
Deze relatie koppeling wordt ingesteld door het toewijzen van de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in @Task.

Configureren en testen Azure AD eenmalige aanmelding met @Task, u moet voltooien van de volgende elementen:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een @Tasktest gebruiker](#creating-a-halogen-software-test-user)**  - hebben een equivalent van Britta Simon in @Taskthat is gekoppeld aan de Azure AD-representatie van haar.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding configureren
Het doel van deze sectie is Azure AD eenmalige aanmelding in de klassieke Azure portal inschakelen en configureren van eenmalige aanmelding in uw @Task toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met @Task, voer de volgende stappen uit:**

1. In de klassieke Azure-portal op de  **@Task**  toepassing Integratiepagina, klikt u op **eenmalige aanmelding configureren** openen de **configureren Single Sign-On** dialoogvenster.
   
    ![Eenmalige aanmelding configureren][6] 
2. Op de **hoe wilt u dat gebruikers zich aanmelden bij @Task**  pagina **Azure AD Single Sign-On**, en klik vervolgens op **volgende**.
   
    ![Azure AD voor eenmalige aanmelding][7] 
3. Op de **App-instellingen configureren** dialoogvenster pagina, voert u de volgende stappen uit:
   
    ![App-instellingen configureren][8] 
   
     a. In de **aanmelding op URL** textbox, typ de URL moet worden gebruikt door uw gebruikers aanmelding bij uw @Task toepassing (bijvoorbeeld:*https://<Tenant name>.attask ondemand.com*).
   
     b. Klik op **Volgende**.
4. Op de **configureren eenmalige aanmelding op @Task**  pagina, klikt u op **metagegevens downloaden**, sla het metagegevensbestand lokaal op uw computer en klik vervolgens op **volgende**.
   
    ![Wat is Azure AD Connect?][9] 
5. Aanmelding bij uw @Task bedrijf site als administrator.
6. Ga naar **eenmalige van configuratie**.
7. Op de **Single Sign-On** dialoogvenster de volgende stappen uitvoeren
   
    ![Eenmalige aanmelding configureren][23]
   
    a. Als **Type**, selecteer **SAML 2.0**.
   
    b. Selecteer **Service Provider-ID**.
   
    c. Kopieer op de klassieke Azure portal, de **externe aanmeldings-URL**, en plak deze in de **Portal de aanmeldings-URL** textbox.
   
    d. Kopieer op de klassieke Azure portal, de **Service-URL met eenmalige Sign-Out**, en plak deze in de **Sign-Out URL** textbox.
   
    e. Kopieer op de klassieke Azure portal, de **URL van wijzigen wachtwoord**, en plak deze in de **URL van wijzigen wachtwoord** textbox.
   
    f. Klik op **Opslaan**.
8. Bij de klassieke Azure portal, selecteert u de configuratie voor één aanmelding bevestiging en klik op **volgende**. 
   
    ![Wat is Azure AD Connect?][10]
9. Op de **eenmalige aanmelding bevestiging** pagina, klikt u op **Complete**.  
   
    ![Wat is Azure AD Connect?][11]

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is een testgebruiker maken in de klassieke Azure portal Britta Simon aangeroepen.  

![Azure AD-gebruiker maken][20]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **klassieke Azure-portal**, klik op het navigatiedeelvenster links **Active Directory**.
   
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-attask-tutorial/create_aaduser_02.png) 
2. Van de **Directory** , selecteert u de map waarvoor u wilt inschakelen van Active directory-integratie.
3. De lijst met gebruikers, in het menu bovenaan, klikt u op **gebruikers**.
   
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-attask-tutorial/create_aaduser_03.png) 
4. Openen van de **gebruiker toevoegen** dialoogvenster op de werkbalk aan de onderkant, klikt u op **gebruiker toevoegen**. 
   
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-attask-tutorial/create_aaduser_04.png) 
5. Op de **Vertel ons meer over deze gebruiker** dialoogvenster pagina, voert u de volgende stappen uit: 
   
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-attask-tutorial/create_aaduser_05.png) 
   
    a. Selecteer de nieuwe gebruiker in uw organisatie als Type gebruiker.
   
    b. De gebruikersnaam **textbox**, type **BrittaSimon**.
   
    c. Klik op **Volgende**.
6. Op de **gebruikersprofiel** dialoogvenster pagina, voert u de volgende stappen uit: 
   
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-attask-tutorial/create_aaduser_06.png) 
   
    a. In de **voornaam** textbox type **Britta**.  
   
    b. In de **achternaam** textbox type, **Simon**.
   
    c. In de **weergavenaam** textbox type **Britta Simon**.
   
    d. In de **rol** selecteert **gebruiker**.

    e. Klik op **Volgende**.

7. Op de **tijdelijk wachtwoord** dialoogvenster pagina, klikt u op **maken**.
   
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-attask-tutorial/create_aaduser_07.png) 
8. Op de **tijdelijk wachtwoord** dialoogvenster pagina, voert u de volgende stappen uit:
   
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-attask-tutorial/create_aaduser_08.png) 
   
    a. Noteer de waarde van de **nieuw wachtwoord**.
   
    b. Klik op **Voltooien**.   

### <a name="creating-an-task-test-user"></a>Maken van een @Task testgebruiker
Het doel van deze sectie is het maken van een gebruiker Britta Simon aangeroepen in @Task.

**Maken van een gebruiker Britta Simon aangeroepen in @Task, voer de volgende stappen uit:**

1. Meld u aan bij uw @Task bedrijf site als administrator.
2. Klik in het menu bovenaan op **mensen**.
3. Klik op **nieuwe persoon**. 
4. In het dialoogvenster nieuwe persoon, moet u de volgende stappen uitvoeren:
   
    ![Maak een @Task testgebruiker][21] 
   
    a. In de **voornaam** textbox, typt u 'Britta'.
   
    b. In de **achternaam** textbox, typt u 'Simon'.
   
    c. In de **e-mailadres** textbox Britta Simon van e-mailadres typt in Azure Active Directory.
   
    d. Klik op **persoon toevoegen**.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD
Het doel van deze sectie is het inschakelen van Britta Simon Azure eenmalige aanmelding gebruiken door haar toegang verlenen tot @Task.

![Gebruiker toewijzen][200] 

**Toewijzen van Britta Simon naar @Task, voer de volgende stappen uit:**

1. In de klassieke Azure portal, de weergave toepassingen in de directoryweergave, klikt u op **toepassingen** in het menu bovenaan.
   
    ![Gebruiker toewijzen][201] 
2. Selecteer in de lijst met toepassingen  **@Task** .
   
    ![Gebruiker toewijzen][202] 
3. Klik in het menu bovenaan op **gebruikers**.
   
    ![Gebruiker toewijzen][203] 
4. Selecteer in de lijst gebruikers **Britta Simon**.
5. Klik in de werkbalk aan de onderkant op **toewijzen**.
   
    ![Gebruiker toewijzen][205]

### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding
Het doel van deze sectie is het testen van uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.  
Wanneer u klikt op de @Task tegel in het deelvenster toegang krijgt u automatisch aangemeld bij uw @Task toepassing.

## <a name="additional-resources"></a>Aanvullende resources
* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-attask-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-attask-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-attask-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-attask-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_01.png
[30]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_02.png


[6]: ./media/active-directory-saas-attask-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_03.png
[8]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_04.png
[9]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_05.png
[10]: ./media/active-directory-saas-attask-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-attask-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-attask-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_08.png


[23]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_06.png

[200]: ./media/active-directory-saas-attask-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-attask-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_09.png
[203]: ./media/active-directory-saas-attask-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-attask-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-attask-tutorial/tutorial_general_205.png






