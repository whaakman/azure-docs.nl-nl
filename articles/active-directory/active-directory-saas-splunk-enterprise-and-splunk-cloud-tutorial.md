---
title: 'Zelfstudie: Azure Active Directory-integratie met Splunk Enterprise en Splunk Cloud | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Splunk Enterprise en Splunk Cloud.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b3e2b4a9-749c-4895-813d-db46f8dfdbf8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/09/2017
ms.author: jeedes
ms.openlocfilehash: b78e9b7161207a74880e912241d5e965b353d1c5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-splunk-enterprise-and-splunk-cloud"></a>Zelfstudie: Azure Active Directory-integratie met Splunk Enterprise en Splunk Cloud

In deze zelfstudie leert u hoe Splunk Enterprise en Splunk Cloud integreren met Azure Active Directory (Azure AD).

Splunk Enterprise en Splunk Cloud integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Splunk Enterprise en Splunk Cloud heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Splunk Enterprise en Splunk Cloud eenmalige aanmelding (SSO) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de klassieke Azure portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Splunk Enterprise en Splunk Cloud, moet u de volgende items:

- Een Azure AD-abonnement
- Een Splunk Enterprise of Splunk Cloud SSO ingeschakeld abonnement


>[!NOTE]
>Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.
>

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- U moet uw productieomgeving niet gebruiken tenzij dit noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een [proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving.

Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Splunk Enterprise en Splunk Cloud uit de galerie toevoegen
2. Configureren en testen van Azure AD-SSO


## <a name="add-splunk-enterprise-and-splunk-cloud-from-the-gallery"></a>Splunk Enterprise en Splunk Cloud uit de galerie toevoegen
Voor het configureren van de integratie van Splunk Enterprise en Splunk Cloud met Azure AD, moet u Splunk Enterprise en Splunk Cloud uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Splunk Enterprise en Splunk Cloud uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **klassieke Azure-portal**, klik op het navigatiedeelvenster links **Active Directory**.

    ![Active Directory][1]

2. Van de **Directory** , selecteert u de map waarvoor u wilt inschakelen van Active directory-integratie.

3. De weergave toepassingen in de directoryweergave, klikt u op **toepassingen** in het menu bovenaan.

    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.

    ![Toepassingen][3]

5. Op de **wat wilt u doen** dialoogvenster, klikt u op **toevoegen van een toepassing uit de galerie**.

    ![Toepassingen][4]

6. Typ in het zoekvak **Splunk Enterprise of Splunk Cloud**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_01.png)

7. Selecteer in het deelvenster met resultaten **Splunk Enterprise en Splunk Cloud**, en klik vervolgens op **Complete** de toepassing toevoegen.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_02.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Splunk voor ondernemingen en Splunk Cloud op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Splunk Enterprise en Splunk Cloud is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Splunk Enterprise en Splunk Cloud worden gemaakt.

Deze relatie koppeling wordt ingesteld door het toewijzen van de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in Splunk Enterprise en Splunk Cloud.

Om te configureren en testen van Azure AD eenmalige aanmelding met Splunk Enterprise en Splunk Cloud, moet u de volgende bouwstenen voltooien:

1. **[Configureren van eenmalige aanmelding Azure AD](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Splunk Enterprise en Splunk Cloud](#creating-a-splunk-enterprise-and-splunk-cloud-test-user)**  : een equivalent van Britta Simon in de onderneming Splunk en Splunk Cloud die is gekoppeld aan de Azure AD-representatie van haar hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD-eenmalige aanmelding inschakelen in de klassieke portal en eenmalige aanmelding configureren in uw toepassing Splunk Enterprise en Splunk Cloud.


**Voor het configureren van Azure AD eenmalige aanmelding met Splunk Enterprise en Splunk Cloud, moet u de volgende stappen uitvoeren:**

1. In de klassieke portal op de **Splunk Enterprise en Splunk Cloud** toepassing Integratiepagina, klikt u op **eenmalige aanmelding configureren** openen de **configureren Single Sign-On**dialoogvenster.
     
    ![Eenmalige aanmelding configureren][6] 

2. Op de **hoe wilt u dat gebruikers zich aanmelden op Splunk Enterprise-en Splunk** pagina **Azure AD Single Sign-On**, en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_03.png) 

