---
title: 'Zelfstudie: Azure Active Directory-integratie met SilkRoad levensduur Suite | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en SilkRoad levensduur Suite.
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 3cd92319-7964-41eb-8712-444f5c8b4d15
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/10/2017
ms.author: jeedes
ms.openlocfilehash: ecf4e31ecea00d003fc47ea4cebb781ca58957f7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-silkroad-life-suite"></a>Zelfstudie: Azure Active Directory-integratie met SilkRoad levensduur Suite
Er is het doel van deze zelfstudie leert u SilkRoad levensduur Suite integreren met Azure Active Directory (Azure AD). 

SilkRoad levensduur Suite integreren met Azure AD biedt de volgende voordelen: 

* U kunt beheren in Azure AD die toegang tot SilkRoad levensduur Suite heeft 
* U kunt uw gebruikers automatisch ophalen aangemeld bij SilkRoad levensduur Suite eenmalige aanmelding (SSO) met hun Azure AD-accounts inschakelen

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten
Voor het configureren van Azure AD-integratie met SilkRoad levensduur Suite, moet u de volgende items:

* Een Azure AD-abonnement
* Een abonnement SilkRoad levensduur Suite SSO-ingeschakeld

>[!NOTE]
>Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving. 
> 

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

* U moet uw productieomgeving niet gebruiken tenzij dit noodzakelijk is.
* Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een [proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/). 

## <a name="scenario-description"></a>Scenariobeschrijving
Het doel van deze zelfstudie is zodat u Azure AD SSO testen in een testomgeving.

Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. SilkRoad levensduur Suite uit de galerie toevoegen 
2. Configureren en testen van Azure AD-SSO

## <a name="add-silkroad-life-suite-from-the-gallery"></a>SilkRoad levensduur Suite uit de galerie toevoegen
Voor het configureren van de integratie van SilkRoad levensduur Suite in Azure AD, moet u SilkRoad levensduur Suite uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen SilkRoad levensduur Suite uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **klassieke Azure-portal**, klik op het navigatiedeelvenster links **Active Directory**. 
   
    ![Active Directory][1]

2. Van de **Directory** , selecteert u de map waarvoor u wilt inschakelen van Active directory-integratie.

3. De weergave toepassingen in de directoryweergave, klikt u op **toepassingen** in het menu bovenaan.
   
    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.
   
    ![Toepassingen][3]

5. Op de **wat wilt u doen** dialoogvenster, klikt u op **toevoegen van een toepassing uit de galerie**.
   
    ![Toepassingen][4]

6. Typ in het zoekvak **SilkRoad levensduur Suite**.
   
    ![Toepassingen][5]

7. Selecteer in het deelvenster met resultaten **SilkRoad levensduur Suite**, en klik vervolgens op **Complete** de toepassing toevoegen.
   
    ![Toepassingen][50]

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD
Het doel van deze sectie is het beschreven hoe u met het configureren en testen van Azure AD-SSO met SilkRoad levensduur Suite op basis van een testgebruiker 'Britta Simon' genoemd.

Azure AD moet weten wat de gebruiker equivalent in SilkRoad leven Suite aan een gebruiker in Azure AD is voor eenmalige aanmelding werkt. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in SilkRoad leven Suite tot stand worden gebracht.

Deze relatie koppeling wordt ingesteld door het toewijzen van de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in SilkRoad leven Suite.

Om te configureren en testen van Azure AD eenmalige aanmelding met SilkRoad levensduur Suite, moet u de volgende bouwstenen voltooien:

