---
title: 'Zelfstudie: Azure Active Directory-integratie met Wizergos productiviteitssoftware | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Wizergos productiviteitssoftware.
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: acc04396-13c5-4c24-ab9a-30fbc9234ebd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2017
ms.author: jeedes
ms.openlocfilehash: 73b3bc05aeb337c12acb7e47c0dbebe6d0196530
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-wizergos-productivity-software"></a>Zelfstudie: Azure Active Directory-integratie met Wizergos productiviteitssoftware
Er is het doel van deze zelfstudie leert u Wizergos productiviteitssoftware integreren met Azure Active Directory (Azure AD).

Wizergos productiviteitssoftware integreren met Azure AD biedt de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot Software voor Wizergos productiviteit heeft
* U kunt uw gebruikers automatisch ophalen aangemeld bij Wizergos productiviteitssoftware eenmalige aanmelding (SSO) met hun Azure AD-accounts inschakelen
* U kunt uw accounts op één centrale locatie - en de klassieke Azure portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten
Voor het configureren van Azure AD-integratie met Wizergos productiviteitssoftware, moet u de volgende items:

* Een Azure AD-abonnement
* Een abonnement Wizergos productiviteit Software SSO-ingeschakeld

>[!NOTE]
>Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving. 
> 

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

* U moet uw productieomgeving niet gebruiken tenzij dit noodzakelijk is.
* Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een [proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
Het doel van deze zelfstudie is zodat u Azure AD SSO testen in een testomgeving.

Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Wizergos productiviteitssoftware uit de galerie toevoegen
2. Configureren en testen van Azure AD-SSO

## <a name="adding-wizergos-productivity-software-from-the-gallery"></a>Wizergos productiviteitssoftware uit de galerie toevoegen
Voor het configureren van de integratie van Software voor Wizergos productiviteit in Azure AD, moet u Wizergos productiviteitssoftware uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Wizergos productiviteitssoftware uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **klassieke Azure-Portal**, klik op het navigatiedeelvenster links **Active Directory**. 
   
    ![Active Directory][1]
2. Van de **Directory** , selecteert u de map waarvoor u wilt inschakelen van Active directory-integratie.
3. De weergave toepassingen in de directoryweergave, klikt u op **toepassingen** in het menu bovenaan.
   
    ![Toepassingen][2]
4. Klik op **toevoegen** aan de onderkant van de pagina.
   
    ![Toepassingen][3]
5. Op de **wat wilt u doen** dialoogvenster, klikt u op **toevoegen van een toepassing uit de galerie**.
   
    ![Toepassingen][4]
6. Typ in het zoekvak **Wizergos productiviteitssoftware**.
   
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_01.png)
7. Selecteer in het deelvenster resultaten **Wizergos productiviteitssoftware**, en klik vervolgens op **Complete** de toepassing toevoegen.
   
    ![De app selecteren in de galerie](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_001.png)

## <a name="configure-and-test-azure-ad-sso"></a>Configureren en testen van Azure AD-SSO
Het doel van deze sectie is het beschreven hoe u met het configureren en testen van Azure AD-SSO met Wizergos productiviteitssoftware op basis van een testgebruiker 'Britta Simon' genoemd.

Azure AD moet weten wat de gebruiker equivalent in Wizergos productiviteitssoftware aan een gebruiker in Azure AD is voor eenmalige aanmelding werkt. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Wizergos productiviteitssoftware worden gemaakt.

Deze relatie koppeling wordt ingesteld door het toewijzen van de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in Wizergos productiviteitssoftware.

Om te configureren en testen van Azure AD eenmalige aanmelding met BynWizergos productiviteit Softwareder, moet u de volgende bouwstenen voltooien:

1. **[Configureren van eenmalige aanmelding Azure AD](#configuring-azure-ad-single-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Wizergos productiviteitssoftware](#creating-a-wizergos-productivity-software-test-user)**  - Wizergos productiviteitssoftware die is gekoppeld aan de Azure AD-representatie van haar van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-sso"></a>Configuratie van Azure AD-SSO
In deze sectie Azure AD eenmalige aanmelding in de klassieke portal inschakelen en configureren van eenmalige aanmelding in uw toepassing Wizergos productiviteit.

**Voor het configureren van Azure AD eenmalige aanmelding met Wizergos productiviteitssoftware, moet u de volgende stappen uitvoeren:**

1. In de klassieke portal op de **Wizergos productiviteitssoftware** toepassing Integratiepagina, klikt u op **eenmalige aanmelding configureren** openen de **configureren Single Sign-On** dialoogvenster.
   
    ![Eenmalige aanmelding configureren][6] 
2. Op de **hoe wilt u dat gebruikers zich aanmelden op voor de Software voor Wizergos productiviteit** pagina **Azure AD Single Sign-On**, en klik vervolgens op **volgende**:
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_03.png)
3. Op de **App-instellingen configureren** dialoogvenster pagina, klikt u op **volgende**:
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_04.png)
4. Op de **eenmalige aanmelding op Wizergos productiviteitssoftware configureren** pagina, klikt u op **certificaat downloaden**, en sla het bestand op uw computer:
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_05.png)
5. In een ander browservenster aanmelding bij uw tenant Wizergos productiviteitssoftware als een beheerder.
6. Selecteer in het menu hamburger **Admin**.
   
    ![Eenmalige aanmelding op App aan clientzijde configureren](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_000.png)
