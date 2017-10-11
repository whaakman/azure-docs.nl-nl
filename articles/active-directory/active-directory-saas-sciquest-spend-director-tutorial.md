---
title: 'Zelfstudie: Azure Active Directory-integratie met SciQuest besteden directeur | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en SciQuest besteden directeur.
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 9fab641b-292e-4bef-91d1-8ccc4f3a0c1f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/17/2017
ms.author: jeedes
ms.openlocfilehash: 84b707668dc45e92e6151f422f1c919f638533b1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sciquest-spend-director"></a>Zelfstudie: Azure Active Directory-integratie met SciQuest besteden directeur
Er is het doel van deze zelfstudie leert u SciQuest besteden directeur integreren met Azure Active Directory (Azure AD).  
SciQuest besteden directeur integreren met Azure AD biedt de volgende voordelen: 

* U kunt beheren in Azure AD die toegang tot SciQuest besteden directeur heeft 
* U kunt uw gebruikers automatisch ophalen aangemeld bij SciQuest besteden directeur (Single Sign-On) inschakelen met hun Azure AD-accounts
* U kunt uw accounts op één centrale locatie - en de klassieke Azure portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten
Voor het configureren van Azure AD-integratie met SciQuest besteden directeur, moet u de volgende items:

* Een Azure AD-abonnement
* Een SciQuest besteden directeur eenmalige aanmelding ingeschakeld abonnement

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

1. SciQuest besteden directeur uit de galerie toevoegen 
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-sciquest-spend-director-from-the-gallery"></a>SciQuest besteden directeur uit de galerie toevoegen
Voor het configureren van de integratie van SciQuest besteden directeur in Azure AD, moet u SciQuest besteden directeur uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen SciQuest besteden directeur uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **klassieke Azure-portal**, klik op het navigatiedeelvenster links **Active Directory**. 
   
    ![Active Directory][1]

2. Van de **Directory** , selecteert u de map waarvoor u wilt inschakelen van Active directory-integratie.

3. De weergave toepassingen in de directoryweergave, klikt u op **toepassingen** in het menu bovenaan.
   
    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.
   
    ![Toepassingen][3]

5. Op de **wat wilt u doen** dialoogvenster, klikt u op **toevoegen van een toepassing uit de galerie**.
   
    ![Toepassingen][4]

6. Typ in het zoekvak **sciQuest besteden directeur**.
   
    ![Toepassingen][5]

7. Selecteer in het deelvenster met resultaten **SciQuest besteden directeur**, en klik vervolgens op **Complete** de toepassing toevoegen.
   
    ![Toepassingen][6]

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van deze sectie is het beschreven hoe u met het configureren en testen eenmalige aanmelding Azure AD met SciQuest besteden directeur op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in SciQuest besteden directeur aan een gebruiker in Azure AD is. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in SciQuest besteden directeur tot stand worden gebracht.  
Deze relatie koppeling wordt ingesteld door het toewijzen van de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in SciQuest besteden directeur.

