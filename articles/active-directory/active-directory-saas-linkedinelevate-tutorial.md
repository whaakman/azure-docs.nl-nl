---
title: 'Zelfstudie: Azure Active Directory-integratie met LinkedIn uitbreiden | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en LinkedIn uitbreiden.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 2ad9941b-c574-42c3-bd0f-5d6ec68537ef
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: jeedes
ms.openlocfilehash: 5b46323dc487bbc714c2306ed006afffe8c1bb6a
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-elevate"></a>Zelfstudie: Azure Active Directory-integratie met LinkedIn uitbreiden

In deze zelfstudie leert u hoe LinkedIn bevoegdheden integreren met Azure Active Directory (Azure AD).

LinkedIn bevoegdheden integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot het LinkedIn uitbreiden heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij LinkedIn bevoegdheden (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - en de Azure Management portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met LinkedIn uitbreiden, moet u de volgende items:

- Een Azure AD-abonnement
- Een LinkedIn bevoegdheden eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- U moet uw productieomgeving niet gebruiken tenzij dit noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Het toevoegen van LinkedIn worden de bevoegdheden van de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-linkedin-elevate-from-the-gallery"></a>Het toevoegen van LinkedIn worden de bevoegdheden van de galerie
Voor het configureren van de integratie van LinkedIn bevoegdheden in Azure AD, moet u LinkedIn bevoegdheden uit de galerie aan de lijst met beheerde SaaS-apps toevoegen.

**Als u wilt toevoegen LinkedIn bevoegdheden uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure Management Portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Klik op **toevoegen** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **LinkedIn bevoegdheden**. Klik in het deelvenster met resultaten, op **LinkedIn bevoegdheden** de toepassing toevoegen.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-linkedinElevate-tutorial/tutorial-linkedinElevate_000.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie configureert en test eenmalige aanmelding Azure AD met LinkedIn worden de bevoegdheden op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in LinkedIn worden de bevoegdheden is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in LinkedIn uitbreiden tot stand worden gebracht.

Deze relatie koppeling wordt ingesteld door het toewijzen van de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in LinkedIn uitbreiden.

Als u wilt configureren en testen Azure AD eenmalige aanmelding met LinkedIn uitbreiden, moet u voltooien van de volgende elementen:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker LinkedIn bevoegdheden](#creating-a-linkedin-elevate-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure-beheerportal en eenmalige aanmelding configureren in uw toepassing LinkedIn uitbreiden.

**Voor het configureren van Azure AD eenmalige aanmelding met LinkedIn uitbreiden, moet u de volgende stappen uitvoeren:**

1. In de Azure-beheerportal op de **LinkedIn bevoegdheden** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster als **modus** Selecteer **op basis van SAML aanmelding** eenmalige aanmelding inschakelen op.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-linkedinElevate-tutorial/tutorial-linkedin_01.png)

3. In een ander browservenster aanmelding in uw tenant, LinkedIn bevoegdheden als beheerder.

4. In **Accountcentrum**, klikt u op **globale instellingen** onder **instellingen**. Schakel ook **uitbreiden - AAD-Test worden de bevoegdheden** uit de vervolgkeuzelijst.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_admin_01.png)

5. Klik op **of Klik hier om te laden en afzonderlijke velden van het formulier kopiëren** en kopieer **entiteit-Id** en **Url Assertion Consumer Access (ACS)**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_admin_03.png)

6. Op de Azure-Portal onder **LinkedIn bevoegdheden domein en de URL's**, voer de volgende stappen uit als u wilt configureren van SSO in **IdP geïnitieerd** modus

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_signon_01.png)

    a. In de **id** textbox, voer de **entiteit-ID** gekopieerd uit LinkedIn-Portal 

    b. In de **antwoord-URL** textbox, voer de **Assertion Consumer Access (ACS) Url** gekopieerd uit LinkedIn-Portal

7. Als u wilt configureren van SSO in **SP geïnitieerd**, klikt u op geavanceerde URL weergeven instelling optie in de configuratiesectie en het teken op URL configureren met het volgende patroon volgen:

    `https://www.linkedin.com/checkpoint/enterprise/login/<AccountId>?application=elevate&applicationInstanceId=<InstanceId>` 
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_signon_02.png) 
    