1. **[Configureren van eenmalige aanmelding Azure AD](#configuring-azure-ad-single-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker SilkRoad levensduur Suite](#creating-a-silkroad-life-suite-test-user)**  - SilkRoad levensduur Suite die is gekoppeld aan de Azure AD-representatie van haar van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren
Het doel van deze sectie is Azure AD-eenmalige aanmelding inschakelen in de klassieke Azure portal en eenmalige aanmelding configureren in uw toepassing SilkRoad levensduur Suite.

**Voor het configureren van Azure AD eenmalige aanmelding met SilkRoad levensduur Suite, moet u de volgende stappen uitvoeren:**

1. Aanmelding bij uw bedrijf SilkRoad site als administrator. 

  >[!NOTE] 
  > Toegang tot de toepassing SilkRoad levensduur Suite verificatie voor federatie configureren met Microsoft Azure AD, neem contact op met ondersteuning voor SilkRoad of uw vertegenwoordiger SilkRoad Services.
  > 

2. Ga naar **serviceprovider**, en klik vervolgens op **Federation Details**. 
   
    ![Azure AD voor eenmalige aanmelding][10] 

3. Klik op **Federatiemetagegevens downloaden**, en sla het bestand met metagegevens op uw computer.
   
    ![Azure AD voor eenmalige aanmelding][11] 

4. In de klassieke Azure-portal op de **SilkRoad levensduur Suite** toepassing Integratiepagina, klikt u op **eenmalige aanmelding configureren** openen de **configureren Single Sign-On** dialoogvenster.
   
    ![Eenmalige aanmelding configureren][6] 

5. Op de **hoe wilt u dat gebruikers zich aanmelden op SilkRoad levensduur Suite** pagina **Azure AD Single Sign-On**, en klik vervolgens op **volgende**.
   
    ![Azure AD voor eenmalige aanmelding][7] 

6. Op de **App-instellingen configureren** dialoogvenster pagina, voert u de volgende stappen uit:
   
    ![Azure AD voor eenmalige aanmelding][8]   
 1. In de **aanmelding op URL** textbox, typ de URL moet worden gebruikt door uw gebruikers aan te melden bij uw site SilkRoad levensduur Suite (bijvoorbeeld: *https://defcompanytest-test-redcarpet.silkroad-eng.com/Authentication/*).  
 2. Open de gedownloade **Silkroad** bestand met metagegevens. 
 3. Zoek de **AssertionConsumerService** labelen en kopieer de **locatie** kenmerk.         
   
    ![Azure AD voor eenmalige aanmelding][21] 
 4. Plak de waarde in de **antwoord-URL** textbox.  
 5. Klik op **Volgende**.

6. Op de **op SilkRoad levensduur Suite eenmalige aanmelding configureren** pagina, voert u de volgende stappen uit:
   
    ![Azure AD voor eenmalige aanmelding][9]  
 1. Klik op downloaden certificaat en sla het bestand op uw computer.  
 2. Klik op **Volgende**.

7. In uw **SilkRoad** toepassing, klikt u op **verificatie bronnen**.
   
    ![Azure AD voor eenmalige aanmelding][12] 

8. Klik op **verificatiebron toevoegen**. 
   
    ![Azure AD voor eenmalige aanmelding][13] 

9. In de **verificatie-bron toevoegen** sectie, voert u de volgende stappen uit: 
   
    ![Azure AD voor eenmalige aanmelding][14]  
 1. Onder **optie 2 - bestand met metagegevens**, klikt u op **Bladeren** het gedownloade metagegevensbestand te uploaden.  
 2. Klik op **maken id-Provider met behulp van bestandsgegevens**.

10. In de **verificatie bronnen** sectie, klikt u op **bewerken**. 
    
     ![Azure AD voor eenmalige aanmelding][15] 

11. Op de **verificatiebron bewerken** dialoogvenster de volgende stappen uitvoeren: 
    
     ![Azure AD voor eenmalige aanmelding][16] 
 1. Als **ingeschakeld**, selecteer **Ja**.   
 2. In de **IdP beschrijving** textbox, typ een beschrijving voor uw configuratie (bijvoorbeeld: *Azure AD SSO*).  
 3. In de **IdP naam** textbox, typ een naam die specifiek is voor uw configuratie (bijvoorbeeld: *Azure SP*).  
 4. Klik op **Opslaan**.

12. Uitschakelen voor alle andere verificatie-bronnen. 
    
     ![Azure AD voor eenmalige aanmelding][17]

13. In de klassieke Azure-portal op de **eenmalige aanmelding bevestiging** pagina, klikt u op **volgende**.  
    
     ![Azure AD voor eenmalige aanmelding][18]

14. Op de **eenmalige aanmelding bevestiging** pagina, klikt u op **Complete**.
    
     ![Azure AD voor eenmalige aanmelding][19]

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is een testgebruiker maken in de klassieke Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][20]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **klassieke Azure-portal**, klik op het navigatiedeelvenster links **Active Directory**.
   
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_09.png)  

