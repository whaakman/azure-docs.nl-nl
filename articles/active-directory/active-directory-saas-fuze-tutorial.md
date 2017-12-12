---
title: 'Zelfstudie: Azure Active Directory-integratie met Fuze | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Fuze.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 9780b4bf-1fd1-48c1-9ceb-f750225ae08a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: jeedes
ms.openlocfilehash: 436fa7d8532d55d3a285917b278fd0adeab7fd7c
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-fuze"></a>Zelfstudie: Azure Active Directory-integratie met Fuze

In deze zelfstudie leert u hoe Fuze integreren met Azure Active Directory (Azure AD).

Fuze integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Fuze heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Fuze (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure Management portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Fuze, moet u de volgende items:

- Een Azure AD-abonnement
- Een Fuze eenmalige aanmelding ingeschakeld abonnement


> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- U moet uw productieomgeving niet gebruiken tenzij dit noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Fuze uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-fuze-from-the-gallery"></a>Fuze uit de galerie toevoegen
Voor het configureren van de integratie van Fuze in Azure AD, moet u Fuze uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Fuze uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure Management Portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Klik op **toevoegen** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Fuze**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-fuze-tutorial/tutorial_fuze_000.png)

5. Selecteer in het deelvenster resultaten **Fuze**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-fuze-tutorial/tutorial_fuze_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie configureert en test eenmalige aanmelding Azure AD met Fuze op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Fuze is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Fuze tot stand worden gebracht.

Deze relatie koppeling wordt ingesteld door het toewijzen van de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in Fuze.

Om te configureren en testen van Azure AD eenmalige aanmelding met Fuze, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Fuze](#creating-a-fuze-test-user)**  - Fuze die is gekoppeld aan de Azure AD-representatie van haar van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure-beheerportal en eenmalige aanmelding in uw toepassing Fuze configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Fuze, moet u de volgende stappen uitvoeren:**

1. In de Azure-beheerportal op de **Fuze** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster als **modus** Selecteer **op basis van SAML aanmelding** eenmalige aanmelding inschakelen op.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-fuze-tutorial/tutorial_fuze_01.png)

3. Op de **Fuze domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-fuze-tutorial/tutorial_fuze_020.png)
    
    In de **aanmelden URL** textbox, typ de eenmalige aanmelding URL als:`https://www.thinkingphones.com/jetspeed/portal/`

4.  Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-fuze-tutorial/tutorial_general_400.png)

5. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het XML-bestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-fuze-tutorial/tutorial_fuze_05.png) 

6. Eenmalige aanmelding configureren op **Fuze** zijde, moet u de gedownloade verzenden **Metadata XML** naar [Fuze ondersteuningsteam](https://www.fuze.com/support). Ze zullen dit instellen om de SAML SSO-verbinding juist is ingesteld op beide zijden.


### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in Azure Management portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure Management portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-fuze-tutorial/create_aaduser_01.png) 

2. Ga naar **gebruikers en groepen** en klik op **alle gebruikers** om de lijst met gebruikers weer te geven.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-fuze-tutorial/create_aaduser_02.png) 

3. Klik aan de bovenkant van het dialoogvenster **toevoegen** openen de **gebruiker** dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-fuze-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-fuze-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**. 


### <a name="creating-a-fuze-test-user"></a>Een testgebruiker Fuze maken

Fuze toepassing ondersteunt volledige Just in tijd gebruiker inrichten, zodat gebruikers automatisch gemaakt wordt wanneer ze aanmelden. Voor alle andere informatie contact op met Fuze [ondersteunen](https://www.fuze.com/support).

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding gebruiken door haar toegang verlenen aan Fuze.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Fuze, moet u de volgende stappen uitvoeren:**

1. In de Azure-beheerportal, opent u de weergave toepassingen en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Fuze**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-fuze-tutorial/tutorial_fuze_50.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    

### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Fuze in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Fuze.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_203.png