---
title: 'Zelfstudie: Azure Active Directory-integratie met GitHub | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en GitHub.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 4395bd95-05de-4deb-87a5-dc3bc8ac4d95
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 2a0e1df5244ef977bdcccc5bcfea615a05efa3bd
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-github"></a>Zelfstudie: Azure Active Directory-integratie met GitHub

In deze zelfstudie leert u hoe GitHub integreren met Azure Active Directory (Azure AD).

GitHub integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot GitHub heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij GitHub (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure Management portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met GitHub, moet u de volgende items:

- Een Azure AD-abonnement
- Een GitHub-eenmalige aanmelding ingeschakeld abonnement


> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- U moet uw productieomgeving niet gebruiken tenzij dit noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. GitHub uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-github-from-the-gallery"></a>GitHub uit de galerie toevoegen
Voor het configureren van de integratie van GitHub in Azure AD, moet u GitHub uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen GitHub uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure Management Portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Klik op **toevoegen** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **GitHub.com**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-github-tutorial/tutorial_github_search02.png)

5. Selecteer in het deelvenster resultaten **GitHub**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-github-tutorial/tutorial_github_search_result02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie configureert en test eenmalige aanmelding Azure AD met GitHub op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in GitHub is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in GitHub tot stand worden gebracht.

Deze relatie koppeling wordt ingesteld door het toewijzen van de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in GitHub.

Om te configureren en testen van Azure AD eenmalige aanmelding met GitHub, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker GitHub](#creating-a-GitHub-test-user)**  : een equivalent van Britta Simon in GitHub die is gekoppeld aan de Azure AD-representatie van haar hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure-beheerportal en eenmalige aanmelding configureren in uw GitHub-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met GitHub, moet u de volgende stappen uitvoeren:**

1. In de Azure-beheerportal op de **GitHub** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster als **modus** Selecteer **op basis van SAML aanmelding** eenmalige aanmelding inschakelen op.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-github-tutorial/tutorial_github_01.png)

3. Op de **GitHub-domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-github-tutorial/tutorial_github_saml011.png)

    a. In de **aanmeldings-URL** textbox, typ de waarde als:`https://github.com/orgs/<entity-id>/sso`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://github.com/orgs/<entity-id>`

    > [!NOTE] 
    > Houd er rekening mee dat deze niet de werkelijke waarden zijn. U moet deze waarden bijwerken met de werkelijke Kana-URL en de id. Hier raden we u voor het gebruik van de unieke waarde van een tekenreeks in de id. Ga naar de sectie GitHub Admin naar deze waarden ophaalt. 

4. Op de **gebruikerskenmerken** sectie **gebruikers-id** als user.mail.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-github-tutorial/tutorial_github_attribute_new01.png)
    
5. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **nieuw certificaat maken**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-github-tutorial/tutorial_github_03.png)

6. Op de **nieuw certificaat maken** dialoogvenster, klikt u op het pictogram van de kalender en selecteer een **vervaldatum**. Klik vervolgens op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-github-tutorial/tutorial_general_300.png)

7. Op de **SAML-certificaat voor ondertekening van** sectie **nieuwe certificaat activeren** en klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-github-tutorial/tutorial_github_04.png)

8. In het pop-upvenster **rollovercertificaat** venster, klikt u op **OK**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-github-tutorial/tutorial_general_400.png)

9. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-github-tutorial/tutorial_github_05.png) 

10. Op de **GitHub configuratie** sectie, klikt u op **configureren GitHub** openen **eenmalige aanmelding configureren** venster.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-github-tutorial/tutorial_github_06.png) 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-github-tutorial/tutorial_github_07.png)

11. Meld u in een ander browservenster in uw organisatie GitHub site als beheerder.

12. Navigeer naar **instellingen** en klik op **beveiliging**

    ![Instellingen](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_03.png)

13. Controleer de **inschakelen SAML-verificatie** vak weer te geven de configuratievelden eenmalige aanmelding. Vervolgens gebruikt u de eenmalige aanmelding URL-waarde voor het bijwerken van de URL met eenmalige aanmelding op de configuratie van Azure AD.

    ![Instellingen](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_13.png)

14. Configureer de volgende velden:

    a. **Meld u aan op de URL voor**: Voer **SAML eenmalige aanmelding Service-URL** van de **GitHub configureren** sectie over Azure AD

    b. **Certificaatverlener**: Voer **SAML entiteit-ID** van de **GitHub configureren** sectie over Azure AD

    c. **Openbaar certificaat**: het gedownloade certificaat openen vanuit Azure AD in Kladblok en kopieer de inhoud die met inbegrip van 'BEGIN CERTIFICATE' en 'END CERTIFICATE'

    ![Instellingen](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_051.png)

15. Klik op **Test SAML-configuratie** om te controleren of er geen validatiefouten of fouten tijdens eenmalige aanmelding.

    ![Instellingen](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_06.png)

16. Klik op **opslaan**

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in Azure Management portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure Management portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-github-tutorial/create_aaduser_01.png) 

2. Ga naar **gebruikers en groepen** en klik op **alle gebruikers** om de lijst met gebruikers weer te geven.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-github-tutorial/create_aaduser_02.png) 

3. Klik aan de bovenkant van het dialoogvenster **toevoegen** openen de **gebruiker** dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-github-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-github-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**. 


### <a name="creating-a-github-test-user"></a>De gebruiker van een GitHub-test maken

Om in te schakelen gebruikers van Azure AD aan te melden bij GitHub, moeten ze worden ingericht in GitHub.  
In het geval van GitHub is inrichting een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw bedrijf GitHub site als beheerder.

2. Klik op **mensen**.

    ![Mensen](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_08.png "personen")

3. Klik op **uitnodiging lid**.

    ![Gebruikers uitnodigen](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_09.png "gebruikers uitnodigen")

4. Op de **uitnodiging lid** dialoogvenster pagina, voert u de volgende stappen uit:

    a. In de **e** textbox, typ de e-mailadres van Britta Simon account.

    ![Personen uitnodigen](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_10.png "personen uitnodigen")
    
    b. Klik op **uitnodiging**.

    ![Personen uitnodigen](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_11.png "personen uitnodigen")

    > [!NOTE]
    > De accounthouder Azure Active Directory wordt een e-mailbericht ontvangen en Ga als volgt een koppeling om hun account te bevestigen voordat deze geactiveerd wordt.


### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding gebruiken door haar toegang verlenen aan GitHub.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon met GitHub, moet u de volgende stappen uitvoeren:**

1. In de Azure-beheerportal, opent u de weergave toepassingen en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **GitHub.com**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-github-tutorial/tutorial_github_search_result021.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    


### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de GitHub-tegel in het deelvenster toegang, u moet ophalen aangemeld bij uw GitHub-toepassing. U zult logboekregistratie in als een organisatie account maar moet zich aanmelden met uw persoonlijke account.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-github-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-github-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-github-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-github-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-github-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-github-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-github-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-github-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-github-tutorial/tutorial_general_203.png