3. Op de **App-instellingen configureren** dialoogvenster pagina, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_04.png) 
  1. In de **aanmelding op URL** textbox, typ de URL moet worden gebruikt door uw gebruikers eenmalige aanmelding voor uw Splunk Enterprise en Splunk Cloud-toepassing met het volgende patroon volgen:`https://<splunkserverUrl>/en-US/app/launcher/home`
  2. In de **id** textbox, typ de URL van uw Splunk Server.
  3. In de **antwoord-URL** textbox, typ de URL met het volgende patroon volgen:`https://<splunkserver>/saml/acs`
  4. Klik op **Volgende**.
 
4. Op de **op Splunk Enterprise- en Splunk Cloud eenmalige aanmelding configureren** pagina, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_05.png)
  1. Klik op **metagegevens downloaden**, en sla het bestand op uw computer.
  2. Klik op **Volgende**.

5. Om eenmalige aanmelding die zijn geconfigureerd voor uw toepassing, neem contact op met Splunk Enterprise en ondersteuningsteam Splunk Cloud en geef met de volgende opties:

    * De gedownloade **federaton metagegevens**
6. In de klassieke portal, selecteert u de configuratie voor één aanmelding bevestiging en klik vervolgens op **volgende**.
    
    ![Azure AD voor eenmalige aanmelding][10]

7. Op de **eenmalige aanmelding bevestiging** pagina, klikt u op **Complete**.  
 
    ![Azure AD voor eenmalige aanmelding][11]

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
In deze sectie maakt u een testgebruiker in de klassieke portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][20]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **klassieke Azure-portal**, klik op het navigatiedeelvenster links **Active Directory**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_09.png) 

2. Van de **Directory** , selecteert u de map waarvoor u wilt inschakelen van Active directory-integratie.

3. De lijst met gebruikers, in het menu bovenaan, klikt u op **gebruikers**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_03.png) 

4. Openen van de **gebruiker toevoegen** dialoogvenster op de werkbalk aan de onderkant, klikt u op **gebruiker toevoegen**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_04.png) 

5. Op de **Vertel ons meer over deze gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_05.png) 
  1. Selecteer de nieuwe gebruiker in uw organisatie als Type gebruiker.
  2. De gebruikersnaam **textbox**, type **BrittaSimon**.
  3. Klik op **Volgende**.

6.  Op de **gebruikersprofiel** dialoogvenster pagina, voert u de volgende stappen uit:
  
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_06.png) 
  1. In de **voornaam** textbox type **Britta**.  
  2. In de **achternaam** textbox type, **Simon**.
  3. In de **weergavenaam** textbox type **Britta Simon**.
  4. In de **rol** selecteert **gebruiker**.
  5. Klik op **Volgende**.

7. Op de **tijdelijk wachtwoord** dialoogvenster pagina, klikt u op **maken**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_07.png) 

8. Op de **tijdelijk wachtwoord** dialoogvenster pagina, voert u de volgende stappen uit:

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_08.png) 
  1. Noteer de waarde van de **nieuw wachtwoord**.
  2. Klik op **Voltooien**.   

### <a name="create-a-splunk-enterprise-and-splunk-cloud-test-user"></a>Een Splunk Enterprise en Splunk Cloud testgebruiker maken

In deze sectie maakt u een gebruiker met de naam Britta Simon in de onderneming Splunk en Splunk Cloud. Neem contact op met de Splunk Enterprise en Splunk Cloud ondersteuningsteam toevoegen van de gebruikers in het Splunk Enterprise en Splunk Cloud-platform.


### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon Azure SSOy haar toegang verlenen Splunk Enterprise-en Splunk gebruiken.

![Gebruiker toewijzen][200] 

**Als u Britta Simon naar de onderneming Splunk en Splunk Cloud toewijzen, moet u de volgende stappen uitvoeren:**

1. In de klassieke portal, de weergave toepassingen in de directoryweergave, klikt u op **toepassingen** in het menu bovenaan.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Splunk Enterprise en Splunk Cloud**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_50.png) 

3. Klik in het menu bovenaan op **gebruikers**.

    ![Gebruiker toewijzen][203]

4. Selecteer in de lijst gebruikers **Britta Simon**.

5. Klik in de werkbalk aan de onderkant op **toewijzen**.

    ![Gebruiker toewijzen][205]

### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD-SSOonfiguration met behulp van het toegangsvenster testen.

Wanneer u klikt op de onderneming Splunk en Splunk Cloud-tegel in het toegangsvenster, u moet ophalen automatisch aangemeld bij uw toepassing Splunk Enterprise en Splunk Cloud.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_205.png