2. Van de **Directory** , selecteert u de map waarvoor u wilt inschakelen van Active directory-integratie.

3. De lijst met gebruikers, in het menu bovenaan, klikt u op **gebruikers**.
   
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_03.png) 

4. Openen van de **gebruiker toevoegen** dialoogvenster op de werkbalk aan de onderkant, klikt u op **gebruiker toevoegen**. 
   
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_04.png) 

5. Op de **Vertel ons meer over deze gebruiker** dialoogvenster pagina, voert u de volgende stappen uit: 
   
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_05.png)  
 1. Selecteer de nieuwe gebruiker in uw organisatie als Type gebruiker.  
 2. De gebruikersnaam **textbox**, type **BrittaSimon**. 
 3. Klik op **Volgende**.

6. Op de **gebruikersprofiel** dialoogvenster pagina, voert u de volgende stappen uit: 
   
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_06.png)  
 1. In de **voornaam** textbox type **Britta**.    
 2. In de **achternaam** textbox type, **Simon**. 
 3. In de **weergavenaam** textbox type **Britta Simon**. 
 4. In de **rol** selecteert **gebruiker**.
 5. Klik op **Volgende**.

7. Op de **tijdelijk wachtwoord** dialoogvenster pagina, klikt u op **maken**.
   
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_07.png) 

8. Op de **tijdelijk wachtwoord** dialoogvenster pagina, voert u de volgende stappen uit:
   
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_08.png)  
 1. Noteer de waarde van de **nieuw wachtwoord**. 
 2. Klik op **Voltooien**.   

### <a name="create-a-silkroad-life-suite-test-user"></a>Maak een testgebruiker SilkRoad levensduur Suite
Het doel van deze sectie is het maken van een gebruiker Britta Simon aangeroepen in SilkRoad leven Suite. De Britta moet een SSO-ID hebben (ook wel een *AuthParam*) die overeenkomt met de Britta **emailaddress** in Azure AD.

**Als u wilt een gebruiker Britta Simon aangeroepen in SilkRoad leven Suite maakt, moet u de volgende stappen uitvoeren:**

- Vraag uw team SilkRoad levensduur Suite ondersteuning voor het maken van een gebruiker die als **SSO-ID** kenmerk dezelfde waarde als de **emailaddress** van Britta Simon in Azure AD.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen
Het doel van deze sectie is het Britta Simon haar toegang verlenen aan SilkRoad levensduur Suite gebruikt Azure eenmalige aanmelding inschakelen.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon SilkRoad levensduur Suite, moet u de volgende stappen uitvoeren:**

1. In de klassieke Azure portal, de weergave toepassingen in de directoryweergave, klikt u op **toepassingen** in het menu bovenaan.
   
    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **SilkRoad levensduur Suite**.
   
    ![Gebruiker toewijzen][202] 

3. Klik in het menu bovenaan op **gebruikers**.
   
    ![Gebruiker toewijzen][203] 

4. Selecteer in de lijst gebruikers **Britta Simon**.

5. Klik in de werkbalk aan de onderkant op **toewijzen**.
   
    ![Gebruiker toewijzen][205]

### <a name="test-single-sign-on"></a>Test eenmalige aanmelding
Het doel van deze sectie is het testen van uw Azure AD SSO-configuratie met behulp van het toegangsvenster.  

Als u op de tegel SilkRoad levensduur Suite in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing SilkRoad levensduur Suite.

## <a name="additional-resources"></a>Aanvullende resources
* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_01.png
[50]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_02.png

[6]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_03.png
[8]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_04.png
[9]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_05.png
[10]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_06.png
[11]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_07.png
[12]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_08.png
[13]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_09.png
[14]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_10.png
[15]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_11.png
[16]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_12.png
[17]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_13.png
[18]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_06.png
[19]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_07.png


[20]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_15.png


[200]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_14.png
[203]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_205.png





