---
title: 'Zelfstudie: Azure Active Directory-integratie met LinkedIn Learning | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en LinkedIn daarvan te leren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d5857070-bf79-4bd3-9a2a-4c1919a74946
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/15/2017
ms.author: jeedes
ms.openlocfilehash: 6ad28cb3adaa63ddc3d3769a650d26ca6a7e2695
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-learning"></a>Zelfstudie: Azure Active Directory-integratie met LinkedIn Learning

In deze zelfstudie leert u hoe LinkedIn Learning integreren met Azure Active Directory (Azure AD).

LinkedIn Learning integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot LinkedIn Learning heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij LinkedIn Learning (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met LinkedIn Learning, moet u de volgende items:

- Een Azure AD-abonnement
- Een LinkedIn Learning eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. LinkedIn Learning uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-linkedin-learning-from-the-gallery"></a>LinkedIn Learning uit de galerie toevoegen
Voor het configureren van de integratie van LinkedIn Learning in Azure AD, moet u LinkedIn Learning uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen LinkedIn Learning uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Klik op **toevoegen** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **LinkedIn Learning**. Klik in het deelvenster met resultaten, op **LinkedIn Learning** de toepassing toevoegen.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-linkedinlearning-tutorial/tutorial-linkedinlearning_000.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met LinkedIn Learning op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in LinkedIn Learning in Azure AD voor een gebruiker is. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in LinkedIn Learning tot stand worden gebracht.

Deze relatie koppeling wordt ingesteld door het toewijzen van de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** LinkedIn weten.

Om te configureren en testen van Azure AD eenmalige aanmelding met LinkedIn Learning, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker LinkedIn Learning](#creating-a-linkedin-learning-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing LinkedIn Learning.

**Voor het configureren van Azure AD eenmalige aanmelding met LinkedIn Learning, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **LinkedIn Learning** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-linkedinlearning-tutorial/tutorial-linkedin_01.png)

3. In een ander browservenster aanmelding in uw tenant, LinkedIn Learning als beheerder.

4. In **Accountcentrum**, klikt u op **globale instellingen** onder **instellingen**. Schakel ook **Learning - standaard** uit de vervolgkeuzelijst.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_admin_01.png)

5. Klik op **of Klik hier om te laden en afzonderlijke velden van het formulier kopiëren** en kopieer **entiteit-Id** en **Url Assertion Consumer Access (ACS)**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_admin_03.png)

6. Op de Azure-portal onder **LinkedIn Learning domein en de URL's**, voer de volgende stappen uit als u wilt configureren van SSO in **IdP geïnitieerd** modus

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_signon_01.png)

    a. In de **id** textbox, voer de **entiteit-ID** gekopieerd uit LinkedIn-Portal 

    b. In de **antwoord-URL** textbox, voer de **Assertion Consumer Access (ACS) Url** gekopieerd uit LinkedIn-Portal

7. Als u wilt configureren van SSO in **SP geïnitieerd**, klikt u op geavanceerde URL weergeven instelling optie in de configuratiesectie en de aanmeldings-URL configureren met het volgende patroon volgen:

    `https://www.linkedin.com/checkpoint/enterprise/login/<AccountId>?application=learning&applicationInstanceId=<InstanceId>`

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_signon_02.png)   
    
8. Uw toepassing LinkedIn Learning verwacht de SAML-asserties in een specifieke indeling waarvoor u aangepaste kenmerktoewijzingen toevoegen aan uw configuratie van SAML-token kenmerken. De volgende Schermafbeelding toont een voorbeeld voor deze. De standaardwaarde van **gebruikers-id** is **user.userprincipalname** maar LinkedIn Learning verwacht dit met het e-mailadres van de gebruiker worden toegewezen. Die u kunt **user.mail** kenmerk uit de lijst of gebruik de juiste kenmerkwaarde op basis van de organisatieconfiguratie van uw. 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-linkedinlearning-tutorial/updateusermail.png)
    
9. In **gebruikerskenmerken** sectie, klikt u op **weergeven en bewerken van alle andere gebruikerskenmerken** en kenmerken instellen. De gebruiker moet vier claims met de naam toe te voegen **e**, **afdeling**, **firstname**, en **lastname** en de waarde moet worden toegewezen met **user.mail**, **user.department**, **user.givenname**, en **user.surname** respectievelijk

    | Naam van kenmerk | De waarde van kenmerk |
    | --- | --- |
    | E-mail| User.mail |    
    | Afdeling| User.Department |
    | Voornaam| User.givenName |
    | Achternaam| User.surname |
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-linkedinlearning-tutorial/userattribute.png)
    
    a. Klik op **kenmerk toevoegen** om het dialoogvenster kenmerk te openen.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-linkedinLearning-tutorial/tutorial_attribute_04.png)

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-linkedinLearning-tutorial/tutorial_attribute_05.png)
    
    b. In de **naam** textbox, typ de naam van het kenmerk wordt weergegeven voor die rij.
    
    c. Van de **waarde** typt u de waarde van het kenmerk wordt weergegeven voor die rij.
    
    d. Klik op **Ok**

10. Voer de volgende stappen uit op de **naam** kenmerk -

    a. Klik op het kenmerk openen de **kenmerk bewerken** venster.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-linkedinLearning-tutorial/url_update.png)

    b. Verwijder de URL-waarde van de **naamruimte**.
    
    c. Klik op **Ok** om op te slaan van de instelling.

11. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het XML-bestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-linkedinlearning-tutorial/tutorial-linkedinlearning_certificate.png) 

12. Klik op **Opslaan**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_400.png)

13. Ga naar **LinkedIn beheerdersinstellingen** sectie. Het XML-bestand dat u hebt gedownload van de Azure-portal door te klikken op de optie uploaden XML-bestand uploaden.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_metadata_03.png)

14. Klik op **op** eenmalige aanmelding inschakelen. Status SSO wordt gewijzigd van **niet verbonden** naar **verbonden**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_admin_05.png)

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-linkedinlearning-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-linkedinlearning-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-linkedinlearning-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-linkedinlearning-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**. 

### <a name="creating-a-linkedin-learning-test-user"></a>Een testgebruiker LinkedIn Learning maken

Gekoppelde Learning toepassing ondersteunt. Alleen bij tijd gebruikers inrichten en na verificatie zijn gebruikers in de toepassing automatisch gemaakt. Op de beheerder van het tabblad instellingen op de portal spiegelen LinkedIn Learning de switch **automatisch toewijzen van licenties** actief voor het inschakelen van Just in time-inrichting en dit wordt ook een licentie toewijzen aan de gebruiker.
   
   ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-linkedinLearning-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan LinkedIn Learning.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen LinkedIn Learning, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **LinkedIn Learning**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-linkedinlearning-tutorial/tutorial-linkedinlearning_0001.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Wanneer u klikt op de tegel LinkedIn Learning in het deelvenster toegang, krijgt u de pagina Azure eenmalige aanmelding en op na geslaagde aanmelding, krijgt u in uw toepassing LinkedIn Learning.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_203.png