Om te configureren en testen van Azure AD eenmalige aanmelding met SciQuest besteden directeur, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD één Single Sign-On](#configuring-azure-ad-single-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker SciQuest besteden directeur](#creating-a-halogen-software-test-user)**  - SciQuest besteden directeur die is gekoppeld aan de Azure AD-representatie van haar van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-single-sign-on"></a>Azure AD één eenmalige aanmelding configureren
Het doel van deze sectie is Azure AD eenmalige aanmelding inschakelen in de klassieke Azure portal en eenmalige aanmelding in uw toepassing SciQuest besteden directeur configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met SciQuest besteden directeur, moet u de volgende stappen uitvoeren:**

1. In de klassieke Azure-portal op de **SciQuest besteden directeur** pagina van de integratie van toepassing, klikt u op **eenmalige aanmelding configureren** openen de **configureren Single Sign-On** dialoogvenster.
   
    ![Eenmalige aanmelding configureren][8]

2. Op de **hoe wilt u dat gebruikers zich aanmelden op SciQuest besteden directeur** pagina **Azure AD Single Sign-On**, en klik vervolgens op **volgende**.
   
    ![Azure AD voor eenmalige aanmelding][9]

3. Op de **App-instellingen configureren** dialoogvenster pagina, voert u de volgende stappen uit: 
   
    ![App-instellingen configureren][10]
   
     a. In de **aanmelding op URL** textbox, typ de URL van uw gebruikt door uw gebruikers aanmelden bij uw SciQuest te besteden aan de directeur-toepassing met het volgende patroon volgen: *https://.* SciQuest.com/.**
   
     b. In de **antwoord-URL** textbox dezelfde waarde die u hebt opgegeven in de **aanmelding op URL** textbox. 
   
     c. Klik op **Volgende**.

4. Op de **eenmalige aanmelding configureren op SciQuest besteden directeur** pagina, klikt u op **metagegevens downloaden**, en sla het bestand met metagegevens lokaal op uw computer.
   
    ![Wat is Azure AD Connect?][11]

5. Neem contact op met SciQuest ondersteuning voor deze methode voor verificatie met behulp van de metagegevens van de bovenstaande gedownloade inschakelen.

6. Bij de klassieke Azure portal, selecteert u de configuratie voor één aanmelding bevestiging en klik op **Complete** sluiten de **configureren eenmalige aanmelding** dialoogvenster. 
   
    ![Wat is Azure AD Connect?][15]

7. Op de **eenmalige aanmelding bevestiging** pagina, klikt u op **Complete**.  

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is een testgebruiker maken in de klassieke Azure portal Britta Simon aangeroepen.

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **klassieke Azure-portal**, klik op het navigatiedeelvenster links **Active Directory**.
   
    ![Wat is Azure AD Connect?][100] 

2. Van de **Directory** , selecteert u de map waarvoor u wilt inschakelen van Active directory-integratie.

3. De lijst met gebruikers, in het menu bovenaan, klikt u op **gebruikers**.
   
    ![Wat is Azure AD Connect?][101] 

4. Openen van de **gebruiker toevoegen** dialoogvenster op de werkbalk aan de onderkant, klikt u op **gebruiker toevoegen**. 
   
    ![Wat is Azure AD Connect?][102] 

5. Op de **Vertel ons meer over deze gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
   
    ![Wat is Azure AD Connect?][103] 
   
    a. Als **Type gebruiker**, selecteer **nieuwe gebruiker in uw organisatie**.
   
    b. De gebruikersnaam **textbox**, type **BrittaSimon**.
   
    c. Klik op **Volgende**.

6. Op de **gebruikersprofiel** dialoogvenster pagina, voert u de volgende stappen uit: 
   
    ![Wat is Azure AD Connect?][104] 
   
    a. In de **voornaam** textbox type **Britta**.  
   
    b. In de **achternaam** txtbox, type, **Simon**.
   
    c. In de **weergavenaam** textbox type **Britta Simon**.
   
    d. In de **rol** selecteert **gebruiker**.
   
    e. Klik op **Volgende**.

7. Op de **tijdelijk wachtwoord** dialoogvenster pagina, klikt u op **maken**.
   
    ![Wat is Azure AD Connect?][105]  

8. Op de **tijdelijk wachtwoord** dialoogvenster pagina, voert u de volgende stappen uit:
   
    ![Wat is Azure AD Connect?][106]   
   
    a. Noteer de waarde van de **nieuw wachtwoord**.
   
    b. Klik op **Voltooien**.   

### <a name="creating-a-sciquest-spend-director-test-user"></a>Een testgebruiker SciQuest besteden directeur maken
Het doel van deze sectie is het maken van een gebruiker Britta Simon aangeroepen in SciQuest besteden directeur.

Moet u contact op met het ondersteuningsteam SciQuest besteden directeur en geeft u de details over uw testaccount downloaden gemaakt.

U kunt ook kunt u gebruikmaken van just-in-time-inrichting, een eenmalige aanmelding functie die wordt ondersteund door SciQuest besteden directeur.  
Als just-in-time-inrichting is ingeschakeld, worden gebruikers automatisch gemaakt door SciQuest besteden directeur tijdens een poging voor aanmelding als ze nog niet bestaan. Deze functie hoeven gebruikers eenmalige aanmelding equivalent handmatig maken.

Als u just-in-time-inrichting is ingeschakeld, moet u contact opnemen met je het ondersteuningsteam SciQuest besteden directeur.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD
Het doel van deze sectie is het Britta Simon Azure eenmalige aanmelding gebruiken door haar toegang te verlenen aan SciQuest besteden directeur inschakelen.

![Wat is Azure AD Connect?][200]

**Als u wilt toewijzen Britta Simon SciQuest besteden directeur, moet u de volgende stappen uitvoeren:**

1. In de klassieke Azure portal, de weergave toepassingen in de directoryweergave, klikt u op **toepassingen** in het menu bovenaan.
   
    ![Wat is Azure AD Connect?][201]

2. Selecteer in de lijst met toepassingen **SciQuest besteden directeur**.
   
    ![Wat is Azure AD Connect?][202]

3. Klik in het menu bovenaan op **gebruikers**.
   
    ![Wat is Azure AD Connect?][203]

4. Selecteer in de lijst gebruikers **Britta Simon**.
   
    ![Wat is Azure AD Connect?][204]

5. Klik in de werkbalk aan de onderkant op **toewijzen**.
   
    ![Wat is Azure AD Connect?][205]

### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding
Het doel van deze sectie is het testen van uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.  
Als u op de tegel SciQuest besteden directeur in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing SciQuest besteden directeur.

## <a name="additional-resources"></a>Aanvullende resources
* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_01.png
[6]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_05.png
[8]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_06.png
[9]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_07.png
[10]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_08.png
[11]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_03.png
[15]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_04.png

[100]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_09.png 
[101]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_10.png 
[102]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_11.png 
[103]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_12.png 
[104]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_13.png 
[105]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_14.png 
[106]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_15.png 
[200]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_16.png 
[201]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_17.png 
[202]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_06.png
[203]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_18.png
[204]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_19.png
[205]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_20.png