8. Uw toepassing LinkedIn bevoegdheden verwacht de SAML-asserties in een specifieke indeling waarvoor u aangepaste kenmerktoewijzingen toevoegen aan uw configuratie van SAML-token kenmerken. De volgende Schermafbeelding toont een voorbeeld voor deze. De standaardwaarde van **gebruikers-id** is **user.userprincipalname** maar deze optie om te worden toegewezen met e-mailadres van de gebruiker LinkedIn bevoegdheden verwacht. Die u kunt **user.mail** kenmerk uit de lijst of gebruik de juiste kenmerkwaarde op basis van de organisatieconfiguratie van uw. 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-linkedinElevate-tutorial/updateusermail.png)

9. In **gebruikerskenmerken** sectie, klikt u op **weergeven en bewerken van alle andere gebruikerskenmerken** en kenmerken instellen. U moet een andere claim met de naam toevoegen **afdeling** en de waarde moet worden toegewezen aan **user.department**.

    | Kenmerknaam | Waarde kenmerk |
    | --- | --- |    
    | Afdeling| User.Department |

      ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-linkedinElevate-tutorial/userattribute.png)

      a. Klik op het kenmerk toevoegen te openen, de detailpagina kenmerk toevoegen het kenmerk voor afdeling, zoals hieronder-

      ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-linkedinElevate-tutorial/adduserattribute.png)

      b. Klik op **Ok** voor het kenmerk niet opslaan.

      c. Wijzig de naam van het kenmerk **emailaddress** naar **e**.


10. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het XML-bestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-linkedinElevate-tutorial/tutorial-linkedinElevate_certificate.png) 

11. Klik op **Opslaan**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_400.png)

12. Ga naar **LinkedIn beheerdersinstellingen** sectie. Het XML-bestand dat u zojuist hebt gedownload vanuit de Azure-portal door te klikken op de optie uploaden XML-bestand uploaden.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_metadata_03.png)

13. Klik op **op** eenmalige aanmelding inschakelen. Status SSO wordt gewijzigd van **niet verbonden** naar **verbonden**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_admin_05.png)

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in Azure Management portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure Management portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-linkedinElevate-tutorial/create_aaduser_01.png) 

2. Ga naar **gebruikers en groepen** en klik op **alle gebruikers** om de lijst met gebruikers weer te geven.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-linkedinElevate-tutorial/create_aaduser_02.png) 

3. Klik aan de bovenkant van het dialoogvenster **toevoegen** openen de **gebruiker** dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-linkedinElevate-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-linkedinElevate-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**. 

### <a name="creating-a-linkedin-elevate-test-user"></a>Maken van een testgebruiker LinkedIn uitbreiden

Gekoppelde bevoegdheden toepassing ondersteunt Just in tijd gebruikers inrichten en na verificatie gebruikers wordt in de toepassing automatisch gemaakt. Op de beheerder van het tabblad instellingen op de portal worden de bevoegdheden LinkedIn-spiegelen de switch **automatisch toewijzen van licenties** actief voor het inschakelen van Just in time-inrichting en dit wordt ook een licentie toewijzen aan de gebruiker.

   ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-linkedinElevate-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding gebruiken door haar toegang verlenen LinkedIn uitbreiden.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon wilt LinkedIn uitbreiden, moet u de volgende stappen uitvoeren:**

1. In de Azure-beheerportal, opent u de weergave toepassingen en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **LinkedIn bevoegdheden**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-linkedinElevate-tutorial/tutorial-linkedinElevate_0001.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel LinkedIn bevoegdheden in het deelvenster toegang, krijgt u de pagina Azure eenmalige aanmelding en op na geslaagde aanmelding, krijgt u in uw toepassing LinkedIn uitbreiden.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Zelfstudie: LinkedIn bevoegdheden configureren voor automatisch gebruikers inrichten met Azure Active Directory](active-directory-saas-linkedinelevate-provisioning-tutorial.md)
* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_203.png