7. Selecteer in de beheerpagina linkerkant menu **verificatie** en klik op **Azure AD**.
   
    ![Eenmalige aanmelding op App aan clientzijde configureren](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_002.png)
8. Voer de volgende stappen uit op **verificatie** sectie.
   
    ![Eenmalige aanmelding op App aan clientzijde configureren](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_003.png)
  1. Klik op **uploaden** knop om de gedownloade certificaat van Azure AD te uploaden. 
  2. In de **URL-verlener** textbox plaatsen de waarde van **URL-verlener** van de configuratiewizard voor Azure AD-toepassing.
  3. In de **-URL met eenmalige aanmelding** textbox plaatsen de waarde van **één Service-URL aanmelding** van de configuratiewizard voor Azure AD-toepassing.
  4. In de **-URL met eenmalige Sign-Out** textbox plaatsen de waarde van **Service-URL met eenmalige Sign-out** van de configuratiewizard voor Azure AD-toepassing.
  5. Klik op **opslaan** knop.
9. In de klassieke portal, selecteert u de configuratie voor één aanmelding bevestiging en klik vervolgens op **volgende**.
   
    ![Azure AD voor eenmalige aanmelding][10]
10. Op de **eenmalige aanmelding bevestiging** pagina, klikt u op **Complete**.  
    
    ![Azure AD voor eenmalige aanmelding][11]

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de klassieke portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][20]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **klassieke Azure-Portal**, klik op het navigatiedeelvenster links **Active Directory**.
   
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_09.png)
2. Van de **Directory** , selecteert u de map waarvoor u wilt inschakelen van Active directory-integratie.
3. De lijst met gebruikers, in het menu bovenaan, klikt u op **gebruikers**.
   
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_03.png)
4. Openen van de **gebruiker toevoegen** dialoogvenster op de werkbalk aan de onderkant, klikt u op **gebruiker toevoegen**.
   
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_04.png)
5. Op de **Vertel ons meer over deze gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
   
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_05.png) 
  1. Selecteer de nieuwe gebruiker in uw organisatie als Type gebruiker.
  2. De gebruikersnaam **textbox**, type **BrittaSimon**.
  3. Klik op **Volgende**.
6. Op de **gebruikersprofiel** dialoogvenster pagina, voert u de volgende stappen uit:
   
   ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_06.png)
  1. In de **voornaam** textbox type **Britta**.  
  2. In de **achternaam** textbox type, **Simon**.
  3. In de **weergavenaam** textbox type **Britta Simon**.
  4. In de **rol** selecteert **gebruiker**.
  5. Klik op **Volgende**.
7. Op de **tijdelijk wachtwoord** dialoogvenster pagina, klikt u op **maken**.
   
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_07.png)
8. Op de **tijdelijk wachtwoord** dialoogvenster pagina, voert u de volgende stappen uit:
   
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_08.png)
  1. Noteer de waarde van de **nieuw wachtwoord**.
  2. Klik op **Voltooien**.   

### <a name="create-a-wizergos-productivity-software-test-user"></a>Een testgebruiker Wizergos productiviteitssoftware maken
In deze sectie kunt u een gebruiker Britta Simon aangeroepen in Wizergos productiviteitssoftware maken. Neem contact op met het ondersteuningsteam Wizergos productiviteitssoftware via [ support@wizergos.com ](emailTo:support@wizergos.com) de gebruikers van het platform Wizergos productiviteitssoftware toevoegen.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen
Het doel van deze sectie is het Britta Simon haar toegang verlenen aan Wizergos productiviteitssoftware gebruikt Azure eenmalige aanmelding inschakelen.

  ![Gebruiker toewijzen][200]

**Britta Simon om aan te wijzen Wizergos productiviteitssoftware, moet u de volgende stappen uitvoeren:**

1. In de klassieke portal, de weergave toepassingen in de directoryweergave, klikt u op **toepassingen** in het menu bovenaan.
   
    ![Gebruiker toewijzen][201]
2. Selecteer in de lijst met toepassingen **Wizergos productiviteitssoftware**.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_50.png)
3. Klik in het menu bovenaan op **gebruikers**.
   
    ![Gebruiker toewijzen][203]
4. Selecteer in de lijst gebruikers **Britta Simon**.
5. Klik in de werkbalk aan de onderkant op **toewijzen**.
   
    ![Gebruiker toewijzen][205]

### <a name="test-single-sign-on"></a>Test eenmalige aanmelding
Het doel van deze sectie is het testen van uw Azure AD SSO-configuratie met behulp van het toegangsvenster.

Als u op de tegel Wizergos productiviteitssoftware in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Wizergos productiviteitssoftware.

## <a name="additional-resources"></a>Aanvullende bronnen
* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_205.png
